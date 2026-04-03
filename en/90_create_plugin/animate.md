# Animate

> The information in this section is relevant for jQuery version 1.8 and above. If you're interested in extension capabilities for older versions, read my article "[Writing Animation Plugins](https://anton.shevchuk.name/javascript/jquery-for-beginners-write-animation-plugins/)".

First, a teaser — the [`animate()`](https://api.jquery.com/animate/) method manipulates the `jQuery.Animation` object, which provides the following extension points:

* `jQuery.Tween.propHooks`
* `jQuery.Animation.preFilter`
* `jQuery.Animation.tweener`

I'll start with `jQuery.Tween.propHooks`, since there are already plugins whose code we can peek at :) For maximum clarity, I'll take a fairly trivial task — let's smoothly change the font color for a given set of elements:

```javascript
$('p').animate({color:'#ff0000'});
```

The code above won't produce any effect, because out of the box the library doesn't animate the `color` property. But we can fix that — we just need to level up `jQuery.Tween.propHooks`:

```javascript
$.Tween.propHooks.color = {
    get: function(tween) {
        return tween.elem.style.color;
    },
    set: function(tween) {
        tween.easing;  // current easing
        tween.elem;    // target element
        tween.options; // animation settings
        tween.pos;     // current progress
        tween.prop;    // property being changed
        tween.start;   // starting values
        tween.now;     // current value
        tween.end;     // desired final values
        tween.unit;    // units of measurement
    }
};
```

This code doesn't work yet — these are our building blocks, and we'll use them to construct our animation. Before we start, it's worth peeking inside each of these properties:

```javascript
console.log(tween);
```

```
>>>
easing: "swing"
elem: HTMLParagraphElement
end: "#ff0000"
now: "NaNrgb(0, 0, 0)"
options: Object
complete: function (){}
duration: 1000
old: false
queue: "fx"
specialEasing: Object
pos: 1
prop: "color"
start: "rgb(0, 0, 0)"
unit: "px"
```

There will be a lot of data in the console, because the method above gets called N times depending on the animation duration, with "tween.pos" gradually increasing its value from 0 to 1. By default, this increase is linear — if you need something different, check out the easing plugin or read on to the end of this section (I already mentioned this in the [Animation](../40_animation/) chapter).

Even at this point we can already modify the selected element (by manipulating `tween.elem`), but there's a more convenient way — you can set the "`run`" property of the `tween` object:

```javascript
$.Tween.propHooks.color = {
    set: function(tween) {
        // initialization goes here
        tween.run = function(progress) {
            // code responsible for changing element properties goes here
        }
    }
};
```

The resulting code will work as follows:

1. the `set()` function will be called once
2. the `run()` function will be called N times, with `progress` behaving just like `tween.pos`

Now, getting back to the original task of changing color, we can whip up the following code:

```javascript
$.Tween.propHooks.color = {
    set: function(tween) {
        // normalize start and end values to a common format
        // #FF0000 == [255,0,0]
        tween.start = parseColor(tween.start);
        tween.end = parseColor(tween.end);
        tween.run = function(progress) {
            // calculate the intermediate value
            tween.elem.style['color'] = buildColor(tween.start, tween.end, progress);
        }
    }
};
```

> You'll find the code for `parseColor()` and `buildColor()` functions in the listing on the [color.html](https://anton.shevchuk.name/book/code/color.html) page.

The result will be a smooth transition from the original color to red (`#F00` == `#FF0000` == `(255, 0, 0)`). You can see it live on the [color.html](https://anton.shevchuk.name/book/code/color.html) page:

{% embed url="https://anton.shevchuk.name/book/code/color.html" %}

{% hint style="info" %}
The [jQuery Color](https://github.com/jquery/jquery-color) plugin used [jQuery.cssHooks](https://api.jquery.com/jQuery.cssHooks/) to solve this same task, but we don't take the easy way out.
{% endhint %}

I also wanted to tell you about animation prefilters, but there's no documentation, and I couldn't figure out how to use them "in real life" — though I did dig up a little info (the code can be found in the `Animation` function):

```javascript
jQuery.Animation.prefilter(function(element, props, opts) {
    // deferred object of animate
    element; // target element
    props;   // animation settings from animate()
    opts;    // animation options
    // disable animation when trying to animate element height
    if (props['height'] != undefined) {
        return this;
    }
});
```

You can play with the example on the [animate.prefilter.html](https://anton.shevchuk.name/book/code/animate.prefilter.html) page:

{% embed url="https://anton.shevchuk.name/book/code/animate.prefilter.html" %}

There isn't much to say about `jQuery.Animation.tweener` either, but the example turned out a bit more interesting — the code below lets you animate an object's width and height along a given diagonal:

> Caution: understanding what's happening here requires 8th-grade geometry knowledge.

```javascript
// create support for a new animation property — diagonal
jQuery.Animation.tweener( "diagonal", function( property, value ) {
    // create a tween object
    var tween = this.createTween( property, value );
    // intermediate calculations and data
    var a = jQuery.css(tween.elem, 'width', true );
    var b = jQuery.css(tween.elem, 'height', true );
    var c = Math.sqrt(a*a + b*b), sinA = a/c, sinB = b/c;
    tween.start = c;
    tween.end = value;
    tween.run = function(progress) {
        // calculate the target value — new hypotenuse value
        var hyp = this.start + ((this.end - this.start) * progress);
        // directly change element properties
        tween.elem.style.width = sinA*hyp + tween.unit; // width
        tween.elem.style.height = sinB*hyp + tween.unit; // height
    };
    return tween;
});
```

Working example at [animate.tweener.html](https://anton.shevchuk.name/book/code/animate.tweener.html):

{% embed url="https://anton.shevchuk.name/book/code/animate.tweener.html" %}
