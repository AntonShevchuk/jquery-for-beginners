# Touch Events

Smartphones with large touchscreens have long been a fact of life, and sooner or later any web developer will need to build interfaces with touch event support. For this purpose, JavaScript provides the following events:

<table data-header-hidden><thead><tr><th width="202">event</th><th>description</th></tr></thead><tbody><tr><td><code>touchstart</code></td><td>similar to <code>mousedown</code>, fires when a finger touches the screen</td></tr><tr><td><code>touchend</code></td><td>finger lifted from the screen, like <code>mouseup</code></td></tr><tr><td><code>touchmove</code></td><td>finger moving across the screen, like <code>mousemove</code></td></tr><tr><td><code>touchcancel</code></td><td>a weird event — cancellation of <code>touch</code> before the finger was lifted</td></tr></tbody></table>

You can learn how to work with them from an excellent article — "[Touching and Gesturing on iPhone, Android, and More](https://www.sitepen.com/blog/touching-and-gesturing-on-iphone-android-and-more)" (although it also covers Dojo Toolkit).
