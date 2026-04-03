---
description: >-
  This section will be useful for beginner front-end developers and anyone who wants to do
  a bit more than just showing an alert box on click.
---

# CSS. Deep Dive

## On Formatting <a href="#codestyle" id="codestyle"></a>

No, I can't help myself, and I'll go ahead and show you the CSS formatting style I use:

```css
/* Header */
header {
    margin-bottom: 16px;
    font-weight: 400;
}

header h1 {
    color: #999;
}

header p {
    font-size: 1.4em;
    margin-top: 0;
}
/* /Header */
```

Why this is good:

* such CSS is easy to read
* there's a block start marker (you can quickly find the right section even in a huge CSS file by searching for the label `* header`)
* this formatting clearly indicates element nesting
* and you can easily trace property inheritance

I'm not insisting on my approach, but I'd like you to adopt one of the many formatting standards and always stick with it.

> When you become seasoned front-end developers, you'll discover the full power of CSS preprocessors. But for now, just listen and remember.

This visual complexity is meant for humans; browsers don't care about it at all, since they perform concatenation anyway when processing styles -- removing line breaks, indentation, comments, and non-critical spaces. Some types of web servers, for example those for banking thin clients, perform compression (and archiving) of files before sending them to where they're used, to save traffic and money.

## Naming Classes and Identifiers <a href="#naming-convention" id="naming-convention"></a>

I already touched on this topic when talking about HTML markup, so here's the thing -- class names can even look like this: `b-service-list__column b-service-list__column_right` and that would be cool, and a "must have" -- but only within the scope of truly large projects. Actually, why am I going on about this? I'll just give you the [starting point](https://en.bem.info/methodology/) for further reading -- there's enough material there for a whole separate book ;)

{% hint style="info" %}
I recommend getting familiar with [BEM principles](https://en.bem.info/methodology/) -- it's useful for broadening your horizons and leveling up your skills.
{% endhint %}

## About Colors <a href="#css-colors" id="css-colors"></a>

The web uses the [RGB](https://www.w3.org/TR/css-color-3/#rgb-color) color model, named after the English names of the "carrier" color channels -- red, green, blue -- which relies on numerical notation for any shade. So the color red can be written not just as "red", but in several other ways too:

```css
p { color: red }

p { color: #ff0000 }

p { color: #f00 }   /* shorthand notation, saves 3 bytes */

p { color: rgb(255, 0, 0) }
```

> Now you should be able to name colors `#f00`, `#0f0`, `#00f` without hesitation, and those who got an A in art class will also name `#ff0`, `#0ff` and `#f0f` ;)

CSS 3 supports the enhanced [RGBA](https://www.w3.org/TR/css-color-3/#rgba-color) model, where we can additionally set the alpha channel value, i.e. transparency:

```css
p { color: rgba(255, 0, 0, 1) }   /* regular text */

p { color: rgba(255, 0, 0, 0.5) } /* semi-transparent text */
```

Another CSS 3 goody is the ability to use the [HSL](https://www.w3.org/TR/css-color-3/#hsl-color) (hue, saturation, lightness) and [HSLA](https://www.w3.org/TR/css-color-3/#hsla-color) (HSL + alpha channel) color models:

```css
p { color: hsl( 0, 100%, 50%) }   /* red */

p { color: hsl(120, 100%, 50%) }  /* green */

p { color: hsl(240, 100%, 50%) }  /* blue */

p { color: hsla( 0, 100%, 50%, 0.5) } /* semi-transparent red */
```

There's a simple algorithm for converting from the HSL model to RGB, but don't burden yourself with it just yet.

> Who even uses HSL? ~~Don't bother!~~ HSL is becoming increasingly popular, and you should know about it, and even understand how it works!

For those of you stumped by the RGB channel mixing question, here's a visual guide:

![](../../.gitbook/assets/colors.gif)

## Block and Inline Elements <a href="#block-and-inline" id="block-and-inline"></a>

You might not know this yet, but HTML tags are divided into block-type and inline-type. Block elements are those that render as a rectangle, take up the full available width inside their parent element, and their height is determined by their content. Block tags start and end on a new line by default -- these are `<div>`, `<h1>` and siblings, `<p>`, and others.

If you want your HTML to stay valid, make sure block elements are not placed inside inline elements. Inside inline tags, there can only be text or other inline elements.

> One of the most common mistakes I see is people trying to stuff a list `<ul>` or a button `<button>` inside a paragraph `<p>`. Don't do that.&#x20;

Useful articles on the topic:

* [Inline Elements List and What's New in HTML5](https://www.tutorialchip.com/tutorials/inline-elements-list-whats-new-in-html5/)
* [HTML5 Block Level Elements: Complete List](https://www.tutorialchip.com/tutorials/html5-block-level-elements-complete-list/)
* [Box Model](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model)
* [CSS Layout: Normal Flow](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Normal_Flow)

## About Block Element Dimensions <a href="#size" id="size"></a>

I also want to specifically address the calculation of width and height for block elements, because there's a nuance here. By default, the height and width of elements are calculated without accounting for border thickness and padding, like this:

![content-box](../../.gitbook/assets/box-height-1.png)

This box model is called "content-box", and CSS3 introduced the ability to change the box model by specifying the "box-sizing" property. Great, now we can choose between two values: "content-box" and "border-box". I already described the first one; the second calculates height and width including padding and border thickness:

![border-box](../../.gitbook/assets/box-height-2.png)

Useful articles on the topic:

* [Block-level Elements](https://developer.mozilla.org/en-US/docs/Glossary/Block-level_content)
* [Inline Elements](https://developer.mozilla.org/en-US/docs/Glossary/Inline-level_content)

## Floating Elements <a href="#float" id="float"></a>

I'd love to tell you more about the CSS `float` property, but I'm afraid the story would be long and tedious. In short: if you set the `float` property on an element, then:

* our element will be shifted horizontally and "stick" to the specified edge of the parent element
* if it was a block element, it will no longer take up the full width of the parent element, freeing up space
* if block elements follow it, they will take its place
* if inline elements follow it, they will wrap around our element on the free side

This is the "default" behavior, and you can see how it looks live in the example [css.float.html](https://anton.shevchuk.name/book/code/css.float.html).

The main thing here is to understand what's happening and be able to control it -- that is, if you want to learn at least a little bit about CSS layout :)

{% hint style="info" %}
Understanding how `float` works in CSS is one of the many skills a front-end developer should have. For a general overview, I recommend the article "[CSS Layout: float](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Floats)"
{% endhint %}

## Positioning <a href="#position" id="position"></a>

I'll just give you a brief intro to `position`. It has five main values:

* `static` -- the "default" state of affairs; elements are placed in the normal document flow, one after another from top to bottom
* `absolute` -- the element is positioned relative to its nearest positioned ancestor (not `static`), according to specified coordinates
* `fixed` -- the element is fixed at a certain position in the viewport, not moving when the page is scrolled
* `relative` -- the element behaves like `static`, but can be offset from its normal position; it also serves as a positioning context for inner "absolute" elements
* `sticky` -- the element "sticks" to a certain position in the viewport when the page is scrolled past a given threshold, working as a combination of `relative` and `fixed`

{% hint style="info" %}
For self-study:

-- [CSS Layout: Positioning](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Positioning) -- always up to date on MDN

-- [CSS-position](https://developer.mozilla.org/en-US/docs/Web/CSS/position) -- docs on dev.mozilla.org are always up to date

-- [MDN CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS) -- the definitive CSS reference
{% endhint %}
