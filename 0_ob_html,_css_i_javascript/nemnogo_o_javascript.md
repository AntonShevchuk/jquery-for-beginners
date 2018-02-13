## Немного о JavaScript {#javascript}

В данный раздел я вынес ту информацию о JavaScript, которую необходимо знать, чтобы у вас не возникало
«детских» проблем с использованием jQuery. Если у вас есть опыт работы с JavaScript, то листайте далее.

_Изучать хотите JavaScript и jQuery? Так силу познайте инструмента истинного:_

* [Developer Tools](http://www.html5rocks.com/en/tutorials/developertools/part1/) - для Chrome, Safari и других webkit-based браузеров
* [Developer Tools](https://developer.mozilla.org/ru/docs/Tools) - для FireFox
* [Developer Tools](https://developer.microsoft.com/en-us/microsoft-edge/platform/documentation/f12-devtools-guide/) - для Microsoft Edge
* [DragonFly](http://www.opera.com/dragonfly/) - для Opera

_Список не полон, но [console](https://developers.google.com/chrome-developer-tools/docs/console) там есть, применять её надо уметь_

### О форматировании

Хотел бы сразу обратить внимание на форматирование JavaScript-кода. Мой опыт мне подсказывает — лучше всего спроецировать стандарты форматирования основного языка разработки на прикладной — JavaScript, а если вы хотите чего-нить из глобального и общепринятого, то я для вас уже погуглил:

*   «jQuery Core Style Guidelines» [[http://contribute.jquery.org/style-guide/js/](http://contribute.jquery.org/style-guide/js/)]
*   «Airbnb JavaScript Style Guide» [[https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)]
*   «Как писать неподдерживаемый код?» – вредные советы от Ильи [[http://learn.javascript.ru/write-unmain-code](http://learn.javascript.ru/write-unmain-code)]

В довесок поделюсь небольшим советом: все переменные, содержащие объект jQuery, лучше всего именовать, начиная с символа «$». Поверьте, такая небольшая хитрость экономит много времени.

И ещё — в конце каждой строки я ставлю точку с запятой, сам JavaScript этого не требует, но и лишним не будет.

### Основы JavaScript

#### Переменные {#javascript-vars}

Первое, с чем столкнёмся – это объявление переменных:

```javascript
var name = "Ivan";
var age = 32;
```

Всё просто, объявляем переменную, используя ключевое слово «var».

_Можно, конечно же, и без «var», но делать я вам это настоятельно не рекомендую, т.к. могут возникнуть непредвиденные проблемы в коде, о чём чуть позже расскажу._

Да, да, JavaScript относится к языкам с динамической типизацией, и нам нет нужды указывать тип данных при объявлении переменных, вы даже можете устроить «holy war» по этому поводу, но делайте это локально, не выходя за рамки своей черепной коробки.

На имена переменных наложено два ограничения:

*   имя может состоять из букв, цифр, и символов «$» и «_»
*   первый символ не должен быть цифрой

Учтите, регистр букв имеет значение:

```javascript
var company = "Facebook";
// совсем другая «компания»
var Company = "Google";
```

Хочу также вас познакомить с таким нововведением ECMAScript-2015 (в дальнейшем ES-2015), как объявление переменных с использованием конструкции «let»:

```javascript
let company = "Facebook";
````

Внешне, от «var» не сильно отличается, зато какая разница в поведении:

*   область видимости переменной «let» ограничена блоком `{...}`, в отличие от «var», которая видна везде внутри функции:

```javascript
var a = 0;
if (true) {
    var a = 1000;
    alert(a); // 1000
}
alert(a); // 1000
```

А теперь сравните с поведением «let»:

```javascript
let a = 0;
if (true) {
    let a = 1000;
    alert(a); // 1000
}
alert(a); // 0
```

*   переменная «let» видна только после объявления:

```javascript
alert(a); // undefined
var a = 0;
alert(b); // error: 'b' is not defined
let b = 0;
```

*   переменную «let» нельзя объявить повторно:

```javascript
var a;
var a; // ок
let b;
let b; // error: 'b' has already been declared

```

*   внутри цикла переменная «let» будет объявлена новая для каждой итерации:

```javascript
for (var i = 0; i < 10; i++) { /* … */ }
alert(i); // 10
for (let j = 0; j < 10; j++) { /* … */ }
alert(j); // error: 'j' is not defined
```

#### Константы {#javascript-constants}

В JavaScript'е до ES-2015 не было констант, но поскольку необходимость в них всё же была и до того, то была негласная договорённость: переменные, набранные в верхнем регистре через подчёркивание, не изменять:


```javascript
var USER_STATUS_ACTIVE = 1;
var USER_STATUS_BANNED = 2;
```

_Константы необходимы, чтобы избежать появления «magic numbers». Взгляните на следующий код «if(status==2)» — о чём тут речь мало кому будет понятно, а вот код «if(status==USER_STATUS_BANNED)» уже более информативный_

Если же говорить об эре ES-2015, то тут уже используем «const», а об остальном уже позаботится сам JavaScript:

```javascript
const USER_STATUS_ACTIVE = 1;
USER_STATUS_ACTIVE = 2; // error: assignment to constant variable
```

_Да-да, я повторюсь, это наглядный пример правильного именования констант — имя однозначно указывает на хранимое значение — «статус активного пользователя»_

Отдельно стоит заметить, константа не позволяет изменять саму переменную, но если мы присвоим константе объект или массив, то с ним вы сможете делать что душа пожелает:


```javascript
const USER_STATUS_ACTIVE = 1;
const USER_STATUS_BANNED = 2;

const USER = {
    name: "mr.Smith",
    status: USER_STATUS_BANNED
};

USER.status = USER_STATUS_ACTIVE; // ok

USER = {name: "mr.Wesson"}; // error
```

Вот и получился «читаемый» код, мотайте на ус

#### Типы данных {#javascript-types}

В JavaScript не так уж и много типов данных:

*   `number` — целое или дробное число:

```javascript
var answer = 42;
var pi = 3.1415;
```

также существуют следующие специальные значения:

`NaN (not-a-number)` — результат числовой операции, которая завершилась ошибкой, запустите следующий код в консоли:

```javascript
Math.sqrt(-5);
```

но учтите:

```javascript
(NaN == NaN) == false;

isNaN(NaN) == true;
```

`Infinity` — за гранью 1.7976931348623157E+10308 (т.е. больше)

`-Infinity` — за гранью -1.7976931348623157E+10308 (т.е. меньше)

*   string — строка, заключается в кавычки:

```javascript
var str = "Hello World!";
```

_в JavaScript нет разницы между двойными кавычками и одинарными (привет PHP)_

*   `boolean` — булево значение, т.е. или «true» или «false»

```javascript
var result = true;
```

*   `object` — это объекты, на них остановлюсь подробнее чуть позже…
*   `symbol` — тип данных из ES-2015, служит для создания уникальных идентификаторов (о нём рассказа не будет, не в ходу ещё)
*   `null` — специальное значение для определения «пустоты»

```javascript
var result = null;
```

*   `undefined` — ещё одно специальное значение, для «неопределенности», используется как значение неопределённой или несуществующей переменной, например, если переменная объявлена, но значение ей ещё не присвоено:

```javascript
// переменная есть, но нет значения
var a;
alert(a); // undefined
if (typeof a == "undefined") {
    alert("variable is undefined");
}

// или может совсем не быть переменной
if (window["a"] == undefined) {
    alert("variable does not exist");
}
```

_Во втором примере нас может ожидать сюрприз, если кто определит переменную «undefined», как обойти такую «неприятность» я ещё расскажу._

#### Массивы {#javascript-arrays}

Массив — это коллекция данных с числовыми индексами. Данные могут быть любого типа, в качестве примера я приведу один из самых простых вариантов — массив со строками:

```javascript
//             0       1       2
var users = ["Ivan", "Petr", "Serg"]
```

Нумерация массивов начинается с «0», так что для получения первого элемента вам потребуется следующий код:

```javascript
alert(users[0]); // выведет Ivan
```

Размер массива хранится в свойстве length:

```javascript
alert(users.length); // выведет 3

users[3] = "Danylo";

alert(users.length); // выведет 4
```

_В действительности «length» возвращает индекс последнего элемента массива+1, так что не попадитесь:_

```javascript
var a = [];

a[4] = 10;

alert(a.length); // выведет 5;
```

Для перебора массива лучше всего использовать цикл «for(;;)»:

```javascript
for (let i = 0; i < users.length; i++) {
    alert(users[i]); // последовательно выведет Ivan, Petr и Serg
}
```

Для работы с последними элементами массива следует использовать методы «push()» и «pop()»:

```javascript
users.push("Sidorov"); // добавляем элемент в конец массива

var sidorov = users.pop(); // удаляем и возращаем последний элемент
```

Для работы с первыми элементами массива следует использовать методы «unshift()» и «shift()»:

```javascript
users.unshift("Sidorov"); // добавляем элемент в начало массива

var sidorov = users.shift(); // удаляем и возращаем первый элемент
```

_Последние два метода работают медленно, т.к. перестраивают весь массив_

#### Функции {#javascript-functions}

С функциями в JavaScript'е всё просто, вот вам элементарный пример:

```javascript
function hello() {
    alert("Hello world");
}
```

Просто, пока не заговорить об анонимных функциях…

#### Анонимные функции {#javascript-anonymous-functions}

В JavaScript можно создавать анонимную функцию (т.е. функцию без имени), для этого достаточно слегка изменить предыдущую конструкцию:

```javascript
function() {
    alert("Hello world");
}
```

Так как функция — это вполне себе объект, то её можно присвоить переменной, и (или) передать в качестве параметра в другую функцию:

```javascript
var myAlert = function(name) {
    alert("Hello " + name);
}

_function helloMike(myFunc) {_ // тут функция передаётся как параметр
    myFunc("Mike"); // а тут мы её вызываем
}

helloMike(myAlert);
```

Анонимную функцию можно создать и тут же вызвать с необходимыми параметрами:

```javascript
(function(name) {
    alert("Hello " + name);
})("Mike");
```

Это несложно, скоро вы к ним привыкнете, и вам их будет недоставать в других языках.

Ах, этот ES-2015, он принёс нам сокращенную запись для анонимных функций — функции-стрелки:

```javascript
// была простая анонимная функция

var inc = function (x) {
    return x+1;
}

// стала запись в одну строчку
var inc = x => x+1;

// была функция с несколькими аргументами
var sum = function (a, b) {
    return a+b;
}

// стала запись в одну строчку
var sum = (a, b) => a+b;
```

Данная запись конечно же удобная, но не стоит увлекаться, если перестараться, то ваш код станет нечитаем, и по итогу вы можете услышать много не лестных выражений в свой адрес.

#### Объекты {#javascript-objects}

На объекты в JavaScript возложено две роли:

*   хранилище данных
*   функционал объекта

Первое предназначение можно описать следующим кодом:

```javascript
var user = {
    name: "Ivan",
    age: 32
};

alert(user.name); // Ivan
alert(user.age); // 32
```

Это фактически реализация key-value хранилища, или хэша, или ассоциативного массива, или …, ну вы поняли, названий много, но в JavaScript'е это объект, и запись выше – это JSON – JavaScript Object Notation (хоть и с небольшими оговорками).

Для перебора такого хранилища можно использовать цикл «for(.. in ..)»:

```javascript
for (let prop in user) {
    alert(prop + "=" + user[prop]); // выведет name=Ivan
                                    // затем age=32
}
```

С объектами, конструкторами и т.д. в JavaScript посложнее будет, хотя для понимания не так уж и много надо. Запоминайте: любая функция, вызванная с использованием ключевого слова «new», возвращает нам объект, а сама становится конструктором данного объекта:

```javascript
function User(name) {
    this.name = name;
    this.status = USER_STATUS_ACTIVE;
}

var me = new User("Anton");
```

Поведение функции «User()» при использовании «new» слегка изменится:

1.  Данная конструкция создаст новый, пустой объект
2.  Ключевое слово «this» получит ссылку на этот объект
3.  Функция выполнится и, возможно, изменит объект через «this» (как в примере выше)
4.  Функция вернёт «this» (по умолчанию)

Результатом выполнения кода будет следующий объект:

```javascript
{name: "Anton", status: 1 }
```

_Опять отправлю читать про ES-2015, в данном стандарте появилась конструкция «class», что по сути — синтаксический сахар для JavaScript’а — специально для тех, кто любит C-подобные языки программирования —_ [_https://learn.javascript.ru/es-class_](https://learn.javascript.ru/es-class)_._

### Область видимости и чудо this {#javascript-this}

Для тех, кто только начинает своё знакомство с JavaScript я расскажу следующие нюансы:

*   когда вы объявляете переменную или функцию, то она становится частью «window»:

```javascript
var a = 1234;

alert(window["a"]); // => 1234

function myLog(message) {
    alert(message); // => 1234
}

window["myLog"](a);
```

*   когда искомая переменная не найдена в текущей области видимости, то её поиски будут продолжены в области видимости родительской функции:

```javascript
var a = 1234;

(function(){
    var b = 4321;
    (function() {
        var c = 1111;
        alert((a+b)/c); // => 5
    })();
})();
```

*   чудо-переменная «this» всегда указывает на текущий объект, вызывающий функцию (поскольку по умолчанию все переменные и функции попадают в «window», то «this == window»):

```javascript
var a = 1234;

function myLog() {
    alert(this);   // => window
    alert(this.a); // => 1234
}
```

*   контекст «this» можно изменить, используя функции «bind()», «call()», и «apply()»

_Всё, что касается «window», относится лишь к браузерам, но поскольку книга о jQuery, то иное поведение я и не рассматриваю, но вот так прозрачно намекаю, что оно есть ;)_

### Замыкания {#javascript-closures}

Изучив замыкания, можно понять много магии в JavaScript’e. Приведу пример кода с пояснениями:

```javascript
var a = 1234;

var myFunc = function(){
    var b = 4321;
    var c = 1111;
    return function() {
        return ((a+b)/c);
    };
};

var anotherFunc = myFunc(); // myFunc возвращает анонимную функцию
                            // с «замкнутыми» значениями c и b

alert(anotherFunc()); // => 5
```

Что же тут происходит? Функция, объявленная внутри другой функции, имеет доступ к переменным родительской функции. Повтыкайте в код, пока вас не осенит, о чём я тут толкую.

Хорошая задачка, которая в полной мере даёт понимание сути проблемы: «Армия функций» [https://learn.javascript.ru/task/make-army](https://learn.javascript.ru/task/make-army)

Рекомендуемые статьи по теме:

*   «Привязка контекста и карринг: "bind"» - [https://learn.javascript.ru/bind](https://learn.javascript.ru/bind)
*   «Явное указание this: "call", "apply"» - [https://learn.javascript.ru/call-apply](https://learn.javascript.ru/call-apply)
*   «Функции "изнутри", замыкания» - [[http://learn.javascript.ru/closures](http://learn.javascript.ru/closures)
*   «Использование замыканий» - [http://learn.javascript.ru/closures-usage](http://learn.javascript.ru/closures-usage)
*   «Closures: Front to Back» - [http://net.tutsplus.com/tutorials/javascript-ajax/closures-front-to-back/](http://net.tutsplus.com/tutorials/javascript-ajax/closures-front-to-back/)

Вводная по JavaScript затянулась, лучше не поленитесь, и изучите весь учебник от Ильи Кантора — [http://learn.javascript.ru/](http://learn.javascript.ru/).
