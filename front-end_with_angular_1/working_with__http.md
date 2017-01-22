---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyMTQ5NiwiY29udGVudF90eXBlIjoiTGVzc29uIn0.KWe3spM0GNGRFLpkJiLl8xplBTshYURp1WBxuJFG50Q

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

At the time, Microsoft had heavily invested in ActiveX, at the time its
newest incarnation of COM, and the API for `IXMLHTTPRequest` reflects its
origins as a wrapper around a C object. As other browser vendors saw the
power of the new functionality, they created their own implementations of
`IXMLHTTPRequest` with Mozilla exposing the functionality behind a
JavaScript wrapper which has become the standard: `XMLHttpRequest`.

Since the early 2000s, almost every ECMAscript library has provided an
abstraction over `XMLHttpRequest` to ease its use by developers, the most
popular being jQuery's `$.ajax` API. Angular 1 is no different.

## $http: The AJAX Angular Service





[^1] ["Article on the history of XMLHTTP by an original developer"](http://www.alexhopmann.com/xmlhttp.htm). Alexhopmann.com. 2007-01-31.