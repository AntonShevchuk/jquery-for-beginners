---
description: Контент — это главное
---

# Управление содержимым

Ну и немного про содержимое элементов, для работы с ним у нас есть 2 метода: `html()` и `text()`, между ними одна существенная разница — первый умеет получать и вставлять HTML код, второй же всегда оперирует текстом. Даже если вы попытаетесь скормить ему HTML, в результате получите текст, где тэги будут приведены к [HTML entities](https://ru.wikipedia.org/wiki/%D0%9C%D0%BD%D0%B5%D0%BC%D0%BE%D0%BD%D0%B8%D0%BA%D0%B8\_%D0%B2\_HTML):

```javascript
$("section").text("Some <strong>text</strong>");

<section>Some &lt;strong&gt;text&lt;/strong&gt;</section>
```

<table data-header-hidden><thead><tr><th width="271">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">html()
</code></pre></td><td>возвращает HTML заданного элемента</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">html(newHtml)
</code></pre></td><td>заменяет HTML в заданном элементе</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">text()
</code></pre></td><td>возвращает текст заданного элемента;<br>если внутри элемента будут другие HTML-теги, то вернётся сборная солянка из текста всех элементов</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">text(newText)
</code></pre></td><td>заменяет текст внутри выбранных элементов, при попытке вставить таким образом HTML, получим всё равно текст</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.html-and-text.html" %}
