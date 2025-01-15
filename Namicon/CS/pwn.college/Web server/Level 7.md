We need to dynamically respond to a GET request. Meaning we need to actually access the file that the GET request specifies, read the content in that file, then send the read content in a response to the connection.

Firstly, we need to extract the filename that's going to be accessed. And we'll also get the request type too (which will come in handy later on).

```asm
# get request type
lea rdi, request_type
lea rsi, read_buffer
mov rdx, 0        # start index
call get_substring
mov r10, rax      # end index

# get filename
lea rdi, filename
lea rsi, read_buffer
mov rdx, r10     # start index
call get_substring
```

We can implement a `get_substring` function to get the substring that can get us the data that we need:

```asm
get_substring:
    mov r15, 0

substring_loop:
    # read each character from buffer
    cmp byte ptr [rsi+rdx], 32        # found a space
    je end_get_substring

    # copy current character to request type buffer
    mov r14b, byte ptr [rsi+rdx]
    mov [rdi+r15], r14b
    inc rdx
    inc r15
    jmp substring_loop

end_get_substring:
    mov byte ptr [rdi+r15], 0x00      # null terminating character
    inc rdx
    mov rax, rdx                      # return end index
    ret
```

This basically loops through each character of the string, copy each character of the string to a new buffer specified in the parameter, and stop when it hits a space. It'll also return the length of the string read too.

Now we can call the syscall `open` to open a file descriptor to the file to be read from in read mode. Then we can save use the `read` syscall to read the content of the file into a buffer, then close the opened file's file descriptor, then write the content in the recently closed file's read buffer into the accepted connection.

```asm
# open file to read
lea rdi, filename
mov rsi, 00000000 # O_RDONLY
mov rax, 2        # sys_open
syscall
mov r10, rax      # opened file fd

# read the opened file
mov rdi, r10      # get opened file fd
lea rsi, file_content
mov rdx, 1024
mov rax, 0        # sys_read
syscall
mov r15, rax      # get number of bytes read

# close the opened file
mov rdi, r10      # get opened file fd
mov rax, 3        # sys_close
syscall

# write response message to socket connection
mov rdi, r9       # get accepted socket fd
lea rsi, response_msg
mov rdx, 19
mov rax, 1        # sys_write
syscall

# write the read file to socket connection
mov rdi, r9       # get accepted socket fd
lea rsi, file_content
mov rdx, r15      # number of bytes in file content
mov rax, 1        # sys_write
syscall
```