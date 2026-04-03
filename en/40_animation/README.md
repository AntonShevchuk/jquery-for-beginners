# 40% Animation

The jQuery library makes it super easy to animate DOM elements — there are several functions built just for that. But let's take it one step at a time. We'll start with the simple `hide()` and `show()` — these two methods hide or display elements, respectively:

<table data-header-hidden><thead><tr><th width="282">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">hide()
</code></pre></td><td>hides elements</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">show()
</code></pre></td><td>displays elements</td></tr></tbody></table>

These calls only operate on the CSS `display` attribute, toggling it from the current state to `none` and back. You can pass the animation speed as the first parameter — either one of the reserved words `slow` or `fast`, or specify the speed in milliseconds (1000 ms = 1 sec):

```javascript
// slowly descending the mountain and… hiding all images
//   slow == 600
//   fast == 200
$("img").hide('slow')
```

```javascript
// now let's bring them back, a bit faster
$("img").show(400)
```

In this case, the disappearance of elements will be accompanied by changes to `width`, `height`, `opacity`, and other attributes. On top of these two methods, there's also `toggle()`, which works as a switch between `hide → show` or `show → hide`:

```javascript
// run this a couple of times
$("img").toggle()
```

Moving on — the second parameter in these methods can be a callback function — it will be executed when the animation is complete:

```javascript
// hide all images
$("img").hide("slow", function() {
    // afterwards, show an alert
    alert("Images was hidden")
});
```

{% embed url="https://anton.shevchuk.name/book/code/hide.html" %}

***

{% hint style="info" %}
The animation of attributes like `height`, `width`, and `opacity` is visible to the naked eye, but in reality that's far from all of it. If you peek inside jQuery, you'll see that internal and external padding — `padding` and `margin` — also change, so don't forget about that.
{% endhint %}

Let's keep going — next up is the slide family of methods: `slideUp()`, `slideDown()`, and `slideToggle()`. Their behavior is similar to the previous functions, but the animation will only affect block heights:

<table data-header-hidden><thead><tr><th width="284">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">slideUp()
</code></pre></td><td>hides elements by reducing height to <code>0</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">slideDown()
</code></pre></td><td>shows all elements by gradually increasing their height from zero</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">slideToggle()
</code></pre></td><td>works as <code>slideDown()</code> for hidden elements, otherwise as <code>slideUp()</code></td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/slide.html" fullWidth="false" %}

***

Before we get to dessert, let me mention the `fade` family of functions — they only manipulate the `opacity` attribute:

<table data-header-hidden><thead><tr><th width="281">method</th><th>description</th><th data-hidden data-type="rating" data-max="5"></th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeOut()
</code></pre></td><td>changes <code>opacity</code> from the current value to <code>0</code></td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeIn()
</code></pre></td><td>changes <code>opacity</code> from <code>0</code> to its previous value</td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeToggle()
</code></pre></td><td>toggles between <code>In</code> and <code>Out</code></td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeTo(duration, opacity)
</code></pre></td><td>changes <code>opacity</code> to the desired value</td><td>null</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/fade.html" %}

***

And now the sweetest part — all animation effects in jQuery revolve around the `animate()` method. This function takes one or more CSS properties of an element and changes them from the original value to the specified one over N iterations (the number of iterations depends on the specified time, but no less than one iteration per 13 ms — if I dug up that value correctly). Alright then, from words to action — let's try to implement `fadeIn()` and `fadeOut()` using `animate()`:

```javascript
// fadeOut()
$("img").animate({
    "opacity": "hide"
})
```

```javascript
// fadeIn()
$("img").animate({
    "opacity": "show"
})
```

Pretty simple, right? Now let's up the ante — change the size of blocks and their opacity:

```javascript
// the values of the specified properties will smoothly change
// from current to the given ones
$("img").animate({
    "opacity": 0.5,
    "height": "100px",
    "width": "100px"
})
```

As you can see, also not complicated. Now let's try working from the current values instead of setting specific ones:

```javascript
// changing step by step
$("img").animate({
    "opacity": "+=0.1",
    "height": "+=12px",
    "width": "+=20px"
})
```

{% embed url="https://anton.shevchuk.name/book/code/animate.html" %}
