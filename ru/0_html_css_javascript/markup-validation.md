# Валидный HTML

Зелёный маркер [W3C validator'а](https://validator.w3.org/) – это правильно, и к этому надо стремиться, так что не забывайте закрывать теги да прописывать обязательные параметры. Приведу пример HTML-кода, в котором согласно спецификации HTML 5 допущено шесть ошибок, найдите их:

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

> Если сомневаетесь в своих знаниях, проверьте данный код на странице [https://validator.w3.org/nu/#textarea](https://validator.w3.org/nu/#textarea)
