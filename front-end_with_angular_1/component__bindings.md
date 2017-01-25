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
name, a last name, and an email address. It will look like this.

<form class="pure-form">
  <fieldset>
        <legend>User Information</legend>

        <div class="pure-g">
          <div class="pure-u-1 pure-u-md-1-3">
            <input type="text" placeholder="First Name" class="pure-u-23-24">
          </div>
          <div class="pure-u-1 pure-u-md-1-3">
            <input type="text" placeholder="Last Name" class="pure-u-23-24">
          </div>
          <div class="pure-u-1 pure-u-md-1-3">
            <input type="email" placeholder="Email" class="pure-u-23-24">
          </div>
        </div>
    </fieldset>
</form>

Because our components act like custom HTML elements, that is, custom HTML tags,
it carries forward that metaphor by passing data to the component through
attributes on the tag in the HTML that we write. 

## Simple Strings

