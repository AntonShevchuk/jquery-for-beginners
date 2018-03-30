## Манипуляции над элементами форм

Мы уже познакомились с методом «.val()». Данный метод отлично работает практически со всеми элементами формы. Угу, практически, вот с `<input type="radio">` установить значение таким образом не получится, тут потребуется небольшой workaround:

{% jqbEval %}{% endjqbEval %}

```javascript
$("input[type=radio][name=sex][value=male]").prop("checked", true)
```

<form action="">
  <label><input type="radio" name="sex" value="male"/> Male</label><br/>
  <label><input type="radio" name="sex" value="female"/> Female</label>
</form>

> _Можно, конечно же, использовать и метод «.click()», дабы эмулировать выбор необходимого пункта, но это вызовет все обработчики события «click», что не желательно._

С `<input type="checkbox">` чуть-чуть попроще:

{% jqbEval %}{% endjqbEval %}

```javascript
$("input[type=checkbox]").prop("checked", true)
```

<form action="">
  <label><input type="checkbox" name="rememberme" value="1"/> remember me</label>
</form>

Проверяем «чекнутость» простым скриптом:

{% jqbEval %}{% endjqbEval %}

```javascript
alert(
  $("input[type=checkbox]").prop("checked")
);
```

Альтернативный, чуть более наглядный способ:

{% jqbEval %}{% endjqbEval %}

```javascript
alert(
  $("input[type=checkbox]").is(":checked")
);
```

Проверять и отправлять форму через AJAX теперь умеем, осталось решить вопрос с динамическим изменением формы. И для этого у нас уже есть все необходимые знания. Вот, к примеру, добавление выпадающего списка:

```javascript
$("form").append('<select name="some"></select>');
```

А если потребуется изменить список? 

<form action="">
  <select name="role">
    <option>User</option>
    <option>Admin</option>
  </select>
</form>

Есть на все случаи жизни:

{% jqbEval %}{% endjqbEval %}

```javascript
// возьмём список заранее, поберегу чернила
var $select = $("form select[name=role]");

// добавим новый элемент в выпадающий список
$select.append("<option>Manager</option>");

// выберем необходимый элемент
$select.val("Manager");
```

{% jqbEval %}{% endjqbEval %}

```javascript
// или по порядковому номеру, начиная с 0
$select.find("option:eq(1)").prop("selected", true);
```

{% jqbEval %}{% endjqbEval %}

```javascript
// преобразуем в multiple
// не забываем, что имя такого селекта должно быть с [], т.е.
// myselect[], иначе сервер получит лишь одно значение
$select.attr("size",
    $select.find("option").length
);
$select.attr('multiple', true);
```

{% jqbEval %}{% endjqbEval %}

```javascript
// очищаем список
$select.find("option").remove();
```

Вот так мы и расправились с «ужасными» формами. Возможно, я ещё приведу несколько примеров из реальной жизни, но это будет уже в последующих версиях данного учебника :)
