## Работа с событиями

Для работы с событиями существует три основных метода:

`on(event, handler)` – добавление обработчика (тут я использую простейшую сигнатуру метода)

`trigger(event)` – инициация события из скрипта

`off(event)` – отключение обработчика событий

Давайте рассмотрим метод «.on()»:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// вешаем обработчик
$("p").on("click", function() {
    // что-то делаем
    alert("Hello!");
});
```

Запустите данный код, и попробуйте кликнуть по параграфу:

<iframe class="jqbook" id="handlers-example" width="100%" height="200px" border="0" src="../code/events.handlers.html"></iframe>

Можете данный обработчик запустить программно:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
$("p").trigger("click");
```

Когда наиграетесь, можете отключить обработчик с помощью метода «.off()»:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
$("p").off("click");
```

Хотя я ещё хотел упомянуть один важный момент – внутри обработчика вы можете получить доступ к DOM-элементу используя ключевое слово `this`. Если же надо будет воспользоваться jQuery-инструментами, то используйте конструкцию «$(this)»: 


<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
$("p").on("click", function() {
    $(this).css("color", "red");
});
```
