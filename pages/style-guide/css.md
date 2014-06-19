---
title: CSS Style Guide
---

This page outlines the style guide for CSS across all jQuery projects.


## Spacing

In general, the jQuery style guide encourages liberal spacing for
improved human readability. The minification process creates a file
that is optimized for browsers to read and process.

- Indentation with tabs.
- No whitespace at the end of line or on blank lines.
- Use a space after a property name’s colon.
- Put a space before `{` in rule declaration.
- Place closing braces of declaration blocks on a new line.
- Put line breaks between rulesets to make the CSS readable.
- When listing multiple selectors associated with the same rule declaration,
 place each selector on it's own line.
- Do not nest CSS selectors with tabs.
- CSS rulesets should be indented by one tab.
- No spaces within `()` for urls.
- The only time you tab in selectors is within a media query.

```css
/* Bad CSS */

   .bad-spacing,.bad-example{color:#222222;background:url( ".../bad.png" );}

     .bad-spacing{font-style:italic}

@media only all{

.media-example{
font-weight:strong;
}  

}

/* Good CSS */
.good-spacing,
.good-example {
  color: #222222;
  background: url("../good.png");
}

.good-spacing {
  font-style:italic;
}

@media only all {
  .media-example {
    font-weight: strong;
  }
}


```

## Formatting
 - Use hex color codes `#000000` unless using `rgba()`.
 - When possible, use shorthand hex values.
 - Use lower case letters when declaring hex codes.
 - Use `em` over `px`, unless unavoidable.
 - Avoid specifying units for 0 values.
 - Avoid cascading selectors that are more than 3 selectors long.
 - Avoid unnecessary cascading selectors.
 - Use `/* */` for comments.
 - Have when comments, have the comment notation (`/* */`) be on the
 same line as the rest of the comment. If you have multiple lines of comment,
 use the comment notation on each line.
 - Avoid the use of uppercase letters. Instead of camelCase,
 separate out css classes with dashes (`-`).
 - Do not use underscores (`_`) in class names.
 - Avoid excessive and arbitrary shorthand notation for classes.
 `.btn` is useful for button, but `.s` doesn't mean anything.
 - Use shorthand properties when possible.
 - Omit leading zeros in decimal values.
 - Use double quotation marks instead of single quotes.
 - Use classes to identify selectors instead of ids.
 - Don't use `@import` in production. It is slower, adds extra page
 requests, and can cause other unforeseen problems.
 - Always have a semi-colon at the end of each ruleset.

```css
/*
  Bad CSS
  comments
*/
body.badCSS {
  color: #FFFFFF;
  background: pink;
  font-size: 12px;
}

body section .DESCRIPTION p {
  margin-top: 0.5em;
  margin-bottom: 0.5em;
}

#submitButton {
  background: url('images/next.png');
}

/* Good CSS */
/* comments */

body {
  color: #fff;
  background: #f57ce5;
  font-size: 1.5em;
}

p {
  margin: .5em 0;
}

.submit-button{
  background: url("images/next.png");
}


```


Above all, use valid CSS when possible
