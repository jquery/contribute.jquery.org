---
title: CSS Style Guide
---

This page outlines the style guide for CSS across all jQuery projects.


## Spacing

In general, the jQuery style guide encourages liberal spacing for improved human readability. The minification process creates a file that is optimized for browsers to read and process.

- Indentation with tabs.
- No whitespace at the end of line or on blank lines.
- Use a space after a property name’s colon.
- Put a space before `{` in rule delectations.
- Place closing braces of declaration blocks on a new line.
- Put line breaks between rulesets to make the CSS readable.
- When listing multiple elements associated with the same rule declaration, place each element on it's own line.
- Do not nest CSS elements with tabs.
- CSS rulesets should be indented by one tab.
- No spaces within `()` for urls.

## Formatting
 - Use hex color codes `#000000` unless using `rgba()`.
 - Use `em` over `px`, unless unavoidable.
 - Avoid specifying units for 0 values (use `0` instead of `0em`).
 - Avoid cascading elements more than 3 elements.
 - Use `/* */` for comments instead of `//`.
 - Avoid uppercase letters. Instead of camel-case, separate out css classes with `-`.
 - use shorthand properties when possible.

 ```css
 		margin: 0 10em 5em 10em
 ```
   instead of
 ```css
 		margin-left: 10em;
 		margin-top: 0;
 ```
 - Omit leading zeros in decimal values (`.3125em` instead of `0.3125em`).
 - Use double quotation marks instead of single quotes.
 - Use classes to identify elements instead of ids.


 - Above all, use valid CSS when possible
