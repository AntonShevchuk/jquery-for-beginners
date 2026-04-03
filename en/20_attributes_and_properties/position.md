---
description: It's not showing off, it's a position statement
---

# Position

Let me add a little more info about the methods that tell you about element positioning:

<table data-header-hidden><thead><tr><th width="323">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">offset()
</code></pre></td><td>returns the DOM element's position relative to the document, the data comes as an object: <br><code>{ top: 10, left: 30 }</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">offset({ top: 10, left: 30 })
</code></pre></td><td>sets the DOM element's position at the specified coordinates</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">position()
</code></pre></td><td>returns the DOM element's position relative to its parent element</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/offset-and-position.html" %}
