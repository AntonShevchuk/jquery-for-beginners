# Events

First things first, let's memorize the events you'll most often work with:

<table data-header-hidden><thead><tr><th width="262">event</th><th>description</th></tr></thead><tbody><tr><td><code>change</code></td><td>change of an element's value</td></tr><tr><td><code>submit</code></td><td>form submission</td></tr></tbody></table>

When will these come in handy? It's simple — tracking `change` lets you handle events like changes to `<select>`, `<input type="radio">`, and others. You'll need this for dynamically modifying a form or validating an entered value.

Tracking `submit` is necessary for validating that all form fields are filled in correctly, as well as for submitting the form via AJAX.

Let's start with the latter. First, let's take a simple form:

```markup
<form action="index.html">
    <input type="text" name="firstname" value="Ivan"/>
    <input type="text" name="lastname" value="Ivanov"/>
    <input type="submit"/>
</form>
```

And try to submit it to the address from the "`action`" attribute via an AJAX request (to track the `submit` event, you can use the shorthand method of the same name):

```javascript
$("form").submit(function(){
  // for code readability
  let $form = $(this);
  // you get what I'm talking about here, right?
  // this is one of the forms of an AJAX request
  $.post(
    $form.attr("action"), // the URL to send data to
    $form.serialize()     // form data
  );
  // disable the default action
  return false;
});
```

There's one unfamiliar method that snuck in here — `serialize()`, it's responsible for "collecting" form data in a format convenient for transmission:

```
firstname=Ivan&lastname=Ivanov
```

There's also a `serializeArray()` method — it presents the collected data as an array of objects:

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

Even though this chapter is about events, we can't go further without methods — meet the method we'll need quite often:

<table data-header-hidden><thead><tr><th width="262">method</th><th>description</th></tr></thead><tbody><tr><td><code>val()</code></td><td>get the value of the first form element in the selection</td></tr><tr><td><code>val(value)</code></td><td>set the value for all form elements in the selection</td></tr></tbody></table>

Now, with its help, we can add some validation to the previous code:

```javascript
if ($form.find("input[name=firstname]").val() === "") {
  alert("Enter the user's name");
  return false;
}
```

Good, we can now work with the form, all that's left is to hook up a more reasonable error output (yes, using `alert()` is frowned upon):

```javascript
if ($form.find("input[name=firstname]").val() === "") {
  $form.find("input[name=firstname]")
       .after('<div class="error">Enter your name</div>');
  return false;
}
```

When resubmitting the form, don't forget to clear the messages left from the previous validation:

```javascript
$form.find('.error').remove()
```

Now we can combine the code snippets and get the following version:

```javascript
$('form').submit(function() {
  // for code readability
  let $form = $(this);

  // clear errors
  $form.find('.error').remove();

  // validate the username field
  if ($form.find('input[name=firstname]').val() === '') {
    // add the error text
    $form.find('input[name=firstname]')
         .after('<div class="error">Enter your name</div>');
    // stop further processing
    return false;
  }

  // everything is fine — send the request to the server
  $.post(
    $form.attr('action'), // the URL to send data to
    $form.serialize()     // form data
  );

  // disable the default action
  return false;
});
```

The previous example has already been added to the [form.basic.html](https://anton.shevchuk.name/book/code/form.basic.html) page — try submitting the form; you can examine the resulting request and response in the browser console:

{% embed url="https://anton.shevchuk.name/book/code/form.basic.html" %}

I wanted to come back to the list of form events and mention the missing ones:

<table><thead><tr><th width="262">event</th><th>description</th></tr></thead><tbody><tr><td><code>focus</code></td><td>focus on an element; there's also a shorthand method <code>focus()</code> for working with this event; you'll need it if you want to show a hint for a form element on focus</td></tr><tr><td><code>blur</code></td><td>focus left the element + the <code>blur()</code> method; useful for validating the form as fields are being filled in</td></tr><tr><td><code>select</code></td><td>text selection in "<code>textarea</code>" and "<code>input[type=text]</code>" + the <code>select()</code> method; if you ever set out to build your own WYSIWYG, you'll get to know this one really well</td></tr></tbody></table>

Examples of tracking all form events are available on the [events.form.html](https://anton.shevchuk.name/book/code/events.form.html) page:

{% embed url="https://anton.shevchuk.name/book/code/events.form.html" %}
