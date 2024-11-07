**Usual registers and their use**:
- **RAX** is normally used in any arithmetic operation.
- **RBX** is normally used as a base register to start referring to memory addresses.
- **RCX** is normally the counter registers for looping.
- **RDX** is normally the register used for storing data for operations.
- **R8**-**R15** registers are normally used to hold certain data for any operations.
-  **RIP** is the instruction pointer that holds the memory address of the next instruction to be run.
- **RBP** is the base pointer that holds the memory address of the base of the current stack frame. If there are certain optimisations made by the assembler, **RBP** becomes another general-purpose register!
- **RSP** is the stack pointer that holds the memory address of the top of the current stack frame.
- **RSI** is the source index where the memory location for the start of a string is stored for string operations.
- **RDI** is the destination index where the memory location to store the string after it has been operated on is stored.

#### Windows API

![[Pasted image 20230621182036.png]]

*Note*: In Win32 Assembly, we saw that the arguments were passed to a function by pushing them in the stack in the reverse order. In Win64 however, the first 4 arguments are passed via registers and the rest via the stack as follows:

- Argument 1 – Passed using the register `RCX`
- Argument 2 – Passed using the register `RDX`
- Argument 3 – Passed using the register `R8`
- Argument 4 – Passed using the register `R9`
- Argument 5+ – Passed via the stack

**Example**: Stack frame during a simple function call

![[Pasted image 20230621185231.png]]

#### Assemble + link assembly code

- To create a 64-bit executable we use the following command:
```
ml64.exe /c /Cp .\assembly.asm
```

where:
- /c - assemble without linking
- /Cp - preserve case sensitivity

- To link external libs we use the following command:
```
link /SUBSYSTEM:console /defaultlib:"C:\Program Files (x86)\Windows Kits\10\Lib\10.0.18362.0\um\x64\kernel32.lib" .\assembly.obj /entry:main
```
where:
- `/SUBSYSTEM`- this option tells the linker that the exe will run through the console only so no GUI is needed
- `/defaultlib` - this option tells the linker where the labels attached to the `extern` directory are found
- `/entry` - this option tells the linker where the entrypoint is.

#### Example

The code below is a simple example that shows how to write a string to the STDOUT using Win64 Assembly:
```
extern  ExitProcess:PROC                    ; Task: Add ExitProcess to the extern directive  
extern  GetStdHandle:PROC                    ; Task: Add GetStdHandle to the extern directive 
extern  WriteConsoleA:PROC                     ; Task: Add WriteConsoleA to the extern directive 

.data                                ; Task: Define the .data section
szMsg       db  "I Love 64 Bit Windows Assembly", 0            ; Task: Define the bytes 'I Love 64 Bit Windows Assembly'
MSG_LEN equ $-szMsg                          ; Task: Calculate the size of the label szMsg

.data?                            ; Task: Define the section which creates a block of uninitialised data
BytesWritten dd  ?

.code                                ; Task: Define the code section
main PROC                                 ; Task: Define the start of the code using the label main with the PROC directive
    mov rcx, -11                          ; Task: Copy the handle to the standard output device to the register RCX
    call GetStdHandle                        ; Task: Call the function 'GetStdHandle' 

    push  0                       ; Task: Push the value '0' to the stack
    mov   r9, offset BytesWritten                       ; Task: Copy the offset of the BytesWritten label to the register R9
    mov   r8, MSG_LEN                       ; Task: Copy the label for the length of the message to the register R8
    mov   rdx, offset szMsg                      ; Task: Copy the offset of the label szMsg to the register RDX
    mov   rcx, rax                       ; Task: Copy the return value of GetStdHandle(located in RAX) to the register RCX
    call  WriteConsoleA                       ; Task: Call the function 'WriteConsoleA' with the amount of bytes for the parameters

    mov rcx, 0
    call    ExitProcess

main ENDP                                ; Task: Define the end of the code using the main with the ENDP directive
END 
```


#### Stack

The stack grows towards lower memory addresses. This is the reason why we create more space in the stack in the function prologue by subtracting from `rsp` as follows:
```
sub rsp, 40h
```

To reclaim this space and point `rsp` back to `rip`, we add to `rsp` as follows:
```
add rsp, 40h
ret
```

![[Pasted image 20230621234131.png]]

#### Shadow Space

- It is the area allocated on the stack by the calling function to store the parameters that are stored in the registers, which are normally stored in the registers.  It is managed by the calling function. 
- An example of shadow space is given below:
```cpp
sub rsp, 48h        ; Create space on the stack for shadow space 7*8 then 16-byte alligned then + 8 for return address
mov rax, 0         ; Move the last three parameters into general purpose registers
mov rbx, buf_LEN      ; Move the last three parameters into general purpose registers
mov r10, offset buf     ; Move the last three parameters into general purpose registers 
mov [rsp+30h], rax     ; Move the 7th parameter into the final place on the stack
mov [rsp+28h], rbx     ; Move the 6th parameter into the second to last place on the stack
mov [rsp+20h], r10     ; Move the 5th parameter into the third to last place on the stack
mov r9, 0          ; 4th parameter set up
mov r8, offset DateTimeInfo ; 3rd parameter set up
mov rdx, 0         ; 2nd parameter set up
mov rcx, 0         ; 1st parameter set up

call GetDateFormatEx

add rsp, 48h        ; Clean up the stack
```
