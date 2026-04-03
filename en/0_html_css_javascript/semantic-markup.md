# Semantic Markup

Semantic HTML markup means using tags for their intended purpose -- i.e. if you need a heading, here's your `<h1>` tag and its siblings; need a tabular data representation -- use the `<table>` tag, and only it.

> Sometimes, in the pursuit of eliminating table-based layouts, people go to absurd extremes, and the `<table>` tag becomes an outcast, no longer used even for actual tabular data. Don't make this mistake.

Looking slightly ahead, it's worth mentioning HTML5 tags: `<article>`, `<aside>`, `<header>`, `<footer>`, `<menu>`, `<section>`, etc. -- use them, don't be afraid.

> Being fearless is the right attitude, but you still need to use them wisely. I recommend the resource [https://html5doctor.com/](https://html5doctor.com/) -- it has a really thorough breakdown of HTML5 spec additions.
>
> And a couple more interesting resources for good measure:
>
> * [https://developer.mozilla.org/](https://developer.mozilla.org/en-US/docs/Web) -- a reference for web developers
> * [https://web.dev/explore](https://web.dev/explore) -- there's a whole community here

Try to avoid redundant elements on the page. Most HTML pages are guilty of having unnecessary block elements:

```html
<div id="header">
    <div id="logo">
        <h1><a href="/">My Blog</a></h1>
    </div>
    <div id="description">
        <h2>Here I share my thoughts</h2>
    </div>
</div>
```

This structure can easily be simplified, making the code more readable, with minimal or even zero CSS changes required:

```html
<header>
    <h1>
        <a href="/">My Blog</a>
    </h1>
    <h2>Here I share my thoughts</h2>
</header>
```

> In English there's a term "divitis" -- it's used to describe HTML markup with excessive use of divs for no good reason. Usually beginners fall into this trap, wrapping elements in divs just to apply CSS styles, which leads to divs multiplying like rabbits.

Another mandatory step for creating "proper" HTML is using class names and identifiers that clearly describe the element's content, rather than some styling detail. Here's an example:

<table><thead><tr><th width="242">Bad</th><th></th></tr></thead><tbody><tr><td><code>red</code>, <code>green</code> etc.</td><td>at some point you'll want to change the design via CSS -- and the element with class "red" will turn blue; or you'll be running around all the pages changing red to blue, only to start all over again later</td></tr><tr><td><code>wide</code>, <code>small</code> etc.</td><td>wide today, and tomorrow?</td></tr><tr><td><code>h90w490</code></td><td>presumably an element with height 90px and width 490px; or am I wrong? and does this class even fit on a smartphone screen?</td></tr><tr><td><code>b_1</code>, <code>px_9</code></td><td>meaningless names that you'll forget by tomorrow</td></tr><tr><td><code>color1</code>, <code>color2</code>, ...</td><td>sometimes seen in "skinned" sites, but such classes are born out of laziness</td></tr><tr><td><code>element1</code>...<code>20</code></td><td>this also happens and nothing good comes of it</td></tr></tbody></table>

And examples of proper naming:

<table><thead><tr><th width="245">Good</th><th></th></tr></thead><tbody><tr><td><code>logo</code>, <code>content</code></td><td>the logo, main content</td></tr><tr><td><code>menu</code>, <code>submenu</code></td><td>menu and submenu</td></tr><tr><td><code>even</code>, <code>odd</code></td><td>even and odd list items (though for such tasks it's easier to use CSS operators <code>:nth-child(even)</code> and <code>:nth-child(odd)</code>, again because elements might shift around in the future)</td></tr><tr><td><code>navigation</code></td><td>page navigation</td></tr><tr><td><code>copyright</code></td><td>licensing information</td></tr></tbody></table>

There's one more thing -- HTML and CSS code formatting. I won't dwell on it, but all code in this book will be formatted with proper indentation, and hopefully that will rub off on your own work.

> Almost forgot -- don't model your classes after things like `p-X`, `m-X` that you'll see in [Bootstrap](https://getbootstrap.com/) or [Tailwind](https://tailwindcss.com/). The "utility first" concept works great for CSS frameworks, but for development without them, such naming is uninformative.
