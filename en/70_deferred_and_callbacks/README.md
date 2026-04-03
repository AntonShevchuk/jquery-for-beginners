---
description: Or synchronicity, depending on the situation
---

# 70% Asynchrony

This is "mad skills" — making asynchronous JavaScript work the way we want it to.

jQuery has several tools that will help us master this skill:

<table data-header-hidden><thead><tr><th width="202">method</th><th>description</th></tr></thead><tbody><tr><td><code>$.Deferred()</code></td><td>the <a href="https://api.jquery.com/category/deferred-object/">Deferred</a> object gives us the ability to register multiple callback functions and control their execution</td></tr><tr><td><code>$.when()</code></td><td>the <a href="https://api.jquery.com/jQuery.when/">when()</a> method allows executing callback functions based on asynchronous Deferred objects</td></tr><tr><td><code>$.Callbacks()</code></td><td>the <a href="https://api.jquery.com/category/callbacks-object/">Callbacks</a> object lets us manage a list of callback functions: add, disable, remove, and fire them</td></tr></tbody></table>

{% hint style="info" %}
Since jQuery version 3.x, the Deferred object became compatible with ES-2015 (a.k.a. ES6) Promise, so practically everything related to [Promise](https://javascript.info/promise-basics) is also true for [Deferred](https://api.jquery.com/category/deferred-object/).
{% endhint %}

Let's take a closer look at each one.
