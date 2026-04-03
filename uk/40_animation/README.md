# 40% Анімація

Бібліотека jQuery дозволяє дуже легко анімувати DOM-елементи, для цього передбачено кілька функцій. Але про все по порядку. Почнемо з простих `hide()` та `show()`, ці два методи, відповідно, приховують або відображають елементи:

<table data-header-hidden><thead><tr><th width="282">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">hide()
</code></pre></td><td>приховаємо елементи</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">show()
</code></pre></td><td>відобразимо елементи</td></tr></tbody></table>

Дані виклики оперують лише CSS-атрибутом `display` та перемикають його з поточного стану в `none` і назад. Як перший параметр можна задати швидкість анімації, для цього можна використати одне із зарезервованих слів `slow` або `fast`, або ж вказувати швидкість у мілісекундах (1000 мс = 1 сек):

```javascript
// повільно спускаємося з гори і… приховуємо всі картинки
//   slow == 600
//   fast == 200
$("img").hide('slow')
```

```javascript
// тепер повернемо їх на місце, трохи швидше
$("img").show(400)
```

У такому разі зникнення елементів буде супроводжуватися зміною атрибутів `width`, `height`, `opacity` та інших. На додачу до цих двох методів є ще метод `toggle()`, який працює як перемикач `hide → show` або `show → hide`:

```javascript
// запустіть пару разів
$("img").toggle()
```

Далі — більше, другим параметром у наведених методах може бути callback-функція — вона буде виконана після закінчення анімації:

```javascript
// приховуємо всі картинки
$("img").hide("slow", function() {
    // після чого відображаємо alert
    alert("Images was hidden")
});
```

{% embed url="https://anton.shevchuk.name/book/code/hide.html" %}

***

{% hint style="info" %}
Анімацію таких атрибутів як `height`, `width` та `opacity` видно неозброєним оком, насправді ж це далеко не все. Зазирнувши всередину jQuery, можна побачити, що також змінюються внутрішні та зовнішні відступи — `padding` та `margin` — тож не варто про це забувати.
{% endhint %}

Ідемо далі — у нас на черзі набір методів із сімейства slide: `slideUp()`, `slideDown()` та `slideToggle()`. Їхня поведінка схожа з попередніми функціями, але анімація зачіпатиме лише висоту блоків:

<table data-header-hidden><thead><tr><th width="284">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">slideUp()
</code></pre></td><td>приховаємо елементи змінюючи висоту до <code>0</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">slideDown()
</code></pre></td><td>відобразимо всі елементи, поступово збільшуючи висоту елементів з нуля</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">slideToggle()
</code></pre></td><td>працює як <code>slideDown()</code> для прихованих елементів, інакше як <code>slideUp()</code></td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/slide.html" fullWidth="false" %}

***

Перш ніж перейти до десерту, згадаю сімейство функцій `fade` — вони маніпулюють лише атрибутом `opacity`:

<table data-header-hidden><thead><tr><th width="281">метод</th><th>опис</th><th data-hidden data-type="rating" data-max="5"></th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeOut()
</code></pre></td><td>змінює <code>opacity</code> від поточного до <code>0</code></td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeIn()
</code></pre></td><td>змінює <code>opacity</code> від <code>0</code> до попереднього значення</td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeToggle()
</code></pre></td><td>перемикач між <code>In</code> та <code>Out</code></td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeTo(duration, opacity)
</code></pre></td><td>змінює значення <code>opacity</code> до потрібного значення</td><td>null</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/fade.html" %}

***

А тепер найсолодше — всі ефекти анімації в jQuery крутяться навколо методу `animate()`. Дана функція бере одну або кілька CSS-властивостей елемента та змінює їх від вихідного до заданого за N-ну кількість ітерацій (кількість ітерацій залежить від вказаного часу, але не рідше однієї ітерації за 13 мс, якщо я правильно накопав це значення). Ну що ж, від слів до діла, спробуємо реалізувати функції `fadeIn()` та `fadeOut()` за допомогою `animate()`:

```javascript
// fadeOut()
$("img").animate({
    "opacity": "hide"
})
```

```javascript
// fadeIn()
$("img").animate({
    "opacity": "show"
})
```

Все просто, еге ж. Давайте тепер ускладнимо задачу — змінимо розмір блоків та прозорість:

```javascript
// значення вказаних властивостей будуть плавно змінюватися
// від поточних до заданих
$("img").animate({
    "opacity": 0.5,
    "height": "100px",
    "width": "100px"
})
```

Як бачите, теж нескладно. Тепер спробуємо відштовхуватися від поточних значень, а не задавати необхідні:

```javascript
// змінюємо, крок за кроком
$("img").animate({
    "opacity": "+=0.1",
    "height": "+=12px",
    "width": "+=20px"
})
```

{% embed url="https://anton.shevchuk.name/book/code/animate.html" %}
