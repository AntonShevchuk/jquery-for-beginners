# Upgrading to Version 4.x

As of now, the jQuery team has released a new version — it's still just a beta, but I think the main changes are already in, and ahead of us is the stable release with minor bug fixes.

The highlights:

*   dropped the "baggage" of old browsers: Internet Explorer 10, Edge Legacy, iOS <11, Firefox <65, and Android Browser;

    > they promise to drop Internet Explorer 11 in jQuery 5.0
* removed about a dozen functions that were declared deprecated in 3.x:
  * event shorthand methods: `click()`, `dblclick()`, `mouseenter()`, `mouseleave()`, `hover()`, `keydown()`, `keyup()`, `keypress()`, `focus()`, `blur()`, `change()`, `submit()`, and others — use `on()` and `trigger()` instead;
  * the `keypress` event — use `keydown` / `keyup`;
  * `$.isArray()` — use `Array.isArray()`;
  * `$.type()` — use `typeof` or `instanceof`;
  * `$.isFunction()` — use `typeof fn === "function"`;
  * `$.isWindow()` — use `window === obj`;
  * `$.now()` — use `Date.now()`;
  * `$.proxy()` — use `Function.prototype.bind()`;
  * `$.trim()` — use `String.prototype.trim()`;
  * `$.parseJSON()` — use `JSON.parse()`;
*   sorted out the event order: `blur` -> `focusout` -> `focus` -> `focusin` — now it's this way and only this way, in accordance with the new W3C edition;

    > now only IE has a different order, and the irony is that only IE followed the previous W3C edition, and only in 2023 was the spec changed to match the implementation in modern browsers&#x20;
* added binary data support in `$.ajax()`;
* removed automatic JSON-as-JSONP processing — now for such requests you need to explicitly specify that you want a JSONP response from the server;
* the framework has fully migrated from AMD to ES modules;
* added [Trusted Types](https://github.com/w3c/trusted-types) support to eliminate Content Security Policy errors in the console.

The list seems short, and I think upgrading to jQuery 4.0 shouldn't cause any problems.
