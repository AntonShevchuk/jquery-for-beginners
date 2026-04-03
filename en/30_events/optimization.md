# Optimization

A hidden bonus of using "live" events is the optimization opportunity. Let me tell you about the "suitable" cases.

Case one, the obvious one — imagine a table with a thousand rows and a dozen columns, and now try to calculate how much memory `click` event handlers will eat up for each cell:

```javascript
$("table td").on("click", function() { 
  /* ... */
});
```

I'm at a loss too. But I know exactly how to rewrite it to save memory and time:

```javascript
$("table").on("click", "td", function() { 
  /* ... */
});
```

Case two, a contrived one — you need to log user actions on the page, meaning you need to track clicks on an innumerable number of objects:

```javascript
$("body").on("click", "*", function() {
    console.info("Click on "+this.tagName);
});
```

To see this "contrived" example in action, open the sandbox, run the code above, and with the developer console open, click on some elements:

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

Try to explain what's happening.
