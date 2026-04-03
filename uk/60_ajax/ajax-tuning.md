---
description: Стаття не для початківців ☝️🧐
---

# Прокачуємо AJAX

У нас є три способи для «прокачки» AJAX в jQuery: це створення префільтрів, додавання нових конверторів та транспортів.

## Префільтри <a href="#prefilters" id="prefilters"></a>

Префільтр — це функція, яка буде викликана до події `ajaxStart`, в ній ви зможете змінити як об'єкт `jqXHR`, так і будь-які супутні налаштування:

```javascript
// реєстрація AJAX префільтра
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // наші маніпуляції над налаштуваннями та jqXHR
});
```

Для чого все це? Та ось проста задачка — не чекати «стару» AJAX-відповідь, якщо ми запитуємо URL заново:

```javascript
// колекція поточних запитів
var currentRequests = {};
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // наше довільне налаштування
    if ( options.abortOnRetry ) {
        if ( currentRequests[ options.url ] ) {
            // скасовуємо старий запит
            currentRequests[ options.url ].abort();
        }
        currentRequests[ options.url ] = jqXHR;
    }
});

// виклик з використанням фільтра
$.ajax({
    /* ... */
    abortOnRetry: true
})
```

Ще можна змінити опції виклику, ось приклад, який за прапорцем `crossDomain` пересилає запит на заздалегідь підготовлену проксуючу сторінку на нашому сервері:

```javascript
$.ajaxPrefilter(function( options ) {
    if ( options.crossDomain ) {
        options.url = "/proxy/" + encodeURIComponent( options.url );
        options.crossDomain = false;
    }
});
```

Префільтри можна «навішувати» на певний тип `dataType` (тобто залежно від очікуваного типу даних від сервера будуть спрацьовувати різні фільтри):

```javascript
$.ajaxPrefilter("json script", function(options, original, jqXHR) {
    /* ... */
});
```

Ну й останнє: для перемикання `dataType` на якийсь інший тип нам достатньо буде повернути необхідне значення:

```javascript
$.ajaxPrefilter(function( options ) {
    // це наша функція-детектор необхідних URL
    if ( isActuallyScript( options.url ) ) {
        // тепер «чекаємо» script
        return "script";
    }
});
```

{% hint style="warning" %}
Будьте дуже обережні, коли оперуєте глобальними налаштуваннями, та ще через таку неявну фічу, як фільтри. Задокументуйте подібні підходи в супровідній документації, інакше розробники, які будуть надалі супроводжувати ваш код, будуть сильно лаятися (як такі через пару місяців можете виявитись і ви самі). І загалом настійно рекомендується достатньо детальне документування та коментування коду. Влучна фраза, помилково приписувана метру програмування МакКоннеллу: _"Пишіть код так, ніби супроводжувати його буде схильний до насильства психопат, який знає, де ви живете"_ (Джон Ф. Вудс).
{% endhint %}

## Конвертори <a href="#converters" id="converters"></a>

Конвертор — функція зворотного виклику, яка викликається в тому випадку, коли отриманий тип даних не збігається з очікуваним (тобто `dataType` вказано неправильно).

Усі конвертори зберігаються в глобальних налаштуваннях `ajaxSettings`:

```javascript
// формат ключа "з_формату в_формат"
// як вхідний формат можна використовувати "*"
{
    converters: {
        "* text":    String,         // що завгодно приводимо до тексту
        "text html": true,           // текст до HTML (прапорець true == без змін)
        "text json": JSON.parse,     // текст до JSON
        "text xml":  jQuery.parseXML // розбираємо текст як XML
    }
}
```

Для розширення набору конверторів знадобиться метод `$.ajaxSetup()`:

```javascript
$.ajaxSetup({
    converters: {
        "text mydatatype": function( textValue ) {
            if ( valid( textValue ) ) {
                // розбір отриманих даних
                return mydatatypeValue;
            } else {
                // виникла помилка
                throw exceptionObject;
            }
        }
    }
});
```

{% hint style="info" %}
Імена `dataType` повинні завжди бути в нижньому регістрі.
{% endhint %}

Конвертори слід використовувати, якщо потрібно впровадити довільні формати `dataType`, або для конвертації даних у потрібний формат. Необхідний `dataType` вказуємо при виклику методу `$.ajax()`:

```javascript
$.ajax( url, { dataType: "mydatatype" });
```

Конвертори можна задавати також безпосередньо при виклику `$.ajax()`, щоб не засмічувати загальні налаштування:

```javascript
$.ajax( url, {
    dataType: "xml text mydatatype",
    converters: {
        "xml text": function( xmlValue ) {
            // отримуємо необхідні дані з XML
            return textValue;
        }
    }
});
```

> Трішки пояснень: ми запитуємо «XML», який конвертуємо в текст, який буде переданий у наш конвертор з «`text`» в «`mydatatype`».

## Транспорт <a href="#transport" id="transport"></a>

{% hint style="warning" %}
Використання свого транспорту — це крайній захід, удавайтеся до нього лише в тому випадку, якщо з поставленим завданням не можна впоратися з використанням префільтрів та конверторів.
{% endhint %}

Транспорт — це об'єкт, який надає два методи `send()` та `abort()` — вони будуть використовуватися всередині методу `$.ajax()`. Для реєстрації свого методу транспортування слід використовувати метод `$.ajaxTransport()`, буде це виглядати якось так:

```javascript
$.ajaxTransport( function( options, originalOptions, jqXHR ) {
    if ( /* transportCanHandleRequest */ ) {
        return {
            send: function( headers, completeCallback ) {
                /* відправляємо запит */
            },
            abort: function() {
                /* скасовуємо запит */
            }
        };
    }
});
```

Поясню трішки параметри, з якими будемо працювати:

<table data-header-hidden><thead><tr><th width="258">параметр</th><th>опис</th></tr></thead><tbody><tr><td><code>options</code></td><td>налаштування запиту<br>тобто те, що вказуємо при виклику <code>$.ajax()</code></td></tr><tr><td><code>originalOptions</code></td><td><p>«чисті» налаштування,</p><p>навіть без урахування змін «за замовчуванням»</p></td></tr><tr><td><code>jqXHR</code></td><td>об'єкт <code>jQuery XMLHttpRequest</code></td></tr><tr><td><code>headers</code></td><td>заголовки запиту у вигляді зв'язки «ключ-значення»</td></tr><tr><td><code>completeCallback</code></td><td>функція зворотного виклику, її слід використовувати для сповіщення про завершення запиту</td></tr></tbody></table>

Функція `completeCallback()` має наступну сигнатуру:

```javascript
function ( status, statusText, responses, headers ) {
    /* якийсь код */
}
```

де:

<table><thead><tr><th width="238">параметр</th><th>опис</th></tr></thead><tbody><tr><td><code>status</code></td><td>HTTP статус відповіді</td></tr><tr><td><code>statusText</code></td><td>текстова інтерпретація статусу відповіді</td></tr><tr><td><code>responses</code></td><td>опціонально — об'єкт, що містить відповіді сервера в усіх форматах, які підтримує транспорт; <br>наприклад, рідний <code>XMLHttpRequest</code> буде виглядати як <code>{ xml: XMLData, text: textData }</code> при запиті XML документа</td></tr><tr><td><code>headers</code></td><td>опціонально — рядок, що містить заголовки відповіді сервера, якщо транспорт може їх отримати; <br>наприклад, метод <code>XMLHttpRequest.getAllResponseHeaders()</code> це здужає</td></tr></tbody></table>

Як і префільтри, транспорт можна прив'язувати до певного типу запитуваних даних:

```javascript
$.ajaxTransport( "script", function( options, originalOptions, jqXHR ) {
    /* прив'язуємося лише до script*/
});
```

А тепер мега-напряг — додамо транспорт «`image`» на нашу сторінку:

```javascript
// реєструємо його для типу image
$.ajaxTransport("image", function (options) {
    // відстежуємо лише GET AJAX-запити
    if (options.type === "GET" && options.async) {
        var image;
        return {
            // 
            send:function (_, callback) {
                image = new Image();
                // підготуємо функцію done
                function done(status) {
                    if (image) {
                        var statusText = ( status == 200 ) ? "success" : "error", tmp = image;
                        image = image.onreadystatechange = image.onerror = image.onload = null;
                        callback(status, statusText, { image:tmp });
                    }
                }
                image.onreadystatechange = image.onload = function () {
                    done(200);
                };
                image.onerror = function () {
                    done(404);
                };
                image.src = options.url;
            },
            abort:function () {
                if (image) {
                    image = image.onreadystatechange = image.onerror = image.onload = null;
                }
            }
        };
    }
});
```

Тепер можна скористатися даним транспортом:

```javascript
$.ajax('../code/img/photo-cat.jpg', {
    dataType:'image',
    success: function(data) {
        $('#image').html(data.image);
    }
})
```

{% embed url="https://anton.shevchuk.name/book/code/ajax.transport.html" %}

> Я хотів би ще раз нагадати, що це «advanced level», і даний розділ зайвий у підручнику «для початківців».

За слідами офіційної документації:

* [Extending Ajax: Prefilters, Converters, and Transports](https://api.jquery.com/jQuery.ajax/#extending-ajax)
