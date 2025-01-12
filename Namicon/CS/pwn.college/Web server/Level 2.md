We're creating sockets with this level.

```asm
mov dil, 2      # AF_INET
mov sil, 1      # SOCK_STREAM
mov dl, 0       # IPPROTO_IP 
mov rax, 41     # sys_socket
syscall
mov r8, rax     # socket fd
```

The [socket syscall](https://man7.org/linux/man-pages/man2/socket.2.html) takes 3 arguments: domain, type, protocol. Here's what each of the parameters mean:
- `AF_INET` is the IPv4 protocol. 
- `SOCK_STREAM` is the communication semantics. This enables 2-way sequenced byte streams.
- `IPPROTO_IP` automatically chooses a protocol based on socket type and family. In this case, the protocol will be TCP.

The socket file descriptor will be saved to `r8`.