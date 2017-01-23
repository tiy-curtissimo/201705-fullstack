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
provided a description of an ideal application architecture for distributed
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

But, the idea of "convention over configuration" is *not* ReST. Someone made
the mistake of calling Rails' APIs ReST. Dr. Fielding said that they are
unequivocally not ReST. So, the term "RESTful" was born to describe this
way of interacting with Web services, of using the HTTP verbs against a set
of well-known URL conventions.

## The Angular RESTful Service

That brings us to the `$resource` service, Angular's attempt at making your
interactions with RESTful services easier than eating pie.

[^1]: [Architectural Styles and the Design of Network-based Software Architectures](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). Dr. Roy T. Fielding. 2000.
