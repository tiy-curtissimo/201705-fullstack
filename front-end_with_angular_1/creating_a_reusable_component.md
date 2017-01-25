---
# This section is used to configure this assignment on Newline.
# `token` can not be changed; however, the rest can be edited
# as needed. Changes you make here will overwrite the
# existing content on Newline when you push to the master
# branch of the path's GitHub repo.

# Begin the body of the assignment below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoxMzA0NiwiY29udGVudF90eXBlIjoiQXNzaWdubWVudCJ9.K_8jaDhlfT4eCFZqXo89Vm6eqLJvvJlfBHSOXbDqXxc

# Everything below this line can be edited.
title: >-
  Creating a reusable component
description: >-
  Use the information from the last three lessons to create a reusable component.
---

Now that we know *everything* there is to know about components in Angular 1,
it's time to write one!

[callout-info]
If you need to, download the starter kit template, again, from GitHub.

- [https://github.com/tiy-curtissimo/angular-starter](https://github.com/tiy-curtissimo/angular-starter).
[/callout-info]

We will need two components:
* a parent component that has data that it passes down into a child component;
  and,
* a child component that displays the data and sends information back to the
  parent component through an output binding.

## Directions

In your `src/index.html`, put the following use of our soon-to-be-created parent
component in the `<main>` tag like this.

```html
<main>
  <parenting-is-hard></parenting-is-hard>
</main>
```

Now, create the following files for that component:

* `src/parenting-is-hard.component.js`
* `src/parenting-is-hard.template.html`

In `src/parenting-is-hard.template.html`, put the following HTML.

```html
<childhood-is-fun info="parent.child" on-provoked="parent.worry($event)"></childhood-is-fun>
```

In the `src/parenting-is-hard.component.js` file, declare the component, set it
to use the HTML template, create a controller alias, and assign a controller.
In the `$onInit` method of the controller, put the following code.

```javascript
// ES2015 with a class
$onInit() {
  this.child = { age: 4, disposition: 'sunny' };
}

// ES5 with just a function
this.$onInit = function () {
  this.child = { age: 4, disposition: 'sunny' };
}
```

That will get bound into the child template.

Now, create the following files for that component:

* `src/childhood-is-fun.component.js`
* `src/childhood-is-fun.template.html`

In the component file, declare the component, set it to use the HTML template,
create a controller alias, create a controller, and make sure to declare its
input and output bindings. Then, in the HTML template, show the information
passed in through the `info` binding and figure out a way to allow the user to
trigger the `onProvoked` output binding.

Finally, include these four files in your `index.html`. Run it and profit from
your newly-found knowledge
