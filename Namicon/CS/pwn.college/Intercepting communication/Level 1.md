This level is pretty simple. All we need to do is to connect to a remote host at `10.0.0.3` at port `31337`. We can do this via `netcat`, which allows us to make a TCP connection by default.

```bash
nc 10.0.0.3 31337
```