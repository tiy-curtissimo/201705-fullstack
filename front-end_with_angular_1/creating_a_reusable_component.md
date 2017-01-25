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
If you need to, download the starter kit template, again, from GitHub. The page
for that is at
[https://github.com/tiy-curtissimo/angular-starter](https://github.com/tiy-curtissimo/angular-starter).
[/callout-info]

We will need two components:
* a parent component that has data that it passes down into a child component;
  and,
* a child component that displays the data and sends information back to the
  parent component through an output binding.

## Directions

Create a component that passes some data into another component through an
attribute binding. Then, on some event in the child component, have it pass
information back to the parent component.
