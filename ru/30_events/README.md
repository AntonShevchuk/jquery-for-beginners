# 30% События

Прежде чем приступить к прочтению данной главы, стоит определиться, что же из себя представляют события web-страницы. Так вот, события – это любые действия пользователя, будь то ввод данных с клавиатуры, проматывание страницы или передвижения мышки, и, конечно же, «клики».

> А ещё существуют события, создаваемые скриптами, и их обработчики – триггеры и хэндлеры, но о них чуть позже.

jQuery работает практически со всеми событиями в JavaScript, приведу список самых востребованных с небольшими пояснениями:

<table data-header-hidden><thead><tr><th width="210">событие</th><th>описание</th></tr></thead><tbody><tr><td><code>click</code></td><td>клик по элементу;<br>порядок событий: <code>mousedown</code> → <code>mouseup</code> → <code>click</code></td></tr><tr><td><code>dblclick</code></td><td>двойной щелчок мышки</td></tr><tr><td><code>mousedown</code></td><td>нажатие клавиши мыши</td></tr><tr><td><code>mouseup</code></td><td>отжатие клавиши мыши</td></tr><tr><td><code>mousemove</code></td><td>движение курсора</td></tr><tr><td><code>mouseenter</code></td><td>наведение курсора на элемент;<br>не срабатывает при переходе фокуса на дочерние элементы</td></tr><tr><td><code>mouseleave</code></td><td>вывод курсора из элемента;<br>не срабатывает при переходе фокуса на дочерние элементы</td></tr><tr><td><code>mouseover</code></td><td>наведение курсора на элемент</td></tr><tr><td><code>mouseout</code></td><td>вывод курсора из элемента</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/events.mouse.html" %}

***

Идём дальше. Вот ещё десяток событий, по большей части они относятся лишь к элементам форм:

<table data-header-hidden><thead><tr><th width="221">событие</th><th>описание</th></tr></thead><tbody><tr><td><code>focus</code></td><td>получение фокуса на элементе; <br>актуально для <code>input[type=text]</code>, но в современных браузерах работает и с другими элементами</td></tr><tr><td><code>blur</code></td><td>фокус ушёл с элемента;<br>срабатывает при клике по другому элементу на странице или по событию клавиатуры (к примеру переключение по tab'у)</td></tr><tr><td><code>focusin</code></td><td>фокус на элементе; <br>данное событие аналогично <code>focus</code>, но при этом умеет «всплывать»</td></tr><tr><td><code>focusout</code></td><td>фокус ушёл с элемента;<br>данное событие аналогично <code>blur</code>, «всплывает» (подробнее про различия вы можете прочитать в статье «<a href="https://learn.javascript.ru/focus-blur">Фокусировка: focus/blur</a>»)</td></tr></tbody></table>

{% hint style="info" %}
Вплоть до 4-ой версии jQuery, порядок событий был следующим:

* `focusout`
* `blur`
* `focusin`
* `focus`

С jQuery версии 4.0 порядок событий будет соответствовать текущей спецификации [W3C](https://www.w3.org/TR/uievents/#events-focusevent-event-order):

* `blur`
* `focusout`
* `focus`
* `focusin`

Единственным браузером, который имеет иной порядок остаётся IE, но ирония заключается в том, что IE в своё время был единственным браузером, который следовал предыдущей спецификации W3C, а все остальные браузеры использовали свой порядок событий, который вот только в 2023-м году стал стандартом ¯\\\_(ツ)\_/¯
{% endhint %}

<table data-header-hidden><thead><tr><th width="221">событие</th><th>описание</th></tr></thead><tbody><tr><td><code>change</code></td><td>изменение значения элемента (значение при потере фокуса отличается от начального значения при получении фокуса)</td></tr><tr><td><code>keydown</code></td><td>нажатие клавиши на клавиатуре</td></tr><tr><td><code>keypress</code></td><td>удержание клавиши на клавиатуре, последовательность <code>keydown</code> → <code>keypress</code> → <code>keyup</code> (только для буковок-циферок)</td></tr><tr><td><code>keyup</code></td><td>отжатие клавиши на клавиатуре</td></tr><tr><td><code>select</code></td><td>выбор текста для <code>input[type=text]</code> и <code>textarea</code></td></tr><tr><td><code>submit</code></td><td>отправка формы</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/events.form.html" %}

***

Так же стоит упомянуть ещё парочку популярных событий с которыми доводится работать:

<table data-header-hidden><thead><tr><th width="223">событие</th><th>описание</th></tr></thead><tbody><tr><td><code>resize</code></td><td>изменение размеров документа</td></tr><tr><td><code>scroll</code></td><td>прокрутка элемента или окна</td></tr></tbody></table>

{% hint style="info" %}
Нет нужды запоминать все события — вы всегда найдёте их на страницах [MDN от Mozilla](https://developer.mozilla.org/ru/docs/Web/Events).
{% endhint %}
