# Селектори та Sizzle

{% hint style="info" %}
Пропустіть цей розділ і поверніться до нього тоді, коли вас зацікавить, як відбувається пошук елементів всередині `$(...)`
{% endhint %}

Як «пошуковик» по елементах DOM jQuery використовує бібліотеку [Sizzle](https://github.com/jquery/sizzle/wiki). Ця бібліотека свого часу була невід'ємною частиною jQuery, потім «відбрунькувалася» в окремий проєкт, який з радістю використовується як самим jQuery, так і Dojo Toolkit. Але досить лірики, перейдімо безпосередньо до пошуку. Для цього в JavaScript передбачені наступні методи (не в jQuery, не в Sizzle, а в JavaScript):

| `getElementById(id)`            | пошук по `id="…"`                    |
| ------------------------------- | ------------------------------------ |
| `getElementsByName(name)`       | пошук по `name="name"` |
| `getElementsByClassName(class)` | пошук по `class="class"`             |
| `getElementsByTagName(tag)`     | пошук за ім'ям тегу                  |
| `querySelectorAll(selector)`    | пошук за довільним CSS-селектором |

Пробігшись побіжним поглядом по списку, можна помітити метод `querySelectorAll()` – він універсальний і дійсно зручний. І саме цей метод бібліотека намагається викликати, коли ви згодовуєте щось як селектор у jQuery, але цей метод іноді нас підводить, і тоді на сцену виходить Sizzle у всій красі, озброєний усіма згаданими методами, та ще й зі своїм унікальним арсеналом:

```javascript
if (document.querySelectorAll) (function(){
    var oldSelect = select;
    /* ... */
    select = function( selector, context, results, seed, xml ) {

    // використовуємо querySelectorAll коли немає фільтрів у запиті,
    // коли це запит не по xml-об'єкту,
    // і коли не виявлено проблем із запитом
    // ще є пара перевірок, які я опустив для наочності

    try {    
        push.apply(
            results,            
            slice.call(context.querySelectorAll( selector ), 0)        
        );
        return results;
    } catch(qsaError) { /* підвів, знову, ну скільки можна */ }
        /* ... */
        // вихід Sizzle
        return oldSelect( selector, context, results, seed, xml );
    };
});
```

Але давайте вже розглянемо, як Sizzle шукає в DOM, якщо-таки метод `document.querySelectorAll()` спіткнувся. Почнемо з розбору алгоритму роботи на наступному прикладі:

```javascript
$("thead > .active, tbody > .active")
```

1. Отримати перший вираз до коми: «`thead > .active`»
2. Розбити на шматочки: «`thead`», «`>`», «`.active`»
3. Знайти вихідну множину елементів за останнім шматочком (усі «`.active`»)
4. Фільтрувати справа наліво (усі «`.active`» які знаходяться безпосередньо в «`thead`»)
5. Шукати наступний запит після коми (повертаємося до першого пункту)
6. Залишити тільки унікальні елементи в результаті

Дивлячись на цей алгоритм, ви повинні помітити, що **правила читаються справа наліво**!

Копаємо глибше. При аналізі навіть найменшого виразу є кілька нюансів, на які варто звернути увагу. Перший – це пріоритет пошуку, він різний для різних браузерів (залежно від їхніх можливостей, або «неможливостей»):

```javascript
{
    order: new RegExp( "ID|TAG" +
        (assertUsableName ? "|NAME" : "") +
        (assertUsableClassName ? "|CLASS" : "")
    )
}
```

> Не звертайте увагу на RegExp – це внутрішня кухня Sizzle.

Таким чином, розглядаючи вираз `div#my`, Sizzle знайде спочатку елемент з `id="my"`, а потім вже перевірить на відповідність з `<div>`. Добре, це наче як фільтрація, і вона теж дотримується черговості – це другий нюанс:

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

Третій нюанс – це відносні фільтри, які працюють одразу з двома елементами (вони вам знайомі по CSS-селекторах):

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

> Ой, навіщо я вас усім цим вантажу? Почитайте краще про оптимізацію запитів у наступному розділі.

Офіційна документація по бібліотеці Sizzle доступна на [GitHub проєкту](https://github.com/jquery/sizzle/wiki/).
