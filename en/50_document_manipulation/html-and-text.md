---
description: Content is king
---

# Managing Content

A few words about element content — we have 2 methods for working with it: `html()` and `text()`. There's one key difference between them — the first one can get and insert HTML code, while the second always operates with text. Even if you try to feed it HTML, you'll get text where the tags are converted to [HTML entities](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references):

```javascript
$("section").text("Some <strong>text</strong>");

<section>Some &lt;strong&gt;text&lt;/strong&gt;</section>
```

<table data-header-hidden><thead><tr><th width="271">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">html()
</code></pre></td><td>returns the HTML of the given element</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">html(newHtml)
</code></pre></td><td>replaces the HTML in the given element</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">text()
</code></pre></td><td>returns the text of the given element;<br>if there are other HTML tags inside the element, you'll get a mixed bag of text from all elements</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">text(newText)
</code></pre></td><td>replaces the text inside the selected elements; if you try to insert HTML this way, you'll still get text</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.html-and-text.html" %}
