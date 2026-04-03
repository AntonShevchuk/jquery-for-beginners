# Working with Events

There are three main methods for working with events:

<table data-header-hidden><thead><tr><th width="306">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">on(event, handler)
</code></pre></td><td>add a handler<br>here I'm using the simplest method signature</td></tr><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">trigger(event)
</code></pre></td><td>trigger an event from a script</td></tr><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">off(event)
</code></pre></td><td>remove an event handler</td></tr></tbody></table>

Let's look at the `.on()` method:

```javascript
// attach a handler
$("p").on("click", function() {
    // do something
    alert("Click!");
});
```

Run the code above in the [sandbox](https://anton.shevchuk.name/book/code/sandbox.html), and try clicking on a paragraph.

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

You can trigger this handler programmatically:

```javascript
$("p").trigger("click");
```

When you've had enough fun, you can remove the handler using the `.off()` method:

```javascript
$("p").off("click");
```

Although I wanted to mention one important thing — inside the handler you can access the DOM element using the `this` keyword. If you need to use jQuery tools, use the `$(this)` construct:

```javascript
$("p").on("click", function() {
    $(this).css("color", "red");
});
```
