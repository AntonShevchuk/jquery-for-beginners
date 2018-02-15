## Touch события {#touch}

Смартфоны с большим сенсорным экраном — это уже норма жизни, и любому web-разработчику рано или поздно потребуется разрабатывать интерфейсы с поддержкой «touch» событий. На этот случай в JavaScript'е предусмотрены следующие события:

* **touchstart** — событие схоже с «mousedown», происходит при касании пальцем экрана

* **touchend** — убираем палец с экрана, ака «mouseup»

* **touchmove** — водим пальцем по экрану — «mousemove»

* **touchcancel** — странное событие, отмена «touch» до того, как палец был убран

О том как с ними работать, можно подчерпнуть из отличной статьи на английском языке — «Touching and Gesturing on iPhone, Android, and More» [[http://www.sitepen.com/blog/?p=3425](http://www.sitepen.com/blog/?p=3425)] (хоть рассказ там и о Dojo Toolkit).

В [jQuery Mobile](../dopolnenie/jquery_mobile.md) работа с touch событиями идёт «из коробки». Чтобы jQuery UI заставить работать с touch событиями следует использовать библиотеку [jQuery UI Touch Punch](http://touchpunch.furf.com/). Пробуйте, но учтите, без touch устройства разработка интерфейсов для подобных устройств – нонсенс (_англ. nonsense_).