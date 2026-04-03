---
description: Трохи технічних деталей про внутрішню кухню jQuery
---

# Доповнення

## Shorthand-методи

Ми вже встигли познайомитися з shorthand-методом `click()`. Насправді цей метод є лише обгорткою для виклику `on()` та `trigger()`:

```javascript
jQuery.fn.click = function( data, fn ) {
    if (arguments.length > 0) {
        this.on("click", fn);
    } else {
        this.trigger("click");
    }
};
```

> Ой, код я трішки змінив, для читабельності. Якщо ж цікавість переможе, то шукайте у вихідному коді по рядку «`dblclick`»

## Обробник подій для об'єкта

Можна повісити обробник подій практично на будь-який об'єкт:

```javascript
// об'єкт простіший нікуди
var obj = {
    test:function() {
        console.log("obj.test");
    }
}

// створюємо обробник довільної події someEvent
$(obj).on("someEvent", function(){
    console.log("obj.someEvent");
    this.test();
});

// поцікавимось результатом
console.log(obj);

// ініціюємо подію someEvent
$(obj).trigger("someEvent");
```

> Скопіюйте наведений код у консоль і запустіть, я думаю, вам буде цікаво ;)

## Де зберігаються обробники подій?

Усередині самого jQuery :) Якщо ви хочете отримати всі обробники подій, то вам знадобиться наступний метод:

```javascript
// тут element - це DOMElement
$._data(element, "events");
```

{% hint style="warning" %}
У коментарях до вихідного коду jQuery є ось така фраза «_Never expose "private" data to user code (TODO: Drop _data, _removeData)_».  Тож в один прекрасний момент цей розділ перестане бути актуальним.
{% endhint %}

> Раніше всі обробники подій були доступні в об'єкті `$.cache`, але в останніх версіях jQuery цього об'єкта немає :(
