# Programming
- Moving data from 1 register to another is the fastest way to move data, so the best practice is to keep data between registers as much as possible

- We'll move the value in EDX into EAX:
![[move-reg.jpg]]

- **Note**: 
	- We can only move data between similar registers (EAX and EDX are 32-bit registers)
	- Each of these registers can be accessed by their 16-bit values as AX and DX respectively
	- Can't move a 32-bit value into a 16-bit value and vice versa

# Debugging
- The move data between regs program:
![[move-reg.jpg]]

- Debugging using GDB:
![[move-reg-gdb.jpg]]

- Let's **si** twice and **i r**:
![[move-reg-gdb-step.jpg]]
- EDX now has the value 22

- Let's **si** again:
![[move-reg-gdb-mov.jpg]]
- EAX now has the same value as EDX

# Hacking
- Let's work with the same move between registers program: 
![[move-reg.jpg]]

- Debugging with GDB:
![[move-reg-gdb-start.jpg]]

- **si** twice and **i r**:
![[move-reg-gdb-start-si.jpg]]
- The value of 0x16 or 22 decimal did move into EDX successfully

- We can set EDX to 0x19:
![[move-reg-gdb-set-edx.jpg]]
