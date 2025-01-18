This requires us to listen on the binded interfaces. This is pretty simple, we just use the `listen` syscall. This syscall take 2 parameters: 1st parameter is the socket file descriptor, second parameter is the backlog, which determines the max length queue of pending connections. So if there's a new connection but the backlog queue is full, the connection will be refused.

For now, the backlog will be set to 0, since this will make the `listen` syscall set the backlog to the implementation's minimum.

```
mov rdi, r8     # get socket fd
mov rsi, 0      # backlog queue length
mov rax, 50     # sys_listen
syscall
```