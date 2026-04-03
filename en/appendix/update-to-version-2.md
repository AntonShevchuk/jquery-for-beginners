# Upgrading to Version 2.x

What does the new version mean for us? Well, we have nothing to fear, but users of old Internet Explorer versions won't be able to enjoy the convenience of this popular library anymore. Yep, version 2 dropped support for IE 6 through 8. Thanks to this "optimization," the library shed 10% of its weight.

> If newer versions of IE run in compatibility mode with old versions, jQuery won't bother and will refuse to work. You can avoid this with the X-UA-Compatible meta tag or HTTP header. Soon other aging browsers will be on the chopping block too — watch out, Android/WebKit 2.x, they're coming for you.

If this diet doesn't excite you, you can go further and build jQuery with only what you actually need. What you'll need for that — read the detailed [step-by-step guide](https://github.com/jquery/jquery/), which doesn't even need translating.

If this list of changes doesn't impress you, that's only thanks to the hard work of the jQuery developers, who ensured full API compatibility between the 1.x and 2.x branches — and now that's truly impressive.

> Alright, not everything was so smooth with compatibility — the path was thorny, and there will be upgrade issues. But you'll need to look for the cause in the version 1.9 changelog — that version was the turning point in branch compatibility.
>
> * [How to build your own jQuery](https://github.com/jquery/jquery/)
> * [jQuery Core 1.9 Upgrade Guide](https://jquery.com/upgrade-guide/1.9/)
