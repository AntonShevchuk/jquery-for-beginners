---
description: Об'єкт $.Callbacks
---

# Callbacks

[Callbacks](https://api.jquery.com/jQuery.Callbacks/) – це крутий об'єкт. Він дозволяє складати списки функцій зворотного виклику, а також дає кермо влади над ними. Працювати з ним простіше, ніж з Deferred, у ньому немає розділення на позитивний та негативний сценарії. Лише стек функцій, який буде виконано за командою `fire()`:

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

Якщо відкрити консоль і запустити скрипт вище, то в результаті отримаєш щось на кшталт такого:

```
>>>
Go first
Go second
```

> — А в чому сила, брате?\
> — В аргументах.

За замовчуванням ти можеш прямо з консолі викликати метод `fire()` знову і знову, і будеш отримувати один і той самий результат раз за разом. А можна задати поведінку Callbacks через прапорці:

<table><thead><tr><th width="213">прапорець</th><th>опис</th></tr></thead><tbody><tr><td><code>once</code></td><td>всі функції будуть викликані лише одного разу (аналогічно як в об'єкті Deferred)</td></tr><tr><td><code>memory</code></td><td>зберігати значення з останнього виклику <code>fire()</code>, і згодовувати його в ново-зареєстровані функції зворотного виклику, і лише потім обробляти нове значення (в Deferred саме так)</td></tr><tr><td><code>unique</code></td><td>список функцій зворотного виклику фільтрується за унікальністю</td></tr><tr><td><code>stopOnFalse</code></td><td>як тільки яка-небудь функція поверне «<code>false</code>», процес запуску зупиниться</td></tr></tbody></table>

Мабуть, буде краще з прикладами, ось `once`:

```javascript
var C = $.Callbacks("once");

C.add(function(msg) {
  console.log(msg + " first")
});

C.add(function(msg) {
  console.log(msg + " second")
});

C.fire("Go");

C.fire("Again"); // не дасть результату, тільки Go
```

```
>>>
Go first
Go second
```

З `memory` складніше, будь уважний:

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
Go second // без прапорця, цього рядка не було б
Again first
Again second
```

Приклад з унікальністю `unique` простий до неподобства:

```javascript
var C = $.Callbacks("unique");

var func = function(msg) {
  console.log(msg + " first")
};

C.add(func);
C.add(func); // цей рядок не вплине на результат

C.fire("Go"); // тільки Go first
```

```
>>>
Go first
```

Прапорець `stopOnFalse`:

```javascript
var C = $.Callbacks("stopOnFalse");

C.add(function(msg) {
  console.log(msg + " first");
  return false; // ось він – фатальний false
});

C.add(function(msg) { 
  console.log(msg + " second"); 
});

C.fire("Go"); // тільки Go first
```

```
>>>
Go first
```

Перелічені прапорці можна комбінувати й отримувати цікаві результати, а можна не отримувати, а лише подивитися на наступний приклад. Ось сторінка для роботи:

{% embed url="https://anton.shevchuk.name/book/code/callbacks.html" %}

```javascript
// машинка
var $car = $('#car').show();
// наш стек команд
var C = $.Callbacks();
```

Додай команди для пересування автомобіля у довільному порядку та кількості. Для цього слід запустити наведені нижче скрипти (жодних змін з машинкою до запуску методу `fire()` не буде):

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот вниз, ставимо в чергу з анімацією
    $(this).css("transform", "rotate(-90deg)").dequeue();
  });
  $car.animate({top:"+=800"}, speed); // їдемо вниз
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот наліво, ставимо в чергу з анімацією
    $(this).css("transform", "rotate(0deg)").dequeue();
  });
  $car.animate({left:"-=400"}, speed); // їдемо вліво
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот направо, ставимо в чергу з анімацією
    $(this).css("transform", "rotate(180deg)").dequeue();
  });
  $car.animate({left:"+=400"}, speed); // їдемо вправо
});
```

```javascript
C.add(function(speed) {
  $car.queue(function() { // поворот вгору, ставимо в чергу з анімацією
    $(this).css("transform", "rotate(90deg)").dequeue();
  });
  $car.animate({top:"-=400"}, speed); // їдемо вгору
});
```

Поїхали!

```javascript
// запускаємо «програму»
// як параметр передаємо швидкість руху
C.fire(400);
```

> З історії: об'єкт «Deferred» відгалузився від методу `$.ajax()` в результаті рефакторингу jQuery версії 1.5. Минав час, з'являлися нові версії jQuery, і ось новий виток рефакторингу – результатом стало відділення «Callbacks» від «Deferred» у версії 1.7, таким чином у поточній версії бібліотеки метод `$.ajax()` працює з об'єктом «Deferred», який є надбудовою над «Callbacks». Щоб не вносити плутанину в термінологію, я використовую визначення «Deferred Callbacks» і при роботі з «Callbacks», бо колбеків багато, і кожного разу уточнювати, що я кажу саме «про той самий» — справа досить втомлива.

{% hint style="info" %}
Стаття за цією темою, рекомендую так би мовити — [Async JS: The Power of $.Deferred](https://web.dev/articles/async-deferred)
{% endhint %}
