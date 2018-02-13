## Sizzle {#sizzle}

_Пропустите это раздел, и вернитесь к нему тогда, когда вас заинтересует, как происходит поиск элементов внутри «$»_

В качестве «поисковика» по элементам DOM&#039;а jQuery использует библиотеку Sizzle. Данная библиотека одно время была неотъемлемой частью jQuery, затем «отпочковалась» в отдельный проект, который с радостью используется как самим jQuery, так и Dojo Toolkit. Но хватит лирики, давайте перейдем непосредственно к поиску. Для оного в JavaScript&#039;е предусмотрены следующие методы (не в jQuery, не в Sizzle, а в JavaScript&#039;е):

getElementById(_id_) — поиск по «id=&quot;…&quot;»

getElementsByName(_name_) — поиск по «name=&quot;name&quot;» и «id=&quot;name&quot;»

getElementsByClassName(_class_) — поиск по «class=&quot;class&quot;»

getElementsByTagName(_tag_) — поиск по тегу

querySelectorAll(_selector_) — поиск по произвольному CSS-селектору

Пробежавшись беглым взглядом по списку, можно заметить метод «querySelectorAll()» – он универсален и действительно удобен. И именно этот метод библиотека пытается вызвать, когда вы скармливаете что-то в качестве селектора в jQuery, но данный метод иногда нас подводит, и тогда на сцену выходит Sizzle во всей красе, вооруженный всеми упомянутыми методами, да ещё со своим уникальным арсеналом:

if (document.querySelectorAll) (function(){

var oldSelect = select

/* ... */

select = function( selector, context, results, seed, xml ) {

// используем querySelectorAll когда нет фильтров в запросе,

// когда это запрос не по xml-объекту,

// и когда не обнаружено проблем с запросом

// ещё есть пара проверок, которые я опустил для наглядности

try {

push.apply(

results,

slice.call(context.querySelectorAll( selector ), 0)

);

return results;

} catch(qsaError) { /* подвёл, опять, ну сколько можно */ }

/* ... */

// выход Sizzle

return oldSelect( selector, context, results, seed, xml );

};

});

Но давайте уже рассмотрим, как Sizzle ищет в DOM&#039;е, если-таки метод «document.querySelectorAll()» споткнулся. Начнём с разбора алгоритма работы на следующем примере:

$(&quot;thead &gt; .active, tbody &gt; .active&quot;)

1.  Получить первое выражение до запятой: «thead &gt; .active»
2.  Разбить на кусочки: «thead», «&gt;», «.active»
3.  Найти исходное множество элементов по последнему кусочку (все «.active»)
4.  Фильтровать справа налево (все «.active» которые находятся непосредственно в «thead»)
5.  Искать следующий запрос после запятой (возвращаемся к первому пункту)
6.  Оставить только уникальные элементы в результате

Глядя на данный алгоритм вы должны заметить, что **правила читаются справа налево**!

Копаем глубже. При анализе даже самого маленького выражения есть несколько нюансов, на которые стоит обратить внимание. Первый – это приоритет поиска, он различен для различных браузеров (в зависимости от их возможностей, или «невозможностей»):

order: new RegExp( &quot;ID|TAG&quot; +

(assertUsableName ? &quot;|NAME&quot; : &quot;&quot;) +

(assertUsableClassName ? &quot;|CLASS&quot; : &quot;&quot;)

)

_Не обращайте внимание на RegExp – это внутренняя кухня Sizzle_

Таким образом, рассматривая выражение «div#my», Sizzle найдёт вначале элемент с «id=&quot;my&quot;», а потом уже проверит на соответствие с &lt;div&gt;. Хорошо, это вроде как фильтрация, и она тоже соблюдает очерёдность – это второй нюанс:

preFilter: {

&quot;ATTR&quot;: function (match) { /* ... */ },

&quot;CHILD&quot;: function (match) { /* ... */ },

&quot;PSEUDO&quot;: function (match) { /* ... */ },

},

filter: {

&quot;ID&quot;: function (id) { /* ... */ },

&quot;TAG&quot;: function (nodeName) { /* ... */ },

&quot;CLASS&quot;: function (className) { /* ... */ },

&quot;ATTR&quot;: function (name, operator, check) { /* ... */ },

&quot;CHILD&quot;: function (type, argument, first, last) { /* ... */ },

&quot;PSEUDO&quot;: function (pseudo, argument, context, xml) { /* ... */ }

}

Третий нюанс – это относительные фильтры, которые работают сразу с двумя элементами (они вам знакомы по CSS-селекторам):

relative: {

&quot;&gt;&quot;: { dir: &quot;parentNode&quot;, first: true },

&quot; &quot;: { dir: &quot;parentNode&quot; },

&quot;+&quot;: { dir: &quot;previousSibling&quot;, first: true },

&quot;~&quot;: { dir: &quot;previousSibling&quot; }

},

Ой, зачем я вас всем этим гружу? Почитайте лучше об оптимизации запросов абзацем ниже.

Официальная документация по библиотеке Sizzle доступна на GitHub&#039;е проекта:

*   «Sizzle Documentation»

[[https://github.com/jquery/sizzle/wiki/Sizzle-Documentation](https://github.com/jquery/sizzle/wiki/Sizzle-Documentation)]