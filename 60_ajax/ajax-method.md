# Метод ajax()

Следующий метод, с которым я вас познакомлю будет «$.ajax()» – собственно, он тут главный, и все остальные AJAX-методы являются лишь обёрткой (и «.load()» в том же числе). Метод «$.ajax()» принимает в качестве параметра пачку настроек и URL цели. Приведу пример, аналогичный вызову «.load()»:

```javascript
$.ajax({
    url: "/get-my-page.html", // указываем URL
    method: "GET",            // HTTP метод, по умолчанию GET
    data: {"id": 42},         // данные, которые отправляем на сервер
    dataType: "html",         // тип данных загружаемых с сервера
    success: function (data) {
        // вешаем свой обработчик события success
        $("#content").html(data);
    }
});
```

> _Тут мы обрабатывали HTML-ответ от сервера – это хорошо когда нам полстраницы обновить надо, но данные лучше передавать в «правильном» формате. XML – это понятно, структурировано, но избыточно и как-то не совсем JavaScript-way, и поэтому наш выбор – это_ [_JSON_](http://ru.wikipedia.org/wiki/JSON)_:_

```javascript
{
  "note": {
    "time": "2012.09.21 13:11:15",
    "text": "Рассказать про JSONP"
  }
}
```

Фактически, это и есть JavaScript-код как есть (JavaScript Object Notation, если быть придирчиво точным). При этом формат уже распространён настолько, что работа с данными в другом формате уже не приветствуется.

> _Жизнь не стоит на месте, есть и более удобные форматы, но не в JavaScript :)_

Для загрузки JSON существует быстрая функция-синоним – «jQuery.getJSON()» – в качестве обязательного параметра лишь ссылка, куда стучимся, опционально можно указать данные для передачи на сервер и функцию обратного вызова.

> _Нельзя просто так взять и описать все возможные параметры для вызова «$.ajax()», таки стоит держать под рукой официальный мануал_ [_http://api.jquery.com/jQuery.ajax/_](http://api.jquery.com/jQuery.ajax/)_._

Ещё есть пара-тройка функций, которые стоит упомянуть:

`get(url, data, success, dataType)` — загружает данные методом GET

`post(url, data, success, dataType)` — загружает данные методом POST

`getScript(url, success)` — загружает JavaScript с сервера методом GET

Все они, как я уже говорил ранее, лишь обёртки над вызовом «$.ajax()», в чём несложно убедиться, заглянув в [исходный код библиотеки](https://github.com/jquery/jquery/blob/32b00373b3f42e5cdcb709df53f3b08b7184a944/src/ajax.js#L834). И там вы найдёте реализацию методов «$.get()» и «$.post()» :)

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

> _А функция `getScript()` в свою очередь это обёртка над `$.get()`._
