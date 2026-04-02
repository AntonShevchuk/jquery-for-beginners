# Манипуляции над элементами

_— Создали мы элемент, и что дальше делать с ним?_\
_— Добавим его на страницу, ведь каждому элементу нужен свой DOM._

Настало время научиться добавлять элементы на страничку. Все необходимые нам методы собраны в одном разделе документации – [Manipulation](https://api.jquery.com/category/manipulation/). С некоторыми из них мы уже познакомились, и осталось совсем чуть-чуть:

<table data-header-hidden><thead><tr><th width="260">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">after(content)
</code></pre></td><td>вставляет контент после каждого элемента из выборки, т.е. если вы встречаете строку <code>$('p').after('&#x3C;hr/>')</code>, читайте её как «<em>после</em> каждого параграфа будет добавлена линия»</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">insertAfter(element)
</code></pre></td><td>вставляет элементы из выборки после каждого элемента переданного в качестве аргумента, т.е. если вы встречаете строку <code>$('&#x3C;hr/>').insertAfter('p')</code> , читайте её как «линия будет <em>добавлена после</em> каждого параграфа»</td></tr></tbody></table>

> _— Хм, а я разницы не увидел!_
>
> _— Тут всё легко, присмотритесь:_
>
> ```javascript
> $("нашли элемент").after("что добавляем после него?")
> $("что добавляем").insertAfter("после какого элемента?")
> ```

Повторим. Берем произвольный текст, и добавляем его после каждого параграфа:

```javascript
$('p').after('(^_^)ノ')
```

Если мы хотим добавить новый DOM-элемент, то можем в качестве параметра передавать строку с HTML и надеятся на то, что jQuery разберётся что с ним делать:

```javascript
$('p').after('<hr/>')
```

Если захотим переместить DOM-элемент, то надо будет его явно «выбрать» с помощью вызова jQuery:

```javascript
$('p').after($('h1'))
```

Если мы так не сделаем, то получим совсем другой результат:

```javascript
$('p').after('h1')
```

Хорошо, метод `after()` рассмотрели, на очереди метод `before()`:

<table data-header-hidden><thead><tr><th width="271">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">before(content)
</code></pre></td><td>вставляет контент перед каждым выбранным элементом</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">insertBefore(element)
</code></pre></td><td>вставляет элементы из выборки перед каждым элементом, переданным в качестве аргумента</td></tr></tbody></table>

> _— Так, ну вроде понятно становится:_
>
> ```javascript
> $("нашли элемент").before("что добавляем перед ним?")
> $("что добавляем").insertBefore("перед каким элементом?")
> ```

{% embed url="https://anton.shevchuk.name/book/code/dom.after-and-before.html" %}

***

Следующие методы берут искомые элементы и вносят в них изменения:

<table data-header-hidden><thead><tr><th width="271">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">append(content)
</code></pre></td><td>вставляет контент в конец каждого элемента из выборки, т.е. строку кода <code>$("p").append("&#x3C;hr/>")</code> следует читать как «в конец каждого параграфа будет добавлена линия»</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">appendTo(element)
</code></pre></td><td>вставляет выбранный контент в конец каждого элемента, переданного в качестве аргумента;<br><code>$("&#x3C;hr/>").appendTo("p")</code> — «линия будет добавлена в конец каждого параграфа»</td></tr></tbody></table>

> _— Опять про разницу:_
>
> ```javascript
> $("нашли элемент").append("что туда дописываем?")
> $("что дописываем").appendTo("в какой элемент?")
> ```

<table data-header-hidden><thead><tr><th width="272">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">prepend(content)
</code></pre></td><td>вставляет контент в начало каждого элемента из выборки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prependTo(element)
</code></pre></td><td>вставляет выбранный контент в начало каждого элемента, переданного в качестве аргумента</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.append-and-prepend.html" %}

***

Так, с этим кусочком документации вроде как разобрались. Опять же, почувствуйте разницу перечисленных методов, ведь дальше будут ещё:

<table data-header-hidden><thead><tr><th width="276">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">replaceWith(content)
</code></pre></td><td>заменяет найденные элементы новым:<br><code>$("p").replaceWith("&#x3C;hr/>")</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">replaceAll(target)
</code></pre></td><td>вставляет контент взамен найденному:<br><code>$("&#x3C;hr/>").replaceAll("h3")</code></td></tr></tbody></table>

> _— А, ну да:_
>
> ```javascript
> $("нашли элемент").replaceWith("на что меняем?")
> $("что вставляем").replaceAll("вместо чего?")
> ```

{% embed url="https://anton.shevchuk.name/book/code/dom.replace.html" %}

***

<table data-header-hidden><thead><tr><th width="271">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">wrap(element)
</code></pre></td><td>оборачивает каждый найденный элемент новым элементом; <br><br>т.е. мы конфеты из коробки заворачиваем в фантики</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">wrapAll(element)
</code></pre></td><td>оборачивает найденные элементы новым элементом;<br><br>мы берём пучок конфет и заворачиваем в один большой фантик</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">wrapInner(element)
</code></pre></td><td>оборачивает контент каждого найденного элемента новым элементом; <br><br>берём конфеты, каждую распаковываем, заворачиваем в свой фантик, и сверху заворачиваем в родной фантик</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">unwrap()
</code></pre></td><td>удаляет родительский элемент у найденных элементов; фантики прочь!</td></tr></tbody></table>

> Я тут конечно могу строить из себя умника, но в действительности, я тоже постоянно подглядываю имена методов, когда работаю с DOM.

{% embed url="https://anton.shevchuk.name/book/code/dom.wrap.html" %}

***

Вот вам домашнее задание – напишите скрипт, который добавит следующий HTML-код на страницу:

```markup
<div id="my">
  <div id="precious">
    <p class="gold">Ring</p>
  </div>
</div>
```
