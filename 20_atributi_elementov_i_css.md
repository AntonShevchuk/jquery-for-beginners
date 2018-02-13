# 20% Атрибуты элементов и CSS {#20-css}

В предыдущих примерах мы уже изменяли CSS-свойства DOM-элементов, используя одноимённый метод «.css()», но это далеко не всё. Теперь копнём поглубже, чтобы не штурмовать форумы банальными вопросами ;)

Копать начнём с более досконального изучения метода «.css()»:

css(_property_) — получение значения CSS-свойства

css(_property_, _value_) — установка значения CSS-свойства

css({_key_: _value_, _key_:_value_}) — установка нескольких значений

css(_property_, function(_index_, _value_) { return _value_ }) — тут для установки значения используется функция обратного вызова (в просторечии — callback-функция), index это порядковый номер элемента в выборке, value — старое значение свойства

_Метод «.css()» возвращает текущее значение, а не прописанное в CSS-файле по указанному селектору_

Примеры использования ([css.html](http://anton.shevchuk.name/book/code/css.html)):

$(&quot;#my&quot;).css(&#039;color&#039;) // получаем значение цвета шрифта

$(&quot;#my&quot;).css(&#039;color&#039;, &#039;red&#039;) // устанавливаем значение цвета шрифта

// установка нескольких значений

$(&quot;#my&quot;).css({

&#039;color&#039;:&#039;red&#039;,

&#039;font-size&#039;:&#039;14px&#039;,

&#039;margin-left&#039;:&#039;10px&#039;

})

// альтернативный способ

$(&quot;#my&quot;).css({

color:&#039;red&#039;,

fontSize:&#039;14px&#039;,

marginLeft:&#039;10px&#039;,

})

// используя функцию обратного вызова

$(&quot;#my&quot;).css(&#039;height&#039;, function(i, value){

return parseFloat(value) * 1.2;

})

Ну, вроде с CSS разобрались, хотя нет — стоит ещё описать манипуляции с классами, тоже из разряда первичных навыков:

addClass(_className_) — добавление класса элементу

addClass(function(_index_, _currentClass_){ return _className_ }) — добавление класса с помощью функции обратного вызова

hasClass(_className_) — проверка на причастность к определённому классу

removeClass(_className_) — удаление класса

removeClass(function(_index_, _currentClass_){ return _className_ }) — удаление класса с помощью функции обратного вызова

toggleClass(_className_) — переключение класса

toggleClass(_className, switch_) — переключение класса по флагу switch

toggleClass(function(_index_, _currentClass, switch_){ return _className_ }, _switch_) — переключение класса с помощью функции обратного вызова

_В приведённых методах в качестве «className» может быть строка содержащая список классов через пробел._

Мне ни разу не приходилось использовать данные методы с функциями обратного вызова, и лишь единожды пригодился флаг «switch», так что не заморачивайтесь всё это запоминать, да и в дальнейшем, цитируя руководство по jQuery, я буду сознательно опускать некоторые «возможности»

Но хватит заниматься переводом официальной документации, перейдём к наглядным примерам ([class.html](http://anton.shevchuk.name/book/code/class.html)):

// добавляем несколько классов за раз

$(&quot;#my&quot;).addClass(&#039;active notice&#039;)

// переключаем несколько классов

$(&quot;#my&quot;).toggleClass(&#039;active notice&#039;)

// работает вот так (похоже на классовый XOR):

&lt;div id=&quot;my&quot; class=&quot;active notice&quot;&gt; → &lt;div id=&quot;my&quot; class=&quot;&quot;&gt;

&lt;div id=&quot;my&quot; class=&quot;active&quot;&gt; → &lt;div id=&quot;my&quot; class=&quot;notice&quot;&gt;

&lt;div id=&quot;my&quot; class=&quot;&quot;&gt; → &lt;div id=&quot;my&quot; class=&quot;active notice&quot;&gt;

// аналогично предыдущему примеру

$(&quot;#my&quot;).toggleClass(&#039;active&#039;)

$(&quot;#my&quot;).toggleClass(&#039;notice&#039;)

// проверяем наличие класса(-ов)

$(&quot;#my&quot;).hasClass(&#039;active&#039;)

// удаляем несколько классов за раз

$(&quot;#my&quot;).removeClass(&#039;active notice&#039;)

Также, стоит вспомнить, что у DOM-элементов бывают атрибуты отличные от класса, и мы их тоже можем изменять. Для этого нам потребуются следующие методы:

attr(_attrName_) — получение значения атрибута

attr(_attrName, attrValue_) — установка значения атрибута (также можно использовать hash либо функцию обратного вызова)

removeAttr(_attrName_) — удаление атрибута

Атрибуты – это всё то, что мы видим внутри угловых скобочек, когда пишем HTML-код:

&lt;!-- В данном примере это href, title, class --&gt;

&lt;a href=&quot;#top&quot; title=&quot;anchor&quot; class=&quot;simple&quot;&gt;To Top&lt;/a&gt;

Атрибуты, с которыми вам чаще других придётся сталкиваться:

// _получение_ альтернативного текста картинки

var altText = $(&#039;img&#039;).attr(&#039;alt&#039;)

// изменение адреса картинки

$(&#039;img&#039;).attr(&#039;src&#039;, &#039;/images/default.png&#039;)

// работаем со ссылками

$(&#039;a#my&#039;).attr({

&#039;href&#039;:&#039;http://anton.shevchuk.name&#039;,

&#039;title&#039;:&#039;My Personal Blog&#039;,

});

Кроме атрибутов также есть свойства элементов, к ним относятся «selectedIndex», «tagName», «nodeName», «nodeType», «ownerDocument», «defaultChecked» и «defaultSelected». Ну, вроде бы, список невелик, можно и запомнить. Для работы со свойствами используем методы из семейства «.prop()»:

prop(_propName_) — получение значения свойства

prop(_propName, propValue_) — установка значения свойства (также можно использовать hash либо функцию обратного вызова)

removeProp(_propName_) — удаление свойства (скорей всего, никогда не понадобится)

А теперь выключите музыку, и запомните следующее – **для отключения элементов формы, и для проверки/изменения состояния чекбоксов мы всегда используем метод «.prop()»,** пусть вас не смущает наличие одноименных атрибутов в HTML (это я про «disabled» и «checked»), используем «.prop()» и точка (наглядный пример [property.html](http://anton.shevchuk.name/book/code/property.html))