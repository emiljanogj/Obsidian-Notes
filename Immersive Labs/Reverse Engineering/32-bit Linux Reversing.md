#32-bit_Linux 
***


![[Pasted image 20230619081654.png]]

- Note the above line `equ $ - string`. It means take the current memory address(using the notation `$`) and subtract the memory location which is labelled as `string`. 
- The `.text` section denotes the code section
- The `.data` section denotes the variables
- The `cmp eax, 0` line checks the number of bytes printed. If it is zero jump back to the `exit` section.

- Useful code snippet
```nasm
OtherCode:   
    ………………………
Output:   ; sys_write implementation   
    mov edx, len   
    mov ecx, string   
    mov ebx, 1   
    mov eax, 4   
    int 80h   

    cmp eax, 0   
    jz exit
input:   ; sys_read implementation   

    mov edx, 1   ; Number of bytes to be read (should match how many bytes are reserved)   
    mov ecx, number    ; Copy the label into ecx, this is located in the .bss section below   
    mov ebx, 0   ; Copy the value for STDIN into ebx   
    mov eax, 3   ; Copy the value of sys_read into eax   
    int 80h   
    mov ebx, [ecx]   ; Copy the VALUE contained in the memory address of ecx into ebx
    
checks:   ; This is code for checking if the number value inside [ecx] is between 1 and 9   
    cmp bl, 31h   ; Compare the lowest 8 bits of the register ebx with 31h which is ‘1’ in ascii   
    jb input   ; Jump if the comparison comes back that the number is below ‘1’ in the ascii table   
    cmp bl, 39h   ; Compare the lowest 8 bits of the register ebx with 39h which is ‘9’ in ascii   
    ja input  ; Jump if the comparison comes back that the number is above ‘9’ in the ascii table 
  
    mov [number], ebx   ; Copy the value in ebx into the location of number in the .bss section
MoreCode:   
    …………………
    
Section .data   
    string: db "Enter a number between 1 and 9: ", 0x0a   
    len:   equ $ - string
Section .bss   
    number: resb 1  ; Reserve 1 byte at the label number in the .bss section
```

*Note*: 0x31 is the ASCII code of 1 and 0x39 is the ASCII code of 5. The `checks` section checks if the number is between 1 and 9. 
*Note*: The value read is stored in the `ecx` register, and is copied to the `ebx` register.

### Registers

![[Pasted image 20230620234250.png]]


### Syscalls

This example creates a file using the `sys_creat` syscall, writes to the file using the `sys_write` syscall and closes the file using the `sys_close` syscall.

![[Pasted image 20230620234727.png]]

![[Pasted image 20230620234758.png]]

