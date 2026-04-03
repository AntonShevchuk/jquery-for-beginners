---
description: Функції для реалістичної анімації
---

# Easing функції

Погралися і досить, час ускладнити вам життя. У методу `animate()` може бути більше ніж один аргумент, і час перейти до їх розбору. Набір аргументів може бути різним; наведу перший, той, що простіший:

<table data-header-hidden><thead><tr><th width="243">параметр</th><th>призначення</th></tr></thead><tbody><tr><td><code>properties</code></td><td>CSS-властивості — з цим ми вже познайомилися</td></tr><tr><td><code>duration</code></td><td>швидкість анімації, вже згадувалася раніше, вказується в мілісекундах, або використовуючи ключові слова «<code>fast</code>» чи «<code>slow</code>»</td></tr><tr><td><code>easing</code></td><td>вказуємо, яку функцію будемо використовувати для зміни значень</td></tr><tr><td><code>complete</code></td><td>функція, яка буде викликана після закінчення анімації</td></tr></tbody></table>

З наведених параметрів нам тільки «`easing`» не зустрічався раніше — я його беріг для даного моменту. Цей параметр вказує, яка функція буде використовуватися для процесу анімації значень. Це можуть бути лінійні, квадратичні, кубічні та будь-які інші функції. «З коробки» ми можемо обрати лише між «`linear`»:

<figure><img src="../../.gitbook/assets/linear.svg" alt="" width="563"><figcaption><p>linear</p></figcaption></figure>

та «`swing`»:

<figure><img src="../../.gitbook/assets/swing.svg" alt="" width="563"><figcaption><p>swing</p></figcaption></figure>

Зазирнувши в код jQuery, ми легко знайдемо відповідний код:

```javascript
jQuery.easing = {
    linear: function( p ) {
        return p;
    },
    swing: function( p ) {
        return 0.5 - Math.cos( p * Math.PI ) / 2;
    },
    _default: "swing"
};
```

{% hint style="info" %}
`p` — коефіцієнт проходження анімації, змінюється від 0 до 1.
{% endhint %}

Хочете більше easing-функцій? Тоді шукайте easing plugin на сторінці [https://gsgd.co.uk/sandbox/jquery/easing/](https://gsgd.co.uk/sandbox/jquery/easing/), він дійсно з розряду «must have».

{% embed url="https://anton.shevchuk.name/book/code/easing.html" %}

> Як путівник по easing-функціях можете використовувати сторінку [https://easings.net/](https://easings.net/).

Про те, як написати свою функцію для анімації я розповім пізніше.
