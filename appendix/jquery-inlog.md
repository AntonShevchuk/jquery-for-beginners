## jQuery-inlog {#jquery-inlog}

Ещё чуть-чуть о полезном инструментарии: есть такой классный плагин [jQuery-inlog](http://prinzhorn.github.com/jquery-inlog/). Основное его назначение — дать нам чуть-чуть больше понимания о происходящем внутри самого jQuery. Вот кусочек HTML:

```html
<body>
    <div class="bar">
        <div class="bar">
            <div id="foo"></div>
        </div>
    </div>
    <div id="bacon"></div>
</body>
```

А вот и код, который его обслуживает:

```javascript
$l(true);
$("#foo").parents(".bar").next().prev().parent().fadeOut();
$l(false);
```

Какие-то странные манипуляции, для какого же элемента будет применён метод «.fadeOut()»? Для выяснения оного наш код обёрнут в вызов метода «$l()». «$l()» — это и есть, собственно, вызов плагина. Результат его работы можно найти в консоли:

![jQuery-inlog](/assets/img/jquery-inlog.png)

У данного плагина есть ещё настройки, которые регулируют объём информации выводимой в консоль.

> _Пример и скриншот можно взять из [официальной документации](http://prinzhorn.github.com/jquery-inlog/) по плагину._