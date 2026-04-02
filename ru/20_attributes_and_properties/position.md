---
description: Это не понты, это позиция ©
---

# Позиция

Давайте добавлю ещё чуть-чуть информации про методы, которые расскажут вам про расположение элементов:

<table data-header-hidden><thead><tr><th width="323">метод</th><th>описание</th></tr></thead><tbody><tr><td><pre class="language-javascript"><code class="lang-javascript">offset()
</code></pre></td><td>возвращает позицию DOM-элемента относительно document, данные будут получены в виде объекта: <br><code>{ top: 10, left: 30 }</code></td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">offset({ top: 10, left: 30 })
</code></pre></td><td>устанавливает расположение DOM-элемента по указанным координатам</td></tr><tr><td><pre class="language-javascript"><code class="lang-javascript">position()
</code></pre></td><td>возвращает позицию DOM-элемента относительно родительского элемента</td></tr></tbody></table>

{% embed url="https://anton.shevchuk.name/book/code/offset-and-position.html" %}
