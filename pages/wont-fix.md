<script>{
	"title": "Won't Fix"
}</script>

## jQuery Core

jQuery Core Won't-Fix issues are tracked and explained on a [jQuery Wiki page](https://github.com/jquery/jquery/wiki/Won't-Fix).

## jQuery UI

### change Event

Widgets that enhance form elements manipulate their element's `value` programmatically, 
therefore we cannot guarantee that a native `change` event will fire when the element's 
`value` changes.

These widgets do all provide a custom `change` event.  For example, the autocomplete 
widget fires an [autocompletechange](http://api.jqueryui.com/autocomplete/#event-change) event.

**For further reference**:

* [#8878]( http://bugs.jqueryui.com/ticket/8878 )

### Cloning

Cloning widgets is not supported by jQuery UI.  It is simply too much effort specific 
to each widget to handle for an uncommon use case.  If you are interested in adding
this functionality feel free to contact the jQuery UI team.

**For further reference**:

* [#3803]( http://bugs.jqueryui.com/ticket/3803 )
