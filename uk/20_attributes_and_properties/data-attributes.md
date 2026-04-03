# Data-атрибути

HTML5 дає нам можливість використовувати довільні data-атрибути для елементів:

```markup
<div data-foo="bar"></div>
```

Для роботи з data-атрибутами в jQuery є кілька методів:

<table data-header-hidden><thead><tr><th width="305">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">data()
</code></pre></td><td>отримання всіх значень у форматі ключ-значення:<br><code>{ key: value }</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>data(key)
</strong></code></pre></td><td>отримання значення за ключем</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">data(key, value)
</code></pre></td><td>встановлення значення</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">data({
  key1: value,
  key2: value
})
</code></pre></td><td>встановлення кількох значень</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeData(key)
</code></pre></td><td>видалення значення за ключем або ключами</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeData([key])
</code></pre></td><td>видалення кількох значень</td></tr></tbody></table>

{% hint style="danger" %}
Ось зараз буде складний і важливий момент!\


Метод`jQuery.data()` не маніпулює атрибутами HTML, а працює зі своїм реєстром, і лише за відсутності там даних намагається отримати інформацію з відповідного атрибута `data-*`.\


Не попадіться!

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

Наочний приклад:

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}
