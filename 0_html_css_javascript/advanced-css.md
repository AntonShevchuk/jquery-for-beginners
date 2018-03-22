## CSS. Погружение {#css}

> _Этот раздел будет полезен начинающим верстальщикам и тем, кто захочет сделать чуть больше, нежели вывод окна сообщения по клику._

### О форматировании {#codestyle}

Нет, я не властен над собой, и таки приведу в качестве примера используемое мной CSS-форматирование:

```css
/* Header */
header {
    margin-bottom: 16px;
    font-weight: 400;
}

header h1 {
    color: #999;
}

header p {
    font-size: 1.4em;
    margin-top: 0;
}
```

Почему это хорошо:

* такой CSS легко читается
* есть идентификатор начала блока (можно быстро найти необходимую часть даже в очень большом CSS-файле используя поиск по метке «* header»)
* подобное форматирование явно указывает на вложенность элементов
* и можно легко проследить наследование свойств

Я не настаиваю на своём варианте, но хотелось бы, чтобы вы приняли на вооружение один из многих стандартов форматирования и всегда следовали ему.

> _Когда станете матёрыми front-end разработчиками – познаете всю силу CSS-препроцессоров, а пока слушайте и запоминайте._

Это визуальное усложнение предназначено для людей; для браузеров оно не играет никакой роли, так как они всё равно при работе со стилями проводят конкатенацию, то есть удаляют переносы строк, отступы, комментарии, некритические пробелы. Некоторые типы вебсерверов, например, для банковских тонких клиентов, проводят сжатие (и архивацию) файлов ещё до пересылки к месту использования, из экономии трафика и денег.

### Именование классов и идентификаторов {#naming-convention}

Я уже затрагивал эту тему, когда рассказывал о релевантности HTML, так вот – имена классов могут быть даже такими: «b-service-list\_\_column b-service-list\_\_column\_right» и это будет круто, и «must have» – но лишь в рамках действительно больших проектов. Собственно, чего я распинаюсь? Дам для изучения [исходную точку](https://ru.bem.info/methodology/), информации там – ещё на одну книгу ;)

> _Обязательно ознакомьтесь с [принципами БЭМ](https://ru.bem.info/methodology/) – это полезно для расширения кругозора и прокачки скиллов._

### О цветах {#css-colors}

В WEB используется цветовая модель [RGB](http://www.w3.org/TR/css3-color/), именуемая по английским именам «несущих» цветовых каналов – red, green, blue – которая опирается на цифровое обозначение для любых оттенков. Поэтому красный цвет можно записать не только как «red», но и ещё несколькими способами:

```css
p { color: red }

p { color: #ff0000 }

p { color: #f00 }   /* сокращённая запись, экономит 3 байта */

p { color: rgb(255, 0, 0) }
```

> _Теперь вы без запинки должны назвать цвета `#f00`, `#0f0`, `#00f`, а те, у кого по рисованию было «отлично», назовут и `#ff0`, `#0ff` и `#f0f` ;)_

В CSS 3 поддерживается усовершенствованная модель RGBA, где мы дополнительно можем задать значение α-канала, т.е. прозрачность:

```css
p { color: rgba(255, 0, 0, 1) }   /* обычный текст */

p { color: rgba(255, 0, 0, 0.5) } /* полупрозрачный текст */
```

Ещё одна примочка CSS 3 – это возможность использования цветовых моделей [HSL](http://www.w3.org/TR/2010/PR-css3-color-20101028/) (hue, saturation, lightness – тон, насыщенность и светлота; озвучивается как «хью, сатурейшн, лайтнесс») и [HSLA](http://www.w3.org/TR/css3-color/) (HSL + α-канал):

```css
p { color: hsl( 0, 100%, 50%) }   /* красный */

p { color: hsl(120, 100%, 50%) }  /* зелёный */

p { color: hsl(240, 100%, 50%) }  /* синий */

p { color: hsla( 0, 100%, 50%, 0.5) } /* полупрозрачный красный */
```

Для перевода из модели HSL в модель RGB существует простой алгоритм, но пока не стоит им себя грузить.

> _Да кто этим HSL пользуется? Не морочьте себе голову!_

Тем, кого вопрос со смешанием каналов RGB поставил в тупик, наглядное руководство:

![](/assets/img/colors.gif)

### Блочные и строчные элементы {#block-and-inline}

> _Опять я буду ссылаться на чей-то учебник — на этот раз от Ивана Сагалаева — [http://softwaremaniacs.org/blog/category/primer/](http://softwaremaniacs.org/blog/category/primer/). И пусть вас не смущают даты написания статей — они повествуют об основах, и актуальность не потеряют ещё очень долго._

Возможно, вы ещё не знаете, но HTML-теги делятся на блочные (block-тип) и строчные (inline-тип). Блочными элементами называют те, которые отображаются как прямоугольник, занимают всю доступную ширину внутри элемента-родителя, и их высота определяется их содержимым. Блочные теги по умолчанию начинаются и заканчиваются новой строкой — это `<div>`, `<h1>` и собратья, `<p>` и другие.

Если хотите, чтобы ваш HTML оставался валидным, следите за тем, чтобы блочные элементы не располагались внутри строчных элементов. Внутри строчных тегов может быть либо текст, либо другие строчные элементы.

> _Одна из самых часто встречаемых ошибок, это оборачивание заголовка в ссылку: `<a href="#"><h1>Название статьи</h1></a>`. Не допускайте подобные промахи. Хотя если мы ориентируемся на HTML 5 – то тег `<a>` теперь может быть блочным элементом, и приведённый пример будет валидным.  
> Ага, вот такой я непоследовательный._

По теме:
* [Inline Elements List and What’s New in HTML5](http://www.tutorialchip.com/tutorials/inline-elements-list-whats-new-in-html5/)
* [HTML5 Block Level Elements: Complete List](http://www.tutorialchip.com/tutorials/html5-block-level-elements-complete-list/)
* [Раскладка в CSS: поток](http://softwaremaniacs.org/blog/2005/08/27/css-layout-flow/)

### О размерах блочных элементов {#size}

Ещё хотел отдельно остановиться на вычислении ширины и высоты блочных элементов, ведь тут есть один нюанс. По умолчанию высота и ширина элементов считаются без учёта толщины границ и внутренних отступов, т.е. как-то так:

![](/assets/img/box-height-1.png)

Эта блочная модель называется «content-box», и вот в CSS 3 появилась возможность изменять блочную модель, указывая атрибут «box-sizing». Отлично, теперь мы можем выбирать между двумя значениями «content-box» и «border-box». Первый я уже описал, а вот второй вычисляет высоту и ширину включая внутренние отступы и толщину границ:

![](/assets/img/box-height-2.png)

_Такая блочная модель была свойственна IE6 в «quirks mode»_

Полезные статьи по теме:

* [Блочные элементы](http://htmlbook.ru/content/blochnye-elementy)
* [Встроенные элементы](http://htmlbook.ru/content/vstroennye-elementy)

### Плавающие элементы {#float}

Я бы хотел ещё рассказать о CSS-свойстве «float», но боюсь, рассказ будет долгим и утомительным. Кратенько: если вы указываете элементу свойство «float», то:

* наш элемент будет смещён по горизонтали, и «прилипнет» к указанному краю родительского элемента
* если это был блочный элемент, то теперь он не будет занимать всю ширину родительского элемента и освободит место
* если следом идут блочные элементы, то они займут его место
* если следом идут строчные элементы, то они будут обтекать наш элемент со свободной стороны

Это поведение «по умолчанию», а как это выглядит вживую можно посмотреть на примере [css.float.html](../code/css.float.html):

{% jqbFrame "html-example", "../code/css.float.html", height="750px" %}{% endjqbFrame %}
 
Тут главное понимать происходящее и уметь управлять, если, конечно, вы хотите хоть чуть-чуть научиться верстать :)

Жизненно необходимая информация для верстальщиков:

* [Раскладка в CSS: float](http://softwaremaniacs.org/blog/2005/12/01/css-layout-float/)

### Позиционирование {#position}

Дам лишь вводную по «position». У него бывает лишь четыре значения:
* `static` — положение дел «по умолчанию», блоки ложатся друг за другом, сверху вниз, по порядку, без отклонений
* `absolute` — блок позиционируется согласно заданным координатам
* `fixed` — похоже на «absolute», с той лишь разницей, что блок не будет скроллиться
* `relative` — такой блок можно сдвигать относительно места, где он расположен, ну и все внутренние «абсолютные» блоки будут использовать данный элемент как точку отсчета при позиционировании по координатам

Для самостоятельного изучения:
* [Раскладка в CSS: позиционирование](http://softwaremaniacs.org/blog/2005/08/03/css-layout-positioning/)