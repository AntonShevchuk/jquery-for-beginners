# Data-атрибуты

HTML5 даёт нам возможность использовать произвольные data-атрибуты для элементов:

```markup
<div data-foo="bar"></div>
```

Для работы с data-атрибутами в jQuery есть несколько методов:

<table data-header-hidden><thead><tr><th width="305">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">data()
</code></pre></td><td>получение всех значений в формате ключ-значение:<br><code>{ key: value }</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>data(key)
</strong></code></pre></td><td>получение значения по ключу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">data(key, value)
</code></pre></td><td>установка значения</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">data({
  key1: value,
  key2: value
})
</code></pre></td><td>установка нескольких значений</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeData(key)
</code></pre></td><td>удаление значения по ключу или ключам</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeData([key])
</code></pre></td><td>удаление нескольких значений</td></tr></tbody></table>

{% hint style="danger" %}
Вот сейчас будет сложный и важный момент!\


Метод`jQuery.data()` не манипулирует атрибутами HTML, а работает со своим реестром, и лишь при отсутствии там данных пытается заполучить информацию из соответствующего атрибута `data-*`.\


Не попадитесь!

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

Наглядный пример:

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}
