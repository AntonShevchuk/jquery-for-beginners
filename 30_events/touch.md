# Touch-события

Смартфоны с большим сенсорным экраном — это уже давно норма жизни, и любому web-разработчику рано или поздно потребуется разрабатывать интерфейсы с поддержкой touch-событий. На этот случай в JavaScript предусмотрены следующие события:

<table data-header-hidden><thead><tr><th width="202">событие</th><th></th></tr></thead><tbody><tr><td><code>touchstart</code></td><td>событие схоже с <code>mousedown</code>, происходит при касании пальцем экрана</td></tr><tr><td><code>touchend</code></td><td>убираем палец с экрана, аля <code>mouseup</code></td></tr><tr><td><code>touchmove</code></td><td>водим пальцем по экрану, как <code>mousemove</code></td></tr><tr><td><code>touchcancel</code></td><td>странное событие, отмена <code>touch</code> до того, как палец был убран</td></tr></tbody></table>

О том, как с ними работать, можно разузнать из отличной статьи на английском языке — «[Touching and Gesturing on iPhone, Android, and More](https://www.sitepen.com/blog/touching-and-gesturing-on-iphone-android-and-more)» (хотя там рассказ и о Dojo Toolkit).
