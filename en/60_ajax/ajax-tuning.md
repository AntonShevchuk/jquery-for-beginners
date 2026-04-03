---
description: Not a beginner-level article
---

# Leveling Up AJAX

We have three ways to "level up" AJAX in jQuery: creating prefilters, adding new converters, and transports.

## Prefilters <a href="#prefilters" id="prefilters"></a>

A prefilter is a function that will be called before the `ajaxStart` event, where you can modify both the `jqXHR` object and any associated settings:

```javascript
// register an AJAX prefilter
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // our manipulations on settings and jqXHR
});
```

What's all this for? Here's a simple task — don't wait for an "old" AJAX response if we're requesting the same URL again:

```javascript
// collection of current requests
var currentRequests = {};
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // our custom setting
    if ( options.abortOnRetry ) {
        if ( currentRequests[ options.url ] ) {
            // abort the old request
            currentRequests[ options.url ].abort();
        }
        currentRequests[ options.url ] = jqXHR;
    }
});

// call using the filter
$.ajax({
    /* ... */
    abortOnRetry: true
})
```

You can also modify the call options — here's an example that redirects the request to a pre-prepared proxy page on our server based on the `crossDomain` flag:

```javascript
$.ajaxPrefilter(function( options ) {
    if ( options.crossDomain ) {
        options.url = "/proxy/" + encodeURIComponent( options.url );
        options.crossDomain = false;
    }
});
```

Prefilters can be attached to a specific `dataType` (i.e., different filters will fire depending on the expected data type from the server):

```javascript
$.ajaxPrefilter("json script", function(options, original, jqXHR) {
    /* ... */
});
```

And lastly: to switch the `dataType` to some other type, we just need to return the desired value:

```javascript
$.ajaxPrefilter(function( options ) {
    // this is our function that detects the necessary URLs
    if ( isActuallyScript( options.url ) ) {
        // now we "expect" a script
        return "script";
    }
});
```

{% hint style="warning" %}
Be very careful when manipulating global settings, especially through such an implicit feature as filters. Document these approaches in your project documentation, otherwise the developers who will maintain your code later will curse you (and that developer might be you in a couple of months). In general, thorough documentation and code commenting is strongly recommended. A sharp quote, mistakenly attributed to programming guru McConnell: _"Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live"_ (John F. Woods).
{% endhint %}

## Converters <a href="#converters" id="converters"></a>

A converter is a callback function that's called when the received data type doesn't match the expected one (i.e., `dataType` was specified incorrectly).

All converters are stored in the global `ajaxSettings`:

```javascript
// key format "from_format to_format"
// you can use "*" as the input format
{
    converters: {
        "* text":    String,         // convert anything to text
        "text html": true,           // text to HTML (flag true == no changes)
        "text json": JSON.parse,     // text to JSON
        "text xml":  jQuery.parseXML // parse text as XML
    }
}
```

To extend the set of converters, you'll need the `$.ajaxSetup()` method:

```javascript
$.ajaxSetup({
    converters: {
        "text mydatatype": function( textValue ) {
            if ( valid( textValue ) ) {
                // parse the received data
                return mydatatypeValue;
            } else {
                // an error occurred
                throw exceptionObject;
            }
        }
    }
});
```

{% hint style="info" %}
`dataType` names must always be lowercase.
{% endhint %}

Converters should be used when you need to introduce custom `dataType` formats or to convert data into the desired format. Specify the required `dataType` when calling `$.ajax()`:

```javascript
$.ajax( url, { dataType: "mydatatype" });
```

Converters can also be specified directly in the `$.ajax()` call, so as not to clutter up the global settings:

```javascript
$.ajax( url, {
    dataType: "xml text mydatatype",
    converters: {
        "xml text": function( xmlValue ) {
            // extract the necessary data from XML
            return textValue;
        }
    }
});
```

> A bit of explanation: we request "XML", which we convert to text, which will then be passed to our converter from "`text`" to "`mydatatype`".

## Transport <a href="#transport" id="transport"></a>

{% hint style="warning" %}
Using your own transport is a last resort — only go for it when the task at hand can't be accomplished using prefilters and converters.
{% endhint %}

A transport is an object that provides two methods — `send()` and `abort()` — which will be used inside the `$.ajax()` method. To register your own transport method, use `$.ajaxTransport()`. It'll look something like this:

```javascript
$.ajaxTransport( function( options, originalOptions, jqXHR ) {
    if ( /* transportCanHandleRequest */ ) {
        return {
            send: function( headers, completeCallback ) {
                /* send the request */
            },
            abort: function() {
                /* abort the request */
            }
        };
    }
});
```

Let me briefly explain the parameters we'll be working with:

<table data-header-hidden><thead><tr><th width="258">parameter</th><th>description</th></tr></thead><tbody><tr><td><code>options</code></td><td>request settings,<br>i.e., what we specify when calling <code>$.ajax()</code></td></tr><tr><td><code>originalOptions</code></td><td><p>"clean" settings,</p><p>without even the default changes applied</p></td></tr><tr><td><code>jqXHR</code></td><td>the <code>jQuery XMLHttpRequest</code> object</td></tr><tr><td><code>headers</code></td><td>request headers as key-value pairs</td></tr><tr><td><code>completeCallback</code></td><td>callback function to be used for notifying about request completion</td></tr></tbody></table>

The `completeCallback()` function has the following signature:

```javascript
function ( status, statusText, responses, headers ) {
    /* some code */
}
```

where:

<table><thead><tr><th width="238">parameter</th><th>description</th></tr></thead><tbody><tr><td><code>status</code></td><td>HTTP status of the response</td></tr><tr><td><code>statusText</code></td><td>text interpretation of the response status</td></tr><tr><td><code>responses</code></td><td>optional — an object containing server responses in all formats supported by the transport; <br>for example, the native <code>XMLHttpRequest</code> would look like <code>{ xml: XMLData, text: textData }</code> when requesting an XML document</td></tr><tr><td><code>headers</code></td><td>optional — a string containing the server response headers, if the transport can obtain them; <br>for example, the <code>XMLHttpRequest.getAllResponseHeaders()</code> method can handle this</td></tr></tbody></table>

Just like prefilters, transports can be bound to a specific requested data type:

```javascript
$.ajaxTransport( "script", function( options, originalOptions, jqXHR ) {
    /* bind only to script */
});
```

And now for a brain-buster — let's add an "`image`" transport to our page:

```javascript
// register it for the image type
$.ajaxTransport("image", function (options) {
    // only track GET AJAX requests
    if (options.type === "GET" && options.async) {
        var image;
        return {
            // 
            send:function (_, callback) {
                image = new Image();
                // prepare the done function
                function done(status) {
                    if (image) {
                        var statusText = ( status == 200 ) ? "success" : "error", tmp = image;
                        image = image.onreadystatechange = image.onerror = image.onload = null;
                        callback(status, statusText, { image:tmp });
                    }
                }
                image.onreadystatechange = image.onload = function () {
                    done(200);
                };
                image.onerror = function () {
                    done(404);
                };
                image.src = options.url;
            },
            abort:function () {
                if (image) {
                    image = image.onreadystatechange = image.onerror = image.onload = null;
                }
            }
        };
    }
});
```

Now we can use this transport:

```javascript
$.ajax('../code/img/photo-cat.jpg', {
    dataType:'image',
    success: function(data) {
        $('#image').html(data.image);
    }
})
```

{% embed url="https://anton.shevchuk.name/book/code/ajax.transport.html" %}

> I'd like to remind you once more that this is "advanced level" material, and this section is arguably out of place in a "beginner" textbook.

Following the official documentation:

* [Extending Ajax: Prefilters, Converters, and Transports](https://api.jquery.com/jQuery.ajax/#extending-ajax)
