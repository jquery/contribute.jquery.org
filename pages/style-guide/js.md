---
title: JavaScript Style Guide
toc: true
---

## Linting

Use JSHint to detect errors and potential problems. Every jQuery project has a
Grunt task for linting all JavaScript files: `grunt jshint`. The options for
JSHint are stored in a `.jshintrc` file; many repositories will have multiple
`.jshintrc` files based on the type of code in each directory.

Each `.jshintrc` file follows a specific format. All options must be alphabetized
and grouped:

```json
{
	"common1": true,
	"common2": true,

	"repoSpecific1": true,
	"repoSpecific2": true,

	"globals": {
		"repoGlobal1": true,
		"repoGlobal2": false
	}
}
```

The following common options must be used in all projects:

```json
{
	"boss": true,
	"curly": true,
	"eqeqeq": true,
	"eqnull": true,
	"expr": true,
	"immed": true,
	"noarg": true,
	"onevar": true,
	"quotmark": "double",
	"smarttabs": true,
	"trailing": true,
	"undef": true,
	"unused": true
}
```

## Spacing

In general, the jQuery style guide encourages liberal spacing for improved human readability. The minification process creates a file that is optimized for browsers to read and process.

- Indentation with tabs.
- No whitespace at the end of line or on blank lines.
- Lines should be no longer than 80 characters, and must not exceed 100 (counting tabs as 4 spaces).
- `if`/`else`/`for`/`while`/`try` always have braces and always go on multiple lines.
- Unary special-character operators (e.g., `!`, `++`) should not have space next to their operand.
- Any `,` and `;` should not have preceding space.
- Any `;` used as a statement terminator should be at the end of the line.
- Any `:` following a property name in an object definition should not have preceding space.
- The `?` and `:` in a ternary conditional should have space on both sides.
- No filler spaces in empty constructs (e.g., `{}`, `[]`, `fn()`)
- New line at the end of each file.

### Bad Examples

```js

// Bad
if(condition) doSomething();
while(!condition) iterating++;
for(var i=0;i<100;i++) object[array[i]] = someFn(i);

```

### Good Examples

```js
var i = 0;

if ( condition ) {
	doSomething();
} else if ( otherCondition ) {
	somethingElse();
} else {
	otherThing();
}

while ( !condition ) {
	iterating++;
}

for ( ; i < 100; i++ ) {
	object[ array[ i ] ] = someFn( i );
}

try {
	// expressions
} catch ( e ) {
	// expressions
}
```


### Objects

Object declarations can be made on a single line if they are short (remember the line length limits). Otherwise they should be broken out one property per line. Property names only need to be quoted if they are reserved words or contain special characters:

```js
// Bad
var map = { ready: 9,
	when: 4, "you are": 15 };

// Good
var map = { ready: 9, when: 4, "you are": 15 };

// Good as well
var map = {
	ready: 9,
	when: 4,
	"you are": 15
};
```


### Arrays and Function Calls

Always include extra spaces around elements and arguments:

```js
array = [ "*" ];

array = [ a, b ];

foo( arg );

foo( "string", object );

foo( options, object[ property ] );

foo( node, "property", 2 );
```

Exceptions:

```js
// Function with a callback, object, or array as the sole argument:
// No space on either side of the argument
foo({
	a: "alpha",
	b: "beta"
});

// Function with a callback, object, or array as the first argument:
// No space before the first argument
foo(function() {
	// do stuff
}, options );

// Function with a callback, object, or array as the last argument:
// No space after after the last argument
foo( data, function() {
	// do stuff
});
```


### Multi-line Statements

When a statement is too long to fit on one line, line breaks should occur after an operator.

```js
// Bad
var html = "<p>The sum of " + a + " and " + b + " plus " + c
	+ " is " + (a + b + c);

// Good
var html = "<p>The sum of " + a + " and " + b + " plus " + c +
	" is " + (a + b + c);
```

Lines should be broken into logical groups if it improves readability, such as splitting each expression of a ternary operator onto its own line even if both will fit on a single line.

```js
var firstCondition( foo ) && secondCondition( bar ) ?
	doStuff( foo, bar ) :
	doOtherStuff( foo, bar );
```

Conditionals that need to be broken onto multiple lines should be indented one extra level to distinguish the successive lines of conditionals from the body.

```js
	if ( fistCondition() && secondCondition() &&
			thirdCondition() ) {
		doStuff();
	}
```


### Chained Method Calls

Chained method calls that need to be broken onto multiple lines should have one call per line, with the first call on a separate line from the object the methods are called on. If the method changes the context, an extra level of indentation should be used.

```js
elements
	.addClass( "foo" )
	.children()
		.html( "hello" )
	.end()
	.appendTo( "body" );
```

## Assignments

Assignments should always have a semicolon after them.

Semicolons should always be followed by a newline.

Assignments in a declaration should always be on their own line. Declarations that don't have an assignment should be listed together at the start of the declaration. For example:

```js
// Bad
var foo = true;
var bar = false;
var a;
var b;
var c;

// Good
var a, b, c,
	foo = true,
	bar = false;
```

## Equality

Strict equality checks (`===`) should be used in favor of abstract equality checks (`==`). The _only_ exception is when checking for `undefined` and `null` by way of `null`.

```js
// Check for both undefined and null values, for some important reason.
undefOrNull == null;
```

## Type Checks

- String: `typeof object === "string"`
- Number: `typeof object === "number"`
- Boolean: `typeof object === "boolean"`
- Object: `typeof object === "object"`
- Plain Object: `jQuery.isPlainObject( object )`
- Function: `jQuery.isFunction( object )`
- Array: `jQuery.isArray( object )`
- Element: `object.nodeType`
- null: `object === null`
- null or undefined: `object == null`
- undefined:
	- Global Variables: `typeof variable === "undefined"`
	- Local Variables: `variable === undefined`
	- Properties: `object.prop === undefined`


## Comments

Comments are always preceeded by a blank line.

Single line comments go __over__ the line they refer to:

```js
// We need an explicit "bar", because later in the code foo is checked.
var foo = "bar";
```

For long comments (spanning several lines), use:

```js
/*
Four score and seven—pause—minutes ago...
*/
```

Inline comments are allowed as an exception when used to annotate special arguments in formal parameter lists:

```js
function foo( types, selector, data, fn, /*INTERNAL*/ one ) {
	// do stuff.
}
```

Do not write API documentation in comments. API documentation lives in its own repository.

## Quotes

jQuery uses double quotes.

```js
var double = "I am wrapped in double quotes";
```

Strings that require inner quoting must use double outside and single inside.

```js
var html = "<div id='my-id'></div>";
```

## Semicolons

Use them. Never rely on ASI.

## Naming Conventions

Variable and function names should be full words, using camel case with a lowercase first letter. Names should be descriptive but not excessively so. Exceptions are allowed for iterators, such as the use of `i` to represent the index in a loop. Constructors do not need a capital first letter.

## Global Variables

Each project may expose at most one global variable.

## DOM Node Rules

`.nodeName` should always be used in favor of `.tagName`.

`.nodeType` should be used to determine the classification of a node (not `.nodeName`).
