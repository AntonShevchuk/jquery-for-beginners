# "Live" Events

> I'm going to jump ahead a bit here, so if something doesn't make sense, put this section aside "for later".

There's one more task worth paying attention to that developers face very often — adding event handlers for elements that are dynamically added to the page. Let me give you an example of such a task:

_— We have an HTML page where all internal links will be loaded via AJAX. This also applies to the loaded HTML itself._

The first condition is solved easily:

```javascript
$("a[href^=\\/]").on("click", function() {
    var url = $(this).attr("href");
    $("body").load(url + " body > *");
    return false;
});
```

> For clarity, let's agree that internal links contain relative paths from the site root.

The second condition is a bit trickier, but also quite solvable:

```javascript
$("body").on("click", "a[href^=\\/]", function() {
    var url = $(this).attr("href");
    $("body").load(url + " body > *");
    return false;
});
```

The differences aren't that many, let me explain what's happening:

* first, a `click` event handler is attached to the `<body>` element
* this handler will only fire when the event originates from an element matching the selector `a[href^=\\/]`

This scheme works based on event "bubbling", so by using `event.stopPropagation()` you can prevent "live" handlers from executing.

> A lyrical digression into history: once upon a time there was a jQuery plugin called "live" — it let you attach handlers to DOM elements that didn't exist yet (loaded via AJAX or whatever), and then ~~it died~~ it was merged into the core. The `live()` method by that time only worked with `document`. Then the `delegate()` method appeared, which could attach handlers to any element, and then it too was absorbed by `on()`. So don't be too scared if you come across the old `live()` method — adapting it for newer versions of jQuery shouldn't be too hard (well, I hope).
