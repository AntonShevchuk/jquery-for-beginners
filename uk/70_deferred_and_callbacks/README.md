---
description: Або синхронність, коли як
---

# 70% Асинхронність

Це «mad skills» – змусити асинхронний JavaScript працювати так, як нам хочеться.

В jQuery є кілька інструментів, які нам допоможуть оволодіти цією навичкою:

<table data-header-hidden><thead><tr><th width="202">метод</th><th>опис</th></tr></thead><tbody><tr><td><code>$.Deferred()</code></td><td>об'єкт <a href="https://api.jquery.com/category/deferred-object/">Deferred</a> дає нам можливість реєструвати безліч callback-функцій та керувати їхнім виконанням</td></tr><tr><td><code>$.when()</code></td><td>метод <a href="https://api.jquery.com/jQuery.when/">when()</a> дозволяє виконувати callback-функції на основі асинхронних об'єктів Deferred</td></tr><tr><td><code>$.Callbacks()</code></td><td>об'єкт <a href="https://api.jquery.com/category/callbacks-object/">Callbacks</a> дозволяє нам керувати списком callback-функцій: додавати, вимикати, видаляти та запускати</td></tr></tbody></table>

{% hint style="info" %}
З jQuery версії 3.x, Deferred об'єкт став сумісним з Promise з ES-2015 (т.зв. ES6), тож практично все, що стосується [Promise](https://uk.javascript.info/promise), справедливо і для [Deferred](https://api.jquery.com/category/deferred-object/).
{% endhint %}

Давай на кожному прикладі зупинимось детальніше.
