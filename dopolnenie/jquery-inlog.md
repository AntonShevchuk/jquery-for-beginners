## jQuery-inlog {#jquery-inlog}

Ещё чуть-чуть о полезном инструментарии: есть такой классный плагин – jQuery-inlog [[http://prinzhorn.github.com/jquery-inlog/](http://prinzhorn.github.com/jquery-inlog/)] – основное его назначение — дать нам чуть-чуть больше понимания о происходящем внутри самого jQuery, вот кусочек HTML:

&lt;body&gt;

&lt;div class=&quot;bar&quot;&gt;

&lt;div class=&quot;bar&quot;&gt;

&lt;div id=&quot;foo&quot;&gt;&lt;/div&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;div id=&quot;bacon&quot;&gt;&lt;/div&gt;

&lt;/body&gt;

А вот и код, который его обслуживает:

$l(true);

$(&quot;#foo&quot;).parents(&quot;.bar&quot;).next().prev().parent().fadeOut();

$l(false);

Какие-то странные манипуляции, для какого же элемента будет применён метод «.fadeOut()»? Для выяснения оного наш код обёрнут в вызов метода «$l()». «$l()» — это и есть собственно вызов плагина, результат его работы можно найти в консоли:

У данного плагина есть ещё настройки, которые регулируют объём информации выводимой в консоль.

_Пример и скриншот взять с_ [_официальной документации_](http://prinzhorn.github.com/jquery-inlog/) _по плагину_