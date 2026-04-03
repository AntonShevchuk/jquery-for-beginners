# Properties

Besides attributes, there are also element properties — these include `selectedIndex`, `tagName`, `nodeName`, `nodeType`, `ownerDocument`, `defaultChecked`, and `defaultSelected`. Well, the list isn't that long, you can memorize it. To work with properties we use methods from the `prop()` family:

<table data-header-hidden><thead><tr><th width="305">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propertyName)
</code></pre></td><td>get the property value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propertyName, value)
</code></pre></td><td>set the property value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop({
  propertyName1: value,
  propertyName2: value
})
</code></pre></td><td>set multiple values</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propName,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>using a callback function<br><code>index</code> is the element's position in the selection<br><code>value</code> is the current property value</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeProp(propName)
</code></pre></td><td>remove a property (most likely, you'll never need this)</td></tr></tbody></table>

{% hint style="danger" %}
Now turn off the music and remember this — **for disabling form elements and for checking/changing the state of checkboxes, we always use the `prop()` method.** Don't be confused by the existence of same-named attributes in HTML (I'm talking about "`disabled`" and "`checked`") — use `prop()`, period.
{% endhint %}

Let's see an example:

{% embed url="https://anton.shevchuk.name/book/code/properties.html" %}

See how the system works without our interference — click the checkbox, the dropdown, try submitting the form.

Now let's run a series of experiments (don't forget to refresh the page):

1.  Check the checkbox using the `attr()` method:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```
2. Now uncheck it with the mouse — the `attr()` value remained unchanged, while the `prop()` value changed
3. Try checking it again using the `attr()` method

{% hint style="info" %}
A brief explanation of what's going on. On the first call to `attr("checked", "checked")`, the checkbox gets checked because both the attribute and the `checked` property are changed. On subsequent calls, nothing happens — only the attribute value changes, and it's already `checked`.
{% endhint %}

Next experiment:

1. Check the checkbox with the mouse
2. Uncheck it — the `attr()` value doesn't change
3.  Try setting the value by calling:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```

> What's interesting in this experiment is this: calling attr("checked", "checked") doesn't work after the user has changed the checkbox state

And one more experiment with the second checkbox:

1.  Remove the check:

    ```javascript
    $("#checkbox").removeAttr("checked")
    ```
2.  Set the check:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```
3. Remove the check again using `attr()`
4. Repeat until you drop

> If it works — don't touch it, you'll break everything with the mouse :)

Compare this with the behavior of `prop()`:

1.  Remove the check:

    ```javascript
    $("#checkbox").prop("checked", false)
    ```
2.  Set the check:

    ```javascript
    $("#checkbox").prop("checked", true)
    ```
3. You can click the checkbox with the mouse and repeat the steps above in any order — everything will work like clockwork

> I hope I've made it clear enough when to use `attr()` and when to use `prop()`

That's not all — we still have the `disabled` property! But don't worry, its behavior is more predictable since the user can't interfere with its state:

1.  Enable the radio button:

    ```javascript
    $("#radio").attr("disabled", false)
    ```
2.  Disable it:

    ```javascript
    $("#radio").attr("disabled", true)
    ```
3. Repeat

Same behavior when using `prop()`:

1.  Enable:

    ```javascript
    $("#radio").prop("disabled", false)
    ```
2.  Disable:

    ```javascript
    $("#radio").prop("disabled", true)
    ```
3. Repeat

> Well, you could use `attr()`, but no!
