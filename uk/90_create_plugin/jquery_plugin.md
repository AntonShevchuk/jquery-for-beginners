---
description: Грудкою...
---

# Перший плагін

Для початку давай згадаємо, для чого нам потрібні плагіни? Моя відповідь — для створення повторно використовуваного коду, причому зі зручним інтерфейсом. Давай напишемо такий код, почнемо з простого завдання:

_— По кліку на параграф текст має змінитися на червоний_

## JavaScript і навіть не jQuery <a href="#javascript-jquery" id="javascript-jquery"></a>

Щоб не забувати витоки, почнемо з реалізації на нативному JavaScript:

```javascript
// загорнемо все у функцію
let loader = function () {
  // знаходимо всі параграфи
  let paragraphs = document.getElementsByTagName('P');
  // перебираємо все і навішуємо обробник
  for (let i = 0; i < paragraphs.length; i++) {
    // власне обробник
    paragraphs[i].onclick = function() {
      this.style.color = "#FF0000";
    }
  }
}

// звісно, весь код має працювати після завантаження всієї сторінки
document.addEventListener("DOMContentLoaded", loader);
```

Цей код не є універсальним і написаний з метою зайвий раз підкреслити зручність використання jQuery ;)

## jQuery, але ще не плагін <a href="#jquery" id="jquery"></a>

Тепер можна цей код спростити, підключаємо jQuery й отримуємо такий варіант:

```javascript
$(function(){
    $('p').click(function(){
        $(this).css('color', '#ff0000');
    })
});
```

## Таки jQuery-плагін <a href="#jquery-plugin" id="jquery-plugin"></a>

З поставленим завданням ми впоралися, але де тут повторне використання коду? Або якщо нам треба не в червоний, а в зелений перефарбувати? Ось тут починається найцікавіше. Щоб написати простий плагін, достатньо розширити об'єкт `$.fn`:

```javascript
$.fn.mySimplePlugin = function() {
    $(this).click(function(){
        $(this).css('color', '#ff0000');
    })
}
```

Якщо ж писати більш грамотно, то нам необхідно обмежити змінну `$` тільки нашим плагіном, а також повертати `this`, щоб можна було використовувати ланцюжки викликів (т.зв. «chaining»). Робиться це наступним чином:

```javascript
(function($) {
    $.fn.mySimplePlugin = function() {
        // код плагіна
        return this;
    };
})(jQuery);
```

{% hint style="info" %}
Внесу невелике пояснення про «магію», що тут відбувається. Код `(function($){…})(jQuery)` створює анонімну функцію і тут же викликає її, передаючи як параметр об'єкт `jQuery`. Таким чином всередині анонімної функції ми можемо використовувати аліас `$`, не боячись конфліктів з іншими бібліотеками, оскільки тепер `$` знаходиться лише в області видимості нашої функції і ми маємо повний контроль над нею. Якщо в тебе виникло відчуття дежавю – то все правильно, я про це вже розповідав.
{% endhint %}

Додамо опцію вибору кольору й отримаємо робочий плагін:

```javascript
(function($) {
    // значення за замовчуванням - ЗЕЛЕНИЙ
    var defaults = { color:'green' };
    // актуальні налаштування, глобальні
    var options;
    $.fn.mySimplePlugin = function(params) {
        // при багаторазовому виклику налаштування зберігатимуться й заміщуватимуться за необхідності
        options = $.extend({}, defaults, options, params);
        $(this).click(function() {
            $(this).css('color', options.color);
        });
        return this;
    };
})(jQuery);
```

Виклик:

```javascript
// перший виклик
$('p:first').mySimplePlugin();

// другий виклик
$('p:eq(1)').mySimplePlugin({ color: 'red' });

// третій виклик
$('p:last').mySimplePlugin();
```

В результаті роботи цього плагіна кожен клік змінюватиме колір параграфа на червоний. Оскільки ми використовуємо глобальну змінну для зберігання налаштувань, другий виклик плагіна змінить значення для всіх елементів:

{% embed url="https://anton.shevchuk.name/book/code/plugin.global.html" %}

Можна внести невеликі зміни і розділити налаштування для кожного виклику:

```javascript
// актуальні налаштування будуть індивідуальними при кожному запуску
var options = $.extend({}, defaults, params);
```

А різниця-то в одному «var». Мені навіть складно уявити, скільки годин вбито в пошуках загубленого «var» у JavaScript. Будь уважний:

{% embed url="https://anton.shevchuk.name/book/code/plugin.html" %}

## Працюємо з колекціями об'єктів <a href="#collections" id="collections"></a>

Тут все просто, достатньо запам'ятати — `this` містить jQuery-об'єкт з колекцією всіх елементів, тобто:

```javascript
$.fn.mySimplePlugin = function(){
    console.log(this); // це jQuery об'єкт
    console.log(this.length); // кількість елементів у вибірці
};
```

Якщо ми хочемо обробляти кожен елемент, то спорудимо таку конструкцію всередині нашого плагіна:

```javascript
// необхідно обробити кожен елемент у колекції
return this.each(function(){
    $(this).click(function(){
        $(this).css('color', options.color);
    });
});

// попередній варіант трохи надлишковий,
// оскільки всередині функції click і так є перебір елементів
return this.click(function(){
    $(this).css('color', options.color);
});
```

{% hint style="warning" %}
Знову ж таки нагадаю, якщо твій плагін не повинен щось повертати за твоїм задумом — повертай `this`. Ланцюжки викликів у jQuery це частина магії, не варто її руйнувати. Методи `each()` та `click()` повертають об'єкт jQuery.
{% endhint %}

## Публічні методи <a href="#public-methods" id="public-methods"></a>

Так, у нас написаний «крутий» плагін, треба б йому ще докрутити функціоналу – нехай колір регулюється кількома кнопками на сайті. Для цього нам знадобиться деякий метод `color`, який і буде відповідати за все. Зараз наведу приклад коду готового плагіна — будемо курити разом (звертай увагу на коментарі):

```javascript
// налаштування зі значенням за замовчуванням
var defaults = { color:'green' };

// наші майбутні публічні методи
var methods = {
    // ініціалізація плагіна
    init: function(params) {
        // налаштування, будуть індивідуальними при кожному запуску
        var options = $.extend({}, defaults, params);
        // ініціалізуємо лише одного разу
        if (!this.data('mySimplePlugin')) {
            // закинемо налаштування в реєстр data
            this.data('mySimplePlugin', options);
            // додамо подій
            this.on('click.mySimplePlugin', function(){
                $(this).css('color', options.color);
            });
        }
        return this;
    },
    // змінюємо колір у реєстрі
    color: function(color) {
        var options = $(this).data('mySimplePlugin');
        options.color = color;
        $(this).data('mySimplePlugin', options);
    },
    // скидання кольору елементів
    reset: function() {
        $(this).css('color', 'black');
    }
};

$.fn.mySimplePlugin = function(method){
    // трохи магії
    if ( methods[method] ) {
        // якщо запитуваний метод існує, ми його викликаємо
        // всі параметри, крім імені методу прийдуть у метод
        // this так само перекочує в метод
        return methods[ method ].apply( this, Array.prototype.slice.call( arguments, 1 ));
    } else if ( typeof method === 'object' || ! method ) {
        // якщо першим параметром іде об'єкт, або зовсім порожньо
        // виконуємо метод init
        return methods.init.apply( this, arguments );
    } else {
        // якщо нічого не вийшло
        $.error('Метод "' + method + '" в плагіні не знайдено');
    }
};
```

Тепер ще невеликий приклад використання цих методів:

```javascript
// виклик без параметрів - буде викликано init
$('p').mySimplePlugin();

// виклик методу color і передача кольору як параметра
$('p').mySimplePlugin('color', '#FFFF00');

// виклик методу reset
$('p').mySimplePlugin('reset');
```

{% embed url="https://anton.shevchuk.name/book/code/plugin.extend.html" %}

Для розуміння цього шматочка коду тобі треба розібратися лише зі змінною `arguments` та методом `apply()`. Тут їм цілі статті присвятили, тож вперед:

* [https://uk.javascript.info/arguments-pseudoarray](https://uk.javascript.info/arguments-pseudoarray)
* [https://uk.javascript.info/call-apply](https://uk.javascript.info/call-apply)

## Про обробники подій <a href="#event-handlers" id="event-handlers"></a>

Якщо твій плагін навішує який-небудь обробник, то найкраще (читай – завжди) цей обробник навісити у своєму власному namespace:

```javascript
return this.on("click.mySimplePlugin", function(){
    $(this).css('color', options.color);
});
```

Цей фінт дозволить у будь-який момент прибрати всі твої обробники або викликати тільки твій, що дуже зручно:

```javascript
// викличемо лише наш обробник
$('p').trigger("click.mySimplePlugin");

// прибираємо всі наші обробники
$('p').off(".mySimplePlugin");
```

> — Дежавю? Ок!

{% hint style="info" %}
Дам ще трішки інформації для роздумів, але англійською:

&#x20;— [Essential jQuery Plugin Patterns](https://www.smashingmagazine.com/2011/10/essential-jquery-plugin-patterns/)
{% endhint %}

На цьому про звичайні плагіни все. Усім дякую за увагу.
