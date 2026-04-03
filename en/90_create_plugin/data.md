# Data Registry

{% hint style="info" %}
If for some reason you're not yet familiar with `data()`, I recommend re-reading the [Data Attributes](../20\_attributes\_and\_properties/data-attributes.md) chapter and checking out the [official documentation](https://api.jquery.com/data/).
{% endhint %}

In the context of plugin development, here's what we need to remember — the `$.data()` method lets you maintain a data registry tied to a specific element. So it's best to store all plugin data in this registry. If you need to save the plugin state — use `data()`, if you need a cache — use `data()`, if you need to save ... well, I think you get the picture. Here's another little example related to initialization:

```javascript
function() { // init function
    var init = $(this).data('mySimplePlugin');
    if (init) {
        return this;
    } else {
        $(this).data('mySimplePlugin', true);
        return this.on('click.mySimplePlugin', function(){
            $(this).css('color', options.color);
        });
    }
}
```

{% hint style="danger" %}
Just a reminder that the `jQuery.data()` method doesn't manipulate HTML attributes — it works with its own registry, and only when its registry has no data does it try to grab the `data-*` attribute.
{% endhint %}

{% embed url="https://anton.shevchuk.name/book/code/data.html" %}

{% hint style="info" %}
I should remember to tell you about `obj[jQuery.expando] = uuid` and `jQuery._data(element)`, and also about [memory leaks](https://javascript.info/memory-leaks-jquery), but probably not today.
{% endhint %}
