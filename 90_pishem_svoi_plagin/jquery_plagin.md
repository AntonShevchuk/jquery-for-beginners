## jQuery плагин {#jquery}

Для начала вспомним, для чего нам нужны плагины? Мой ответ — создание повторно используемого кода, и да — с удобным интерфейсом. Давайте напишем такой код, вот простая задачка: «По клику на параграф, текст должен измениться на красный»

### JavaScript и даже не jQuery {#javascript-jquery}

Дабы не забывать истоков — начнем с реализации на нативном JavaScript'е:

var loader = function () {

// находим все параграфы

var para = document.getElementsByTagName('P');

// перебираем все, и вешаем обработчик

for (var i=0,size=para.length;i<size;i++) {

// обработчик

para[i].onclick = function() {

this.style.color = "#FF0000";

}

}

}

// естественно, весь код должен работать после загрузки всей страницы

document.addEventListener("DOMContentLoaded", loader, false);

Данный код не является кроссбраузерным, и написан с целью лишний раз подчеркнуть удобство использования библиотеки ;)

### jQuery, но ещё не плагин {#jquery-0}

Теперь можно этот код упростить, подключаем jQuery и получаем следующий вариант:

$(function(){

$('p').click(function(){

$(this).css('color', '#ff0000');

})

});

### Таки jQuery плагин {#jquery-1}

С поставленной задачей мы справились, но где тут повторное использование кода? Или если нам надо не в красный, а в зеленый перекрасить? Вот тут начинается самое интересное, чтобы написать простой плагин достаточно расширить объект «$.fn»:

$.fn.mySimplePlugin = function() {

$(this).click(function(){

$(this).css('color', '#ff0000');

})

}

Если же писать более грамотно, то нам необходимо ограничить переменную $ только нашим плагином, а так же возвращать «this», чтобы можно было использовать цепочки вызовов (т.н. «chaining»), делается это следующим образом:

(function($) {

$.fn.mySimplePlugin = function(){

// код плагина

return this;

};

})(jQuery);

Внесу небольшое пояснение о происходящей тут «магии», код «(function($){…})(jQuery)» создает анонимную функцию, и тут же вызывает ее, передавая в качестве параметра объект jQuery, таким образом внутри анонимной функции мы можем использовать алиас $ не боясь за конфликты с другими библиотеками — так как теперь $ находится лишь в области видимости нашей функции, и мы имеем полный контроль над ней. Если у вас возникло ощущение дежавю – то всё верно, я об этом уже рассказывал

Добавим опцию по выбору цвета и получим рабочий плагин (см. [plugin.global.html](http://anton.shevchuk.name/book/code/plugin.global.html)):

(function($) {

// значение по умолчанию - ЗЕЛЁНЫЙ

var defaults = { color:'green' };

// актуальные настройки, глобальные

var options;

$.fn.mySimplePlugin = function(params){

// при многократном вызове настройки будут сохранятся

// и замещаться при необходимости

options = $.extend({}, defaults, options, params);

$(this).click(function(){

$(this).css('color', options.color);

});

return this;

};

})(jQuery);

Вызов:

// первый вызов

$('p:first,p:last').mySimplePlugin();

// второй вызов

$('p:eq(1)').mySimplePlugin({ color: 'red' });

В результате работы данного плагина, каждый клик будет изменять цвет параграфа на красный, т.к. мы используем глобальную переменную для хранения настроек, то второй вызов плагина изменят значение для всех элементов.

Можно внести небольшие изменения, и разделить настройки для каждого вызова (см. [plugin.html](http://anton.shevchuk.name/book/code/plugin.html)):

// актуальные настройки, будут индивидуальными при каждом запуске

var options = $.extend({}, defaults, params);

А разница то в одном «var». Мне даже сложно себе представить как много часов убито в поисках потерянного «var» в JavaScript'е, будьте внимательны

### Работаем с коллекциями объектов

Тут все просто, достаточно запомнить — «this» содержит jQuery объект с коллекцией всех элементов, т.е.:

$.fn.mySimplePlugin = function(){

console.log(this); // это jQuery объект

console.log(this.length); // число элементов в выборке

};

Если мы хотим обрабатывать каждый элемент то соорудим следующую конструкцию внутри нашего плагина:

// необходимо обработать каждый элемент в коллекции

return this.each(function(){

$(this).click(function(){

$(this).css('color', options.color);

});

});

// предыдущий вариант немного избыточен,

// т.к. внутри функции click и так есть перебор элементов

return this.click(function(){

$(this).css('color', options.color);

});

Опять же напомню, если ваш плагин не должен что-то возвращать по вашей задумке — возвращайте «this» — цепочки вызовов в jQuery это часть магии, не стоит её разрушать. Методы «.each()» и «.click()» возвращают объект jQuery.

### Публичные методы {#-0}

Так, у нас написан крутой плагин, надо бы ему ещё докрутить функционала, пусть цвет регулируется несколькими кнопками на сайте. Для этого нам понадобится некий метод «color», который и будет в ответе за всё. Сейчас приведу пример кода готового плагина — будем курить вместе (обращайте внимание на комментарии):

// настройки со значением по умолчанию

var defaults = { color:'green' };

// наши будущие публичные методы

var methods = {

// инициализация плагина

init: function(params) {

// настройки, будут индивидуальными при каждом запуске

var options = $.extend({}, defaults, params);

// инициализируем лишь единожды

if (!this.data('mySimplePlugin')) {

// закинем настройки в реестр data

this.data('mySimplePlugin', options);

// добавим событий

this.on('click.mySimplePlugin', function(){

$(this).css('color', options.color);

});

}

return this;

},

// изменяем цвет в реестре

color: function(color) {

var options = $(this).data('mySimplePlugin');

options.color = color;

$(this).data('mySimplePlugin', options);

},

// сброс цвета элементов

reset: function() {

$(this).css('color', 'black');

}

};

$.fn.mySimplePlugin = function(method){

// немного магии

if ( methods[method] ) {

// если запрашиваемый метод существует, мы его вызываем

// все параметры, кроме имени метода прийдут в метод

// this так же перекочует в метод

return methods[ method ].apply( this, Array.prototype.slice.call( arguments, 1 ));

} else if ( typeof method === 'object' || ! method ) {

// если первым параметром идет объект, либо совсем пусто

// выполняем метод init

return methods.init.apply( this, arguments );

} else {

// если ничего не получилось

$.error('Метод "' + method + '" в плагине не найден');

}

};

Теперь ещё небольшой пример использование данных методов:

// вызов без параметров - будет вызван init

$('p').mySimplePlugin();

// вызов метода color и передача цвета в качестве параметров

$('p').mySimplePlugin('color', '#FFFF00');

// вызов метода reset

$('p').mySimplePlugin('reset');

Для понимания данного кусочка кода, вы должны разобраться лишь с переменной «arguments», и с методом «apply()». Тут им целые статьи посвятили, дерзайте:

*   [_http://www.seifi.org/?p=673_](http://www.seifi.org/?p=673)
*   [_https://learn.javascript.ru/arguments-pseudoarray_](https://learn.javascript.ru/arguments-pseudoarray)
*   [_https://learn.javascript.ru/call-apply_](https://learn.javascript.ru/call-apply)

### Об обработчиках событий {#-1}

Если ваш плагин вешает какой-либо обработчик, то лучше всего (читай всегда) данный обработчик повесить в своём собственном namespace:

return this.on("click.mySimplePlugin", function(){

$(this).css('color', options.color);

});

Данный финт позволит в любой момент убрать все ваши обработчики, или вызвать только ваш, что очень удобно:

// вызовем лишь наш обработчик

$('p').trigger("click.mySimplePlugin");

// убираем все наши обработчики

$('p').off(".mySimplePlugin");

Дежавю? Ок!

На этом об обычных плагинах всё, хотя дам ещё чуток информации к размышлению, но на английском:

*   «Essential jQuery Plugin Patterns»

[[http://coding.smashingmagazine.com/?p=115389](http://coding.smashingmagazine.com/?p=115389)]