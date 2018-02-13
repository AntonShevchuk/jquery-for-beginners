## CSS-правила и селекторы {#css}

Теперь приступим к CSS, и начнём, пожалуй, с расшифровки аббревиатуры CSS. Это Cascading Style Sheets, дословно «каскадная таблица стилей», но:

_— Почему же она называется каскадной?_ — этот вопрос я часто задаю на собеседованиях претендентам. Ответом же будет аналогия, ибо она незыблема как перпендикулярная лягушка: представьте себе каскад водопада, вот вы стоите на одной из ступенек каскада с чернильницей в руках, и выливаете её содержимое в воду — вся вода ниже по каскаду будет окрашиваться в цвет чернил. Применив эту аналогию на HTML — создавая правило для элемента, вы автоматически применяете его на все дочерние элементы (конечно, не все правила так работают, но о них позже) — это наследование стилей. Теперь, если таких умников с чернильницей больше чем один и цвета разные, то в результате получим смешение цветов. Но это в жизни, а в CSS работают правила приоритетов, если кратко и по делу:

*   самый низкий приоритет имеют стили браузера по умолчанию — в разных браузерах они могут отличаться, поэтому придумали [CSS Reset](http://www.google.com/search?q=CSS+Reset) (гуглится и юзается), и все будут на равных
*   чуть важнее — стили, заданные пользователем в недрах настроек браузера; встречаются редко
*   самые важные — стили автора странички, но и там всё идёт по порядку
    *   самый низкий приоритет у тех, что лежат во внешнем подключённом файле
    *   затем те, что встроили внутрь HTML с помощью тега &lt;style&gt;
    *   потом те, что захардкодили плохие люди (не вы, вы так делать не будете) в атрибуте «style»
    *   самый высокий приоритет у правил с меткой «!important»
    *   при равенстве приоритетов тапки у того, кто объявлен последним

Если голова ещё не бо-бо, то я также упомяну, что при расчёте, чьи же правила главней, анализируется специфичность селекторов, и тут считается следующим образом:

расчёт происходит по четырём весовым позициям [0:0:0:0]

стили заданные в атрибуте «style=&quot;&quot;» имеют наибольший приоритет и получают еденицу по первой позиции — [1:0:0:0]

за каждый идентификатор элемента — [0:1:0:0] (#id)

за каждый класс, либо псевдо класс — [0:0:1:0] (.my, :pseudo)

за каждый тег — [0:0:0:1] (div, a)

при этом [1:0:0:0] &gt; [0:x:y:z] &gt; [0:0:x:y] &gt; [0:0:0:x].

Пример селекторов, выстроенных по приоритету (первые важнее):

#my p#id — [0:2:0:1]

#my #id — [0:2:0:0]

#my p — [0:1:0:1]

#id — [0:1:0:0]

.wrapper .content p — [0:0:2:1]

.content div p — [0:0:1:2]

.content p — [0:0:1:1]

P — [0:0:0:1]

HTML-код для иллюстрации специфичности из предыдущего примера (см. [css.priority.html](http://anton.shevchuk.name/book/code/css.priority.html)):

&lt;div class=&quot;wrapper&quot;&gt;

&lt;div id=&quot;my&quot; class=&quot;content&quot;&gt;

&lt;p id=&quot;id&quot;&gt;

Lorem ipsum dolor sit amet, consectetuer...

&lt;/p&gt;

&lt;/div&gt;

&lt;/div&gt;

При равенстве счета — последний главный.

Говорят, что правило с 255 классами будет выше по приоритету, нежели правило с одним «id», но я надеюсь, такого кода в реальности не существует

Вот такое краткое вступительное слово, но пора вернуться к jQuery. Так вот, работая с jQuery, вы должны «на отлично» читать правила CSS, а также уметь составлять CSS-селекторы для поиска необходимых элементов на странице. Но давайте обо всём по порядку, возьмём следующий простенький пример вполне семантического HTML (см. [html.example.html](http://anton.shevchuk.name/book/code/html.example.html)):

&lt;!DOCTYPE html&gt;

&lt;html dir=&quot;ltr&quot; lang=&quot;en-US&quot;&gt;

&lt;head&gt;

&lt;meta charset=&quot;UTF-8&quot;/&gt;

&lt;title&gt;Page Title&lt;/title&gt;

&lt;link rel=&quot;profile&quot; href=&quot;http://gmpg.org/xfn/11&quot;/&gt;

&lt;style type=&quot;text/css&quot;&gt;

body {

font: 62.5%/1.6 Verdana, Tahoma, sans-serif;

color: #333333;

}

h1, h2 {

color: #ff6600;

}

header, main, footer {

margin: 30px auto;

width: 600px;

}

#content {

padding: 8px;

}

.box {

border:1px solid #ccc;

border-radius:4px;

box-shadow:0 0 2px #ccc;

}

&lt;/style&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;header&gt;

&lt;h1&gt;Page Title&lt;/h1&gt;

&lt;p&gt;Page Description&lt;/p&gt;

&lt;/header&gt;

&lt;main id=&quot;content&quot; class=&quot;wrapper box&quot;&gt;

&lt;article&gt;

&lt;h2&gt;Article Title&lt;/h2&gt;

&lt;p&gt;

Lorem ipsum dolor sit amet, consectetuer adipiscin.

Nunc urna metus, ultricies eu, congue vel, laoreet...

&lt;/p&gt;

&lt;/article&gt;

&lt;article&gt;

&lt;h2&gt;Article Title&lt;/h2&gt;

&lt;p&gt;

Morbi malesuada, ante at feugiat tincidunt, enim massa

gravida metus, commodo lacinia massa diam vel eros...

&lt;/p&gt;

&lt;/article&gt;

&lt;/main&gt;

&lt;footer&gt;&amp;copy;copyright 2016&lt;/footer&gt;

&lt;/body&gt;

&lt;/html&gt;

Это пример простого и правильного HTML5 с небольшим добавлением CSS3\. Давайте разберём селекторы в приведённом CSS-коде (я умышленно не выносил CSS в отдельный файл, ибо так наглядней):

body — данные правила будут применены к тегу &lt;body&gt; и всем его потомкам, запомните: настройки шрифтов распространяются вниз «по каскаду»

h1,h2 — мы выбираем теги &lt;h1&gt; и &lt;h2&gt;, и устанавливаем цвет шрифта для данных тегов и их потомков

#content — выбираем элемент с «id=&quot;content&quot;», настройки отступов не распространяются на потомков, они будут изменяться тольки для данного элемента

.box — выбираем элементы с «class=&quot;box&quot;», и изменяем внешний вид границ элементов с заданным классом

Теперь подробнее и с усложнёнными примерами:

| h1 | ищем элементы по имени тега |
| --- | --- |
| #container | ищем элемент по идентификатору «id=container» (**идентификатор уникален**, значит, на странице он должен быть только один) |
| div#container | ищем &lt;div&gt; c идентификатором container, но предыдущий селектор работает быстрее, но этот важнее |
| .news | выбираем элементы по имени класса «class=&quot;news&quot;» |
| div.news | все элементы &lt;div&gt; c классом «news» (так работает быстрее в IE8, т.к. в нём не реализован метод «getElementsByClassName()») |
| #wrap .post | ищем все элементы с классом «post» внутри элемента с «id = wrap» |
| .cls1.cls2 | выбираем элементы с двумя классами «class=&quot;cls1 cls2&quot;» |
| h1,h2,.posts | перечисление селекторов, выберем всё перечисленное |
| .post &gt; h2 | выбираем элементы &lt;h2&gt;, которые являются непосредственными потомками элемента с классом «post» |
| a + span | будут выбраны все элементы &lt;span&gt; следующие сразу за элементом &lt;a&gt; |
| a[href^=http] | будут выбраны все элементы &lt;a&gt; у которых атрибут «href» начинается с http (предположительно, все внешние ссылки) |

_Это отнюдь не весь список, описание же всех CSS3 селекторов можно найти на соответствующей страничке W3C:_ [_http_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_://_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_www_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_._](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_w_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_3._](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_org_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_/_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_TR_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_/_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_css_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_3-_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_selectors_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)[_/_](http://www.google.com/url?q=http%3A%2F%2Fwww.w3.org%2FTR%2Fcss3-selectors%2F&sa=D&sntz=1&usg=AFQjCNGwwWrRQwrxYspj1m-IczSL59wx1w)

_40% задач, которые вы будете решать с помощью jQuery, сводятся к поиску необходимого элемента на странице, так что_ **_знание CSS селекторов обязательно_**_. Вот ещё кусочек CSS для тренировки, напишите соответствующий ему HTML (это тоже вопрос с собеседования ;):_

#my p.announce, .tt.pm li li a:hover+span { color: #f00; }

_Пишите прям тут:_