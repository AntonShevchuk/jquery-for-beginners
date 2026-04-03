# Queue It Up

A few words about how the `animate()` method handles sequencing. Most readers are probably already familiar with setting up sequential animations. For this, we can use method chaining:

```javascript
// find the element we need
$("#box")
  // specify what we want to animate
  .animate({ left:"-=100" })
  // the next animation call is added to the execution queue
  .animate({ top:"-=100" })
```

To run animations in parallel, you'll need to make the following change:

```javascript
// find the element we need
$("#box")
  // specify what we want to animate
  .animate({ left:"+=100" })
  // the next animation call will ignore the queue
  .animate({ top:"+=100" }, { queue:false })
```

{% hint style="info" %}
Although this particular example is better written as a single `animate()` call:

```javascript
$("#box")
  .animate({
    left: "+=100",
    top: "+=100"
  })
```
{% endhint %}

There's also a wonderful function `stop()` that lets you halt the current animation midway and clear the queue if needed. To provide different behaviors, it accepts three parameters:

<table data-header-hidden><thead><tr><th width="275">parameter</th><th>purpose</th></tr></thead><tbody><tr><td><code>queue</code></td><td>queue name; <br>for working with the animation queue "<code>fx</code>" this parameter is omitted ("<code>fx</code>" is the default queue)</td></tr><tr><td><code>clearQueue</code></td><td>flag to clear the queue</td></tr><tr><td><code>jumpToEnd</code></td><td>apply the animation result, or not</td></tr></tbody></table>

Here's an example that begs for some experimentation:

```javascript
// a very slow example
$("#box")
  .animate({ left:"-=100" }, { duration: 10000 })
  .animate({ top: "-=100" }, { duration: 10000 })
```

```javascript
// stop the current animation
$("#box").stop();
```

```javascript
// stop the current animation
// and all subsequent ones (clear the queue)
$("#box").stop(true);
```

```javascript
// stop the current animation and all subsequent ones
// but apply the result of the current one
$('#box').stop(true, true);
```

```javascript
// stop only the current animation
// and apply its result
$('#box').stop(false, true);
```

> A note for the future: if you've built a dropdown menu that keeps dropping down and disappearing after you've been playing with the mouse, you need to stick a `stop()` in the event handler.

Let's try it out on a live example:

{% embed url="https://anton.shevchuk.name/book/code/animate.queue.html" %}

By default, all animation on an object goes into the "`fx`" queue, but since version 1.7 you can specify a custom queue:

```javascript
$("#box")
  .animate({ "top":"+=100" }, { duration: 10000, queue:"x" }) // build queue X
  .dequeue("x") // start queue X
```

```javascript
$("#box").stop("x") // stop the animation in queue X
```

Why would we need a custom queue? For parallelizing animation, of course — so we can start one animation queue and at any other moment start another one. Maybe this was designed with games in mind? But why guess — let's play:

{% embed url="https://anton.shevchuk.name/book/code/animate.game.html" %}

Open the page and run the script — a game character will appear, and the game is on!

```javascript
$('#player').show()
```

![Mario Player](../../.gitbook/assets/mario.svg)

The script with the `keydown` event handler is already running, and you can make Mario run around the page using the arrow keys or the `R`, `D`, `F`, and `G` keys:

```javascript
var $player = $("#player");
$(document).keydown(function(event){
    // 37 - `←` | 68 - `d` | left
    // 38 - `↑` | 82 - `r` | up
    // 39 - `→` | 71 - `g` | right
    // 40 - `↓` | 70 - `f` | down
    switch (event.keyCode) {
        case 37:
        case 68:
            $player.stop("x", true);
            $player.animate({ "left":"-=100" }, { queue:"x" }).dequeue("x");
            break;
        case 38:
        case 82:
            $player.stop("y", true);
            $player.animate({ "top": "-=100" }, { queue:"y" }).dequeue("y");
            break;
        case 39:
        case 71:
            $player.stop("x", true);
            $player.animate({ "left":"+=100" }, { queue:"x" }).dequeue("x");
            break;
        case 40:
        case 70:
            $player.stop("y", true);
            $player.animate({ "top": "+=100" }, { queue:"y" }).dequeue("y");
            break;
    }
    event.stopImmediatePropagation();
})
```

In this example, two queues are used — `x` and `y`, corresponding to the coordinate axes along which we move. When the `←` key is pressed, the `left` value decreases by `100px` in the `x` queue. When `→` is pressed, we clear the `x` queue and increase `left` by `100px`. For movement along the `y` axis, we use the queue of the same name and the `↑` and `↓` keys.

> From this chapter you should have learned that the `WASD` layout has an alternative :)
>
> All rights to Mario belong to [Nintendo](https://www.nintendo.com/), so be careful with him.
