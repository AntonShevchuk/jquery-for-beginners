# The load() Method

Let's start with the simplest thing — loading HTML code into the DOM element we need on the page. For this purpose, the `load()` method will do. This method accepts the following parameters:

<table data-header-hidden><thead><tr><th width="213">argument</th><th>description</th></tr></thead><tbody><tr><td><code>url</code></td><td>of the requested page</td></tr><tr><td><code>data</code></td><td>data to send (optional parameter)</td></tr><tr><td><code>callback</code></td><td>function that will be called when the server request completes (optional parameter)</td></tr></tbody></table>

Now for examples. Open the test page:

{% embed url="https://anton.shevchuk.name/book/code/ajax.load.html" %}

Using the following request, all the HTML from the specified page will be inserted into the element with `id="content"`:

```javascript
// the HTML from the specified page will be inserted into the element with id="content"
$("#content").load("example.html");
```

As you can see, the entire HTML is loaded along with all the styles. To avoid this kind of mess, you should select only the element you need from the loaded page. To do that, just add a selector after a space:

```javascript
// the HTML from the specified page will be inserted into the element with id="content",
// but not all of it — only the main tag selected by the given selector
$("#content").load("example.html main");
```

Once loading is complete, we can call a custom function to process the loaded HTML. The catch is that this function will only fire after the content has been added to the target element, and I'd like to control that process:

```javascript
// process the received data
$("#content").load("example.html main", function(data) {
    // couldn't think of anything more original
    alert("We loaded HTML that is " + data.length + " characters long");
});
```

If you need to send some data to the server, you can easily do it like this:

```javascript
// send data to the server as the second parameter
// our server doesn't actually process them
// but you can see the data in the console, in the sent requests
$("#content").load("example.html main", { id: 42 });
```

> From my experience — you'll very often use the `load()` method as shown in the first example. I also recommend remembering the second example — it can save you when you need to implement loading via AJAX but you don't have access to the server-side, or it's limited.
