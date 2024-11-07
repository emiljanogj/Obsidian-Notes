- Registers
![[Pasted image 20230621170939.png]]


- Instructions where the first operand is both a source and a destination:
```nasm
add <register>, <register>
cmp <register>, <memory>
mov <register>, <immediate>
xor <memory>, <register>
add <memory>, <immediate>
```

- `.bss` is a section with uninitialised(reserved) variables as shown below:
```nasm
Section .bss
    number:   resb 4  ; Reserve 4 bytes
    word:    resw 1  ; Reserve 1 word(16 bits)
    doubleword: resd 1  ; Reserve 1 dword(32 bits)
    quadword:  resq 1  ; Reserve 1 qword(64 bits)
```


### Examples

1. Reading a number, and writing it if it is in the interval [1,9]
```nasm
OtherCode:   
    ………………………
Output:   ; sys_write implementation   
    mov rdx, len   
    mov rsi, string   
    mov rdi, 1   
    mov rax, 1   
    syscall
       
    cmp rax, 0   
	jz exit
input:   ; sys_read implementation   
	mov rdx, 1   ; Number of bytes to be read (should match how many bytes are reserved)   
	mov rsi, number    ; Copy the label into rsi, this is located in the .bss section below   
	mov rdi, 0   ; Copy the value for STDIN into rdi   
	mov rax, 0   ; Copy the value of sys_read into rax   
	syscall   
	
	mov rbx, [rsi]   ; Copy the VALUE contained in the memory address of rsi into rbx
checks:   ; This is code for checking if the number value inside [rbx] is between 1 and 9   
	cmp bl, 31h   ; Compare the lowest 8 bits of the register rbx with 31h which is ‘1’ in ascii   
	jb input   ; Jump if the comparison comes back that the number is above ‘1’ in the ascii table   
	cmp bl, 39h   ; Compare the lowest 8 bits of the register rbx with 39h which is ‘9’ in ascii   
	ja input  ; Jump if the comparison comes back that the number is above ‘9’ in the ascii table   
	mov [number], rbx   ; Copy the value in rbx into the location of number in the .bss section
MoreCode:   
	…………………

Section .data   
	string: db "Enter a number between 1 and 9: ", 0x0a   
	len:   equ $ - string
Section .bss   
	number: resb 1  ; Reserve 1 byte at the label number in the .bss section
```

2. File Manipulation

```nasm
EarlierCode:   ; Other code to do other things   
	…………
FileManipulation:   ; Label for File Manipulation code
.createfile:   
	mov rsi, 0777o  ; Copy the permissions to rsi for the file. This is the same as rwx for everyone.
	mov rdi, filename  ; Copy the label related to the filename into rdi   
	mov rax, 85  ; Copy the syscall number for sys_creat into rax   
	syscall   
	mov [fd], rax  ; Copy the file descriptor into the VALUE located at the label fd. fd is located in the .bss section
.writefile:   
	mov rdx, 1  ; Copy the size of the string to write to the file into rdx   
	mov rsi, number ; Copy the number label from the .bss section into   
	mov rdi, [fd]   
	mov rax, 1   
	syscall
.closefile:   
	mov rdi, [fd]   
	mov rax, 3   
	syscall
MoreCode:  ; The rest of the code goes here   
	………
Section .data   
	filename: db “File.txt”, 0x00  ; Store the filename in the .data section with a null terminator at the end   
	number: db “9”  ; Store the value “9” in the .data section
Section .bss   
	fd: resb 1  ; Reserve a byte in the label fd for file descriptor
```

