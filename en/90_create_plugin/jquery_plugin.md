---
description: A rough start...
---

# Your First Plugin

First, let's remember what we need plugins for. My answer — for creating reusable code with a convenient interface. Let's write some, starting with a simple task:

_— On clicking a paragraph, the text should turn red_

## JavaScript, Not Even jQuery <a href="#javascript-jquery" id="javascript-jquery"></a>

So we don't forget our roots, let's start with a native JavaScript implementation:

```javascript
// wrap everything in a function
let loader = function () {
  // find all paragraphs
  let paragraphs = document.getElementsByTagName('P');
  // iterate and attach a handler
  for (let i = 0; i < paragraphs.length; i++) {
    // the handler itself
    paragraphs[i].onclick = function() {
      this.style.color = "#FF0000";
    }
  }
}

// naturally, all the code should run after the page is fully loaded
document.addEventListener("DOMContentLoaded", loader);
```

This code isn't universal and was written just to emphasize the convenience of using jQuery one more time ;)

## jQuery, But Not a Plugin Yet <a href="#jquery" id="jquery"></a>

Now we can simplify this code — plug in jQuery and get the following version:

```javascript
$(function(){
    $('p').click(function(){
        $(this).css('color', '#ff0000');
    })
});
```

## An Actual jQuery Plugin <a href="#jquery-plugin" id="jquery-plugin"></a>

We've accomplished the task, but where's the code reuse? Or what if we need green instead of red? This is where things get interesting. To write a simple plugin, you just need to extend the `$.fn` object:

```javascript
$.fn.mySimplePlugin = function() {
    $(this).click(function(){
        $(this).css('color', '#ff0000');
    })
}
```

If we want to do it properly, we need to scope the `$` variable to our plugin only, and also return `this` so we can use chaining. Here's how:

```javascript
(function($) {
    $.fn.mySimplePlugin = function() {
        // plugin code
        return this;
    };
})(jQuery);
```

{% hint style="info" %}
Let me add a small clarification about the "magic" happening here. The code `(function($){...})(jQuery)` creates an anonymous function and immediately invokes it, passing the `jQuery` object as a parameter. This way, inside the anonymous function we can use the `$` alias without worrying about conflicts with other libraries, since `$` now exists only within our function's scope and we have full control over it. If you're getting a deja vu feeling — that's right, I've already talked about this.
{% endhint %}

Let's add a color option and get a working plugin:

```javascript
(function($) {
    // default value - GREEN
    var defaults = { color:'green' };
    // current settings, global
    var options;
    $.fn.mySimplePlugin = function(params) {
        // on multiple calls, settings persist and get overridden as needed
        options = $.extend({}, defaults, options, params);
        $(this).click(function() {
            $(this).css('color', options.color);
        });
        return this;
    };
})(jQuery);
```

Usage:

```javascript
// first call
$('p:first').mySimplePlugin();

// second call
$('p:eq(1)').mySimplePlugin({ color: 'red' });

// third call
$('p:last').mySimplePlugin();
```

As a result, every click will change the paragraph color to red. Since we're using a global variable to store settings, the second plugin call will change the value for all elements:

{% embed url="https://anton.shevchuk.name/book/code/plugin.global.html" %}

We can make a small change and separate the settings for each call:

```javascript
// current settings will be individual for each run
var options = $.extend({}, defaults, params);
```

The difference is just one "var". I can't even imagine how many hours have been killed searching for a missing "var" in JavaScript. Be careful:

{% embed url="https://anton.shevchuk.name/book/code/plugin.html" %}

## Working with Object Collections <a href="#collections" id="collections"></a>

This is straightforward — just remember that `this` contains a jQuery object with a collection of all elements, i.e.:

```javascript
$.fn.mySimplePlugin = function(){
    console.log(this); // this is a jQuery object
    console.log(this.length); // number of elements in the selection
};
```

If we want to process each element, we build the following construct inside our plugin:

```javascript
// need to process each element in the collection
return this.each(function(){
    $(this).click(function(){
        $(this).css('color', options.color);
    });
});

// the previous version is a bit redundant,
// since the click function already iterates over elements
return this.click(function(){
    $(this).css('color', options.color);
});
```

{% hint style="warning" %}
Once again, a reminder — if your plugin isn't supposed to return something specific by design, return `this`. Chaining in jQuery is part of the magic, don't break it. The `each()` and `click()` methods return a jQuery object.
{% endhint %}

## Public Methods <a href="#public-methods" id="public-methods"></a>

Alright, we've written a "cool" plugin, and now we need to add more functionality — let the color be controlled by several buttons on the site. For this, we'll need a `color` method that will be in charge of everything. Here's the code for the finished plugin — let's study it together (pay attention to the comments):

```javascript
// settings with default values
var defaults = { color:'green' };

// our future public methods
var methods = {
    // plugin initialization
    init: function(params) {
        // settings will be individual for each run
        var options = $.extend({}, defaults, params);
        // initialize only once
        if (!this.data('mySimplePlugin')) {
            // store settings in the data registry
            this.data('mySimplePlugin', options);
            // add events
            this.on('click.mySimplePlugin', function(){
                $(this).css('color', options.color);
            });
        }
        return this;
    },
    // change color in the registry
    color: function(color) {
        var options = $(this).data('mySimplePlugin');
        options.color = color;
        $(this).data('mySimplePlugin', options);
    },
    // reset element colors
    reset: function() {
        $(this).css('color', 'black');
    }
};

$.fn.mySimplePlugin = function(method){
    // a bit of magic
    if ( methods[method] ) {
        // if the requested method exists, we call it
        // all parameters except the method name will be passed to the method
        // this will also carry over to the method
        return methods[ method ].apply( this, Array.prototype.slice.call( arguments, 1 ));
    } else if ( typeof method === 'object' || ! method ) {
        // if the first parameter is an object, or nothing at all
        // execute the init method
        return methods.init.apply( this, arguments );
    } else {
        // if nothing worked
        $.error('Method "' + method + '" not found in plugin');
    }
};
```

And here's a small example of using these methods:

```javascript
// call without parameters — init will be called
$('p').mySimplePlugin();

// call the color method, passing a color as parameter
$('p').mySimplePlugin('color', '#FFFF00');

// call the reset method
$('p').mySimplePlugin('reset');
```

{% embed url="https://anton.shevchuk.name/book/code/plugin.extend.html" %}

To understand this piece of code, you just need to wrap your head around the `arguments` variable and the `apply()` method. Here are some articles dedicated to them, go for it:

* [https://learn.javascript.ru/arguments-pseudoarray](https://learn.javascript.ru/arguments-pseudoarray)
* [https://learn.javascript.ru/call-apply](https://learn.javascript.ru/call-apply)

## About Event Handlers <a href="#event-handlers" id="event-handlers"></a>

If your plugin attaches any handler, it's best (read — always) to attach that handler in its own namespace:

```javascript
return this.on("click.mySimplePlugin", function(){
    $(this).css('color', options.color);
});
```

This trick lets you remove all your handlers at any time or trigger only yours, which is super convenient:

```javascript
// trigger only our handler
$('p').trigger("click.mySimplePlugin");

// remove all our handlers
$('p').off(".mySimplePlugin");
```

> — Deja vu? Ok!

{% hint style="info" %}
Here's a bit more food for thought:

&#x20;— [Essential jQuery Plugin Patterns](https://www.smashingmagazine.com/2011/10/essential-jquery-plugin-patterns/)
{% endhint %}

That's all about regular plugins. Thanks for your attention, everyone.
