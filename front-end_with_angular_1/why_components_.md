---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyMTk3MCwiY29udGVudF90eXBlIjoiTGVzc29uIn0.gRLiyIouuPv_9z5tPOzXvqI0D5BIZ3R6hBP-2-dCj6Y

# Everything below this line can be edited.
title: >-
  Why Components?
description: >-
  The reason for another kind of "thing" in Angular 1.
---

There are two answers to the question "Why did Angular 1 introduce the
`.component()` method?" This lesson addresses those two answers in preparation
for the next lesson which shows us how to create and use components in our
Angular 1 applications.

## The Pragmatic Answer: An Upgrade Path

When Angular 1 premiered, it represented the most promising and complete
client-side framework for creating complex browser-based applications. The fact
that it came from Google made it an even more appealing choice for developers
and software development companies that relied on the thought that it would
present a stable platform on which to build Web applications for years to come. 

In September 2014, the Angular team announced that Angular 2 would not only
represent a complete rewrite of the framework, but that they had no intent on
keeping it backward compatible with code written with the Angular 1 framework.
This directly contradicted many adopters' perceptions of the stability of the
Angular source code. While many of those companies and developers wanted to
abandon their current Angular code in favor of a framework that hadn't
betrayed their trust, their investment in their current products prevented them
from easily switching to something else.

At the beginning of February 2016, the Angular team released Angular 1.5 which
contained a new way to define reusable pieces of UI: the __component__. This new
building block combined the semantics of a __directive__ based on a tag name
with a __controller__ while creating a new, isolated scope for the instance. It
provided a bridge between the `@Component` concept in Angular 2 and the existing
functionality of Angular 1.5. This small but heartening development provided
consumers of Angular the opportunity to focus their energies on making a small
step of refactoring their existing Angular 1 code-bases to use the new
`.component()` method. Then, when they found the time and energy to upgrade
the code base to Angular 2, they would find it less troublesome because their
code would now have the same organization as demanded by Angular 2's
expectations.

## The Philosophical Answer: It's Always Been About Components

Angular 1 has always been about components, just under different names.
Directives encapsulate behavior and DOM manipulation under tag or attribute
names. Services encapsulate behavior for sharing across directive concerns.
Controllers encapsulate rendering behavior for sections of a page. These are all
aspects of a componentized architecture. The `.component()` method just takes
the ideas one step further by providing sane defaults for the mixture of
behavior, DOM manipulation and generation, and reliance on shared state.

But more than just Angular 1, the idea of a "component" has become central to
competing frameworks such as React[^1] and knockout.js[^2]. The emergence of
this concept steered the Angular team to reconsider the direction of the
development of the framework. If they wanted to remain relevant, they had to
produce something that provided a component-based perspective.

Fundamental to the on-going advancement of Web standards, the Web component
family of standards[^3] has allowed for native support for custom tags
indistinguishable from HTML tags present in the standard. This means that your
custom tag `<custom-button>` would be treated no differently than the `<button>`
element built into the HTML standard. More importantly, it sparked a new way
to declaratively program Web browsers. Only with a strong component-based
philosophy would this be possible.

There should be no surprise that Angular jumped on the component bandwagon. The
only surprise should be that it took them years to get it implemented into their
flagship product at the time, their version 1.

[^1]: [https://facebook.github.io/react/](https://facebook.github.io/react/)
[^2]: [http://knockoutjs.com/](http://knockoutjs.com/)
[^3]: [https://www.webcomponents.org/](https://www.webcomponents.org/)
