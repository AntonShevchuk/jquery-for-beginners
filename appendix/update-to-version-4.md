# Обновление на версию 4.х

На данный момент команда jQuery зарелизила новую версию, пока это лишь beta-версия, но я думаю основные изменения уже внесены, и впереди нас ждёт основной релиз с мелкими исправлениями найденных багов.

Из основного:

*   избавились от «багажа» старых браузеров: Internet Explorer 10, Edge Legacy, iOS <11, Firefox <65 и Android Browser;

    > от Internet Explorer 11 обещают избавится в jQuery 5.0
* удалили с дюжину функций, которые в 3.x были объявлены устаревшими;
*   навели порядок с порядком в событиях `blur` → `focusout` → `focus` → `focusin`, теперь так, и только так, в соответствии с новой редакцией от W3C;

    > теперь только у IE остался другой порядок, а ирония заключается в том, что только IE следовал предыдущей редакции W3C, и лишь в 2023-м году спецификацию изменили под реализацию в современных браузерах&#x20;
* добавлена поддержка двоичных данных в `$.ajax()`;
* убрали автоматический процессинг JSON как JSONP, теперь при подобном запросе нужно будет явно указывать, что вы хотите получить именно JSONP-ответ от сервера;
* фреймворк переехал полностью с AMD на ES модули;
* добавлены поддержка [Trusted Types](https://github.com/w3c/trusted-types), чтобы в консоли не было ошибок Content Security Policy.

Вроде список небольшой, и я думаю, что при обновлении на jQuery 4.0 никаких проблем не должно будет возникнуть.
