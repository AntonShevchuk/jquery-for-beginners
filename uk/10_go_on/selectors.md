# Селектори

Як я вже казав раніше, у пошуку елементів на сторінці полягає практично половина успішної роботи з jQuery. Тож приступимо до пошуків по документу.

### Ідентифікатор та класи

Вибір елементів по `id` або імені класу, аналогічно до використовуваних у CSS:

<table data-header-hidden><thead><tr><th width="303">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("#content")</code></td><td>обираємо елемент з <code>id="content"</code></td></tr><tr><td><code>$("section#content")</code></td><td>обираємо <code>&#x3C;section></code> з <code>id="content"</code></td></tr><tr><td><code>$(".intro")</code></td><td>обираємо елементи з <code>class="intro"</code></td></tr><tr><td><code>$(".intro.pinned")</code></td><td>обираємо елементи з класами <code>intro</code> та <code>pinned</code></td></tr><tr><td><code>$("h3")</code></td><td>обираємо всі теги <code>&#x3C;h3></code></td></tr><tr><td><code>$("h1, h2")</code></td><td>обираємо всі теги <code>&#x3C;h1></code> та <code>&#x3C;h2></code></td></tr></tbody></table>

{% hint style="info" %}
Використовуйте валідні імена класів та ідентифікаторів
{% endhint %}

### Дочірні елементи

Тепер згадаємо, що ми в DOM не одні, це-таки ієрархічна структура:

<table data-header-hidden><thead><tr><th width="307">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("article h3")</code></td><td>обираємо всі теги <code>&#x3C;h3></code> всередині тегу <code>&#x3C;article></code></td></tr><tr><td><code>$("article").find("h3")</code></td><td>аналогічно до прикладу вище</td></tr><tr><td><code>$("section article h3")</code></td><td>обираємо всі теги <code>&#x3C;h3></code> всередині тегу <code>&#x3C;article></code>, які знаходяться всередині тегу <code>&#x3C;section></code>, <em>в DOM який побудував Джек</em></td></tr><tr><td><code>$("section")</code><br>  <code>.find("article")</code><br>  <code>.find("h3")</code></td><td>і ще раз, але на інший лад, і під капотом працює інакше, але ми про це ще поговоримо</td></tr></tbody></table>

### Сусідні елементи

У нас є сусіди, і з ними в нас налагоджений контакт. Ось вам кілька способів як їх знайти:

<table data-header-hidden><thead><tr><th width="309">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("article + article")</code></td><td>вибір усіх елементів <code>&#x3C;article></code>, перед якими є тег <code>&#x3C;article></code></td></tr><tr><td><code>$("#stick ~ article")</code></td><td>вибір усіх елементів <code>&#x3C;article></code> після елемента з <code>id="stick"</code></td></tr><tr><td><code>$("#stick").next()</code></td><td>вибір наступного елемента після елемента з <code>id="stick"</code></td></tr></tbody></table>

### Нащадки та батьки

Родинні зв'язки вирішують:

<table data-header-hidden><thead><tr><th width="306">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("article > h3")</code></td><td>обираємо всі теги <code>&#x3C;h3></code>, які є безпосередніми нащадками тегу <code>&#x3C;article></code></td></tr><tr><td><code>$("article > *")</code></td><td>вибір усіх нащадків елементів <code>&#x3C;article></code></td></tr><tr><td><code>$("article").children()</code></td><td>аналогічно до прикладу вище</td></tr><tr><td><code>$("p").parent()</code></td><td>вибір усіх прямих предків елементів <code>&#x3C;p></code></td></tr><tr><td><code>$("p").parents()</code></td><td>вибір усіх предків елементів <code>&#x3C;p></code><br>досить екзотичне завдання</td></tr><tr><td><code>$("p").parents("section")</code></td><td>вибір усіх предків елемента <code>&#x3C;p></code>, які є <code>&#x3C;section></code> (<code>parents()</code> приймає як параметр селектор)</td></tr></tbody></table>

{% hint style="info" %}
`$("*")` – вибір усіх елементів; слід використовувати з величезною обережністю
{% endhint %}

### Пошук за атрибутами

Ще з часів CSS2 була можливість знайти елемент з певними атрибутами, у [CSS3](https://www.w3.org/TR/selectors-3/) розширили можливості [пошуку за атрибутами](https://www.w3.org/TR/selectors-3/#attribute-selectors):

<table data-header-hidden><thead><tr><th width="306">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("a[href]")</code></td><td>усі посилання з атрибутом <code>href</code></td></tr><tr><td><code>$("a[href=#]")</code></td><td>усі посилання з <code>href=#</code></td></tr><tr><td><code>$("a[href~=#]")</code></td><td>усі посилання, у яких <code>#</code> є одним зі слів у <code>href</code></td></tr><tr><td><code>$("a[hreflang|=en]")</code></td><td><p>усі посилання зі словом <code>en</code> у <code>hreflang</code> </p><p>символ <code>-</code>  сприймається як роздільник для слів:  <code>en</code>, <code>en-US</code>, <code>en-UK</code></p></td></tr><tr><td><code>$("a[href^=https]")</code></td><td>посилання, що починаються з <code>https</code></td></tr><tr><td><code>$("a[href*='google.com']")</code></td><td>посилання на «погуглити»</td></tr><tr><td><code>$("a[href$=.pdf]")</code></td><td>посилання на PDF-файли (за ідеєю)</td></tr></tbody></table>

{% hint style="info" %}
Якщо ви вирішите зазирнути всередину jQuery, то ви, швидше за все, знайдете те саме місце, де ваш вираз буде аналізуватися за допомогою регулярних виразів, тому в селекторах необхідно екранувати спеціальні символи, використовуючи подвійний зворотний слеш «`\\`»:

```javascript
$("a[href^=\\/]").addClass("internal");
```
{% endhint %}

### Структурні псевдокласи

Хотілося б ще звернути увагу на [структурні псевдокласи](https://www.w3.org/TR/selectors-3/#structural-pseudos) зі специфікації [CSS3](https://www.w3.org/TR/selectors-3/), там багато цікавих і корисних, наприклад:

<table data-header-hidden><thead><tr><th width="321">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("ul li:first-child")</code></td><td>перший дочірній елемент</td></tr><tr><td><code>$("ul li:last-child")</code></td><td>останній дочірній елемент</td></tr><tr><td><code>$("ul li:nth-child(2n+1)")</code></td><td><p>вибірка елементів за нескладним рівнянням </p><p>детальніше можна прочитати у статті «<a href="https://web-standards.ru/articles/nth-child/">Як працює nth-child</a>»</p></td></tr></tbody></table>

### Псевдоклас заперечення

Псевдоклас заперечення `:not()` єдиний у своєму роді, він дозволяє обрати всі елементи, що не підпадають під вкладену вибірку у дужках

<table data-header-hidden><thead><tr><th width="321">селектор</th><th></th></tr></thead><tbody><tr><td><code>$("a:not(.active)")</code></td><td>усі посилання <code>&#x3C;a></code> без класу <code>active</code></td></tr><tr><td><code>$("a").not(".active")</code></td><td>аналогічний результат, використовуючи метод <code>.not()</code></td></tr></tbody></table>

> Якщо хочете пограти із селекторами від душі, то відкрийте сторінку [css.selectors.html](https://anton.shevchuk.name/book/code/css.selectors.html) у новій вкладці, і там ви зможете потренуватися у написанні селекторів, використовуючи меню справа

### Результати «виборів»

Коли за допомогою перелічених запитів ви знайшли (або не знайшли) DOM-елементи, вам повернеться jQuery-об'єкт, який міститиме масив цих елементів. Ось так це буде виглядати для запиту

:

![jQuery length](../../.gitbook/assets/jquery-length.png)

Можливо, ви помітили властивість `length`. Так-так, саме так, це кількість знайдених елементів. Тож ми можемо легко отримати це число за допомогою наступного коду:

```javascript
alert( $("p").length )
```

Якщо перед вами стоїть завдання дістати знайдений DOM-елемент, то ви зможете це зробити, знаючи його індекс. По суті, це виглядає як звернення до елемента масиву:

```javascript
// ми шукаємо всі параграфи
// беремо перший з них
// беремо текст параграфа
// повертаємо довжину тексту
alert( $("p")[0].innerText.length )
```

Якщо вам не подобається цей спосіб з естетичних міркувань, то ви можете скористатися методом `.get()`:

```javascript
alert( $("p").get(0).innerText.length )
```
