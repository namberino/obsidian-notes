# Programming
- CMOV can prevent the processor from utilizing the *JMP* instructions and speed up the binary

- There are unsigned CMOV instructions:
	- CMOVA or CMOVNBE = Above \[Carry flag or zero flag = 0]
	- CMOVAE or CMOVNB = Above or equal \[Carry flag = 0]
	- CMOVNC = Not carry \[Carry flag = 0]
	- CMOVB or CMOVNAE = Below \[Carry flag = 1]
	- CMOVC = Carry \[Carry flag = 1]
	- CMOVBE or CMOVNA = Below or equal \[Carry flag or zero flag = 1]
	- CMOVE or CMOVZ = Equal \[Zero flag = 1]
	- CMOVNE or CMOVNZ = Not equal \[Zero flag = 0]
	- CMOVP or CMOVPE = Parity \[Parity flag = 1]
	- CMOVNP or CMOVPO = Not parity \[Parity flag =0]

- There are also signed CMOV instructions: 
	- CMOVGE or CMOVNL = Greater or equal \[Sign flag xor overflow flag = 0]
	- CMOVL or CMOVNGE = Less \[Sign flag xor overflow flag = 1]
	- CMOVLE or CMOVNG = Less or equal \[Sign flag xor overflow flag or ZF = 1]
	- CMOVO = Overflow \[Overflow flag = 1]
	- CMOVNO = Not overflow \[Overflow flag = 0]
	- CMOVS = Sign NEGATIVE \[Sign Flag = 1]
	- CMOVNS = Not sign POSITIVE \[Sign Flag = 0]

- The unsigned instructions utilize CF, ZF and PF to determine the difference between the two operands whereas the signed instructions utilize SF and OF to indicate the condition of the comparison between the operands
- The CMOV instructions rely on a mathematical instruction that sets the EFLAGS register to operate and therefore saves the programmer from using JMP statements after the compare statement

- Program:
![[cmov.jpg]]
- On line 24 we see the *find_smallest_value* function to where we are cycling through the array and using the CMOVB to find the lowest value ultimately
- We see **cmp %ebx, %eax** to which **cmp** subtracts the first operand from the second and sets the EFLAGS register appropriately. At this point the **cmovb** is used to replace the value in ebx with the value in eax if the value is smaller than what was originally in the ebx register
- After we exit the loop we see three sets of sys_writes to first display our message, second to display our converted integer to ASCII value and then finally a period and line feed

# Debugging
![[cmov.jpg]]

- Let's break on line 31 (**0x08048092**). Let's do **a r** to run and then type **print $ebx**:
![[cmov-gdb.jpg]]

- Let's break on line 46 (**0x080480b1**). When we're examining the value of **answer**, it has been converted to its ascii printable equivalent so in order to see the value of '7' you would type **x/1c &answer**:
![[cmov-gdb-answer.jpg]]

# Hacking
![[cmov-gdb-1.jpg]]

![[cmov-gdb-jmp.jpg]]
- **set $eip = 0x080480dd** is the exit routine. We see now that it bypasses all of the code from the **nop** instruction when we broke on `_start`. Use this command to jump anywhere inside of any binary within the debugger