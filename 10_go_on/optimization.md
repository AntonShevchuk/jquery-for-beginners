# Оптимизация

{% hint style="danger" %}
Ну, перво-наперво, вам следует запомнить, что&#x20;

**результаты поиска не кэшируются — каждый раз, запрашивая элементы по селектору, вы инициируете поиск элементов снова и снова**
{% endhint %}

При ознакомлении с алгоритмом работы Sizzle, сходу напрашиваются несколько советов об оптимизации по работе с выборками:

* Сохранять результаты поиска (исходя из постулата выше):

```javascript
// было
$("a.button").addClass("active");
/* ... */
$("a.button").click(function(){ /* ... */ });

// стало
var $button = $("a.button");
$button.addClass("active");
/* ... .*/
$button.click(function(){ /* ... */ });
```

> Правильная IDE о подобных вещах знает, и будет вам время от времени подсказывать ;)

* Использовать цепочки вызовов (что по сути аналогично первому правилу):

```javascript
// было
$("a.button").addClass("active");
$("a.button").click(function(){ /* ... */ });

// стало
$("a.button")
  .addClass("active")
  .click(function(){ /* ... */ });
```

* Использовать `context` (это такой второй параметр при выборе по селектору):

```javascript
// было
$(".content a.button");

// стало
$("a.button", ".content");

// ещё вариант, не быстрее, но читаемость выше
$(".content").find("a.button");
```

* Разбивать запрос на более простые составные части, используя `context`, и сохранять промежуточные данные:

```javascript
// было
$(".content a.button");
$(".content h3.title");

// стало
let $content = $(".content")
$content.find("a.button");
$content.find("h3.title");
```

* Использовать более «съедобные» селекторы, дабы помочь методу `.querySelectorAll()`, т.е. если у вас нет уверенности в правильности написания селектора, или вы сомневаетесь в том, что все браузеры поддерживают необходимый CSS-селектор, то лучше разделить «сложный» селектор на несколько более простых:

```javascript
// было
$(".content div input:disabled");

// стало
$(".content div").find("input:disabled");
```

* Не использовать jQuery, а работать с «native» функциями JavaScript

> Есть ещё один пункт – выбирать самый быстрый селектор из возможных, но тут без хорошего багажа знаний не обойтись, так что дерзайте, пробуйте и присылайте ваши примеры.

Для наглядности лучше всего взглянуть на сравнительный тест [benchmark.html](https://anton.shevchuk.name/book/code/benchmark.html):

<figure><img src="../.gitbook/assets/benchmark.png" alt=""><figcaption></figcaption></figure>

> Данный тест выполняет поиск элементов несколькими способами и был изначально разработан Ильёй Кантором для мастер-класса по JavaScript и jQuery

{% hint style="info" %}
Маленькая хитрость от создателей jQuery – запросы по id элемента не доходят до Sizzle, а скармливаются `document.getElementById()` в качестве параметра:

{% code fullWidth="false" %}
```javascript
$("#content");
// -> 
document.getElementById("content");
```
{% endcode %}
{% endhint %}

## Примеры оптимизаций

Выбор по идентификатору элемента — самый быстрый из возможных, старайтесь использовать оный если есть такая возможность:

```javascript
// внутри одна регулярочка + getElementById()
$("#content");

// вот так ещё быстрее
// экономия незначительна, а удобство использования стремится к нулю
$(document.getElementById("content"));
```

Селектор `div#content` работает на порядок медленнее, нежели поиск лишь по идентификатору `#content`, но и он имеет право на существование в случае, если ваш скрипт используется на нескольких страницах, а логика требует лишь обрабатывать поведение для элемента `<div>`. Данный селектор можно представить в двух вариантах:

```javascript
// getElementById() + фильтрация
$("#content").filter("div");

// оставляем как есть и надеемся на QuerySelectorAll()
$("div#content");
```

Но и тут есть разница в производительности, в результате [тестирования](https://jsperf.app/biwafe) получаем, что пример с использованием `.filter()` работает быстрее на 20-30%:

{% tabs %}
{% tab title="Chrome" %}
<figure><img src="../.gitbook/assets/chrome-benchmark.png" alt=""><figcaption><p>Testing in Chrome</p></figcaption></figure>
{% endtab %}

{% tab title="Safari" %}
<figure><img src="../.gitbook/assets/safari-benchmark.png" alt=""><figcaption><p>Testing in Safari</p></figcaption></figure>
{% endtab %}
{% endtabs %}

Но всё же, подобная тонкая оптимизация нужна далеко не всегда. \
Выводы делаем сами.
