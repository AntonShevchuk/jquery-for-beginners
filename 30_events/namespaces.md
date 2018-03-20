## Пространство имен

Как вы уже узнали, когда мы хотим создать свой обработчик событий, мы пишем вот такой код:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// создаем свой обработчик
$("p").on("click", function() {
    // что-то делаем
    alert("Click!");
});
```

Когда нам надо удалить обработчики, используем следующий код:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// удаляем все обработчики
$("p").off();

// удаляем все обработчики click
$("p").off("click");
```

Для экспериментов воспользуемся уже знакомым примером:

{% jqbFrame "handlers-example", "../code/events.handlers.html", height="200px" %}
{% sticky %}
{% endjqbFrame %}

Но, как всегда, есть ситуации, когда нам необходимо задействовать или отключить не все обработчики (как пример, надо отключить обработку какого-то контрола определённым плагином). В этом случае нам на помощь приходят пространства имён, и использовать их достаточно легко.

При создании обработчика события добавляем «namespace» через точку:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// создаём обработчик
$("p").on("click.namespace", function(event){
    // что-то делаем
    alert("Click: " + event.namespace + "!");
});
```

Когда нам надо вызвать обработчик события привязанный только к нашему «namespace» используем следующий синтаксис:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
$("p").trigger("click.namespace");
```

Когда вызываем событие из другого пространства имён, наш обработчик не будет вызван:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
$("p").trigger("click.other");
```

Когда вызываем все-все обработчики событий:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// вызываем все обработчики события click
$("p").trigger("click");
```

Когда вызываем все обработчики без пространства имён:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// вызываем все обработчики без пространства имён
$("p").trigger("click.$");
```

> _До версии 1.9 для это цели следовало использовать имя события с восклицательным знаком «click!», а после — вот такой workaround завязанный на недостатке одного регулярного выражения._

И последний случай — удаление обработчика событий привязанного к нашему «namespace»:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// удаляем все обработчики click в данном пространстве имён
$("p").off("click.namespace");
```

Хот можно одним махом удалить все обработчики из определённого пространства имён:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// обработчик клика
$("p").on("click.color", function() {
  $(this).css("color", "red");
});

// обработчик при наведении
$("p").on("mouseenter.color", function() {
  $(this).css("color", "green");
});
```

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// передумали, и все отменили
$("p").off(".namespace");
```

Ещё полезный пример хитрого обработчика — он может ловить и обрабатывать данные:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// создаём обработчик
$("p").on("click.data", function(event, one, two, three) {
    alert("Click data: " + one + ", " + two + ", " + three);
});
```

Иницируем обработку события, в качестве данных передаём массив аргументов: 

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
$("p").trigger("click.data", [1, 2, 3]);
```

Так же хотел обратить внимание на поддержку нескольких пространств имён:

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// создаём обработчик для a
$("p").on("click.a", function(event) {
    alert("Click A!");
});

// создаём обработчик для b
$("p").on("click.b", function(event) {
    alert("Click B!");
});

// создаём обработчик для пространства a и b
$("p").on("click.a.b", function(event) {
    // для пространства имён a и b    
    alert("Click: " + event.namespace + "!");
});
```

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// вызываем обработчик из пространства a
$("p").trigger("click.a");
```

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// вызываем обработчик из пространства b
$("p").trigger("click.b");
```

{% jqbRun "#handlers-example" %}{% endjqbRun %}

```javascript
// отменяем обработчик click для пространства b
$("p").off("click.b");
```

[Официальная документация](http://api.jquery.com/event.namespace/) скудна на этот счёт, и я надеюсь, мои примеры помогли вам лучше разобраться в данном вопросе.
