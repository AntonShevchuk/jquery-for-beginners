# Event Bubbling and Handling

Most JavaScript events can be emulated directly from a script. Say we have a page with just one link:

{% embed url="https://anton.shevchuk.name/book/code/events.click.html" %}

We can "click" on it using the aptly named `click()` method:

```javascript
$("a").get(0).click()
```

{% hint style="info" %}
Notice that I'm calling the click method not on the jQuery object, but directly on the DOM element. If we use jQuery methods `click()` or `trigger()`, we won't get the result:

```javascript
$("a").click()
$("a").trigger("click")
```

This is because they don't trigger the browser's default action — they only call event handlers. Come back to this code once you've added event handlers, and it'll work.
{% endhint %}

So we can pretend to be the user, but that's not enough for real work. We need to learn how to track user behavior and respond to their actions in a timely manner. Let's start from the beginning — take a link and add a simple `click` event handler:

```javascript
$("a").on("click", function(event) {
    alert("Hello!")
})
```

{% hint style="info" %}
For the most common events there are "shorthand" methods — for tracking `click` there's `click()`, for `hover` there's `hover()`, and so on :)&#x20;

In all the following examples I'll be using the `click()` method
{% endhint %}

Now, when you click the link, you'll see a greeting, and after closing it the browser will navigate to the URL specified in the `href` attribute.

_— Just closed it, enough already!_

Yeah, this isn't quite what I wanted — I only needed to display text and not go anywhere. Aha, for that we need to cancel the default action:

```javascript
$("a").click(function(event) {
    alert("Hello!");
    event.preventDefault();
})
```

Now there's no navigation, because `event.preventDefault()` prevents that action. But what if someone attaches another handler to the parent element?

```javascript
$("article").click(function(event) {
    alert("Article!");
})
```

As a result, we'll get two messages — but why? If you're asking this question, it means you're not yet familiar with how events are processed. Let me give you a quick intro. When you click on an element in the DOM tree, event "capturing" happens — first, all parent elements get a chance to handle the "click", and only then does it reach the element that was actually clicked. But that's not all. Then the event starts making its way back — it "bubbles up", giving parent elements a second chance to handle the event.

> But things aren't so smooth — we've got the "immortal" IE, which fundamentally doesn't work with "capturing", so everyone decided to take the path of least resistance and handle events only during the "bubbling" phase.

{% hint style="info" %}
I recommend reading the article "[Bubbling and Capturing](https://learn.javascript.ru/bubbling-and-capturing)" from the already mentioned Kantor's tutorial.
{% endhint %}

Alright, that seems clear. Now let's get back to our example and try to understand what's happening. There's a click handler on the link and a handler on the menu containing this link. When clicking the link, the handler on the link fires, then the event bubbles up to the menu, and its "click" handler fires. But this isn't quite what we want, and to fight this mischief we need to stop event "bubbling":

```javascript
$("a").click(function(event) {
    alert("Hello!");
    event.preventDefault();
    event.stopPropagation();
})
```

To speed up development, jQuery has a quick way to call both methods at once:

```javascript
$("a").click(function(event) {
    return false; // here it is :)
})
```

Now you've got enough knowledge to easily manipulate events on the page. Although I'll add a bit more — to make sure only your event handler fires, you can use `event.stopImmediatePropagation()`:

```javascript
$("a").click(function(event) {        // your event handler
    alert("Hello!");                  // it's the chosen one
    event.stopImmediatePropagation(); // there can be only one
    return false;
})

$("a").click(function(event) { // and this is someone else's handler
    alert("Hello again!");     // it does everything wrong
    return false;
})
```

In this example, clicking the link will display only one message. And yes, order matters.
