# Progress

Let's get back to the `animate()` method, which can accept an extended set of options as arguments — `animate(properties, options)`. The first argument is the CSS properties we've already worked with, and the second is a set of options that give us a bunch of new capabilities (I'll list all available options — some of them we've already seen):

<table data-header-hidden><thead><tr><th width="216">option</th><th>purpose</th></tr></thead><tbody><tr><td><code>duration</code></td><td>animation speed</td></tr><tr><td><code>easing</code></td><td>"<code>linear</code>" or "<code>swing</code>" function, or any other</td></tr><tr><td><code>queue</code></td><td>flag/parameter for the queue; I'll talk about it shortly</td></tr><tr><td><code>specialEasing</code></td><td><p>a hash where you can specify which easing function to use for changing specific properties:</p><pre class="language-javascript"><code class="lang-javascript">$("img").animate({
  "height": "+=100px",
  "width": "+=100px"
}, {
  "specialEasing": {
    "height": "linear",
    "width": "swing"
  }
})
</code></pre></td></tr><tr><td><code>step</code></td><td>function that will be called on each animation step for each CSS property; I'll describe it in more detail shortly</td></tr><tr><td><code>complete</code></td><td>function that will be called after the animation ends</td></tr><tr><td><code>start</code></td><td>function that will be called before the animation begins</td></tr><tr><td><code>progress</code></td><td>function that will be called on each step, but only once per element, regardless of the number of CSS properties</td></tr><tr><td><code>done</code></td><td>function that will be called after the animation completes successfully</td></tr><tr><td><code>fail</code></td><td>function that will be called after the animation fails</td></tr><tr><td><code>always</code></td><td>function that will be called after the animation ends, regardless of the outcome</td></tr></tbody></table>

{% hint style="info" %}
Support for the last five functions was added in version 1.8. This [jQuery release was marked by](https://blog.jquery.com/2012/08/09/jquery-1-8-released/) a global refactoring of animation and migration to Deferred.

Why do I keep drawing your attention to changes in such old versions of jQuery? Simply because nobody knows what you'll end up dealing with, so it's better to prepare for the worst.
{% endhint %}

When would this be useful? Well, if you need to track animation progress, this is exactly what you need — specifically, the `progress(animation, progress, remainingMs)` function:

```javascript
// change the height and width of images
$("img").animate({
  "height": "+=100px",
  "width": "+=100px"
}, {
  "start":    () => console.log("start"),
  "progress": (animation, progress, time) => console.log("progress", progress),
  "done":     () => console.log("done"),
  "fail":     () => console.log("fail"),
  "always":   () => console.log("always")
})
```



{% embed url="https://anton.shevchuk.name/book/code/animate.options.html" %}
