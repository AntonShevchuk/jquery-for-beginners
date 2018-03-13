## Классы {#classes}

Ну, вроде с CSS разобрались, хотя нет — стоит ещё описать манипуляции с классами, тоже из разряда первичных навыков:

`addClass(className)` — добавление класса элементу

`addClass(function(index, currentClass){ return className })` — добавление класса с помощью функции обратного вызова

`hasClass(className)` — проверка на причастность к определённому классу

`removeClass(className)` — удаление класса

`removeClass(function(index, currentClass){ return className })` — удаление класса с помощью функции обратного вызова

`toggleClass(className)` — переключение класса

`toggleClass(className, switch)` — переключение класса по флагу switch

`toggleClass(function(index, currentClass, switch){ return className }, switch)` — переключение класса с помощью функции обратного вызова

> _В приведённых методах в качестве «className» может быть строка, содержащая список классов через пробел._

> _Мне ни разу не приходилось использовать данные методы с функциями обратного вызова, и лишь единожды пригодился флаг «switch», так что не заморачивайтесь всё это запоминать, да и в дальнейшем, цитируя руководство по jQuery, я буду сознательно опускать некоторые «возможности»._

Но хватит заниматься переводом официальной документации, перейдём к наглядным примерам:

<iframe class="jqbook" id="class-example" width="100%" height="320px" border="0" src="../code/class.html"></iframe>

<a class="jqbook" href="#" data-target="#class-example" data-type="append-script">$("#my").addClass('active')</a> - добавляем класс «active»

<a class="jqbook" href="#" data-target="#class-example" data-type="append-script">$("#my").addClass('active notice')</a> - добавляем несколько классов за раз

<a class="jqbook" href="#" data-target="#class-example" data-type="append-script">$("#my").toggleClass('active')</a> - переключаем класс «active»

<a class="jqbook" href="#" data-target="#class-example" data-type="append-script">$("#my").toggleClass('active notice')</a> - переключаем несколько классов

Работает переключение классов следующим образом (это похоже на классовый XOR):
```html
<div id="my" class="active notice"> → <div id="my" class="">

<div id="my" class="active"> → <div id="my" class="notice">

<div id="my" class=""> → <div id="my" class="active notice">
```

<a class="jqbook" href="#" data-target="#class-example" data-type="append-script">$("#my").removeClass('active')</a> - удаляем класс «active»

<a class="jqbook" href="#" data-target="#class-example" data-type="append-script">$("#my").removeClass('active notice')</a> - удаляем несколько классов
