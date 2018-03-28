## Манипуляции над DOM-элементами

_— Создали мы элемент, и что дальше делать с ним?_   
_— Добавим его на страницу, ведь каждому элементу нужен свой DOM._
 
Настало время научиться добавлять элементы на страничку. Все необходимые нам методы собраны в одном разделе документации – [Manipulation](http://api.jquery.com/category/manipulation/). С некоторыми из них мы уже познакомились, и осталось совсем чуть-чуть:

`after(content)` — вставляет контент после каждого элемента из выборки, т.е. если вы встречаете строку {% jqbScript "#html-example", code="$('p').after('<code>hr/</code>')" %}$("p").after("&lt;hr/&gt;"){% endjqbScript %}, читайте её как «после каждого параграфа будет добавлена линия»

`insertAfter(element)` — вставляет элементы из выборки после каждого элемента переданного в качестве аргумента, т.е. если вы встречаете строку {% jqbScript "#html-example", code="$('<code>hr/</code>').insertAfter('p')" %}$("&lt;hr/&gt;").insertAfter("p"){% endjqbScript %} – читайте её как «линия будет добавлена после каждого параграфа»

> _— Хм, а я разницы не увидел! — тут всё легко, присмотритесь:_
  ```javascript
$("нашли элемент").after("что добавляем после него?")
$("что добавляем").insertAfter("после какого элемента?")
```

{% jqbFrame "html-example", "../code/css.selectors.html", height="700px" %}
{% sticky %}
{% reload %}
{% endjqbFrame %}

Повторим. Берём произвольный текст, и добавляем его после каждого параграфа:

{% jqbRun "#html-example" %}{% endjqbRun %}
```javascript
$('p').after('(^_^)ノ');
```

Если мы хотим добавить новый DOM-элемент, то можем в качестве параметра передавать строку с HTML и надеятся на то, что jQuery разберётся что с ним делать: 

{% jqbRun "#html-example", code="$('p').after('<code>hr/</code>')" %}{% endjqbRun %}
```javascript
$('p').after('<hr/>');
```

Если захотим переместить DOM-элемент, то надо будет его явно «выбрать» с помощью вызова jQuery:

{% jqbRun "#html-example" %}{% endjqbRun %}
```javascript
$('p').after($('h1'));
```

Если мы так не сделаем, то получим совсем другой результат:

{% jqbRun "#html-example" %}{% endjqbRun %}
```javascript
$('p').after('h1');
```

`before(content)` — вставляет контент перед каждым выбранным элементом

`insertBefore(element)` — вставляет элементы из выборки перед каждым элементом, переданным в качестве аргумента

`append(content)` — вставляет контент в конец каждого элемента из выборки, т.е. строку кода {% jqbScript "#html-example", code="$('p code:odd').before('<code>hr/</code>')" %}$("p").append("&lt;hr/&gt;"){% endjqbScript %} следует читать как «в конец каждого параграфа будет добавлена линия»

`appendTo(element)` — вставляет выбранный контент в конец каждого элемента, переданного в качестве аргумента: {% jqbScript "#html-example", code="$('p code:odd').before('<code>hr/</code>')" %}$("&lt;hr/&gt;").appendTo("p"){% endjqbScript %} — «линия будет добавлена в конец каждого параграфа»

> _Опять про разницу:_
  ```javascript
$("нашли элемент").append("что туда дописываем?")
$("что дописываем").appendTo("в какой элемент?")
```

`prepend(content)` — вставляет контент в начало каждого элемента из выборки

`prependTo(element)` — вставляет выбранный контент в начало каждого элемента, переданного в качестве аргумента

Так, с этим кусочком документации вроде как разобрались. Опять же, почувствуйте разницу перечисленных методов, ведь дальше будут ещё:

`replaceWith(content)` – заменяет найденные элементы новым: {% jqbScript "#html-example", code="$('p').replaceWith('<code>hr/</code>')" %}$("p").replaceWith("&lt;hr/&gt;"){% endjqbScript %}

`replaceAll(target)` – вставляет контент взамен найденному: {% jqbScript "#html-example", code="$('<code>hr/</code>').replaceAll('h3')" %}$("&lt;hr/&gt;").replaceAll("h3"){% endjqbScript %}
  ```javascript
$("нашли элемент").replaceWith("на что меняем?")
$("что вставляем").replaceAll("вместо чего?")
```

`wrap(element)` – оборачивает каждый найденный элемент новым элементом; т.е. мы конфеты из коробки заворачиваем в фантики

`wrapAll(element)` – оборачивает найденные элементы новым элементом; мы берём пучок конфет и заворачиваем в один большой фантик

`wrapInner(element)` – оборачивает контент каждого найденного элемента новым элементом; берём конфеты, каждую распаковываем, заворачиваем в свой фантик, и сверху заворачиваем в родной фантик

`unwrap()` – удаляет родительский элемент у найденных элементов; фантики прочь!

`clone(withDataAndEvents)` – клонирует выбранные элементы, для дальнейшей вставки копий назад в DOM, позволяет так же копировать и обработчики событий

`detach()` – удаляет элемент из DOM, но при этом сохраняет все данные о нём в jQuery; следует использовать, если надо лишь временно удалить элемент

`empty()` – удаляет текст и дочерние DOM-элементы

`remove()` – насовсем удаляет элемент из DOM

`html()` – возвращает HTML заданного элемента

`html(newHtml)` – заменяет HTML в заданном элементе

`text()` – возвращает текст заданного элемента; если внутри элемента будут другие HTML-теги, то вернётся сборная солянка из текста всех элементов

`text(newText)` – заменяет текст внутри выбранных элементов, при попытке вставить таким образом HTML, будет получен текст, где тэги будут приведены к [HTML entities](http://ru.wikipedia.org/wiki/%D0%9C%D0%BD%D0%B5%D0%BC%D0%BE%D0%BD%D0%B8%D0%BA%D0%B8_%D0%B2_HTML):

{% jqbRun "#html-example" %}{% endjqbRun %}

  ```javascript
$("section").text("Some <strong>text</strong>");

// Some &lt;strong&gt;text&lt;/strong&gt;
```

Вот вам домашнее задание – напишите скрипт, который добавит следующий HTML-код на страницу:

```html
<div id="my">
  <div id="precious">
    <p class="gold">Ring</p>
  </div>
</div>
```