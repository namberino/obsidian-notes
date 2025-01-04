# Programming
- Program: 
![[indirect-addr.jpg]]
- We can place more than one value in memory
- This creates a sequential series of data values placed in memory. Each data value occupies one unit of memory which is an integer or 4 bytes
- We must use an index system to determine these values
- We will utilize the indexed memory mode where the memory address is determined by a base address, an offset address to add to the base address and the size of the data element, in our case an integer of 4 bytes and an index to determine which data element to select

# Debugging
![[indirect-addr.jpg]]

- Debugging with GDB:
![[indirect-addr-gdb.jpg]]
- **x/6 &constants**: Examine 6 of the 11 values inside the constants label
- We then move the memory address of the constants label into EDI and move the immediate value of 25 decimal into the second index of our array

![[indirect-addr-gdb-1.jpg]]

# Hacking
![[indirect-addr.jpg]]

![[indirect-addr-gdb-print.jpg]]
- The command print `*0x804909e` yields a value of 5 decimal. The binary at runtime puts the values inside the constants label to a respective memory address
- The pointer to 0x804909e or `*0x804909e` holds 5 decimal as we have stated above (An integer holds 4 bytes of data)
- If we were to continue to advance through the array we would move 4 bytes to the next value and so forth. Remember each memory location in x86 32-bit assembly holds 4 bytes of data

- Changing values: 
![[indirect-addr-gdb-set.jpg]]
- **Note**: 9e + 4 bytes = a2