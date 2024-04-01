# Метод ajax()

Следующий метод, с которым я вас познакомлю будет `$.ajax()` – собственно, он тут главный, и все остальные AJAX-методы являются лишь обёрткой (и `load()` в том же числе). Метод `$.ajax()` принимает в качестве параметра пачку настроек и URL.&#x20;

Приведу пример, аналогичный вызову `load()`:

```javascript
$.ajax({
    url: "/get-my-page.html", // указываем URL
    method: "GET",            // HTTP метод, по умолчанию GET
    data: { "id": 42 },       // данные, которые отправляем на сервер
    dataType: "html",         // тип данных загружаемых с сервера
    success: function (data) {
        // вешаем свой обработчик события success
        $("#content").html(data);
    }
});
```

Тут мы обрабатывали HTML-ответ от сервера – это хорошо когда нам полстраницы обновить надо, но данные лучше передавать в «правильном» формате. XML – это понятно, структурировано, но избыточно и как-то не совсем JavaScript-way, и поэтому наш выбор – это [JSON](http://ru.wikipedia.org/wiki/JSON):

```javascript
{
  "note": {
    "time": "2012.09.21 13:11:15",
    "text": "Рассказать про JSONP"
  }
}
```

Фактически, это и есть JavaScript-код как есть (JavaScript Object Notation, если быть придирчиво точным). При этом формат уже распространён настолько, что работа с данными в другом формате уже не приветствуется.

> Жизнь не стоит на месте, есть и более удобные форматы, но не в JavaScript :)

Для загрузки JSON существует быстрая функция-синоним — `$.getJSON()` — в качестве обязательного параметра лишь ссылка, куда стучимся, опционально можно указать данные для передачи на сервер и функцию обратного вызова.

> Нельзя просто так взять и описать все возможные параметры для вызова «$.ajax()», таки стоит держать под рукой официальный мануал [http://api.jquery.com/jQuery.ajax/](http://api.jquery.com/jQuery.ajax/).

{% hint style="info" %}
Нельзя просто так взять и описать все возможные параметры для вызова `$.ajax()`, таки стоит держать под рукой официальный мануал [https://api.jquery.com/jQuery.ajax/](https://api.jquery.com/jQuery.ajax/).
{% endhint %}

Ещё есть пара-тройка методов, которые стоит упомянуть:

<table data-header-hidden><thead><tr><th width="361">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">get(url, data, success, dataType)
</code></pre></td><td>загружает данные методом GET</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">post(url, data, success, dataType)
</code></pre></td><td>загружает данные методом POST</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">getScript(url, success)
</code></pre></td><td>загружает JavaScript с сервера методом GET</td></tr></tbody></table>

Все они, как я уже говорил ранее, лишь обёртки над вызовом `$.ajax()`, в чём несложно убедиться, заглянув в [исходный код библиотеки](https://github.com/jquery/jquery/blob/32b00373b3f42e5cdcb709df53f3b08b7184a944/src/ajax.js#L834). И там вы найдёте реализацию методов `$.get()` и `$.post()` :)

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

> А метод `getScript()` в свою очередь это обёртка над `$.get()`.
