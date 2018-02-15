## Прокачиваем AJAX {#ajax}

У нас есть три способа для «прокачки» AJAX'а в jQuery: это создание префильтров, добавление новых конверторов и транспортов.

### Префильтры {#prefilters}

Префильтр – это функция, которая будет вызвана до «ajaxStart», в ней вы сможете изменить как объект «jqXHR», так и любые сопутствующие настройки:

```javascript
// регистрация AJAX префильтра
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // наши манипуляции над настройками и jqXHR
});
```

Для чего всё это? Да вот простая задачка – не ждать «старый» AJAX ответ, если мы запрашиваем URL заново:

```javascript
// коллекция текущих запросов
var currentRequests = {};
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // наша произвольная настройка
    if ( options.abortOnRetry ) {
        if ( currentRequests[ options.url ] ) {
            // отменяем старый запрос
            currentRequests[ options.url ].abort();
        }
        currentRequests[ options.url ] = jqXHR;
    }
});

// вызов с использованием фильтра
$.ajax({
    /* ... */
    abortOnRetry: true
})
```

Ещё можно изменить опции вызова, вот пример который по флагу «crossDomain» пересылает запрос на заранее подготовленную проксирующую страницу на нашем сервере:

```javascript
$.ajaxPrefilter(function( options ) {
    if ( options.crossDomain ) {
        options.url = "/proxy/" + encodeURIComponent( options.url );
        options.crossDomain = false;
    }
});
```

Префильтры можно «вешать» на определенный тип «dataType» (т.е. в зависимости от ожидаемого типа данных от сервера будут срабатывать различные фильтры):

```javascript
$.ajaxPrefilter("json script", function(options, original, jqXHR) {
    /* ... */
});
```

Ну и последнее, для переключение «dataType» на какой-нить другой нам достаточно будет вернуть необходимое значение:

```javascript
$.ajaxPrefilter(function( options ) {
    // это наша функция-детектор необходимых URL
    if ( isActuallyScript( options.url ) ) {
        // теперь «ждём» script
        return "script";
    }
});
```

_Будьте очень осторожны когда оперируете глобальными настройками, да ещё через такую неявную фичу как фильтры – задокументируйте подобные подходы в сопроводительной документации, иначе разработчики которые будут в дальнейшем сопровождать ваш код будут сильно ругаться (в качестве оных можете оказаться и вы сами, ну через пару месяцев)_

### Конверторы {#converters}

Конвертор – функция обратного вызова, которая вызывается в том случае, когда полученный типа данных не совпадает с ожидаемым (т.е. «dataType» указан неверно).

Всё конверторы хранятся в глобальных настройках «ajaxSettings»:

```javascript
// формат ключа "из_формата в_формат"
// в качестве входного формата можно использовать "*"
{
    converters: {
        "* text": window.String,       // что угодно приводим к тексту
        "text html": true,             // текст к html (флаг true == без изменений)
        "text json": jQuery.parseJSON, // текст к JSON
        "text xml": jQuery.parseXML    // разбираем текст как xml
    }
}
```

Для расширения набора конверторов потребуется функция «$.ajaxSetup()»:

```javascript
$.ajaxSetup({
    converters: {
        "text mydatatype": function( textValue ) {
            if ( valid( textValue ) ) {
                // разбор пришедших данных
                return mydatatypeValue;
            } else {
                // возникла ошибка
                throw exceptionObject;
            }
        }
    }
});
```

_Имена «dataType» должны всегда быть в нижнем регистре_

Конверторы следует использовать, если требуется внедрить произвольные форматы «dataType», или для конвертации данных в нужный формат. Необходимый «dataType» указываем при вызове метода «$.ajax()»:

```javascript
$.ajax( url, { dataType: "mydatatype" });
```

Конверторы можно задавать так же непосредственно при вызове «$.ajax()», дабы не засорять общие настройки:

```javascript
$.ajax( url, {
    dataType: "xml text mydatatype",
    converters: {
        "xml text": function( xmlValue ) {
            // получаем необходимые данные из XML
            return textValue;
        }
    }
});
```

_Чуть-чуть пояснений – мы запрашиваем «XML», который конвертируем в текст, который будет передан в наш конвертор из «text» в «mydatatype»_

### Транспорт {#transport}

_Использование своего транспорта – это крайняя мера, прибегайте к ней, только в том случае если с поставленной задачей нельзя справиться с использованием префильтров и конверторов_

Транспорт – это объект, который предоставляет два метода – «send()» и «abort()» – они будут использоваться внутри метода «$.ajax()». Для регистрации своего метода транспортировки следует использовать метод «$.ajaxTransport()», будет это выглядеть как-то так:

```javascript
$.ajaxTransport( function( options, originalOptions, jqXHR ) {
    if ( /* transportCanHandleRequest */ ) {
        return {
            send: function( headers, completeCallback ) {
                /* отправляем запрос */
            },
            abort: function() {
                /* отменяем запрос */
            }
        };
    }
});
```

Проясню чуток параметры с которыми будем работать:

`options` – настройки запроса (то что указываем при вызове «$.ajax()»)

`originalOptions` – «чистые» настройки, даже без учёта изменений «по умолчанию»

`jqXHR` – объект «jQuery XMLHttpRequest»

`headers` – заголовки запроса в виде связки ключ-значение

`completeCallback` – функция обратного вызова, её следует использовать для оповещения о завершении запроса

Функция «completeCallback()» имеет следующую сигнатуру:

```javascript
function ( status, statusText, responses, headers ) {
    /* какой-то код */
}
```

где:

`status` – HTTP статус ответа.

`statusText` – текстовая интерпретация ответа

`responses` – это объект содержащий ответы сервера во всех форматах, которые поддерживает транспорт, для примера: родной «XMLHttpRequest» будет выглядеть как «{ xml: XMLData, text: textData }» при запросе XML документа; опционален

`headers` – строка содержащая заголовки ответа сервера, ну если конечно транспорт может их получить (вот например метод «XMLHttpRequest.getAllResponseHeaders()» это осилит); опционален

Как и префильтры, транспорт можно привязывать к определенному типу запрашиваемых данных:

```javascript
$.ajaxTransport( "script", function( options, originalOptions, jqXHR ) {
    /* привязываемся лишь к script*/
});
```

А теперь мега-напряг – пример транспорта «image»:

```javascript
$.ajaxTransport( "image", function( options ) {
    if (options.type === "GET" && options.async ) {
        var image;
        return {
            send: function( _ , callback ) {
                image = new Image();
                function done( status ) { // подготовим
                    if ( image ) {
                        var statusText = ( status == 200 ) ? "success" : "error", tmp = image;
                        image = image.onreadystatechange = image.onerror = image.onload = null;
                        callback( status, statusText, { image: tmp } );
                    }
                }
                image.onreadystatechange = image.onload = function() {
                    done( 200 );
                };
                image.onerror = function() {
                    done( 404 );
                };
                image.src = options.url;
            },
            abort: function() {
                if ( image ) {
                    image = image.onreadystatechange = image.onerror = image.onload = null;
                }
            }
        }; // /return
    } // /if
}); // /ajaxTransport
```

Рабочий пример вы сможете найти в файле [ajax.transport.html](http://anton.shevchuk.name/book/code/ajax.transport.html), но я хотел бы ещё раз напомнить, что это «advanced level», и данный раздел лишний в учебнике «для начинающих».

По следам официальной документации:

* [Extending Ajax: Prefilters, Converters, and Transports](http://api.jquery.com/extending-ajax/)
