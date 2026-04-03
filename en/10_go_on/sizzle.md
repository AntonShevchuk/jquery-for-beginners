# Selectors and Sizzle

{% hint style="info" %}
Skip this section and come back to it when you're curious about how element searching works inside `$(...)`
{% endhint %}

As its "search engine" for DOM elements, jQuery uses the [Sizzle](https://github.com/jquery/sizzle/wiki) library. This library was once an integral part of jQuery, then it "branched off" into a separate project, which is happily used by both jQuery itself and Dojo Toolkit. But enough backstory, let's get straight to the searching. For this purpose, JavaScript provides the following methods (not jQuery, not Sizzle, but JavaScript):

| `getElementById(id)`            | search by `id="…"`                    |
| ------------------------------- | ------------------------------------ |
| `getElementsByName(name)`       | search by `name="name"` |
| `getElementsByClassName(class)` | search by `class="class"`             |
| `getElementsByTagName(tag)`     | search by tag name                  |
| `querySelectorAll(selector)`    | search by any CSS selector |

Glancing quickly through the list, you might notice the `querySelectorAll()` method — it's universal and truly convenient. And this is exactly the method the library tries to call when you feed something as a selector into jQuery. But this method sometimes lets us down, and that's when Sizzle takes the stage in all its glory, armed with all the methods mentioned above, plus its own unique arsenal:

```javascript
if (document.querySelectorAll) (function(){
    var oldSelect = select;
    /* ... */
    select = function( selector, context, results, seed, xml ) {

    // use querySelectorAll when there are no filters in the query,
    // when it's not a query against an xml object,
    // and when no issues with the query have been detected
    // there are a couple more checks that I've omitted for clarity

    try {    
        push.apply(
            results,            
            slice.call(context.querySelectorAll( selector ), 0)        
        );
        return results;
    } catch(qsaError) { /* let us down, again, how many times now */ }
        /* ... */
        // Sizzle enters
        return oldSelect( selector, context, results, seed, xml );
    };
});
```

But let's already look at how Sizzle searches the DOM when the `document.querySelectorAll()` method does stumble. Let's start by breaking down the algorithm using this example:

```javascript
$("thead > .active, tbody > .active")
```

1. Get the first expression before the comma: "`thead > .active`"
2. Split it into chunks: "`thead`", "`>`", "`.active`"
3. Find the initial set of elements by the last chunk (all "`.active`")
4. Filter from right to left (all "`.active`" that are directly inside "`thead`")
5. Look for the next query after the comma (go back to step one)
6. Keep only unique elements in the result

Looking at this algorithm, you should notice that **rules are read from right to left**!

Let's dig deeper. When analyzing even the smallest expression, there are several nuances worth paying attention to. The first is search priority — it differs across browsers (depending on their capabilities, or "incapabilities"):

```javascript
{
    order: new RegExp( "ID|TAG" +
        (assertUsableName ? "|NAME" : "") +
        (assertUsableClassName ? "|CLASS" : "")
    )
}
```

> Don't mind the RegExp — that's Sizzle's internal plumbing.

So, when examining the expression `div#my`, Sizzle will first find the element with `id="my"`, and only then check if it matches `<div>`. Alright, this is basically filtering, and it also follows a specific order — that's the second nuance:

```javascript
{
    preFilter: {    
        "ATTR": function (match) { /* ... */ },
        "CHILD": function (match) { /* ... */ },
        "PSEUDO": function (match) { /* ... */ }
    },    
    filter: {    
        "ID": function (id) { /* ... */ },
        "TAG": function (nodeName) { /* ... */ },
        "CLASS": function (className) { /* ... */ },
        "ATTR": function (name, operator, check) { /* ... */ },
        "CHILD": function (type, argument, first, last) { /* ... */ },
        "PSEUDO": function (pseudo, argument, context, xml) { /* ... */ }
    }
}
```

The third nuance is relative filters, which work with two elements at once (you know them from CSS selectors):

```javascript
{
    relative: {    
        ">": { dir: "parentNode", first: true },    
        " ": { dir: "parentNode" },    
        "+": { dir: "previousSibling", first: true },    
        "~": { dir: "previousSibling" }    
    }
}
```

> Oops, why am I dumping all this on you? Better read about query optimization in the next chapter.

The official Sizzle documentation is available on the [project's GitHub](https://github.com/jquery/sizzle/wiki/).
