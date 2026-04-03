---
description: Контент — це головне
---

# Керування вмістом

Ну і трохи про вміст елементів, для роботи з ним у нас є 2 методи: `html()` та `text()`, між ними одна суттєва різниця — перший вміє отримувати та вставляти HTML код, другий же завжди оперує текстом. Навіть якщо ви спробуєте згодувати йому HTML, в результаті отримаєш текст, де теги будуть приведені до [HTML entities](https://uk.wikipedia.org/wiki/%D0%9C%D0%BD%D0%B5%D0%BC%D0%BE%D0%BD%D1%96%D0%BA%D0%B8_%D0%B2_HTML):

```javascript
$("section").text("Some <strong>text</strong>");

<section>Some &lt;strong&gt;text&lt;/strong&gt;</section>
```

<table data-header-hidden><thead><tr><th width="271">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">html()
</code></pre></td><td>повертає HTML заданого елемента</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">html(newHtml)
</code></pre></td><td>замінює HTML в заданому елементі</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">text()
</code></pre></td><td>повертає текст заданого елемента;<br>якщо всередині елемента будуть інші HTML-теги, то повернеться мішанина з тексту всіх елементів</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">text(newText)
</code></pre></td><td>замінює текст всередині вибраних елементів, при спробі вставити таким чином HTML, отримаємо все одно текст</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.html-and-text.html" %}
