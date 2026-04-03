# Optimization

{% hint style="danger" %}
First and foremost, you should remember that&#x20;

**search results are not cached — every time you query elements by selector, you're triggering the element search all over again**
{% endhint %}

After getting familiar with Sizzle's algorithm, a few optimization tips come to mind right away:

* Save search results (based on the postulate above):

```javascript
// before
$("a.button").addClass("active");
/* ... */
$("a.button").click(function(){ /* ... */ });

// after
var $button = $("a.button");
$button.addClass("active");
/* ... .*/
$button.click(function(){ /* ... */ });
```

> A proper IDE knows about these things and will give you hints from time to time ;)

* Use call chains (which is essentially the same as the first rule):

```javascript
// before
$("a.button").addClass("active");
$("a.button").click(function(){ /* ... */ });

// after
$("a.button")
  .addClass("active")
  .click(function(){ /* ... */ });
```

* Use `context` (that's the second parameter when selecting by selector):

```javascript
// before
$(".content a.button");

// after
$("a.button", ".content");

// another option, not faster, but more readable
$(".content").find("a.button");
```

* Break the query into simpler components using `context`, and save intermediate results:

```javascript
// before
$(".content a.button");
$(".content h3.title");

// after
let $content = $(".content")
$content.find("a.button");
$content.find("h3.title");
```

* Use more "digestible" selectors to help `.querySelectorAll()` — meaning, if you're not sure about the selector syntax, or you have doubts that all browsers support the CSS selector you need, it's better to split a "complex" selector into several simpler ones:

```javascript
// before
$(".content div input:disabled");

// after
$(".content div").find("input:disabled");
```

* Don't use jQuery at all, and work with native JavaScript functions instead

> There's one more point — choose the fastest selector possible, but you can't get by without a solid knowledge base for that, so go ahead, experiment and send me your examples.

For a visual demonstration, take a look at this comparison test [benchmark.html](https://anton.shevchuk.name/book/code/benchmark.html):

<figure><img src="../../.gitbook/assets/benchmark.png" alt=""><figcaption></figcaption></figure>

> This test searches for elements using several methods and was originally developed by Ilya Kantor for a JavaScript and jQuery masterclass

{% hint style="info" %}
A little trick from the jQuery creators — queries by element id don't reach Sizzle at all, they're fed to `document.getElementById()` as a parameter:

{% code fullWidth="false" %}
```javascript
$("#content");
// -> 
document.getElementById("content");
```
{% endcode %}
{% endhint %}

## Optimization Examples

Selecting by element identifier is the fastest option available — try to use it whenever possible:

```javascript
// just one regex + getElementById() under the hood
$("#content");

// this way is even faster
// the savings are negligible, but usability drops to near zero
$(document.getElementById("content"));
```

The selector `div#content` is an order of magnitude slower than searching by just the identifier `#content`, but it has its right to exist when your script is used across several pages and the logic requires processing behavior only for a `<div>` element. This selector can be represented in two ways:

```javascript
// getElementById() + filtering
$("#content").filter("div");

// leave it as is and hope for QuerySelectorAll()
$("div#content");
```

But even here there's a performance difference — [testing](https://jsperf.app/biwafe) shows that the example using `.filter()` works 20-30% faster:

{% tabs %}
{% tab title="Chrome" %}
<figure><img src="../../.gitbook/assets/chrome-benchmark.png" alt=""><figcaption><p>Testing in Chrome</p></figcaption></figure>
{% endtab %}

{% tab title="Safari" %}
<figure><img src="../../.gitbook/assets/safari-benchmark.png" alt=""><figcaption><p>Testing in Safari</p></figcaption></figure>
{% endtab %}
{% endtabs %}

But still, this kind of fine-tuned optimization isn't always necessary. \
Draw your own conclusions.
