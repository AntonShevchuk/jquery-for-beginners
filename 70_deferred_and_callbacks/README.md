---
description: Или синхронность, когда как
---

# 70% Ассинхронность

Это «mad skills» – заставлять асинхронный JavaScript работать так, как нам хочется.

В jQuery есть несколько инструментов которые нам помогут овладеть данным навыком:

<table data-header-hidden><thead><tr><th width="202">метод</th><th>описание</th></tr></thead><tbody><tr><td><code>$.Deferred()</code></td><td>объект <a href="https://api.jquery.com/category/deferred-object/">Deferred</a> даёт нам возможность регистрировать множество callback-функций и управлять их выполнением</td></tr><tr><td><code>$.when()</code></td><td>метод <a href="https://api.jquery.com/jQuery.when/">when()</a> позволяет выполнять callback-функции на основе асинхронных объектов Deferred</td></tr><tr><td><code>$.Callbacks()</code></td><td>объект <a href="https://api.jquery.com/category/callbacks-object/">Callbacks</a> позволяет нам управлять списком callback-функций: добавлять, отключать, удалять и запускать</td></tr></tbody></table>

{% hint style="info" %}
С jQuery версии 3.x, Deferred объект стал совместим с Promise из ES-2015 (т.н. ES6), так что практически всё, что относится к [Promise](https://learn.javascript.ru/promise) верно и для [Deferred](https://api.jquery.com/category/deferred-object/).
{% endhint %}

Давайте на каждом примере остановимся подробней.
