## *Chapter 1*

**==Patterns==** -> Valuable algorithimic structures that are commonly seen in efficient parallel programs.
- Algorithm skeletons

Patterns also provide a vocabulary to design new efficient parallel algorithms and to communicate these designs to others.

The book shows examples that are appropriately freed of unnecessary distortions to map algorithms to particular hardware. Goal is to write scalable applications to utilise e.g., all cores on 4-core processor et cetera.

### Thinking Parallel (1.1)
===**Serialization**=== -> Act of putting some set of operations into a specific order.
- Reading a code from top bottom is easy to understand as it runs in that order.
- Naturally determinisitic, gives same answer when program is run if the input is the same.

===**Serial semantics**=== and ==**serial illusion**== is a mental model of the computer as a machine that executes operations sequentially. Programmers came to depend on it too much.

### **Performance (1.2)**
Computation may not be bottleneck, frequently access to memory or communication may constrain performance.

Algorithm's span determines whether scaling performance will perform well on a parallel computer. ==**Critical path**== -> Span is the time it takes to perform the longest chain of tasks that must be performed sequentially.

Improved performance is done through shortening the span.

===**Locality**=== -> asserts the memory accesses close together in time and space

When designing a parallel algorithm there's three things that need attention:
- The total amount of computational work.
- The span (the critical path).
- The total amount of communication (including that implicit in sharing memory).

===**Load Balancing**=== -> Distributing the workload across processors to achieve the common goal, if they're idle they look for work available. 

### Pervasive Parallelism (1.3)

**Moore’s Law** -> Integrated silicon chips, number of transistors roughly double about every 2 years this is still the case.

![[Pasted image 20230214212437.png]]

**Instruction pipelining** -> One way to automatically take advantage of instruction-level parallelism.

> An increase in clock rate, if the instruction set remains the same (as has mostly been the case for the Intel architecture), translates roughly into an increase in the rate at which instructions are completed and therefore an increase in computational performance.

Default clock rates (ghz) remain mostly the same now a days, because if you increase its clock rate the temperature increases and a demand for keeping it cool increases. Processors shifted their focus to expanding cores rather than rates.

This is limited by a **three walls**: 
- **Power wall**: Unacceptable growth in power usage with clock rate.
- **Instruction-level parallelism (ILP) wall**: Limits to available low-level parallelism.
- **Memory wall**: A growing discrepancy of processor speeds relative to memory speeds.

**Superscalar instruction issue** -> Modern processors can start multiple instructions at the same time.

**Very Large Instruction Word (VLIW)** -> Analysis of which instructions to execute in parallel done in advance by the compiler.

**Pipelining** -> Many operations are broken into a sequence of stages. Which can greatly increase the overall instructions throughput of a processor. Limits to number of stages which instructions can be decomposed.

**Bandwidth** (overall data rate) & **latency** (time between when a request is submitted and when it is satisfied)
- Latency can be hidden given sufficient additional parallelism to satisfy computational units.
- Algorithms need to be designed to avoid memory access and communication as much as possible. 

> To achieve higher performance, you now have to write explicitly parallel programs. And finally, when you write these parallel programs, the memory wall means that you also have to seriously consider communication and memory access costs and may even have to use additional parallelism to hide latency.

**Throughput** -> Total performance of multiple independent applications.

**Mandatory parallelism** -> *forces* the system to execute operations in parallel but may lead to poor performance.

**Explicit paralellism** -> Constructs allow algorithms to be expressed without specifying unintended and unnecessary serial constraints. Avoiding specifiying ordering and other constraints when they are not required is fundamental.

### Section 2.4 on Machine Models

Functional units are being able to perform a single arithemtic operation.

A **cache** memory hierarchy is typically used to manage the tradeoff between memory performance and capacity.

**Superscalar processor** -> Analzyes an instruction stream and executes multiple instructions in parallel as long as they do not depend on each other.

**Simultaenous multithreading** -> Instructions from multiple streams feed into a superscalar schedular and **switch-on-event multithreading** switches to a different hardware where a long-latency operation such as memory read happens.

Functional units operate directly on values stored in registers (very fast, small comaprable to functional units).

**Cache lines** -> Caches are organised into blocks of storage.

> When data is read from memory, the cache is populated with an entire cache line. This allows subsequent rapid access to nearby data in the same cache line.

> Normally writing is more expensive than reading because of reading, modyfiying and writing out.

**Virutal memory** -> Lets each processor use its own logical address space, which the hardware maps to the actual physical memory. Mapping is done per **page** -> relatively large blocks of memory.

- Larger data sets in programs than would not fit in physical memory.

- Parts of VM not used are called swap files (disk) physical memory can remap these to other local addresses in use.

If the page fault is too high it will decrease performance due to it being millions times slower.

**Data locality** is important at the level of virtual memory for two reasons.

	1. Good performance requires the page fault to be low
	2. Addresses must be translated rapidly from virutal addresses to physical address by   specialsied hardware called: Translation Lookaside Buffer (TLB)

A large number of pages being accessed in short timeframe can cause **TLB thrashing**, a high TLB miss can decrease performance.

**Multi-processor systems** -> Attaching memory directly to each processor and then connecting the processors with fast point-to-point communication channels.

![[Pasted image 20230217112857.png]]
==Data locality & availability of parallel operations dictate good performance!==

**Parallel slack** -> Amount of *extra* parallelism available above the minimum necessary to use the parallel hardware resources. 

> Specifying a significant amount of potential parallelism higher than the actual parallelism of the hardware gives the underlying software and hardware schedulers more flexibility to exploit machine resources.

**Flynn's Characterisation** -> Divides parallel processors into categories based on whether they have multiple flows of control, multiple streams of data or both.


