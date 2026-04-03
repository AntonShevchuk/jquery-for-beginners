# Dimensions

Done with the previous section? Good, now it's time for the methods that work with element dimensions.

{% hint style="info" %}
But before we continue, I recommend refreshing your memory on [calculating the height and width of block elements](../0_html_css_javascript/advanced-css.md#size) ;)
{% endhint %}

<table data-header-hidden><thead><tr><th width="323">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">height()
</code></pre></td><td>returns the element's height excluding padding and borders; <br><br>if there are multiple elements in the selection, the first one is returned; <br><br>unlike the <code>css("height")</code> method, the value is returned without units</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">height(height)
</code></pre></td><td><p></p><p>sets the height of all elements in the selection; <br><br>if the height value is passed without units, it will be in pixels <code>px</code></p></td></tr></tbody></table>

{% hint style="info" %}
A note from the manual

```javascript
$(window).height();   // window height
$(document).height(); // HTML document height
```
{% endhint %}

The `width()` and `width(width)` methods behave the same as `height()`, but work with the element's width:

<table data-header-hidden><thead><tr><th width="323">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">width()
</code></pre></td><td>returns the element's width excluding padding and borders; <br><br>if there are multiple elements in the selection, the first one is returned; <br><br>the value is returned without units</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">width(width)
</code></pre></td><td><p></p><p>sets the width of all elements in the selection; <br><br>if the width value is passed without units, it will be in pixels <code>px</code></p></td></tr></tbody></table>

{% hint style="warning" %}
The `height()` and `width()` methods **do not change** their behavior depending on the box model — they always return the content area dimensions, excluding the element's `margin`, `padding`, and `border`.
{% endhint %}

<table data-header-hidden><thead><tr><th width="323">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">innerHeight()
innerWidth()
</code></pre></td><td>return the element's height and width respectively, including <code>padding</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">outerHeight()
outerWidth()
</code></pre></td><td>return the element's height and width, including <code>padding</code> and <code>border</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">outerHeight(true)
outerWidth(true)
</code></pre></td><td>return the element's height and width, including <code>padding</code>, <code>border</code>, and <code>margin</code></td></tr></tbody></table>

To illustrate the differences between `height()`, `innerHeight()`, and `outerHeight()`, I created the following example:

{% embed url="https://anton.shevchuk.name/book/code/height.html" %}

In this example, the central element with `id=block` has the following styles:

```css
#block {
  height: 40px;
  margin: 40px;
  padding: 40px;
  border: 40px solid #777;
}
```

Now let's see what each of these functions returns:

```javascript
alert(`
  height()          = ${$("#block").height()}
  innerHeight()     = ${$("#block").innerHeight()}
  outerHeight()     = ${$("#block").outerHeight()}
  outerHeight(true) = ${$("#block").outerHeight(true)}
`);
```

To make it easier to understand what's going on, I went the extra mile and combined several images from the official documentation into one full illustration:

![box model](../../.gitbook/assets/box.png)

