# Be Ready!

Now it's time to get to work — let's grab some element on the page and try to change it. To do this, we'll insert the following code into the `<head>`:

```markup
<script>
    // we're trying to find all <h2> elements on the page
    // and change the font color to red
    jQuery("h2").css("color", "red");
</script>
```

But code like this won't actually do anything, because at the time of execution there won't be any `<h2>` tags on the page yet — the script runs too early, before the entire HTML document has loaded. For the code to work correctly, we need to either place the code after the target `<h2>`, or better yet at the very bottom of the page, or use the `ready()` method to track the `DOMContentLoaded` event on our `document`:

```markup
<script>
    // wait for the entire document to load
    // after that, the anonymous function
    // we passed as a parameter will be executed
    jQuery(document).ready(function(){
        jQuery("h2").css("color", "red");
    });
</script>
```

You can also use the shorthand version without explicitly calling the `ready()` method:

```markup
<script>
    $(function(){
        $("h2").css("color", "red");
    });
</script>
```

{% hint style="warning" %}
This last version should already be considered the primary one to use, since in jQuery 3.0 the `ready()` method has been marked as deprecated.
{% endhint %}

You can create as many such functions as you like — you don't have to cram all the necessary functionality into one.

{% hint style="info" %}
If your code is added to the page and will execute after the `load` event has already occurred, it will be called immediately — no additional manipulations needed.
{% endhint %}

`$()` is a synonym for `jQuery()`. To avoid conflicts with other ~~countries~~ libraries over the use of "`$`", I recommend wrapping your code in an anonymous function like this (best practice, so to speak):

```javascript
;(function($, undefined){
    // it's quiet and cozy in here
    // we'll always be sure that $ === jQuery
    // and undefined hasn't been redefined ;)
})(jQuery);
```

Take a look at the example in [ready.html](https://anton.shevchuk.name/book/code/ready.html). Open the source code of that page and find the `<script>` tag. Think about it and try to understand what's going on.
