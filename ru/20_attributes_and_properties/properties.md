# Свойства

Кроме атрибутов также есть свойства элементов, к ним относятся `selectedIndex`, `tagName`, `nodeName`, `nodeType`, `ownerDocument`, `defaultChecked` и `defaultSelected`. Ну, вроде бы, список невелик, можно и запомнить. Для работы со свойствами используем методы из семейства `prop()`:

<table data-header-hidden><thead><tr><th width="305">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propertyName)
</code></pre></td><td>получение значения свойства</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propertyName, value)
</code></pre></td><td>установка значения свойства</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop({
  propertyName1: value,
  propertyName2: value
})
</code></pre></td><td>установка нескольких значений</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propName,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>используя функцию обратного вызова<br><code>index</code> это порядковый номер элемента в выборке<br><code>value</code> — текущее значение атрибута</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeProp(propName)
</code></pre></td><td>удаление свойства (скорей всего, никогда не понадобится)</td></tr></tbody></table>

{% hint style="danger" %}
А теперь выключите музыку, и запомните следующее – **для отключения элементов формы и для проверки/изменения состояния чекбоксов мы всегда используем метод `prop()`,** пусть вас не смущает наличие одноименных атрибутов в HTML (это я про «`disabled`» и «`checked`»), используем `prop()` и точка.
{% endhint %}

Давайте на примере:

{% embed url="https://anton.shevchuk.name/book/code/properties.html" %}

Посмотрите, как работает система без нашего вмешательства — кликните чекбокс, селектбокс, попробуйте отправить форму.

Теперь приступим к серии экспериментов (не забудьте обновить страничку):

1.  Ставим галочку на чекбоксе посредством метода `attr()`:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```
2. Теперь снимите галочку мышкой — значение `attr()` осталось без изменений, значение `prop()` изменилось
3. Попробуйте ещё раз поставить галочку, используя метод `attr()`

{% hint style="info" %}
Небольшое пояснение сути происходящего. При первом вызове метода `attr("checked", "checked")` проставляется галочка, т.к. изменяются и атрибут и свойство `checked`. При повторном вызове уже ничего не происходит, меняется только значение атрибута, которое и так уже`checked`.
{% endhint %}

Следующий эксперимент:

1. Поставьте мышкой галочку на чекбоксе
2. Снимите галочку — значение `attr()` не изменяется
3.  Попробуйте установить значение посредством вызова:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```

> В данном эксперименте интересен следующий момент: вызов метода attr("checked", "checked") не срабатывает после того, как пользователь изменял статус чекбокса

Ну и ещё один эксперимент со вторым чекбоксом:

1.  Удаляем галочку:

    ```javascript
    $("#checkbox").removeAttr("checked")
    ```
2.  Ставим галочку:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```
3. Опять удаляем галочку, используя метод `attr()`
4. Повторяем до упаду&#x20;

> Работает — не трожь, мышкой всё сломаете :)

Сравните с поведением метода `prop()`:

1.  Удаляем галочку:

    ```javascript
    $("#checkbox").prop("checked", false)
    ```
2.  Ставим галочку:

    ```javascript
    $("#checkbox").prop("checked", true)
    ```
3. Можем кликать мышкой по чекбоксу и повторять предыдущие пункты в произвольном порядке, всё будет работать как часы

> Надеюсь, я достаточно наглядно дал понять, когда надо использовать `attr()`, а когда `prop()`

Это ещё не всё, у нас же есть ещё свойство `disabled`! Но не волнуйтесь, его поведение более предсказуемо, т.к. пользователь не может вмешиваться в его состояние:

1.  Включаем радио-кнопку:

    ```javascript
    $("#radio").attr("disabled", false)
    ```
2.  Выключаем:

    ```javascript
    $("#radio").attr("disabled", true)
    ```
3. Повторяем

Аналогичное поведение при использовании метода `prop()`:

1.  Включаем:

    ```javascript
    $("#radio").prop("disabled", false)
    ```
2.  Выключаем:

    ```javascript
    $("#radio").prop("disabled", true)
    ```
3. Повторяем

> Ну, как бы, можно использовать `attr()`, но нет!
