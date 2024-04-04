# События

Для начала, стоит запомнить события, с которыми чаще всего придется работать:

<table data-header-hidden><thead><tr><th width="262">событие</th><th>описание</th></tr></thead><tbody><tr><td><code>change</code></td><td>изменение значения элемента</td></tr><tr><td><code>submit</code></td><td>отправка формы</td></tr></tbody></table>

В каких же случаях они нам помогут? Да всё просто – отслеживание `change` позволяет обрабатывать такие события, как изменение элементов `<select>`, `<input type="radio">` и прочих. Это потребуется для динамического изменения формы или проверки введённого значения.&#x20;

Отслеживание `submit` необходимо для проверки правильности заполнения всех полей формы, а также для отправки формы посредством AJAX.

Начнём с последнего, для начала возьмём форму попроще:

```markup
<form action="index.html">
    <input type="text" name="firstname" value="Ivan"/>
    <input type="text" name="lastname" value="Ivanov"/>
    <input type="submit"/>
</form>
```

И попробуем отправить её по адресу из атрибута «`action`» посредством AJAX-запроса (для отслеживания события `submit` можно использовать одноименный «shorthand» метод):

```javascript
$("form").submit(function(){
  // для читаемости кода
  let $form = $(this);
  // вы же понимаете, о чём я тут толкую?
  // это ведь одна из ипостасей AJAX-запроса 
  $.post(
    $form.attr("action"), // ссылка куда отправляем данные
    $form.serialize()     // данные формы
  );
  // отключаем действие по умолчанию
  return false;
});
```

Тут затесался один незнакомый метод — `serialize()`, он в ответе за «сбор» данных с формы в удобном для передачи данных формате:

```
firstname=Ivan&lastname=Ivanov
```

Также есть метод `serializeArray()` — он представляет собранные данные в виде объекта:

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

Хоть данная глава и про события, но всё же без методов нам дальше не обойтись — встречайте метод, который нам будет частенько нужен:

<table data-header-hidden><thead><tr><th width="262">метод</th><th>описание</th></tr></thead><tbody><tr><td><code>val()</code></td><td>получение значения первого элемента формы из выборки</td></tr><tr><td><code>val(value)</code></td><td>установка значения всем элементам формы из выборки</td></tr></tbody></table>

Теперь, с его помощью, мы можем добавить в предыдущий код немного проверки данных:

```javascript
if ($form.find("input[name=firstname]").val() === "") {
  alert("Введите имя пользователя");
  return false;
}
```

Хорошо, работать с формой теперь можем, осталось прикрутить более вменяемый вывод ошибок (да-да, за `alert()` бьём по рукам):

```javascript
if ($form.find("input[name=firstname]").val() === "") {
  $form.find("input[name=firstname]")
       .after('<div class="error">Введите имя</div>');
  return false;
}
```

При повторной отправке формы не забудьте убрать сообщения, оставшиеся от предыдущей проверки:

```javascript
$form.find('.error').remove()
```

Теперь можно объединить кусочки кода и получить следующий вариант:

```javascript
$('form').submit(function() {
  // для читаемости кода
  let $form = $(this);

  // чистим ошибки
  $form.find('.error').remove();

  // проверяем поле с именем пользователя
  if ($form.find('input[name=firstname]').val() === '') {
    // добавляем текст ошибки
    $form.find('input[name=firstname]')
         .after('<div class="error">Введите имя</div>');
    // прерываем дальнейшую обработку
    return false;
  }

  // всё хорошо – отправляем запрос на сервер
  $.post(
    $form.attr('action'), // ссылка куда отправляем данные
    $form.serialize()     // данные формы
  );

  // отключаем действие по умолчанию
  return false;
});
```

Предыдущий пример уже добавлен на страницу [form.basic.html](https://anton.shevchuk.name/book/code/form.basic.html), попробуйте отправить форму; получившийся запрос и ответ вы сможете изучить в консоли браузера:

{% embed url="https://anton.shevchuk.name/book/code/form.basic.html" %}

Я хотел ещё вернуться к списку событий формы и перечислить недостающие:

<table><thead><tr><th width="262">событие</th><th>описание</th></tr></thead><tbody><tr><td><code>focus</code></td><td>фокус на элементе, для работы с данным событием так же есть «shorthand» метод <code>focus()</code>; потребуется, если надо вывести подсказку к элементу формы при наведении</td></tr><tr><td><code>blur</code></td><td>фокус ушёл с элемента + метод <code>blur()</code>; пригодится при валидации формы по мере заполнения полей</td></tr><tr><td><code>select</code></td><td>выбор текста в «<code>textarea</code>» и «<code>input[type=text]</code>» + метод <code>select()</code>; если соберётесь разрабатывать свой WYSIWYG, то познакомитесь очень плотно</td></tr></tbody></table>

Примеры отслеживания всех событий формы доступны на странице [events.form.html](https://anton.shevchuk.name/book/code/events.form.html):

{% embed url="https://anton.shevchuk.name/book/code/events.form.html" %}
