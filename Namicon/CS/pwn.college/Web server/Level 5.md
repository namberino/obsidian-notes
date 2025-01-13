This level requires us to accept a connection. The `accept` syscall is used with connection-based socket types like `SOCK_STREAM`. It extracts the first connection request on the queue of pending connections for the listening socket (`sockfd`), create an new connected socket, and return a file descriptor referring to that new connected socket.

It takes in 3 parameters: the socket file descriptor, the `sockaddr` structure, and the address length. The `sockaddr` structure will contain the address of the peer for the accepted connection (kinda like the address of the client). The structure will be filled in by `accept`. Same goes for the address length, which just contains the size of the peer address. The structure might be useful for when you want to communicate back with the client.

Since we're just accepting here, we can just set the peer address to `NULL`. We'll also save the return value, which is the accepted socket file descriptor, to `r9`.

```asm
mov rdi, r8     # get socket fd
mov rsi, 0x0    # NULL
mov rdx, 0x0    # NULL
mov rax, 43     # sys_accept
syscall
mov r9, rax     # accepted socket fd
```