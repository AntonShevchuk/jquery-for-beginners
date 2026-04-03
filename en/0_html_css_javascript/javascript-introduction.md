---
description: Although who am I kidding, this turned out to be quite a hefty chapter
---

# A Bit About JavaScript

In this section, I've put together the JavaScript knowledge you need to avoid "beginner" problems when using jQuery. If you already have experience with JavaScript, feel free to skip [ahead](../10\_go\_on/).

> Want to learn JavaScript and jQuery? Then first master the power of the true instrument:
>
> * [Developer Tools](https://developer.chrome.com/docs/devtools/) - for Chrome, Safari, Opera and other webkit-based browsers
> * [Developer Tools](https://firefox-source-docs.mozilla.org/devtools-user/index.html) - for FireFox
> * [Developer Tools](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/overview) - for Microsoft Edge
>
> The list isn't complete, but the [console](https://developer.chrome.com/docs/devtools/console) is there, and you need to know how to use it

## On Formatting <a href="#style-guide" id="style-guide"></a>

I'd like to immediately draw your attention to JavaScript code formatting. My experience tells me -- the best approach is to project the formatting standards of your main development language onto JavaScript. And if you want something global and widely accepted, I've already googled it for you:

* [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/) -- guidelines from the jQuery developers
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) -- best practices for JavaScript developers
* [How to write unmaintainable code?](https://learn.javascript.ru/ninja-code) -- bad advice from Ilya

> As a bonus, here's a small tip: all variables containing a jQuery object are best named starting with the "$" symbol. Trust me, this little trick saves a lot of time.&#x20;

## JavaScript Basics

### Variables <a href="#vars" id="vars"></a>

The first thing we'll encounter is variable declaration:

```javascript
var name = "Ivan";
var age = 32;
```

Pretty simple -- we declare a variable using the `var` keyword.&#x20;

{% hint style="info" %}
You can skip the `var`, of course, but I strongly advise against it, because unexpected problems may arise in your code, which I'll tell you about a bit later.
{% endhint %}

Yes, yes, JavaScript is a dynamically typed language, and we don't need to specify the data type when declaring variables. You can even start a "holy war" over this, but please keep it inside your own head.

Two restrictions apply to variable names:

* the name can consist of letters, digits, and the "$" and "\_" symbols
* the first character must not be a digit

Keep in mind, letter case matters:

```javascript
var company = "Facebook";
// a completely different "company"
var Company = "Google";
```

I'd also like to introduce you to an ES6 (a.k.a. ECMAScript-2015 or ES-2015) feature -- declaring variables using the `let` construct:

```javascript
let company = "Facebook";
```

On the surface, it doesn't look much different from `var`, but what a difference in behavior:

*   the scope of a `let` variable is limited to the block `{...}`, unlike `var`, which is visible everywhere within the function:

    here's how a variable declared with `var` behaves:

    ```javascript
    var a = 0;
    if (true) {
        var a = 1000;
        alert(a); // 1000
    }
    alert(a);     // 1000
    ```

    now compare that with `let`:

    ```javascript
    let a = 0;
    if (true) {
        let a = 1000;
        alert(a); // 1000
    }
    alert(a);     // 0
    ```
*   a `let` variable is only visible after its declaration:

    "`var`":

    ```javascript
    alert(a);  // undefined
    var a = 0;
    ```

    "`let`":

    ```javascript
    alert(b);  // error: "b" is not defined
    let b = 0;
    ```
*   a `let` variable cannot be re-declared:

    "`var`":

    ```javascript
    var a;
    var a; // ok
    ```

    "`let`":

    ```javascript
    let b;
    let b; // error: "b" has already been declared
    ```
*   inside a loop, `let` creates a new variable for each iteration:

    "`var`":

    ```javascript
    for (var i = 0; i < 10; i++) { /* ... */ }
    alert(i); // 10
    ```

    "`let`":

    ```javascript
    for (let j = 0; j < 10; j++) { /* ... */ }
    alert(j); // error: "j" is not defined
    ```

Here's your next challenge -- guess the values of `x` and `y` in the following example:

```javascript
var x;
let y;
for (let y = 0; y < 10; y++) {
    x = y;
}
y = y ? x : 5;
```

<details>

<summary>answer</summary>

```javascript
x = 9
y = 5
```

</details>

### Constants <a href="#constants" id="constants"></a>

JavaScript had no constants before ES6, but since the need for them existed, there was an unspoken agreement: variables written in UPPER_CASE with underscores should not be changed:

```javascript
var USER_STATUS_ACTIVE = 1;
var USER_STATUS_BANNED = 2;
```

{% hint style="info" %}
Such constants are needed to avoid "magic numbers".&#x20;

Look at this code `if(status==2)` -- what it's about is hardly clear to anyone, but `if(status==USER_STATUS_BANNED)` is already much more informative.
{% endhint %}

In the ES-2015 era, we use `const`, and JavaScript takes care of the rest:

```javascript
const USER_STATUS_ACTIVE = 1;
USER_STATUS_ACTIVE = 2; // error: assignment to constant variable
```

> Yeah-yeah, I'll repeat myself -- this is a perfect example of proper constant naming. The name clearly indicates what's stored -- "active user status".

It's worth noting separately that a constant doesn't allow you to change the variable itself, but if you assign an object or array to a constant, you can do whatever your heart desires with it:

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

And there you go -- "readable" code. Take note.

### Data Types <a href="#types" id="types"></a>

JavaScript doesn't have all that many data types:

*   **number** -- integer or floating-point:

    ```javascript
    let answer = 42
    let pi = 3.1415
    ```

    there are also the following special values:

    * `Infinity` -- beyond 1.7976931348623157E+10308 (i.e. bigger)
    * `-Infinity` -- beyond -1.7976931348623157E+10308 (i.e. smaller)
    *   `NaN (not-a-number)` -- the result of a numeric operation that failed; for example, try running this code in the console:

        ```javascript
        Math.sqrt(-5);
        ```

        but keep in mind -- there's only one correct way to check a computation result for `NaN`:

        ```javascript
        isNaN(NaN);   // true
        ```

        and many wrong ones:

        ```javascript
        NaN == false; // false
        NaN == NaN;   // false
        if (NaN) {
        // this block will never execute
        }
        ```
*   **string** -- a string, enclosed in quotes:

    ```javascript
    let str = "Hello World!"
    ```

    > In JavaScript there's no difference between double and single quotes, you just need to make sure they match (hello PHP and Ruby).
*   **boolean** -- a boolean value, i.e. either "`true`" or "`false`"

    ```javascript
    let result = true
    ```
* **object** -- these are objects; I'll cover them in more detail shortly...
* **symbol** -- a data type from ES6, used for creating unique identifiers (I won't be covering it, it's not widely used yet)
*   **null** -- a special value for defining "emptiness"

    ```javascript
    let result = null
    ```
*   **undefined** -- another special value, for "undefinedness", used as the value of an undefined or non-existent variable. For example, if a variable is declared but no value has been assigned to it yet:

    ```javascript
    let a;
    // the variable exists, but has no value
    if (typeof a === "undefined") {
        alert("variable is undefined")
    }

    // the variable doesn't exist at all
    if (window["b"] === undefined) {
        alert("variable does not exist")
    }
    ```

    > In the second example, a surprise may await us if something defines a variable named "undefined"; I'll tell you how to work around this "unpleasantness" later.

{% hint style="info" %}
I also want to draw your attention to type coercion in JavaScript. This is a topic that causes many questions for beginner developers, and bugs related to it will haunt you for the first six months of work. Open the tutorial page about [operator quirks](https://learn.javascript.ru/javascript-specials#%D0%BE%D1%81%D0%BE%D0%B1%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D0%B8-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2), study it, and when you're ready, move on to the next exercise.
{% endhint %}

Take a careful look at the code and tell me -- what will the result be:

```javascript
let x
x = 42 + "x" + 42
x = x/2 || x
```

<details>

<summary>answer</summary>

```javascript
x = "42x42"
```

</details>

### Arrays <a href="#arrays" id="arrays"></a>

An array is a data collection with numeric indices. Data can be of any type; as an example, I'll show one of the simplest cases -- an array of strings:

```javascript
//             0       1       2
var users = ["Ivan", "Petr", "Serg"]
```

Array numbering starts at "0", so to get the first element you'll need this code:

```javascript
alert(users[0]); // will output Ivan
```

The array size is stored in the length property:

```javascript
alert(users.length); // will output 3

users[3] = "Danylo";

alert(users.length); // will output 4
```

{% hint style="info" %}
In reality, `length` returns the index of the last array element + 1, so don't fall for this "beginner mistake":
{% endhint %}

```javascript
var a = [];

a[4] = 10;       // declaring the fifth element right away

alert(a.length); // will output 5;
```

The best way to iterate over an array is with a `for(;;)` loop:

```javascript
for (let i = 0; i < users.length; i++) {
    alert(users[i]); // will sequentially output Ivan, Petr and Serg
}
```

In the previous example, the loop would show four `undefined` values, which could cause problems, and only then work with the value `10`. The similar but not equivalent `for (in)` loop behaves differently (it will go straight to the value `10`). Loop example:

```javascript
for (let i in users) {
    alert(users[i]);
}
```

To work with the last element of an array, use the `push()` and `pop()` methods:

```javascript
users.push("Sidorov"); // add an element to the end of the array

let sidorov = users.pop(); // remove and return the last element
```

To work with the first element of an array, use the `unshift()` and `shift()` methods:

```javascript
users.unshift("Sidorov"); // add an element to the beginning of the array

let sidorov = users.shift(); // remove and return the first element
```

> The last two methods are slow because they rebuild the entire array step by step.

Exercise to reinforce the material:

* how do I get the user named `Andrey` from the `users` array?
* what is the array length?

```javascript
var users = new Array;
users.push("Anton");
users.push("Andrey");
users[100] = "Bogdan";
users.push("Boris");
```

<details>

<summary>answer</summary>

```javascript
// will output Andrey
alert(users[1])
// will return 102
alert(users.length)
```

</details>

### Functions <a href="#functions" id="functions"></a>

Functions in JavaScript are simple. Here's an elementary example:

```javascript
function hello() {
    alert("Hello world");
}
```

Simple, until you start talking about anonymous functions...

### Anonymous Functions <a href="#anonymous-functions" id="anonymous-functions"></a>

In JavaScript you can create an anonymous function (i.e. a function without a name). All you need to do is slightly modify the previous construct:

```javascript
function() {
    alert("Hello world");
}
```

Since a function is a perfectly valid object, you can assign it to a variable and/or pass it as a parameter to another function:

```javascript
var myAlert = function(name) {
    alert("Hello, " + name);
}

function helloMike(myFunc) {  // here the function is passed as a parameter
    myFunc("Mike");           // and here we call it
}

helloMike(myAlert); // "Hello, Mike"
```

You can create an anonymous function and immediately invoke it with the required parameters:

```javascript
(function(name) {
    alert("Hello, " + name);
})("Mike");
```

It's not that hard. Soon you'll get used to them, and you'll miss them in other languages.

### Arrow Functions

Ah, ES6 -- it brought an even more concise form for creating functions, the so-called "arrow functions":

```javascript
// was a simple anonymous function
var inc = function (x) {
    return x+1;
}

// became a one-liner
var inc = x => x+1;

// was a function with multiple arguments
var sum = function (a, b) {
    return a+b;
}

// became a one-liner
var sum = (a, b) => a+b;
```

This syntax is convenient, of course, but don't overdo it -- if you go too far, your code becomes unreadable, and you might end up hearing some rather unflattering words directed at you.

{% hint style="info" %}
Arrow functions have some other peculiarities too, but this isn't a [JavaScript textbook](https://learn.javascript.ru/arrow-functions-basics) after all ;)
{% endhint %}

Solve a simple FizzBuzz problem:

> Write a function that takes a number as an argument. If the number is divisible by 3, the function should output `Fizz`; if divisible by 5, then `Buzz`; if divisible by both 3 and 5, the function should return `FizzBuzz`; otherwise, return the number itself

<details>

<summary>answer</summary>

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

### Objects <a href="#objects" id="objects"></a>

Objects in JavaScript serve two roles:

* data storage
* object functionality

The first purpose can be described with the following code:

```javascript
let user = {
    name: "Ivan",
    age: 32
};

alert(user.name) // Ivan
alert(user.age)  // 32
```

This is essentially an implementation of a key-value store, or a hash, or an associative array, or... Well, you get it, lots of names. In JavaScript, it's an object, and the notation above is JSON or JavaScript Object Notation (with a few caveats, though).

To iterate over such a store, you can use the `for(.. in ..)` loop:

```javascript
for (let prop in user) {
  alert(prop + "=" + user[prop]); // will output name=Ivan
                                  // then age=32
}
```

### Classes

Classes, constructors, and all that in JavaScript are a bit more complex, although to understand them you don't need that much. Remember this: any function called with the `new` keyword returns an object, and the function itself becomes the constructor of that object:

```javascript
const USER_STATUS_ACTIVE = 'active';

function User(name) {
    this.name = name;
    this.status = USER_STATUS_ACTIVE;
}

const me = new User("Anton");
```

The behavior of the `User()` function when using `new` changes slightly:

1. This construct will create a new, empty object
2. The `this` keyword will get a reference to this object
3. The function will execute and possibly modify the object via `this` (as in the example above)
4. The function will return `this` (by default)

The result of executing the code will be the following object:

```javascript
{
    "name": "Anton",
    "status": "active"
}
```

The ES6 (ES2015) standard introduced the `class` construct, which is essentially syntactic sugar for JavaScript -- specifically for those who don't like working with prototypes (yep, people like me).

If we rewrite the code above using classes, we get the following:

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

> Off you go again to [read the JavaScript textbook](https://learn.javascript.ru/class) :)&#x20;

A simple task to reinforce the material:&#x20;

> Write a class for a cat, so we can find out its breed and color.

<details>

<summary>answer</summary>

{% code fullWidth="false" %}
```javascript
// using function syntax
function Cat(breed, color) {
    this.breed = breed;
    this.color = color;
}

// using class syntax
class Cat {
    constructor(breed, color) {
        this.breed = breed;
        this.color = color;
    }
}

// either way, we'd use it like this
const cat = new Cat("British Shorthair", "silver");

// and the result will be an object
{
    "breed": "British Shorthair",
    "color": "silver"
}
```
{% endcode %}

</details>

## Scope and `this` <a href="#this" id="this"></a>

For those just starting their acquaintance with JavaScript, here are a few nuances:

* when you declare a variable or function, it becomes part of "window":

```javascript
var a = 1234;

alert(window["a"]); // => 1234

function myLog(message) {
    alert(message); // => 1234
}

window["myLog"](a);
```

* when the desired variable is not found in the current scope, the search continues in the parent function's scope:

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

* the magical `this` variable always points to the current object calling the function (since by default all variables and functions end up in "window", then "this == window"):

```javascript
var a = 1234;

function myLog() {
    alert(this);   // => window
    alert(this.a); // => 1234
}
```

* the `this` context can be changed using the [`bind()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/bind), [`call()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/call), and [`apply()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/apply) functions

> Everything related to "window" only applies to browsers. And since this book is about jQuery, I'm not covering any other behavior. But I'm dropping a not-so-subtle hint that there's an alternative reality with its own "jQuery", and it goes by the name [cheerio](https://cheerio.js.org/) ;).

## Closures <a href="#closures" id="closures"></a>

Once you understand closures, a lot of JavaScript magic will make sense. Here's a code example with explanations:

```javascript
var a = 1234;

var myFunc = function(){
    var b = 4321;
    var c = 1111;
    return function() {
        return ((a+b)/c);
    };
};

var anotherFunc = myFunc(); // myFunc returns an anonymous function
                            // with "closed over" values of c and b

alert(anotherFunc()); // => 5
```

What's happening here? A function declared inside another function has access to the parent function's variables. Stare at the code until it clicks.

A great exercise that fully illustrates the essence of the problem: "[An Army of Functions](https://learn.javascript.ru/task/make-army)"

Recommended articles on the topic:

* [Context binding and currying: "bind"](https://learn.javascript.ru/bind)
* [Explicit this: "call", "apply"](https://learn.javascript.ru/call-apply)
* [Functions "from the inside", closures](https://learn.javascript.ru/closures)
* [Using closures](https://learn.javascript.ru/closures-usage)
* [Closures: Front to Back](https://code.tutsplus.com/closures-front-to-back--net-24869t)

This JavaScript intro has dragged on. You'd better not be lazy and study the entire [textbook by Ilya Kantor](https://learn.javascript.ru/).
