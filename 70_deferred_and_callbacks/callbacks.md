## Callbacks {#callbacks}

Callbacks – это крутой объект. Он позволяет составлять списки функций обратного вызова, а также даёт бразды правления над ними. Работать с ним проще нежели с Deferred, в нём нет разделения на позитивный и негативный сценарии. Лишь стек функций, который будет выполнен по команде «.fire()»:

{% jqbEval %}{% endjqbEval %}

```javascript
var C = $.Callbacks();

C.add(function(msg) {
    console.log(msg + " first")
});

C.add(function(msg) {
    console.log(msg + " second")
});

C.fire("Go");
```

Если открыть консоль и запустить скрипт выше, то в результате получите что-то типа такого:
```
>>>
Go first
Go second
```

> _— А в чём сила, брат?_
> _— В аргументах._

По умолчанию вы можете прямо из консоли вызывать метод «.fire()» снова и снова, и будете получать один и тот же результат раз за разом. А можно задать поведение Callbacks через флаги:

`once` — все функции будут вызваны единожды (аналогично как в объекте Deferred).

`memory` — сохранять значение с последнего вызова «.fire()», и скармливать его в ново-зарегистрированные функции обратного вызова, и лишь потом обрабатывает новое значение (в Deferred именно так).

`unique` — список функций обратного вызова фильтруется по уникальности

`stopOnFalse` — как только какая-нибудь функция вернёт «false», процесс запуска остановится

Наверное, будет лучше с примерами, вот «once»:

{% jqbEval %}{% endjqbEval %}

```javascript
var C = $.Callbacks("once");

C.add(function(msg) {
    console.log(msg + " first")
});

C.add(function(msg) {
    console.log(msg + " second")
});

C.fire("Go");

C.fire("Again"); // не даст результата, только Go
```
```
>>>
Go first
Go second
```

C «memory» посложнее, будьте внимательней:

{% jqbEval %}{% endjqbEval %}

```javascript
var C = $.Callbacks("memory");

C.add(function(msg) {
    console.log(msg + " first")
});

C.fire("Go");

C.add(function(msg) {
  console.log(msg + " second")
});

C.fire("Again");
```
```
>>>
Go first
Go second // без флага, этой строчки не было бы
Again first
Again second
```

Пример с уникальностью прост до безобразия:

{% jqbEval %}{% endjqbEval %}

```javascript
var C = $.Callbacks("unique");

var func = function(msg) {
    console.log(msg+" first")
};

C.add(func);
C.add(func); // эта строка не повлияет на результат

C.fire("Go"); // только Go first
```

```
>>>
Go first
```

Флаг «stopOnFalse»:

{% jqbEval %}{% endjqbEval %}

```javascript
var C = $.Callbacks("stopOnFalse");

C.add(function(msg) {
    console.log(msg+" first");
    return false; // вот он – роковой false
});

C.add(function(msg) { 
    console.log(msg+" second"); 
});

C.fire("Go"); // только Go first
```

```
>>>
Go first
```

Перечисленные флаги можно комбинировать и получать интересные результаты, а можно не получать, а лишь посмотреть на пример [callbacks.html](http://anton.shevchuk.name/book/code/callbacks.html)

> _Из истории: объект «Deferred» отпочковался от метода «$.ajax()» в результате рефакторинга jQuery версии 1.5. Шло время, появлялись новые версии jQuery, и вот новый виток рефакторинга – результатом стало отделение «Callbacks» от «Deferred» в версии 1.7, таким образом в текущей версии библиотеки метод «$.ajax()» работает с объектом «Deferred», который является надстройкой над «Callbacks». Дабы не вносить путаницу в терминологию, я использую определение «Deferred Callbacks» и при работе с «Callbacks», ибо колбэков много, и каждый раз уточнять, что я говорю именно «о том самом» — дело достаточно утомительное._

Статьи по данной теме:

* [Что такое этот новый jQuery.Callbacks Object](http://habrahabr.ru/post/135821/)
* [jQuery Deferred Object (подробное описание)](http://habrahabr.ru/post/113073/)
* [Async JS: The Power of $.deffered](http://www.html5rocks.com/en/tutorials/async/deferred/)
