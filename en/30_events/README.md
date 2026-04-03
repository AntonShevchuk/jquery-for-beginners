# 30% Events

Before diving into this chapter, let's figure out what web page events actually are. So, events are user actions — typing on the keyboard, scrolling the page, moving the mouse, and of course, "clicking".

> There are also events created by scripts, along with their handlers — triggers and handlers, but more on that a bit later.

jQuery works with practically all JavaScript events. Here's a list of the most commonly used ones with brief explanations:

<table data-header-hidden><thead><tr><th width="210">event</th><th>description</th></tr></thead><tbody><tr><td><code>click</code></td><td>click on an element;<br>event order: <code>mousedown</code> → <code>mouseup</code> → <code>click</code></td></tr><tr><td><code>dblclick</code></td><td>double mouse click</td></tr><tr><td><code>mousedown</code></td><td>mouse button pressed</td></tr><tr><td><code>mouseup</code></td><td>mouse button released</td></tr><tr><td><code>mousemove</code></td><td>cursor movement</td></tr><tr><td><code>mouseenter</code></td><td>cursor enters an element;<br>does not fire when focus moves to child elements</td></tr><tr><td><code>mouseleave</code></td><td>cursor leaves an element;<br>does not fire when focus moves to child elements</td></tr><tr><td><code>mouseover</code></td><td>cursor enters an element</td></tr><tr><td><code>mouseout</code></td><td>cursor leaves an element</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/events.mouse.html" %}

***

Moving on. Here's another dozen events, mostly related to form elements:

<table data-header-hidden><thead><tr><th width="221">event</th><th>description</th></tr></thead><tbody><tr><td><code>focus</code></td><td>element receives focus; <br>relevant for <code>input[type=text]</code>, but in modern browsers works with other elements too</td></tr><tr><td><code>blur</code></td><td>element loses focus;<br>fires when clicking on another element on the page or via a keyboard event (e.g., tabbing)</td></tr><tr><td><code>focusin</code></td><td>element receives focus; <br>this event is similar to <code>focus</code>, but it also "bubbles"</td></tr><tr><td><code>focusout</code></td><td>element loses focus;<br>this event is similar to <code>blur</code>, and it "bubbles" (you can read more about the differences in the article "<a href="https://javascript.info/focus-blur">Focusing: focus/blur</a>")</td></tr></tbody></table>

{% hint style="info" %}
Up to jQuery version 4, the event order was as follows:

* `focusout`
* `blur`
* `focusin`
* `focus`

Starting with jQuery 4.0, the event order will match the current [W3C](https://www.w3.org/TR/uievents/#events-focusevent-event-order) specification:

* `blur`
* `focusout`
* `focus`
* `focusin`

The only browser that has a different order is IE, but the irony is that IE was once the only browser that followed the previous W3C specification, while all other browsers used their own event order, which only became the standard in 2023 ¯\\\_(ツ)\_/¯
{% endhint %}

<table data-header-hidden><thead><tr><th width="221">event</th><th>description</th></tr></thead><tbody><tr><td><code>change</code></td><td>element value changed (the value on blur differs from the initial value on focus)</td></tr><tr><td><code>keydown</code></td><td>keyboard key pressed</td></tr><tr><td><code>keypress</code></td><td>keyboard key held down, sequence: <code>keydown</code> → <code>keypress</code> → <code>keyup</code> (only for letters and numbers)</td></tr><tr><td><code>keyup</code></td><td>keyboard key released</td></tr><tr><td><code>select</code></td><td>text selection for <code>input[type=text]</code> and <code>textarea</code></td></tr><tr><td><code>submit</code></td><td>form submission</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/events.form.html" %}

***

Also worth mentioning are a couple more popular events you'll work with:

<table data-header-hidden><thead><tr><th width="223">event</th><th>description</th></tr></thead><tbody><tr><td><code>resize</code></td><td>document size changed</td></tr><tr><td><code>scroll</code></td><td>element or window scrolled</td></tr></tbody></table>

{% hint style="info" %}
No need to memorize all events — you can always find them on the [MDN pages from Mozilla](https://developer.mozilla.org/en-US/docs/Web/Events).
{% endhint %}
