# 10% Including, Finding, Getting Ready

We've got the basics down, now it's time to move on to actually studying jQuery. It all starts with including the library. And even at this stage we can go several routes:

1.  Download jQuery from the project's home page [https://jquery.com/](https://jquery.com/) and place it right next to our HTML page (I recommend downloading the development version — it's always fun to dig through the source code :):

    ```html
    <script type="text/javascript" src="js/jquery.js"></script>
    ```

    > This approach is great for working offline or when you have a really slow internet connection. Pay special attention to the path — scripts are collected in a separate folder "`js`". And this is no accident, you need to train yourself to keep things organized.
2.  Use a [CDN](https://en.wikipedia.org/wiki/Content\_Delivery\_Network). I prefer the service from [Google](https://developers.google.com/speed/libraries/#jquery), but there's also [Microsoft](https://docs.microsoft.com/en-us/aspnet/ajax/cdn/overview), as well as the universal [https://cdnjs.com/](https://cdnjs.com/libraries/jquery) and [https://www.jsdelivr.com/](https://www.jsdelivr.com/package/npm/jquery) — many of them, by the way, host lots of popular plugins, and a special thanks to them for that.

    > There's also a CDN provided by the jQuery developers themselves, but it's far from as advanced as the others, and from my experience it's had stability issues, so be careful when working with it – [https://releases.jquery.com/](https://releases.jquery.com/)

    <pre class="language-html"><code class="lang-html"><strong>&#x3C;script src="https://cdn.jsdelivr.net/npm/jquery@3.7.1/dist/jquery.min.js">&#x3C;/script>
    </strong></code></pre>

    > CDN is a pretty clever thing — when you request the jQuery library this way, you'll get HTTP headers back saying that this file won't "expire" for a whole year.&#x20;
    >
    > If you request the file at `jquery@3.7/dist/jquery.min.js`, you'll get the latest available version from the 3.7.x branch — at the time of writing, that was version 3.7.1, and the headers will indicate that this file will be cached on the browser side for 7 days.
    >
    > Same situation with the request `jquery@3/dist/jquery.min.js` — it will return the latest available version from the 3.x.x branch, cached for 7 days.
3.  Using the [NPM](https://www.npmjs.com/) package manager, install [the library we need](https://www.npmjs.com/package/jquery). This manager lets you install all sorts of libraries and packages — you can't keep track of them all.

    > Why do I mention this package manager? Well, maybe one of you will be curious enough to figure out how to work with it on your own :)
