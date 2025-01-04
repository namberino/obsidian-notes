# Programming
- Moving data between memory and registers:
![[move-memory.jpg]]
- Move the constant integer 10 from memory into the ECX register

# Debugging
- Program:
![[move-memory.jpg]]

- Debugging with GDB:
![[move-mem-gdb.jpg]]

- The ECX register is 0:
![[move-mem-gdb-ecx.jpg]]

- Step in twice, the ECX now holds the value 10 or 0xa
![[move-mem-gdb-ecx-10.jpg]]

# Hacking
- Hacking time:
![[move-memory.jpg]]

![[move-mem-gdb.jpg]]

![[move-mem-gdb-ecx.jpg]]

- The value of ECX is 0. Let's do 2 **si**:
![[move-mem-gdb-hack.jpg]]
- Now the ECX value is 10 decimal

- Set ECX to 1337:
![[mov-mem-gdb-set.jpg]]
