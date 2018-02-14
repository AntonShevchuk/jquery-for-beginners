## Sizzle {#sizzle}

_Пропустите это раздел, и вернитесь к нему тогда, когда вас заинтересует, как происходит поиск элементов внутри «$»_

В качестве «поисковика» по элементам DOM'а jQuery использует библиотеку Sizzle. Данная библиотека одно время была неотъемлемой частью jQuery, затем «отпочковалась» в отдельный проект, который с радостью используется как самим jQuery, так и Dojo Toolkit. Но хватит лирики, давайте перейдем непосредственно к поиску. Для оного в JavaScript'е предусмотрены следующие методы (не в jQuery, не в Sizzle, а в JavaScript'е):

`getElementById(id)` — поиск по «id="…"»

`getElementsByName(name)` — поиск по «name="name"» и «id="name"»

`getElementsByClassName(class)` — поиск по «class="class"»

`getElementsByTagName(tag)` — поиск по тегу

`querySelectorAll(selector)` — поиск по произвольному CSS-селектору

Пробежавшись беглым взглядом по списку, можно заметить метод «querySelectorAll()» – он универсален и действительно
удобен. И именно этот метод библиотека пытается вызвать, когда вы скармливаете что-то в качестве селектора в jQuery,
но данный метод иногда нас подводит, и тогда на сцену выходит Sizzle во всей красе, вооруженный всеми упомянутыми
методами, да ещё со своим уникальным арсеналом:

```javascript
if (document.querySelectorAll) (function(){
    var oldSelect = select;
    /* ... */
    select = function( selector, context, results, seed, xml ) {

    // используем querySelectorAll когда нет фильтров в запросе,
    // когда это запрос не по xml-объекту,
    // и когда не обнаружено проблем с запросом
    // ещё есть пара проверок, которые я опустил для наглядности

    try {    
        push.apply(
            results,            
            slice.call(context.querySelectorAll( selector ), 0)        
        );
        return results;
    } catch(qsaError) { /* подвёл, опять, ну сколько можно */ }
        /* ... */
        // выход Sizzle
        return oldSelect( selector, context, results, seed, xml );
    };
});
```

Но давайте уже рассмотрим, как Sizzle ищет в DOM'е, если-таки метод «document.querySelectorAll()» споткнулся. 
Начнём с разбора алгоритма работы на следующем примере:

```javascript
$("thead > .active, tbody > .active")
```

1.  Получить первое выражение до запятой: «thead > .active»
2.  Разбить на кусочки: «thead», «>», «.active»
3.  Найти исходное множество элементов по последнему кусочку (все «.active»)
4.  Фильтровать справа налево (все «.active» которые находятся непосредственно в «thead»)
5.  Искать следующий запрос после запятой (возвращаемся к первому пункту)
6.  Оставить только уникальные элементы в результате

Глядя на данный алгоритм вы должны заметить, что **правила читаются справа налево**!

Копаем глубже. При анализе даже самого маленького выражения есть несколько нюансов, на которые стоит обратить внимание. Первый – это приоритет поиска, он различен для различных браузеров (в зависимости от их возможностей, или «невозможностей»):

```javascript
{
    order: new RegExp( "ID|TAG" +
        (assertUsableName ? "|NAME" : "") +
        (assertUsableClassName ? "|CLASS" : "")
    )
}
```


_Не обращайте внимание на RegExp – это внутренняя кухня Sizzle_

Таким образом, рассматривая выражение «div#my», Sizzle найдёт вначале элемент с «id="my"», а потом уже проверит 
на соответствие с `<div>`. Хорошо, это вроде как фильтрация, и она тоже соблюдает очерёдность – это второй нюанс:

```javascript
{
    preFilter: {    
        "ATTR": function (match) { /* ... */ },
        "CHILD": function (match) { /* ... */ },
        "PSEUDO": function (match) { /* ... */ }
    },    
    filter: {    
        "ID": function (id) { /* ... */ },
        "TAG": function (nodeName) { /* ... */ },
        "CLASS": function (className) { /* ... */ },
        "ATTR": function (name, operator, check) { /* ... */ },
        "CHILD": function (type, argument, first, last) { /* ... */ },
        "PSEUDO": function (pseudo, argument, context, xml) { /* ... */ }
    }
}
```

Третий нюанс – это относительные фильтры, которые работают сразу с двумя элементами (они вам знакомы по CSS-селекторам):

```javascript
{
    relative: {    
        ">": { dir: "parentNode", first: true },    
        " ": { dir: "parentNode" },    
        "+": { dir: "previousSibling", first: true },    
        "~": { dir: "previousSibling" }    
    }
}
```

Ой, зачем я вас всем этим гружу? Почитайте лучше об оптимизации запросов абзацем ниже.

Официальная документация по библиотеке Sizzle доступна на GitHub'е проекта:

* [Sizzle Documentation](https://github.com/jquery/sizzle/wiki/Sizzle-Documentation)
