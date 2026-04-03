---
description: Хоча кого я обманюю, вийшла досить увесиста глава
---

# Трохи про JavaScript

У цей розділ я виніс ту інформацію про JavaScript, яку необхідно знати, щоб у тебе не виникало «дитячих» проблем з використанням jQuery. Якщо в тебе є досвід роботи з JavaScript, то гортай [далі](../10\_go\_on/).

> Вивчати хочеш JavaScript та jQuery? Тоді силу пізнай інструмента справжнього:
>
> * [Developer Tools](https://developer.chrome.com/docs/devtools/) - для Chrome, Safari, Opera та інших webkit-based браузерів
> * [Developer Tools](https://firefox-source-docs.mozilla.org/devtools-user/index.html) - для FireFox
> * [Developer Tools](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/overview) - для Microsoft Edge
>
> Список не повний, але [console](https://developer.chrome.com/docs/devtools/console) там є, застосовувати її треба вміти

## Про форматування <a href="#style-guide" id="style-guide"></a>

Хотів би одразу звернути увагу на форматування JavaScript-коду. Мій досвід мені підказує — найкраще спроєктувати стандарти форматування основної мови розробки на прикладну — JavaScript, а якщо ти хочеш чогось із глобального та загальноприйнятого, то я для тебе вже погуглив:

* [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/) — керівництво від розробників jQuery
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) — бест практікс для JavaScript-розробників
* [Как писать неподдерживаемый код?](https://learn.javascript.ru/ninja-code) — шкідливі поради від Іллі

> На додачу поділюся невеличкою порадою: всі змінні, що містять об'єкт jQuery, найкраще іменувати, починаючи з символу «$». Повір, така невеличка хитрість економить багато часу.&#x20;

## Основи JavaScript

### Змінні <a href="#vars" id="vars"></a>

Перше, з чим ми зіткнемося — це оголошення змінних:

```javascript
var name = "Ivan";
var age = 32;
```

Все просто, оголошуємо змінну, використовуючи ключове слово `var`.&#x20;

{% hint style="info" %}
Можна, звісно ж, і без `var`, але робити я тобі це наполегливо не рекомендую, бо можуть виникнути непередбачені проблеми в коді, про що трохи пізніше розкажу.
{% endhint %}

Так, так, JavaScript належить до мов з динамічною типізацією, і нам немає потреби вказувати тип даних при оголошенні змінних, ти навіть можеш улаштувати «holy war» із цього приводу, але роби це локально, не виходячи за рамки своєї черепної коробки.

На імена змінних накладено два обмеження:

* ім'я може складатися з букв, цифр, і символів «$» та «\_»
* перший символ не повинен бути цифрою

Зверни увагу, регістр букв має значення:

```javascript
var company = "Facebook";
// зовсім інша «компанія»
var Company = "Google";
```

Хочу також тебе познайомити з таким нововведенням ES6 (він же ECMAScript-2015 або ES-2015), як оголошення змінних з використанням конструкції `let`:

```javascript
let company = "Facebook";
```

Зовні, від `var` не сильно відрізняється, зате яка різниця в поведінці:

*   область видимості змінної `let` обмежена блоком `{...}`, на відміну від `var`, яка видна скрізь усередині функції:

    ось так поводиться змінна оголошена за допомогою `var`:

    ```javascript
    var a = 0;
    if (true) {
        var a = 1000;
        alert(a); // 1000
    }
    alert(a);     // 1000
    ```

    а тепер порівняй з поведінкою `let`:

    ```javascript
    let a = 0;
    if (true) {
        let a = 1000;
        alert(a); // 1000
    }
    alert(a);     // 0
    ```
*   змінна `let` видна тільки після оголошення:

    «`var`»:

    ```javascript
    alert(a);  // undefined
    var a = 0;
    ```

    «`let`»:

    ```javascript
    alert(b);  // error: "b" is not defined
    let b = 0;
    ```
*   змінну `let` не можна оголосити повторно:

    «`var`»:

    ```javascript
    var a;
    var a; // ок
    ```

    «`let`»:

    ```javascript
    let b;
    let b; // error: "b" has already been declared
    ```
*   всередині циклу змінна `let` буде оголошена нова для кожної ітерації:

    «`var`»:

    ```javascript
    for (var i = 0; i < 10; i++) { /* … */ }
    alert(i); // 10
    ```

    «`let`»:

    ```javascript
    for (let j = 0; j < 10; j++) { /* … */ }
    alert(j); // error: "j" is not defined
    ```

Ось тобі наступне завдання — вгадай значення `x` та `y` в наступному прикладі:

```javascript
var x;
let y;
for (let y = 0; y < 10; y++) {
    x = y;
}
y = y ? x : 5;
```

<details>

<summary>відповідь</summary>

```javascript
x = 9
y = 5
```

</details>

### Константи <a href="#constants" id="constants"></a>

В JavaScript до ES6 не було констант, але оскільки необхідність в них все ж була і до того, то була негласна домовленість: змінні, набрані у верхньому регістрі через підкреслення, не змінювати:

```javascript
var USER_STATUS_ACTIVE = 1;
var USER_STATUS_BANNED = 2;
```

{% hint style="info" %}
Такі константи необхідні, щоб уникнути появи «magic numbers».&#x20;

Поглянь на наступний код `if(status==2)` — про що тут мова, мало кому буде зрозуміло, а ось код `if(status==USER_STATUS_BANNED)` вже більш інформативний.
{% endhint %}

Якщо ж говорити про еру ES-2015, то тут уже використовуємо `const`, а про решту вже подбає сам JavaScript:

```javascript
const USER_STATUS_ACTIVE = 1;
USER_STATUS_ACTIVE = 2; // error: assignment to constant variable
```

> Так-так, я повторюся, це наочний приклад правильного іменування констант — ім'я однозначно вказує на збережене значення — «статус активного користувача».

Окремо варто зауважити, константа не дозволяє змінювати саму змінну, але якщо ми присвоїмо константі об'єкт або масив, то з ним ти зможеш робити, що душа забажає:

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

Ось і вийшов «читабельний» код, мотай на вус

### Типи даних <a href="#types" id="types"></a>

В JavaScript не так вже й багато типів даних:

*   **number** — ціле або дробове число:

    ```javascript
    let answer = 42
    let pi = 3.1415
    ```

    також існують наступні спеціальні значення:

    * `Infinity` — за межею 1.7976931348623157E+10308 (тобто більше)
    * `-Infinity` — за межею -1.7976931348623157E+10308 (тобто менше)
    *   `NaN (not-a-number)` — результат числової операції, яка завершилася помилкою; для зразка запусти наступний код у консолі:

        ```javascript
        Math.sqrt(-5);
        ```

        але зверни увагу — є тільки один правильний спосіб перевірки результату обчислення на `NaN`:

        ```javascript
        isNaN(NaN);   // true
        ```

        і багато неправильних:

        ```javascript
        NaN == false; // false
        NaN == NaN;   // false
        if (NaN) {
        // цей блок ніколи не виконається
        }
        ```
*   **string** — рядок, береться в лапки:

    ```javascript
    let str = "Hello World!"
    ```

    > В JavaScript немає різниці між подвійними лапками та одинарними, достатньо дотримання їх парності (привіт PHP та Ruby).
*   **boolean** — булеве значення, тобто або «`true`» або «`false`»

    ```javascript
    let result = true
    ```
* **object** — це об'єкти; на них зупинюсь детальніше трохи пізніше…
* **symbol** — тип даних з ES6, слугує для створення унікальних ідентифікаторів (про нього розповіді не буде, ще не в ходу)
*   **null** — спеціальне значення для визначення «порожнечі»

    ```javascript
    let result = null
    ```
*   **undefined** — ще одне спеціальне значення, для «невизначеності», використовується як значення невизначеної або неіснуючої змінної, наприклад, якщо змінна оголошена, але значення їй ще не присвоєно:

    ```javascript
    let a;
    // змінна є, значення немає
    if (typeof a === "undefined") {
        alert("variable is undefined")
    }

    // змінної немає, зовсім
    if (window["b"] === undefined) {
        alert("variable does not exist")
    }
    ```

    > У другому прикладі нас може чекати сюрприз, якщо щось визначить змінну «undefined»; як обійти таку «неприємність», я ще розкажу.

{% hint style="info" %}
Хотів ще акцентувати твою увагу на приведення типів в JavaScript. Цей момент викликає багато питань у початківців розробників, а помилки пов'язані з ним будуть переслідувати тебе перші півроку роботи. Відкрий сторінку підручника про [особливості операторів](https://learn.javascript.ru/javascript-specials#%D0%BE%D1%81%D0%BE%D0%B1%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2), вивчи її, а коли будеш готовий переходь до виконання наступного завдання.
{% endhint %}

Подивись уважно на код, і скажи — яким буде результат обчислення:

```javascript
let x
x = 42 + "x" + 42
x = x/2 || x
```

<details>

<summary>відповідь</summary>

```javascript
x = "42x42"
```

</details>

### Масиви <a href="#arrays" id="arrays"></a>

Масив — це колекція даних з числовими індексами. Дані можуть бути будь-якого типу, як приклад я наведу один з найпростіших варіантів — масив з рядками:

```javascript
//             0       1       2
var users = ["Ivan", "Petr", "Serg"]
```

Нумерація масивів починається з «0», тож для отримання першого елемента тобі знадобиться наступний код:

```javascript
alert(users[0]); // виведе Ivan
```

Розмір масиву зберігається у властивості length:

```javascript
alert(users.length); // виведе 3

users[3] = "Danylo";

alert(users.length); // виведе 4
```

{% hint style="info" %}
Насправді `length` повертає індекс останнього елемента масиву+1, тож не потрапся на наступну «помилку новачка»:
{% endhint %}

```javascript
var a = [];

a[4] = 10;       // оголошуємо одразу п'ятий елемент

alert(a.length); // виведе 5;
```

Для перебору масиву найкраще використовувати цикл `for(;;)`:

```javascript
for (let i = 0; i < users.length; i++) {
    alert(users[i]); // послідовно виведе Ivan, Petr та Serg
}
```

На попередньому прикладі цикл покаже чотири значення `undefined`, що може викликати проблеми, і тільки потім буде працювати зі значенням `10`. Інакше діє схожий, але не рівнозначний цикл `for (in)` (він одразу візьметься за значення `10`). Приклад циклу:

```javascript
for (let i in users) {
    alert(users[i]);
}
```

Для роботи з останнім елементом масиву слід використовувати методи `push()` та `pop()`:

```javascript
users.push("Sidorov"); // додаємо елемент в кінець масиву

let sidorov = users.pop(); // видаляємо і повертаємо останній елемент
```

Для роботи з першим елементом масиву слід використовувати методи `unshift()` та `shift()`:

```javascript
users.unshift("Sidorov"); // додаємо елемент на початок масиву

let sidorov = users.shift(); // видаляємо і повертаємо перший елемент
```

> Останні два методи працюють повільно, бо перебудовують весь масив покроково.

Вправа для закріплення матеріалу:

* як мені отримати користувача на ім'я `Andrey` з масиву `users`?
* яка довжина масиву?

```javascript
var users = new Array;
users.push("Anton");
users.push("Andrey");
users[100] = "Bogdan";
users.push("Boris");
```

<details>

<summary>відповідь</summary>

```javascript
// виведе Andrey
alert(users[1])
// поверне 102
alert(users.length)
```

</details>

### Функції <a href="#functions" id="functions"></a>

З функціями в JavaScript все просто, ось тобі елементарний приклад:

```javascript
function hello() {
    alert("Hello world");
}
```

Просто, поки не заговорити про анонімні функції…

### Анонімні функції <a href="#anonymous-functions" id="anonymous-functions"></a>

В JavaScript можна створити анонімну функцію (тобто функцію без імені), для цього достатньо трохи змінити попередню конструкцію:

```javascript
function() {
    alert("Hello world");
}
```

Оскільки функція — це цілком собі об'єкт, то її можна присвоїти змінній, і (або) передати як параметр в іншу функцію:

```javascript
var myAlert = function(name) {
    alert("Hello, " + name);
}

function helloMike(myFunc) {  // тут функція передається як параметр
    myFunc("Mike");           // а тут ми її викликаємо
}

helloMike(myAlert); // "Hello, Mike"
```

Анонімну функцію можна створити і тут же викликати з необхідними параметрами:

```javascript
(function(name) {
    alert("Hello, " + name);
})("Mike");
```

Це нескладно, скоро ти до них звикнеш, і тобі їх буде бракувати в інших мовах.

### Стрілочні функції

Ах, цей ES6 — він приніс ще більш коротку форму для створення функцій, це так звані «функції-стрілки»:

```javascript
// була проста анонімна функція
var inc = function (x) {
    return x+1;
}

// став запис в один рядок
var inc = x => x+1;

// була функція з декількома аргументами
var sum = function (a, b) {
    return a+b;
}

// став запис в один рядок
var sum = (a, b) => a+b;
```

Цей запис, звісно ж, зручний, але не варто захоплюватись, бо якщо перестаратись, то твій код стане нечитабельним, і в підсумку ти можеш почути багато несхвальних висловлювань на свою адресу.

{% hint style="info" %}
Функції-стрілки мають ще деякі особливості, але це все ж не [підручник з JavaScript](https://learn.javascript.ru/arrow-functions-basics) ;)
{% endhint %}

Розв'яжи просту FizzBuzz-задачку:

> Необхідно написати функцію, яка буде як аргумент приймати число, якщо число кратне 3, то функція повинна виводити `Fizz`, якщо число кратне 5, то `Buzz`, якщо число кратне і 3 і 5, то функція повинна повернути `FizzBuzz`, інакше повертати саме число

<details>

<summary>відповідь</summary>

<pre class="language-javascript"><code class="lang-javascript">/**
 * Ha-ha, classic! 🚬🐱
 */
<strong>function fizzBuzz(n) {
</strong>  let output = '';
  if (n % 3 === 0) output += 'Fizz';
  if (n % 5 === 0) output += 'Buzz';
  console.log(output || n);
}

for (let i = 1; i &#x3C;= 42; i++) {
  fizzBuzz(i)
}
</code></pre>

</details>

### Об'єкти <a href="#objects" id="objects"></a>

На об'єкти в JavaScript покладено дві ролі:

* сховище даних
* функціонал об'єкта

Перше призначення можна описати наступним кодом:

```javascript
let user = {
    name: "Ivan",
    age: 32
};

alert(user.name) // Ivan
alert(user.age)  // 32
```

Це, фактично, реалізація key-value сховища, або хешу, або асоціативного масиву, або…. Ну, ти зрозумів, назв багато. В JavaScript це об'єкт, а запис вище — це JSON або JavaScript Object Notation (хоч і з невеликими застереженнями).

Для перебору такого сховища можна використовувати цикл `for(.. in ..)`:

```javascript
for (let prop in user) {
  alert(prop + "=" + user[prop]); // виведе name=Ivan
                                  // потім age=32
}
```

### Класи

З класами, конструкторами тощо в JavaScript складніше буде, хоча для розуміння не так вже й багато треба. Запам'ятовуй: будь-яка функція, викликана з використанням ключового слова `new`, повертає нам об'єкт, а сама стає конструктором цього об'єкта:

```javascript
const USER_STATUS_ACTIVE = 'active';

function User(name) {
    this.name = name;
    this.status = USER_STATUS_ACTIVE;
}

const me = new User("Anton");
```

Поведінка функції `User()` при використанні `new` трохи зміниться:

1. Ця конструкція створить новий, порожній об'єкт
2. Ключове слово `this` отримає посилання на цей об'єкт
3. Функція виконається і, можливо, змінить об'єкт через `this` (як у прикладі вище)
4. Функція поверне `this` (за замовчуванням)

Результатом виконання коду буде наступний об'єкт:

```javascript
{
    "name": "Anton",
    "status": "active"
}
```

В стандарті ES6(ES2015) з'явилась конструкція `class`, що по суті — синтаксичний цукор для JavaScript — спеціально для тих, хто не любить працювати з прототипами (ага, для таких як я).

Якщо переписати код вище на класи, то отримаємо наступний результат:

```javascript
const USER_STATUS_ACTIVE = 'active';

class User {
    constructor(name) {
        this.name = name;
        this.status = USER_STATUS_ACTIVE;
    }
}

const me = new User("Anton");
```

> Знову відправлю [читати підручник з JavaScript](https://learn.javascript.ru/class) :)&#x20;

Проста задачка для закріплення матеріалу:&#x20;

> Напиши клас для кота, і щоб ми могли дізнатися якої він породи, і якого кольору.

<details>

<summary>відповідь</summary>

{% code fullWidth="false" %}
```javascript
// якщо використовувати синтаксис функцій
function Cat(breed, color) {
    this.breed = breed;
    this.color = color;
}

// якщо використовувати синтаксис класів
class Cat {
    constructor(breed, color) {
        this.breed = breed;
        this.color = color;
    }
}

// в будь-якому разі, використовувати його будемо наступним чином
const cat = new Cat("British Shorthair", "silver");

// і результатом буде об'єкт
{
    "breed": "British Shorthair",
    "color": "silver"
}
```
{% endcode %}

</details>

## Область видимості та `this` <a href="#this" id="this"></a>

Для тих, хто тільки починає своє знайомство з JavaScript я розкажу наступні нюанси:

* коли ти оголошуєш змінну або функцію, то вона стає частиною «window»:

```javascript
var a = 1234;

alert(window["a"]); // => 1234

function myLog(message) {
    alert(message); // => 1234
}

window["myLog"](a);
```

* коли шукана змінна не знайдена в поточній області видимості, її пошуки будуть продовжені в області видимості батьківської функції:

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

* чудо-змінна `this` завжди вказує на поточний об'єкт, що викликає функцію (оскільки за замовчуванням всі змінні та функції потрапляють в «window», то «this == window»):

```javascript
var a = 1234;

function myLog() {
    alert(this);   // => window
    alert(this.a); // => 1234
}
```

* контекст `this` можна змінити, використовуючи функції [`bind()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/bind), [`call()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/call), та [`apply()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/apply)

> Все, що стосується «window», стосується лише браузерів, хоча оскільки книга про jQuery, то іншу поведінку я і не розглядаю, але ось так прозоро натякаю, що є альтернативна реальність зі своїм «jQuery», і звуть її там [cheerio](https://cheerio.js.org/) ;).

## Замикання <a href="#closures" id="closures"></a>

Вивчивши замикання, можна зрозуміти багато магії в JavaScript. Наведу приклад коду з поясненнями:

```javascript
var a = 1234;

var myFunc = function(){
    var b = 4321;
    var c = 1111;
    return function() {
        return ((a+b)/c);
    };
};

var anotherFunc = myFunc(); // myFunc повертає анонімну функцію
                            // із «замкнутими» значеннями c та b

alert(anotherFunc()); // => 5
```

Що ж тут відбувається? Функція, оголошена всередині іншої функції, має доступ до змінних батьківської функції. Повтикай в код, поки тебе не осінить, про що я тут товкую.

Гарна задачка, яка повною мірою дає розуміння суті проблеми: «[Армия функций](https://learn.javascript.ru/task/make-army)»

Рекомендовані статті з теми:

* [Привязка контекста и карринг: "bind"](https://learn.javascript.ru/bind)
* [Явное указание this: "call", "apply"](https://learn.javascript.ru/call-apply)
* [Функции "изнутри", замыкания](https://learn.javascript.ru/closures)
* [Использование замыканий](https://learn.javascript.ru/closures-usage)
* [Closures: Front to Back](https://code.tutsplus.com/closures-front-to-back--net-24869t)

Вступна по JavaScript затягнулась, краще не полінуйся, і вивчи весь [підручник від Іллі Кантора](https://learn.javascript.ru/).
