# Animate

> Информация в данном разделе актуальна для jQuery версии 1.8 и выше. Если вас заинтересуют возможности расширения для более старых версий, читайте мою статью «[Пишем плагины анимации](https://anton.shevchuk.name/javascript/jquery-for-beginners-write-animation-plugins/)».

Для начала затравка – метод [`animate()`](https://api.jquery.com/animate/) манипулирует объектом `jQuery.Animation`, который предусматривает следующие точки для расширения функционала:

* `jQuery.Tween.propHooks`
* `jQuery.Animation.preFilter`
* `jQuery.Animation.tweener`

Начну рассказ с `jQuery.Tween.propHooks`, т.к. уже есть плагины, в код которых можно заглянуть :) Для пущей наглядности я возьму достаточно тривиальную задачу – заставим плавно изменить цвет шрифта для заданного набора элементов:

```javascript
$('p').animate({color:'#ff0000'});
```

Приведенный выше код не даст никакого эффекта, т.к. «из коробки» библиотека свойство `color` не анимирует, но это можно исправить – надо лишь прокачать `jQuery.Tween.propHooks`:

```javascript
$.Tween.propHooks.color = {
    get: function(tween) {
        return tween.elem.style.color;
    },
    set: function(tween) {
        tween.easing;  // текущий easing
        tween.elem;    // испытуемый элемент
        tween.options; // настройки анимации
        tween.pos;     // текущий прогресс
        tween.prop;    // свойство, которое изменяем
        tween.start;   // начальные значения
        tween.now;     // текущее значение
        tween.end;     // желаемые результирующие значения
        tween.unit;    // единицы измерения
    }
};
```

Этот код ещё не работает, это наши кирпичики, с их помощью будем строить нашу анимацию. Перед работой стоит заглянуть внутрь каждого из приведённых свойств:

```javascript
console.log(tween);
```

```
>>>
easing: "swing"
elem: HTMLParagraphElement
end: "#ff0000"
now: "NaNrgb(0, 0, 0)"
options: Object
complete: function (){}
duration: 1000
old: false
queue: "fx"
specialEasing: Object
pos: 1
prop: "color"
start: "rgb(0, 0, 0)"
unit: "px"
```

В консоли у нас будет очень много данных, т.к. приведённый метод вызывается N раз в зависимости от продолжительности анимации, при этом «tween.pos» постепенно наращивает своё значение с 0 до 1. По умолчанию наращивание происходит линейно, если надо как-то иначе, то стоит посмотреть на easing плагин или дочитать раздел до конца (об этом я уже упоминал в главе [Анимация](../40\_animation/)).

Даже при таком раскладе мы уже можем изменять выбранный элемент (путём манипуляций над `tween.elem`), но есть более удобный способ – можно установить свойство «run» объекта `tween`:

```javascript
$.Tween.propHooks.color = {
    set: function(tween) {
        // тут будет инициализация
        tween.run = function(progress) {
            // тут код, отвечающий за измение свойств элемента
        }
    }
};
```

Получившийся код будет работать следующим образом:

1. единожды будет вызвана функция `set()`
2. функция `run()` будет вызвана N раз, при этом `progress` будет себя вести аналогично `tween.pos`

Теперь, возвращаясь к изначальной задаче по изменению цвета, можно наворотить следующий код:

```javascript
$.Tween.propHooks.color = {
    set: function(tween) {
        // приводим начальное и конечное значения к единому формату
        // #FF0000 == [255,0,0]
        tween.start = parseColor(tween.start);
        tween.end = parseColor(tween.end);
        tween.run = function(progress) {
            // вычисляем промежуточное значение
            tween.elem.style['color'] = buildColor(tween.start, tween.end, progress);
        }
    }
};
```

> Код функций `parseColor()` и `buildColor()` вы найдёте в листинге на странице [color.html](https://anton.shevchuk.name/book/code/color.html).

Результатом станет плавное перетекание исходного цвета к красному (#F00 == #FF0000 == 255,0,0), вживую можно посмотреть на странице:

{% embed url="https://anton.shevchuk.name/book/code/color.html" %}

{% hint style="info" %}
В плагине [jQuery Color](https://github.com/jquery/jquery-color) для решения поставленной задачи использовали [jQuery.cssHooks](http://api.jquery.com/jQuery.cssHooks/), но мы же не ищем лёгких путей.
{% endhint %}

Ещё хотел было рассказать про префильтры анимации, но документации нет, а как использовать «в жизни», я не догадался, но чуть-чуть информации-таки накопал (код можно найти в функции `Animation`):

```javascript
jQuery.Animation.prefilter(function(element, props, opts) {
    // deffered объект animate
    element; // искомый элемент
    props;   // настройки анимации из animate()
    opts;    // опции анимации
    // отключаем анимацию при попытке анимировать высоту элемента
    if (props['height'] != undefined) {
        return this;
    }
});
```

> Пример можно пощупать [animate.prefilter.html](http://anton.shevchuk.name/book/code/animate.prefilter.html).

Про `jQuery.Animation.tweener` также много не расскажешь, но пример получилось сделать чуток интересней – приведённый код позволяет анимировать ширину и высоту объекта по заданной диагонали:

> Осторожно, для понимания происходящего потребуются знания геометрии за 8-й класс.

```javascript
// создаём поддержку нового свойства для анимации – diagonal
jQuery.Animation.tweener( "diagonal", function( property, value ) {
    // создаём tween объект
    var tween = this.createTween( property, value );
    // промежуточные вычисления и данные
    var a = jQuery.css(tween.elem, 'width', true );
    var b = jQuery.css(tween.elem, 'height', true );
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
```

Пример работы:

{% embed url="https://anton.shevchuk.name/book/code/animate.tweener.html" %}
