# Namespaces

As you've already learned, when we want to create our own event handler, we write code like this:

```javascript
// create our handler
$("p").on("click", function() {
    // do something
    alert("Click!");
});
```

When we need to remove handlers, we use:

```javascript
// remove all handlers
$("p").off();

// remove all click handlers
$("p").off("click");
```

But, as always, there are situations when we need to enable or disable not all handlers (for example, we need to disable a specific plugin's handling of some control). That's where namespaces come to the rescue, and they're pretty easy to use.

When creating an event handler, add a `namespace` with a dot:

```javascript
// create a handler
$("p").on("click.namespace", function(event){
    // do something
    alert("Click: " + event.namespace + "!");
});
```

When we need to trigger only the handler bound to our `namespace`, we use the following syntax:

```javascript
$("p").trigger("click.namespace");
```

When we trigger an event from a different namespace, our handler won't be called:

```javascript
$("p").trigger("click.other");
```

When we trigger all event handlers:

```javascript
// trigger all click handlers
$("p").trigger("click");
```

When we trigger all handlers without a namespace:

```javascript
// trigger all handlers without a namespace
$("p").trigger("click.$");
```

> Before version 1.9, you would use an exclamation mark with the event name `click!` for this purpose, and after — this workaround exploiting a quirk in a regular expression.

And the last case — removing an event handler bound to our "namespace":

```javascript
// remove all click handlers in this namespace
$("p").off("click.namespace");
```

You can even remove all handlers from a specific namespace in one shot:

```javascript
// click handler
$("p").on("click.color", function() {
  $(this).css("color", "red");
});

// mouseenter handler
$("p").on("mouseenter.color", function() {
  $(this).css("color", "green");
});
```

```javascript
// changed our mind, cancel everything
$("p").off(".color");
```

Here's another useful example of a crafty handler — it can catch and process data:

```javascript
// create a handler
$("p").on("click.data", function(event, one, two, three) {
    alert("Click data: " + one + ", " + two + ", " + three);
});
```

Trigger the event, passing an array of arguments as data:

```javascript
$("p").trigger("click.data", [1, 2, 3]);
```

I also wanted to draw attention to the support for multiple namespaces:

```javascript
// create a handler for a
$("p").on("click.a", function(event) {
    alert("Click A!");
});

// create a handler for b
$("p").on("click.b", function(event) {
    alert("Click B!");
});

// create a handler for namespace a and b
$("p").on("click.a.b", function(event) {
    // for namespace a and b    
    alert("Click: " + event.namespace + "!");
});
```

```javascript
// trigger handler from namespace a
$("p").trigger("click.a");
```

```javascript
// trigger handler from namespace b
$("p").trigger("click.b");
```

```javascript
// remove click handler for namespace b
$("p").off("click.b");
```

The [official documentation](https://api.jquery.com/event.namespace/) is scarce on this topic, and I hope my examples helped you understand it better. And here's one more comprehensive example of working with `namespace`:

{% embed url="https://anton.shevchuk.name/book/code/events.namespace.html" %}
