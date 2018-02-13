## Селекторы

Как я уже говорил ранее, в поиске элементов на странице заключается практически половина успешной работы с jQuery. Так что приступим к поискам по документу (чтобы не листать, пусть пример HTML будет и тут):

&lt;!DOCTYPE html&gt;

&lt;html dir=&quot;ltr&quot; lang=&quot;en-US&quot;&gt;

&lt;head&gt;

&lt;meta charset=&quot;UTF-8&quot;/&gt;

&lt;title&gt;Page Title&lt;/title&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;header&gt;

&lt;h1&gt;Page Title&lt;/h1&gt;

&lt;p&gt;Page Description&lt;/p&gt;

&lt;/header&gt;

&lt;div id=&quot;content&quot; class=&quot;wrapper box&quot;&gt;

&lt;article&gt;

&lt;h2&gt;Article Title&lt;/h2&gt;

&lt;p&gt;Lorem ipsum dolor sit amet, consectetuer adipiscing.

Nunc urna metus, ultricies eu, congue, laoreet...&lt;/p&gt;

&lt;/article&gt;

&lt;hr id=&quot;stick&quot;/&gt;

&lt;article&gt;

&lt;h2&gt;Article Title&lt;/h2&gt;

&lt;p&gt;Morbi malesuada, ante at feugiat tincidunt, enim

gravida metus, lacinia massa diam vel eros...&lt;/p&gt;

&lt;/article&gt;

&lt;/div&gt;

&lt;footer&gt;&amp;copy;copyright 2014&lt;/footer&gt;

&lt;/body&gt;

&lt;/html&gt;

А теперь приступим к выборкам — выбор элементов по «id» либо «className» аналогично используемым в CSS:

$(&quot;#content&quot;) // выбираем элемент с id=content

$(&quot;div#content&quot;) // выбираем div с id=content (хотя и без div работает)

$(&quot;.wrapper&quot;) // выбираем элементы с class=wrapper

$(&quot;div.wrapper&quot;) // выбираем div&#039;ы с class=wrapper

$(&quot;.wrapper.box&quot;) // выбираем элементы с class=wrapper и box

$(&quot;h2&quot;) // выбираем все теги h2

$(&quot;h1, h2&quot;) // выбираем все теги h1 и h2

_Используйте валидные имена классов и идентификаторов_

Теперь вспомним, что мы в DOMе не одни, это-таки иерархическая структура:

$(&quot;article h2&quot;) // выбираем все теги h2 внутри тега article

$(&quot;div article h2&quot;) // выбираем все теги h2 внутри тега article

// внутри тега div, в доме который построил Джек

$(&quot;article&quot;).find(&quot;h2&quot;) // аналогично примерам выше

$(&quot;div&quot;).find(&quot;article&quot;).find(&quot;h2&quot;) //

У нас есть соседи:

$(&quot;h1 + p&quot;) // выбор всех p элементов, перед которыми есть h1

// элементы (у нас только один такой)

$(&quot;#stick ~ article&quot;) // выбор всех article элементов после элемента

// c id=stick

$(&quot;#stick&quot;).prev() // выбор предыдущего элемента от найденного

$(&quot;#stick&quot;).next() // выбор следующего элемента от найденного

Родственные связи:

$(&quot;*&quot;) // выбор всех элементов

$(&quot;article &gt; h2&quot;) // выбираем все теги h2 которые являются

// непосредственными потомками тега article

$(&quot;article &gt; *&quot;) // выбор всех потомков элементов article

$(&quot;article&quot;).children()

$(&quot;p&quot;).parent() // выбор всех прямых предков элементов p

$(&quot;p&quot;).parents() // выбор всех предков элементов p (не понадобится)

$(&quot;p&quot;).parents(&quot;div&quot;) // выбор всех предков элемента p которые есть div

// parents принимает в качестве параметра селектор

_Если хотите поиграться с селекторами от души, то для этого я припас для вас соответствующую страничку —_ [_css.selectors.html_](http://anton.shevchuk.name/book/code/css.selectors.html)

### ****Поиск по атрибутам**** {#-0}

Ещё со времён CSS2 была возможность найти элемент с определёнными атрибутами, в CSS3 добавили ещё возможностей по поиску:

a[href] — все ссылки с атрибутом «href»

a[href=#] — все ссылки с «href=#»

a[href~=#] — все ссылки с «#» где-то в «href»

a[hreflang|=en] — все ссылки, для которых hreflang начинается с «en» и обрезается по символу «-» — «en», «en-US», «en-UK»

a[href^=http] — ссылки, начинающиеся с «http»

a[href*=&quot;google.com&quot;] — ссылки на погуглить

a[href$=.pdf] — ссылки на PDF-файлы (по идее)

_Заглянув внутрь jQuery, вы, скорей всего, найдёте то место, где ваше выражение будет анализироваться с помощью регулярных выражений, по этой причине в селекторах необходимо экранировать специальные символы, используя двойной обратный слеш «_\\_»:_

$(&quot;a[href^=\\/]&quot;).addClass(&quot;internal&quot;);

### Поиск по дочерним элементам {#-1}

Хотелось бы ещё обратить внимание на селекторы из спецификации CSS3 [[http://www.w3.org/TR/css3-selectors/](http://www.w3.org/TR/css3-selectors/)] — много интересных:

:first-child — первый дочерний элемент

:last-child — последний дочерний элемент

:nth-child(2n+1) — выборка элементов по несложному уравнению

подробнее можно прочитать в статье «Как работает nth-child» [[http://web-standards.ru/articles/nth-child/](http://web-standards.ru/articles/nth-child/)]

:not(…) — выбрать те, что не подпадают под вложенную выборку

Но поскольку не все браузеры знакомы с CSS3-селекторами, то мы можем использовать jQuery для назначения стилей:

$(&quot;div:last-child&quot;).addClass(&quot;last-paragraph&quot;);