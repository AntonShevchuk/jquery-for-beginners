## Callbacks {#callbacks}

Callbacks – это крутой объект – он позволяет составлять списки функций обратного вызова, а также даёт бразды правления над ними. Работать с ним проще нежели с Deferred, тут нет разделения на позитивный и негативный сценарии, лишь стек функций, который будет выполнен по команде «.fire()»:

var C = $.Callbacks();

C.add(function(msg) {

console.log(msg+&quot; first&quot;)

});

C.add(function(msg) {

console.log(msg+&quot; second&quot;)

});

C.fire(&quot;Go&quot;);

&gt;&gt;&gt;

Go first

Go second

_— А в чём сила, брат?_

_— В аргументах_

По умолчанию, вы можете прямо из консоли вызвать метод «.fire()» снова и снова, и будете получать один и тот же результат раз за разом. А можно задать поведение Callbacks через флаги:

once — все функции будут вызваны единожды (аналогично как в объекте Deferred).

memory — сохранять значение с последнего вызова «.fire()», и скармливать его в ново-зарегистрированные функции обратного вызова, и лишь потом обрабатывает новое значение (в Deferred именно так).

unique — список функций обратного вызова фильтруется по уникальности

stopOnFalse — как только какая-нить функция вернёт «false», процесс запуска остановится

Наверное, будет лучше с примерами, вот «once»:

var C = $.Callbacks(&quot;once&quot;);

C.add(function(msg) {

console.log(msg+&quot; first&quot;)

});

C.add(function(msg) {

console.log(msg+&quot; second&quot;)

});

C.fire(&quot;Go&quot;);

C.fire(&quot;Again&quot;); // не даст результата, только Go

&gt;&gt;&gt;

Go first

Go second

C «memory» посложнее, будьте внимательней:

var C = $.Callbacks(&quot;memory&quot;);

C.add(function(msg) {

console.log(msg+&quot; first&quot;)

});

C.fire(&quot;Go&quot;);

C.add(function(msg) {

console.log(msg+&quot; second&quot;)

});

C.fire(&quot;Again&quot;);

&gt;&gt;&gt;

Go first

Go second // без флага, этой строчки не было бы

Again first

Again second

Пример с уникальностью прост до безобразия:

var C = $.Callbacks(&quot;unique&quot;);

var func = function(msg) {

console.log(msg+&quot; first&quot;)

};

C.add(func);

C.add(func); // эта строка не повлияет на результат

C.fire(&quot;Go&quot;); // только Go first

&gt;&gt;&gt;

Go first

Флаг «stopOnFalse»:

var C = $.Callbacks(&quot;stopOnFalse&quot;);

C.add(function(msg) {

console.log(msg+&quot; first&quot;);

return false; // вот он – роковой false

});

C.add(function(msg) { console.log(msg+&quot; second&quot;) });

C.fire(&quot;Go&quot;); // только Go first

&gt;&gt;&gt;

Go first

Перечисленные флаги можно комбинировать и получать интересные результаты, а можно не получать, а лишь посмотреть на пример [callbacks.html](http://anton.shevchuk.name/book/code/callbacks.html)

_Из_ истории_: объект «Deferred» отпочковался от метода «$.ajax()» в результате рефакторинга версии 1.5\. Шло время, появлялись новые версии jQuery, и вот новый виток рефакторинга – результатом стало отделение «Callbacks» от «Deferred» в версии 1.7, таким образом в текущей версии библиотеки метод «$.ajax()» работает с объектом «Deferred», который является надстройкой над «Callbacks». Дабы не вносить путаницу в терминологию, я использую определение «Deferred Callbacks» и при работе с «Callbacks», ибо колбэков много, и каждый раз уточнять, что я говорю именно «о том самом» — дело достаточно утомительное._

Статьи по данной теме:

*   «Что такое этот новый jQuery.Callbacks Object»

[[http://habrahabr.ru/post/135821/](http://habrahabr.ru/post/135821/)]

*   «jQuery Deferred Object (подробное описание)»

[[http://habrahabr.ru/post/113073/](http://habrahabr.ru/post/113073/)]

*   «Async JS: The Power of $.deffered»

[[http://www.html5rocks.com/en/tutorials/async/deferred/](http://www.html5rocks.com/en/tutorials/async/deferred/)]