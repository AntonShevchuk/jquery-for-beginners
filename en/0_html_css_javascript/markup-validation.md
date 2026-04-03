# Valid HTML

A green checkmark from the [W3C validator](https://validator.w3.org/) -- that's the right thing to aim for, so don't forget to close your tags and include all required attributes. Here's an example of HTML code that, according to the HTML 5 spec, contains six errors -- find them:

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

> If you're not sure about your skills, check this code at [https://validator.w3.org/nu/#textarea](https://validator.w3.org/nu/#textarea)
