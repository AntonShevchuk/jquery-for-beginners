# Cloning and Removing

While the world debates about cloning, programmers already have it on the assembly line :)

For cloning an element there's the `clone()` method — it creates a clone of the element, and you can insert it into the DOM using any of the [appropriate methods](manipulation.md).

What's more, if you had event handlers attached, you can clone those too — just set the first argument to `true`:

```javascript
$("h1").click(() => alert("h1!"))

// clone without event handlers
$("h1").clone()

// clone with event handlers
$("h1").clone(true)
```

<table data-header-hidden><thead><tr><th width="326">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">clone()
</code></pre></td><td>clones the selected elements for further insertion of copies back into the DOM</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">clone(
  withDataAndEvents = true
)
</code></pre></td><td>clones the selected elements along with current event handlers and <code>data()</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">clone(
  withDataAndEvents = true,
  deepWithDataAndEvents = true
)
</code></pre></td><td>also clones event handlers and data of child elements</td></tr></tbody></table>

Besides cloning, you can detach any element from the DOM to modify and insert it back, or you can even remove it entirely:

<table data-header-hidden><thead><tr><th width="326">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">detach()
</code></pre></td><td>detaches the element from the DOM while preserving all its data in jQuery; use this when you only need to temporarily remove an element</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">empty()
</code></pre></td><td>removes text and child DOM elements</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">remove()
</code></pre></td><td>permanently removes the element from the DOM</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.clone-and-other.html" %}
