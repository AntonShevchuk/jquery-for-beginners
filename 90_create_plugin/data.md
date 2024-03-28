# Data

Если по какой-то причине вы ещё не знакомы с `data()`, советую перечитать главу [Data-атрибуты ](../20\_attributes\_and\_properties/data-atributy.md)и ознакомиться с [официальной документацией](https://api.jquery.com/data/). Если же в двух словах — это реестр данных, и все данные, привязанные к какому-либо элементу, лучше хранить в нём, это же правило касается и плагинов. Если вам надо сохранить состояние плагина — используйте `data()`, если необходим кэш — используйте `data()`, если вам необходимо сохранить … ну, думаю, понятно. Приведу ещё примерчик, связанный с инициализацией:

```javascript
function() { // функция init
    var init = $(this).data('mySimplePlugin');
    if (init) {
        return this;
    } else {
        $(this).data('mySimplePlugin', true);
        return this.on('click.mySimplePlugin', function(){
            $(this).css('color', options.color);
        });
    }
}
```

По стечению обстоятельств в HTML 5 появились data-атрибуты и для доступа к ним jQuery использует тот же метод `data()`, но вот дела – `jQuery.data()` не манипулирует атрибутами HTML, а работает со своим реестром, и лишь при отсутствии там данных пытается заполучить атрибут `data-*`. Не попадитесь:

```markup
<div id="my" data-foo="bar"></div>
```

```javascript
$("#my").data("foo");      // >>> bar
$("#my").attr("data-foo"); // >>> bar

$("#my").data("foo", "xyz");

$("#my").data("foo");      // >>> xyz
$("#my").attr("data-foo"); // >>> bar

$("#my").attr("data-foo", "def");

$("#my").data("foo");      // >>> xyz
$("#my").attr("data-foo"); // >>> def
```

Наглядный пример:

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}

{% hint style="info" %}
Надо бы не забыть рассказать про `obj[jQuery.expando] = uuid` и `jQuery._data(element)`, а так же про [утечки памяти](https://learn.javascript.ru/memory-leaks-jquery)
{% endhint %}
