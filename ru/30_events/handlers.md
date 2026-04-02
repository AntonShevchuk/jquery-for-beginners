# Работа с событиями

Для работы с событиями существует три основных метода:

<table data-header-hidden><thead><tr><th width="306">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">on(event, handler)
</code></pre></td><td>добавление обработчика<br>тут я использую простейшую сигнатуру метода</td></tr><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">trigger(event)
</code></pre></td><td>инициация события из скрипта</td></tr><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">off(event)
</code></pre></td><td>отключение обработчика событий</td></tr></tbody></table>

Давайте рассмотрим метод `.on()`:

```javascript
// вешаем обработчик
$("p").on("click", function() {
    // что-то делаем
    alert("Click!");
});
```

Запустите код выше в [песочнице](https://anton.shevchuk.name/book/code/sandbox.html), и попробуйте кликнуть по параграфу.

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

Можете данный обработчик запустить программно:

```javascript
$("p").trigger("click");
```

Когда наиграетесь, можете отключить обработчик с помощью метода `.off()`:

```javascript
$("p").off("click");
```

Хотя я ещё хотел упомянуть один важный момент — внутри обработчика вы можете получить доступ к DOM-элементу используя ключевое слово `this`. Если же надо будет воспользоваться jQuery-инструментами, то используйте конструкцию `$(this)`:

```javascript
$("p").on("click", function() {
    $(this).css("color", "red");
});
```
