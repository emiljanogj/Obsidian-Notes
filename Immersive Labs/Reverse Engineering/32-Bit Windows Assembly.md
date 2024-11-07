- Useful Instructions
```nasm
call <location>  ; Change the execution flow to the location shown in <location> - Synonymous to a normal function call in programming
push <value>   ; Take the <value> and put it onto the local stack frame
db <bytes>  ; Define bytes: This is an instruction to masm to reserve bytes inside the section it is referenced
dd <bytes> ; Define dword: This is an instruction to masm to reserve bytes inside the section it is referenced
```

- The Assembly code below prints 'Hello World'
![[Pasted image 20230621005259.png]]
*Note*: `push -11` pushes the value -11 onto the stack, which represents the constant `STD_OUTPUT_HANDLE`. 
*Note*: `call GetStdHandle@4` is part of the Windows API, where the `@4` specifies that this function expects a single parameter to be passed on the stack. In this case a single 4-byte parameter(`STD_OUTPUT_HANDLE`) is expected.

### What Happens when We call the Windows API CreateFileA

![[Pasted image 20230621010154.png]]


![[Pasted image 20230621010213.png]]

![[Pasted image 20230621010232.png]]

### Example

We have the following Windows Assembly:
```
push 0                         
push offset BytesWritten                         
push MSG_LEN                        
push offset szMsg                         
push eax                        
call WriteConsoleA@20
```

1. `push 0`: Pushes the value `0` onto the stack. This value represents the handle to the standard output device (stdout). It is being pushed as the first argument to the `WriteConsoleA` function.
    
2. `push offset BytesWritten`: Pushes the address of the `BytesWritten` variable onto the stack. This variable will receive the number of bytes actually written to the console by the `WriteConsoleA` function. It is being pushed as the second argument.
    
3. `push MSG_LEN`: Pushes the value of `MSG_LEN` onto the stack. This value represents the length of the message to be written to the console. It is being pushed as the third argument.
    
4. `push offset szMsg`: Pushes the address of the `szMsg` string onto the stack. This string contains the message to be written to the console. It is being pushed as the fourth argument.
    
5. `push eax`: Pushes the value of `eax` register onto the stack. The purpose of this instruction depends on the context of the surrounding code. `eax` might contain additional arguments for the `WriteConsoleA` function.
    
6. `call WriteConsoleA@20`: Calls the `WriteConsoleA` function, passing the arguments pushed onto the stack in the previous instructions. The `@20` signifies that the function expects a total of 20 bytes (5 arguments * 4 bytes per argument) to be cleaned up from the stack after the call.

*Note*: The `offset` keyword is used to denote a memory address.

**Useful Instructions**
```nasm
mov <destination> <source>    ; Copy the value from the source location to the destination
dw    ; Declare word to reserve enough space for 16 bits at the location specified by the label next to it
db <number> DUP (<value>)    ; Declare bytes of quantity <number> duplicated from the <value> inside the brackets. A great way to declare an empty buffer
; for example, db 128 DUP (0) will declare 128 NULL bytes 
```

![[Pasted image 20230621143659.png]]

*Note*: In the above example, we push all the function arguments in the stack in the reverse order. Then, in the end when we call `GetDateFormatEx@28` we specify `28` the number of bytes, i.e. 7 arguments x 4 bytes/argument = 28 bytes.

**Generating .exe file**

- Generating .obj file
```
ml.exe /c /coff /Cp .\assembly.asm
```

- Linking libraries + no GUI(`SUBSYSTEM:console`)
```
link /SUBSYSTEM:console /defaultlib:"C:\Program Files (x86)\Windows Kits\10\Lib\10.0.18362.0\um\x86\kernel32.lib" .\assembly.obj
```


**Struct Example in Assembly**

![[Pasted image 20230621144257.png]]

#### Full Example: Printing the date
```nasm
.386
.model flat, stdcall 
option casemap:none

extern  ExitProcess@4:PROC           ; Task: Add ExitProcess with the correct amount of bytes for the arguments  
extern  GetStdHandle@4:PROC           ; Task: Add GetStdHandle with the correct amount of bytes for the arguments  
extern  WriteConsoleW@20:PROC           ; Task: Add WriteConsoleW with the correct amount of bytes for the arguments  
extern  GetSystemTime@4:PROC           ; Task: Add GetSystemTime with the correct amount of bytes for the arguments  
extern  GetDateFormatEx@28:PROC           ; Task: Add GetDateFormatEx with the correct amount of bytes for the arguments  

.data
buf db 128 DUP(0)                    ; Task: define bytes of a buffer size of 128 '0's
buf_LEN     equ $ - buf

LPSYSTEMTIME STRUCT                               ; Task: Initiate the struct with the keyword 'STRUCT'
wYear dw ?                                            ; Task: Define words called 'wYear' with a '?' value
wMonth dw ?                                            ; Task: Define words called 'wMonth' with a '?' value
wDayOfWeek dw ?                                            ; Task: Define words called 'wDayOfWeek' with a '?' value
wDay dw ?                                            ; Task: Define words called 'wDay' with a '?' value
wHour dw ?                                            ; Task: Define words called 'wHour' with a '?' value
wMinute dw ?                                            ; Task: Define words called 'wMinute' with a '?' value
wSecond dw ?                                            ; Task: Define words called 'wSecond' with a '?' value
wMilliseconds dw ?                                            ; Task: Define words called 'wMilliseconds' with a '?' value
LPSYSTEMTIME ENDS                    ; Task: END the struct with the keyword 'ENDS'
DateTimeInfo LPSYSTEMTIME  <>                               

.data?                          
BytesWritten dd  ?               
HANDLE      dd  ?

.code                                       
WinMain:                                 ; Task: Define 'WinMain' to start the main program
    push offset DateTimeInfo                         ; Task: Push the offset of the struct to the stack
    call  GetSystemTime@4                      ; Task: Call the function 'GetSystemTime' with the amount of bytes for the parameters

    push -11                        
    call    GetStdHandle@4
    mov   HANDLE, eax                       ; Task: Copy the return value located in 'eax' into the label 'HANDLE'
  
    push 0                              ; Task: Push the value '0' to the stack
    push buf_LEN                              ; Task: Push the label 'buf_LEN' to the stack 
    push offset buf                             ; Task: Push the offset of the label 'buf' to the stack
    push 0                             ; Task: Push the value '0' to the stack
    push offset DateTimeInfo                             ; Task: Push the offset of the struct 'DateTimeInfo' to the stack
    push 0                             ; Task: Push the value '0' to the stack
    push 0                             ; Task: Push the value '0' to the stack
    call GetDateFormatEx@28                              ; Task: Call the function 'GetDateFormatEx' with the amount of bytes for the parameters

    push 0                             ; Task: Push the value '0' to the stack
    push offset BytesWritten                             ; Task: Push the offset of the label 'BytesWritten' to the stack
    push eax                             ; Task: Push the register eax to the stack which contains the return value from 'GetDateFormatEx'
    push offset buf                             ; Task: Push the offset of the label 'buf' to the stack
    push HANDLE                             ; Task: Push the label 'HANDLE' to the stack
    call WriteConsoleW@20                              ; Task: Call the function 'WriteConsoleW' with the amount of bytes for the parameters

    push 0
    call    ExitProcess@4
end WinMain 
```

