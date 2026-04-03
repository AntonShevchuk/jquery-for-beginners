# CSS Styles

In the [previous chapter](../10\_go\_on/be-ready.md) we already encountered the `.css()` method — we used it to change the font color, but its capabilities don't end there. Time to dig a bit deeper so we don't have to storm the forums with basic questions ;)

Let's start digging with a more thorough look at the `.css()` method:

<table data-header-hidden data-full-width="false"><thead><tr><th width="402">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">css(property)
</code></pre></td><td>get the value of a CSS property</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">css(property, value)
</code></pre></td><td>set the value of a CSS property</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>css({
</strong>  property1: value,
  property2: value
})
</code></pre></td><td>set multiple values</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">css(property,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>here a callback function is used to set the value, <code>index</code> is the element's position in the selection, <code>value</code> is the current property value</td></tr></tbody></table>

{% hint style="info" %}
The `css()` method returns the current computed value, not the one specified in the CSS file for the given selector.
{% endhint %}

Here are some usage examples:

```javascript
// set the font color value
$("#my").css("color", "red")

// change the background color
$("#my").css("background-color", "yellow")
```

To change multiple parameters, pass an object in key-value format (this is essentially JSON):

```javascript
$("#my").css({
    "color": "red",
    "font-size": "18px",
    "background-color": "white"
})
```

For property names you can use either CSS notation (see the example above) or the JavaScript variant:

```javascript
$("#my").css({
    color: "black",
    fontSize: "12px",
    backgroundColor: "transparent"
})
```

And here's an exotic way of changing the font size using a callback function:

```javascript
$("#my").css("font-size", function(i, value){
    return parseFloat(value) * 1.5;
})
```

An interactive example:

{% embed url="https://anton.shevchuk.name/book/code/css.html" %}
