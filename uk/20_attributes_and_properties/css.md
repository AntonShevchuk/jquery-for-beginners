# CSS стилі

У [попередньому розділі](../10\_go\_on/be-ready.md) ми вже стикалися з методом `.css()`, з його допомогою ми змінювали колір шрифту, але на цьому його можливості не закінчуються. Час копнути глибше, щоб не штурмувати форуми банальними питаннями ;)

Копати почнемо з більш досконального вивчення методу `.css()`:

<table data-header-hidden data-full-width="false"><thead><tr><th width="402">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">css(property)
</code></pre></td><td>отримання значення CSS-властивості</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">css(property, value)
</code></pre></td><td>встановлення значення CSS-властивості</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>css({
</strong>  property1: value,
  property2: value
})
</code></pre></td><td>встановлення кількох значень</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">css(property,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>тут для встановлення значення використовується функція зворотного виклику (простіше кажучи — callback-функція), <code>index</code> це порядковий номер елемента у вибірці, <code>value</code> — поточне значення властивості</td></tr></tbody></table>

{% hint style="info" %}
Метод `css()` повертає поточне значення, а не прописане в CSS-файлі за вказаним селектором.
{% endhint %}

Наведу приклади використання:

```javascript
// встановлюємо значення кольору шрифту
$("#my").css("color", "red")

// змінюємо колір фону
$("#my").css("background-color", "yellow")
```

Для зміни кількох параметрів передаємо об'єкт у форматі ключ-значення (це фактично JSON):

```javascript
$("#my").css({
    "color": "red",
    "font-size": "18px",
    "background-color": "white"
})
```

Для іменування властивостей можна використовувати як CSS-нотацію (див. приклад вище), так і JavaScript варіант:

```javascript
$("#my").css({
    color: "black",
    fontSize: "12px",
    backgroundColor: "transparent"
})
```

А ось перед нами екзотичний спосіб зміни шрифту з використанням функції зворотного виклику:

```javascript
$("#my").css("font-size", function(i, value){
    return parseFloat(value) * 1.5;
})
```

Наочний інтерактивний приклад:

{% embed url="https://anton.shevchuk.name/book/code/css.html" %}
