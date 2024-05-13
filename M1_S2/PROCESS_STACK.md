# process management introduction💻

What's a process?
      A process is an **active entity**, **a program in main memory** and an **instance** of a program loaded in memory and also a unit of execution of operation system.
      
the kernel is responsible for the transition from a program to a process. how ❓
   ```mermaid
     graph LR;
                         A(program) --kernel--> B(process)                 
   ```
steps of this transition 🪜
-  program is a binary or an executable which we will want to run using the terminal, kernel loads this program into the memory
-  then from memory to virtual memory
-  kernel gives the process
    -  ID
    -  Priority
    -   Time of execution 
What kind of operation that can be done to **manipulate process** in userland?
    1. run this process or create it(creation is done by the kernel) 
    2. terminate the process
    3. change priority
    4. restrict the resources on the process
    5. monitor the processes of system 
    6. know the relation between processes
    7. send information to process (signal)
what's commands?
- use `foreground` or `background` to start a process
- use `kill` to terminate a process
- use `pstree` to know the relations of processes
- use `top` or `ps` to show all processes that's happening in system **top** for periodical monitoring **ps** snapshots of current status
- use `rnice` to change priority but its rarely used
- use `strace` for monitoring processes
what's files of interaction?
    `/proc/`      
### `top` is like windows **task manger**
      clock   hoursinsystem   numberofusers   loadin1min5min15min
      task: totalofproc numberof running, sleeping, stopped and zombie processes
      percentage of cpu load: userspace kernelspace ? idealstate  ????
      ?
      ?
      processid user priority nice ? ? ? ? cpuload memoryload  timeitsrunning command

![[Pasted image 20240513060359.png]]

### `tldr` common usages of a certain command
![[Pasted image 20240513063213.png]]

>[!note]
>- `tldr` is used like `man` command to show you common  uses of a command and its options.
>- `ps` is mainly used to get the path of a process or if `top` doesn't exist in the image.
>- we rarely change priority because we are not kernel developers it cannot be a decision
>- anything in blue is a directory


>

