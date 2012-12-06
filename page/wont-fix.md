---
title: Won't Fix
---

## Object.prototype Issues

Modifying the Object.prototype is a bad idea - it'll break a lot of code, and
not just in jQuery. While the fix inside jQuery would be relatively simple
(using hasOwnProperty in a number of loops) the performance overhead would be
considerable, thus we're not inclined to fix issues related to this.

Reference: [Object.prototype is verboten](http://erik.eae.net/archives/2005/06/06/22.13.54/)

**For further reference**: [ #2721 ]( http://bugs.jquery.com/ticket/2721 )

## SVG/XML/VML Bugs

We simply don't have the time to work on all the issues related to SVG/XML/VML
documents and jQuery. If you wish to help out with this effort, help would be
appreciated.

**For further reference**:

* [#4208](http://bugs.jquery.com/ticket/4208)
* [#7071](http://bugs.jquery.com/ticket/7071)
* [#7584](http://bugs.jquery.com/ticket/7584)
* [#9807](http://bugs.jquery.com/ticket/9807)

## Warnings in Console

There are some limited cases where an unavoidable warning (not an error) is
produced in the Firefox console. This has been resolved in later versions of
Firefox - and fixing it in jQuery itself would result in a compromised
experience for users. A similar problem exists in some versions of Chrome,
which generate console warnings.

**For further reference**:

* [#7535](http://bugs.jquery.com/ticket/7535)
* [#10531](http://bugs.jquery.com/ticket/10531)

## CSS Transition/Animation

While some browsers do provide native means of doing CSS animations, most of
that has been encapsulated into plugins. We will continue to recommend that
users use those plugins for that particular functionality.

**For further reference**: [#4171](http://bugs.jquery.com/ticket/4171)

Plugins available:

* [jQuery Animate Enhanced](https://github.com/benbarnett/jQuery-Animate-Enhanced/)
* [jquery.transform.js](https://github.com/louisremi/jquery.transform.js)
* [jQuery++ Animate](http://jquerypp.com/#animate)

## Inline Event Handlers

In general, inline event handlers are incredibly hard to work with and should
not be used. In nearly all cases, issues involving inline event handlers are
out of scope.

**For further reference**: http://www.quirksmode.org/blog/archives/2005/06/three_javascrip_1.html

## Quirks Mode

Some browsers provide a "Quirks Mode" that does not conform to the standard W3C
CSS box model; jQuery does not support this mode due to the differences it
introduces in measuring element dimensions and positions. To ensure that an
HTML document does not render in Quirks Mode, always use a doctype at the top
of the document.

**For further reference**: http://en.wikipedia.org/wiki/Quirks_mode

