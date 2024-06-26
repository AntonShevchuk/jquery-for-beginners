---
description: Или Server-Side Rendering для самых маленьких
---

# Лечим JavaScript-зависимость

Любовь к AJAX бывает чрезмерной и в погоне за Web2.0 (3.0, 4.0, … — желаемое подчеркнуть) мы создаём сайты, в которых все наши действия бегут через [XMLHTTPRequest](https://ru.wikipedia.org/wiki/XMLHttpRequest). Нет, это, конечно, неплохо — снижаем нагрузку на сервер, канал и т.д. и т.п., но есть одно «но» — у нас есть поисковые машины, которые не всегда озадачивают себя выполнением JavaScript-кода, а контент, спрятанный за AJAX-запросом, им отдать всё-таки нужно. Следовательно, у нас возникает необходимость дублирования навигации (это как минимум) для клиентов без JavaScript.

> Стоит помнить, что есть ещё пользователи, у которых JavaScript отключён в браузере (или даже не поддерживается; привет тебе, «Linx»), но эти знают, что делают. А ещё есть скрипты, которые ломаются и не дают обычным пользователям воспользоваться навигацией по сайту, что пользователей очень сильно расстраивает, так что эта глава не «просто так».

Как же всё это обойти и на грабли не наступить? Да всё очень просто — создавайте обычную навигацию, которую вы бы делали, не слышав ни разу об AJAX и компании:

```markup
<ul class="navigation">
    <li><a href="/">Home</a></li>
    <li><a href="/about.html">About Us</a></li>
    <li><a href="/contact.html">Contact Us</a></li>
</ul>
<section id="content"><!-- Content --></section>
```

Данный пример работает у нас без JavaScript, все страницы в нашем меню используют один и тот же шаблон для вывода информации, и по факту у нас изменяется лишь содержимое элемента с `id="content"`. Теперь приступим к загрузке контента посредством AJAX – для этого добавим следующий код:

```javascript
$(function() {
    // вешаем обработчик на все ссылки в нашем меню navigation
    $("ul.navigation a").click(function(){
        var url = $(this).attr("href");   // возьмем ссылку
        url += "?ajax=true";              // добавим к ней параметр ajax=true
        $("#content").load(url);          // загружаем обновлённое содержимое
        return false; // возвращаем false, дабы не сработал переход по ссылке
    });
});
```

В данном примере мы предполагаем, что сервер, видя параметр `ajax=true` вернет нам не полностью всю страницу, а лишь обновление для искомого элемента с `id="content"`.

> Конечно, сервер должен быть умнее и не требовать явного указания для использования AJAX, а должен вполне удовлетвориться, словив header «`X_REQUESTED_WITH`» со значением «`XMLHttpRequest`». Большинство современных фреймворков для web-разработки с этим справляются «из коробки».

Если же управлять поведением сервера проблематично и он упёрто отправляет нам всю страницу целиком, то можно написать следующий код:

```javascript
$(function() {
    // вешаем обработчик на все ссылки в нашем меню navigation
    $("ul.navigation a").click(function(){
        var url = $(this).attr("href"); // возьмем ссылку
        // загружаем страницу целиком, но в наш контейнер вставляем лишь содержимое #content загружаемой страницы
        $("#content").load(url + " #content > *");
        return false; // возвращаем false
    });
});
```

Если в подгружаемом содержимом также есть ссылки, то вы уже сможете «оживить» события:

{% embed url="https://anton.shevchuk.name/book/code/ajax.live.html" %}
