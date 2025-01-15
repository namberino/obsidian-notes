This level requires us to deal with multi-processing. For multi-processing, we can make use of the `fork` syscall. This syscall allows us to duplicate our process, effectively creating a child process. The 2 processes maintain its own memory, there is a concept called shared memory but we're not using it here. When a process close a file descriptor, it basically closed that file descriptor in that process only, the forked child process still maintain that file descriptor, and if the child process wants to close that file descriptor, it would have to do it on its own.

The fork syscall will return a child process ID to the parent process, and 0 to the child process. This is useful for identifying which is the child and which is the parent. For the web server, the parent process will be responsible for accepting the socket connection, but not for serving and talking to the accepted connection. The child process will be responsible for serving and talking to the accepted connection, but not for accepting new socket connection.

So the parent process will accept a new connection, fork itself to create a child process to handle that connection, close that connection (since the child process still have access to that connection to talk to), and listen for new connection. If there's another new connection, it does the same thing again, creating another different child process to handle that new connection.

The child process will close the binded and listening socket file descriptor (since it doesn't handle listening for new connections) and process the current accepted connection by communicating through the accepted connection's file descriptor.

So we can code the forking like this:

```asm
accept_conn:
    # accept connection to socket
    mov rdi, r8       # get socket fd
    mov rsi, 0x0      # NULL
    mov rdx, 0x0      # NULL
    mov rax, 43       # sys_accept
    syscall
    mov r9, rax       # accepted socket fd

    mov rax, 57       # sys_fork
    syscall

    # serve the accepted socket connection if is child process
    cmp rax, 0        # child process always returns 0 on fork call
    je serve_conn

    # close the accepted socket connection if is parent process
    mov rdi, r9       # get accepted socket fd
    mov rax, 3        # sys_close
    syscall

    jmp accept_conn

serve_conn:
    # close the socket fd
    mov rdi, r8       # get binded socket fd
    mov rax, 3        # sys_close
    syscall

    # read data from the connected socket
    mov rdi, r9       # get accepted socket fd
    lea rsi, read_buffer
    mov rdx, 1024     # buffer size
    mov rax, 0        # sys_read
    syscall
    ...
```

When the child process exits, it will terminate that process So we need to make sure we closed and cleaned up well before killing the child:

```asm
serve_stop:
    # close socket connection
    mov rdi, r9       # get accepted socket fd
    mov rax, 3        # sys_close
    syscall

    # exit(0)
    mov rdi, 0
    mov rax, 60       # sys_exit
    syscall
```