# Клонирование и удаление

Пока вокруг клонирования ведутся дискуссии, у программистов всё уже на конвейере :)

Для клонирования элемента есть метод `clone()`,  он создаст для вас клон элемента, и вы сможете его вставить в DOM используя любой из [подходящих методов](manipulation.md).

Мало того, если у вас там были обработчики событий, то их также можно клонировать, достаточно лишь выставить первый аргумент в `true`:

```javascript
$("h1").click(() => alert("h1!"))

// клон без обработчиков событий
$("h1").clone()

// клон с обработчиком событий
$("h1").clone(true)
```

<table data-header-hidden><thead><tr><th width="326">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">clone()
</code></pre></td><td>клонирует выбранные элементы, для дальнейшей вставки копий назад в DOM</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">clone(
  withDataAndEvents = true
)
</code></pre></td><td>клонирует выбранные элементы, вместе с текущими обработчиками событий и данными <code>data()</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">clone(
  withDataAndEvents = true,
  deepWithDataAndEvents = true
)
</code></pre></td><td>клонирует также обработчики событий и данные дочерних элементов</td></tr></tbody></table>

Кроме клонирования, вы можете изъять из DOM любой элемент, чтобы изменить и вставить назад, или вправе даже удалить его:

<table data-header-hidden><thead><tr><th width="326">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">detach()
</code></pre></td><td>изымает элемент из DOM, но при этом сохраняет все данные о нём в jQuery; следует использовать, если надо лишь временно удалить элемент</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">empty()
</code></pre></td><td>удаляет текст и дочерние DOM-элементы</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">remove()
</code></pre></td><td>насовсем удаляет элемент из DOM</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.clone-and-other.html" %}
