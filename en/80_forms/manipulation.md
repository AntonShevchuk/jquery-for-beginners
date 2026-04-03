# Manipulating Elements

We've already met the `val()` method. This method works great with practically all form elements. Yep, practically — setting a value this way won't work with `<input type="radio">`, you'll need a small workaround:

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

> You could, of course, use the `click()` method to emulate selecting the desired option, but that would trigger all "`click`" event handlers, which is undesirable.

With `<input type="checkbox">` it's a tiny bit simpler:

```javascript
$("input[type=checkbox]").prop("checked", true)
```

```html
<label>
  <input type="checkbox" name="remember"/> remember me
</label>
```

Checking the "checkedness" with a simple script:

```javascript
$("input[type=checkbox]").prop("checked")
```

An alternative, slightly more visual way:

```javascript
$("input[type=checkbox]").is(":checked")
```

We now know how to validate and submit a form via AJAX — all that's left is to deal with dynamically modifying the form. And we already have all the knowledge we need for that. For example, adding a dropdown:

```javascript
$("form").append('<select name="some"></select>');
```

What if we need to modify the dropdown?

```html
<select name="role">
  <option value="admin">Admin</option>
  <option value="user">User</option>
</select>
```

Got it covered for every occasion:

```javascript
// let's grab the select ahead of time, for brevity
let $select = $("form select[name=role]");

// add a new item to the dropdown
$select.append('<option value="manager">Manager</option>');

// select the desired item
$select.val("manager");
```

```javascript
// or by index, starting from 0
$select.find("option:eq(1)").prop("selected", true);
```

```javascript
// convert to multiple
// don't forget that the name of such a select should have [], i.e.
// myselect[], otherwise the server will only receive one value
$select.attr("size",
    $select.find("option").length
);
$select.attr('multiple', true);
```

```javascript
// clear the dropdown
$select.find("option").remove();
```

And just like that, we've conquered the "terrible" forms. Maybe I'll add a few more real-life examples, but that'll be in future versions of this textbook :)

{% embed url="https://anton.shevchuk.name/book/code/form.methods.html" %}
