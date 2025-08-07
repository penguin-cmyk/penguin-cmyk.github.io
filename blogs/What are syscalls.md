If you're in any sort of cheating community you have probably heard the term `syscall` before, often associated with wpm (write process memory) and rpm (read process memory). But what are they, why are they talked about so much and what do they actually do?

### What are they

`syscall`, as the name suggests, refers to a system call, a way for user-mode applications to request services from the specific operating system kernel. In particular, `syscall` is an instruction available in the x86_64 architecture.

You might recognize something like `int $0x80` or `int 0x2e` from a TikTok or a YouTube short about assembly. These are software interrupts which are used on a 32-bit operating system to make system calls. 

A software interrupt causes the CPU to stop executing the current user-mode code and transfer control to the kernel to handle the request. Once the system call is completed, control back to the user process

In contrast, 64-bit systems use the more efficient `syscall` or `sysenter` instruction instead. 

### Why are the so efficient 

Since I keep talking about them being so efficient, I think we would need to address why. 

`syscall` and `sysenter` are more efficient than older interrupt based methods like `int 0x80` because they are specifically designed for fast transitions between user mode and kernel mode. They avoid the overhead of a normal interrupt handling by using pre set CPU registers to jump directly into the kernel.  

### Why do we use them

In user-mode anti-cheats or anti-tamper, raw `syscall` instructions are often used to read and write memory because they can sneak past the usual Windows APIs like `ReadProcessMemory`. Normally when a program calls RPM, it eventually goes trough  a chain of calls that lead into `NtReadVirtualMemory` in `ntdll.dll`, which itself makes a system call to the kernel. But since these "APIs" are well-known and commonly hooked or monitored by (um) anti-cheats, relying on them can easily get flagged.

By using raw syscalls, you're basically invoking the system call number and jumping straight into kernel mode where the program avoids the um layers that are frequently patched or intercepted. 

In other words, while `RPM` and `NtReadVirtualMemory` both perform syscalls under the hood, calling them trough the usual API means going trough monitored / hooked functions. Raw syscalls cut out these out and talk straight to the kernel. 
