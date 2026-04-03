# Writing Your Own Widget

Your starting point when writing a jQuery UI widget will be the [official documentation](https://jqueryui.pbworks.com/w/page/12138135/Widget%20factory), but since the docs can be a bit dry, I'll try to present and adapt the information in a more digestible way.

The first thing worth mentioning is that the rules for writing jQuery plugins are too loose, which doesn't help their quality. When creating jQuery UI, they apparently decided to go the route of standardizing the plugin and widget writing process. I can't say how well the idea worked out, but things definitely got better than before. Let me start with the widget skeleton:

```javascript
$.widget("book.expose", {
  // default settings
  options: {
    color: "red"
  },
  // widget initialization
  // make DOM changes and attach handlers
  _create: function() {
    this.element;    // target object in a jQuery wrapper
    this.name;       // name - expose
    this.namespace;  // namespace – book
    this.element.on("click." + this.eventNamespace, function(){
      console.log("click");
    });
  },
  // method responsible for applying settings
  _setOption: function( key, value ) {
      // apply settings changes
      this._super( key, value );
  },
  // the _destroy method should be the opposite of _create
  // it should remove all DOM changes and remove all handlers if there were any
  _destroy: function() {
    this.element.off('.'+this.eventNamespace);
  }
});
```

For those who didn't read the comments, let me explain:

<table data-header-hidden><thead><tr><th width="287"></th><th></th></tr></thead><tbody><tr><td><code>options</code></td><td>settings storage for a specific element's widget</td></tr><tr><td><code>_create()</code></td><td>responsible for widget initialization — this is where DOM changes and event handler attachment should happen</td></tr><tr><td><code>_destroy()</code></td><td>the opposite of <code>_create()</code> — should clean up everything we've messed up</td></tr><tr><td><code>_setOption(key, value)</code></td><td><p>this method will be called when trying to change any settings:</p><pre class="language-javascript"><code class="lang-javascript">$("#my").expose({key:value})
</code></pre></td></tr></tbody></table>

{% hint style="info" %}
A keen eye will notice that all the listed methods start with an underscore — this is a way to mark "private" methods that can't be invoked externally. If we try to run `$('#my').expose('_destroy')`, we'll get an error. But keep in mind — this is just a convention, respect it!

To bypass the privacy convention, you can use the `data()` method:

```javascript
$("#my").data("expose")._destroy() // insert evil smiley here
```
{% endhint %}

In this example, I tried to set good widget-writing practices — I attached event handlers in a `namespace`. This will let you control what's happening later without needing to dig into the widget code. True story.

> The code in the `_destroy()` method is redundant, since it's already executed in the public `destroy()`. It's included here for clarity.

And for the lazy folks, so you don't have to specify `eventNamespace` in event handlers every time, the developers added two methods in version 1.9.0: `_on()` and `_off()`. The first one accepts two parameters:

* a DOM element, or selector, or jQuery object
* a set of event handlers as an object

All listed events will be attached in the `eventNamespace` space, meaning the result should be the same:

```javascript
this._on(this.element, {
  mouseover:function(event) {
    console.log("Hello mouse");
  },
  mouseout:function(event) {
    console.log("Bye mouse");
  }
});
```

The second method, `_off()`, lets you selectively disable handlers:

```javascript
this._off(this.element, "mouseout click");
```

Alright, enough with the skeleton — time to move on to functionality. Let's add a custom function with custom functionality:

```javascript
callMe:function() {
  console.log("Hello?");
}
```

We can easily access this function both from other widget methods and from outside:

```javascript
// from inside
this.callMe()
// from outside
$("#my").expose("callMe")
```

If your function accepts parameters, they're passed like this:

```javascript
$("#my").expose("callMe", "Hello!")
```

If you want to access a widget method from an event handler, don't forget about variable scope and make the following maneuver:

```javascript
{
  _create: function() {
    var self = this; // there it is!
    this.element.on("click."+this.eventNamespace, function() {
      // here we use self, since this already points to
      // the element being clicked
      self.callMe();
    })
  }
}
```

Looking good. Now let's talk about events. For more flexible widget development and integration, there's functionality for creating custom events and "listening" to them:

```javascript
// trigger an event
this._trigger("incomingCall");

// subscribe to an event during widget initialization
$("#my").expose({
  incomingCall: function(ev) {
    console.log("ding-dong");
  }
})

// or after, using widget name + event name as the event name
$("#my").on("exposeincomingCall", function() {
  console.log("tra-la-la")
});
```

That's a lot of material, I know, but let me add descriptions of a few more methods that can be called from within the widget:

`_delay()` — this function works like `setTimeout()`, except the context of the passed function will point to the widget itself (so you don't have to fuss with scope)

`_hoverable()` and `_focusable()` — you need to feed these methods elements for which you want to track `hover` and `focus` events, so the classes `ui-state-hover` and `ui-state-focus` are automatically added when those events occur

`_hide()` and `_show()` — these two methods appeared in version 1.9.0, they were created to standardize widget behavior when using animation methods; the settings are typically stored in options under the "hide" and "show" keys respectively. Here's how to use them:

```javascript
{
  options: {
    hide: {
      effect: "slide",     // settings equivalent to calling
      duration: 500        // .hide("slide", 500)
    }
  }
}
// inside the widget, use _hide() and _show() calls
this._hide( this.element, this.options.hide, function() {
  // this is our callback function
  console.log('hidden');
});
```

There's also a couple of methods that are already implemented for us:

```javascript
{
  enable: function() {
    return this._setOption( "disabled", false );
  },
  disable: function() {
    return this._setOption( "disabled", true );
  }
}
```

Essentially, these functions create a shortcut for calling:

```javascript
$("#my").expose({ "disabled": true }) // or false
```

Our job boils down to just tracking this flag in the `_setOption()` method.

This widget might not become popular, but it clearly demonstrates how to create widgets for jQuery UI:

{% embed url="https://anton.shevchuk.name/book/code/widget.html" %}

{% hint style="warning" %}
Be careful — with the release of jQuery UI version 1.9.0, changes were made to the Widget API, so most of the available information is outdated. Read the [official documentation](https://jqueryui.pbworks.com/w/page/12137947/FrontPage). Or better yet — peek into the code of ready-made widgets from the manufacturer.
{% endhint %}

Resources on widget development:

* [Understanding jQuery UI widgets: A tutorial](https://bililite.com/blog/understanding-jquery-ui-widgets-a-tutorial/)
* [Coding your First jQuery UI Plugin](https://net.tutsplus.com/tutorials/javascript-ajax/coding-your-first-jquery-ui-plugin/)
