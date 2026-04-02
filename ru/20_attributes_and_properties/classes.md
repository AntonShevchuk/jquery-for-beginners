# CSS классы

Ну, вроде с CSS разобрались, хотя нет — стоит ещё описать манипуляции с классами, тоже из разряда первичных навыков:

<table data-header-hidden><thead><tr><th width="413">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">addClass(className)
</code></pre></td><td>добавление класса элементу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">addClass(function(index, currentClass){
  return className
})
</code></pre></td><td>добавление класса с помощью функции обратного вызова</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">hasClass(className)
</code></pre></td><td>проверка на причастность к определённому классу</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeClass(className)
</code></pre></td><td>удаление класса</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">removeClass(function(index, currentClass){
  return className
})
</code></pre></td><td>удаление класса с помощью функции обратного вызова</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">toggleClass(className)
</code></pre></td><td>переключение класса</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">toggleClass(className, state)
</code></pre></td><td>переключение класса по флагу <code>state</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript"><strong>toggleClass(function(index, currentClass, state) {
</strong>  return className
}, state)
</code></pre></td><td>переключение класса с помощью функции обратного вызова, флаг <code>state</code> опционален</td></tr></tbody></table>

{% hint style="info" %}
В приведённых методах в качестве `className` может быть строка, содержащая список классов через пробел.
{% endhint %}

> Мне ни разу не приходилось использовать данные методы с функциями обратного вызова, и лишь единожды пригодился флаг «`state`», так что не заморачивайтесь всё это запоминать, да и в дальнейшем, цитируя руководство по jQuery, я буду сознательно опускать некоторые «возможности».

Но хватит заниматься переводом официальной документации, перейдем к наглядным примерам.

В первую очередь — добавление классов:

```javascript
// добавляем класс «active»
$("#my").addClass("active")

// добавляем несколько классов за раз
$("#my").addClass("active notice")
```

Переключение классов так же востребованный метод:

```javascript
// переключаем класс «active»:
//  - удаляем класс, если он есть
//  - добавляем, если его нет
$("#my").toggleClass("active")

// переключаем несколько классов
$("#my").toggleClass("active notice")
```

Работает переключение нескольких классов следующим образом:

```markup
<div id="my" class="active notice"> → <div id="my" class="">

<div id="my" class="active"> → <div id="my" class="notice">

<div id="my" class=""> → <div id="my" class="active notice">
```

Ну и примеры с удалением, для полного комплекта, так сказать:

```javascript
// удаляем класс «active»
$("#my").removeClass("active") 

// удаляем несколько классов за раз
$("#my").removeClass("active notice")
```

Наглядный интерактивный пример:

{% embed url="https://anton.shevchuk.name/book/code/class.html" %}
