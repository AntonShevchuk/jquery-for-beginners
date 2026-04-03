---
description: Or dump everything together and suffer
---

# Divide and Conquer

Here are a few simple rules to remember:

* put JavaScript in external files
* never write inline event handlers `onclick="some()"`
* put CSS in external files
* never write inline styles `style="color:red"`
* and one more time for good measure -- never write inline!

Now let me show you some code that deserves a good beating (this is an example of bad code, just to be clear for those who don't get hints):

```markup
<script>
function doSomething(){ /* ... */ }
/* crack! that's the wrist bones, so they can't type anymore */
</script>
<style>
p { line-height:20px; }
/* crunch... the shinbone, and they won't be going to work */
</style>
<div style="color:red;font-size:1.2em">
    <p onclick="doSomething();">Lorem ipsum dolor sit amet...</p>
    <!-- bang, head on the desk... dead, as a mercy kill -->
</div>
```

Not clear why this is bad? Looks like you've never had to redesign an existing website :) Let me clarify the problem: you get a task -- "_change the font color across all pages of the site_", and there could be three dozen of them. These might not even be just HTML files, but pages of some [template engine](https://en.wikipedia.org/wiki/Template_processor), scattered across twenty folders (and that's not even the worst case). And then it appears -- the red paragraph. The probability of hearing "words of encouragement" directed at the author of this code approaches one. The situation with inline event handlers is similar. Just imagine -- you're writing JavaScript code, everything's great, everything works, until you try to click on the red paragraph. It turns out to be beyond your control, living its own life, ignoring all your efforts. You look at the code, and once again someone hears those words...

By following these four rules of the "red paragraph", you should end up with clean and predictable HTML:

```markup
<div id="abzac">
    <p>Lorem ipsum dolor sit amet...</p>
</div>
```

Styles can be easily applied to the `<div>` with the identifier, and so can the event handler for our paragraph.

> In Russian, "abzac" (paragraph) also means "the end" (colloquially) -- but for the sake of a catchy name and easy learning, it'll do
