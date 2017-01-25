---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyMTk3MSwiY29udGVudF90eXBlIjoiTGVzc29uIn0.-9h3TrhvMR66MjQ_XwylkyqSOwFWjzR8mJLPwTcWQBs

# Everything below this line can be edited.
title: >-
  Components: The Basics
description: >-
  This lesson covers how to declare an Angular 1 component.
---

Before we start writing a component, let's review the advice of the official
documentation about components.

> Advantages of Components:
> * simpler configuration than plain directives
> * promote sane defaults and best practices
> * writing component directives will make it easier to upgrade to Angular 2
> 
> When not to use Components:
> * for directives that need to perform actions in compile and pre-link
>   functions, because they aren't available
> * when you need advanced directive definition options like priority, terminal,
>   multi-element
> * when you want a directive that is triggered by an attribute or CSS class,
>   rather than an element

So, don't go thinking you'll make a component based on a CSS class or when you
need to manipulate the DOM before children elements of the component exist. The
framework just doesn't support it. But, if you live in the sweet spot of "I have
some UI and some behavior that I'd like to bind data to", then you've come to
the right place. Component on!

## The Minimum Declaration

A component really *needs* only one thing: a template. So, we can start off with
just the visual portion of the component, if that's all we need. Because the
inline version using the `template` property of the configuration is trivial and
silly, I present only the `templateUrl` version, here.

```javascript
angular
  .module('visual')
  .component('visualOnly', {
    templateUrl: 'visual-only.template.html'
  });
```

When we use this in our HTML, now, we can just use the tag name like this.

```html
<visual-only></visual-only>
```

## Adding Behavior

Of course, a static component is kind of useless. Well, not useless, but boring.
So, we should add something to it. To add behavior, we just need to associate
a controller with the template in the component. I like aliasing the name of the
controller in the template, as well, using the `controllerAs` property. It seems
that the official Angular 1 Style Guide also recommends this.[^1]

```javascript
class VisualOnlyController {
  $onInit() {
    this.data = [];
  }

  makeSparkly() {
    this.sparkles = true;
  }
}

function factory() {
  return new VisualOnlyController();
}

angular
  .module('visual')
  .component('visualOnly', {
    templateUrl: 'visual-only.template.html',
    controller: factory,
    controllerAs: 'visualOnly'
  });
```

We can use that `makeSparkly` function in our template, like to handle a click
event.

```html
<button ng-click="visualOnly.makeSparkly()">Sparkle!</button>
```

## Transclusion is Ok. Transfats are Not.

Now that we have our `<visual-only>` component reacting nicely to events, maybe
we also want to allow child elements to pass through into the resulting DOM.
That's transclusion, and it's as easy as indicating that we want it to occur and
marking where it should occur in the template.

Here, we add it to the configuration of the component like this.

```javascript
angular
  .module('visual')
  .component('visualOnly', {
    transclude: true,
    templateUrl: 'visual-only.template.html',
    controller: factory,
    controllerAs: 'visualOnly'
  });
```

And, in our template, we add the `ng-transclude` attribute directive like this.

```html
<div ng-transclude></div>
```

Then, when someone uses the `<visual-only>` component in their Angular app, like
this,

```html
<visual-only>
  <audio src="can-you-hear-me-now.mp3" controls>
</visual-only>
```

The `<audio>` element will pass through and be placed in the `<div>` that has
the transclude directive.

```html
<visual-only>
  <div ng-transclude>
    <audio src="can-you-hear-me-now.mp3" controls>
  </div>
</visual-only>
```

## Stateless and Harmless

The configuration covered in this lesson keeps the component isolated from the
other things going on around it in the Angular app. To communicate with the
component, to pass in data and get events, we need to examine "bindings" which
we'll cover in the next lesson.

[^1]: [https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md)
