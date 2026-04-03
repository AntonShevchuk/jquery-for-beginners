# Метод ajax()

Наступний метод, з яким я вас познайомлю буде `$.ajax()` — власне, він тут головний, і всі інші AJAX-методи є лише обгорткою (і `load()` в тому числі). Метод `$.ajax()` приймає як параметр пачку налаштувань та URL.

Наведу приклад, аналогічний виклику `load()`:

```javascript
$.ajax({
    url: "/get-my-page.html", // вказуємо URL
    method: "GET",            // HTTP метод, за замовчуванням GET
    data: { "id": 42 },       // дані, які відправляємо на сервер
    dataType: "html",         // тип даних що завантажуються з сервера
    success: function (data) {
        // навішуємо свій обробник події success
        $("#content").html(data);
    }
});
```

{% embed url="https://anton.shevchuk.name/book/code/ajax.html" %}

Тут ми обробляли HTML-відповідь від сервера — це добре коли нам півсторінки оновити треба, але дані краще передавати в «правильному» форматі. XML — це зрозуміло, структуровано, але надлишково і якось не зовсім JavaScript-way, і тому наш вибір — це [JSON](https://uk.wikipedia.org/wiki/JSON):

```javascript
{
  "note": {
    "time": "2012.09.21 13:11:15",
    "text": "Розповісти про JSONP"
  }
}
```

Фактично, це і є JavaScript-код як є (JavaScript Object Notation, якщо бути прискіпливо точним). При цьому формат вже поширений настільки, що робота з даними в іншому форматі вже не вітається.

> Життя не стоїть на місці, є й зручніші формати, але не в JavaScript :)\
> Ми ж задовольняємося малим — [текст, HTML, JSON або XML](https://anton.shevchuk.name/book/code/ajax.datatype.html).

Для завантаження JSON існує швидка функція-синонім — `$.getJSON()` — як обов'язковий параметр лише посилання, куди стукаємось, опціонально можна вказати дані для передачі на сервер та функцію зворотного виклику.

{% hint style="info" %}
Не можна просто так взяти й описати всі можливі параметри для виклику `$.ajax()`, таки варто тримати під рукою офіційний мануал [https://api.jquery.com/jQuery.ajax/](https://api.jquery.com/jQuery.ajax/).
{% endhint %}

Ще є пара-трійка методів, які варто згадати:

<table data-header-hidden><thead><tr><th width="361">метод</th><th>опис</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">get(url, data, success, dataType)
</code></pre></td><td>завантажує дані методом GET</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">post(url, data, success, dataType)
</code></pre></td><td>завантажує дані методом POST</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">getScript(url, success)
</code></pre></td><td>завантажує JavaScript з сервера методом GET</td></tr></tbody></table>

Всі вони, як я вже казав раніше, лише обгортки над викликом `$.ajax()`, в чому нескладно переконатися, зазирнувши в [вихідний код бібліотеки](https://github.com/jquery/jquery/blob/32b00373b3f42e5cdcb709df53f3b08b7184a944/src/ajax.js#L834). І там ви знайдете реалізацію методів `$.get()` та `$.post()` :)

```javascript
jQuery.each( [ "get", "post" ], function( i, method ) {
    jQuery[ method ] = function( url, data, callback, type ) {

        // Shift arguments if data argument was omitted
        if ( isFunction( data ) ) {
            type = type || callback;
            callback = data;
            data = undefined;
        }

        // The url can be an options object (which then must have .url)
        return jQuery.ajax( jQuery.extend( {
            url: url,
            type: method,
            dataType: type,
            data: data,
            success: callback
        }, jQuery.isPlainObject( url ) && url ) );
    };
} );
```

Метод `getScript()` в свою чергу це лише обгортка над `$.get()`:

{% embed url="https://anton.shevchuk.name/book/code/ajax.script.html" %}
