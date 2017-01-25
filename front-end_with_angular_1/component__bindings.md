---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyMTk3MiwiY29udGVudF90eXBlIjoiTGVzc29uIn0.Cb9TGUNIorZ04v5t6At-1IV7D0yCrmYKOAP6JVEW2RQ

# Everything below this line can be edited.
title: >-
  Component: Bindings
description: >-
  Getting data into and out of an Angular 1 component.
---

<link rel="stylesheet" href="https://unpkg.com/purecss@0.6.2/build/pure-min.css" integrity="sha384-UQiGfs9ICog+LwheBSRCt1o5cbyKIHbwjWscjemyBMT9YCUMZffs6UqUTd0hObXD" crossorigin="anonymous">
<link rel="stylesheet" href="https://unpkg.com/purecss@0.6.2/build/grids-responsive-min.css">


The last thing that we want are *boring* components. Angular 1 provides a way
for us to pass data into a component as well as sending data out. This lesson
describes each of those types of bindings and gives advice about their use.

## Overview

In this lesson, we sill write a component that shows a small form which allows
us to create or edit a user for our system. It will allow us to specify a first
name, a last name, and an email address.

The markup that we will use in our HTML templates will look like this.

```html
<user-form ...></user-form>
```

The component definition that we'll use in `user-form.component.js` will look
like this.

```javascript
(function () {
  class UserFormController {
  }

  function factory() { return new UserFormController(); }

  angular
    .module('user')
    .component('userForm', {
      template: 'user/user-form.template.html',
      controller: factory,
      controllerAs: 'form'
      bindings: { /* We will put stuff here. */ }
    });
})();
```

The HTML template in `user-form.template.html` will look like this with more
styling in it. I've not included the CSS stuff so that we can focus on how the
bindings works rather than the presentation.

```html
<form>
  <input type="text" placeholder="First Name">
  <input type="text" placeholder="Last Name">
  <input type="email" placeholder="Email">
  <button type="submit">Create this user</button>
</form>
```

Finally, the component will render something like this in the browser.

<form class="pure-form">
  <fieldset>
        <legend>The User Widget</legend>

        <div class="pure-g">
          <div class="pure-u-1 pure-u-md-1-4">
            <input type="text" placeholder="First Name" class="pure-u-23-24">
          </div>
          <div class="pure-u-1 pure-u-md-1-4">
            <input type="text" placeholder="Last Name" class="pure-u-23-24">
          </div>
          <div class="pure-u-1 pure-u-md-1-4">
            <input type="email" placeholder="Email" class="pure-u-23-24">
          </div>
          <div class="pure-u-1 pure-u-md-1-4">
            <button type="submit" class="pure-button pure-button-primary">Create this user</button>
          </div>
        </div>
    </fieldset>
</form>

# Getting Data Into the Component

Because our components act like custom HTML elements, that is, custom HTML tags,
Angular carries forward that metaphor by passing data to the component through
attributes on the tag in the HTML that we write. This means, that we can pass
through to the component information through attributes we define. The next few
sections show how to pass information in.

## Simple String Binding

We have three text fields in our component. We can provide attributes on the
component that provide information to each of those components. The markup that
we'd use with the component could look like this, for example.

[callout-info]
Please note that the line breaks and the white space in the format of the tag
below is only to make it look better on this page. In your source code, you
do not have to use line breaks to make the data binding work.
[/callout-info]

```html
<user-form first-name="Jack"
           last-name="Long"
           email="jack@long.info"></user-form>
```

Now that we know what it'll look like, let's create bindings. In the
`user-form.component.js` file, let's add the one-way, string bindings for those
attributes.

```javascript
  angular
    .module('user')
    .component('userForm', {
      template: 'user/user-form.template.html',
      controller: factory,
      controllerAs: 'form'
      bindings: {
        firstName: '@',
        lastName:  '@',
        email:     '@'
      }
    });
```

Each of those '@' symbols means that Angular will look for an attribute on the
component and bring in that value. Now, all we need to do to get it to work is
bind each of those input bindings to our `input` tags using the `ngModel`
directive. That changes our template to look like this.

```html
<form>
  <input type="text" placeholder="First Name" ng-model="form.firstName">
  <input type="text" placeholder="Last Name" ng-model="form.lastName">
  <input type="email" placeholder="Email" ng-model="form.email">
  <button type="submit">Create this user</button>
</form>
```

That's all it takes to get simple scalar data to flow into our component.

## One-Way Complex Data Binding

Let's say that we get feedback from the developers that are using the
`<user-form>` component. They all say

> Hey, it's nice that we have three attributes, but I'd really like just one
> attribute where I can pass in a user object that has those properties.

Luckily, Angular supports this, too. We will change our usage of the component
to look like this. The `userInfo` name is a variable in the calling component's
scope that contains a user object.

```html
<user-form user="userInfo"></user-form>
```

Now, we change our component definition to look like this. The "<" means that
we want the complex data to flow into the component but we don't care about
letting it flow back out.

```javascript
  angular
    .module('user')
    .component('userForm', {
      template: 'user/user-form.template.html',
      controller: factory,
      controllerAs: 'form'
      bindings: {
        user: '<'
      }
    });
```

Finally, we need to update the model bindings in `user-form.template.html` to
reflect those changes.

```html
<form>
  <input type="text" placeholder="First Name" ng-model="form.user.firstName">
  <input type="text" placeholder="Last Name" ng-model="form.user.lastName">
  <input type="email" placeholder="Email" ng-model="form.user.email">
  <button type="submit">Create this user</button>
</form>
```

Now, it all works, again.

## Two-Way Complex Data Binding

[callout-warning]
### Don't do this.

It seems the general concensus from people that have built large Angular Web
applications warn against this. It can cause confusion about which component
should control the manipulation of a specific piece of data. It's best to use
one-way binding to get the data into the component, like we saw in the last
section, and to use output bindings to get data out, like we'll see in the next
section.
[/callout-warning]

That seems to be the general concensus. However, if want to do this, you can
change the previous component definition from using the "<" symbol to the "="
symbol like this.

```javascript
  angular
    .module('user')
    .component('userForm', {
      template: 'user/user-form.template.html',
      controller: factory,
      controllerAs: 'form'
      bindings: {
        user: '='
      }
    });
```

## Output Binding

This is the neatest trick of all. Angular 1 uses the event hook metaphor to
simulate output bindings. Now that we have a form, let's hook up an even that
will fire when the user submits the `form` in our component.

In our component's template, we update the HTML to capture the `submit` event
and have our component do something with it.

```html
<form ng-submit="form.handleSubmit()">
  <input type="text" placeholder="First Name" ng-model="form.user.firstName">
  <input type="text" placeholder="Last Name" ng-model="form.user.lastName">
  <input type="email" placeholder="Email" ng-model="form.user.email">
  <button type="submit">Create this user</button>
</form>
```

Now, we need to create that `handleSubmit` in our component, as well as an
output binding. We specify the output binding which we will call `onSubmit` by
using the "&" symbol. Now, when the user submits the form, it will call the
`handleSubmit` method on the controller which, in turn, creates an Angular event
for the output binding, and fires the `onSubmit` event.

```javascript
  class UserFormController {
    handleSubmit() {
      var angularEvent = { $event: this.user };
      this.onSubmit(angularEvent);
    }
  }

  function factory() { return new UserFormController(); }

  angular
    .module('user')
    .component('userForm', {
      template: 'user/user-form.template.html',
      controller: factory,
      controllerAs: 'form'
      bindings: {
        user:     '=',
        onSubmit: '&'
      }
    });
```

The consuming component would modify its usage of the `user-form` element like
this, now.

```html
<user-form user="userInfo" on-submit="handleSubmit($event)"></user-form>
```

And, now, the world works nicely.
