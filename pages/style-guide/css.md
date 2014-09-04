---
title: CSS Style Guide
---

This page outlines the style guide for CSS across all jQuery projects.

## Linting
Use `csslint` to detect errors and potential problems. Most jQuery projects have a
Grunt task for linting all css files: `grunt csslint`. The options for `csslint`
are stored in a `.csslintrc` file.

Each `.csslintrc` file follows a specific format. All options must be alphabetized
and grouped:

```json
{
	"adjoining-classes": false,
	"box-model": false,
	"compatible-vendor-prefixes": false,
	"duplicate-background-images": false,
	"import": false,
	"important": false,
	"outline-none": false,
	"overqualified-elements": false,
	"text-indent": false
}
```

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
- When listing multiple selectors associated with the same CSS rule,
place each selector on it's own line.
- Do not nest CSS selectors with tabs.
- The only time you should tab in selectors is within a media query.
- CSS rules' properties should be indented by one tab.
- No spaces within `()` for urls.

```css

/* Bad CSS */

   .bad-spacing,.bad-example{color:#222222;background:url( "../bad.png" );}

     .bad-spacing{font-style:italic}

@media only all{

.media-example{
font-weight:strong;
}  

}

/* Good CSS */
.good-spacing,
.good-example {
  color: #222;
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
 - Use hex color codes `#5c8fb3` unless using `rgba()`.
 - When possible, use shorthand hex values.
 - Use lower case letters when declaring hex codes.
 - Use `em` over `px`, unless unavoidable.
 - Avoid specifying units for 0 values.
 - Avoid cascading selectors that are more than 3 selectors long.
 - Avoid unnecessary cascading selectors.
 - Use shorthand properties when possible.
 - Omit leading zeros in decimal values.
 - Use double quotation marks instead of single quotes.
 - Always have a semi-colon at the end of each ruleset.
 - Don't use `@import` in distribution files. It is slower, adds extra page
 requests, and can cause other unforeseen problems.
 - Do not use more than 31 `@imports` in a single css file, it will break IE.
 - Do not nest `@imports`.  You will no longer be able to guarantee their order.


```css

/* Bad CSS Example */
table.badCSS thead tr th{
  color: #FFFFFF;
  background: pink url('images/polka-dots.png');
  font-size: 12px;
  padding-top: 0.5em;
  padding-bottom: 0.5em;
}

/* Good CSS Example*/

.goodTable thead th{
  color: #fff;
  background: #ffc0cb url("images/polka-dots.png");
  font-size: 1em;
  padding: .5em 0;
}

```


## Comments
- Use `/* */` for comments.
- When using comments, have the comment notation (`/* */`) be on the
same line as the rest of the comment. If writing a comment that takes up
multiple lines, use the comment notation on each line.
- Have a space between first word and comment opening.
- Have a blank line before each comment.

```css

/*
  Bad Comment Example
  Do not do this, please.
*/

/* Good comment Example */
/* An example of a good comment */
```


## Class Names

 - Avoid the use of uppercase letters. Instead of camelCase,
 separate out css classes with dashes (`-`).
 - Do not use underscores (`_`) in class names.
 - Avoid excessive and arbitrary shorthand notation for classes.
 `.btn` is useful for button, but `.s` doesn't mean anything.
 - Use classes to identify selectors instead of ids.


```css
/* Bad Example */
.cssButton,
.css_button,
#cssButton,
.b {

}

/* Good Example */
.css-btn{

}

```





Above all, use valid CSS when possible
