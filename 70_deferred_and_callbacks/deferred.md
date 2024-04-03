---
description: Объект $.Deferred
---

# Deferred

Начнем с объекта Deferred, разобраться с ним и уметь применять – это прям высший пилотаж. Давайте посмотрим, как он работает, откройте нашу песочницу:

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

Запустите скрипт в консоли:

```javascript
// инициализация Deferred объекта
// статус «ожидает выполнение»
var D = $.Deferred();

// подключаем обработчики
D.then(function() { console.log("first") });
D.then(function() { console.log("second") });

// изменяем статус на «fulfilled» - «выполнен успешно»
// для этого вызываем resolve()
// для наглядности подождём секундочку
// наши обработчики будут вызваны в порядке очереди, но они не ждут друг друга
setTimeout(() => D.resolve(), 1000)

// данный обработчик подключён слишком поздно и будет вызван сразу
D.then(function() { console.log("third") });
```

{% hint style="info" %}
Приведенные примеры будут работать на любой странице, где подключен jQuery ;)
{% endhint %}

Если всё это перевести на человеческий язык, получится следующий сценарий:

* если всё будет хорошо, тогда выполни вот эту функцию и выведи «`first`»
* и ещё вот эту функцию — «`second`»
* `resolve()` — мы узнали, всё хорошо
* ~~если~~ всё хорошо, выполняем функцию и выводим «`third`»

Кроме сценариев с «happy end», есть ещё и грустные истории, когда всё пошло не так, как нам бы хотелось:

```javascript
// инициализация Deferred объекта
var D = $.Deferred();

// подключаем обработчики
D.then(function() { console.info("done") });
D.catch(function() { console.error("fail") });

// изменяем статус на «rejected» - «выполнен с ошибкой»
// для наглядности подождём секундочку
setTimeout(() => D.reject(), 1000)

// в консоли нас будет ожидать лишь «fail» :(
```

Получается так:

* если всё будет хорошо, тогда выполни вот эту функцию — «`done`»
* если всё будет плохо, тогда вот эта функция выведет «`fail`»
* ой, всё плохо

В действительности, метод `then()` позволяет вешать одновременно как обработчики для положительного сценария, так и для варианта с ошибкой:

```javascript
D.then(
    function(result) { console.info("done") },
    function(error) { console.error("fail") }
);
```

Становится ли от этого код читаемым – сомневаюсь, но такой вариант существует и используется повсеместно. Я же предпочитаю для отлова ошибок использовать метод `catch()`, который по своей сути лишь сокращенная запись для `then(null, fn)`:

```javascript
var D = $.Deferred();

// подключаем обработчик ошибок через then()
D.then(null, function() { console.error("fail") });

// подключаем обработчик ошибок через catch()
D.catch(function() { console.error("again fail") });
```

Ещё упомяну метод `always()` – он добавляет обработчики, которые будут выполнены вне зависимости от случившегося (в действительности, внутри происходит вызов `done(arguments)` и `fail(arguments)`).

Чтобы не путаться в перечисленных методах, приведу блок-схему:

<figure><img src="../.gitbook/assets/deferred.svg" alt=""><figcaption></figcaption></figure>

При вызове [`resolve()`](https://api.jquery.com/deferred.resolve/) и [`reject()`](https://api.jquery.com/deferred.reject/) можно передать произвольные данные в зарегистрированные callback-функции для дальнейшей работы:

```javascript
// инициализация Deferred объекта
var D = $.Deferred();

// подключаем обработчики
D.then(function(message) { console.log(`first: ${message}`) });
D.then(function(message) { console.log(`second: ${message}`) });

// вызываем resolve()
D.resolve("A-a-a")
```

Кроме того, существуют ещё методы [`resolveWith()`](https://api.jquery.com/deferred.resolveWith/) и [`rejectWith()`](https://api.jquery.com/deferred.rejectWith/), они позволяют изменять контекст вызываемых callback-функций (т.е. внутри них «`this`» будет смотреть на указанный контекст):

```javascript
/// инициализация Deferred объекта
var D = $.Deferred();

// счётчик электрический
function Counter () {
  this.counter = 0
  this.tick = function() {
    this.counter++
    console.log(`tick: ${this.counter}`)
  }
}

var counter = new Counter()

// подключаем обработчики
// this будет смотреть на наш counter
D.then(function() { this.tick() });
D.then(function() { this.tick() });

// вызываем resolveWith()
D.resolveWith(counter)
```

Отдельно отмечу, что если вы собираетесь передать Deferred объект «на сторону», чтобы «там» могли повесить свои обработчики событий, но не хотите потерять контроль, то возвращайте не сам объект, а результат выполнения метода `promise()` – фактически это будет искомый объект в режиме «read-only»:

<table><thead><tr><th width="366">доступные методы</th><th>будут недоступны</th></tr></thead><tbody><tr><td><code>then</code>, <code>done</code>, <code>fail</code>, <code>always</code>, <code>pipe</code>, <code>progress</code>, <code>state</code> и <code>promise</code></td><td><code>resolve</code>, <code>reject</code>, <code>notify</code>, <code>resolveWith</code>, <code>rejectWith</code>, и <code>notifyWith</code></td></tr></tbody></table>

А ещё, кроме поведения «ждём чуда» с помощью Deferred можно выстраивать цепочки вызовов – «живые очереди»:

```javascript
var D = $.Deferred();

D.then(function() {
  // ждём resolve()
  console.log("hide images")
  return $("article img").slideUp(2000).promise()
})
.then(function(){
  // подождём, пока спрячутся картинки
  console.log("hide paragraphs")
  return $("article p").slideUp(2000).promise()
})
.then(function(){
  // подождём, пока спрячутся параграфы
  console.log("hide articles")
  return $("article").hide(2000).promise()
})
.then(function(){
  // всё сделано, шеф
  console.info("done")
});

// секунду и всё будет
setTimeout(() => D.resolve(), 1000)
```

Подобное поведение мы уже реализовывали используя метод `animate()`, но нам же хочется найти свой путь:

{% hint style="info" %}
До jQuery 1.8 тут шла речь о методе `pipe()`, а теперь о `then()`
{% endhint %}

В данном примере мы вызываем метод `then()`, которому скормлена callback-функция, которая должна возвращать объект Promise. Это необходимо для соблюдения порядка в очереди – попробуйте убрать в примере один «`return`», и вы заметите, что следующая анимация наступит не дождавшись завершения предыдущей:

```javascript
var D = $.Deferred();

D.then(function() {
  // ждём resolve()
  console.log("hide images")
  $('article img').slideUp(2000).promise()
})
.then(function(){
  // подождём, пока спрячутся картинки
  console.log("hide paragraphs")
  $('article p').slideUp(2000).promise()
})
.then(function(){
  // подождём, пока спрячутся параграфы
  console.log("hide articles")
  return $('article').hide(2000).promise()
})
.then(function(){
  // всё сделано, шеф
  console.info("done")
});

// секунду и всё будет
setTimeout(() => D.resolve(), 1000)
```

{% embed url="https://anton.shevchuk.name/book/code/deferred.html" %}

На этом возможности Deferred ещё не завершились. Есть ещё связка методов `notify()` и `progress()` – первый шлёт послания в callback-функции, которые зарегистрированы с помощью второго. Приведу наглядный код для демонстрации (откройте консоль и посмотрите, что получается):

```javascript
var D = $.Deferred();

var money = 100; // наш бюджет

// съём денежки
D.progress(function($){
    console.log(`${money} - ${$} = ${money-$}`);
    if (money < $) { // деньги закончились
        D.reject();
    }
    money -= $;
});

// тратим деньги
setTimeout(function(){ D.notify(40); }, 500);  // покупка 1
setTimeout(function(){ D.notify(50); }, 1000); // покупка 2
setTimeout(function(){ D.notify(30); }, 1500); // покупка 3
setTimeout(function(){ D.resolve();  }, 3000); // всё купили

D.then(function(){ console.info("All Ok") });
D.catch(function(){ console.error("Insufficient Funds") });
```

***

> Испытайте всю мощь Deferred!

Создадим сервис для поиска картинок на сервисе Flickr.  Начнем с объекта, который будет отвечать лишь за загрузку данных с Flickr:

```javascript
var Flickr = {
  search: function(query) {
    // метод $.getJSON возвращает $.Deferred
    return $.getJSON(
      "https://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?",
      {
        tags: query,
        tagmode: "any",
        format: "json"
      }
    );
  }
};
```

Теперь запустим поиск и обработаем ответ сервера:

```javascript
// контейнер, куда будем складывать картинки
// скрываем его, пока не загружены картинки
var $images = $("#images").hide();

// прогресс-бар отображающий процесс загрузки картинок
var $progress = $("#progress").find("div");

// наш объект
Flickr
    // запускаем поиск по ключевому слову
    .search("apple")
    // когда вернулся AJAX-ответ, добавляем картинки и ждём их загрузку
    .then(function (data) {
        var D = $.Deferred();
        var total = data.items.length;
        var loaded = 0;

        $.each(data.items, function(i, item){
            // создаём картинку
            var img = new Image();
            img.onload = img.onerror = function() {
                // изменяем прогресс загрузки
                D.notify(1)
            };
            img.src = item.media.m;
            $(img).prependTo($images);
        });

        D.progress(function() {
            // инкрементим кол-во загруженных картинок
            loaded ++;

            // обновляем прогресс-бар
            $progress.width(100/total*loaded + '%');

            // когда все картинки загрузились
            if (total === loaded) {
                D.resolve();
            }
        });
        return D.promise();
    })
    // когда все картинки загрузились
    .then(function() {
        // прячем прогресс-бар      
        $progress.width(0);
        // отображаем контейнер со всеми картинками
        $images.show("slow");
    })
```

В результате у нас получился простенький поисковый сервис:

{% embed url="https://anton.shevchuk.name/book/code/deferred.flickr.html" %}
