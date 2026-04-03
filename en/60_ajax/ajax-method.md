# The ajax() Method

The next method I'll introduce you to is `$.ajax()` — it's the main one here, and all other AJAX methods are just wrappers around it (including `load()`). The `$.ajax()` method accepts a bunch of settings and a URL as parameters.

Here's an example analogous to a `load()` call:

```javascript
$.ajax({
    url: "/get-my-page.html", // specify the URL
    method: "GET",            // HTTP method, GET by default
    data: { "id": 42 },       // data we're sending to the server
    dataType: "html",         // type of data loaded from the server
    success: function (data) {
        // attach our own success event handler
        $("#content").html(data);
    }
});
```

{% embed url="https://anton.shevchuk.name/book/code/ajax.html" %}

Here we were processing an HTML response from the server — that's great when you need to update half a page, but data is better transferred in a "proper" format. XML is understandable, structured, but verbose and somehow not quite the JavaScript way, so our choice is [JSON](https://en.wikipedia.org/wiki/JSON):

```javascript
{
  "note": {
    "time": "2012.09.21 13:11:15",
    "text": "Tell about JSONP"
  }
}
```

Essentially, this is JavaScript code as-is (JavaScript Object Notation, if we're being picky about it). And this format has become so widespread that working with data in any other format is already frowned upon.

> Life doesn't stand still, there are more convenient formats out there, but not in JavaScript :)\
> We make do with what we have — [text, HTML, JSON, or XML](https://anton.shevchuk.name/book/code/ajax.datatype.html).

For loading JSON there's a handy shortcut function — `$.getJSON()` — the only required parameter is the URL you're hitting. Optionally, you can specify data to send to the server and a callback function.

{% hint style="info" %}
You can't just up and describe all possible parameters for `$.ajax()` — you really should keep the official manual handy: [https://api.jquery.com/jQuery.ajax/](https://api.jquery.com/jQuery.ajax/).
{% endhint %}

There are also a couple more methods worth mentioning:

<table data-header-hidden><thead><tr><th width="361">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">get(url, data, success, dataType)
</code></pre></td><td>loads data using the GET method</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">post(url, data, success, dataType)
</code></pre></td><td>loads data using the POST method</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">getScript(url, success)
</code></pre></td><td>loads JavaScript from the server using the GET method</td></tr></tbody></table>

All of them, as I mentioned before, are just wrappers around `$.ajax()`, which you can easily verify by looking at the [library source code](https://github.com/jquery/jquery/blob/32b00373b3f42e5cdcb709df53f3b08b7184a944/src/ajax.js#L834). And there you'll find the implementation of `$.get()` and `$.post()` :)

```javascript
jQuery.each( [ "get", "post" ], function( i, method ) {
    jQuery[ method ] = function( url, data, callback, type ) {

        // Shift arguments if data argument was omitted
        if ( isFunction( data ) ) {
            type = type || callback;
            callback = data;
            data = undefined;
        }

        // The url can be an options object (which then must have .url)
        return jQuery.ajax( jQuery.extend( {
            url: url,
            type: method,
            dataType: type,
            data: data,
            success: callback
        }, jQuery.isPlainObject( url ) && url ) );
    };
} );
```

The `getScript()` method in turn is just a wrapper around `$.get()`:

{% embed url="https://anton.shevchuk.name/book/code/ajax.script.html" %}
