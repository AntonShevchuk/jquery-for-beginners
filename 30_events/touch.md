# Touch-события

Смартфоны с большим сенсорным экраном — это уже норма жизни, и любому web-разработчику рано или поздно потребуется разрабатывать интерфейсы с поддержкой touch-событий. На этот случай в JavaScript предусмотрены следующие события:

`touchstart` — событие схоже с `mousedown`, происходит при касании пальцем экрана

`touchend` — убираем палец с экрана, ака `mouseup`

`touchmove` — водим пальцем по экрану — `mousemove`

`touchcancel` — странное событие, отмена `touch` до того, как палец был убран

О том, как с ними работать, можно разузнать из отличной статьи на английском языке — «[Touching and Gesturing on iPhone, Android, and More](https://www.sitepen.com/blog/touching-and-gesturing-on-iphone-android-and-more)» (хотя там рассказ и о Dojo Toolkit).
