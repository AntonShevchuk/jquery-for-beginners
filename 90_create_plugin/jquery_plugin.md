# jQuery плагин

Для начала давайте вспомним, для чего нам нужны плагины? Мой ответ — для создания повторно используемого кода, причём с удобным интерфейсом. Давайте напишем такой код, начнём с простой задачки:

_— По клику на параграф текст должен измениться на красный_

## JavaScript и даже не jQuery <a href="#javascript-jquery" id="javascript-jquery"></a>

Дабы не забывать истоки, начнём с реализации на нативном JavaScript:

```javascript
var loader = function () {
    // находим все параграфы
    var para = document.getElementsByTagName('P');
    // перебираем всё и вешаем обработчик
    for (var i=0,size=para.length;i<size;i++) {
        // обработчик
        para[i].onclick = function() {
            this.style.color = "#FF0000";
        }
    }
}

// естественно, весь код должен работать после загрузки всей страницы
document.addEventListener("DOMContentLoaded", loader, false);
```

Данный код не является кроссбраузерным и написан с целью лишний раз подчеркнуть удобство использования jQuery ;)

## jQuery, но ещё не плагин <a href="#jquery" id="jquery"></a>

Теперь можно этот код упростить, подключаем jQuery и получаем следующий вариант:

```javascript
$(function(){
    $('p').click(function(){
        $(this).css('color', '#ff0000');
    })
});
```

## Таки jQuery плагин <a href="#jquery-plugin" id="jquery-plugin"></a>

С поставленной задачей мы справились, но где тут повторное использование кода? Или если нам надо не в красный, а в зеленый перекрасить? Вот тут начинается самое интересное. Чтобы написать простой плагин, достаточно расширить объект `$.fn`:

```javascript
$.fn.mySimplePlugin = function() {
    $(this).click(function(){
        $(this).css('color', '#ff0000');
    })
}
```

Если же писать более грамотно, то нам необходимо ограничить переменную `$` только нашим плагином, а также возвращать `this`, чтобы можно было использовать цепочки вызовов (т.н. «chaining»). Делается это следующим образом:

```javascript
(function($) {
    $.fn.mySimplePlugin = function() {
        // код плагина
        return this;
    };
})(jQuery);
```

>

{% hint style="info" %}
Внесу небольшое пояснение о происходящей тут «магии». Код `(function($){…})(jQuery)` создает анонимную функцию и тут же вызывает её, передавая в качестве параметра объект `jQuery`. Таким образом внутри анонимной функции мы можем использовать алиас `$`, не боясь за конфликты с другими библиотеками, так как теперь `$` находится лишь в области видимости нашей функции и мы имеем полный контроль над ней. Если у вас возникло ощущение дежавю – то всё верно, я об этом уже рассказывал.
{% endhint %}

Добавим опцию по выбору цвета и получим рабочий плагин (см. [plugin.global.html](https://anton.shevchuk.name/book/code/plugin.global.html)):

```javascript
(function($) {
    // значение по умолчанию - ЗЕЛЁНЫЙ
    var defaults = { color:'green' };
    // актуальные настройки, глобальные
    var options;
    $.fn.mySimplePlugin = function(params) {
        // при многократном вызове настройки будут сохраняться и замещаться при необходимости
        options = $.extend({}, defaults, options, params);
        $(this).click(function() {
            $(this).css('color', options.color);
        });
        return this;
    };
})(jQuery);
```

Вызов:

```javascript
// первый вызов
$('p:first,p:last').mySimplePlugin();

// второй вызов
$('p:eq(1)').mySimplePlugin({ color: 'red' });
```

В результате работы данного плагина каждый клик будет изменять цвет параграфа на красный. Та как мы используем глобальную переменную для хранения настроек, то второй вызов плагина изменит значение для всех элементов.

Можно внести небольшие изменения и разделить настройки для каждого вызова (см. [plugin.html](https://anton.shevchuk.name/book/code/plugin.html)):

```javascript
// актуальные настройки будут индивидуальными при каждом запуске
var options = $.extend({}, defaults, params);
```

А разница-то в одном «var». Мне даже сложно себе представить, как много часов убито в поисках потерянного «var» в JavaScript. Будьте внимательны.

## Работаем с коллекциями объектов <a href="#collections" id="collections"></a>

Тут всё просто, достаточно запомнить — `this` содержит jQuery-объект с коллекцией всех элементов, т.е.:

```javascript
$.fn.mySimplePlugin = function(){
    console.log(this); // это jQuery объект
    console.log(this.length); // число элементов в выборке
};
```

Если мы хотим обрабатывать каждый элемент, то соорудим следующую конструкцию внутри нашего плагина:

```javascript
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
```

> Опять же напомню, если ваш плагин не должен что-то возвращать по вашей задумке — возвращайте «this». Цепочки вызовов в jQuery это часть магии, не стоит её разрушать. Методы «.each()» и «.click()» возвращают объект jQuery.

{% hint style="warning" %}
Опять же напомню, если ваш плагин не должен что-то возвращать по вашей задумке — возвращайте `this`. Цепочки вызовов в jQuery это часть магии, не стоит её разрушать. Методы `each()` и `click()` возвращают объект jQuery.
{% endhint %}

## Публичные методы <a href="#public-methods" id="public-methods"></a>

Так, у нас написан крутой плагин, надо бы ему ещё докрутить функционала – пусть цвет регулируется несколькими кнопками на сайте. Для этого нам понадобится некий метод `color`, который и будет в ответе за всё. Сейчас приведу пример кода готового плагина — будем курить вместе (обращайте внимание на комментарии):

```javascript
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
```

Теперь ещё небольшой пример использования данных методов:

```javascript
// вызов без параметров - будет вызван init
$('p').mySimplePlugin();

// вызов метода color и передача цвета в качестве параметров
$('p').mySimplePlugin('color', '#FFFF00');

// вызов метода reset
$('p').mySimplePlugin('reset');
```

Для понимания данного кусочка кода вы должны разобраться лишь с переменной `arguments` и методом `apply()`. Тут им целые статьи посвятили, дерзайте:

* [https://learn.javascript.ru/arguments-pseudoarray](https://learn.javascript.ru/arguments-pseudoarray)
* [https://learn.javascript.ru/call-apply](https://learn.javascript.ru/call-apply)

## Об обработчиках событий <a href="#event-handlers" id="event-handlers"></a>

Если ваш плагин вешает какой-либо обработчик, то лучше всего (читай – всегда) данный обработчик повесить в своём собственном namespace:

```javascript
return this.on("click.mySimplePlugin", function(){
    $(this).css('color', options.color);
});
```

Данный финт позволит в любой момент убрать все ваши обработчики или вызвать только ваш, что очень удобно:

```javascript
// вызовем лишь наш обработчик
$('p').trigger("click.mySimplePlugin");

// убираем все наши обработчики
$('p').off(".mySimplePlugin");
```

> — Дежавю? Ок!

На этом об обычных плагинах всё. Дам ещё чуток информации к размышлению, но на английском:

* [Essential jQuery Plugin Patterns](https://www.smashingmagazine.com/2011/10/essential-jquery-plugin-patterns/)
