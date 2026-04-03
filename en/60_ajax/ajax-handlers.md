# Event Handlers

For convenience, AJAX requests fire several events, and naturally, you can and should handle them. jQuery lets you handle these events both for each individual AJAX request and globally. Here's a diagram that clearly shows the order in which events occur in jQuery:

<figure><img src="../../.gitbook/assets/events.svg" alt=""><figcaption><p>AJAX Events</p></figcaption></figure>

Here's the full list of events with some brief remarks:

<table><thead><tr><th width="195">event</th><th>description</th></tr></thead><tbody><tr><td><code>ajaxStart</code></td><td>this event fires when the first AJAX request starts running and there are no other active AJAX requests at the moment</td></tr><tr><td><code>beforeSend</code></td><td>fires before the request is sent, allows editing the XMLHttpRequest; local event</td></tr><tr><td><code>ajaxSend</code></td><td>fires before the request is sent, same as <code>beforeSend</code></td></tr><tr><td><code>success</code></td><td>fires when the response comes back with no errors; local event</td></tr><tr><td><code>ajaxSuccess</code></td><td>fires when the response comes back, analogous to the <code>success</code> event</td></tr><tr><td><code>error</code></td><td>fires in case of an error; local event</td></tr><tr><td><code>ajaxError</code></td><td>fires in case of an error</td></tr><tr><td><code>complete</code></td><td>fires when the current AJAX request completes, regardless of the outcome; local event</td></tr><tr><td><code>ajaxComplete</code></td><td>global event, analogous to <code>complete</code></td></tr><tr><td><code>ajaxStop</code></td><td>this event fires when there are no more active AJAX requests</td></tr></tbody></table>

Here's an example for showing an element with `id="loading"` during any AJAX request (i.e., we're handling a global event):

```javascript
$(document).on("ajaxSend", function() {
    $("#loading").show(); // show the element
}).on("ajaxStop", function(){
    $("#loading").hide(); // hide the element
});
```

{% hint style="warning" %}
This is a usability task — we should always keep the site user informed about what's happening on the page, and sending an AJAX request falls under the "must know" category. You'll have a solution like this on pretty much any site that uses AJAX.
{% endhint %}

For local events — we make changes in the `ajax()` method options:

```javascript
$.ajax({
    beforeSend: function() {
        // this handler will be called before sending this AJAX request
    },
    success: function() {
        // and this one on successful completion
    },
    error: function() {
        // this one when an error occurs
    },
    complete: function() {
        // and when the request finishes (successful or not)
    }
});
```

You can forcibly disable global handlers using the "`global`" flag. Set it to `false` and write your functionality bypassing the `ajaxStart` and `ajaxStop` events:

```javascript
$.ajax({
    global: false,
    beforeSend: function() {
        // ...
    },
    success: function() {
        // ...
    },
    error: function() {
        // ...
    },
    complete: function() {
        // ...
    }
});
```

> This option often helps avoid conflicts when working with AJAX requests, but don't overuse it.
