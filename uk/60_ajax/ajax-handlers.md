# Обробники подій

Для зручності розробки AJAX-запити кидають кілька подій, і їх, природно, можна і потрібно обробляти. jQuery дозволяє обробляти ці події для кожного AJAX-запиту і окремо і глобально. Наведу схемку, на якій наочно видно порядок виникнення подій в jQuery:

<figure><img src="../../.gitbook/assets/events.svg" alt=""><figcaption><p>AJAX Events</p></figcaption></figure>

Ось і повний список подій з невеликими ремарками:

<table><thead><tr><th width="195">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>ajaxStart</code></td><td>дана подія виникає у випадку, коли побіг перший AJAX-запит, і при цьому інших активних AJAX-запитів в даний момент немає</td></tr><tr><td><code>beforeSend</code></td><td>виникає до відправки запиту, і дозволяє редагувати XMLHttpRequest, локальна подія</td></tr><tr><td><code>ajaxSend</code></td><td>виникає до відправки запиту, як і <code>beforeSend</code></td></tr><tr><td><code>success</code></td><td>виникає при поверненні відповіді, коли в ній немає помилок, локальна подія</td></tr><tr><td><code>ajaxSuccess</code></td><td>виникає при поверненні відповіді, аналогічно події <code>success</code></td></tr><tr><td><code>error</code></td><td>виникає у випадку помилки, локальна подія</td></tr><tr><td><code>ajaxError</code></td><td>виникає у випадку помилки</td></tr><tr><td><code>complete</code></td><td>виникає після завершення поточного AJAX-запиту за будь-якого розкладу, локальна подія</td></tr><tr><td><code>ajaxComplete</code></td><td>глобальна подія, аналогічна <code>complete</code></td></tr><tr><td><code>ajaxStop</code></td><td>дана подія виникає у випадку, коли більше немає активних AJAX-запитів</td></tr></tbody></table>

Приклад для відображення елемента з `id="loading"` під час виконання будь-якого AJAX-запиту (тобто ми обробляємо глобальну подію):

```javascript
$(document).on("ajaxSend", function() {
    $("#loading").show(); // показуємо елемент
}).on("ajaxStop", function(){
    $("#loading").hide(); // приховуємо елемент
});
```

{% hint style="warning" %}
Це задачка з юзабіліті — ми завжди повинні тримати користувача сайту в курсі справи про те, що відбувається на сторінці і відправка AJAX-запиту теж потрапляє під розряд «must know». Подібне рішення у тебе буде практично на будь-якому сайті, де ходить AJAX.
{% endhint %}

Для локальних подій — вносимо зміни в опції методу `ajax()`:

```javascript
$.ajax({
    beforeSend: function() {
        // даний обробник буде викликано перед відправкою даного AJAX-запиту
    },
    success: function() {
        // а цей при вдалому завершенні
    },
    error: function() {
        // цей при виникненні помилки
    },
    complete: function() {
        // і після завершення запиту (вдалому чи ні)
    }
});
```

Можна глобальні обробники відключити примусово, використовуючи прапорець «`global`». Для цього виставляємо його на `false` і пишемо функціонал в обхід подій `ajaxStart` та `ajaxStop`:

```javascript
$.ajax({
    global: false,
    beforeSend: function() {
        // ...
    },
    success: function() {
        // ...
    },
    error: function() {
        // ...
    },
    complete: function() {
        // ...
    }
});
```

> Дана опція частенько допомагає уникнути конфліктів при роботі з AJAX-запитами, але не варто нею зловживати.
