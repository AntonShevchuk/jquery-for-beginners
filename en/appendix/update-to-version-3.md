# Upgrading to Version 3.x

First and foremost, this update affected old browser versions — specifically, the following versions are now supported:

* Internet Explorer 9+
* Chrome, Edge, Firefox, and Safari — thanks to built-in update systems in these browsers, the current and previous versions are supported
* Opera — not a favorite, so only the current version is supported
* Safari Mobile iOS 7+
* Android 4+

Beyond that, many changes were introduced that break backward compatibility with previous versions. I won't list them all — I'll focus on the ones I've already had to deal with.

> Most of the changes listed apply not only to version 3.x, but also to older branches starting from versions 1.12.x and 2.2.x

## AJAX <a href="#ajax" id="ajax"></a>

*   The `jQuery.ajax()` method is now compatible with Promise, and you can use `then()` and `catch()` methods:

    <pre class="language-javascript"><code class="lang-javascript">$.ajax({url:"/get-my-page.html" /* etc. */ })
    <strong> .then(function() { /* all OK */ })
    </strong> .catch(function() { /* error */ })
    </code></pre>
* A new signature has been added for calling two AJAX methods `$.get(settings)` and `$.post(settings)` — the settings are now compatible with `$.ajax(settings)`.
* When loading scripts from another domain, specifying `dataType: "script"` is now mandatory.
* When making AJAX requests with a URL containing a hash, the hash is no longer trimmed and is sent to the server as-is.

## Attributes <a href="#attributes" id="attributes"></a>

*   Previously, the `removeAttr()` method for `true`/`false` attributes like "`checked`", "`selected`", and "`readonly`" would silently set the corresponding DOM element property to "`false`". Now you have to do it manually:

    ```javascript
    $("input[type=email]").removeAttr("readonly").prop("readonly", false)
    ```
* If you call `val()` on a multi-select with nothing selected, you'll now get an empty array instead of `null`.
* Class manipulation methods now work with SVG (though full SVG support in jQuery is still not there).

## Core <a href="#core" id="core"></a>

* The jQuery core now runs in [strict mode](https://learn.javascript.ru/strict-mode).
* The `document-ready` event handlers now run asynchronously — if one handler fails, it won't affect the execution of other handlers.
* The `$.isNumeric()` method no longer tries to cast the `toString()` method on arbitrary objects (who needed that anyway?).
* The `width()`, `height()` methods and others — previously, calling them on an empty object collection would return `null`, now it returns `undefined`.
*   The `jQuery.ready` promise has been officially added, which is super handy to use with `$.when()`:

    ```javascript
    $.when($.ready, $.getScript("script.js") )
     .then(function() {
       // document is ready and script.js is loaded
     })
     .catch( function() {
       // error
     });
    ```
* The `jQuery.unique()` method has been renamed to `jQuery.uniqueSort()`
* The `jQuery.parseJSON()` method is deprecated — switch to `JSON.parse()`
* Internal data storage has moved to camelCase — this is important for those who used it without going through the `data()` method

## Deferred Object <a href="#deferred" id="deferred"></a>

As I mentioned earlier, the Deferred object is now compatible with ES-2015 Promise, which means we'll need to rewrite `done()` to `then()` and `fail()` to `catch()`.

The second important point — according to the ES-2015 specification, callback functions should accept only one argument: for successful execution it's some result, and in case of error it's the error itself. If you can't do that, the old `done()` and `fail()` functions still stick around, though I have a feeling they'll be removed soon:

```javascript
// before
$.get("/get-my-page.html")
 .done(function(data, textStatus, jqXHR) { /* all OK */ })
 .fail(function(jqXHR, textStatus, errorThrown) { /* error */ });

// after
$.get("/get-my-page.html")
 .then(function(data) { /* all OK */ })
 .catch(function(error) { /* error */ });
```

## Dimensions <a href="#size" id="size"></a>

Minor changes hit the `width()`, `height()`, `css("width")`, and `css("height")` functions — they can now return not only integer width and height values but also float values. This precision is due to the switch to using `getBoundingClientRect()`.

One more thing — `$(window).outerWidth()` and `$(window).outerHeight()` calls will now include the window scrollbar sizes.

## Effects <a href="#effects" id="effects"></a>

Animation has moved to using the requestAnimationFrame API, so everything is now faster and smoother.

The `.show()`, `.hide()`, and `.toggle()` functions learned to remember the previous state of the CSS `display` property.

The number of easing function arguments has been reduced to one argument containing the animation progress from 0 to 1.

## Events <a href="#events" id="events"></a>

Shorthand methods for the following events have been removed: `load()`, `unload()`, and `error()`. This change is due to conflicts that arose when using these methods, so rewrite them using `on()`:

```javascript
// before
$("img").load(fn);

// after
$("img").on("load", fn);
```

The synthetic `ready` event has been removed, so `on("ready", fn)` no longer works — use the `$(fn)` syntax.

Delegated events, if you try to attach them with incorrect selectors, will now immediately complain and throw an error. Debugging just got easier:

```javascript
// example of a broken selector div:not
$("body").on("click", "div:not", e => false);
```

## Selectors <a href="#selectors" id="selectors"></a>

The `:hidden` and `:visible` selectors now use `getClientRects()` — if the requested element has a layout box, it's considered visible. As a result, an empty `<span>` or `<br/>` is now considered visible.

Invalid selectors `$("#")` and `find("#")` will now throw an error.

I've described a lot, but not everything — the full guide is available on the official site:

* [jQuery Core 3.0 Upgrade Guide](https://jquery.com/upgrade-guide/3.0/)
* [jQuery Core 3.5 Upgrade Guide](https://jquery.com/upgrade-guide/3.5/)
