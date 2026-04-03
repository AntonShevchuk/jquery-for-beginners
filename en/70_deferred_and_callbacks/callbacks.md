---
description: The $.Callbacks object
---

# Callbacks

[Callbacks](https://api.jquery.com/jQuery.Callbacks/) is a really cool object. It lets you create lists of callback functions and gives you full control over them. It's simpler to work with than Deferred — there's no separation into positive and negative scenarios. Just a stack of functions that gets executed on the `fire()` command:

```javascript
var C = $.Callbacks();

C.add(function(msg) {
    console.log(msg + " first")
});

C.add(function(msg) {
    console.log(msg + " second")
});

C.fire("Go");
```

If you open the console and run the script above, you'll get something like this:

```
>>>
Go first
Go second
```

> — Where's the power, bro?\
> — In the arguments.

By default, you can call the `fire()` method over and over again right from the console, and you'll get the same result every single time. But you can also define the Callbacks behavior through flags:

<table><thead><tr><th width="213">flag</th><th>description</th></tr></thead><tbody><tr><td><code>once</code></td><td>all functions will be called only once (similar to the Deferred object)</td></tr><tr><td><code>memory</code></td><td>stores the value from the last <code>fire()</code> call and feeds it to newly registered callback functions, only then processes the new value (that's exactly how Deferred works)</td></tr><tr><td><code>unique</code></td><td>the callback function list is filtered for uniqueness</td></tr><tr><td><code>stopOnFalse</code></td><td>as soon as any function returns "<code>false</code>", the firing process stops</td></tr></tbody></table>

Probably better with examples, here's `once`:

```javascript
var C = $.Callbacks("once");

C.add(function(msg) {
  console.log(msg + " first")
});

C.add(function(msg) {
  console.log(msg + " second")
});

C.fire("Go");

C.fire("Again"); // no result, only Go
```

```
>>>
Go first
Go second
```

The `memory` flag is a bit trickier, pay attention:

```javascript
var C = $.Callbacks("memory");

C.add(function(msg) {
  console.log(msg + " first")
});

C.fire("Go");

C.add(function(msg) {
  console.log(msg + " second")
});

C.fire("Again");
```

```
>>>
Go first
Go second // without the flag, this line wouldn't be here
Again first
Again second
```

The `unique` example is dead simple:

```javascript
var C = $.Callbacks("unique");

var func = function(msg) {
  console.log(msg + " first")
};

C.add(func);
C.add(func); // this line won't affect the result

C.fire("Go"); // only Go first
```

```
>>>
Go first
```

The `stopOnFalse` flag:

```javascript
var C = $.Callbacks("stopOnFalse");

C.add(function(msg) {
  console.log(msg + " first");
  return false; // there it is — the fateful false
});

C.add(function(msg) { 
  console.log(msg + " second"); 
});

C.fire("Go"); // only Go first
```

```
>>>
Go first
```

You can combine these flags and get interesting results, or you can skip that and just look at the following example. Here's the page to work with:

{% embed url="https://anton.shevchuk.name/book/code/callbacks.html" %}

```javascript
// the car
var $car = $('#car').show();
// our command stack
var C = $.Callbacks();
```

Add movement commands for the car in any order and quantity. To do this, run the scripts listed below (nothing will happen with the car until you call the `fire()` method):

```javascript
C.add(function(speed) {
  $car.queue(function() { // turn down, add to queue with animation
    $(this).css("transform", "rotate(-90deg)").dequeue();
  });
  $car.animate({top:"+=800"}, speed); // drive down
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // turn left, add to queue with animation
    $(this).css("transform", "rotate(0deg)").dequeue();
  });
  $car.animate({left:"-=400"}, speed); // drive left
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // turn right, add to queue with animation
    $(this).css("transform", "rotate(180deg)").dequeue();
  });
  $car.animate({left:"+=400"}, speed); // drive right
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // turn up, add to queue with animation
    $(this).css("transform", "rotate(90deg)").dequeue();
  });
  $car.animate({top:"-=400"}, speed); // drive up
});
```

Let's go!

```javascript
// run the "program"
// passing the movement speed as a parameter
C.fire(400);
```

> A bit of history: the "Deferred" object branched off from the `$.ajax()` method as a result of jQuery 1.5 refactoring. Time passed, new versions of jQuery came out, and another round of refactoring happened — resulting in "Callbacks" being separated from "Deferred" in version 1.7. So in the current version of the library, the `$.ajax()` method works with the "Deferred" object, which is a layer on top of "Callbacks". To avoid confusion in terminology, I use the term "Deferred Callbacks" when working with "Callbacks" too, because there are many callbacks out there, and clarifying every time that I mean "that particular one" is quite tiresome.

{% hint style="info" %}
Here's an article on this topic that I recommend — [Async JS: The Power of $.Deferred](https://web.dev/articles/async-deferred)
{% endhint %}
