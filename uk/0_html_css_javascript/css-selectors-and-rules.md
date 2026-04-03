# CSS-правила та селектори

Тепер приступимо до CSS, і почнемо, мабуть, з розшифровки абревіатури CSS. Це Cascading Style Sheets, дослівно «каскадна таблиця стилів», але:

_— Чому ж вона називається каскадною?_ — це питання я часто задаю на співбесідах претендентам. Відповіддю ж буде аналогія, бо вона непохитна, як перпендикулярна жаба: уявіть собі каскад водоспаду, ось ви стоїте на одній зі сходинок каскаду із чорнильницею в руках, і виливаєте її вміст у воду — уся вода нижче по каскаду буде забарвлюватись у колір чорнил. Так і в CSS: якщо ви задаєте стиль на певному рівні, він поширюється вниз по "каскаду" до елементів вебсторінки, забарвлюючи їх у "колір" вашого стилю.

_— Навіщо мені все це?_ — працюючи з jQuery, ви маєте «на відмінно» читати правила CSS, а також уміти складати CSS-селектори для пошуку необхідних елементів на сторінці. Практично всі задачі, які ви будете вирішувати за допомогою jQuery, починаються з пошуку необхідного елемента на сторінці, тож **знання CSS-селекторів обов'язкове**.

Але давайте про все по черзі, візьмемо наступний простенький приклад цілком семантичного HTML:

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
      © jQuery for beginners
  </footer>
</body>
</html>
```
{% endcode %}

Це приклад простого і правильного HTML з невеличким додаванням CSS. Давайте розберемо селектори в наведеному CSS-коді (я навмисно не виносив CSS в окремий файл, бо так наочніше):

* `body` — ці правила будуть застосовані до тегу `<body>` та всіх його нащадків, запам'ятайте: налаштування шрифтів поширюються вниз по ієрархії елементів
* `h1,h2,h3` — ми обираємо теги `<h1>`, `<h2>` та `<h3>`, і встановлюємо колір шрифту для цих тегів та їх нащадків
* `#content` — обираємо елемент з `id="content"`, налаштування відступів не поширюються на нащадків, вони будуть змінюватися тільки для цього елемента
* `.box` — обираємо елементи з `class="box"`, і змінюємо зовнішній вигляд границь елементів із заданим класом

Тепер детальніше і з ускладненими прикладами:

<table><thead><tr><th width="215">селектор</th><th></th></tr></thead><tbody><tr><td><code>h1</code></td><td>шукаємо елементи за іменем тегу</td></tr><tr><td><code>#container</code></td><td>шукаємо елемент за ідентифікатором <code>id=container</code> (<strong>ідентифікатор унікальний</strong>, отже, на сторінці він має бути лише один)</td></tr><tr><td><code>div#container</code></td><td>шукаємо <code>&#x3C;div></code> з ідентифікатором <code>container</code>, але попередній селектор працює швидше, а цей важливіший</td></tr><tr><td><code>.news</code></td><td>обираємо елементи за іменем класу <code>class="news"</code></td></tr><tr><td><code>div.news</code></td><td>всі елементи <code>&#x3C;div></code> з класом <code>news</code> </td></tr><tr><td><code>#wrap .post</code></td><td>шукаємо всі елементи з класом <code>class="post"</code> всередині елемента з <code>id="wrap"</code></td></tr><tr><td><code>.cls1.cls2</code></td><td>обираємо елементи з двома класами <code>class="cls1 cls2"</code></td></tr><tr><td><code>h1, h2, .posts</code></td><td>перелік селекторів, оберемо все перелічене</td></tr><tr><td><code>.post > h2</code></td><td>обираємо елементи <code>&#x3C;h2></code>, які є безпосередніми нащадками елемента з класом «post»</td></tr><tr><td><code>a + span</code></td><td>будуть обрані всі елементи <code>&#x3C;span></code>, що йдуть одразу за елементом <code>&#x3C;a></code></td></tr><tr><td><code>a[href^=https]</code></td><td>будуть обрані всі елементи <code>&#x3C;a></code>, у яких атрибут <code>href</code> починається з <code>https</code> (ймовірно, всі зовнішні посилання)</td></tr></tbody></table>

> Це аж ніяк не весь список, опис усіх CSS-селекторів можна знайти на відповідній сторінці W3C: [https://www.w3.org/TR/selectors-3/](https://www.w3.org/TR/selectors-3/)

Повертаючись до нашої аналогії з водоспадом, уявіть, що на наступній сходинці хтось виллє чорнила іншого кольору, ми ж у результаті отримаємо змішування кольорів. Але це в житті, а в CSS ця зміна "перефарбує воду" на всіх наступних сходинках, адже в CSS важливий порядок, порядок і пріоритети. Перейдімо до суті:

1.  перша сходинка нашого каскаду має найнижчий пріоритет, це стилі браузера за замовчуванням

    > як би не заявляли розробники, але в різних браузерах ці стилі можуть відрізнятись, тому все ще існує таке явище як [CSS Reset](https://www.google.com/search?q=CSS+Reset), такий собі вирівнювач
2. на другій сходинці стилі, задані користувачем у надрах налаштувань браузера; на практиці зустрічаються рідко
3. найважливіші для нас — стилі автора сторінки, але й тут усе йде за порядком
   *   найнижчий пріоритет у стилів, що підключені як файл `<link rel="stylesheet" type="text/css" href="...">` або вбудовані всередину HTML за допомогою тегу `<style>`:

       ```html
       <link rel="stylesheet" type="text/css" href="styles.css">
       <style>
         p {
           color: red;
         }
       </style>
       <p>
         Цей параграф буде червоним
       </p>
       ```

       > і звісно, при рівності пріоритетів «тапки» у того, хто оголошений останнім
   *   потім ті, що захардкодили погані люди (не ви, ви так робити не будете) в `style=""`:

       ```html
       <p style="color: blue">
         Цей параграф буде синім
       </p>
       ```
   *   і тут з'являється мітка `!important`, і все нам ламає, бо правило з такою міткою стає важливішим навіть за inline-стилі, і все йде шкереберть:

       ```html
       <style>
         p {
           color: red !important;
         }
       </style>
       <p style="color: blue">
         Цей параграф усе одно буде червоним!
       </p>
       ```
   *   щоб перебити такий стиль, залишається лише варіант прописувати inline-стилі з міткою `!important`:

       ```html
       <style>
         p {
           color: red !important;
         }
       </style>
       <p style="color: blue !important">
         Цей параграф буде синім!
       </p>
       ```
4. пригоди з `!important` не закінчились, якщо є користувацькі стилі із цією міткою, то тепер їх вихід
5. і навіть браузери грішать з  `!important`, і такі стилі мають практично максимальний пріоритет

{% hint style="info" %}
Є ще пріоритети у правил, які використовуються для [CSS анімації](https://developer.mozilla.org/ru/docs/Web/CSS/CSS_animations/Using_CSS_animations) та [CSS переходів](https://developer.mozilla.org/ru/docs/Web/CSS/CSS_transitions/Using_CSS_transitions), але це вже зовсім інша історія
{% endhint %}

Якщо голова ще не болить, то я також згадаю, що при розрахунку, чиї ж правила головніші, аналізується [специфічність](https://developer.mozilla.org/ru/docs/Web/CSS/Specificity) селекторів, і тут обчислюється наступним чином:

* розрахунок відбувається за трьома ваговими позиціями — `[0:0:0]`
* за кожен ідентифікатор елемента (`#id`) — `[1:0:0]`
* за кожен клас (`.class`), або псевдоклас (`:pseudo`) — `[0:1:0]`
* за кожен тег (`<a>`, `<div>`, тощо) — `[0:0:1]`
* при цьому `[1:0:0]` > `[0:x:y]` > `[0:0:x]`
* при рівності рахунку — знову «капці» у того, хто оголошений останнім

Приклад селекторів, вибудуваних за зростанням пріоритету (відповідний код HTML буде нижче):

<table><thead><tr><th width="608">селектор</th><th>пріоритет</th></tr></thead><tbody><tr><td>тег має найменший пріоритет</td><td><code>[0:0:1]</code></td></tr><tr><td><code>p { color: orange }</code></td><td></td></tr><tr><td>додаємо до тегу клас <code>intro</code></td><td><code>[0:1:1]</code></td></tr><tr><td><code>p.intro { color: green }</code></td><td></td></tr><tr><td>додаємо ще тег</td><td><code>[0:1:2]</code></td></tr><tr><td><code>article p.intro { color: blue }</code></td><td></td></tr><tr><td>... нам потрібно більше класів</td><td><code>[0:2:2]</code></td></tr><tr><td><code>article.news p.intro { color: red }</code></td><td></td></tr><tr><td>ідентифікатор <code>id="pinned"</code> навіть сам по собі важливіший за всі теги та класи разом узяті</td><td><code>[1:0:0]</code></td></tr><tr><td><code>#pinned { color: darkblue }</code></td><td></td></tr><tr><td>додаємо тег <code>&#x3C;p></code>, і специфічність зростає</td><td><code>[1:0:1]</code></td></tr><tr><td><code>p#pinned { color: darkcyan }</code></td><td></td></tr><tr><td>додаємо ще один ідентифікатор <code>id="top"</code></td><td><code>[2:0:1]</code></td></tr><tr><td><code>#top p#pinned { color: darkgreen }</code></td><td></td></tr><tr><td>використовуємо <code>style</code> і перевизначаємо будь-які правила із зовнішніх файлів та стилів </td><td>¯_(ツ)_/¯</td></tr><tr><td><code>&#x3C;p style="color:#333">...&#x3C;/p></code></td><td></td></tr></tbody></table>

<details>

<summary>приклад HTML сторінки</summary>

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

Є ще правила розрахунку специфічності, які стосуються використання певних псевдоселекторів типу `:not()` та `:where()`, сам по собі такий селектор не додає ваги до специфічності, але правило, що знаходиться в дужках, має вагу:

```css
/* [0:1:0] */
.red {
  color: red;
}

/* [0:1:1] */
.red:not(div) {
  color: red;
}
```

Так, розрахунок специфічності може дійсно стати нетривіальною задачею, тому ось вам пару калькуляторів на вибір:

* [https://www.codecaptain.io/tools/css-specificity-calculator](https://www.codecaptain.io/tools/css-specificity-calculator)
* [https://polypane.app/css-specificity-calculator/](https://polypane.app/css-specificity-calculator/)

{% hint style="warning" %}
Запам'ятайте, що першочергово при використанні правил CSS ключовим є рівень специфічності селектора. І тільки у випадку, коли специфічності збігаються, буде важливим порядок підключення стилів:

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
  Цей параграф буде синім
</p>
```
{% endhint %}

{% hint style="danger" %}
Ну й повторюся, мітка `!important` - страшна річ, використовувати слід лише в крайньому разі, таке правило має найвищий пріоритет, хоча й не має нічого спільного зі специфічністю.
{% endhint %}

***

#### Домашнє завдання

Ось шматочок CSS для тренування, напишіть відповідний йому HTML (це питання зі співбесіди ;):

```css
#my p.announce, .tt.pm li li a:hover + span { 
    color: #f00;
}
```

І ще один:

```css
#my > li, dd.dd.tt ~ span {
    text-decoration: underline;
}
```
