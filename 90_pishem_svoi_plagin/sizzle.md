## Sizzle {#sizzle}

Когда я рассказывал о

Sizzle

я решил вас не грузить возможностями по расширению библиотеки, но вот время настало… В Sizzle можно расширять много чего:

*   Sizzle.selectors.match
*   Sizzle.selectors.find
*   Sizzle.selectors.filter
*   Sizzle.selectors.attrHandle
*   Sizzle.selectors.pseudos

Но браться мы будем лишь за расширение псевдо-селекторов, наподобие:

$(&quot;div:animated&quot;); // поиск анимированных элементов

$(&quot;div:hidden&quot;); // поиск скрытых элементов div

$(&quot;div:visible&quot;); // поиск видимых элементов div

Почему я привел только эти фильтры? Всё просто — только они не входят в Sizzle, и относятся лишь к jQuery, именно такие плагины мы будем тренироваться разрабатывать. Начнем с кода фильтра «:visible»:

// пример для расширения Sizzle внутри jQuery

// для расширения самого Sizzle нужен чуть-чуть другой код

jQuery.expr.pseudos.visible = function( elem ) {

// проверяем ширину и высоту каждого элемента в выборке

return !!( elem.offsetWidth || elem.offsetHeight);

};

Выглядит данный код несложно, но, пожалуй, я таки дам каркас для нового фильтра и добавлю чуть-чуть пояснений:

$.extend($.expr.pseudos, {

/**

* @param element DOM элемент

* @param i порядковый номер элемента

* @param match объект матчинга регулярного выражения

* @param elements массив всех найденных DOM элементов

*/

test: function(element, i, match, elements) {

/* тут будет наш код, и будет решать кто виноват */

return true || false; // выносим вердикт

}

})

Ну теперь попробуем решить следующую задачку:

_— Необходимо выделить ссылки в тексте в зависимости от её типа: внешняя, внутренняя или якорь_

Для решения лучше всего подошли бы фильтры для селекторов следующего вида:

$(&quot;a:internal&quot;);

$(&quot;a:anchor&quot;);

$(&quot;a:external&quot;);

Поскольку «из коробки» данный функционал не доступен, мы напишем его сами, для этого нам понадобится не так уж и много (пример лишь для последнего «:external», рабочий код на странице [sizzle.filter.html](http://anton.shevchuk.name/book/code/sizzle.filter.html)):

$.extend($.expr.pseudos, {

// определения внешней ссылки

// нам понадобится лишь DOM Element

external: function(element) {

// а у нас ссылка?

if (element.tagName.toUpperCase() != &#039;A&#039;) return false;

// есть ли атрибут href

if (element.getAttribute(&#039;href&#039;)) {

var href = element.getAttribute(&#039;href&#039;);

// отсекаем ненужное

if ((href.indexOf(&#039;/&#039;) === 0) // внутренняя ссылка

_|| (href.indexOf(&#039;#&#039;) === 0)_ // якорь

// наш домен по http:// или https://

|| (href.indexOf(window.location.hostname) === 7)

|| (href.indexOf(window.location.hostname) === 8)

) {

return false; // мимо

} else {

_return true_; // да, мы нашли внешние ссылки

}

} else {

return false;

}

}

})

**_ВСЕГДА_****_используйте фильтр вместе с HTML тэгом который ищите:_**

**_$(&quot;tag:filter&quot;)_**

_Это один из пунктов оптимизации работы с фильтрами jQuery, иначе ваш фильтр будет обрабатывать все DOM элементы на странице, а это может очень сильно сказаться на производительности. Если же у вас несколько тегов, то пишите уж лучше так — «$(&quot;tag1:filter, tag2:filter, tag3:filter&quot;)», или ещё лучше через вызов метода «.filter()»._

*   «Sizzle Documentation» — скудненькая официальная документация

[[https://github.com/jquery/sizzle/wiki/Sizzle-Documentation](https://github.com/jquery/sizzle/wiki/Sizzle-Documentation)]