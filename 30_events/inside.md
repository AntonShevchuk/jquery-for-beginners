---
description: Немного технических деталей о внутренней кухне jQuery
---

# Дополнение

## Shorthand-методы

Мы уже успели познакомиться с shorthand-методом `click()`. В действительности, этот метод представляет из себя лишь обёртку для вызова `on()` и `trigger()`:

```javascript
jQuery.click = function( data, fn ) {
    if (arguments.length > 0) {
        this.on("click", fn);
    } else {
        this.trigger("click");
    }
};
```

> Ой, код я чуть-чуть изменил, для читаемости. Если же любопытство восторжествует, то ищите в исходном коде по строке «`dblclick`»

## Обработчик событий для объекта

Можно повесить обработчик событий практически на любой объект:

```javascript
// объект проще некуда
var obj = {
    test:function() {
        console.log("obj.test");
    }
}

// создаём обработчик произвольного события someEvent
$(obj).on("someEvent", function(){
    console.log("obj.someEvent");
    this.test();
});

// полюбопытствуем результатом
console.log(obj);

// инициируем событие someEvent
$(obj).trigger("someEvent");
```

> Скопируйте приведённый код в консоль и запустите, я думаю, вам будет интересно ;)

## Где хранятся обработчики событий?

Внутри самого jQuery :) Если вы хотите получить все обработчики событий, то вам потребуется следующий метод:

```javascript
// тут element - это DOMElement
$._data(element, "events");
```

{% hint style="warning" %}
В комментариях к исходному коду jQuery есть вот такая фраза «_Never expose "private" data to user code (TODO: Drop \_data, \_removeData)_».  Так что в один прекрасный момент данный раздел перестанет быть актуальным.
{% endhint %}

> Раньше всё обработчики событий были доступны в объекте `$.cache`, но в последних версиях jQuery данного объекта нет :(
