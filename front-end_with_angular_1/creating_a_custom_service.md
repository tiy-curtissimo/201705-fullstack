---
# This section is used to configure this assignment on Newline.
# `token` can not be changed; however, the rest can be edited
# as needed. Changes you make here will overwrite the
# existing content on Newline when you push to the master
# branch of the path's GitHub repo.

# Begin the body of the assignment below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoxMjc5NiwiY29udGVudF90eXBlIjoiQXNzaWdubWVudCJ9.iTlqjgY2NDkfDqKL9dVpHbsG2fx1Od9Pan9oGncBMNY

# Everything below this line can be edited.
title: >-
  Creating a Custom Service
description: >-
  Create an Angular 1 service that provides access to localStorage.
---

In this assignment, we will create a service that allows our Angular
application to access the local and session storages of the browser.
When we build our application as we move along, we can use this to
store non-sensitive information in the appropriate kind of storage,
depending on how long our application would like to maintain the data,
during the current user's browser session or over many browser sessions.

Your job is to modify the `storage/storage.service.js` file to make the
UI work.

## Set Up

You have a choice on how to start this exercise:

### Pull from the GitHub repo.

`git clone https://github.com/tiy-curtissimo/angular-storage-exercise.git`

### Copy and paste by hand

Start with these five files.

__index.html__
```html
<!doctype html>
<html ng-app="app">
  <head>
    <title>Storage builder</title>
    <link rel="stylesheet" href="https://unpkg.com/purecss@0.6.2/build/pure-min.css" integrity="sha384-UQiGfs9ICog+LwheBSRCt1o5cbyKIHbwjWscjemyBMT9YCUMZffs6UqUTd0hObXD" crossorigin="anonymous">
  </head>
  <body ng-controller="DemoController">
    <header>
      <h1>Storage Service Demonstration</h1>
      <div id="storage-options">
        <form class="pure-form" novalidate ng-submit="saveKeyValuePair()">
          <label class="pure-checkbox">
            <input ng-model="storageType" type="radio" value="local">
            Use local storage.
          </label>
          <label class="pure-checkbox">
            <input ng-model="storageType" type="radio" value="session">
            Use session storage.
          </label>
          <fieldset>
            <legend>Set a key and value</legend>
            <input ng-model="key" type="text" placeholder="key">
            <input ng-model="value" type="text" placeholder="value">
            <button type="submit" class="pure-button pure-button-primary">Save</button>
          </fieldset>
        </form>
      </div>
    </header>
    <main>
      <fieldset>
        <legend>Local storage</legend>
        <table class="pure-table">
          <thead>
            <tr>
              <th>Key</th>
              <th>Value</th>
            </tr>
          </thead>
          <tbody>
            <tr ng-repeat="keyValue in localKeyValues">
              <td>{{keyValue.key}}</td>
              <td>{{keyValue.value}}</td>
            </tr>
          </tbody>
        </table>
      </fieldset>
      <fieldset>
        <legend>Session storage</legend>
        <table class="pure-table">
          <thead>
            <tr>
              <th>Key</th>
              <th>Value</th>
            </tr>
          </thead>
          <tbody>
            <tr ng-repeat="keyValue in sessionKeyValues">
              <td>{{keyValue.key}}</td>
              <td>{{keyValue.value}}</td>
            </tr>
          </tbody>
        </table>
      </fieldset>
    </main>

    <script src="angular-1.6.1.min.js"></script> 
    <script src="app.module.js"></script>
    <script src="storage/storage.module.js"></script>
    <script src="storage/storage.service.js"></script>
    <script src="demo.controller.js"></script>
  </body>
</html>
```

__app.module.js__
```javascript
(function () {
  angular
    .module('app', ['storage']);
})();
```

__storage/storage.module.js__
```javascript
(function () {
  angular
    .module('storage', []);
})();
```

__storage/storage.service__
```javascript
(function () {
  angular
    .module('storage')
    .factory('storage', ['$window', storageService]);
  
  function storageService($window) {

    /********************************
     * YOUR IMPLEMENTATION HERE
     */
    return {}; // REPLACE THIS RETURN WITH YOUR STUFF

  }
})();
```

__demo.controller.js__
```javascript
(function () {
  angular
    .module('app')
    .controller('DemoController', ['$scope', 'storage', DemoController]);
  
  function DemoController($scope, storage) {
    $scope.storageType = 'local';
    $scope.localKeyValues = storage.for('local').getAll();
    $scope.sessionKeyValues = storage.for('session').getAll();

    $scope.saveKeyValuePair = function () {
      var vault = storage.for($scope.storageType);
      vault.set($scope.key, $scope.value);
      
      $scope.localKeyValues = storage.for('local').getAll();
      $scope.sessionKeyValues = storage.for('session').getAll();
    };

  }
})();
```