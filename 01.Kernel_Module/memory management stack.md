# Virtual Memory: illusion of a larger memory.
- Virtual memory is a **memory management technique** used by operating systems to give the appearance of a large, continuous block of memory to applications, even if the physical memory (RAM) is limited.

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6663f6e6ab9c39191bf6d41245d542fe/raw/image.png&w=250&h=400)
- Linux configures the **Virtual Memory** into a user space and kernel space.
    -  **User Address Space**: 
    ***Isolated for each process*:** Every process has its own unique address space, meaning it can't directly access the memory of other processes. This isolation is crucial for security and stability.
    - **kernel Address space:**
    ***Shared by all processes:*** There's only one instance of the kernel in a system, and all processes share this address space.
- Virtual Addresses in this virtual address space are mapped to physical addresses by the memory management unit (**MMU**), it has a "page table" to manage this mapping.
>The split between the two is set by a kernel configuration parameter named PAGE_OFFSET.
![Screenshot from 2024-08-08 18-22-52.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d0be3ac0f9cfe3642eb2ca378740534d/raw/Screenshot%20from%202024-08-08%2018-22-52.png&w=190&h=280)


# Memory Management unit
MMU stands for Memory management unit also known as PMMU (paged memory management unit), Every computer system has a memory management unit , it is a hardware component whose main purpose is to convert virtual addresses created by the CPU into physical addresses in the computer’s memory. In simple words, it is responsible for memory management In a device as it acts as a bridge between the CPU and the RAM, which ensures that programs can run smoothly and access the required data without clashes or unauthorized access.
# Mapping of memory management unit (mmu)
The memory management unit uses **Page Table** to keep track of VA --> PA mappings
two cases 
1. **Unmapped:**
    - This means there's no corresponding entry in the page table for this virtual address.
    - Any attempt to access this page will result in a **segmentation fault (SIGSEGV)**.
2. **Mapped to a Physical Address**

![Screenshot from 2024-08-08 19-18-42.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/1c1fb61c401c90449c55ef69441af67e/raw/Screenshot%20from%202024-08-08%2019-18-42.png)
# Advantages of Virtual Memory
- Allowing users to operate multiple applications at the same time or applications that are larger than the main memory
- Improving security by isolating and segmenting where the computer stores information
- Increasing the amount of memory available by working outside the limits of a computer's physical main memory space



# Consumers of kernel address space
1. kernel itself, the code and the data that are loaded from kernel image at boot time. 
2. kernel stacks.
3. kernel data structures allocated by stab allocator.
4. mapping for device drivers .
you can see the memory taken up by the kernel code and data by using `size` command
![Screenshot from 2024-08-08 21-18-22.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/cc0b2729b78892d01404eca6665ad9c6/raw/Screenshot%20from%202024-08-08%2021-18-22.png)
You can get more information about memory usage by reading `/proc/meminfo`
![Screenshot from 2024-08-08 21-23-04.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/673415ba2ddac3d778145adec148b644/raw/Screenshot%20from%202024-08-08%2021-23-04.png)
# MMU view 
use `cat /proc/<PID>/maps`
![Screenshot from 2024-08-08 21-36-26.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/7a0c02bf1b9457bd9b2d961cc5e1ef18/raw/Screenshot%20from%202024-08-08%2021-36-26.png)
The first three columns show the start and end virtual addresses and the permissions
for each mapping. The permissions are shown here:
• r = read
• w = write
• x = execute
• s = shared
• p = private (copy on write)

# Swap
**Swap** is essentially the hard drive space used as an extension of your computer's RAM (physical memory). It comes into play when your system runs out of physical memory.
### How it works:

1. **RAM fills up:** When all the physical memory (RAM) is occupied by running programs and data, the operating system faces a dilemma.
2. **Swap space activated:** To make room for new processes or data, the system moves less actively used pages of data from RAM to the swap space on the hard drive. This process is called _swapping out_.
3. **Data retrieval:** When the data that was swapped out is needed again, it's brought back from the swap space to RAM, a process known as _swapping in_.

### Key points about swap:

- **Slower than RAM:** Accessing data from the hard drive is significantly slower than accessing it from RAM.
- **Impact on performance:** Excessive swapping can dramatically slow down your computer.

### Not used in embedded system because it would wear out the flash quickly.


# Memory  leaks
A memory leak is when you dynamically allocate memory but didn't free it.
- use `mtrace`
- use `valgrind`
![Screenshot from 2024-08-08 22-31-26.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/14a862bc1169e507d9ea61e261e54662/raw/Screenshot%20from%202024-08-08%2022-31-26.png)
