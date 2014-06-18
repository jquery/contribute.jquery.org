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
- When listing multiple elements associated with the same rule declaration,
 place each element on it's own line.
- Do not nest CSS elements with tabs.
- CSS rulesets should be indented by one tab.
- No spaces within `()` for urls.
- The only time you tab in elements is within a media query.

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
 - Use lower case letters when declaring hex codes.
 - Use `em` over `px`, unless unavoidable.
 - Avoid specifying units for 0 values.
 - Avoid cascading elements more than 3 elements.
 - Avoid unnecessary cascading elements.
 - Use `/* */` for comments.
 - Avoid uppercase letters. Instead of camel-case, separate out css classes with `-`.
 - Use shorthand properties when possible.
 - Omit leading zeros in decimal values.
 - Use double quotation marks instead of single quotes.
 - Use classes to identify elements instead of ids.

```css
/*
  Bad CSS
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

body {
  color: #ffffff;
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
