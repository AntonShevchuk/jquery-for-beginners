---
description: The $.when() method
---

# When

Now, using our [test page](https://anton.shevchuk.name/book/code/when.html) as an example, I'll show you the nifty `$.when()` method:

```javascript
$.when(
  $("article h3").hide(3000),
  $("article img").slideUp(1000),
  $("article p").fadeTo(2000, 0.1)
).then(() =>
  $("article").slideUp(500)
).done(() =>
  console.info("done")
).catch(() =>
  console.error("fail :(")
)
```

Let me explain what's happening — all animations start simultaneously, and when they all finish, the function we pass as an argument to the `then()` method gets called (one of two, depending on the outcome).&#x20;

To make this "magic" work, `$.when()`, `$.ajax()`, and `animate()` all implement the Deferred interface. That's why we can use all the methods we explored in the previous chapter.

{% hint style="info" %}
And now for the fancy version — the `when()` method returns a projection of a Deferred object, accepts an arbitrary number of Deferred objects as parameters, and when all of them have completed, the `when` object changes its state to "resolved", triggering all subscribers.
{% endhint %}

{% embed url="https://anton.shevchuk.name/book/code/when.html" %}
