## Animate {#animate}

_Информация в данном разделе актуальна для jQuery версии 1.8 и выше, если вас заинтересуют возможности расширения для более старых версий, то читайте мою статью «_[_Пишем плагины анимации_](http://anton.shevchuk.name/javascript/jquery-for-beginners-write-animation-plugins/)_»_

Для начала затравка – метод «[.animate()](http://api.jquery.com/animate/)» манипулирует объектом «jQuery.Animation», который предусматривает следующие точки для расширения функционала:

*   jQuery.Tween.propHooks
*   jQuery.Animation.preFilter
*   jQuery.Animation.tweener

Начну рассказ с «jQuery.Tween.propHooks», т.к. уже есть плагины, в код которых можно заглянуть :) Для пущей наглядности я возьму достаточно тривиальную задачу – заставим плавно изменить цвет шрифта для заданного набора элементов:

$(&#039;p&#039;).animate({color:&#039;#ff0000&#039;});

Приведенный выше код не даст никакого эффекта, т.к. свойство «color» библиотека из коробки не анимирует, но это можно исправить – надо лишь прокачать «jQuery.Tween.propHooks»:

$.Tween.propHooks.color = {

get: function(tween) {

return tween.elem.style.color;

}

set: function(tween) {

tween.easing; // текущий easing

tween.elem; // испытуемый элемент

_tween.options;_ // настройки анимация

_tween.pos;_ // текущий прогресс

_tween.prop;_ // свойство которое изменяем

tween.start; // начальные значения

tween.now; // текущее значение

tween.end; // желаемое результирующие значения

tween.unit; // еденицы измерения

}

}

Этот код ещё не работает, это наши кирпичики, с их помощью будем строить нашу анимацию. Перед работой стоит заглянуть внутрь каждого из приведенных свойств:

console.log(tween);

&gt;&gt;&gt;

easing: &quot;swing&quot;

elem: HTMLParagraphElement

end: &quot;#ff0000&quot;

now: &quot;NaNrgb(0, 0, 0)&quot;

options: Object

complete: function (){}

duration: 1000

old: false

queue: &quot;fx&quot;

specialEasing: Object

pos: 1

prop: &quot;color&quot;

start: &quot;rgb(0, 0, 0)&quot;

unit: &quot;px&quot;

В консоле у нас будет очень много данных, т.к. приведённый метод вызывается N кол-во раз, в зависимости от продолжительности анимации, при этом «tween.pos» постепенно наращивает своё значение с 0 до 1\. По умолчанию, наращивание происходит линейно, если надо как-то иначе — то стоит посмотреть на easing плагин или дочитать раздел до конца (об этом я уже упоминал в главе [Анимация](../40_animatsiya/README.md))

Даже при таком раскладе мы уже можем изменять выбранный элемент (путём манипуляций над «tween.elem»), но есть более удобный способ – можно установить свойство «run» объекта «tween»:

$.Tween.propHooks.color = {

set: function(tween) {

// тут будет инициализация

tween.run = function(progress) {

// тут код отвечающий за измение свойств элемента

}

}

}

Получившийся код будет работать следующим образом:

1.  единожды будет вызвана функция «set»
2.  функция «run()» будет вызвана N-раз, при этом «progress» будет себя вести аналогично «tween.pos»

Теперь, возвращаясь к изначальной задачи по изменению цвета можно наворотить следующий код:

$.Tween.propHooks.color = {

set: function(tween) {

// приводим начальное и конечное значения к единому формату

// #FF0000 == [255,0,0]

tween.start = parseColor(tween.start);

tween.end = parseColor(tween.end);

tween.run = function(progress) {

tween.elem.style[&#039;color&#039;] =

// вычисляем промежуточное значение

buildColor(tween.start, tween.end, progress);

}

}

}

_Код функций «parseColor()» и «buildColor()» вы найдёте в листинге на странице_ [_color.html_](http://anton.shevchuk.name/book/code/color.html)

Результатом станет плавное перетекание исходного цвета к красному (#F00 == #FF0000 == 255,0,0), вживую можно посмотреть на странице [color.html](http://anton.shevchuk.name/book/code/color.html)

_В плагине_ [_jQuery Color_](https://github.com/jquery/jquery-color) _для решения поставленной задачи использовали_ [_jQuery.cssHooks_](http://api.jquery.com/jQuery.cssHooks/)_, но мы же не ищем лёгких путей._

Ещё хотел было рассказать про префильтры анимации, но – документации нет, а как использовать «в жизни» – я не догадался, но чуть-чуть информации таки накопал (код можно найти в функции «Animation»):

jQuery.Animation.prefilter(function(element, props, opts) {

// deffered объект animate

element; // искомый элемент

props; // настройки анимации из animate()

opts; // опции анимации

// отключаем анимацию при попытке анимировать высоту элемента

if (props[&#039;height&#039;] != undefined) {

return this;

}

});

_Пример можно пощупать_ [_animate.prefilter.html_](http://anton.shevchuk.name/book/code/animate.prefilter.html)

Про «jQuery.Animation.tweener» так же много не расскажешь, но пример получилось сделать чуток интересней – приведённый код позволяет анимировать ширину и высоту объекта по заданной диагонали:

_Осторожно, для понимания происходящего потребуются знания геометрии за 8-ой класс_

// создаём поддержку нового свойства для анимации – diagonal

jQuery.Animation.tweener( &quot;diagonal&quot;, function( property, value ) {

// создаём tween объект

var tween = this.createTween( property, value );

// промежуточные вычисления и данные

var a = jQuery.css(tween.elem, &#039;width&#039;, true );

var b = jQuery.css(tween.elem, &#039;height&#039;, true );

var c = Math.sqrt(a*a + b*b), sinA = a/c, sinB = b/c;

tween.start = c;

tween.end = value;

tween.run = function(progress) {

// вычисление искомого значения – новое значение гипотенузы

var hyp = this.start + ((this.end - this.start) * progress);

// непосредственно измение свойств элемента

tween.elem.style.width = sinA*hyp + tween.unit; // ширина

tween.elem.style.height = sinB*hyp + tween.unit; // высота

};

return tween;

});

_Пример работы_ [_animate.tweener.html_](http://anton.shevchuk.name/book/code/animate.tweener.html)