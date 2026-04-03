# Sizzle

Коли я розповідав про Sizzle, то вирішив тебе не вантажити можливостями розширення бібліотеки, і ось час настав... У Sizzle можна розширювати багато чого:

* `Sizzle.selectors.match`
* `Sizzle.selectors.find`
* `Sizzle.selectors.filter`
* `Sizzle.selectors.attrHandle`
* `Sizzle.selectors.pseudos`

Але братися ми будемо лише за розширення псевдо-селекторів, на кшталт:

```javascript
$("div:animated"); // пошук анімованих елементів
$("div:hidden");   // пошук прихованих елементів div
$("div:visible");  // пошук видимих елементів div
```

Чому я навів тільки ці фільтри? Все просто — тільки вони не входять до Sizzle і стосуються лише jQuery, саме такі плагіни ми будемо тренуватися розробляти. Почнемо з коду фільтра `:visible`:

```javascript
// приклад для розширення Sizzle всередині jQuery
// для розширення самого Sizzle потрібен трішки інший код
jQuery.expr.pseudos.visible = function( elem ) {
    // перевіряємо ширину та висоту кожного елемента у вибірці
    return !!( elem.offsetWidth || elem.offsetHeight);
};
```

Виглядає цей код нескладно, але, мабуть, я таки дам каркас для нового фільтра і додам трішки пояснень:

```javascript
$.extend($.expr.pseudos, {
    /**
    * @param element DOM-елемент
    * @param i порядковий номер елемента
    * @param match об'єкт матчингу регулярного виразу
    * @param elements масив усіх знайдених DOM-елементів
    */
    test: function(element, i, match, elements) {
        /* тут буде наш код, і він вирішуватиме, хто винен */
        return true || false; // виносимо вердикт
    }
})
```

Ну, тепер спробуємо вирішити таку задачку:

_— Необхідно виділити посилання в тексті залежно від його типу: зовнішнє, внутрішнє або якір на сторінці._

Для вирішення найкраще підійшли б фільтри для селекторів такого виду:

```javascript
$("a:internal");

$("a:anchor");

$("a:external");
```

Оскільки «з коробки» цей функціонал недоступний, ми напишемо його самі. Для цього нам знадобиться не так вже й багато (приклад лише для останнього `:external`, робочий код на сторінці [sizzle.filter.html](https://anton.shevchuk.name/book/code/sizzle.filter.html)):

```javascript
$.extend($.expr.pseudos, {
    // визначення зовнішнього посилання
    // нам знадобиться лише DOM-Element
    external: function(element) {
        // а в нас посилання?
        if (element.tagName.toUpperCase() != 'A') return false;
        // чи є атрибут href
        if (element.getAttribute('href')) {
            var href = element.getAttribute('href');
            // відсікаємо непотрібне
            if ((href.indexOf('/') === 0)  // внутрішнє посилання
              || (href.indexOf('#') === 0) // якір
              || (href.indexOf(window.location.hostname) === 7) // наш домен по http://
              || (href.indexOf(window.location.hostname) === 8) // або https://
            ) {
                return false; // мимо
            } else {
                return true;  // так, ми знайшли зовнішні посилання
            }
        } else {
            return false;
        }
    }
});
```

{% embed url="https://anton.shevchuk.name/book/code/sizzle.filter.html" %}

{% hint style="danger" %}
ЗАВЖДИ використовуй фільтр разом з HTML-тегом, який шукаєш:

<pre class="language-javascript"><code class="lang-javascript"><strong>// tag name + filter!
</strong><strong>$("div:filter")
</strong></code></pre>
{% endhint %}

{% hint style="info" %}
Це один з пунктів оптимізації роботи з фільтрами jQuery, інакше твій фільтр оброблятиме всі DOM-елементи на сторінці, а це може дуже сильно позначитися на продуктивності. Якщо ж у тебе кілька тегів, то пиши вже краще так — `$("tag1:filter, tag2:filter, tag3:filter")`, або, ще краще, через виклик методу `filter()`.
{% endhint %}

* [Sizzle Documentation](https://github.com/jquery/sizzle/wiki/) — скупенька офіційна документація
