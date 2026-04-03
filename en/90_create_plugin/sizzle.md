# Sizzle

When I talked about Sizzle, I decided not to overwhelm you with the library's extension capabilities, and now the time has come... In Sizzle, you can extend quite a few things:

* `Sizzle.selectors.match`
* `Sizzle.selectors.find`
* `Sizzle.selectors.filter`
* `Sizzle.selectors.attrHandle`
* `Sizzle.selectors.pseudos`

But we're only going to tackle extending pseudo-selectors, like these:

```javascript
$("div:animated"); // find animated elements
$("div:hidden");   // find hidden div elements
$("div:visible");  // find visible div elements
```

Why did I list only these filters? Simple — they're the only ones that aren't part of Sizzle and belong exclusively to jQuery. These are exactly the kind of plugins we'll be practicing to develop. Let's start with the `:visible` filter code:

```javascript
// example for extending Sizzle inside jQuery
// extending Sizzle itself requires slightly different code
jQuery.expr.pseudos.visible = function( elem ) {
    // check the width and height of each element in the selection
    return !!( elem.offsetWidth || elem.offsetHeight);
};
```

This code doesn't look complicated, but I'll still provide a skeleton for a new filter and add a few clarifications:

```javascript
$.extend($.expr.pseudos, {
    /**
    * @param element DOM element
    * @param i element's ordinal number
    * @param match regex matching object
    * @param elements array of all found DOM elements
    */
    test: function(element, i, match, elements) {
        /* our code goes here, and it will decide who's guilty */
        return true || false; // deliver the verdict
    }
})
```

Alright, now let's try to solve the following task:

_— We need to highlight links in the text depending on their type: external, internal, or anchor on the page._

The best solution would be selector filters like these:

```javascript
$("a:internal");

$("a:anchor");

$("a:external");
```

Since this functionality isn't available out of the box, we'll write it ourselves. We don't need much for that (example only for the last one `:external`, working code on the [sizzle.filter.html](https://anton.shevchuk.name/book/code/sizzle.filter.html) page):

```javascript
$.extend($.expr.pseudos, {
    // detecting an external link
    // we only need the DOM Element
    external: function(element) {
        // is it a link?
        if (element.tagName.toUpperCase() != 'A') return false;
        // does it have an href attribute
        if (element.getAttribute('href')) {
            var href = element.getAttribute('href');
            // filter out the unnecessary
            if ((href.indexOf('/') === 0)  // internal link
              || (href.indexOf('#') === 0) // anchor
              || (href.indexOf(window.location.hostname) === 7) // our domain via http://
              || (href.indexOf(window.location.hostname) === 8) // or https://
            ) {
                return false; // nope
            } else {
                return true;  // yes, we found external links
            }
        } else {
            return false;
        }
    }
});
```

{% embed url="https://anton.shevchuk.name/book/code/sizzle.filter.html" %}

{% hint style="danger" %}
ALWAYS use the filter together with the HTML tag you're looking for:

<pre class="language-javascript"><code class="lang-javascript"><strong>// tag name + filter!
</strong><strong>$("div:filter")
</strong></code></pre>
{% endhint %}

{% hint style="info" %}
This is one of the optimization points when working with jQuery filters — otherwise your filter will process all DOM elements on the page, which can seriously hurt performance. If you have multiple tags, it's better to write it like this — `$("tag1:filter, tag2:filter, tag3:filter")`, or even better, using the `filter()` method.
{% endhint %}

* [Sizzle Documentation](https://github.com/jquery/sizzle/wiki/) — the rather sparse official documentation
