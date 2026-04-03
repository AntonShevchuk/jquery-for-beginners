# Creating Elements

Let's start with creating elements for further work with them. The [documentation](https://api.jquery.com/jQuery/#jQuery2) kindly tells us that it's all pretty simple:

```javascript
var $myDiv = $('<div id="my" class="some"></div>');
```

This example works just fine, but it won't shine in terms of performance, because internally it'll all be parsed using the `jQuery.parseHTML()` method, which isn't exactly fast. But we can help the parser by passing the element's attributes as a second parameter:

```javascript
var $myDiv = $("<div>", { "id":"my", "class":"some" });
```

We can make it even simpler:

```javascript
var $myDiv = $("<div>").attr({ "id":"my", "class":"some" });
```

And this approach will work a tiny bit faster. But why? To answer that question, take a look inside jQuery's code, at the main `init()` function. In its code you'll find the algorithm for parsing the previous example:

1. Parse the string and create a DOM element in a jQuery wrapper
2. Enter a **loop** to process the passed parameters:
   1. Check if the element has a function with that name
   2. If not, set the element's attribute using the `attr()` method

Draw your own conclusions about where we lost time there :)

And finally, let me describe the fastest approach, which I use often:

```javascript
var myDiv = document.createElement("div");

myDiv.id = "my";
myDiv.className = "some";
```

Yes, this is "pure" JavaScript, but if you ask me — in this case it's no less convenient than any framework.
