---
description: Объект $.Callbacks
---

# Callbacks

[Callbacks](https://api.jquery.com/jQuery.Callbacks/) – это крутой объект. Он позволяет составлять списки функций обратного вызова, а также даёт бразды правления над ними. Работать с ним проще нежели с Deferred, в нём нет разделения на позитивный и негативный сценарии. Лишь стек функций, который будет выполнен по команде `fire()`:

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

> — А в чём сила, брат?\
> — В аргументах.

По умолчанию вы можете прямо из консоли вызывать метод `fire()` снова и снова, и будете получать один и тот же результат раз за разом. А можно задать поведение Callbacks через флаги:

<table><thead><tr><th width="213">флаг</th><th>описание</th></tr></thead><tbody><tr><td><code>once</code></td><td>все функции будут вызваны единожды (аналогично как в объекте Deferred)</td></tr><tr><td><code>memory</code></td><td>сохранять значение с последнего вызова <code>fire()</code>, и скармливать его в ново-зарегистрированные функции обратного вызова, и лишь потом обрабатывает новое значение (в Deferred именно так)</td></tr><tr><td><code>unique</code></td><td>список функций обратного вызова фильтруется по уникальности</td></tr><tr><td><code>stopOnFalse</code></td><td>как только какая-нибудь функция вернёт «<code>false</code>», процесс запуска остановится</td></tr></tbody></table>

Наверное, будет лучше с примерами, вот `once`:

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

C `memory` посложнее, будьте внимательней:

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

Пример с уникальностью `unique` прост до безобразия:

```javascript
var C = $.Callbacks("unique");

var func = function(msg) {
  console.log(msg + " first")
};

C.add(func);
C.add(func); // эта строка не повлияет на результат

C.fire("Go"); // только Go first
```

```
>>>
Go first
```

Флаг `stopOnFalse`:

```javascript
var C = $.Callbacks("stopOnFalse");

C.add(function(msg) {
  console.log(msg + " first");
  return false; // вот он – роковой false
});

C.add(function(msg) { 
  console.log(msg + " second"); 
});

C.fire("Go"); // только Go first
```

```
>>>
Go first
```

Перечисленные флаги можно комбинировать и получать интересные результаты, а можно не получать, а лишь посмотреть на следующий пример. Вот страничка для работы:

{% embed url="https://anton.shevchuk.name/book/code/callbacks.html" %}

```javascript
// машинка
var $car = $('#car').show();
// наш стек команд
var C = $.Callbacks();
```

Добавьте команды по передвижению автомобиля в произвольном порядке и количестве. Для этого следует запустить перечисленные ниже скрипты (никаких изменений с машинкой до запуска метода `fire()` не будет):

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот вниз, ставим в очередь с анимацией
    $(this).css("transform", "rotate(-90deg)").dequeue();
  });
  $car.animate({top:"+=800"}, speed); // едем вниз
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот налево, ставим в очередь с анимацией
    $(this).css("transform", "rotate(0deg)").dequeue();
  });
  $car.animate({left:"-=400"}, speed); // едем влево
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот вправо, ставим в очередь с анимацией
    $(this).css("transform", "rotate(180deg)").dequeue();
  });
  $car.animate({left:"+=400"}, speed); // едем вправо
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот наверх ставим в очередь с анимацией
    $(this).css("transform", "rotate(90deg)").dequeue();
  });
  $car.animate({top:"-=400"}, speed); // едем вверх
});
```

Поехали!

```javascript
// запускаем «программу»
// в качестве параметра передаём скорость движения
C.fire(400);
```

> Из истории: объект «Deferred» отпочковался от метода `$.ajax()` в результате рефакторинга jQuery версии 1.5. Шло время, появлялись новые версии jQuery, и вот новый виток рефакторинга – результатом стало отделение «Callbacks» от «Deferred» в версии 1.7, таким образом в текущей версии библиотеки метод `$.ajax()` работает с объектом «Deferred», который является надстройкой над «Callbacks». Дабы не вносить путаницу в терминологию, я использую определение «Deferred Callbacks» и при работе с «Callbacks», ибо колбэков много, и каждый раз уточнять, что я говорю именно «о том самом» — дело достаточно утомительное.

{% hint style="info" %}
Статья по данной теме, рекомендую так сказать — [Async JS: The Power of $.deffered](https://web.dev/articles/async-deferred)
{% endhint %}
