# Властивості

Крім атрибутів також є властивості елементів, до них відносяться `selectedIndex`, `tagName`, `nodeName`, `nodeType`, `ownerDocument`, `defaultChecked` та `defaultSelected`. Ну, наче б, список невеликий, можна й запам'ятати. Для роботи з властивостями використовуємо методи з родини `prop()`:

<table data-header-hidden><thead><tr><th width="305">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propertyName)
</code></pre></td><td>отримання значення властивості</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propertyName, value)
</code></pre></td><td>встановлення значення властивості</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop({
  propertyName1: value,
  propertyName2: value
})
</code></pre></td><td>встановлення кількох значень</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prop(propName,
  function(index, value) {
    return value
  }
)
</code></pre></td><td>використовуючи функцію зворотного виклику<br><code>index</code> це порядковий номер елемента у вибірці<br><code>value</code> — поточне значення атрибута</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeProp(propName)
</code></pre></td><td>видалення властивості (швидше за все, ніколи не знадобиться)</td></tr></tbody></table>

{% hint style="danger" %}
А тепер вимкніть музику, і запам'ятайте наступне – **для вимикання елементів форми та для перевірки/зміни стану чекбоксів ми завжди використовуємо метод `prop()`,** хай вас не бентежить наявність однойменних атрибутів у HTML (це я про «`disabled`» та «`checked`»), використовуємо `prop()` і крапка.
{% endhint %}

Давайте на прикладі:

{% embed url="https://anton.shevchuk.name/book/code/properties.html" %}

Подивіться, як працює система без нашого втручання — клікніть чекбокс, селектбокс, спробуйте відправити форму.

Тепер приступимо до серії експериментів (не забудьте оновити сторінку):

1.  Ставимо галочку на чекбоксі за допомогою методу `attr()`:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```
2. Тепер зніміть галочку мишкою — значення `attr()` залишилося без змін, значення `prop()` змінилося
3. Спробуйте ще раз поставити галочку, використовуючи метод `attr()`

{% hint style="info" %}
Невелике пояснення суті того, що відбувається. При першому виклику методу `attr("checked", "checked")` проставляється галочка, оскільки змінюються і атрибут і властивість `checked`. При повторному виклику вже нічого не відбувається, змінюється лише значення атрибута, яке й так вже `checked`.
{% endhint %}

Наступний експеримент:

1. Поставте мишкою галочку на чекбоксі
2. Зніміть галочку — значення `attr()` не змінюється
3.  Спробуйте встановити значення за допомогою виклику:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```

> У цьому експерименті цікавий наступний момент: виклик методу attr("checked", "checked") не спрацьовує після того, як користувач змінював статус чекбокса

Ну і ще один експеримент з другим чекбоксом:

1.  Видаляємо галочку:

    ```javascript
    $("#checkbox").removeAttr("checked")
    ```
2.  Ставимо галочку:

    ```javascript
    $("#checkbox").attr("checked", "checked")
    ```
3. Знову видаляємо галочку, використовуючи метод `attr()`
4. Повторюємо до знемоги

> Працює — не чіпай, мишкою все зламаєте :)

Порівняйте з поведінкою методу `prop()`:

1.  Видаляємо галочку:

    ```javascript
    $("#checkbox").prop("checked", false)
    ```
2.  Ставимо галочку:

    ```javascript
    $("#checkbox").prop("checked", true)
    ```
3. Можемо клацати мишкою по чекбоксу та повторювати попередні пункти в довільному порядку, усе буде працювати як годинник

> Сподіваюся, я достатньо наочно дав зрозуміти, коли треба використовувати `attr()`, а коли `prop()`

Це ще не все, у нас же є ще властивість `disabled`! Але не хвилюйтеся, його поведінка більш передбачувана, оскільки користувач не може втручатися в його стан:

1.  Вмикаємо радіо-кнопку:

    ```javascript
    $("#radio").attr("disabled", false)
    ```
2.  Вимикаємо:

    ```javascript
    $("#radio").attr("disabled", true)
    ```
3. Повторюємо

Аналогічна поведінка при використанні методу `prop()`:

1.  Вмикаємо:

    ```javascript
    $("#radio").prop("disabled", false)
    ```
2.  Вимикаємо:

    ```javascript
    $("#radio").prop("disabled", true)
    ```
3. Повторюємо

> Ну, як би, можна використовувати `attr()`, але ні!
