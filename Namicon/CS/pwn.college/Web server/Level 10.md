This level requires us to implement processing for the POST request too. The POST request will specify a file and contain a content at the end of the file specifying what to write to the specified file (Lots of specifying). The content of the file will be separated with a double newline like this: `\r\n\r\n`.

So getting the filename is simple, we've already done that for the GET request.

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

This is where the get request type function comes in play. We can differentiate which handling function to use for a particular request type since we want to handle both POST and GET requests.

```asm
# jump to correct request handling section
mov r15b, [request_type]
cmp r15b, 71     # 'G'
jne POST_request

GET_request:
	...
```

The POST request requires us to open up a file to write or create a new file if it doesn't exist yet:

```asm
# open file to read
lea rdi, filename
mov rsi, 00000101 # O_WRONLY | O_CREAT
mov rdx, 0777
mov rax, 2        # sys_open
syscall
mov r10, rax      # opened file fd
```

Now we need to find the content in the request. For this, we have the `find_string` function:

```asm
# get request content
lea rdi, read_buffer
lea rsi, content_buffer
lea rdx, double_newline
call find_string
mov r11, rax      # get length of content

...

find_string:
    mov r15, 0
    mov r14, 0

next_char:
    cmp byte ptr [rdi+r15], 0x00               # check for null terminator
    je end_find

compare_loop:
    # only compare 4 bytes
    cmp r14, 4
    je found_match

    # compare the next 4 bytes
    mov rax, r14
    add rax, r15
    mov r11b, byte ptr [rdx+r14]               # byte from comparison source
    cmp byte ptr [rdi+rax], r11b
    jne no_match

    # byte matches
    inc r14
    jmp compare_loop

no_match:
    mov r14, 0
    inc r15
    jmp next_char

found_match:
    # r15 now has starting position of content string
    add r15, 4
    mov rax, 0

copy_content:
    # copy bytes to content buffer
    mov rbx, r15
    add rbx, rax
    
    # end of string
    cmp byte ptr [rdi+rbx], 0x00                         # null terminator
    je end_find

    # copy bytes
    mov r14b, byte ptr [rdi+rbx]
    mov byte ptr [rsi+rax], r14b
    inc rax

    jmp copy_content

end_find:
    ret
```

The `find_string` function has 2 parts, the first part goes through each character in the request string until the end of the string. For each of that character, it compares that character along with 3 characters following that character to the double newline pattern. If it matches, we save the index of the character following those 4 characters and move to the second part.

The second part basically copies each characters starting from the index given by the first part into a buffer that contains the content of the request.

With the request content read, we can write the content to the file, then close the file descriptor for that file.

```asm
# write to file with request content
mov rdi, r10       # get opened file fd
lea rsi, content_buffer
mov rdx, r11      # number of bytes in content buffer
mov rax, 1        # sys_write
syscall

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
```