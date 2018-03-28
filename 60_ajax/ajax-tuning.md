## Прокачиваем AJAX {#ajax}

У нас есть три способа для «прокачки» AJAX в jQuery: это создание префильтров, добавление новых конверторов и транспортов.

### Префильтры {#prefilters}

Префильтр – это функция, которая будет вызвана до «ajaxStart», в ней вы сможете изменить как объект «jqXHR», так и любые сопутствующие настройки:

```javascript
// регистрация AJAX префильтра
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    // наши манипуляции над настройками и jqXHR
});
```

Для чего всё это? Да вот простая задачка – не ждать «старый» AJAX-ответ, если мы запрашиваем URL заново:

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

Ещё можно изменить опции вызова, вот пример, который по флагу «crossDomain» пересылает запрос на заранее подготовленную проксирующую страницу на нашем сервере:

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

Ну и последнее: для переключения «dataType» на какой-нибудь другой тип нам достаточно будет вернуть необходимое значение:

```javascript
$.ajaxPrefilter(function( options ) {
    // это наша функция-детектор необходимых URL
    if ( isActuallyScript( options.url ) ) {
        // теперь «ждём» script
        return "script";
    }
});
```

> _Будьте очень осторожны, когда оперируете глобальными настройками, да ещё через такую неявную фичу, как фильтры. Задокументируйте подобные подходы в сопроводительной документации, иначе разработчики, которые будут в дальнейшем сопровождать ваш код, будут сильно ругаться (в качестве оных через пару месяцев можете оказаться и вы сами). И в целом настоятельно рекомендуется достаточно подробное документирование и комментирование кода. Меткая фраза, ошибочно приписываемая мэтру программирования МакКоннеллу: "Пишите код так, как будто сопровождать его будет склонный к насилию психопат, который знает, где вы живёте" (Джон Ф. Вудс)._

### Конверторы {#converters}

Конвертор – функция обратного вызова, которая вызывается в том случае, когда полученный тип данных не совпадает с ожидаемым (т.е. «dataType» указан неверно).

Все конверторы хранятся в глобальных настройках «ajaxSettings»:

```javascript
// формат ключа "из_формата в_формат"
// в качестве входного формата можно использовать "*"
{
    converters: {
        "* text": window.String,       // что угодно приводим к тексту
        "text html": true,             // текст к HTML (флаг true == без изменений)
        "text json": jQuery.parseJSON, // текст к JSON
        "text xml": jQuery.parseXML    // разбираем текст как XML
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

> _Имена «dataType» должны всегда быть в нижнем регистре._

Конверторы следует использовать, если требуется внедрить произвольные форматы «dataType», или для конвертации данных в нужный формат. Необходимый «dataType» указываем при вызове метода «$.ajax()»:

```javascript
$.ajax( url, { dataType: "mydatatype" });
```

Конверторы можно задавать также непосредственно при вызове «$.ajax()», дабы не засорять общие настройки:

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

> _Чуть-чуть пояснений: мы запрашиваем «XML», который конвертируем в текст, который будет передан в наш конвертор из «text» в «mydatatype»._

### Транспорт {#transport}

> _Использование своего транспорта – это крайняя мера, прибегайте к ней только в том случае, если с поставленной задачей нельзя справиться с использованием префильтров и конверторов._

Транспорт – это объект, который предоставляет два метода «send()» и «abort()» – они будут использоваться внутри метода «$.ajax()». Для регистрации своего метода транспортировки следует использовать метод «$.ajaxTransport()», будет это выглядеть как-то так:

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

Проясню чуток параметры, с которыми будем работать:

`options` – настройки запроса (то, что указываем при вызове «$.ajax()»)

`originalOptions` – «чистые» настройки, даже без учёта изменений «по умолчанию»

`jqXHR` – объект «jQuery XMLHttpRequest»

`headers` – заголовки запроса в виде связки «ключ-значение»

`completeCallback` – функция обратного вызова, её следует использовать для оповещения о завершении запроса

Функция «completeCallback()» имеет следующую сигнатуру:

```javascript
function ( status, statusText, responses, headers ) {
    /* какой-то код */
}
```

где:

`status` – HTTP статус ответа

`statusText` – текстовая интерпретация ответа

`responses` – опционально – объект, содержащий ответы сервера во всех форматах, которые поддерживает транспорт; для примера, родной «XMLHttpRequest» будет выглядеть как «{ xml: XMLData, text: textData }» при запросе XML документа

`headers` – опционально – строка, содержащая заголовки ответа сервера, если транспорт может их получить; например, метод «XMLHttpRequest.getAllResponseHeaders()» это осилит

Как и префильтры, транспорт можно привязывать к определенному типу запрашиваемых данных:

```javascript
$.ajaxTransport( "script", function( options, originalOptions, jqXHR ) {
    /* привязываемся лишь к script*/
});
```

А теперь мега-напряг – добавим транспорт «image» на данную страницу:

{% jqbEval %}{% endjqbEval %}

```javascript
// регистрируем его для типа image
$.ajaxTransport("image", function (options) {
    // отслеживаем лишь GET AJAX-запросы
    if (options.type === "GET" && options.async) {
        var image;
        return {
            // 
            send:function (_, callback) {
                image = new Image();
                // подготовим функцию done
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

Теперь можно воспользоваться данным транспортом:

{% jqbEval %}{% endjqbEval %}

```javascript
$.ajax('../code/img/photo-cat.jpg', {
    dataType:'image',
    success: function(data) {
        $('#image').html(data);
    }
})
```

<div id="image"></div>

> _Я хотел бы ещё раз напомнить, что это «advanced level», и данный раздел лишний в учебнике «для начинающих»._

По следам официальной документации:
* [Extending Ajax: Prefilters, Converters, and Transports](http://api.jquery.com/extending-ajax/)
