# Touch-події

Смартфони з великим сенсорним екраном — це вже давно норма життя, і будь-якому web-розробнику рано чи пізно знадобиться розробляти інтерфейси з підтримкою touch-подій. На цей випадок у JavaScript передбачені наступні події:

<table data-header-hidden><thead><tr><th width="202">подія</th><th>опис</th></tr></thead><tbody><tr><td><code>touchstart</code></td><td>подія схожа на <code>mousedown</code>, відбувається при дотику пальцем до екрана</td></tr><tr><td><code>touchend</code></td><td>прибираємо палець з екрана, аля <code>mouseup</code></td></tr><tr><td><code>touchmove</code></td><td>водимо пальцем по екрану, як <code>mousemove</code></td></tr><tr><td><code>touchcancel</code></td><td>дивна подія, скасування <code>touch</code> до того, як палець було прибрано</td></tr></tbody></table>

Про те, як з ними працювати, можна дізнатися з чудової статті англійською мовою — «[Touching and Gesturing on iPhone, Android, and More](https://www.sitepen.com/blog/touching-and-gesturing-on-iphone-android-and-more)» (хоча там розповідь і про Dojo Toolkit).
