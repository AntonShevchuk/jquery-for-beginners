---
description: The $.Deferred object
---

# Deferred

Let's start with the Deferred object — understanding it and knowing how to use it is basically next-level wizardry. Let's see how it works, open up our sandbox:

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

Run this script in the console:

```javascript
// initialize the Deferred object
// status is "pending"
var D = $.Deferred();

// attach handlers
D.then(function() { console.log("first") });
D.then(function() { console.log("second") });

// change the status to "fulfilled" — "completed successfully"
// to do this we call resolve()
// for clarity, let's wait a second
// our handlers will be called in order, but they don't wait for each other
setTimeout(() => D.resolve(), 1000)

// this handler is attached too late and will be called immediately
D.then(function() { console.log("third") });
```

{% hint style="info" %}
The examples above will work on any page where jQuery is included ;)
{% endhint %}

If we translate all this into plain English, we get the following scenario:

* if everything goes well, then run this function and output "`first`"
* and also this function — "`second`"
* `resolve()` — we found out everything is fine
* ~~if~~ everything is fine, run the function and output "`third`"

Besides "happy end" scenarios, there are also sad stories when things don't go as we'd like:

```javascript
// initialize the Deferred object
var D = $.Deferred();

// attach handlers
D.then(function() { console.info("done") });
D.catch(function() { console.error("fail") });

// change the status to "rejected" — "completed with error"
// for clarity, let's wait a second
setTimeout(() => D.reject(), 1000)

// in the console we'll only see "fail" :(
```

So it goes like this:

* if everything goes well, then run this function — "`done`"
* if everything goes wrong, then this function will output "`fail`"
* oops, everything went wrong

In reality, the `then()` method allows you to attach handlers for both the positive scenario and the error case simultaneously:

```javascript
D.then(
    function(result) { console.info("done") },
    function(error) { console.error("fail") }
);
```

Does this make the code more readable? I doubt it, but this pattern exists and is used everywhere. Personally, I prefer using the `catch()` method to handle errors, which is essentially just shorthand for `then(null, fn)`:

```javascript
var D = $.Deferred();

// attach error handler via then()
D.then(null, function() { console.error("fail") });

// attach error handler via catch()
D.catch(function() { console.error("again fail") });
```

I'll also mention the `always()` method — it adds handlers that will execute regardless of the outcome (under the hood, it calls `done(arguments)` and `fail(arguments)`).

To avoid confusion with all these methods, here's a flowchart:

<figure><img src="../../.gitbook/assets/deferred.svg" alt=""><figcaption></figcaption></figure>

When calling [`resolve()`](https://api.jquery.com/deferred.resolve/) and [`reject()`](https://api.jquery.com/deferred.reject/), you can pass arbitrary data to the registered callback functions for further processing:

```javascript
// initialize the Deferred object
var D = $.Deferred();

// attach handlers
D.then(function(message) { console.log(`first: ${message}`) });
D.then(function(message) { console.log(`second: ${message}`) });

// call resolve()
D.resolve("A-a-a")
```

On top of that, there are [`resolveWith()`](https://api.jquery.com/deferred.resolveWith/) and [`rejectWith()`](https://api.jquery.com/deferred.rejectWith/) methods — they let you change the context of the called callback functions (i.e., inside them "`this`" will point to the specified context):

```javascript
/// initialize the Deferred object
var D = $.Deferred();

// electric counter
function Counter () {
  this.counter = 0
  this.tick = function() {
    this.counter++
    console.log(`tick: ${this.counter}`)
  }
}

var counter = new Counter()

// attach handlers
// this will point to our counter
D.then(function() { this.tick() });
D.then(function() { this.tick() });

// call resolveWith()
D.resolveWith(counter)
```

One more thing worth noting — if you're going to pass the Deferred object to someone else so they can attach their event handlers, but you don't want to lose control, then return not the object itself, but the result of the `promise()` method — effectively, this will be the same object in "read-only" mode:

<table><thead><tr><th width="366">available methods</th><th>will be unavailable</th></tr></thead><tbody><tr><td><code>then</code>, <code>done</code>, <code>fail</code>, <code>always</code>, <code>pipe</code>, <code>progress</code>, <code>state</code> and <code>promise</code></td><td><code>resolve</code>, <code>reject</code>, <code>notify</code>, <code>resolveWith</code>, <code>rejectWith</code>, and <code>notifyWith</code></td></tr></tbody></table>

And besides the "waiting for a miracle" behavior, with Deferred you can build call chains — "live queues":

```javascript
var D = $.Deferred();

D.then(function() {
  // waiting for resolve()
  console.log("hide images")
  return $("article img").slideUp(2000).promise()
})
.then(function(){
  // wait until images are hidden
  console.log("hide paragraphs")
  return $("article p").slideUp(2000).promise()
})
.then(function(){
  // wait until paragraphs are hidden
  console.log("hide articles")
  return $("article").hide(2000).promise()
})
.then(function(){
  // all done, boss
  console.info("done")
});

// one second and it's all happening
setTimeout(() => D.resolve(), 1000)
```

We've already implemented similar behavior using the `animate()` method, but we want to find our own way:

{% hint style="info" %}
Before jQuery 1.8, we used the `pipe()` method here, but now it's `then()`
{% endhint %}

In this example, we call the `then()` method and pass it a callback function that should return a Promise object. This is necessary to maintain order in the queue — try removing one "`return`" from the example, and you'll notice the next animation starts without waiting for the previous one to finish:

```javascript
var D = $.Deferred();

D.then(function() {
  // waiting for resolve()
  console.log("hide images")
  $('article img').slideUp(2000).promise()
})
.then(function(){
  // wait until images are hidden
  console.log("hide paragraphs")
  $('article p').slideUp(2000).promise()
})
.then(function(){
  // wait until paragraphs are hidden
  console.log("hide articles")
  return $('article').hide(2000).promise()
})
.then(function(){
  // all done, boss
  console.info("done")
});

// one second and it's all happening
setTimeout(() => D.resolve(), 1000)
```

{% embed url="https://anton.shevchuk.name/book/code/deferred.html" %}

But the Deferred possibilities don't end here. There's also a pair of methods `notify()` and `progress()` — the first sends messages to callback functions registered by the second. Here's a clear example to demonstrate (open the console and see what happens):

```javascript
var D = $.Deferred();

var money = 100; // our budget

// spending money
D.progress(function(cost){
    console.log(`${money} - ${cost} = ${money-cost}`);
    if (money < cost) { // out of money
        D.reject();
        return;
    }
    money -= cost;
});

// making purchases
setTimeout(function(){ D.notify(40); }, 500);  // purchase 1
setTimeout(function(){ D.notify(50); }, 1000); // purchase 2
setTimeout(function(){ D.notify(30); }, 1500); // purchase 3
setTimeout(function(){ D.resolve();  }, 3000); // bought everything

D.then(function(){ console.info("All Ok") });
D.catch(function(){ console.error("Insufficient Funds") });
```

***

> Experience the full power of Deferred!

Let's create a service for searching images on Flickr. We'll start with an object that handles loading data from Flickr:

```javascript
var Flickr = {
  search: function(query) {
    // the $.getJSON method returns a $.Deferred
    return $.getJSON(
      "https://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?",
      {
        tags: query,
        tagmode: "any",
        format: "json"
      }
    );
  }
};
```

Now let's run the search and process the server response:

```javascript
// container where we'll put the images
// hide it until the images are loaded
var $images = $("#images").hide();

// progress bar showing the image loading process
var $progress = $("#progress").find("div");

// our object
Flickr
    // start searching by keyword
    .search("apple")
    // when the AJAX response comes back, add images and wait for them to load
    .then(function (data) {
        var D = $.Deferred();
        var total = data.items.length;
        var loaded = 0;

        $.each(data.items, function(i, item){
            // create an image
            var img = new Image();
            img.onload = img.onerror = function() {
                // update loading progress
                D.notify(1)
            };
            img.src = item.media.m;
            $(img).prependTo($images);
        });

        D.progress(function() {
            // increment the number of loaded images
            loaded ++;

            // update the progress bar
            $progress.width(100/total*loaded + '%');

            // when all images are loaded
            if (total === loaded) {
                D.resolve();
            }
        });
        return D.promise();
    })
    // when all images are loaded
    .then(function() {
        // hide the progress bar      
        $progress.width(0);
        // show the container with all images
        $images.show("slow");
    })
```

As a result, we get a simple little search service:

{% embed url="https://anton.shevchuk.name/book/code/deferred.flickr.html" %}
