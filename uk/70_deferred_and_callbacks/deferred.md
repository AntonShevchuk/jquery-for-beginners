---
description: Об'єкт $.Deferred
---

# Deferred

Почнемо з об'єкта Deferred, розібратися з ним і вміти застосовувати – це прям вищий пілотаж. Подивімося, як він працює, відкрийте нашу пісочницю:

{% embed url="https://anton.shevchuk.name/book/code/sandbox.html" %}

Запустіть скрипт у консолі:

```javascript
// ініціалізація Deferred об'єкта
// статус «очікує виконання»
var D = $.Deferred();

// підключаємо обробники
D.then(function() { console.log("first") });
D.then(function() { console.log("second") });

// змінюємо статус на «fulfilled» - «виконано успішно»
// для цього викликаємо resolve()
// для наочності зачекаємо секундочку
// наші обробники будуть викликані в порядку черги, але вони не чекають один на одного
setTimeout(() => D.resolve(), 1000)

// цей обробник підключено занадто пізно і буде викликано одразу
D.then(function() { console.log("third") });
```

{% hint style="info" %}
Наведені приклади працюватимуть на будь-якій сторінці, де підключено jQuery ;)
{% endhint %}

Якщо все це перекласти людською мовою, вийде такий сценарій:

* якщо все буде добре, тоді виконай ось цю функцію і виведи «`first`»
* і ще ось цю функцію — «`second`»
* `resolve()` — ми дізналися, усе добре
* ~~якщо~~ все добре, виконуємо функцію і виводимо «`third`»

Крім сценаріїв з «happy end», є ще й сумні історії, коли все пішло не так, як нам би хотілося:

```javascript
// ініціалізація Deferred об'єкта
var D = $.Deferred();

// підключаємо обробники
D.then(function() { console.info("done") });
D.catch(function() { console.error("fail") });

// змінюємо статус на «rejected» - «виконано з помилкою»
// для наочності зачекаємо секундочку
setTimeout(() => D.reject(), 1000)

// у консолі на нас чекатиме лише «fail» :(
```

Виходить так:

* якщо все буде добре, тоді виконай ось цю функцію — «`done`»
* якщо все буде погано, тоді ось ця функція виведе «`fail`»
* ой, усе погано

Насправді метод `then()` дозволяє навішувати одночасно як обробники для позитивного сценарію, так і для варіанту з помилкою:

```javascript
D.then(
    function(result) { console.info("done") },
    function(error) { console.error("fail") }
);
```

Чи стає від цього код читабельним – сумніваюся, але такий варіант існує і використовується повсюдно. Я ж надаю перевагу для виловлювання помилок методу `catch()`, який по суті лише скорочений запис для `then(null, fn)`:

```javascript
var D = $.Deferred();

// підключаємо обробник помилок через then()
D.then(null, function() { console.error("fail") });

// підключаємо обробник помилок через catch()
D.catch(function() { console.error("again fail") });
```

Ще згадаю метод `always()` – він додає обробників, які будуть виконані незалежно від того, що сталося (насправді всередині відбувається виклик `done(arguments)` і `fail(arguments)`).

Щоб не плутатися в перелічених методах, наведу блок-схему:

<figure><img src="../../.gitbook/assets/deferred.svg" alt=""><figcaption></figcaption></figure>

При виклику [`resolve()`](https://api.jquery.com/deferred.resolve/) та [`reject()`](https://api.jquery.com/deferred.reject/) можна передати довільні дані в зареєстровані callback-функції для подальшої роботи:

```javascript
// ініціалізація Deferred об'єкта
var D = $.Deferred();

// підключаємо обробники
D.then(function(message) { console.log(`first: ${message}`) });
D.then(function(message) { console.log(`second: ${message}`) });

// викликаємо resolve()
D.resolve("A-a-a")
```

Крім того, існують ще методи [`resolveWith()`](https://api.jquery.com/deferred.resolveWith/) та [`rejectWith()`](https://api.jquery.com/deferred.rejectWith/), вони дозволяють змінювати контекст callback-функцій, що викликаються (тобто всередині них «`this`» вказуватиме на зазначений контекст):

```javascript
/// ініціалізація Deferred об'єкта
var D = $.Deferred();

// лічильник електричний
function Counter () {
  this.counter = 0
  this.tick = function() {
    this.counter++
    console.log(`tick: ${this.counter}`)
  }
}

var counter = new Counter()

// підключаємо обробники
// this вказуватиме на наш counter
D.then(function() { this.tick() });
D.then(function() { this.tick() });

// викликаємо resolveWith()
D.resolveWith(counter)
```

Окремо зазначу, що якщо ви збираєтеся передати Deferred об'єкт «на сторону», щоб «там» могли навісити свої обробники подій, але не хочете втратити контроль, то повертай не сам об'єкт, а результат виконання методу `promise()` – фактично це буде шуканий об'єкт у режимі «read-only»:

<table><thead><tr><th width="366">доступні методи</th><th>будуть недоступні</th></tr></thead><tbody><tr><td><code>then</code>, <code>done</code>, <code>fail</code>, <code>always</code>, <code>pipe</code>, <code>progress</code>, <code>state</code> і <code>promise</code></td><td><code>resolve</code>, <code>reject</code>, <code>notify</code>, <code>resolveWith</code>, <code>rejectWith</code>, і <code>notifyWith</code></td></tr></tbody></table>

А ще, крім поведінки «чекаємо дива», за допомогою Deferred можна вибудовувати ланцюжки викликів – «живі черги»:

```javascript
var D = $.Deferred();

D.then(function() {
  // чекаємо resolve()
  console.log("hide images")
  return $("article img").slideUp(2000).promise()
})
.then(function(){
  // зачекаємо, поки сховаються картинки
  console.log("hide paragraphs")
  return $("article p").slideUp(2000).promise()
})
.then(function(){
  // зачекаємо, поки сховаються параграфи
  console.log("hide articles")
  return $("article").hide(2000).promise()
})
.then(function(){
  // все зроблено, шефе
  console.info("done")
});

// секунду і все буде
setTimeout(() => D.resolve(), 1000)
```

Подібну поведінку ми вже реалізовували, використовуючи метод `animate()`, але нам же хочеться знайти свій шлях:

{% hint style="info" %}
До jQuery 1.8 тут йшлося про метод `pipe()`, а тепер про `then()`
{% endhint %}

У цьому прикладі ми викликаємо метод `then()`, якому згодовано callback-функцію, яка повинна повертати об'єкт Promise. Це необхідно для дотримання порядку в черзі – спробуйте прибрати в прикладі один «`return`», і ви помітите, що наступна анімація настане, не дочекавшись завершення попередньої:

```javascript
var D = $.Deferred();

D.then(function() {
  // чекаємо resolve()
  console.log("hide images")
  $('article img').slideUp(2000).promise()
})
.then(function(){
  // зачекаємо, поки сховаються картинки
  console.log("hide paragraphs")
  $('article p').slideUp(2000).promise()
})
.then(function(){
  // зачекаємо, поки сховаються параграфи
  console.log("hide articles")
  return $('article').hide(2000).promise()
})
.then(function(){
  // все зроблено, шефе
  console.info("done")
});

// секунду і все буде
setTimeout(() => D.resolve(), 1000)
```

{% embed url="https://anton.shevchuk.name/book/code/deferred.html" %}

На цьому можливості Deferred ще не завершилися. Є ще зв'язка методів `notify()` та `progress()` – перший шле послання в callback-функції, які зареєстровані за допомогою другого. Наведу наочний код для демонстрації (відкрийте консоль і подивіться, що виходить):

```javascript
var D = $.Deferred();

var money = 100; // наш бюджет

// знімаємо грошики
D.progress(function(cost){
    console.log(`${money} - ${cost} = ${money-cost}`);
    if (money < cost) { // гроші закінчилися
        D.reject();
        return;
    }
    money -= cost;
});

// витрачаємо гроші
setTimeout(function(){ D.notify(40); }, 500);  // покупка 1
setTimeout(function(){ D.notify(50); }, 1000); // покупка 2
setTimeout(function(){ D.notify(30); }, 1500); // покупка 3
setTimeout(function(){ D.resolve();  }, 3000); // все купили

D.then(function(){ console.info("All Ok") });
D.catch(function(){ console.error("Insufficient Funds") });
```

***

> Випробуйте всю потужність Deferred!

Створимо сервіс для пошуку картинок на сервісі Flickr. Почнемо з об'єкта, який відповідатиме лише за завантаження даних з Flickr:

```javascript
var Flickr = {
  search: function(query) {
    // метод $.getJSON повертає $.Deferred
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

Тепер запустимо пошук і обробимо відповідь сервера:

```javascript
// контейнер, куди будемо складати картинки
// приховуємо його, поки не завантажені картинки
var $images = $("#images").hide();

// прогрес-бар, що відображає процес завантаження картинок
var $progress = $("#progress").find("div");

// наш об'єкт
Flickr
    // запускаємо пошук за ключовим словом
    .search("apple")
    // коли повернулася AJAX-відповідь, додаємо картинки і чекаємо їх завантаження
    .then(function (data) {
        var D = $.Deferred();
        var total = data.items.length;
        var loaded = 0;

        $.each(data.items, function(i, item){
            // створюємо картинку
            var img = new Image();
            img.onload = img.onerror = function() {
                // змінюємо прогрес завантаження
                D.notify(1)
            };
            img.src = item.media.m;
            $(img).prependTo($images);
        });

        D.progress(function() {
            // інкрементимо кількість завантажених картинок
            loaded ++;

            // оновлюємо прогрес-бар
            $progress.width(100/total*loaded + '%');

            // коли всі картинки завантажилися
            if (total === loaded) {
                D.resolve();
            }
        });
        return D.promise();
    })
    // коли всі картинки завантажилися
    .then(function() {
        // ховаємо прогрес-бар      
        $progress.width(0);
        // відображаємо контейнер з усіма картинками
        $images.show("slow");
    })
```

В результаті у нас вийшов простенький пошуковий сервіс:

{% embed url="https://anton.shevchuk.name/book/code/deferred.flickr.html" %}
