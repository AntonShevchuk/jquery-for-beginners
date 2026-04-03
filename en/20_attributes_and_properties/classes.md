# CSS Classes

Alright, seems like we've figured out CSS, although wait — we should also cover class manipulations, another one of those essential skills:

<table data-header-hidden><thead><tr><th width="413">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">addClass(className)
</code></pre></td><td>add a class to an element</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">addClass(function(index, currentClass){
  return className
})
</code></pre></td><td>add a class using a callback function</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">hasClass(className)
</code></pre></td><td>check if an element has a specific class</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeClass(className)
</code></pre></td><td>remove a class</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeClass(function(index, currentClass){
  return className
})
</code></pre></td><td>remove a class using a callback function</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">toggleClass(className)
</code></pre></td><td>toggle a class</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">toggleClass(className, state)
</code></pre></td><td>toggle a class based on the <code>state</code> flag</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>toggleClass(function(index, currentClass, state) {
</strong>  return className
}, state)
</code></pre></td><td>toggle a class using a callback function, the <code>state</code> flag is optional</td></tr></tbody></table>

{% hint style="info" %}
In the methods listed above, `className` can be a string containing a list of classes separated by spaces.
{% endhint %}

> I've never once had to use these methods with callback functions, and only once did the "`state`" flag come in handy, so don't bother memorizing all of this. Going forward, when quoting the jQuery docs, I'll deliberately skip some of these "features".

But enough translating the official documentation, let's move on to some visual examples.

First up — adding classes:

```javascript
// add the "active" class
$("#my").addClass("active")

// add multiple classes at once
$("#my").addClass("active notice")
```

Toggling classes is also a popular method:

```javascript
// toggle the "active" class:
//  - remove the class if it exists
//  - add it if it doesn't
$("#my").toggleClass("active")

// toggle multiple classes
$("#my").toggleClass("active notice")
```

Here's how toggling multiple classes works:

```markup
<div id="my" class="active notice"> → <div id="my" class="">

<div id="my" class="active"> → <div id="my" class="notice">

<div id="my" class=""> → <div id="my" class="active notice">
```

And removal examples, for the complete set, so to speak:

```javascript
// remove the "active" class
$("#my").removeClass("active") 

// remove multiple classes at once
$("#my").removeClass("active notice")
```

An interactive example:

{% embed url="https://anton.shevchuk.name/book/code/class.html" %}
