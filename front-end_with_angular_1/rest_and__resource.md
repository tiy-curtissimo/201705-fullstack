---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyMTQ5NywiY29udGVudF90eXBlIjoiTGVzc29uIn0.5hFLgw81qj7259yXw-mvmXh5CgTPxKWucEs8z5ixGMI

# Everything below this line can be edited.
title: >-
  ReST and $resource
description: >-
  Exploring the ReST-over-HTTP abstraction of $resource.
---

In 2000, Dr. Roy Fielding was working on his dotoral dissertation. Having
recently worked with the group defining the HypterText Transfer Protocol 1.1,
he had spent a lot of time thinking about how to build applications using an
infrastructure similar to the World Wide Web. His dissertation,
"Architectural Styles and the Design of Network-based Software Architectures"[^1]
provides a description of an ideal application architecture for distributed
systems that have an unknown number of intermediary caching nodes and
unreliable network connectivity.

His paper went largely ignroed while software architects explored Remote
Procedure Call-based solutions like COM, CORBA, J2EE, and SOAP. These very
complex solutions led to a software development industry unable to pay down
its technical debt. Each so-called RPC Web service became a messy proliferation
of (usually) poorly documented methods. Consuming an RPC Web service became
a large source of friction, a condition that normally leads developers to
either implement their own RPC Web service that provides the same data as
the one they want to use or, in much rarer cases, find a new way to solve
the problem.

While the enterprise-based architects continued to refine their proposals, a
software devleoper at a company named Basecamp extracted a framework from the Ruby
application that ran the Basecamp application. He released this framework under an
open source license and called it Ruby on Rails. Originally, Rails used a
lightweight implementation of SOAP-based RPC Web services. However, the
author found this distasteful and moved the framework to resource-based Web
services. Instead of building endpoints-as-services that contained an unknown
collection of methods, discovered only after reviewing any available
documentation, he adopted some of the core tenets of Fielding's ReST
architecture. The endpoints in
Rails represent *resources*, some concept of a *thing* modeled by the
application. The way that developers interacted with these *resources* was
through the well-known HTTP verbs of `GET`, `PUT`, `POST`, and `DELETE`.
Instead of remote-calling methods on some RPC Web service (`createBook`,
for example), the developer would `POST` a new representation of the
object to a URI (to `/api/books`, for example) thereby creating a new
resource from the representation. The outcome was the same, but the API
through which developers consumed it was based on a well-known convention
rather than the whims of programmers. It heralded the "convention over
configuration" era of Web-based application development.

One day, someone made the mistake of calling the Rails' API convention an
example of ReST. Dr. Fielding said that they are unequivocally not ReST. After
many long and heated debates on email servers and from blogs and in chat rooms,
the term "RESTful" was born to describe this style of interaction, of using the
HTTP verbs in conjunction with a set of well-known URL conventions as the
definition of an API.

## The Conventions of RESTful

The Rails authors came to consider a very specific form of URL to become the
convention of RESTful Web services. For a given resource (like "Invoice"), the
series of interactions that you can have with a resource are:

| Pattern           | Verbs  | Reason                                    |
|-------------------|--------|-------------------------------------------|
| /«resources»      | GET    | Get a list of all the resources           |
| /«resources»      | POST   | Create a new resource                     |
| /«resources»/«id» | GET    | Get the details of a specific resource    |
| /«resources»/«id» | PUT    | Replace a resource with the supplied data |
| /«resources»/«id» | PATCH  | Update specific fields of the resource    |
| /«resources»/«id» | DELETE | Remove the resource                       |

Using those conventions, we would know that for a resource like "Invoice", we
could interact with URLs that provide a Web service for interacting with
"Invoices" with the following URLs:

| Example        | Verbs  | Reason                                    |
|----------------|--------|-------------------------------------------|
| /invoices      | GET    | Get a list of all the invoices            |
| /invoices      | POST   | Create a new invoice                      |
| /invoices/1172 | GET    | Get the details of invoice #1172          |
| /invoices/1172 | PUT    | Update the invoice by replacing all it    |
| /invoices/1172 | PATCH  | Update one or more invoice properties     |
| /invoices/1172 | DELETE | Delete the invoice                        |


## The Angular RESTful Service

That brings us to the `$resource` service, Angular's attempt at making your
interactions with RESTful services easier than eating pie. Because "all"
RESTful Web services follow that convention, we can configure `$resource`
to handle all of the complexity of the interaction behind methods that
make more sense than the more obscure names of the HTTP verbs. To use the
example of the Invoice Web service in the previous section, we can use
Angular's `$resource` in the following manner after Angular injects into
our controller.

```javascript
angular
  .module('accounting')
  .controller('invoice', ['$scope', '$resource', function ($scope, $resource) {
    var Invoice = $resource('/invoices/:id');

    $scope.getAll = function () {
      Invoice.query(function (invoices) {
        $scope.invoices = invoices;
      });
    };

    $scope.getDetail = function () {
      Invoice.get({id: $scope.selectedInvoice.id}, function (invoice) {
        $scope.invoiceDetails = invoice;
      });
    };

    $scope.create = function () {
      var newInvoice = new Invoice($scope.invoice);
      newInvoice.$save();
    };

    $scope.delete = function () {
      Invoice.get({id: $scope.selectedInvoice.id}, function (invoice) {
        invoice.$delete();
      });
    };
  }]);
```

As you can see, `$resource` gives us all of the usual CRUD interactions that
we've come to expect from APIs. This has a lot more meaning than using the
more protocol-specific `$http.get` or `$http.post`.

Unlike `$http`, `$resource` uses a callback mechanism to perform its follow-up
data reconciliation rather than a Promise. This provides inconsistencies in the
overall Angular API that we should prevent from leaking out of our services
into the rest of our code and others'. We should choose either a Promise-based
or callback-centric idiom and stick with it through all of our code.

## Idiomatic Use

For example, instead of using `$resource` directly in our controllers, we
should encapsulate its use in a service of our crafting. Using the invoice
example from earlier, we could create an invoice service to hide the use
of `$resource` so that we can provide a Promise-based API for the
consumption of invoice data.

```javascript
angular
  .module('dataAccess')
  .factory('Invoice', ['$resource', '$q', function ($resource, $q) {
    var api = $resource('/invoices/:id');

    return {
      query: function () {
        return $q(function (good, bad) {
          api.query(good, bad);
        });
      }
    };
  }]);
```

With this kind of service created, we can have cleaner interactions
with the RESTful Web service specifically tailored to invoices.

```javascript
angular
  .module('accounting')
  .controller('invoice', ['Invoice', function (Invoice) {
    $scope.getAll = function () {
      Invoice
        .query()
        .then(function (invoices) {
          $scope.invoices = invoices;
        })
        .catch(function (error) {
          $scope.error = error;
        });
    };
  }]);
```

[^1]: [Architectural Styles and the Design of Network-based Software Architectures](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). Dr. Roy T. Fielding. 2000.
