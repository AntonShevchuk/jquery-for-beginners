# Смуга прокрутки

Ну і остання пара методів про які я хотів би розповісти:

<table data-header-hidden><thead><tr><th width="271">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollLeft()
</code></pre></td><td>повертає значення «проскроленості» по горизонталі для першого елемента з вибірки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollLeft(value)
</code></pre></td><td>встановлює значення горизонтального скролу для кожного елемента з вибірки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollTop()
</code></pre></td><td>повертає значення «проскроленості» по вертикалі для першого елемента з вибірки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollTop(value)
</code></pre></td><td>встановлює значення вертикального скролу для кожного елемента з вибірки</td></tr></tbody></table>

Ось таким чином ми можемо дізнатися «відстань» пройдену від початку сторінки:

```javascript
$('html').scrollTop();
```

Або можемо «стрибнути» на самий початок сторінки:

```javascript
$('html').scrollTop(0);
```

Значення `scrollTop` та `scrollLeft` піддаються анімації і не працюють для прихованих елементів DOM:

```javascript
$('html').animate({ scrollTop: '+=200px' });
```

{% embed url="https://anton.shevchuk.name/book/code/dom.scroll.html" %}

***

Методів реально багато, я й сам не завжди пам'ятаю що і для чого (особливо це стосується wrap-сімейства), тож не обтяжуйте себе запам'ятовуванням усього перерахованого, головне пам'ятати, що такі є, і тримати під рукою [документацію](https://api.jquery.com/category/manipulation/).
