---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyNzUyNywiY29udGVudF90eXBlIjoiTGVzc29uIn0.DG506E3E2ZX8yLEm-0dhOScZRgCbVnoyl48PdcbQvYI

# Everything below this line can be edited.
title: >-
  DOM Services
description: >-
  Exploring $document, $interval, $location, $rootElement, $timeout, and $window.
---

# Just a Handy Reference

The global scope is bad. The things we write in Angular should never go to the
global scope to use its functionality. Why?

> It makes testing hard.

So, instead of using anything on the `window` object, the global scope of
JavaScript in the browser, we use services provided by Angular.

## $window

Inject this into your code when you want to access the `window` object. You
can do this to listen for the `resize` event, for example.

```javascript
angular
  .module('example')
  .controller('ExampleController', ['$window', ExampleController]);

function ExampleController($window) {
  $window.addEventListener('resize', function (e) {
    // Do something when the window resizes.
  });
}
```

## $document

Inject this into your code when you want to access the `window.document`
object. You can do this to listen for the 'full screen' event, for example.

```javascript
angular
  .module('example')
  .controller('ExampleController', ['$document', ExampleController]);

function ExampleController($document) {
  $document.addEventListener('fullscreenchange', function (e) {
    // Do something when the document goes full screen
  });
}
```

## $location

Inject this into your code when you want to access the `window.location`
object. You can do this to read the current URL, for example.

```javascript
angular
  .module('example')
  .controller('ExampleController', ['$location', ExampleController]);

function ExampleController($location) {
  var currentUrl = $location.href;
}
```

## $timeout

Inject this into your code when you want to access the `window.setTimeout`
function. You can do this when you want to delay execution of code.

```javascript
angular
  .module('example')
  .controller('ExampleController', ['$timeout', ExampleController]);

function ExampleController($timeout) {
  $timeout(function () {
    // Do something in about a second...
  }, 1000);
}
```

## $interval

Inject this into your code when you want to access the `window.setInterval`
function. You can do this when you want to repeatedly execute code on a
specified interval.

```javascript
angular
  .module('example')
  .controller('ExampleController', ['$interval', ExampleController]);

function ExampleController($interval) {
  $interval(function () {
    // Do something every second...
  }, 1000);
}
```

## $rootElement

Inject this into your code when you want a reference to the element where
the `ng-app` is declared.

```javascript
angular
  .module('example')
  .controller('ExampleController', ['$rootElement', ExampleController]);

function ExampleController($rootElement) {
  var el = $rootElement;
  // Do something clever with the root element...
}
```

