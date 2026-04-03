# Маніпуляція над елементами

Ми вже познайомилися з методом `val()`. Цей метод чудово працює практично з усіма елементами форми. Угу, практично, ось з `<input type="radio">` встановити значення таким чином не вийде, тут знадобиться невеликий workaround:

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

> Можна, звісно, використовувати і метод `click()`, щоб емулювати вибір необхідного пункту, але це викличе всі обробники події «`click`», що небажано.

З `<input type="checkbox">` трішки простіше:

```javascript
$("input[type=checkbox]").prop("checked", true)
```

```html
<label>
  <input type="checkbox" name="remember"/> remember me
</label>
```

Перевіряємо «чекнутість» простим скриптом:

```javascript
$("input[type=checkbox]").prop("checked")
```

Альтернативний, трохи більш наочний спосіб:

```javascript
$("input[type=checkbox]").is(":checked")
```

Перевіряти й відправляти форму через AJAX тепер вміємо, залишилося вирішити питання з динамічною зміною форми. І для цього в нас вже є всі необхідні знання. Ось, наприклад, додавання випадаючого списку:

```javascript
$("form").append('<select name="some"></select>');
```

А якщо потрібно змінити список?

```html
<select name="role">
  <option value="admin">Admin</option>
  <option value="user">User</option>
</select>
```

Є на всі випадки життя:

```javascript
// візьмемо список заздалегідь, побережу чорнило
let $select = $("form select[name=role]");

// додамо новий елемент у випадаючий список
$select.append('<option value="manager">Manager</option>');

// виберемо необхідний елемент
$select.val("manager");
```

```javascript
// або за порядковим номером, починаючи з 0
$select.find("option:eq(1)").prop("selected", true);
```

```javascript
// перетворюємо на multiple
// не забуваємо, що ім'я такого селекта має бути з [], тобто
// myselect[], інакше сервер отримає лише одне значення
$select.attr("size",
    $select.find("option").length
);
$select.attr('multiple', true);
```

```javascript
// очищаємо список
$select.find("option").remove();
```

Ось так ми й розправилися з «жахливими» формами. Можливо, я ще наведу кілька прикладів з реального життя, але це буде вже в наступних версіях цього підручника :)

{% embed url="https://anton.shevchuk.name/book/code/form.methods.html" %}
