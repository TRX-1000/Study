# Operating System Concepts — Notes Mapped to Silberschatz, Galvin & Gagne, 9th Edition

*Structured to follow the book's own chapter/section numbering so you can read the original text alongside this. Page numbers refer to the 9th edition.*

---

# Chapter 1 — Introduction

## 1.1 What Operating Systems Do (p.4)

The book presents two complementary views of an OS:

**User view** — what the OS looks like depends on the type of system:
- On most PCs, users sit at a keyboard/mouse-driven screen; the OS is designed for *ease of use*, with performance a secondary concern and resource utilization barely a concern at all (single user, dedicated hardware).
- On systems where users interact through networked terminals sharing one large machine (mainframe/minicomputer style), the OS is designed to maximize **resource utilization** — ensuring CPU time, memory, and I/O are shared fairly and efficiently among many users.
- On handheld/embedded devices, there's minimal or no user visibility of the OS at all.

**System view** — from the computer's own point of view, the OS is the program most intimately involved with the hardware. It can be seen as:
- A **resource allocator** — the OS manages all resources (CPU, memory, storage, I/O) and decides between conflicting requests for efficient/fair use.
- A **control program** — the OS manages execution of user programs to prevent errors and improper use of the computer.

The book also gives a minimalist definition worth quoting the *idea* of (not the exact wording): the one program running at all times on the computer is the **kernel**; everything else is either a **system program** or an **application program**.

## 1.2 Computer-System Organization (p.7)

A general-purpose system = one or more CPUs + device controllers, connected through a common **bus** providing access to shared memory.

- Each **device controller** is in charge of one specific device type and has its own local buffer.
- The CPU moves data between main memory and its internal registers; device controllers move data between the device and their local buffer.
- **I/O is performed via interrupts**: the device driver loads registers within the device controller; the device controller examines the contents and takes appropriate action; once done, it triggers an interrupt to let the driver know.

**Interrupts (Section 1.2.1):**
- The CPU hardware has an interrupt-request line sensed after every instruction.
- On seeing a signal, the CPU saves the interrupted instruction's address and jumps to a fixed location containing the interrupt-handling routine.
- The book describes **interrupt chaining**, where each element in an interrupt-vector table points to the head of a list of handlers for that interrupt number, since modern systems have far more devices than vector-table entries.
- **Traps (or exceptions)** are software-generated interrupts, caused either by an error or a specific user-program request (a system call).

**Storage Structure (Section 1.2.2):**
- **Main memory** is the only large storage area the CPU can access directly (via load/store instructions to registers) — it's random-access and typically volatile.
- **Secondary storage** extends main memory — non-volatile, large capacity. Magnetic disks are the traditional bulk secondary storage; SSDs are increasingly common (faster, more expensive, no moving parts).

**I/O Structure (Section 1.2.3):**
- The book distinguishes **synchronous I/O** — after an I/O starts, control returns to the user program only upon completion — from **asynchronous I/O**, where control returns immediately without waiting, letting the process continue other work while I/O proceeds concurrently.
- **DMA (Direct Memory Access)** is used for devices that transfer large blocks of data (like disks): after set-up, the device controller transfers an entire block directly to/from memory, generating one interrupt per block rather than the byte-level interrupt overhead you'd get otherwise.

## 1.3 Computer-System Architecture (p.12)

- **1.3.1 Single-Processor Systems**: one main general-purpose CPU. Special-purpose processors may exist (e.g., disk controllers) but run limited instruction sets and don't count for classification.
- **1.3.2 Multiprocessor Systems**: also called **parallel systems** or **tightly coupled systems**, having two or more processors in close communication, sharing the bus, clock, and sometimes memory/peripherals.

  Advantages the book lists:
  1. **Increased throughput** — though N processors don't give exactly N× speedup, due to overhead in keeping all parts working correctly (coordination costs).
  2. **Economy of scale** — cheaper than multiple single-processor systems since peripherals, storage, and power can be shared.
  3. **Increased reliability** — functions can be distributed among several processors; failure of one doesn't halt the system, only slows it. This is called **graceful degradation**; systems designed this way are **fault tolerant**.

  Two types (Section 1.3.2):
  - **Asymmetric multiprocessing**: each processor is assigned a specific task; a boss processor schedules and allocates work to worker processors. This is more common in embedded/special-purpose systems.
  - **Symmetric multiprocessing (SMP)**: each processor performs all tasks within the OS — no boss–worker relationship exists; all processors are peers. The book notes virtually all modern OSes support SMP.

  The book also covers **multicore systems**, where multiple computing cores reside on a single chip — faster and consuming less power than multiple chips each with one core, and on-chip communication is faster than bus communication between separate chips.

- **1.3.3 Clustered Systems**: composed of two or more individual systems (nodes) coupled together, sharing storage and connected via a local-area network (LAN) or a faster interconnect like InfiniBand.
  - Provides **high availability** — if one node fails, another can take over its workload.
  - **Asymmetric clustering**: one machine is in **hot-standby mode**, monitoring the active server; if the active server fails, the standby takes over.
  - **Symmetric clustering**: two or more nodes run applications and monitor each other — more efficient as no hardware sits idle, but more complex.
  - Some clusters are used for **high-performance computing (HPC)**, requiring applications specifically written using techniques like **parallelization** to take advantage of it.

## 1.4 Operating-System Structure (p.19)

The book introduces **multiprogrammed** and **timeshared** systems here:

- **Multiprogramming** increases CPU utilization by organizing jobs so the CPU always has something to execute. A subset of total jobs in the system is kept in memory; one is picked and run via **job scheduling**. When it must wait, the OS switches to another job.
- **Timesharing (multitasking)** is a logical extension: the CPU switches jobs so frequently users can interact with each program while it runs, creating interactive computing. Response time should be under 1 second. Each user has at least one program in memory called a **process**. If several processes are ready to run, **CPU scheduling** decides which runs next. **Virtual memory** allows execution of processes not completely in memory.

## 1.5 Operating-System Operations (p.21)

- **Dual-mode operation**: to ensure proper execution, the OS must be able to distinguish between execution on behalf of the OS and execution on behalf of the user. Hardware provides at least two modes: **user mode** and **kernel mode (also: supervisor, system, or privileged mode)**.
  - A **mode bit** is added to hardware: 0 = kernel mode, 1 = user mode.
  - At system boot, hardware starts in kernel mode; the OS is loaded and starts user processes in user mode. Whenever a trap or interrupt occurs, hardware switches from user to kernel mode (i.e., changes the bit to 0).
  - **Privileged instructions** (I/O control, timer management) can be issued only in kernel mode; attempting to execute one in user mode causes the hardware to treat it as illegal and trap to the OS.
  - The book also notes many contemporary CPUs support **multimode** operation, e.g., a virtual machine manager (VMM) may have its own mode, with even more privileges than the user process mode but fewer than kernel mode, to indicate it can do almost everything the kernel can *except* interact directly with hardware.
- The System Call section (1.5, continued) previews that user processes must be able to request the OS to do privileged tasks on their behalf — this is elaborated fully in Chapter 2.

## Sections 1.6–1.8 (Process, Memory, Storage Management — overview level)

These are previews of Chapters 3, 8–9, and 10–12 respectively. Briefly:
- **1.6 Process Management**: a process needs CPU time, memory, files, I/O devices to accomplish its task. The OS is responsible for creating/deleting processes, suspending/resuming, providing synchronization/communication mechanisms.
- **1.7 Memory Management**: main memory is a large array of bytes, each with an address; the OS keeps track of which parts are in use and by whom, decides which processes/data to move in/out of memory, allocates/deallocates memory space as needed.
- **1.8 Storage Management**: the OS provides a uniform, logical view of information storage via the **file** abstraction — mapping files onto physical media, and managing storage on secondary and tertiary storage.

## 1.9 Protection and Security (p.30)

- **Protection** is any mechanism for controlling access of processes/users to resources.
- **Security** is defense of the system against internal and external attacks (denial-of-service, worms/viruses, identity theft, theft of service).

---

# Chapter 2 — System Structures

## 2.1 Operating-System Services (p.53)

The book groups OS services into two sets:

**Services for helping the user:**
1. **User interface** — CLI, GUI, or batch.
2. **Program execution** — load a program into memory, run it, end execution (normally or abnormally, indicating error).
3. **I/O operations** — a running program may require I/O involving a file or device.
4. **File-system manipulation** — read/write files/directories, create/delete, search, list, permissions.
5. **Communications** — exchange information between processes on the same computer or over a network, via **shared memory** or **message passing**.
6. **Error detection** — errors may occur in the CPU/memory hardware, I/O devices, or user programs; the OS must take appropriate action for each type to ensure correct/consistent computing.

**Services for ensuring efficient operation of the system:**
7. **Resource allocation** — when multiple users/jobs run concurrently, resources must be allocated among them.
8. **Accounting** — track which users use how much and what kinds of resources.
9. **Protection and security** — owners of information in a multiuser system may want to control its use; when several processes execute concurrently, one shouldn't interfere with others; security requires authenticating users to protect against outside intrusion.

## 2.2 User and Operating-System Interface (p.56)

- **Command interpreters (CLI)** — a program that gets and executes the next user-specified command; can be implemented in the kernel itself, or (more commonly) as a system program fetched at boot / when a user session begins.
- **GUI** — provides a mouse-based window-and-menu system; the book notes historically GUIs came later (Xerox), and modern systems often let a user switch between both interfaces.

## 2.3 System Calls (p.60)

- **System calls** provide an interface to the services made available by the OS — typically written in C/C++, though some low-level tasks are written in assembly.
- Programmers usually don't invoke system calls directly, but through a high-level **Application Programming Interface (API)** (e.g., Windows API, POSIX API, Java API) — the book explains that the run-time support system for most programming languages provides a **system-call interface** that intercepts function calls in the API and invokes the necessary actual system call.
- The book illustrates this with the example of the standard C library `read()` function acting as a stub that invokes the associated system call within the OS.
- **Parameter passing** — three general methods: (1) pass parameters in registers, (2) store parameters in a block/table in memory and pass the block's address in a register, (3) push parameters onto a stack, popped by the OS.

## 2.4 Types of System Calls (p.64)

Six broad categories, per the book:

1. **Process control** — create/terminate process, load/execute, get/set process attributes, wait for time/event, signal event, allocate/free memory.
2. **File management** — create/delete file, open/close, read/write/reposition, get/set file attributes.
3. **Device management** — request/release device, read/write/reposition, get/set device attributes, logically attach/detach devices.
4. **Information maintenance** — get/set time or date, get/set system data, get/set process/file/device attributes.
5. **Communications** — create/delete communication connection, send/receive messages, transfer status information, attach/detach remote devices.
6. **Protection** — get/set file/resource permissions, allow/deny user access.

## 2.5 System Programs (p.72)

System programs (also called **system utilities**) provide a convenient environment for program development/execution. Categories: file management, status information, file modification, programming-language support, program loading/execution, communications, background services. Most users' view of the OS is actually defined by system programs, not the actual system calls.

## 2.6 Operating-System Design and Implementation (p.73)

- **Design goals**: user goals (convenient, easy to use/learn, reliable, safe, fast) vs. system goals (easy to design/implement/maintain, flexible, reliable, error-free, efficient).
- **Mechanism vs. policy**: a key design principle — **mechanisms** determine *how* to do something; **policies** decide *what* will be done. Separating the two gives flexibility, since policies are likely to change across places/times, while the underlying mechanism stays fixed. The book gives the example of a timer mechanism (ensuring CPU protection) being separate from the policy decision of how long a time slice should be.

## 2.7 Operating-System Structure (p.76)

- **2.7.1 Simple Structure (Monolithic)**: e.g., original MS-DOS — the whole OS is written as a collection of components, but interfaces/levels of functionality aren't well separated; UNIX is similarly described as originally consisting of two separable parts: the kernel and the system programs, with the kernel itself further separated into a series of interfaces and device drivers.
- **2.7.2 Layered Approach**: the OS is broken into layers, each built on top of lower layers; the bottom layer (0) is the hardware, the highest is the user interface. Each layer uses only functions/services of lower-level layers, simplifying debugging/verification (each layer implemented using only lower, already-tested layers) — but layers must be defined carefully since a layer can only use lower layers, and this can be less efficient than other approaches (a user request may pass through many layers).
- **2.7.3 Microkernels**: structures the OS by removing all nonessential components and implementing them as user-level programs, resulting in a smaller kernel. The book cites Mach as a well-known example. Main function of the microkernel is to provide **communication between the client program and the various services**, also running in user space, via **message passing**. Benefits: easier to extend, easier to port to new architectures, more reliable (less code running in kernel mode) and secure. Main disadvantage is performance overhead of user-space-to-user-space communication.
- **2.7.4 Modules**: perhaps the best current OS-design methodology, using **loadable kernel modules** — the kernel has a set of core components and links in additional services dynamically, either at boot or during run time. This design is common in modern implementations of UNIX (e.g., Solaris, Linux, macOS). It's similar to layers in that each core component is separately implemented, but more flexible than a layered system since any module can call any other module — combining the benefits of the layered and microkernel approaches while avoiding their difficulties.
- **2.7.5 Hybrid Systems**: in practice, very few OSes adopt a single, strictly-defined structure — most combine elements of different structures (e.g., the book cites Linux as monolithic plus modules, and macOS/iOS as layered with Mach microkernel components).

## 2.10 System Boot (p.90)

- The mechanism by which the first code the CPU executes at power-up/reboot is the **bootstrap program**, stored in ROM/EEPROM (**firmware**) so it doesn't need to rely on a disk-based OS or a disk device driver.
- It initializes all aspects of the system, from CPU registers to device controllers to memory, and then finds and loads the OS kernel into memory, and starts its execution. Some systems (like PCs) use a two-step process where a small bootstrap loader disk block is loaded first, whose only job is to load the full bootstrap program from disk.

---

# Chapter 3 — Process Concept

## 3.1 Process Concept (p.103)

- **3.1.1**: A process is defined as **a program in execution**. A batch system executes *jobs*; a time-shared system has *user programs* or *tasks* — the book uses "process" interchangeably with these but generally with the connotation of a program in execution in a time-shared context.
- A **program** is a *passive* entity, like the contents of a file stored on disk; a **process** is an *active* entity, with a program counter specifying the next instruction to execute and a set of associated resources. A program becomes a process when an executable file is loaded into memory. Note: two processes may be associated with the same program (e.g., several users running the same mail program), each considered a separate execution sequence — the **text section** is the same, but the data, heap, and stack sections vary.

**Process in Memory (Section 3.1.2, Figure 3.1)**:
```
max
+-------------------+
|      Stack         |   (grows downward)
|         |            |
|         v            |
|                     |
|         ^            |
|         |            |
|      Heap           |   (grows upward)
+-------------------+
|      Data           |
+-------------------+
|      Text           |
+-------------------+
0
```
- **Text section**: the executable code.
- **Data section**: global variables.
- **Heap**: memory dynamically allocated during process run time.
- **Stack section**: temporary data storage used when calling functions — parameters, return addresses, local variables.
The sizes of the text and data sections are fixed (they don't change size during the program's run time), whereas the stack and heap can shrink/grow dynamically as the program executes.

**Process State (Section 3.1.3)**: as a process executes, it changes state:
- **New**: process being created.
- **Running**: instructions being executed.
- **Waiting**: process waiting for some event to occur.
- **Ready**: process waiting to be assigned to a processor.
- **Terminated**: process finished execution.
Only one process can be *running* on any processor core at any instant, though many may be *ready* or *waiting*.

**Process Control Block (Section 3.1.4, Figure 3.3)**: each process is represented by a PCB (also called a **task control block**), containing:
- Process state
- Program counter
- CPU registers (accumulators, index registers, stack pointers, general-purpose registers, condition-code info) — must be saved when an interrupt occurs so the process can be correctly continued afterward
- CPU-scheduling information — priority, pointers to scheduling queues, other scheduling parameters
- Memory-management information — base/limit register values, page/segment tables
- Accounting information — CPU/real time used, time limits, account numbers, job/process numbers
- I/O status information — list of I/O devices allocated, list of open files

## 3.2 Process Scheduling (p.108)

- **3.2.1 Scheduling Queues**: 
  - **Job queue**: as processes enter the system, they're put into this queue, consisting of all processes in the system.
  - **Ready queue**: processes residing in main memory, ready and waiting to execute — generally stored as a linked list; a ready-queue header contains pointers to the first and last PCBs.
  - **Device queues**: list of processes waiting for a particular I/O device.
  The book represents this with a **queueing diagram** — each rectangle a queue, circles representing resources serving the queues, arrows the flow of processes.

- **3.2.2 Schedulers**:
  - **Long-term scheduler (job scheduler)**: selects processes from this pool and loads them into memory for execution. Executes much less frequently — minutes may separate creation of one new process and the next; controls the **degree of multiprogramming** (number of processes in memory).
  - **Short-term scheduler (CPU scheduler)**: selects among processes ready to execute, allocating the CPU. Invoked very frequently — must be fast (e.g., if it takes 10ms to decide to execute a process for 100ms, 10/(100+10) = 9% of CPU time is being wasted purely on scheduling).
  - The book notes that in some systems, the long-term scheduler may be absent or minimal (e.g., time-sharing systems like UNIX/Windows often have no long-term scheduler at all — simply put every new process into memory for the short-term scheduler).
  - **I/O-bound vs. CPU-bound processes**: an I/O-bound process spends more time doing I/O than computations, with many short CPU bursts; a CPU-bound process spends more time doing computations, with few very long CPU bursts. It's important the long-term scheduler selects a good process mix.
  - **Medium-term scheduler**: some systems, particularly time-sharing ones, introduce this additional intermediate level. The key idea: sometimes it's advantageous to remove a process from memory (and from active contention for the CPU), reducing the degree of multiprogramming — this is called **swapping**. Later, the process is reintroduced and its execution continued from where it left off.

- **3.2.3 Context Switch**: switching the CPU to another process requires saving the state of the old process and loading the saved state of the new — this task is the **context switch**. Context-switch time is pure overhead, since the system does no useful work while switching; the more complex the OS and PCB, the longer this takes. Speed varies from hardware to hardware, based on memory speed, the number of registers to copy, and existence of special instructions.

## 3.3 Operations on Processes (p.113)

- **3.3.1 Process Creation**: a process may create several new processes via a **create-process** system call, forming a tree of processes — the **parent process** creates **child processes**, which in turn create their own children. Each process is identified by a unique **process identifier (pid)**.

  When a process creates a child:
  - **Resource sharing** options: (1) parent and child share all resources, (2) child shares a subset of parent's resources, (3) parent and child share no resources.
  - **Execution** options: (1) parent and children execute concurrently, (2) parent waits until some/all children terminate.
  - **Address space** options: (1) child duplicate of parent (same program/data), (2) child has a new program loaded into it.

  The book uses the UNIX example in detail:
  - `fork()` creates a new process consisting of a copy of the address space of the original — allows easy communication between parent and child.
  - Both processes continue execution at the instruction after the `fork()`, with one difference: for the (newly created) child process, the return value of `fork()` is **0**; for the parent, it's the (nonzero) process ID of the child.
  - Typically, the `exec()` system call is used **after** a `fork()` by one of the two processes to replace the process's memory space with a new program — this allows the two processes to communicate and then go their separate ways.
  - The parent can create more children, or, if nothing else to do while the child runs, use the `wait()` system call to move itself off the ready queue until the child terminates.

- **3.3.2 Process Termination**: a process terminates after executing its final statement, asking the OS to delete it using the `exit()` system call — at that point the process may return a status value (typically an integer) to its parent via `wait()`. All resources are deallocated by the OS.

  A parent may terminate a child for reasons such as: the child exceeded its usage of some allocated resources, the task assigned to the child is no longer required, the parent is exiting and the OS doesn't allow a child to continue if its parent terminates (**cascading termination**, initiated by the OS).

  The book discusses **zombie** processes: when a process terminates, its resources are deallocated by the OS, but its entry in the process table must remain until the parent calls `wait()`, because the process table contains the process's exit status. A process that has terminated but whose parent hasn't yet called `wait()` is a **zombie process**. If a parent terminates without invoking `wait()`, its children (now parentless) become **orphan processes** — most operating systems handle this by having the init process (in UNIX) or a designated system process periodically invoke `wait()` to collect exit status and remove orphans, preventing them from remaining zombies indefinitely.

---

# Chapter 5 — Process Scheduling
*(Note: In the 9th edition, CPU scheduling is Chapter 5, since Chapter 4 covers Multithreaded Programming — which your syllabus doesn't list separately, but is referenced implicitly wherever "thread" comes up.)*

## 5.1 Basic Concepts (p.201)

- In a system with a single CPU core, only one process can run at a time; others must wait until the CPU is free.
- **The objective of multiprogramming** is to have some process running at all times, to maximize CPU utilization.
- **CPU–I/O Burst Cycle**: process execution consists of a cycle of CPU execution and I/O wait — processes alternate between these two states. Execution begins with a **CPU burst**, followed by an **I/O burst**, then another CPU burst, and so on — the final CPU burst ends with a system request to terminate execution.
- The book notes an I/O-bound program would typically have many short CPU bursts; a CPU-bound program might have a few long CPU bursts. This distribution can help select an appropriate CPU-scheduling algorithm.
- **5.1.1 CPU Scheduler**: whenever the CPU becomes idle, the OS must select a process from the ready queue to execute — this selection is carried out by the **short-term scheduler (CPU scheduler)**. Note the ready queue isn't necessarily FIFO — depending on the scheduling algorithm, it could be a FIFO queue, priority queue, tree, or even an unordered linked list.
- **5.1.2 Preemptive and Nonpreemptive Scheduling**: CPU-scheduling decisions may take place under four circumstances:
  1. When a process switches from the running state to the waiting state.
  2. When a process switches from the running state to the ready state (e.g., interrupt occurs).
  3. When a process switches from the waiting state to the ready state.
  4. When a process terminates.

  Under circumstances 1 and 4, there's **no choice** in terms of scheduling — a new process (if one exists in the ready queue) must be selected. Under 2 and 3, there **is** a choice.

  - When scheduling takes place only under 1 and 4, the scheme is **nonpreemptive** (or **cooperative**) — once the CPU is allocated to a process, the process keeps it until it releases the CPU either by terminating or switching to the waiting state.
  - Under all other circumstances, the scheme is **preemptive**.
  - The book notes preemptive scheduling can result in **race conditions** when data is shared among several processes — needing careful synchronization (foreshadowing Chapter 6).
  - Preemption also affects the design of the OS kernel — while the kernel is busy servicing one system call, on behalf of a process, is another interrupt/system call allowed to change important kernel data? The book discusses how this is handled differently across kernel designs.

- **5.1.3 Dispatcher**: the **dispatcher** is the module that gives control of the CPU to the process selected by the short-term scheduler. This function involves:
  - Switching context
  - Switching to user mode
  - Jumping to the proper location in the user program to restart it
  The dispatcher should be as fast as possible, since it's invoked during every process switch; the time it takes is called **dispatch latency**.

## 5.2 Scheduling Criteria (p.205)

Many criteria have been suggested for comparing CPU-scheduling algorithms, and the choice of which to use can make a substantial difference in which algorithm is judged best:
- **CPU utilization** — keep the CPU as busy as possible; conceptually ranges from 0-100%. In a real system it should range from 40% (lightly loaded) to 90% (heavily loaded).
- **Throughput** — number of processes completed per unit time.
- **Turnaround time** — the interval from the time of submission of a process to the time of completion. It's the sum of periods spent waiting to get into memory, waiting in the ready queue, executing on the CPU, and doing I/O.
- **Waiting time** — the sum of periods spent waiting in the ready queue. The book notes the CPU-scheduling algorithm doesn't affect the *amount of time* a process spends executing or doing I/O — it only affects the *waiting time* a process spends in the ready queue, so this is often the metric algorithms specifically try to minimize.
- **Response time** — for interactive systems, turnaround time isn't the best criterion, since a process can produce output early and continue computing new results while previous results are output to the user. Response time is the time from submission of a request until the *first* response is produced — measures how long it takes to *start* responding, not how long it takes to output the full response.

Generally, average CPU utilization and throughput should be **maximized**, while average turnaround time, waiting time, and response time should be **minimized**. In most cases, the average measure is optimized, though under some circumstances the minimum/maximum values are preferred (e.g., for a good interactive system, minimizing the *variance* in response time is more important than minimizing the average response time — a system that is reasonably predictable is often preferred even if slower on average).

## 5.3 Scheduling Algorithms (p.206)

**5.3.1 First-Come, First-Served (FCFS) Scheduling**: the process that requests the CPU first is allocated the CPU first, managed with a FIFO queue. Simple to write/understand, but average waiting time is often quite long, and the algorithm is nonpreemptive. The book illustrates the **convoy effect** with an example: consider one CPU-bound process and many I/O-bound processes — the I/O-bound processes must wait for the big CPU-bound process to get off the CPU, resulting in lower CPU and device utilization than might be possible if the shorter processes were allowed to go first.

**5.3.2 Shortest-Job-First (SJF) Scheduling**: associates with each process the length of its next CPU burst, using these lengths to schedule the process with the shortest time. If two processes have the same length next CPU burst, FCFS is used to break the tie. Note the more appropriate term is **shortest-next-CPU-burst**, since scheduling depends on the length of the *next* burst, not the total length of the process.

The book proves SJF is **optimal** — it gives the minimum average waiting time for a given set of processes, since moving a short process before a long one decreases the waiting time of the short process more than it increases the waiting time of the long process, hence decreasing the average waiting time.

The real difficulty is *knowing the length of the next CPU request*. The book presents this as approximated by using the length of previous CPU bursts, via **exponential averaging**:

τ(n+1) = α·t(n) + (1 − α)·τ(n)

where t(n) = actual length of the nth CPU burst, τ(n+1) = predicted value for the next CPU burst, and 0 ≤ α ≤ 1 is a control constant. Setting α = 0 means recent history has no effect (τ(n+1) = τ(n)); setting α = 1 means only the most recent burst matters (τ(n+1) = t(n)). More commonly, α = 1/2, weighting recent and past history equally. Expanding the formula shows each successive term has less weight than its predecessor — this is why it's called *exponential* averaging.

SJF can be either preemptive or nonpreemptive:
- If a new process arrives at the ready queue while a previous process is still executing, and the new process's next CPU burst is shorter than what's left of the currently executing process, a **preemptive SJF** algorithm will preempt the currently running process. This is also known as **Shortest-Remaining-Time-First (SRTF)**.
- **Nonpreemptive SJF** will allow the currently running process to finish its CPU burst.

**5.3.3 Priority Scheduling**: a priority is associated with each process, and the CPU is allocated to the process with the highest priority (the book notes equal-priority processes are scheduled in FCFS order). Note SJF is a special case of general priority scheduling, where priority (p) is the inverse of the (predicted) next CPU burst. Priorities can be defined either internally (using measurable quantities like time limits, memory requirements, number of open files) or externally (set by criteria outside the OS, like importance of the process, funds paid for computer use, department sponsoring the work).

Priority scheduling can be preemptive or nonpreemptive.

**Major problem: indefinite blocking (starvation)** — a low-priority process may wait indefinitely for the CPU, as higher-priority processes continue to be added. The book cites a real case: an IBM 7094 system at MIT shut down in 1973, and upon search, a process was found that had been submitted in 1967 and had not yet been run, having been continuously bumped by higher-priority incoming processes.

**Solution: aging** — a technique of gradually increasing the priority of processes that wait in the system for a long time, guaranteeing eventual execution.

**5.3.4 Round-Robin Scheduling**: designed especially for time-sharing systems — similar to FCFS but with preemption added to switch between processes. A small unit of time, the **time quantum (time slice)**, is defined, generally 10-100 milliseconds. The ready queue is treated as a circular queue; the CPU scheduler goes around it, allocating the CPU to each process for a time interval of up to 1 time quantum.

Implementation: the ready queue is a FIFO queue of processes; new processes are added at the tail. The CPU scheduler picks the first process, sets a timer to interrupt after 1 time quantum, and dispatches it.
- If the process's CPU burst is ≤ the time quantum, the process itself releases the CPU voluntarily; the scheduler then proceeds to the next process.
- If the CPU burst is longer, the timer goes off and causes an interrupt, causing a context switch, with the process put at the *tail* of the ready queue.

The book emphasizes performance depends heavily on the size of the time quantum:
- If the quantum is very large, RR is the same as FCFS.
- If the quantum is very small, RR is called **processor sharing** and appears (to users) as though each of N processes has its own processor running at 1/N the speed of the real processor. But in practice, a small quantum causes a large number of context switches — increasing overhead — so quantum size should be large relative to context-switch time, but not so large that response time to short interactive requests suffers.
- The book's rule of thumb: 80% of CPU bursts should be shorter than the time quantum.

**5.3.5 Multilevel Queue Scheduling**: used when processes can be readily categorized into different groups (e.g., **foreground / interactive** processes vs. **background / batch** processes, which have different response-time requirements and thus different scheduling needs). The **multilevel queue scheduling** algorithm partitions the ready queue into several separate queues, and processes are permanently assigned to one queue, generally based on some property like memory size, process priority, or process type.

Each queue has its own scheduling algorithm — e.g., foreground queue might use RR, background queue FCFS.

In addition, there must be scheduling **between the queues**, commonly implemented as fixed-priority preemptive scheduling — e.g., the foreground queue may have absolute priority over the background queue. An example the book gives: five queues, ordered by priority (system processes, interactive processes, interactive editing processes, batch processes, student processes) — each queue has absolute priority over lower-priority queues (no process in the batch queue could run unless the queues for system, interactive, and interactive-editing processes were all empty). If an interactive process entered the ready queue while a batch process was running, the batch process would be preempted.

Another possibility is **time-slicing between the queues** — each queue gets a certain portion of CPU time to schedule among its processes (e.g., foreground queue given 80% of CPU time for RR scheduling among its processes, background queue given 20% for FCFS).

**5.3.6 Multilevel Feedback Queue Scheduling**: normally, in multilevel queue scheduling, processes are permanently assigned to a queue when they enter the system, and don't move between queues. Since this setup has the advantage of low scheduling overhead but is inflexible, the book introduces the **multilevel feedback queue scheduling** algorithm, which allows a process to move between queues — separating processes with different CPU-burst characteristics. If a process uses too much CPU time, it's moved to a lower-priority queue; this scheme leaves I/O-bound and interactive processes (with short CPU bursts) in higher-priority queues. Similarly, a process that waits too long in a lower-priority queue may be moved to a higher-priority queue — a form of **aging** that prevents starvation.

The book describes a multilevel feedback queue scheduler defined by these parameters:
- The number of queues
- The scheduling algorithm for each queue
- The method used to determine when to upgrade a process to a higher-priority queue
- The method used to determine when to demote a process to a lower-priority queue
- The method used to determine which queue a process will enter when it needs service

The multilevel feedback queue is the most general CPU-scheduling algorithm — it can be configured to match a specific system under design, but it's also the most complex, requiring some means of selecting values for all the parameters to define the best scheduler.

---

# Topics in Your Syllabus Not Covered in Silberschatz Ch. 1/2/3/5

**Shell Programming (variables, control flow, shell scripts, cron)** is not a topic in Silberschatz at all — it's practical UNIX/Linux shell scripting, usually taught as a separate lab/practical component alongside the OS theory course, often from a UNIX shell scripting reference rather than the OS textbook. I've kept the shell-programming section from the earlier notes doc as-is, since the book doesn't apply there — happy to build out runnable examples in your Docker container separately.

---

## A note on citations and further reading

Everything above is my own explanation of the book's concepts and structure — not reproduced text — so that I stay within a strict 15-word/one-quote-per-source copyright limit. If there's a specific paragraph, definition, or example from the book you want me to walk through more closely (e.g., a specific figure, or the exact wording of a definition for exam purposes), tell me the page/section and I'll explain that particular passage in detail without quoting it directly — or you can read it straight from your PDF alongside my explanation.
