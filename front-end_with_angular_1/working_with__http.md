---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyNzUyNSwiY29udGVudF90eXBlIjoiTGVzc29uIn0.bmZUuXwhO8OnaOLbiwLPyNb9QkYn3QR8kKF8i2aTh88

# Everything below this line can be edited.
title: >-
  Working with $http
description: >-
  Getting to know our new best friend, $http, our AJAX assistant.
---
Quite possibly the most important advance in Web browser technology, the
`IXMLHTTPRequest` was developed in the late 1990s for Outlook Web Access
for Microsoft Exchange Server 2000.[^1] It provided a way for developers
to make HTTP calls without refreshing the Web page or using a hack like
a hidden `IFRAME`.

At the time, Microsoft had heavily invested in ActiveX, the newest
incarnation of COM, and the API for `IXMLHTTPRequest` reflects its origins
as a wrapper around a C object. As other browser vendors saw the power of
the new functionality, they created their own implementations of
`IXMLHTTPRequest` with Mozilla exposing the functionality behind a
JavaScript wrapper which has become the standard: `XMLHttpRequest`.

Since the early 2000s, almost every ECMAscript library has provided an
abstraction over `XMLHttpRequest` to ease its use by developers, the most
popular being jQuery's `$.ajax` API. Angular 1 is no different.

## The Angular AJAX Service

The essence of `$http` is to make your life easier when making AJAX
requests. Compare this use of `XMLHttpRequest`

```javascript
var xhr = new XMLHttpRequest();
xhr.addEventListener('readystatechange', function () {
  var DONE = this.DONE || 4;
  if (this.readyState === DONE && this.status === 200) {
    var o = JSON.parse(xhr.responseText);
    publish(o); // Do something with the response.
  } else if (this.readyState === DONE) {
    handleError(xhr);
  }
});
xhr.open('GET', 'http://date.jsontest.com/');
xhr.setRequestHeader('Accept', 'application/json, text/plain, */*');
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.setRequestHeader('XSRF-TOKEN', csrfCookeValue);
xhr.send();
```

with this use of `$http`:

```javascript
$http
  .get('http://date.jsontest.com/')
  .then(function (response) {
    publish(response.data); // Do something with the response.
  })
  .catch(function (error) {
    handleError(error);
  });
```

It's easy to see that the syntax of the `$http` service promotes a
more intuitive understanding of what's happening than the unadorned
use of the `XMLHttpRequest`. Moreover, the `$http` methods return
a [Promise](http://promisesaplus.com/)-compliant object that allows
us to chain `then` handlers to work with the response from the HTTP
call. In this way, the `$http` service mimics the upcoming
[fetch](https://fetch.spec.whatwg.org) standard.

## The Convenience methods

The real form for `$http` looks like this:

```javascript
$http({
  method: 'GET',
  url: 'http://date.jsontest.com'
})
```

However, that seems a little unwieldy. The Angular team decided to
provide convenience methods for the HTTP verbs with which we're
familiar as well as some we may not use regularly.

* The Usual Suspects
  * `$http.get(url, [config])`
  * `$http.post(url, data, [config])`
* ReST Buddies
  * `$http.put(url, data, [config])`
  * `$http.delete(url, [config])`
* New Kid on the Block
  * `$http.patch(url, data, [config])`
* That Uncle You See Once a Year
  * `$http.head(url, [config])`

## Configuring the Request

Each of the methods contains an optional parameter to configure the
properties of the request. I'm not going to list all of the options
available to you; that's the
[documentation](https://docs.angularjs.org/api/ng/service/$http#usage)'s
job. However, I will highlight a couple that we use often.

### `withCredentials: true|false`

You need to specify `withCredentials` with a `true`-thy value for the
request to contain cookies set in the client's browser. Important for
things like session management, you should do this almost all the time.

### `transformRequest: function(data, headers)`

You can specify a method to alter the headers for the request. Whatever
data you want sent in the request you have the responsibility to return
it.

For example, let's say you want to change a request with a content type
of `application/json` into of `application/x-www-form-urlencoded` and
make the included JSON the value of a field named "json". Your transform
function would look something like this.

```javascript
$http
  .post('http://validate.jsontest.com', { key: 'value' }, {
    headers: {
      'content-type': 'application/x-www-form-urlencoded'
    },
    transformRequest: function (data, headers) {
      return 'json=' + angular.toJson(data);
    }
  })
  .then(function (response) {
    publish(response.data); // Do something with the response.
  })
  .catch(function (error) {
    handleError(error);
  });
```


[^1]: ["Article on the history of XMLHTTP by an original developer"](http://www.alexhopmann.com/xmlhttp.htm). Alexhopmann.com. 2007-01-31.