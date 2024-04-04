# Манипуляциия над элементами

Мы уже познакомились с методом `val()`. Данный метод отлично работает практически со всеми элементами формы. Угу, практически, вот с `<input type="radio">` установить значение таким образом не получится, тут потребуется небольшой workaround:

```javascript
$("input[type=radio][name=sex][value=male]").prop("checked", true)
```

```html
<label>
  <input type="radio" name="sex" value="male"/> Male
</label>
<label>
  <input type="radio" name="sex" value="female"/> Female
</label>
```

> Можно, конечно же, использовать и метод `click()`, дабы эмулировать выбор необходимого пункта, но это вызовет все обработчики события «`click`», что не желательно.

С `<input type="checkbox">` чуть-чуть попроще:

```javascript
$("input[type=checkbox]").prop("checked", true)
```

```html
<label>
  <input type="checkbox" name="remember"/> remember me
</label>
```

Проверяем «чекнутость» простым скриптом:

```javascript
$("input[type=checkbox]").prop("checked")
```

Альтернативный, чуть более наглядный способ:

```javascript
$("input[type=checkbox]").is(":checked")
```

Проверять и отправлять форму через AJAX теперь умеем, осталось решить вопрос с динамическим изменением формы. И для этого у нас уже есть все необходимые знания. Вот, к примеру, добавление выпадающего списка:

```javascript
$("form").append('<select name="some"></select>');
```

А если потребуется изменить список?

```html
<select name="role">
  <option value="admin">Admin</option>
  <option value="user">User</option>
</select>
```

Есть на все случаи жизни:

```javascript
// возьмём список заранее, поберегу чернила
let $select = $("form select[name=role]");

// добавим новый элемент в выпадающий список
$select.append("<option value="manager">Manager</option>");

// выберем необходимый элемент
$select.val("manager");
```

```javascript
// или по порядковому номеру, начиная с 0
$select.find("option:eq(1)").prop("selected", true);
```

```javascript
// преобразуем в multiple
// не забываем, что имя такого селекта должно быть с [], т.е.
// myselect[], иначе сервер получит лишь одно значение
$select.attr("size",
    $select.find("option").length
);
$select.attr('multiple', true);
```

```javascript
// очищаем список
$select.find("option").remove();
```

Вот так мы и расправились с «ужасными» формами. Возможно, я ещё приведу несколько примеров из реальной жизни, но это будет уже в последующих версиях данного учебника :)

{% embed url="https://anton.shevchuk.name/book/code/form.methods.html" %}
