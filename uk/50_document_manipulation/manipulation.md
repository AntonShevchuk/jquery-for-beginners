# Маніпуляції над елементами

_— Створили ми елемент, і що далі робити з ним?_\
_— Додамо його на сторінку, адже кожному елементу потрібен свій DOM._

Настав час навчитися додавати елементи на сторінку. Всі необхідні нам методи зібрані в одному розділі документації — [Manipulation](https://api.jquery.com/category/manipulation/). З деякими з них ми вже познайомилися, і залишилося зовсім трішки:

<table data-header-hidden><thead><tr><th width="260">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">after(content)
</code></pre></td><td>вставляє контент після кожного елемента з вибірки, тобто якщо ви зустрічаєте рядок <code>$('p').after('&#x3C;hr/>')</code>, читайте його як «<em>після</em> кожного параграфа буде додано лінію»</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">insertAfter(element)
</code></pre></td><td>вставляє елементи з вибірки після кожного елемента переданого як аргумент, тобто якщо ви зустрічаєте рядок <code>$('&#x3C;hr/>').insertAfter('p')</code> , читайте його як «лінія буде <em>додана після</em> кожного параграфа»</td></tr></tbody></table>

> _— Хм, а я різниці не побачив!_
>
> _— Тут все легко, придивися:_
>
> ```javascript
> $("знайшли елемент").after("що додаємо після нього?")
> $("що додаємо").insertAfter("після якого елемента?")
> ```

Повторимо. Беремо довільний текст і додаємо його після кожного параграфа:

```javascript
$('p').after('(^_^)ノ')
```

Якщо ми хочемо додати новий DOM-елемент, то можемо як параметр передавати рядок з HTML і сподіватися на те, що jQuery розбереться що з ним робити:

```javascript
$('p').after('<hr/>')
```

Якщо захочемо перемістити DOM-елемент, то треба буде його явно «вибрати» за допомогою виклику jQuery:

```javascript
$('p').after($('h1'))
```

Якщо ми так не зробимо, то отримаємо зовсім інший результат:

```javascript
$('p').after('h1')
```

Добре, метод `after()` розглянули, на черзі метод `before()`:

<table data-header-hidden><thead><tr><th width="271">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">before(content)
</code></pre></td><td>вставляє контент перед кожним вибраним елементом</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">insertBefore(element)
</code></pre></td><td>вставляє елементи з вибірки перед кожним елементом, переданим як аргумент</td></tr></tbody></table>

> _— Так, ну наче зрозуміло стає:_
>
> ```javascript
> $("знайшли елемент").before("що додаємо перед ним?")
> $("що додаємо").insertBefore("перед яким елементом?")
> ```

{% embed url="https://anton.shevchuk.name/book/code/dom.after-and-before.html" %}

***

Наступні методи беруть шукані елементи та вносять до них зміни:

<table data-header-hidden><thead><tr><th width="271">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">append(content)
</code></pre></td><td>вставляє контент в кінець кожного елемента з вибірки, тобто рядок коду <code>$("p").append("&#x3C;hr/>")</code> слід читати як «в кінець кожного параграфа буде додано лінію»</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">appendTo(element)
</code></pre></td><td>вставляє вибраний контент в кінець кожного елемента, переданого як аргумент;<br><code>$("&#x3C;hr/>").appendTo("p")</code> — «лінія буде додана в кінець кожного параграфа»</td></tr></tbody></table>

> _— Знову про різницю:_
>
> ```javascript
> $("знайшли елемент").append("що туди дописуємо?")
> $("що дописуємо").appendTo("в який елемент?")
> ```

<table data-header-hidden><thead><tr><th width="272">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">prepend(content)
</code></pre></td><td>вставляє контент на початок кожного елемента з вибірки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prependTo(element)
</code></pre></td><td>вставляє вибраний контент на початок кожного елемента, переданого як аргумент</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.append-and-prepend.html" %}

***

Так, з цим шматочком документації наче розібралися. Знову ж таки, відчуйте різницю перерахованих методів, адже далі будуть ще:

<table data-header-hidden><thead><tr><th width="276">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">replaceWith(content)
</code></pre></td><td>замінює знайдені елементи новим:<br><code>$("p").replaceWith("&#x3C;hr/>")</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">replaceAll(target)
</code></pre></td><td>вставляє контент замість знайденого:<br><code>$("&#x3C;hr/>").replaceAll("h3")</code></td></tr></tbody></table>

> _— А, ну так:_
>
> ```javascript
> $("знайшли елемент").replaceWith("на що міняємо?")
> $("що вставляємо").replaceAll("замість чого?")
> ```

{% embed url="https://anton.shevchuk.name/book/code/dom.replace.html" %}

***

<table data-header-hidden><thead><tr><th width="271">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">wrap(element)
</code></pre></td><td>обгортає кожен знайдений елемент новим елементом; <br><br>тобто ми цукерки з коробки загортаємо у фантики</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">wrapAll(element)
</code></pre></td><td>обгортає знайдені елементи новим елементом;<br><br>ми беремо пучок цукерок і загортаємо в один великий фантик</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">wrapInner(element)
</code></pre></td><td>обгортає контент кожного знайденого елемента новим елементом; <br><br>беремо цукерки, кожну розпаковуємо, загортаємо у свій фантик, і зверху загортаємо в рідний фантик</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">unwrap()
</code></pre></td><td>видаляє батьківський елемент у знайдених елементів; фантики геть!</td></tr></tbody></table>

> Я тут, звісно, можу будувати із себе розумника, але насправді я теж постійно підглядаю імена методів, коли працюю з DOM.

{% embed url="https://anton.shevchuk.name/book/code/dom.wrap.html" %}

***

Ось вам домашнє завдання — напишіть скрипт, який додасть наступний HTML-код на сторінку:

```markup
<div id="my">
  <div id="precious">
    <p class="gold">Ring</p>
  </div>
</div>
```
