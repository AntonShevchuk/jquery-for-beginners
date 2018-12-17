## Немного о JavaScript {#javascript}

В данный раздел я вынес ту информацию о JavaScript, которую необходимо знать, чтобы у вас не возникало
«детских» проблем с использованием jQuery. Если у вас есть опыт работы с JavaScript, то листайте [далее](/10_podklyuchaem,_nahodim,_gotovim/README.md).

> _Изучать хотите JavaScript и jQuery? Так силу познайте инструмента истинного:_
* [Developer Tools](http://www.html5rocks.com/en/tutorials/developertools/part1/) - для Chrome, Safari и других webkit-based браузеров
* [Developer Tools](https://developer.mozilla.org/ru/docs/Tools) - для FireFox
* [Developer Tools](https://developer.microsoft.com/en-us/microsoft-edge/platform/documentation/f12-devtools-guide/) - для Microsoft Edge
* [DragonFly](http://www.opera.com/dragonfly/) - для Opera

> _Список не полон, но [console](https://developers.google.com/chrome-developer-tools/docs/console) там есть, применять её надо уметь_

### О форматировании {#style-guide}

Хотел бы сразу обратить внимание на форматирование JavaScript-кода. Мой опыт мне подсказывает — лучше всего спроецировать стандарты форматирования основного языка разработки на прикладной — JavaScript, а если вы хотите чего-нить из глобального и общепринятого, то я для вас уже погуглил:
* [jQuery Core Style Guidelines](http://contribute.jquery.org/style-guide/js/)
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
* [Как писать неподдерживаемый код?](http://learn.javascript.ru/write-unmain-code) – вредные советы от Ильи

> _В довесок поделюсь небольшим советом: все переменные, содержащие объект jQuery, лучше всего именовать, начиная с символа «$». Поверьте, такая небольшая хитрость экономит много времени._
> _И ещё — в конце каждой строки я ставлю точку с запятой, сам JavaScript этого не требует, но и лишним не будет._

### Основы JavaScript

#### Переменные {#vars}

Первое, с чем мы столкнёмся – это объявление переменных:

```javascript
var name = "Ivan";
var age = 32;
```

Всё просто, объявляем переменную, используя ключевое слово «var». Попробуйте и вы создать переменную `x` и присвойте ей значение 10:

{% exercise %}
{% initial %}

{% solution %}
var x = 10;
{% validation %}
assert(x == 10);
{% endexercise %}

_Можно, конечно же, и без «var», но делать я вам это настоятельно не рекомендую, т.к. могут возникнуть непредвиденные проблемы в коде, о чём чуть позже расскажу._

Да, да, JavaScript относится к языкам с динамической типизацией, и нам нет нужды указывать тип данных при объявлении переменных, вы даже можете устроить «holy war» по этому поводу, но делайте это локально, не выходя за рамки своей черепной коробки.

На имена переменных наложено два ограничения:

* имя может состоять из букв, цифр, и символов «$» и «_»
* первый символ не должен быть цифрой

Учтите, регистр букв имеет значение:

```javascript
var company = "Facebook";
// совсем другая «компания»
var Company = "Google";
```

Хочу также вас познакомить с таким нововведением ECMAScript-2015 (в дальнейшем ES-2015), как объявление переменных с использованием конструкции «let»:

```javascript
let company = "Facebook";
```

Внешне, от «var» не сильно отличается, зато какая разница в поведении:
*   область видимости переменной «let» ограничена блоком `{...}`, в отличие от «var», которая видна везде внутри функции:

  вот так ведёт себя переменная объявленная с помощью «var»:
  
  ```javascript
var a = 0;
if (true) {
        var a = 1000;
        alert(a); // 1000
}
alert(a);     // 1000
```

  а теперь сравните с поведением «let»:
  ```javascript
let a = 0;
if (true) {
        let a = 1000;
        alert(a); // 1000
}
alert(a);     // 0
```

*   переменная «let» видна только после объявления:

  «var»:
  ```javascript
alert(a);  // undefined
var a = 0;
```

  «let»:
  ```javascript
alert(b);  // error: "b" is not defined
let b = 0;
```

*   переменную «let» нельзя объявить повторно:

  «var»:
  ```javascript
var a;
var a; // ок
  ```

  «let»:
  ```javascript
let b;
let b; // error: "b" has already been declared
```

*   внутри цикла переменная «let» будет объявлена новая для каждой итерации:

  «var»:
  ```javascript
for (var i = 0; i < 10; i++) { /* … */ }
alert(i); // 10
  ```

  «let»:
  ```javascript
for (let j = 0; j < 10; j++) { /* … */ }
alert(j); // error: "j" is not defined
```

Вот вам следующее задание - угадайте значение `x` и `y` в следующем примере:
```javascript
var x;
let y;
for (let y = 0; y <= 10; y++) {
    x = y;
}
y = y ? x : 5;
```
{% exercise %}
{% initial %}
x = 
y = 
{% solution %}
x = 10
y = 5
{% validation %}
assert(x == 10);
assert(y == 5);
{% context %}
var x;
var y;
{% endexercise %}

#### Константы {#constants}

В JavaScript до ES-2015 не было констант, но поскольку необходимость в них всё же была и до того, то была негласная договорённость: переменные, набранные в верхнем регистре через подчёркивание, не изменять:

```javascript
var USER_STATUS_ACTIVE = 1;
var USER_STATUS_BANNED = 2;
```

> _Константы необходимы, чтобы избежать появления «magic numbers». Взгляните на следующий код «if(status==2)» — о чём тут речь, мало кому будет понятно, а вот код «if(status==USER_STATUS_BANNED)» уже более информативный._

Если же говорить об эре ES-2015, то тут уже используем «const», а об остальном уже позаботится сам JavaScript:

```javascript
const USER_STATUS_ACTIVE = 1;
USER_STATUS_ACTIVE = 2; // error: assignment to constant variable
```

> _Да-да, я повторюсь, это наглядный пример правильного именования констант — имя однозначно указывает на хранимое значение — «статус активного пользователя»._

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

#### Типы данных {#types}

В JavaScript не так уж и много типов данных:

* **number** - целое или дробное число:
```javascript
var answer = 42;
var pi = 3.1415;
```

  также существуют следующие специальные значения:
  * `Infinity` — за гранью 1.7976931348623157E+10308 (т.е. больше)
  * `-Infinity` — за гранью -1.7976931348623157E+10308 (т.е. меньше)
  * `NaN (not-a-number)` — результат числовой операции, которая завершилась ошибкой; для образца запустите следующий код в консоли:
  ```javascript
Math.sqrt(-5);
```
  но учтите – есть только один правильный способ проверки результата вычисления на `NaN`:
  ```javascript
isNaN(NaN);   // true
```
  и много неправильных:
  ```javascript
NaN == false; // false
NaN == NaN;   // false
if (NaN) {
    // данный блок никогда не выполнится
}
```

* **string** — строка, заключается в кавычки:
```javascript
var str = "Hello World!";
```

  > _В JavaScript нет разницы между двойными кавычками и одинарными, достаточно соблюдения их парности (привет PHP и Ruby)._

* **boolean** — булево значение, т.е. или «true» или «false»
```javascript
var result = true;
```

* **object** — это объекты; на них остановлюсь подробнее чуть позже…
* **symbol** — тип данных из ES-2015, служит для создания уникальных идентификаторов (о нём рассказа не будет, не в ходу ещё)
* **null** — специальное значение для определения «пустоты»
 ```javascript
var result = null;
```

* **undefined** — ещё одно специальное значение, для «неопределенности», используется как значение неопределённой или несуществующей переменной, например, если переменная объявлена, но значение ей ещё не присвоено:
```javascript
  var a;
  // переменная есть, значения нет
  if (typeof a == "undefined") {
      alert("variable is undefined")
  }
   
  // переменной нет, совсем
  if (window["b"] == undefined) {
      alert("variable does not exist");
  }
```

  > _Во втором примере нас может ожидать сюрприз, если что-то определит переменную «undefined»; как обойти такую «неприятность», я ещё расскажу._

Хотел ещё акцентировать ваше внимание на приведение типов в JavaScript. Этот момент вызывает много вопросов у начинающих разработчиков, а ошибки связанные с ним будут преследовать вас первые полгода работы. Откройте страничку учебника об [особенностях операторов](https://learn.javascript.ru/javascript-specials#%D0%BE%D1%81%D0%BE%D0%B1%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2), изучите её, а когда будете готовы переходите к выполнению следующего задания.

Посмотрите внимательно на код, и скажите - каков будет результат вычисления:

```javascript
let x;
x = 42 + "x" + 42;
x = x/2 || x;
```
{% exercise %}
{% initial %}
x = 
{% solution %}
x = "42x42"
{% validation %}
assert(x == "42x42");
{% context %}
var x;
{% endexercise %}

#### Массивы {#arrays}

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
> _В действительности «length» возвращает индекс последнего элемента массива+1, так что не попадитесь:_

```javascript
var a = [];

a[4] = 10;       // объявляем сразу пятый элемент

alert(a.length); // выведет 5;
```

Для перебора массива лучше всего использовать цикл «for(;;)»:

```javascript
for (let i = 0; i < users.length; i++) {
    alert(users[i]); // последовательно выведет Ivan, Petr и Serg
}
```

На предыдущем примере цикл покажет четыре значения undefined, что может вызвать проблемы, и только затем будет работать со значением 10. Иначе действует похожий, но не равнозначный цикл for (in) (он сразу возьмётся за значение 10). Образец цикла:

```javascript
for (let i in users) {
    alert(users[i]);
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

> _Последние два метода работают медленно, т.к. перестраивают весь массив пошагово_.

Упражнение для закрепления материала:
* как мне получить пользователя по имени `Andrey` из массива `users`?
* какова длина массива?
 
```javascript
var users = new Array;
users.push("Anton");
users.push("Andrey");
users[100] = "Bogdan";
users.push("Boris");
```

{% exercise %}
{% initial %}
Andrey = 
length = 
{% solution %}
Andrey = users[1];
length = 102;
{% validation %}
assert(Andrey == users[1]);
assert(length == 102);
{% context %}
var length;
var Andrey;
var users = new Array;
users.push("Anton");
users.push("Andrey");
users[100] = "Bogdan";
users.push("Boris");
{% endexercise %}


#### Функции {#functions}

С функциями в JavaScript всё просто, вот вам элементарный пример:

```javascript
function hello() {
    alert("Hello world");
}
```

Просто, пока не заговорить об анонимных функциях…

#### Анонимные функции {#anonymous-functions}

В JavaScript можно создать анонимную функцию (т.е. функцию без имени), для этого достаточно слегка изменить предыдущую конструкцию:

```javascript
function() {
    alert("Hello world");
}
```

Так как функция — это вполне себе объект, то её можно присвоить переменной, и (или) передать в качестве параметра в другую функцию:

```javascript
var myAlert = function(name) {
    alert("Hello, " + name);
}

function helloMike(myFunc) {  // тут функция передаётся как параметр
    myFunc("Mike");           // а тут мы её вызываем
}

helloMike(myAlert); // "Hello, Mike"
```

Анонимную функцию можно создать и тут же вызвать с необходимыми параметрами:

```javascript
(function(name) {
    alert("Hello, " + name);
})("Mike");
```

Это несложно, скоро вы к ним привыкнете, и вам их будет недоставать в других языках.

Ах, этот ES-2015 – он принёс нам сокращенную запись для анонимных функций — «функции-стрелки»:

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

Данная запись конечно же, удобная, но не стоит увлекаться, так как если перестараться, то ваш код станет нечитаем, и по итогу вы можете услышать много нелестных выражений в свой адрес.

Решите простую FizzBuzz-задачку:

> _Необходимо написать функцию, которая будет в качестве аргумента принимать число, если число кратно 3, то функция должна выводить `Fizz`, если число кратно 5, то `Buzz`, если число кратно и 3 и 5, то функция должна вернуть `FizzBuzz`_

{% exercise %}
{% initial %}
function fizzBuzz(i) {
    
}
{% solution %}
function fizzBuzz(i) {
    if (i % 15 == 0)
        return "FizzBuzz";
    else if (i % 3 == 0)
        return "Fizz";
    else if (i % 5 == 0)
        return "Buzz";
    else
        return i;
}
{% validation %}
assert(fizzBuzz(1) == 1);
assert(fizzBuzz(2) == 2);
assert(fizzBuzz(3) == "Fizz");
assert(fizzBuzz(5) == "Buzz");
assert(fizzBuzz(15) == "FizzBuzz");
{% context %}

{% endexercise %}

#### Объекты {#objects}

На объекты в JavaScript возложено две роли:

* хранилище данных
* функционал объекта

Первое предназначение можно описать следующим кодом:

```javascript
var user = {
    name: "Ivan",
    age: 32
};

alert(user.name); // Ivan
alert(user.age); // 32
```

Это, фактически, реализация key-value хранилища, или хэша, или ассоциативного массива, или …. Ну, вы поняли, названий много. В JavaScript это объект, а запись выше – это JSON или JavaScript Object Notation (хоть и с небольшими оговорками).

Для перебора такого хранилища можно использовать цикл «for(.. in ..)»:

```javascript
for (let prop in user) {
    alert(prop + "=" + user[prop]); // выведет name=Ivan
                                    // затем age=32
}
```

С объектами, конструкторами и т.д. в JavaScript посложнее будет, хотя для понимания не так уж много и надо. Запоминайте: любая функция, вызванная с использованием ключевого слова «new», возвращает нам объект, а сама становится конструктором данного объекта:

```javascript
function User(name) {
    this.name = name;
    this.status = USER_STATUS_ACTIVE;
}

var me = new User("Anton");
```

Поведение функции «User()» при использовании «new» слегка изменится:

1. Данная конструкция создаст новый, пустой объект
2. Ключевое слово «this» получит ссылку на этот объект
3. Функция выполнится и, возможно, изменит объект через «this» (как в примере выше)
4. Функция вернёт «this» (по умолчанию)

Результатом выполнения кода будет следующий объект:

```json
{
    "name": "Anton",
    "status": 1
}
```

> _Опять отправлю [читать про ES-2015](https://learn.javascript.ru/es-class), в данном стандарте появилась конструкция «class», что по сути — синтаксический сахар для JavaScript — специально для тех, кто любит C-подобные языки программирования._

Простая задачка - нам нужен кот, и чтобы мы могли узнать какой он породы, и какого цвета: 

{% exercise %}
{% initial %}
function Cat(breed, color) {
    
}

var cat = new Cat("British Shorthair", "silver");
cat.breed;
cat.color;
{% solution %}
function Cat(breed, color) {
    this.breed = breed;
    this.color = color;
}

var cat = new Cat("British Shorthair", "silver");
cat.breed;
cat.color;
{% validation %}
assert(cat.breed == "British Shorthair"); 
assert(cat.color == "silver");
{% context %}
{% endexercise %}

### Область видимости и чудо this {#this}

Для тех, кто только начинает своё знакомство с JavaScript я расскажу следующие нюансы:

* когда вы объявляете переменную или функцию, то она становится частью «window»:

```javascript
var a = 1234;

alert(window["a"]); // => 1234

function myLog(message) {
    alert(message); // => 1234
}

window["myLog"](a);
```

* когда искомая переменная не найдена в текущей области видимости, её поиски будут продолжены в области видимости родительской функции:

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

* чудо-переменная «this» всегда указывает на текущий объект, вызывающий функцию (поскольку по умолчанию все переменные и функции попадают в «window», то «this == window»):

```javascript
var a = 1234;

function myLog() {
    alert(this);   // => window
    alert(this.a); // => 1234
}
```

* контекст «this» можно изменить, используя функции «[bind()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)», «[call()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call)», и «[apply()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)»

> _Всё, что касается «window», относится лишь к браузерам, хотя поскольку книга о jQuery, то иное поведение я и не рассматриваю, но вот так прозрачно намекаю, что есть альтернативная реальность со своим «jQuery», и звать её там [cheerio](https://www.npmjs.com/package/cheerio) ;)._

### Замыкания {#closures}

Изучив замыкания, можно понять много магии в JavaScript. Приведу пример кода с пояснениями:

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

Хорошая задачка, которая в полной мере даёт понимание сути проблемы: «[Армия функций](https://learn.javascript.ru/task/make-army)»

Рекомендуемые статьи по теме:

* [Привязка контекста и карринг: "bind"](https://learn.javascript.ru/bind)
* [Явное указание this: "call", "apply"](https://learn.javascript.ru/call-apply)
* [Функции "изнутри", замыкания](http://learn.javascript.ru/closures)
* [Использование замыканий](http://learn.javascript.ru/closures-usage)
* [Closures: Front to Back](http://net.tutsplus.com/tutorials/javascript-ajax/closures-front-to-back/)

Вводная по JavaScript затянулась, лучше не поленитесь, и изучите весь [учебник от Ильи Кантора](http://learn.javascript.ru/).
