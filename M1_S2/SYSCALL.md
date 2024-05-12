# Session two 2️⃣
## System Call Introduction
  **what is a system call?**

  System call provides a **service** of the linux kernel, in another definition its a software interrupt. its also the interface provided for us to use a service made available by os.
  ___
   **how does it work?**

  When a userland request a service from kernel space, meaning that if a program needs to access a specific resource it has to go thru the kernel first
this service is the system call.
    **Steps**
    
    1.  invoke a function or a service from the kernel by using glibc 
    -  save context of userspace 
    -  context switch from userspace to kernelspace
    -  search for syscall in [syscall table](https://filippo.io/linux-syscall-table/)
    -  do operation
    -  return result 
            | status | return value |
            | --------- | -------- |
            | failure | cause of failure or any value except 0 |
            | success | content or 0 or null |
    - restore context of userspace 
    ___
**Example of System Call**
```mermaid
 
    
    
    
  
