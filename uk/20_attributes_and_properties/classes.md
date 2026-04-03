# CSS класи

Ну, наче з CSS розібралися, хоча ні — варто ще описати маніпуляції з класами, теж із розряду первинних навичок:

<table data-header-hidden><thead><tr><th width="413">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">addClass(className)
</code></pre></td><td>додавання класу елементу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">addClass(function(index, currentClass){
  return className
})
</code></pre></td><td>додавання класу за допомогою функції зворотного виклику</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">hasClass(className)
</code></pre></td><td>перевірка на причетність до певного класу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeClass(className)
</code></pre></td><td>видалення класу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeClass(function(index, currentClass){
  return className
})
</code></pre></td><td>видалення класу за допомогою функції зворотного виклику</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">toggleClass(className)
</code></pre></td><td>перемикання класу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">toggleClass(className, state)
</code></pre></td><td>перемикання класу за прапорцем <code>state</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>toggleClass(function(index, currentClass, state) {
</strong>  return className
}, state)
</code></pre></td><td>перемикання класу за допомогою функції зворотного виклику, прапорець <code>state</code> опціональний</td></tr></tbody></table>

{% hint style="info" %}
У наведених методах як `className` може бути рядок, що містить список класів через пробіл.
{% endhint %}

> Мені жодного разу не доводилося використовувати ці методи з функціями зворотного виклику, і лише одного разу знадобився прапорець «`state`», тож не заморочуйтеся все це запам'ятовувати, та й надалі, цитуючи керівництво по jQuery, я буду свідомо опускати деякі «можливості».

Але досить займатися перекладом офіційної документації, перейдемо до наочних прикладів.

В першу чергу — додавання класів:

```javascript
// додаємо клас «active»
$("#my").addClass("active")

// додаємо кілька класів за раз
$("#my").addClass("active notice")
```

Перемикання класів також затребуваний метод:

```javascript
// перемикаємо клас «active»:
//  - видаляємо клас, якщо він є
//  - додаємо, якщо його немає
$("#my").toggleClass("active")

// перемикаємо кілька класів
$("#my").toggleClass("active notice")
```

Працює перемикання кількох класів наступним чином:

```markup
<div id="my" class="active notice"> → <div id="my" class="">

<div id="my" class="active"> → <div id="my" class="notice">

<div id="my" class=""> → <div id="my" class="active notice">
```

Ну і приклади з видаленням, для повного комплекту, так би мовити:

```javascript
// видаляємо клас «active»
$("#my").removeClass("active") 

// видаляємо кілька класів за раз
$("#my").removeClass("active notice")
```

Наочний інтерактивний приклад:

{% embed url="https://anton.shevchuk.name/book/code/class.html" %}
