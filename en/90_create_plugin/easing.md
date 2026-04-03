---
description: It's not easy
---

# Easing

Now let's revisit `easing` — I'll show you an example of a custom function that the animation will follow. To save my imagination, I borrowed an example from an article on the once-popular Habr about animation in the MooTools framework — a visual example of a heartbeat described by the following functions:

<figure><img src="../../.gitbook/assets/easing.svg" alt=""><figcaption></figcaption></figure>

There's nothing scary about extending the easing functionality:

```javascript
$.extend($.easing, {
    /**
     * Heart Beat
     *
     * @param {Float} x progress
     */
    heart:function(x) {
        if (x < 0.3) return Math.pow(x, 4) * 49.4;
        if (x < 0.4) return 9 * x - 2.3;
        if (x < 0.5) return -13 * x + 6.5;
        if (x < 0.6) return 4 * x - 2;
        if (x < 0.7) return 0.4;
        if (x < 0.75) return 4 * x - 2.4;
        if (x < 0.8) return -4 * x + 3.6;
        if (x >= 0.8) return 1 - Math.sin(Math.acos(x));
    }
});
```

A quick explanation — the `$.extend({}, {})` construct "merges" objects:

```javascript
$.extend({name:"Anton"}, {location:"Kharkiv"});
// >>>
{
  "name": "Anton",
  "location": "Kharkiv"
};

$.extend({name:"Anton", location:"Kharkiv"}, {location:"Kyiv"});
// >>>
{
  "name": "Anton",
  "location": "Kyiv"
}
```

This way we "mix in" the new method into the existing `$.easing` object; according to the code, our method accepts only one parameter:

`x` — the animation progress coefficient, ranges from 0 to 1, fractional

The result is certainly interesting, but we can extend it a bit further with additional functions (reverse and combine):

```javascript
$.extend(jQuery.easing, {
    heartIn: function (x) {
        return $.easing.heart(x);
    },
    heartOut: function (x) {
        return $.easing.heart(1 - x);
    },
    heartInOut: function (x) {
        if (x < 0.5) return $.easing.heartIn(x);
        return $.easing.heartOut(x);
    }
});
```

We get the following derived functions:

|                 **heartIn**                |                 **heartOut**                 |                  **heartInOut**                  |
| :----------------------------------------: | :------------------------------------------: | :----------------------------------------------: |
| ![heartIn](../../.gitbook/assets/heartIn.png) | ![heartOut](../../.gitbook/assets/heartOut.png) | ![heartInOut](../../.gitbook/assets/heartInOut.png) |

Here's how to use this creation:

```javascript
$("#my").animate({height:"+200px"}, 2000, "heartIn"); // there it is
```

You can play with a working example of this function on the [animate.easing.html](https://anton.shevchuk.name/book/code/animate.easing.html) page:

{% embed url="https://anton.shevchuk.name/book/code/animate.easing.html" %}
