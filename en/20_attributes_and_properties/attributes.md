# Attributes

Also, it's worth remembering that DOM elements have attributes other than class, and we can change those too. For this we'll need the following methods:

<table data-header-hidden><thead><tr><th width="317">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">attr(attrName) 
</code></pre></td><td>get the attribute value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">attr(attrName, value)
</code></pre></td><td>set the attribute value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">attr({
  attribute1: value,
  attribute2: value
})
</code></pre></td><td>set multiple values</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">attr(attrName,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>using a callback function<br><code>index</code> is the element's position in the selection<br><code>value</code> is the current attribute value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeAttr(attrName) 
</code></pre></td><td>remove an attribute</td></tr></tbody></table>

Attributes are everything we see inside the angle brackets when writing HTML code:

```markup
<!-- In this example those are href, title, class -->
<a href="#top" title="anchor" class="simple">To Top</a>
```

Attributes you'll encounter most often:

```javascript
// get the link's URL
$("a").attr("href");

// change the link's URL and title
$("a").attr({
    "href": "https://anton.shevchuk.name",
    "title": "My Personal Blog"
});

// get the image's alt text
$("img").attr("alt");

// change the image's source
$("img").attr("src", "/images/default.png");
```

