# Data реєстр

{% hint style="info" %}
Якщо з якоїсь причини ти ще не знайомий з `data()`, раджу перечитати главу [Data-атрибути ](../20\_attributes\_and\_properties/data-attributes.md)та ознайомитися з [офіційною документацією](https://api.jquery.com/data/).&#x20;
{% endhint %}

В розрізі розробки плагінів нам слід запам'ятати наступне — метод `$.data()` дає можливість вести реєстр даних, які будуть прив'язані до якого-небудь елемента. Таким чином, всі дані плагінів краще зберігати в цьому реєстрі. Тобто якщо тобі треба зберегти стан плагіна — використовуй `data()`, якщо необхідний кеш — використовуй `data()`, якщо тобі необхідно зберегти … ну, думаю, зрозуміло. Наведу ще приклад, пов'язаний з ініціалізацією:

```javascript
function() { // функція init
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
Нагадую, що метод `jQuery.data()` не маніпулює атрибутами HTML, а працює зі своїм реєстром, і лише за відсутності даних у своєму реєстрі намагається отримати атрибут `data-*`.
{% endhint %}

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}

{% hint style="info" %}
Треба б не забути розповісти про `obj[jQuery.expando] = uuid` та `jQuery._data(element)`, а також про [витоки пам'яті](https://uk.javascript.info/memory-leaks-jquery), але мабуть не сьогодні.
{% endhint %}
