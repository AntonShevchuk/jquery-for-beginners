# Полоса прокрутки

Ну и последняя пара методов о которых я бы хотел рассказать:

<table data-header-hidden><thead><tr><th width="271">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollLeft()
</code></pre></td><td>возвращает значение «проскролленности» по горизонтали для первого элемента из выборки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollLeft(value)
</code></pre></td><td>устанавливает значение горизонтального скролла для каждого элемента из выборки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollTop()
</code></pre></td><td>возвращает значение «проскролленности» по вертикали для первого элемента из выборки</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">scrollTop(value)
</code></pre></td><td>устанавливает значение вертикального скролла для каждого элемента из выборки</td></tr></tbody></table>

Вот таким образом мы можем узнать «расстояние» пройденное от начала страницы:

```javascript
$('html').scrollTop();
```

Или можем «прыгнуть» в самое начало страницы:

```javascript
$('html').scrollTop(0);
```

Значения `scrollTop` и `scrollLeft` поддаются анимации и не работают для спрятанных элементов DOM:

```javascript
$('html').animate({ scrollTop: '+=200px' });
```

{% embed url="https://anton.shevchuk.name/book/code/dom.scroll.html" %}

***

Методов реально много, я и сам не всегда помню что и для чего (особенно это касается wrap-семейства), так что не утруждайте себя запоминанием всего перечисленного, главное помнить, что таковые имеются, и держать под рукой [документацию](https://api.jquery.com/category/manipulation/).
