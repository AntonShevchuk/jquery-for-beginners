# Manipulating Elements

_— So we've created an element, what do we do with it now?_\
_— Let's add it to the page — every element deserves its own DOM._

It's time to learn how to add elements to a page. All the methods we need are gathered in one section of the documentation — [Manipulation](https://api.jquery.com/category/manipulation/). We've already met some of them, and there's just a little bit left:

<table data-header-hidden><thead><tr><th width="260">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">after(content)
</code></pre></td><td>inserts content after each element in the selection, i.e. if you see the line <code>$('p').after('&#x3C;hr/>')</code>, read it as "<em>after</em> each paragraph a line will be added"</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">insertAfter(element)
</code></pre></td><td>inserts elements from the selection after each element passed as an argument, i.e. if you see the line <code>$('&#x3C;hr/>').insertAfter('p')</code>, read it as "a line will be <em>inserted after</em> each paragraph"</td></tr></tbody></table>

> _— Hmm, I don't see the difference!_
>
> _— It's easy, look closer:_
>
> ```javascript
> $("found element").after("what to add after it?")
> $("what to add").insertAfter("after which element?")
> ```

Let's repeat. Take some arbitrary text and add it after each paragraph:

```javascript
$('p').after('(^_^)ノ')
```

If we want to add a new DOM element, we can pass an HTML string as the parameter and hope that jQuery figures out what to do with it:

```javascript
$('p').after('<hr/>')
```

If we want to move a DOM element, we'll need to explicitly "select" it using a jQuery call:

```javascript
$('p').after($('h1'))
```

If we don't do that, we'll get a completely different result:

```javascript
$('p').after('h1')
```

Good, we've covered the `after()` method — next up is the `before()` method:

<table data-header-hidden><thead><tr><th width="271">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">before(content)
</code></pre></td><td>inserts content before each selected element</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">insertBefore(element)
</code></pre></td><td>inserts elements from the selection before each element passed as an argument</td></tr></tbody></table>

> _— OK, I think I'm starting to get it:_
>
> ```javascript
> $("found element").before("what to add before it?")
> $("what to add").insertBefore("before which element?")
> ```

{% embed url="https://anton.shevchuk.name/book/code/dom.after-and-before.html" %}

***

The following methods take the target elements and modify them:

<table data-header-hidden><thead><tr><th width="271">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">append(content)
</code></pre></td><td>inserts content at the end of each element in the selection, i.e. the line <code>$("p").append("&#x3C;hr/>")</code> should be read as "a line will be added to the end of each paragraph"</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">appendTo(element)
</code></pre></td><td>inserts the selected content at the end of each element passed as an argument;<br><code>$("&#x3C;hr/>").appendTo("p")</code> — "a line will be added to the end of each paragraph"</td></tr></tbody></table>

> _— Again about the difference:_
>
> ```javascript
> $("found element").append("what to append there?")
> $("what to append").appendTo("to which element?")
> ```

<table data-header-hidden><thead><tr><th width="272">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">prepend(content)
</code></pre></td><td>inserts content at the beginning of each element in the selection</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">prependTo(element)
</code></pre></td><td>inserts the selected content at the beginning of each element passed as an argument</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/dom.append-and-prepend.html" %}

***

Alright, we've sorted out this chunk of documentation. Again, feel the difference between the listed methods, because there are more coming:

<table data-header-hidden><thead><tr><th width="276">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">replaceWith(content)
</code></pre></td><td>replaces the found elements with a new one:<br><code>$("p").replaceWith("&#x3C;hr/>")</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">replaceAll(target)
</code></pre></td><td>inserts content in place of the found elements:<br><code>$("&#x3C;hr/>").replaceAll("h3")</code></td></tr></tbody></table>

> _— Ah, right:_
>
> ```javascript
> $("found element").replaceWith("what to replace it with?")
> $("what to insert").replaceAll("instead of what?")
> ```

{% embed url="https://anton.shevchuk.name/book/code/dom.replace.html" %}

***

<table data-header-hidden><thead><tr><th width="271">method</th><th>description</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">wrap(element)
</code></pre></td><td>wraps each found element with a new element; <br><br>think of it as taking candies from a box and wrapping each one</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">wrapAll(element)
</code></pre></td><td>wraps the found elements with a new element;<br><br>we take a bunch of candies and wrap them in one big wrapper</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">wrapInner(element)
</code></pre></td><td>wraps the content of each found element with a new element; <br><br>take the candies, unwrap each one, wrap in a new wrapper, then put the original wrapper back on top</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">unwrap()
</code></pre></td><td>removes the parent element of the found elements; wrappers off!</td></tr></tbody></table>

> Sure, I could pretend to be a smarty-pants here, but in reality I'm constantly peeking at method names too when working with the DOM.

{% embed url="https://anton.shevchuk.name/book/code/dom.wrap.html" %}

***

Here's a homework assignment for you — write a script that adds the following HTML to the page:

```markup
<div id="my">
  <div id="precious">
    <p class="gold">Ring</p>
  </div>
</div>
```
