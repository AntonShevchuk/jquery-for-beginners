# Робота з подіями

Для роботи з подіями існує три основних методи:

<table data-header-hidden><thead><tr><th width="306">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">on(event, handler)
</code></pre></td><td>додавання обробника<br>тут я використовую найпростішу сигнатуру методу</td></tr><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">trigger(event)
</code></pre></td><td>ініціація події зі скрипта</td></tr><tr><td><pre class="language-javascript" data-full-width="true"><code class="lang-javascript">off(event)
</code></pre></td><td>вимкнення обробника подій</td></tr></tbody></table>

Давайте розглянемо метод `.on()`:

```javascript
// вішаємо обробник
$("p").on("click", function() {
    // щось робимо
    alert("Click!");
});
```

Запустіть код вище у [пісочниці](https://anton.shevchuk.name/book/code/sandbox.html), і спробуйте клікнути по параграфу.

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

Можете цей обробник запустити програмно:

```javascript
$("p").trigger("click");
```

Коли награєтеся, можете вимкнути обробник за допомогою методу `.off()`:

```javascript
$("p").off("click");
```

Хоча я ще хотів згадати один важливий момент — всередині обробника ви можете отримати доступ до DOM-елемента, використовуючи ключове слово `this`. Якщо ж треба буде скористатися jQuery-інструментами, то використовуйте конструкцію `$(this)`:

```javascript
$("p").on("click", function() {
    $(this).css("color", "red");
});
```
