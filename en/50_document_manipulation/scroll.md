# Scrollbar

And the last pair of methods I'd like to tell you about:

<table data-header-hidden><thead><tr><th width="271">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollLeft()
</code></pre></td><td>returns the horizontal scroll position for the first element in the selection</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollLeft(value)
</code></pre></td><td>sets the horizontal scroll value for each element in the selection</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollTop()
</code></pre></td><td>returns the vertical scroll position for the first element in the selection</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollTop(value)
</code></pre></td><td>sets the vertical scroll value for each element in the selection</td></tr></tbody></table>

Here's how we can find out the "distance" traveled from the top of the page:

```javascript
$('html').scrollTop();
```

Or we can "jump" to the very top of the page:

```javascript
$('html').scrollTop(0);
```

The `scrollTop` and `scrollLeft` values can be animated and don't work for hidden DOM elements:

```javascript
$('html').animate({ scrollTop: '+=200px' });
```

{% embed url="https://anton.shevchuk.name/book/code/dom.scroll.html" %}

***

There are really a lot of methods, and I don't always remember what each one does either (especially when it comes to the wrap family), so don't bother memorizing everything listed here. The main thing is to remember that these methods exist and keep the [documentation](https://api.jquery.com/category/manipulation/) handy.
