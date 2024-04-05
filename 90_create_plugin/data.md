# Data реестр

{% hint style="info" %}
Если по какой-то причине вы ещё не знакомы с `data()`, советую перечитать главу [Data-атрибуты ](../20\_attributes\_and\_properties/data-atributy.md)и ознакомиться с [официальной документацией](https://api.jquery.com/data/).&#x20;
{% endhint %}

В разрезе разработки плагинов нам следует запомнить следующее — метод `$.data()`  дает возможность вести реестр данных, который будут привязаны к какому-либо элементу. Таким образом, все данные плагинов лучше хранить в этом реестре. Т.е. если вам надо сохранить состояние плагина — используйте `data()`, если необходим кэш — используйте `data()`, если вам необходимо сохранить … ну, думаю, понятно. Приведу ещё примерчик, связанный с инициализацией:

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

{% hint style="danger" %}
Напоминаю, что метод`jQuery.data()` не манипулирует атрибутами HTML, а работает со своим реестром, и лишь при отсутствии данных в своём реестре пытается заполучить атрибут `data-*`.
{% endhint %}

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}

{% hint style="info" %}
Надо бы не забыть рассказать про `obj[jQuery.expando] = uuid` и `jQuery._data(element)`, а так же про [утечки памяти](https://learn.javascript.ru/memory-leaks-jquery), но наверное не сегодня.
{% endhint %}
