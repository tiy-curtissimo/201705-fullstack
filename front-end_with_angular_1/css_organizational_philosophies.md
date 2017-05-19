---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyNzUzMywiY29udGVudF90eXBlIjoiTGVzc29uIn0.kLnFwadVCUzm7hffSUVEP5BTc6WLKqgTD4U11aj4mh8

# Everything below this line can be edited.
title: >-
  CSS Organizational Philosophies
description: >-
  An overview and links to resources regarding BEM and SMACSS.
---

# Some Extra information (You won't be tested on it. Here.)

In an execution environment like the browser where styling rules can easily
conflict in a large application, guidelines for declaring CSS rules based on
selectors of tag names, ids, and class names have become a necessity to promote
the maintainability of non-trivial Web applications. In the crucible of
software philosophy, three schools have emerged as the thought leaders of the
differing CSS organization and naming schools. This short lesson introduces you
to them and provides links if you'd like to find out more.

# BEM

BEM stands for Block Element Modifier, the syntax for how BEM users apply names
to their elements. It is aimed at providing a series of CSS classes that
provides single-level highly-targeted rules to style elements.

The Block Element Modifier syntax manifests itself in the following manner.

<pre class="highlight css"><span class="highlight-copy-clipboard hint--left hint--rounded hint--no-animate" data-hint="Copy Code"></span><code><span class="o">.</span><i><span class="nt">block</span></i>
<span class="o">.</span><i><span class="nt">block</span></i><span class="nt">__</span><i><span class="nt">element</span></i>
<span class="o">.</span><i><span class="nt">block</span></i><span class="nt">--</span><i><span class="nt">modifier</span></i>
<span class="o">.</span><i><span class="nt">block</span></i><span class="nt">__</span><i><span class="nt">element</span></i><span class="nt">--</span><i><span class="nt">modifier</span></i>
</code></pre>

We substitute into «block» the semantic name of the block, like "header" or
"employee-card". If there's an HTML element in the block, we can further specify
it in the «element» placeholder. Finally, should something have the ability to
be in a specific state, like "focused" or "active", then we put that state name
in the «modifier» place holder.

For example, let's say we have a header in our Web page that acts as the main
header for the page. In it, you can find a logo, an input, a menu, some menu
items, and a button. From that list of different elements available in the main
header block, we could, conceivably, have the following CSS classes defined.

```css
.main-header {}
.main-header__logo {}
.main-header__input {}
.main-header__input--focused {}
.main-header__button {}
.main-header__button--focused {}
.main-header__button--pressed {}
.main-header__button--disabled {}
.main-header-menu__menu-item {}
.main-header-menu__menu-item--expanded {}
```

It is very concerned with providing a single class that represents each way that
a **block** and its components and states can express themselves. To learn more
about BEM, please go to its Web site [http://getbem.com/](http://getbem.com/).

# SMACSS

SMACSS, which stands for Scalable and Modular Architecture for CSS, goes one
step further than BEM. It also has a naming scheme, not as rigid as BEM's, which
is intimately tied into its real strength of CSS rule categorization.

SMACSS advocates that CSS rules fall into one of five categories:

* **Base rules** are rules that define the default style of HTML elements (and
  pseudo classes) in the Web site. This usually includes a CSS reset or
  normalization. All the selectors in the *base rules* are
  * an element selector, like `body, html {}`;
  * a descendant selector, like `body * {}`;
  * a child selector, like `body > * {}`; or,
  * a pseudo-class selector, like `a:hover`.
* **Layout rules** are rules that define the general layout of your Web page.
  This includes headers, footers, main content areas, call-outs, and the like.
  Generally, the selectors in the *layout rules* are
  * an id selector, like `#main-header {}`; or,
  * a class selector with an "l" prefix, like `.l-grid-md-4 {}`.
* **Module rules** are rules that define discrete, sometimes reusable components
  of your Web page. This category could include navigation bars, carousels,
  dialogs, and search panels. Modules sit inside *layout rules* but are not
  scoped by them in their selectors. Module selectors should be
  1. rooted in a meaningful class selector, like `.dialog {}`; or,
  1. rooted in a meaningful class selector with a descendant, like `.dialog
     .confirm-button {}

  Modules can be "subclassed", according to the SMACSS documentation, through
  the combination of base CSS classes like `.dialog` and more specific classes
  the combine to make something more specific, like `.dialog.file-dialog`.
* **State rules** are rules that augment and override all other styles because
  they describe a specific state for the module or piece of a module like
  "is-active", "is-expanded", or "is-error". Two key points differentiate a
  state rule from a module rule:
  * State styles can apply to both layout or modules styles; and,
  * State styles indicate a JavaScript dependency.

You can find out more from the [SMACSS Web site](https://smacss.com/) where the
author has articles, a book, and a workshop available to help you further
understand the practice.

# OOCSS

For completeness' sake, I've included OOCSS. It's an "oldie but a goodie";
however, no one really talks about it and no one updates the GitHub
repo, anymore. BEM is an implementation of this model and has superseded this
model in widespread usage.

OOCSS stands for Object-Oriented CSS. It tries to bring the philosophies of the
[single responsibility
principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) and
[separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns)
to the world of CSS naming and organization. It does this through two guiding
principles:

* Separate structure and skin
* Separate container and content

We accomplish this in OOCSS by identifying a "CSS Object" (known in other circles
as a "component") that can be modularized into an independent collection of
HTML, CSS, and JavaScript. Once a "CSS Object" is modularized, you can use it
anywhere throughout your site.

You can learn more about OOCSS at its [official and unmaintained GitHub
repo](https://github.com/stubbornella/oocss/wiki).

