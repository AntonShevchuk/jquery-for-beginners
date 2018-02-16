## Easing {#easing}

Теперь опять обратимся к easing’у – приведу пример произвольной функции, которой будет следовать анимация. Дабы особо не фантазировать – я взял пример из статьи на вездесущем хабре o [анимации в MooTools фреймворке](http://habrahabr.ru/post/43379/) – наглядный пример с сердцебиением, которое описывается следующими функциями:

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#FFFFFF&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;url&quot;:&quot;https://raw.githubusercontent.com/AntonShevchuk/jquery-book/master/assets/easing.xml&quot;}"></div>
<script type="text/javascript" src="https://www.draw.io/embed2.js?&fetch=https%3A%2F%2Fraw.githubusercontent.com%2FAntonShevchuk%2Fjquery-book%2Fmaster%2Fassets%2Feasing.xml"></script>

В расширении функционала easing нет ничего военного:

```javascript
$.extend($.easing, {
    /**
     * Heart Beat
     *
     * @param x progress
     * @link http://habrahabr.ru/blogs/mootools/43379/
     */
    heart:function(x) {
        if (x < 0.3) return Math.pow(x, 4) * 49.4;
        if (x < 0.4) return 9 * x - 2.3;
        if (x < 0.5) return -13 * x + 6.5;
        if (x < 0.6) return 4 * x - 2;
        if (x < 0.7) return 0.4;
        if (x < 0.75) return 4 * x - 2.4;
        if (x < 0.8) return -4 * x + 3.6;
        if (x >= 0.8) return 1 - Math.sin(Math.acos(x));
    }
});
```

Чуть-чуть пояснений, конструкция «$.extend({}, {})» «смешивает» объекты:

```javascript
$.extend({name:"Anton"}, {location:"Kharkiv"});
// >>>
{
  "name":"Anton",
  "location":"Kharkiv"
};

$.extend({name:"Anton", location:"Kharkiv"}, {location:"Kyiv"});
// >>>
{
  "name":"Anton",
  "location":"Kyiv"
}
```

Таким образом мы «вмешиваем» новый метод к существующему объекту «$.easing»; согласно коду, наш метод принимает в качестве параметра лишь одно значение:

`x` – коэффициент прохождения анимации, изменяется от 0 до 1, дробное

Результат конечно интересен, но его можно ещё чуть-чуть расширить дополнительными функциями (развернем и скомбинируем):

```javascript
{
    heartIn: function (x) {
        return $.easing.heart(x);
    },
    heartOut: function (x) {
        return $.easing.heart(1 - x);
    },
    heartInOut: function (x) {
        if (x < 0.5) return $.easing.heartIn(x);
        return $.easing.heartOut(x);
    }
}
```

Получим следующие производные функции:

| **heartIn** | **heartOut** | **heartInOut** |
| --- | --- | --- |

Работать с данным творением надо следующим образом:

```javascript
$("#my").animate({height:"+200px"}, 2000, "heartIn"); // вот оно
```

Пример работы данной функции можно пощупать на странице [easing.html](http://anton.shevchuk.name/book/code/easing.html)
