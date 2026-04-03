# Прогрес

Давай повернемося до методу `animate()`, який як аргументи може приймати розширений набір опцій — `animate(properties, options)`, першим ідуть CSS-властивості, з якими ми вже працювали раніше, а ось другим аргументом — опції, які нам дають низку нових можливостей (я перерахую всі доступні опції, частина з них нам вже зустрічалася):

<table data-header-hidden><thead><tr><th width="216">опція</th><th>призначення</th></tr></thead><tbody><tr><td><code>duration</code></td><td>швидкість анімації</td></tr><tr><td><code>easing</code></td><td>функція «<code>linear</code>» або «<code>swing</code>», або будь-яка інша</td></tr><tr><td><code>queue</code></td><td>прапорець/параметр черги; про неї розповім трохи нижче</td></tr><tr><td><code>specialEasing</code></td><td><p>хеш, в якому можна описати, яку саме easing-функцію слід використовувати для зміни певних параметрів:</p><pre class="language-javascript"><code class="lang-javascript">$("img").animate({
  "height": "+=100px",
  "width": "+=100px"
}, {
  "specialEasing": {
    "height": "linear",
    "width": "swing"
  }
})
</code></pre></td></tr><tr><td><code>step</code></td><td>функція, яка буде викликана на кожному кроці анімації для кожної CSS-властивості; трохи пізніше опишу детальніше</td></tr><tr><td><code>complete</code></td><td>функція, яка буде викликана після закінчення анімації</td></tr><tr><td><code>start</code></td><td>функція, яка буде викликана до початку анімації</td></tr><tr><td><code>progress</code></td><td>функція, яка буде викликана на кожному кроці, але лише одноразово для елемента, незалежно від кількості CSS-властивостей</td></tr><tr><td><code>done</code></td><td>функція, яка буде викликана після успішного закінчення анімації</td></tr><tr><td><code>fail</code></td><td>функція, яка буде викликана після невдалого закінчення анімації</td></tr><tr><td><code>always</code></td><td>функція, яка буде викликана після закінчення анімації за будь-якого результату</td></tr></tbody></table>

{% hint style="info" %}
Підтримка останніх п'яти функцій додана у версії 1.8. Даний [реліз jQuery ознаменувався](https://blog.jquery.com/2012/08/09/jquery-1-8-released/) глобальним рефакторингом анімації та переїздом на Deferred.

Навіщо я постійно звертаю вашу увагу на зміни в таких старих версіях jQuery? Просто ніхто не знає з чим вам доведеться зіткнутися, тому краще готуватися до гіршого.
{% endhint %}

В яких випадках нам це буде корисно? Ну якщо вам потрібно відстежувати прогрес анімації, то це саме те, що вам потрібно, точніше, вам потрібна буде функція `progress(animation, progress, remainingMs)`:

```javascript
// змінюємо висоту та ширину картинок
$("img").animate({
  "height": "+=100px",
  "width": "+=100px"
}, {
  "start":    () => console.log("start"),
  "progress": (animation, progress, time) => console.log("progress", progress),
  "done":     () => console.log("done"),
  "fail":     () => console.log("fail"),
  "always":   () => console.log("always")
})
```



{% embed url="https://anton.shevchuk.name/book/code/animate.options.html" %}
