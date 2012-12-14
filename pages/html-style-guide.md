---
title: HTML Style Guide
---

This page outlines the style guide for HTML pages in all jQuery projects.
These rules apply to web sites, demo pages, inline examples, test pages, etc.
Exceptions are allowed for pages that must violate the rules by their very
nature, e.g., a page that tests XHTML.

## Doctype
Always use the minimal, versionless doctype.

```
<!doctype html>
```

## Language

Always define which language the page is written in.

```
<head lang="en">
```

## Encoding

Always define the character encoding. The encoding should be defined as early as possible.

```
<meta charset="utf-8">
```

## Elements and Attributes

All element and attribute names should be lowercase. Attribute values should be quoted. Optional closing tags should be included. Self-closing elements should not be closed. Optional attributes should be omitted. Always include `html`, `head`, and `body` tags.

*   No `type` or `language` attributes on `script` tags.
*   No `type` attribute on `link` or `style` tags.

```
<script src="..."><script>
<script></script>
<link rel="stylesheet" href="...">
<style></style>
```

## Indentation

Don't indent inside `html`, `body`, `script`, or `style`. Indent inside `head` and all other elements.

### Example

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Sample Page</title>

  <link rel="stylesheet" href="/style.css">
  <style>
  body {
    font-size: 100em;
  }
  </style>

  <script src="/jquery.js"></script>
  <script>
  $(function() {
    $( "p" ).text( $.fn.jquery );
  });
  </script>
</head>
<body>

  <p>jQuery is awesome!<p>

</body>
</html>
```
