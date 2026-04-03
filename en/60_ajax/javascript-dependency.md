---
description: Or Server-Side Rendering for beginners
---

# Curing JavaScript Addiction

The love for AJAX can be excessive, and in the pursuit of Web 2.0 (3.0, 4.0, ... — pick your favorite) we build sites where all our actions run through [XMLHTTPRequest](https://en.wikipedia.org/wiki/XMLHttpRequest). No, that's not bad in itself — we reduce the load on the server, bandwidth, etc., etc. — but there's one "but": we have search engines that don't always bother executing JavaScript code, and yet we still need to serve them the content hidden behind AJAX requests. Consequently, we need to duplicate navigation (at the very least) for clients without JavaScript.

> It's worth remembering that there are also users who have JavaScript disabled in their browser (or it's not even supported — hello there, "Lynx"), but those folks know what they're doing. And then there are scripts that break and prevent regular users from navigating the site, which upsets them a great deal. So this chapter isn't just "for fun."

How do we get around all this without shooting ourselves in the foot? It's actually very simple — create regular navigation, the kind you'd build if you'd never heard of AJAX and friends:

```markup
<ul class="navigation">
    <li><a href="/">Home</a></li>
    <li><a href="/about.html">About Us</a></li>
    <li><a href="/contact.html">Contact Us</a></li>
</ul>
<section id="content"><!-- Content --></section>
```

This example works without JavaScript — all pages in our menu use the same template for displaying information, and in practice only the content of the element with `id="content"` changes. Now let's add content loading via AJAX — add the following code:

```javascript
$(function() {
    // attach a handler to all links in our navigation menu
    $("ul.navigation a").click(function(){
        var url = $(this).attr("href");   // grab the link
        url += "?ajax=true";              // append the ajax=true parameter
        $("#content").load(url);          // load the updated content
        return false; // return false so the link doesn't navigate
    });
});
```

In this example, we assume that the server, seeing the `ajax=true` parameter, will return not the entire page but only the update for the target element with `id="content"`.

> Of course, the server should be smarter and not require an explicit flag for AJAX usage — it should be perfectly satisfied by catching the "`X-Requested-With`" header with the value "`XMLHttpRequest`". Most modern web development frameworks handle this out of the box.

If controlling the server's behavior is problematic and it stubbornly sends us the entire page, you can write the following code:

```javascript
$(function() {
    // attach a handler to all links in our navigation menu
    $("ul.navigation a").click(function(){
        var url = $(this).attr("href"); // grab the link
        // load the full page, but insert only the #content contents from the loaded page into our container
        $("#content").load(url + " #content > *");
        return false; // return false
    });
});
```

If the loaded content also contains links, you'll already be able to "bring events to life":

{% embed url="https://anton.shevchuk.name/book/code/ajax.live.html" %}
