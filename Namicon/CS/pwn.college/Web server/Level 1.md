This level is pretty simple, just a program that can exit cleanly. We just use the `sys_exit` syscall (`rax = 60`) with `rdi` set to 0 (First argument is 0)

```asm
mov rdi, 0      # exit code
mov rax, 60     # sys_exit
syscall
```