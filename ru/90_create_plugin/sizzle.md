# Sizzle

Когда я рассказывал о Sizzle, то решил вас не грузить возможностями по расширению библиотеки, и вот время настало… В Sizzle можно расширять много чего:

* `Sizzle.selectors.match`
* `Sizzle.selectors.find`
* `Sizzle.selectors.filter`
* `Sizzle.selectors.attrHandle`
* `Sizzle.selectors.pseudos`

Но браться мы будем лишь за расширение псевдо-селекторов, наподобие:

```javascript
$("div:animated"); // поиск анимированных элементов
$("div:hidden");   // поиск скрытых элементов div
$("div:visible");  // поиск видимых элементов div
```

Почему я привел только эти фильтры? Всё просто — только они не входят в Sizzle и относятся лишь к jQuery, именно такие плагины мы будем тренироваться разрабатывать. Начнем с кода фильтра `:visible`:

```javascript
// пример для расширения Sizzle внутри jQuery
// для расширения самого Sizzle нужен чуть-чуть другой код
jQuery.expr.pseudos.visible = function( elem ) {
    // проверяем ширину и высоту каждого элемента в выборке
    return !!( elem.offsetWidth || elem.offsetHeight);
};
```

Выглядит данный код несложно, но, пожалуй, я-таки дам каркас для нового фильтра и добавлю чуть-чуть пояснений:

```javascript
$.extend($.expr.pseudos, {
    /**
    * @param element DOM-элемент
    * @param i порядковый номер элемента
    * @param match объект матчинга регулярного выражения
    * @param elements массив всех найденных DOM-элементов
    */
    test: function(element, i, match, elements) {
        /* тут будет наш код, и он будет решать, кто виноват */
        return true || false; // выносим вердикт
    }
})
```

Ну, теперь попробуем решить следующую задачку:

_— Необходимо выделить ссылку в тексте в зависимости от её типа: внешняя, внутренняя или якорь на странице._

Для решения лучше всего подошли бы фильтры для селекторов следующего вида:

```javascript
$("a:internal");

$("a:anchor");

$("a:external");
```

Поскольку «из коробки» данный функционал не доступен, мы напишем его сами. Для этого нам понадобится не так уж и много (пример лишь для последнего `:external`, рабочий код на странице [sizzle.filter.html](https://anton.shevchuk.name/book/code/sizzle.filter.html)):

```javascript
$.extend($.expr.pseudos, {
    // определения внешней ссылки
    // нам понадобится лишь DOM-Element
    external: function(element) {
        // а у нас ссылка?
        if (element.tagName.toUpperCase() != 'A') return false;
        // есть ли атрибут href
        if (element.getAttribute('href')) {
            var href = element.getAttribute('href');
            // отсекаем ненужное
            if ((href.indexOf('/') === 0)  // внутренняя ссылка
              || (href.indexOf('#') === 0) // якорь
              || (href.indexOf(window.location.hostname) === 7) // наш домен по http://
              || (href.indexOf(window.location.hostname) === 8) // или https://
            ) {
                return false; // мимо
            } else {
                return true;  // да, мы нашли внешние ссылки
            }
        } else {
            return false;
        }
    }
});
```

{% embed url="https://anton.shevchuk.name/book/code/sizzle.filter.html" %}

{% hint style="danger" %}
ВСЕГДА используйте фильтр вместе с HTML-тегом, который ищете:

<pre class="language-javascript"><code class="lang-javascript"><strong>// tag name + filter!
</strong><strong>$("div:filter")
</strong></code></pre>
{% endhint %}

{% hint style="info" %}
Это один из пунктов оптимизации работы с фильтрами jQuery, иначе ваш фильтр будет обрабатывать все DOM-элементы на странице, а это может очень сильно сказаться на производительности. Если же у вас несколько тегов, то пишите уж лучше так — `$("tag1:filter, tag2:filter, tag3:filter")`, или, ещё лучше, через вызов метода `filter()`.
{% endhint %}

* [Sizzle Documentation](https://github.com/jquery/sizzle/wiki/) — скудненькая официальная документация
