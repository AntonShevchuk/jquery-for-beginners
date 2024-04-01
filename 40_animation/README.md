# 40% Анимация

Библиотека jQuery позволяет очень легко анимировать DOM-элементы, для этого предусмотрено несколько функций. Но обо всём по порядку. Начнём с простых `hide()` и `show()`, эти два метода, соответственно, скрывают либо отображают элементы:

<table data-header-hidden><thead><tr><th width="282">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">hide()
</code></pre></td><td>скроем элементы</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">show()
</code></pre></td><td>отобразим элементы</td></tr></tbody></table>

Данные вызовы оперируют лишь CSS-атрибутом `display` и переключают его из текущего состояния в `none` и обратно. В качестве первого параметра можно задать скорость анимации, для этого можно использовать одно из зарезервированных слов `slow` или `fast`, либо же указывать скорость в миллисекундах (1000 мс = 1 сек):

```javascript
// медленно спускаемся с горы и… скрываем все картинки
//   slow == 600
//   fast == 200
$("img").hide('slow')
```

```javascript
// теперь вернём их на место, чуть быстрее
$("img").show(400)
```

В таком случае исчезновение элементов будет сопровождаться изменением атрибутов `width`, `height`, `opacity` и прочих. В довесок к этим двум методам есть ещё метод `toggle()`, который работает как переключатель `hide → show` или `show → hide`:

```javascript
// запустите пару раз
$("img").toggle()
```

Дальше – больше, вторым параметром в приведенных методах может быть callback-функция – она будет выполнена по окончании анимации:

```javascript
// скрываем все картинки
$("img").hide("slow", function() {
    // опосля отображаем alert
    alert("Images was hidden")
});
```

{% embed url="https://anton.shevchuk.name/book/code/hide.html" %}

***

{% hint style="info" %}
Анимацию таких атрибутов как `height`, `width` и `opacity` видно невооруженным взглядом, в действительности же это далеко не всё. Заглянув внутрь jQuery, можно увидеть, что также изменяются внутренние и внешние отступы – `padding` и `margin` – так что не стоит об этом забывать.
{% endhint %}

Идём дальше – у нас на очереди набор методов из семейства slide: `slideUp()`, `slideDown()` и `slideToggle()`. Их поведение схоже с предыдущими функциями, но анимация будет затрагивать лишь высоту блоков:

<table data-header-hidden><thead><tr><th width="284">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">slideUp()
</code></pre></td><td>скроем элементы изменяя высоту до <code>0</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">slideDown()
</code></pre></td><td>отобразим все элементы, постепенно увеличивая высоту элементов с нуля</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">slideToggle()
</code></pre></td><td>работает как <code>slideDown()</code> для скрытых элементов, иначе  как <code>slideUp()</code></td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/slide.html" fullWidth="false" %}

***

Прежде, чем перейти к десерту, упомяну семейство функций `fade` — они манипулируют лишь атрибутом `opacity`:

<table data-header-hidden><thead><tr><th width="281">метод</th><th>описание</th><th data-hidden data-type="rating" data-max="5"></th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeOut()
</code></pre></td><td>изменяет <code>opacity</code> от текущего до <code>0</code></td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeIn()
</code></pre></td><td>изменяет <code>opacity</code> от <code>0</code> до предыдущего значения</td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeToggle()
</code></pre></td><td>переключатель между <code>In</code> и <code>Out</code></td><td>null</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">fadeTo(0.5)
</code></pre></td><td>изменяет значение <code>opacity</code> до требуемого значения</td><td>null</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/fade.html" %}

***

А теперь самое сладкое – все эффекты анимации в jQuery крутятся вокруг метода `animate()`. Данная функция берёт один или несколько CSS-свойств элемента и изменяет их от исходного до заданного за N-ое количество итераций (количество итераций зависит от указанного времени, но не реже одной итерации в 13 мс, если я правильно накопал это значение). Ну что же, от слов к делу, попробуем реализовать функции `fadeIn()` и `fadeout()` с помощью `animate()`:

```javascript
// fadeOut()
$("img").animate({
    "opacity": "hide"
})
```

```javascript
// fadeIn()
$("img").animate({
    "opacity": "show"
})
```

Всё просто ведь. Давайте-ка теперь усложним задачу – изменим размер блоков и прозрачность:

```javascript
// значения указанных свойств будут плавно изменяться
// от текущих до заданных
$("img").animate({
    "opacity": 0.5,
    "height": "100px",
    "width": "100px"
})
```

Как видите, тоже несложно. Теперь попробуем отталкиваться от текущих значений, а не задавать необходимые:

```javascript
// изменяем, шаг за шагом
$("img").animate({
    "opacity": "+=0.1",
    "height": "+=12px",
    "width": "+=20px"
})
```

{% embed url="https://anton.shevchuk.name/book/code/animate.html" %}
