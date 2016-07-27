<script>{
	"title": "jQuery's Style Guides"
}</script>

Style guides aim to make a code base appear as if it were written by one meticulous programmer. As with many aspects of coding and writing, there can be differences of opinion about the best style. Contentious issues like "spaces versus tabs" or "semicolons or not" distract from the goal of writing code that works.

Long term, we would like to have formatting automatically done by the build process, so that the resulting code would always follow the style guide. We have taken a few steps in that direction. For example, our projects contain an [EditorConfig file](http://editorconfig.org) that many programming editors can use to enforce the space and tab rules. Our [ESLint](http://eslint.org/) configuration checks for common style _faux pas_ such as trailing spaces on a line, or using a variable before it is defined. Some projects use [JSHint](http://jshint.com/) or [JSCS](http://jscs.info/).

jQuery Foundation projects may choose any style guide that fits their team's preferences; check with the project before starting work to see what style they follow. The style guides here are generally known as the "jQuery Style" and are used by jQuery, jQuery UI, and jQuery Mobile. Many projects outside the Foundation use this style as well.


### [Prose Style Guide](/style-guide/prose/)

### [JavaScript Style Guide](/style-guide/js/)

### [HTML Markup Style Guide](/style-guide/html/)

### [CSS Style Guide](/style-guide/css/)
