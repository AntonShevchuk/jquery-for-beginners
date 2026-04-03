# Події

Для початку, варто запам'ятати події, з якими найчастіше доведеться працювати:

<table data-header-hidden><thead><tr><th width="262">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>change</code></td><td>зміна значення елемента</td></tr><tr><td><code>submit</code></td><td>відправка форми</td></tr></tbody></table>

В яких же випадках вони нам допоможуть? Та все просто – відстеження `change` дозволяє обробляти такі події, як зміна елементів `<select>`, `<input type="radio">` та інших. Це знадобиться для динамічної зміни форми або перевірки введеного значення.

Відстеження `submit` необхідно для перевірки правильності заповнення всіх полів форми, а також для відправки форми за допомогою AJAX.

Почнемо з останнього, для початку візьмемо форму простішу:

```markup
<form action="index.html">
    <input type="text" name="firstname" value="Ivan"/>
    <input type="text" name="lastname" value="Ivanov"/>
    <input type="submit"/>
</form>
```

І спробуємо відправити її за адресою з атрибута «`action`» за допомогою AJAX-запиту (для відстеження події `submit` можна використовувати однойменний «shorthand» метод):

```javascript
$("form").submit(function(){
  // для читабельності коду
  let $form = $(this);
  // ти ж розумієш, про що я тут кажу?
  // це ж одна з іпостасей AJAX-запиту 
  $.post(
    $form.attr("action"), // посилання куди відправляємо дані
    $form.serialize()     // дані форми
  );
  // вимикаємо дію за замовчуванням
  return false;
});
```

Тут затесався один незнайомий метод — `serialize()`, він відповідає за «збір» даних з форми у зручному для передачі даних форматі:

```
firstname=Ivan&lastname=Ivanov
```

Також є метод `serializeArray()` — він представляє зібрані дані у вигляді масиву об'єктів:

```javascript
[
  {
    "name": "firstname",
    "value": "Ivan"
  },
  {
    "name": "lastname",
    "value": "Ivanov"
  }
]
```

Хоч ця глава і про події, але все ж без методів нам далі не обійтися — зустрічай метод, який нам часто знадобиться:

<table data-header-hidden><thead><tr><th width="262">метод</th><th>опис</th></tr></thead><tbody><tr><td><code>val()</code></td><td>отримання значення першого елемента форми з вибірки</td></tr><tr><td><code>val(value)</code></td><td>встановлення значення всім елементам форми з вибірки</td></tr></tbody></table>

Тепер, з його допомогою, ми можемо додати в попередній код трохи перевірки даних:

```javascript
if ($form.find("input[name=firstname]").val() === "") {
  alert("Введіть ім'я користувача");
  return false;
}
```

Добре, працювати з формою тепер вміємо, залишилося прикрутити більш адекватний вивід помилок (так-так, за `alert()` б'ємо по руках):

```javascript
if ($form.find("input[name=firstname]").val() === "") {
  $form.find("input[name=firstname]")
       .after('<div class="error">Введіть ім\'я</div>');
  return false;
}
```

При повторній відправці форми не забудьте прибрати повідомлення, що залишилися від попередньої перевірки:

```javascript
$form.find('.error').remove()
```

Тепер можна об'єднати шматочки коду й отримати такий варіант:

```javascript
$('form').submit(function() {
  // для читабельності коду
  let $form = $(this);

  // чистимо помилки
  $form.find('.error').remove();

  // перевіряємо поле з ім'ям користувача
  if ($form.find('input[name=firstname]').val() === '') {
    // додаємо текст помилки
    $form.find('input[name=firstname]')
         .after('<div class="error">Введіть ім\'я</div>');
    // перериваємо подальшу обробку
    return false;
  }

  // все добре – відправляємо запит на сервер
  $.post(
    $form.attr('action'), // посилання куди відправляємо дані
    $form.serialize()     // дані форми
  );

  // вимикаємо дію за замовчуванням
  return false;
});
```

Попередній приклад вже додано на сторінку [form.basic.html](https://anton.shevchuk.name/book/code/form.basic.html), спробуйте відправити форму; запит та відповідь, що вийшли, зможете вивчити в консолі браузера:

{% embed url="https://anton.shevchuk.name/book/code/form.basic.html" %}

Я хотів ще повернутися до списку подій форми і перелічити ті, що бракують:

<table><thead><tr><th width="262">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>focus</code></td><td>фокус на елементі, для роботи з цією подією також є «shorthand» метод <code>focus()</code>; знадобиться, якщо треба вивести підказку до елемента форми при наведенні</td></tr><tr><td><code>blur</code></td><td>фокус пішов з елемента + метод <code>blur()</code>; стане в нагоді при валідації форми в міру заповнення полів</td></tr><tr><td><code>select</code></td><td>вибір тексту в «<code>textarea</code>» і «<code>input[type=text]</code>» + метод <code>select()</code>; якщо зберетеся розробляти свій WYSIWYG, то познайомитеся дуже щільно</td></tr></tbody></table>

Приклади відстеження всіх подій форми доступні на сторінці [events.form.html](https://anton.shevchuk.name/book/code/events.form.html):

{% embed url="https://anton.shevchuk.name/book/code/events.form.html" %}
