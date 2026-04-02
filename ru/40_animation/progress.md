# Прогресс

Давайте вернёмся к методу `animate()`, который в качестве аргументов может принимать расширенный набор опций — `animate(properties, options)`, первым идут CSS-свойства, с которыми мы уже работали ранее, а вот вторым аргументом — опции, которые нам дают ряд новых возможностей (я перечислю все доступные опции, часть из них нам уже встречались):

<table data-header-hidden><thead><tr><th width="216">опция</th><th>назначение</th></tr></thead><tbody><tr><td><code>duration</code></td><td>скорость анимации</td></tr><tr><td><code>easing</code></td><td>функция «<code>linear</code>» или «<code>swing</code>», или любая другая</td></tr><tr><td><code>queue</code></td><td>флаг/параметр очереди; о ней расскажу чуть ниже</td></tr><tr><td><code>specialEasing</code></td><td><p>хэш, в котором можно описать, какую именно easing-функцию следует использовать для изменения определённых параметров:</p><pre class="language-javascript"><code class="lang-javascript">$("img").animate({
  "height": "+=100px",
  "width": "+=100px"
}, {
  "specialEasing": {
    "height": "linear",
    "width": "swing"
  }
})
</code></pre></td></tr><tr><td><code>step</code></td><td>функция, которая будет вызвана на каждом шаге анимации для каждого CSS-свойства; чуть позже опишу подробнее</td></tr><tr><td><code>complete</code></td><td>функция, которая будет вызвана после окончания анимации</td></tr><tr><td><code>start</code></td><td>функция, которая будет вызвана до начала анимации</td></tr><tr><td><code>progress</code></td><td>функция, которая будет вызвана на каждом шаге, но только единожды для элемента, вне зависимости от количества CSS-свойств</td></tr><tr><td><code>done</code></td><td>функция, которая будет вызвана после успешного окончания анимации</td></tr><tr><td><code>fail</code></td><td>функция, которая будет вызвана после неудачного окончания анимации</td></tr><tr><td><code>always</code></td><td>функция, которая будет вызвана после окончания анимации при любом исходе</td></tr></tbody></table>

{% hint style="info" %}
Поддержка последних пяти функций добавлена в версии 1.8. Данный [релиз jQuery ознаменовался](https://blog.jquery.com/2012/08/09/jquery-1-8-released/) глобальным рефакторингом анимации и переездом на Deferred.

Зачем я постоянно обращаю ваше внимание на изменения в таких старых версиях jQuery? Просто никто не знает с чем вам придется столкнуться, поэтому лучше готовиться к худшему.
{% endhint %}

В каких случаях нам это будет полезно? Ну если вам нужно отслеживать прогресс анимации, то это именно то, что вам нужно, точнее, вам нужна будет функция `progress(animation, progress, remainingMs)`:

```javascript
// изменяем высоту и ширину картинок
$("img").animate({
  "height": "+=100px",
  "width": "+=100px"
}, {
  "start":    () => console.log("start"),
  "progress": (animation, progress, time) => console.log("progress", progress),
  "done":     () => console.log("done"),
  "fail":     () => console.log("fail"),
  "always":   () => console.log("always")
})
```



{% embed url="https://anton.shevchuk.name/book/code/animate.options.html" %}
