## Easing {#easing}

Теперь опять обратимся к easing’у – приведу пример произвольной функции, которой будет следовать анимация. Дабы особо не фантазировать – я взял пример из статьи на вездесущем хабре o анимации в MooTools фреймворке [[http://habrahabr.ru/post/43379/](http://habrahabr.ru/post/43379/)] – наглядный пример с сердцебиением, которое описывается следующими функциями:

В расширении функционала easing нет ничего военного:

$.extend($.easing, {

/**

* Heart Beat

*

* @param x progress

* @link http://habrahabr.ru/blogs/mootools/43379/

*/

heart:function(x) {

if (x &lt; 0.3) return Math.pow(x, 4) * 49.4;

if (x &lt; 0.4) return 9 * x - 2.3;

if (x &lt; 0.5) return -13 * x + 6.5;

if (x &lt; 0.6) return 4 * x - 2;

if (x &lt; 0.7) return 0.4;

if (x &lt; 0.75) return 4 * x - 2.4;

if (x &lt; 0.8) return -4 * x + 3.6;

if (x &gt;= 0.8) return 1 - Math.sin(Math.acos(x));

}

});

Чуть-чуть пояснений, конструкция «$.extend({}, {})» «смешивает» объекты:

$.extend({name:&quot;Anton&quot;}, {location:&quot;Kharkiv&quot;});

&gt;&gt;&gt;

{

name:&quot;Anton&quot;,

location:&quot;Kharkiv&quot;

}

$.extend({name:&quot;Anton&quot;, location:&quot;Kharkiv&quot;}, {location:&quot;Kyiv&quot;});

&gt;&gt;&gt;

{

name:&quot;Anton&quot;,

location:&quot;Kyiv&quot;

}

Таким образом мы «вмешиваем» новый метод к существующему объекту «$.easing»; согласно коду, наш метод принимает в качестве параметра лишь одно значение:

x – коэффициент прохождения анимации, изменяется от 0 до 1, дробное

Результат конечно интересен, но его можно ещё чуть-чуть расширить дополнительными функциями (развернем и скомбинируем):

heartIn: function (x) {

return $.easing.heart(x);

},

heartOut: function (x) {

return $.easing.heart(1 - x);

},

heartInOut: function (x) {

if (x &lt; 0.5) return $.easing.heartIn(x);

return $.easing.heartOut(x);

}

Получим следующие производные функции:

| **heartIn** | **heartOut** | **heartInOut** |
| --- | --- | --- |

Работать с данным творением надо следующим образом:

$(&quot;#my&quot;).animate({height:&quot;+200px&quot;}, 2000, &quot;heartIn&quot;); // вот оно

Пример работы данной функции можно пощупать на странице [easing.html](http://anton.shevchuk.name/book/code/easing.html)