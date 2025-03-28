This is an SQL injection level. The goal here is to inject SQL commands into the server's account check query to log into the admin account.

```python
#!/opt/pwn.college/python

import random
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
db.execute("""CREATE TABLE users AS SELECT "admin" AS username, ? as pin""", [random.randrange(2**32, 2**63)])

# https://www.sqlite.org/lang_insert.html
db.execute("""INSERT INTO users SELECT "guest" as username, 1337 as pin""")

@app.route("/", methods=["POST"])
def challenge_post():
    username = flask.request.form.get("username")
    pin = flask.request.form.get("pin")

    if not username:
        flask.abort(400, "Missing `username` form parameter")

    if not pin:
        flask.abort(400, "Missing `pin` form parameter")

    if pin[0] not in "0123456789":
        flask.abort(400, "Invalid pin")

    try:
        # https://www.sqlite.org/lang_select.html
        query = f"SELECT rowid, * FROM users WHERE username = '{username}' AND pin = { pin }"
        print(f"DEBUG: {query=}")
        user = db.execute(query).fetchone()
    except sqlite3.Error as e:
        flask.abort(500, f"Query: {query}\nError: {e}")

    if not user:
        flask.abort(403, "Invalid username or pin")

    flask.session["user"] = username
    return flask.redirect(flask.request.path)

@app.route("/", methods=["GET"])
def challenge_get():
    if not (username := flask.session.get("user", None)):
        page = "<html><body>Welcome to the login service! Please log in as admin to get the flag."
    else:
        page = f"<html><body>Hello, {username}!"
        if username == "admin":
            page += "<br>Here is your flag: " + open("/flag").read()

    return (
        page
        + """
        <hr>
        <form method=post>
        User:<input type=text name=username>Pin:<input type=text name=pin><input type=submit value=Submit>
        </form>
        </body></html>
    """
    )

app.secret_key = os.urandom(8)
app.config["SERVER_NAME"] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

Since the username is set as a session, and that session will be the thing that will be used to check if we have permission to view the flag, we need to keep the username field as `admin`. Therefore, the SQL injection is in the pin field. The pin field does have a validation, it checks if the pin field contains numbers or not. We can perform a boolean-based SQL injection in the pin field. Since we don't know the pin, we just input some arbitrary number into it, then perform a conditional `OR` with a condition that's always true and top it off with a comment at the end to remove any trailing quotes (even though there's none in this level.

```
Username: admin
Pin: 01234 OR '1'='1'--
```