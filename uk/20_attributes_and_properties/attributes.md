# Атрибути

Також варто згадати, що у DOM-елементів бувають атрибути, відмінні від класу, і ми їх теж можемо змінювати. Для цього нам знадобляться наступні методи:

<table data-header-hidden><thead><tr><th width="317">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">attr(attrName) 
</code></pre></td><td>отримання значення атрибута</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">attr(attrName, value)
</code></pre></td><td>встановлення значення атрибута</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">attr({
  attribute1: value,
  attribute2: value
})
</code></pre></td><td>встановлення кількох значень</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">attr(attrName,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>використовуючи функцію зворотного виклику<br><code>index</code> це порядковий номер елемента у вибірці<br><code>value</code> — поточне значення атрибута</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeAttr(attrName) 
</code></pre></td><td>видалення атрибута</td></tr></tbody></table>

Атрибути – це все те, що ми бачимо всередині кутових дужок, коли пишемо HTML-код:

```markup
<!-- У цьому прикладі це href, title, class -->
<a href="#top" title="anchor" class="simple">To Top</a>
```

Атрибути, з якими вам частіше за інших доведеться стикатися:

```javascript
// отримання адреси посилання
$("a").attr("href");

// зміна адреси та заголовку посилання
$("a").attr({
    "href": "http://anton.shevchuk.name",
    "title": "My Personal Blog"
});

// отримання альтернативного тексту картинки
$("img").attr("alt");

// зміна адреси картинки
$("img").attr("src", "/images/default.png");
```

