# CSS Rules and Selectors

Now let's get to CSS, and we'll start, perhaps, with decoding the abbreviation. CSS stands for Cascading Style Sheets, but:

_-- Why is it called "cascading"?_ -- this is a question I often ask candidates during interviews. The answer is an analogy, and it's a pretty good one: imagine a cascade of waterfalls, and you're standing on one of the steps holding an inkwell, pouring its contents into the water -- all the water below will be colored by the ink. Same with CSS: if you set a style at a certain level, it flows down the "cascade" to the web page elements, coloring them with the "ink" of your style.

_-- Why do I need all this?_ -- when working with jQuery, you need to be able to read CSS rules fluently and compose CSS selectors to find the elements you need on the page. Practically every task you'll solve with jQuery starts with finding the right element on the page, so **knowing CSS selectors is mandatory**.

But let's take it step by step. Here's a simple example of quite semantic HTML:

{% code fullWidth="false" %}
```html
<!DOCTYPE html>
<html dir="ltr" lang="en-US">
<head>
  <meta charset="UTF-8"/>
  <title>Page Title</title>
  <link rel="shortcut icon" href="/favicon.ico"/>
  <style>
    body {
      font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
      font-size: 14px;
    }
    h1, h2, h3 {
      color: #333333;
    }
    header, section, footer {
      position: relative;
      max-width: 800px;
      margin: 16px auto;
    }
    article {
      padding: 16px;
      margin-bottom: 16px;
    }
    #content {
      padding-bottom: 16px;
    }
    .box {
      border:1px solid #ccc;
      border-radius:4px;
      box-shadow:0 0 2px #ccc;
    }
  </style>
</head>
<body>
  <header>
    <h1>Page Title</h1>
    <p>Page Description</p>
  </header>
  <section id="content">
    <h2>Section Title</h2>
    <article class="box">
      <h3>Article Title</h3>
      <p>Lorem ipsum dolor sit amet, consectetuer adipiscing...</p>
    </article>
    <article class="box">
      <h3>Article Title</h3>
      <p>Morbi malesuada, ante at feugiat tincidunt...</p>
    </article>
  </section>
  <footer>
      © jQuery for beginners
  </footer>
</body>
</html>
```
{% endcode %}

This is an example of simple and proper HTML with a bit of CSS sprinkled in. Let's break down the selectors in the CSS code above (I intentionally kept the CSS inline rather than in a separate file, because it's more visual this way):

* `body` -- these rules will be applied to the `<body>` tag and all its descendants; remember: font settings cascade down through the element hierarchy
* `h1,h2,h3` -- we're selecting `<h1>`, `<h2>` and `<h3>` tags and setting the font color for these tags and their descendants
* `#content` -- we're selecting the element with `id="content"`; padding settings don't cascade to descendants, they only change for this specific element
* `.box` -- we're selecting elements with `class="box"` and modifying the border appearance of elements with that class

Now in more detail, with more advanced examples:

<table><thead><tr><th width="215">selector</th><th></th></tr></thead><tbody><tr><td><code>h1</code></td><td>finding elements by tag name</td></tr><tr><td><code>#container</code></td><td>finding an element by identifier <code>id=container</code> (<strong>identifiers are unique</strong>, meaning there should be only one on the page)</td></tr><tr><td><code>div#container</code></td><td>finding a <code>&#x3C;div></code> with identifier <code>container</code>, but the previous selector is faster, while this one has higher specificity</td></tr><tr><td><code>.news</code></td><td>selecting elements by class name <code>class="news"</code></td></tr><tr><td><code>div.news</code></td><td>all <code>&#x3C;div></code> elements with class <code>news</code> </td></tr><tr><td><code>#wrap .post</code></td><td>finding all elements with <code>class="post"</code> inside the element with <code>id="wrap"</code></td></tr><tr><td><code>.cls1.cls2</code></td><td>selecting elements with two classes <code>class="cls1 cls2"</code></td></tr><tr><td><code>h1, h2, .posts</code></td><td>listing selectors -- we'll select everything listed</td></tr><tr><td><code>.post > h2</code></td><td>selecting <code>&#x3C;h2></code> elements that are direct children of the element with class "post"</td></tr><tr><td><code>a + span</code></td><td>all <code>&#x3C;span></code> elements immediately following an <code>&#x3C;a></code> element will be selected</td></tr><tr><td><code>a[href^=https]</code></td><td>all <code>&#x3C;a></code> elements whose <code>href</code> attribute starts with <code>https</code> will be selected (presumably, all external links)</td></tr></tbody></table>

> This is by no means the full list; a description of all CSS selectors can be found on the corresponding W3C page: [https://www.w3.org/TR/selectors-3/](https://www.w3.org/TR/selectors-3/)

Going back to our waterfall analogy, imagine that on the next step someone pours ink of a different color. In real life, we'd get a mix of colors. But in CSS, this change will "repaint the water" on all subsequent steps, because in CSS what matters is order -- order and priorities. Let's get to the point:

1.  the first step of our cascade has the lowest priority -- these are the browser's default styles

    > no matter what browser vendors claim, these styles can differ across browsers, which is why there's still such a thing as [CSS Reset](https://www.google.com/search?q=CSS+Reset) -- a great equalizer
2. on the second step are styles set by the user deep in the browser settings; rarely seen in practice
3. the most important for us -- the page author's styles, but even here everything follows an order
   *   the lowest priority goes to styles linked as a file `<link rel="stylesheet" type="text/css" href="...">` or embedded inside HTML with the `<style>` tag:

       ```html
       <link rel="stylesheet" type="text/css" href="styles.css">
       <style>
         p {
           color: red;
         }
       </style>
       <p>
         This paragraph will be red
       </p>
       ```

       > and naturally, when priorities are equal, the last one declared wins
   *   then come the ones that bad people hardcoded (not you, you won't do this) in `style=""`:

       ```html
       <p style="color: blue">
         This paragraph will be blue
       </p>
       ```
   *   and then the `!important` flag shows up and breaks everything, because a rule with this flag becomes more important than even inline styles, turning everything upside down:

       ```html
       <style>
         p {
           color: red !important;
         }
       </style>
       <p style="color: blue">
         This paragraph will still be red!
       </p>
       ```
   *   to override such a style, the only option left is to write inline styles with the `!important` flag:

       ```html
       <style>
         p {
           color: red !important;
         }
       </style>
       <p style="color: blue !important">
         This paragraph will be blue!
       </p>
       ```
4. the `!important` adventure isn't over yet -- if there are user styles with this flag, now it's their turn
5. and even browsers use `!important`, and such styles have practically the highest priority

{% hint style="info" %}
There are also priority rules for [CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_animations/Using\_CSS\_animations) and [CSS transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_transitions/Using\_CSS\_transitions), but that's a whole different story
{% endhint %}

If your head isn't hurting yet, I'll also mention that when calculating whose rules take precedence, the [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) of selectors is analyzed, and here's how the math works:

* the calculation uses three weight positions -- `[0:0:0]`
* for each element identifier (`#id`) -- `[1:0:0]`
* for each class (`.class`) or pseudo-class (`:pseudo`) -- `[0:1:0]`
* for each tag (`<a>`, `<div>`, etc.) -- `[0:0:1]`
* where `[1:0:0]` > `[0:x:y]` > `[0:0:x]`
* when the score is tied -- again, the last one declared wins

Here are selectors arranged by ascending priority (the corresponding HTML code will follow):

<table><thead><tr><th width="608">selector</th><th>priority</th></tr></thead><tbody><tr><td>a tag has the lowest priority</td><td><code>[0:0:1]</code></td></tr><tr><td><code>p { color: orange }</code></td><td></td></tr><tr><td>adding class <code>intro</code> to the tag</td><td><code>[0:1:1]</code></td></tr><tr><td><code>p.intro { color: green }</code></td><td></td></tr><tr><td>adding another tag</td><td><code>[0:1:2]</code></td></tr><tr><td><code>article p.intro { color: blue }</code></td><td></td></tr><tr><td>... we need more classes</td><td><code>[0:2:2]</code></td></tr><tr><td><code>article.news p.intro { color: red }</code></td><td></td></tr><tr><td>the identifier <code>id="pinned"</code> on its own outweighs all tags and classes combined</td><td><code>[1:0:0]</code></td></tr><tr><td><code>#pinned { color: darkblue }</code></td><td></td></tr><tr><td>adding the <code>&#x3C;p></code> tag, and specificity increases</td><td><code>[1:0:1]</code></td></tr><tr><td><code>p#pinned { color: darkcyan }</code></td><td></td></tr><tr><td>adding another identifier <code>id="top"</code></td><td><code>[2:0:1]</code></td></tr><tr><td><code>#top p#pinned { color: darkgreen }</code></td><td></td></tr><tr><td>using <code>style</code> overrides any rules from external files and stylesheets</td><td>¯\_(ツ)_/¯</td></tr><tr><td><code>&#x3C;p style="color:#333">...&#x3C;/p></code></td><td></td></tr></tbody></table>

<details>

<summary>HTML page example</summary>

```html
<!DOCTYPE html>
<html dir="ltr" lang="en-US">
<head>
  <meta charset="UTF-8"/>
  <title>CSS Priority</title>
</head>
<body>
<section id="content" class="wrapper">
  <h2>Section Title</h2>
  <article id="top" class="news">
    <h3>Article Title</h3>
    <p id="pinned" class="intro">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
      Curabitur semper imperdiet felis, sit amet imperdiet arcu
      varius et. Donec condimentum pulvinar sollicitudin.
    </p>
  </article>
  <article class="news">
    <h3>Article Title</h3>
    <p class="intro" style="color:#333">
      Sed rutrum accumsan ultricies. Nunc iaculis enim vel augue 
      porta pellentesque. Nunc efficitur ex non ullamcorper ultricies.
      Nunc tempus vulputate enim, non egestas orci sodales eget.
    </p>
  </article>
</section>
</body>
</html>
```

</details>

There are also specificity calculation rules related to certain pseudo-selectors like `:not()` and `:where()`. The pseudo-selector itself doesn't add weight to specificity, but the rule inside the parentheses does:

```css
/* [0:1:0] */
.red {
  color: red;
}

/* [0:1:1] */
.red:not(div) {
  color: red;
}
```

Yeah, calculating specificity can genuinely become a non-trivial task, so here are a couple of calculators for you to choose from:

* [https://www.codecaptain.io/tools/css-specificity-calculator](https://www.codecaptain.io/tools/css-specificity-calculator)
* [https://polypane.app/css-specificity-calculator/](https://polypane.app/css-specificity-calculator/)

{% hint style="warning" %}
Remember that when applying CSS rules, the key factor is the specificity level of the selector. Only when specificities are equal does the order of style inclusion matter:

```html
<style>
  .red {
    color: red;
  }
  .blue {
    color: blue;
  }
</style>
<p class="red blue">
  This paragraph will be blue
</p>
```
{% endhint %}

{% hint style="danger" %}
And let me repeat: the `!important` flag is a scary thing. Use it only as a last resort. Such a rule has the highest priority, even though it has nothing to do with specificity.
{% endhint %}

***

#### Homework

Here's a piece of CSS for practice -- write the corresponding HTML for it (this is an interview question ;):

```css
#my p.announce, .tt.pm li li a:hover + span { 
    color: #f00;
}
```

And one more:

```css
#my > li, dd.dd.tt ~ span {
    text-decoration: underline;
}
```
