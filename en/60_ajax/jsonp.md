# The Power of JSONP

JSONP is our old friend JSON with a callback function layer on top O_o. \
OK, let's look at examples. Here's what a server response in JSON format looks like:

```javascript
{
  "note": {
    "time":"2012.09.21 13:12:42",
    "text":"Explain why JSONP is needed"
  }
}
```

It's great when this data comes from our own server — we process it and everything's peachy. But if we need to get data from another server, the browser's security policy won't allow us to send an XMLHTTPRequest to a different server, and we'll need to get creative. If we strain our memory a bit, we can recall that we can include JavaScript from another server and it will be executed. There's our hook — and if the included script contains a call to our function with prepared data, then we're onto something:

```javascript
alertMe({
  "note":{
    "time":"2012.09.21 13:13:13",
    "text":"What's the benefit of using JSONP?"
  }
})
```

So by defining an `alertMe()` function in our code, we can process data from a remote server. Typically, servers catch the "`callback`" or "`jsonp`" parameter and use it as the wrapper function name:

```markup
<script type="text/javascript" src="https://domain.com/getUsers/?callback=alertMe"></script>
```

Well, that was the backstory — now let's get back to jQuery and the `ajax()` method:

```javascript
$.ajax({
  url: "http://domain.com/getUsers/?callback=?", // specify the URL
  dataType: "jsonp",
  success: function (data) {
    // process the data
  }
});
```

In the requested URL, the observant reader will notice the unfinished structure "`callback=?`". The thing is, instead of "`?`" a newly generated function name will be substituted, inside which the `success()` function will be called. Instead of this proxy function, you can use your own — just specify its name as the "`jsonpCallback`" parameter when calling `$.ajax()`.

> It's also worth mentioning that you can specify what the callback parameter is called using the `jsonp` parameter. So by setting `jsonp:"my"`, the URL will get the structure `my=?`.

Currently, quite a few services provide APIs with JSONP support:

* [Flickr](https://www.flickr.com/services/api/response.json.html) — working with this service's search
* [MediaWiki](https://en.wikipedia.org/w/api.php) — and consequently its derivatives: Wikipedia, Wiktionary
* [Tumblr](https://www.tumblr.com/docs/en/api/v2) — fetching posts from the service

To illustrate how it works, let's take the Flickr API for a spin:

```javascript
$.getJSON(
    "https://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?",
    {
        tags: "orange",
        tagmode: "any",
        format: "json"
    },
    function(data) {
        $.each(data.items, function(i, item){
            $("<img/>")
                .attr("src", item.media.m)
                .appendTo("article");
        });
    }
);
```

{% embed url="https://anton.shevchuk.name/book/code/ajax.jsonp.html" %}

Using this approach lets you bypass the limitations services impose on the number of requests from a single IP, plus it doesn't burden the server with additional work of proxying requests from users to services. It's precisely this principle that allowed me to create the [Analyser](https://analyser.hohli.com/) service.

> Unfortunately, many service providers (like Google) refuse to provide access to their APIs using JSONP.
