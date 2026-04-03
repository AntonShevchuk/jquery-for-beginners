# Data Attributes

HTML5 gives us the ability to use custom data attributes on elements:

```markup
<div data-foo="bar"></div>
```

For working with data attributes, jQuery has several methods:

<table data-header-hidden><thead><tr><th width="305">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">data()
</code></pre></td><td>get all values as key-value pairs:<br><code>{ key: value }</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>data(key)
</strong></code></pre></td><td>get a value by key</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">data(key, value)
</code></pre></td><td>set a value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">data({
  key1: value,
  key2: value
})
</code></pre></td><td>set multiple values</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeData(key)
</code></pre></td><td>remove a value by key or keys</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeData([key])
</code></pre></td><td>remove multiple values</td></tr></tbody></table>

{% hint style="danger" %}
Here comes a tricky and important moment!\


The `jQuery.data()` method does not manipulate HTML attributes — it works with its own internal registry, and only when data is missing there does it try to fetch information from the corresponding `data-*` attribute.\


Don't get caught!

```html
<div id="my" data-foo="bar"></div>
```

```javascript
$("#my").data("foo");      // >>> bar
$("#my").attr("data-foo"); // >>> bar

$("#my").data("foo", "xyz");

$("#my").data("foo");      // >>> xyz
$("#my").attr("data-foo"); // >>> bar !!!

$("#my").attr("data-foo", "def");

$("#my").data("foo");      // >>> xyz !!!
$("#my").attr("data-foo"); // >>> def
```
{% endhint %}

An interactive example:

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}
