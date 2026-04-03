# Animate

> Інформація в цьому розділі актуальна для jQuery версії 1.8 і вище. Якщо вас зацікавлять можливості розширення для старіших версій, читайте мою статтю «[Пишем плагины анимации](https://anton.shevchuk.name/javascript/jquery-for-beginners-write-animation-plugins/)».

Для початку затравка – метод [`animate()`](https://api.jquery.com/animate/) маніпулює об'єктом `jQuery.Animation`, який передбачає наступні точки для розширення функціоналу:

* `jQuery.Tween.propHooks`
* `jQuery.Animation.preFilter`
* `jQuery.Animation.tweener`

Почну розповідь з `jQuery.Tween.propHooks`, оскільки вже є плагіни, в код яких можна зазирнути :) Для більшої наочності я візьму досить тривіальну задачу – змусимо плавно змінити колір шрифту для заданого набору елементів:

```javascript
$('p').animate({color:'#ff0000'});
```

Наведений вище код не дасть жодного ефекту, оскільки «з коробки» бібліотека властивість `color` не анімує, але це можна виправити – треба лише прокачати `jQuery.Tween.propHooks`:

```javascript
$.Tween.propHooks.color = {
    get: function(tween) {
        return tween.elem.style.color;
    },
    set: function(tween) {
        tween.easing;  // поточний easing
        tween.elem;    // досліджуваний елемент
        tween.options; // налаштування анімації
        tween.pos;     // поточний прогрес
        tween.prop;    // властивість, яку змінюємо
        tween.start;   // початкові значення
        tween.now;     // поточне значення
        tween.end;     // бажані результуючі значення
        tween.unit;    // одиниці виміру
    }
};
```

Цей код ще не працює, це наші цеглинки, з їхньою допомогою будемо будувати нашу анімацію. Перед роботою варто зазирнути всередину кожної з наведених властивостей:

```javascript
console.log(tween);
```

```
>>>
easing: "swing"
elem: HTMLParagraphElement
end: "#ff0000"
now: "NaNrgb(0, 0, 0)"
options: Object
complete: function (){}
duration: 1000
old: false
queue: "fx"
specialEasing: Object
pos: 1
prop: "color"
start: "rgb(0, 0, 0)"
unit: "px"
```

У консолі в нас буде дуже багато даних, оскільки наведений метод викликається N разів залежно від тривалості анімації, при цьому «tween.pos» поступово нарощує своє значення з 0 до 1. За замовчуванням нарощування відбувається лінійно, якщо треба якось інакше, то варто подивитися на easing плагін або дочитати розділ до кінця (про це я вже згадував у главі [Анімація](../40_animation/README.md)).

Навіть при такому розкладі ми вже можемо змінювати вибраний елемент (шляхом маніпуляцій над `tween.elem`), але є зручніший спосіб – можна встановити властивість «`run`» об'єкта `tween`:

```javascript
$.Tween.propHooks.color = {
    set: function(tween) {
        // тут буде ініціалізація
        tween.run = function(progress) {
            // тут код, що відповідає за зміну властивостей елемента
        }
    }
};
```

Код, що вийшов, працюватиме наступним чином:

1. одноразово буде викликано функцію `set()`
2. функція `run()` буде викликана N разів, при цьому `progress` поводитиметься аналогічно `tween.pos`

Тепер, повертаючись до початкової задачі зі зміною кольору, можна наворотити такий код:

```javascript
$.Tween.propHooks.color = {
    set: function(tween) {
        // приводимо початкове та кінцеве значення до єдиного формату
        // #FF0000 == [255,0,0]
        tween.start = parseColor(tween.start);
        tween.end = parseColor(tween.end);
        tween.run = function(progress) {
            // обчислюємо проміжне значення
            tween.elem.style['color'] = buildColor(tween.start, tween.end, progress);
        }
    }
};
```

> Код функцій `parseColor()` та `buildColor()` ви знайдете у лістингу на сторінці [color.html](https://anton.shevchuk.name/book/code/color.html).

Результатом стане плавне перетікання вихідного кольору до червоного (`#F00` == `#FF0000` == `(255, 0, 0)`), наживо можна подивитися на сторінці [color.html](https://anton.shevchuk.name/book/code/color.html):

{% embed url="https://anton.shevchuk.name/book/code/color.html" %}

{% hint style="info" %}
У плагіні [jQuery Color](https://github.com/jquery/jquery-color) для вирішення поставленої задачі використали [jQuery.cssHooks](https://api.jquery.com/jQuery.cssHooks/), але ми ж не шукаємо легких шляхів.
{% endhint %}

Ще хотів було розповісти про префільтри анімації, але документації немає, а як використовувати «в житті», я не здогадався, але трішки інформації таки накопав (код можна знайти у функції `Animation`):

```javascript
jQuery.Animation.prefilter(function(element, props, opts) {
    // deferred об'єкт animate
    element; // шуканий елемент
    props;   // налаштування анімації з animate()
    opts;    // опції анімації
    // вимикаємо анімацію при спробі анімувати висоту елемента
    if (props['height'] != undefined) {
        return this;
    }
});
```

Приклад можна помацати на сторінці [animate.prefilter.html](https://anton.shevchuk.name/book/code/animate.prefilter.html):

{% embed url="https://anton.shevchuk.name/book/code/animate.prefilter.html" %}

Про `jQuery.Animation.tweener` також багато не розкажеш, але приклад вдалося зробити трішки цікавішим – наведений код дозволяє анімувати ширину та висоту об'єкта за заданою діагоналлю:

> Обережно, для розуміння того, що відбувається, знадобляться знання геометрії за 8-й клас.

```javascript
// створюємо підтримку нової властивості для анімації – diagonal
jQuery.Animation.tweener( "diagonal", function( property, value ) {
    // створюємо tween об'єкт
    var tween = this.createTween( property, value );
    // проміжні обчислення та дані
    var a = jQuery.css(tween.elem, 'width', true );
    var b = jQuery.css(tween.elem, 'height', true );
    var c = Math.sqrt(a*a + b*b), sinA = a/c, sinB = b/c;
    tween.start = c;
    tween.end = value;
    tween.run = function(progress) {
        // обчислення шуканого значення – нове значення гіпотенузи
        var hyp = this.start + ((this.end - this.start) * progress);
        // безпосередньо зміна властивостей елемента
        tween.elem.style.width = sinA*hyp + tween.unit; // ширина
        tween.elem.style.height = sinB*hyp + tween.unit; // висота
    };
    return tween;
});
```

Приклад роботи [animate.tweener.html](https://anton.shevchuk.name/book/code/animate.tweener.html):

{% embed url="https://anton.shevchuk.name/book/code/animate.tweener.html" %}
