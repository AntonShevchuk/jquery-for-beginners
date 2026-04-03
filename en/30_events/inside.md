---
description: A few technical details about jQuery's inner workings
---

# Appendix

## Shorthand Methods

We've already gotten acquainted with the `click()` shorthand method. In reality, this method is just a wrapper for calling `on()` and `trigger()`:

```javascript
jQuery.fn.click = function( data, fn ) {
    if (arguments.length > 0) {
        this.on("click", fn);
    } else {
        this.trigger("click");
    }
};
```

> Oops, I tweaked the code a tiny bit for readability. If curiosity gets the better of you, search the source code for the string "`dblclick`"

## Event Handlers on Objects

You can attach event handlers to practically any object:

```javascript
// an object that couldn't be simpler
var obj = {
    test:function() {
        console.log("obj.test");
    }
}

// create a handler for a custom event someEvent
$(obj).on("someEvent", function(){
    console.log("obj.someEvent");
    this.test();
});

// let's see the result
console.log(obj);

// trigger the someEvent event
$(obj).trigger("someEvent");
```

> Copy the code above into the console and run it — I think you'll find it interesting ;)

## Where Are Event Handlers Stored?

Inside jQuery itself :) If you want to get all event handlers, you'll need the following method:

```javascript
// here element is a DOMElement
$._data(element, "events");
```

{% hint style="warning" %}
In the jQuery source code comments there's this phrase: "_Never expose "private" data to user code (TODO: Drop _data, _removeData)_". So one fine day this section will become outdated.
{% endhint %}

> Back in the day, all event handlers were available in the `$.cache` object, but in recent versions of jQuery this object no longer exists :(
