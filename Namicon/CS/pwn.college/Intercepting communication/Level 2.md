Now we need to listen for a remote host. This remote host will always be trying to connect to our host via port `31337`. So we can use `netcat` again to open up that port and listen for connections.

```bash
nc -lvp 31337
```