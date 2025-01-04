# Programming
- Moving data between memory and registers:
![](../../Assets/move-memory.jpg)
- Move the constant integer 10 from memory into the ECX register

# Debugging
- Program:
![](../../Assets/move-memory.jpg)

- Debugging with GDB:
![](../../Assets/move-mem-gdb.jpg)

- The ECX register is 0:
![](../../Assets/move-mem-gdb-ecx.jpg)

- Step in twice, the ECX now holds the value 10 or 0xa
![](../../Assets/move-mem-gdb-ecx-10.jpg)

# Hacking
- Hacking time:
![](../../Assets/move-memory.jpg)

![](../../Assets/move-mem-gdb.jpg)

![](../../Assets/move-mem-gdb-ecx.jpg)

- The value of ECX is 0. Let's do 2 **si**:
![](../../Assets/move-mem-gdb-hack.jpg)
- Now the ECX value is 10 decimal

- Set ECX to 1337:
![](../../Assets/mov-mem-gdb-set.jpg)
