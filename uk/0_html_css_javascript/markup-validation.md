# Валідний HTML

Зелений маркер [W3C validator's](https://validator.w3.org/) — це правильно, і до цього треба прагнути, тож не забувайте закривати теги та прописувати обов'язкові параметри. Наведу приклад HTML-коду, в якому згідно зі специфікацією HTML 5 допущено шість помилок, знайдіть їх:

```html
<!DOCTYPE html>
<html>
    <body>
        <h2>Lorem ipsum
        <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
        Nunc urna metus, ultricies eu, congue vel, laoreet id, justo.
        Aliquam fermentum adipiscing pede. Suspendisse dapibus ornare
        quam. Lorem ipsum dolor sit amet, consectetuer adipiscing elit.<p>
        <a href="/index.php?mod=default&act=image"><img src="/img001.jpg"></a>
    </body>
</html>
```

> Якщо сумніваєтесь у своїх знаннях, перевірте цей код на сторінці [https://validator.w3.org/nu/#textarea](https://validator.w3.org/nu/#textarea)
