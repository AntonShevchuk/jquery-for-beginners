---
description: Step-by-step
---

# Step by Step

I'd like to take a closer look at the `step` function, and for clarity, here's an example implementation that displays the current value of the animated property:

```javascript
// element that will be used to display info
let $h2 = $("h2");
// our step function
let customStep = function(now, fx) {

    fx.elem;    // the DOM element being animated
    fx.prop;    // the property being animated
    fx.start;   // the starting value
    fx.end;     // the ending value
    fx.pos;     // the coefficient, changes from 0 to 1
    fx.options; // animation settings options
    now;        // current value of the animated property, calculated as
                // now = (fx.end - fx.start) * fx.pos

    $h2.html(`${fx.prop}: ${now}${fx.unit}`); // display text
};
// call animation with a custom function
$("img").animate({ height: "-=10px" }, { step: customStep });
```

{% hint style="warning" %}
Keep in mind that the step function will be called for each element in the selection and for each property being animated.
{% endhint %}

Using this function, you can stop the animation when certain values are reached:

```javascript
// step function that stops the animation
// when the 'left' value is less than or equal to zero
let limitForLeft = function (now, fx) {
  if (fx.prop === "left" && fx.start > fx.end && now <= 0) {
    $(fx.elem).stop() 
  }
}
```

> I've never actually had to use step functions in commercial projects — only for code examples in this textbook.
