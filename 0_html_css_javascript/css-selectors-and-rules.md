# CSS-правила и селекторы

Теперь приступим к CSS, и начнём, пожалуй, с расшифровки аббревиатуры CSS. Это Cascading Style Sheets, дословно «каскадная таблица стилей», но:

_— Почему же она называется каскадной?_ — этот вопрос я часто задаю на собеседованиях претендентам. Ответом же будет аналогия, ибо она незыблема как перпендикулярная лягушка: представьте себе каскад водопада, вот вы стоите на одной из ступенек каскада с чернильницей в руках, и выливаете её содержимое в воду — вся вода ниже по каскаду будет окрашиваться в цвет чернил. Так и в CSS: если вы задаёте стиль на определённом уровне, он распространяется вниз по "каскаду" к элементам веб-страницы, окрашивая их в "цвет" вашего стиля.&#x20;

_— Зачем мне всё это?_ — работая с jQuery, вы должны «на отлично» читать правила CSS, а также уметь составлять CSS-селекторы для поиска необходимых элементов на странице. Практически все задачи, которые вы будете решать с помощью jQuery, начинаются с поиска необходимого элемента на странице, так что **знание CSS-селекторов обязательно**.

Но давайте обо всём по порядку, возьмём следующий простенький пример вполне семантического HTML:

{% code fullWidth="false" %}
```html
<!DOCTYPE html>
<html dir="ltr" lang="en-US">
<head>
  <meta charset="UTF-8"/>
  <title>Page Title</title>
  <link rel="shortcut icon" href="/favicon.ico"/>
  <style>
    body {
      font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
      font-size: 14px;
    }
    h1, h2, h3 {
      color: #333333;
    }
    header, section, footer {
      position: relative;
      max-width: 800px;
      margin: 16px auto;
    }
    article {
      padding: 16px;
      margin-bottom: 16px;
    }
    #content {
      padding-bottom: 16px;
    }
    .box {
      border:1px solid #ccc;
      border-radius:4px;
      box-shadow:0 0 2px #ccc;
    }
  </style>
</head>
<body>
  <header>
    <h1>Page Title</h1>
    <p>Page Description</p>
  </header>
  <section id="content">
    <h2>Section Title</h2>
    <article class="box">
      <h3>Article Title</h3>
      <p>Lorem ipsum dolor sit amet, consectetuer adipiscing...</p>
    </article>
    <article class="box">
      <h3>Article Title</h3>
      <p>Morbi malesuada, ante at feugiat tincidunt...</p>
    </article>
  </section>
  <footer>
      © copyright 2018
  </footer>
</body>
</html>
```
{% endcode %}

Это пример простого и правильного HTML с небольшим добавлением CSS. Давайте разберём селекторы в приведённом CSS-коде (я умышленно не выносил CSS в отдельный файл, ибо так наглядней):

* `body` — данные правила будут применены к тегу `<body>` и всем его потомкам, запомните: настройки шрифтов распространяются вниз по иерархии элементов
* `h1,h2,h3` — мы выбираем теги `<h1>`, `<h2>` и `<h3>`, и устанавливаем цвет шрифта для данных тегов и их потомков
* `#content` — выбираем элемент с `id="content"`, настройки отступов не распространяются на потомков, они будут изменяться только для данного элемента
* `.box` — выбираем элементы с `class="box"`, и изменяем внешний вид границ элементов с заданным классом

Теперь подробнее и с усложнёнными примерами:

<table><thead><tr><th width="215">селектор</th><th></th></tr></thead><tbody><tr><td><code>h1</code></td><td>ищем элементы по имени тега</td></tr><tr><td><code>#container</code></td><td>ищем элемент по идентификатору <code>id=container</code> (<strong>идентификатор уникален</strong>, значит, на странице он должен быть только один)</td></tr><tr><td><code>div#container</code></td><td>ищем <code>&#x3C;div></code> c идентификатором <code>container</code>, но предыдущий селектор работает быстрее, а этот важнее</td></tr><tr><td><code>.news</code></td><td>выбираем элементы по имени класса <code>class="news"</code></td></tr><tr><td><code>div.news</code></td><td>все элементы <code>&#x3C;div></code> c классом <code>news</code> </td></tr><tr><td><code>#wrap .post</code></td><td>ищем все элементы с классом <code>class="post"</code> внутри элемента с <code>id="wrap"</code></td></tr><tr><td><code>.cls1.cls2</code></td><td>выбираем элементы с двумя классами <code>class="cls1 cls2"</code></td></tr><tr><td><code>h1, h2, .posts</code></td><td>перечисление селекторов, выберем всё перечисленное</td></tr><tr><td><code>.post > h2</code></td><td>выбираем элементы <code>&#x3C;h2></code>, которые являются непосредственными потомками элемента с классом «post»</td></tr><tr><td><code>a + span</code></td><td>будут выбраны все элементы <code>&#x3C;span></code> следующие сразу за элементом <code>&#x3C;a></code></td></tr><tr><td><code>a[href^=https]</code></td><td>будут выбраны все элементы <code>&#x3C;a></code> у которых атрибут <code>href</code> начинается с <code>https</code> (предположительно, все внешние ссылки)</td></tr></tbody></table>

> Это отнюдь не весь список, описание же всех CSS-селекторов можно найти на соответствующей страничке W3C: [https://www.w3.org/TR/selectors-3/](https://www.w3.org/TR/selectors-3/)

Возвращаясь к нашей аналогии с водопадом, представьте, что на следующей ступеньке кто-то выльет чернила другого цвета, мы же в результате получим смешение цветов. Но это в жизни, а в CSS это изменение "перекрасит воду" на всех последующих ступенях, ведь в CSS важен порядок, порядок и приоритеты. Давайте перейдём к сути:

1.  первая ступенька нашего каскада имеет самый низкий приоритет, это стили браузера по умолчанию

    > как бы не заявляли разработчики, но в разных браузерах эти стили могут отличаться, поэтому всё ещё существует такое явление как [CSS Reset](https://www.google.com/search?q=CSS+Reset), такой себе уравнитель
2. на второй ступеньке стили, заданные пользователем в недрах настроек браузера; на практике встречаются редко
3. самые важные для нас — стили автора странички, но и тут всё идёт по порядку
   *   самый низкий приоритет у стилей, что подключены как файл `<link rel="stylesheet" type="text/css" href="...">` или встроены внутрь HTML с помощью тега `<style>`:

       ```html
       <link rel="stylesheet" type="text/css" href="styles.css">
       <style>
         p {
           color: red;
         }
       </style>
       <p>
         Этот параграф будет красным
       </p>
       ```

       > и естественно, при равенстве приоритетов «тапки» у того, кто объявлен последним
   *   потом те, что захардкодили плохие люди (не вы, вы так делать не будете) в `style=""`:

       ```html
       <p style="color: blue">
         Этот параграф будет синим
       </p>
       ```
   *   и тут появляется метка `!important`, и всё нам ломает, т.к. правило с такой меткой становится важнее даже inline-стилей, и всё идёт вверх-дном:

       ```html
       <style>
         p {
           color: red !important;
         }
       </style>
       <p style="color: blue">
         Этот параграф всё равное будет красным!
       </p>
       ```
   *   чтобы перебить такой стиль, остаётся только вариант прописывать inline-стили с меткой `!important`:&#x20;

       ```html
       <style>
         p {
           color: red !important;
         }
       </style>
       <p style="color: blue !important">
         Этот параграф будет синим!
       </p>
       ```
4. приключения с `!important` не закончились, если есть пользовательские стили с данной меткой, то теперь их выход
5. и даже браузеры грешат с  `!important`, и такие стили имеют практически максимальный приоритет

{% hint style="info" %}
Есть ещё приоритеты у правил которые используются для [CSS анимации](https://developer.mozilla.org/ru/docs/Web/CSS/CSS\_animations/Using\_CSS\_animations) и [CSS переходов](https://developer.mozilla.org/ru/docs/Web/CSS/CSS\_transitions/Using\_CSS\_transitions), но это уже совсем другая история
{% endhint %}

Если голова ещё не болит, то я также упомяну, что при расчёте, чьи же правила главней, анализируется [специфичность](https://developer.mozilla.org/ru/docs/Web/CSS/Specificity) селекторов, и тут считается следующим образом:

* расчёт происходит по трём весовым позициям — `[0:0:0]`
* за каждый идентификатор элемента (`#id`) — `[1:0:0]`
* за каждый класс (`.class`), либо псевдо-класс (`:pseudo`) — `[0:1:0]`
* за каждый тег (`<a>`, `<div>`, и т.д.) — `[0:0:1]`
* при этом `[1:0:0]` > `[0:x:y]` > `[0:0:x]`&#x20;
* при равенстве счета — снова «тапки» у того, кто объявлен последним

Пример селекторов, выстроенных по возрастанию приоритета (соответствующий код HTML будет ниже):

<table><thead><tr><th width="608">селектор</th><th>приоритет</th></tr></thead><tbody><tr><td>тег имеет наименьший приоритет</td><td><code>[0:0:1]</code></td></tr><tr><td><code>p { color: orange }</code></td><td></td></tr><tr><td>добавляем к тегу класс <code>intro</code></td><td><code>[0:1:1]</code></td></tr><tr><td><code>p.intro { color: green }</code></td><td></td></tr><tr><td>добавляем ещё тег</td><td><code>[0:1:2]</code></td></tr><tr><td><code>article p.intro { color: blue }</code></td><td></td></tr><tr><td>... нам нужно больше классов</td><td><code>[0:2:2]</code></td></tr><tr><td><code>article.news p.intro { color: red }</code></td><td></td></tr><tr><td>идентификатор <code>id="pinned"</code> даже сам по себе важней всех тегов и классов вместе взятых</td><td><code>[1:0:0]</code></td></tr><tr><td><code>#pinned { color: darkblue }</code></td><td></td></tr><tr><td>добавляем тег <code>&#x3C;p></code>, и специфичность увеличивается</td><td><code>[1:0:1]</code></td></tr><tr><td><code>p#pinned { color: darkcyan }</code></td><td></td></tr><tr><td>добавляем ещё один идентификатор <code>id="top"</code></td><td><code>[2:0:1]</code></td></tr><tr><td><code>#top p#pinned { color: darkgreen }</code></td><td></td></tr><tr><td>используем <code>style</code> и переопределяем любые правила из внешних файлов и стилей </td><td>¯\_(ツ)_/¯</td></tr><tr><td><code>&#x3C;p style="color:#333">...&#x3C;/p></code></td><td></td></tr></tbody></table>

<details>

<summary>пример HTML странички</summary>

```html
<!DOCTYPE html>
<html dir="ltr" lang="en-US">
<head>
  <meta charset="UTF-8"/>
  <title>CSS Priority</title>
</head>
<body>
<section id="content" class="wrapper">
  <h2>Section Title</h2>
  <article id="top" class="news">
    <h3>Article Title</h3>
    <p id="pinned" class="intro">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
      Curabitur semper imperdiet felis, sit amet imperdiet arcu
      varius et. Donec condimentum pulvinar sollicitudin.
    </p>
  </article>
  <article class="news">
    <h3>Article Title</h3>
    <p class="intro" style="color:#333">
      Sed rutrum accumsan ultricies. Nunc iaculis enim vel augue 
      porta pellentesque. Nunc efficitur ex non ullamcorper ultricies.
      Nunc tempus vulputate enim, non egestas orci sodales eget.
    </p>
  </article>
</section>
</body>
</html>
```

</details>

Есть ещё правила расчёта специфичности которые относятся к использованию определенных псевдоселекторов типа `:not()` и `:where()`, сам по себе такой селектор не добавляет веса к специфичности, но правило которое находится в скобках имеет вес:

```css
/* [0:0:1] */
.red {
  color: red;
}

/* [0:1:1] */
.red:not(div) {
  color: red;
}
```

Да, расчёт специфичности может действительно стать нетривиальной задачей, поэтому вот вам пару калькуляторов на выбор:

* [https://www.codecaptain.io/tools/css-specificity-calculator](https://www.codecaptain.io/tools/css-specificity-calculator)
* [https://polypane.app/css-specificity-calculator/](https://polypane.app/css-specificity-calculator/)

{% hint style="warning" %}
Запомните, что первоначально при использовании правил CSS ключевым является уровень специфичности селектора. И только случае, когда специфичности совпадают, будет важен порядок подключения стилей:

```html
<style>
  .red {
    color: red;
  }
  .blue {
    color: blue;
  }
</style>
<p class="red blue">
  Этот параграф будет синим
</p>
```
{% endhint %}

{% hint style="danger" %}
Ну и повторюсь, метка `!important` - страшная вещь, использовать следует лишь в крайнем случае, такое правило имеет наивысший приоритет, хотя и не имеет ничего общего со специфичностью.
{% endhint %}

***

#### Домашнее задание

Вот кусочек CSS для тренировки, напишите соответствующий ему HTML (это вопрос с собеседования ;):

```css
#my p.announce, .tt.pm li li a:hover + span { 
    color: #f00;
}
```

И ещё один:

```css
#my > li, dd.dd.tt ~ span {
    text-decoration: underline;
}
```
