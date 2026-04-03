# Клонування та видалення

Поки навколо клонування ведуться дискусії, у програмістів все вже на конвеєрі :)

Для клонування елемента є метод `clone()`, він створить для тебе клон елемента, і ти зможеш його вставити в DOM використовуючи будь-який з [відповідних методів](manipulation.md).

Мало того, якщо у тебе там були обробники подій, то їх також можна клонувати, достатньо лише виставити перший аргумент в `true`:

```javascript
$("h1").click(() => alert("h1!"))

// клон без обробників подій
$("h1").clone()

// клон з обробником подій
$("h1").clone(true)
```

<table data-header-hidden><thead><tr><th width="326">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">clone()
</code></pre></td><td>клонує вибрані елементи, для подальшої вставки копій назад в DOM</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">clone(
  withDataAndEvents = true
)
</code></pre></td><td>клонує вибрані елементи, разом з поточними обробниками подій та даними <code>data()</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">clone(
  withDataAndEvents = true,
  deepWithDataAndEvents = true
)
</code></pre></td><td>клонує також обробники подій та дані дочірніх елементів</td></tr></tbody></table>

Окрім клонування, ти можеш вилучити з DOM будь-який елемент, щоб змінити та вставити назад, або маєш право навіть видалити його:

<table data-header-hidden><thead><tr><th width="326">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">detach()
</code></pre></td><td>вилучає елемент з DOM, але при цьому зберігає всі дані про нього в jQuery; слід використовувати, якщо треба лише тимчасово видалити елемент</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">empty()
</code></pre></td><td>видаляє текст та дочірні DOM-елементи</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">remove()
</code></pre></td><td>назовсім видаляє елемент з DOM</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.clone-and-other.html" %}
