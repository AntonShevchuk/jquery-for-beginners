# 30% Події

Перш ніж братися до прочитання цього розділу, варто визначитися, що ж таке події web-сторінки. Отже, події – це будь-які дії користувача, будь-то уведення даних з клавіатури, прокручування сторінки чи рухи мишки, і, звичайно ж, «кліки».

> А ще існують події, що створюються скриптами, та їхні обробники – тригери та хендлери, але про них трохи пізніше.

jQuery працює практично з усіма подіями в JavaScript, наведу список найбільш популярних з невеликими поясненнями:

<table data-header-hidden><thead><tr><th width="210">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>click</code></td><td>клік по елементу;<br>порядок подій: <code>mousedown</code> → <code>mouseup</code> → <code>click</code></td></tr><tr><td><code>dblclick</code></td><td>подвійний клік мишкою</td></tr><tr><td><code>mousedown</code></td><td>натискання клавіші миші</td></tr><tr><td><code>mouseup</code></td><td>відпускання клавіші миші</td></tr><tr><td><code>mousemove</code></td><td>рух курсора</td></tr><tr><td><code>mouseenter</code></td><td>наведення курсора на елемент;<br>не спрацьовує при переході фокуса на дочірні елементи</td></tr><tr><td><code>mouseleave</code></td><td>виведення курсора з елемента;<br>не спрацьовує при переході фокуса на дочірні елементи</td></tr><tr><td><code>mouseover</code></td><td>наведення курсора на елемент</td></tr><tr><td><code>mouseout</code></td><td>виведення курсора з елемента</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/events.mouse.html" %}

***

Ідемо далі. Ось ще десяток подій, здебільшого вони стосуються лише елементів форм:

<table data-header-hidden><thead><tr><th width="221">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>focus</code></td><td>отримання фокуса на елементі; <br>актуально для <code>input[type=text]</code>, але в сучасних браузерах працює і з іншими елементами</td></tr><tr><td><code>blur</code></td><td>фокус пішов з елемента;<br>спрацьовує при кліку по іншому елементу на сторінці або по події клавіатури (наприклад, перемикання по tab)</td></tr><tr><td><code>focusin</code></td><td>фокус на елементі; <br>ця подія аналогічна <code>focus</code>, але при цьому вміє «спливати»</td></tr><tr><td><code>focusout</code></td><td>фокус пішов з елемента;<br>ця подія аналогічна <code>blur</code>, «спливає» (детальніше про відмінності ви можете прочитати у статті «<a href="https://uk.javascript.info/focus-blur">Фокусування: focus/blur</a>»)</td></tr></tbody></table>

{% hint style="info" %}
Аж до 4-ї версії jQuery, порядок подій був наступним:

* `focusout`
* `blur`
* `focusin`
* `focus`

З jQuery версії 4.0 порядок подій відповідатиме поточній специфікації [W3C](https://www.w3.org/TR/uievents/#events-focusevent-event-order):

* `blur`
* `focusout`
* `focus`
* `focusin`

Єдиним браузером, який має інший порядок, залишається IE, але іронія полягає в тому, що IE свого часу був єдиним браузером, який дотримувався попередньої специфікації W3C, а всі інші браузери використовували свій порядок подій, який ось тільки у 2023-му році став стандартом ¯\\_(ツ)_/¯
{% endhint %}

<table data-header-hidden><thead><tr><th width="221">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>change</code></td><td>зміна значення елемента (значення при втраті фокуса відрізняється від початкового значення при отриманні фокуса)</td></tr><tr><td><code>keydown</code></td><td>натискання клавіші на клавіатурі</td></tr><tr><td><code>keypress</code></td><td>утримування клавіші на клавіатурі, послідовність <code>keydown</code> → <code>keypress</code> → <code>keyup</code> (тільки для літер-цифр)</td></tr><tr><td><code>keyup</code></td><td>відпускання клавіші на клавіатурі</td></tr><tr><td><code>select</code></td><td>вибір тексту для <code>input[type=text]</code> та <code>textarea</code></td></tr><tr><td><code>submit</code></td><td>відправлення форми</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/form.events.html" %}

***

Також варто згадати ще парочку популярних подій, з якими доводиться працювати:

<table data-header-hidden><thead><tr><th width="223">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>resize</code></td><td>зміна розмірів документа</td></tr><tr><td><code>scroll</code></td><td>прокрутка елемента або вікна</td></tr></tbody></table>

{% hint style="info" %}
Немає потреби запам'ятовувати всі події — ви завжди знайдете їх на сторінках [MDN від Mozilla](https://developer.mozilla.org/ru/docs/Web/Events).
{% endhint %}
