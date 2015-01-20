<script>{
	"title": "CSS Style Guide"
}</script>

This page outlines the style guide for CSS across all jQuery projects.

## Linting

Use CSSLint to detect errors and potential problems. Most jQuery projects have a Grunt task for linting all CSS files: `grunt csslint`. The options for CSSLint are stored in a `.csslintrc` file.

Each `.csslintrc` file follows a specific format. All options must be alphabetized and grouped:

```json
{
	"common1": true,
	"common2": true,

	"repo-specific1": true,
	"repo-specific2": true
}
```

The following common options must be used in all projects with css:

```json
{
	"adjoining-classes": false,
	"box-model": false,
	"box-sizing": false,
	"compatible-vendor-prefixes": false,
	"duplicate-background-images": false,
	"import": false,
	"important": true,
	"outline-none": false,
	"overqualified-elements": false,
	"text-indent": false
}
```

## Spacing

In general, the jQuery style guide encourages liberal spacing for improved human readability. The minification process creates a file that is optimized for browsers to read and process.

- Indentation with tabs.
- No whitespace at the end of line or on blank lines.
- Use a space after a property name's colon, do not use a space before the colon.
- Put a space before `{` in rule declaration.
- Place the closing brace of a rule declaration on a new line.
- Put a blank line between rule declarations to make the CSS readable.
- When listing multiple selectors associated with the same CSS rule, add a new line after each `,`.
- The only time you should indent selectors is within nested rule declarations, like media queries.
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
	font-style: italic;
}

@media only all {
	.media-example {
		font-weight: strong;
	}
}
```

## Formatting

 - Use hex color codes (`#5c8fb3`) unless using `rgba()`.
 - When possible, use shorthand hex values.
 - Use lower case letters when declaring hex codes.
 - Use `em` over `px`, unless unavoidable.
 - Don't specify units for 0 values.
 - Avoid cascading selectors that are more than 3 selectors long.
 - Avoid unnecessary cascading selectors.
 - Use shorthand properties when possible. 
  - Instead of writing out `padding-left: 10px; padding-right: 10px; padding-top: 20px; padding-bottom: 5px;`, shorten it to one rule: `padding: 20px 10px 5px 10px;`. However, if you're only changing the left padding, then it is appropriate to use `padding-left`. 
 - Omit leading zeros in decimal values.
 - Use double quotes instead of single quotes.
 - Always have a semicolon at the end of each declaration.
 - Avoid the use of `!important`.


```css
/* Good CSS Example*/
.good-table thead th {
	color: #fff;
	background: #ffc0cb url("../images/polka-dots.png");
	font-size: 1em;
	padding: .5em 0;
}

.good-table thead th.selected {
	padding-left: 1em;
}
```

## Imports

- Don't use `@import` in distribution files. It is slower, adds extra page requests, and can cause other unforeseen problems.
- Do not use more than 31 `@import`s in a single CSS file, it will break IE.
- Do not nest `@import`s, you will not be able to guarantee their order.

## Comments

Comments are always preceded by a blank line. Comments start with a capital first letter, but don't require a period at the end, unless you're writing full sentences. There must be a single space between the comment token and the comment text.

When comments take up multiple lines, use a `*` at the beginning of each line. There must be a space between the first word and the `*` on each line.

```css
/* Good single line comment */

/*
* Good example of a multi-line comment.
* If your comment takes up multiple lines, please do this.
*/
```

## Class Names

- Class names should be all lowercase letters.
- Build class names with hyphens (`-`).
- Avoid excessive and arbitrary shorthand notation for classes. `.ui-button` is useful for button, but `.b` doesn't mean anything.
- Use classes to identify selectors instead of ids.

```css
/* Bad Example */
.uiButton,
.ui_button,
#uiButton,
.b {

}

/* Good Example */
.ui-button {

}
```
