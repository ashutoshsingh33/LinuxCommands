A monolithic kernel is a kernel where all services (file system, VFS, device drivers, etc) as well as core functionality (scheduling, memory allocation, etc.) are a tight knit group sharing the same space. This directly opposes a microkernel.

A microkernel prefers an approach where core functionality is isolated from system services and device drivers (which are basically just system services). For instance, VFS (virtual file system) and block device file systems (i.e. minixfs) are separate processes that run outside of the kernel's space, using IPC to communicate with the kernel, other services and user processes. In short, if it's a module in Linux, it's a service in a microkernel, indicating an isolated process.

Do not confuse the term modular kernel to be anything but monolithic. Some monolithic kernels can be compiled to be modular (e.g Linux), what matters is that the module is inserted to and runs from the same space that handles core functionality (kernel space).

The advantage to a microkernel is that any failed service can be easily restarted, for instance, there is no kernel halt if the root file system throws an abort. This can also be seen as a disadvantage, though, because it can hide pretty critical bugs (or make them seem not-so-critical, because the problem seems to continuously fix itself). It's seen as a big advantage in scenarios where you simply can't conveniently fix something once it has been deployed.

The disadvantage to a microkernel is that asynchronous IPC messaging can become very difficult to debug, especially if fibrils are implemented. Additionally, just tracking down a FS/write issue means examining the user space process, the block device service, VFS service, file system service and (possibly) the PCI service. If you get a blank on that, its time to look at the IPC service. This is often easier in a monolithic kernel. GNU Hurd suffers from these debugging problems (reference). I'm not even going to go into checkpointing when dealing with complex message queues. Microkernels are not for the faint of heart.

The shortest path to a working, stable kernel is the monolithic approach. Either approach can offer a POSIX interface, where the design of the kernel becomes of little interest to someone simply wanting to write code to run on any given design.

I use Linux (monolithic) in production. However, most of my learning, hacking or tinkering with kernel development goes into a microkernel, specifically HelenOS.

## Edit

If you got this far through my very long-winded answer, you will probably have some fun reading the 'Great Torvalds-Tanenbaum debate on kernel design'. It's even funnier to read in 2013, more than 20 years after it transpired. The funniest part was Linus' signature in one of the last messages:

Linus "my first, and hopefully last flamefest" Torvalds

Obviously, that did not come true any more than Tanenbaum's prediction that x86 would soon be obsolete.

NB:

When I say "Minix", I do not imply Minix 3. Additionally, when I mention The HURD, I am referencing (mostly) the Mach microkernel. It is not my intent to disparage the recent work of others. 
