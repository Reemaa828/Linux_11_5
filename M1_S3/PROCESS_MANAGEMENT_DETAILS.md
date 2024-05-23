# process management stack

## What's a process?
it's a program in memory also a process is a way which Linux organizes the different program waiting for its turn at the CPU.
## How does the kernel transition the program to a process?
* loads program to RAM
* reserve a Virtual Memory
* kernel provides process characteristics 1) id 2) priority 3) time from CPU
## Ways to Monitor processes
* use `top` Display tasks
* use `ps` Report a snapshot of current processes
* use `jobs` List active jobs
* use `bg` Place a job in the background.  
* use `fg` Place a job in the foreground.  
* use `kill` Send a signal to a process.  
* use `killall` Kill processes by name.  
* use `shutdown` Shut down or reboot the system.

**The fact that a program can launch other programs is expressed in the process scheme as a ==parent process== ==producing a child process==**

## Operations done on processes
### 1. viewing processes with `ps`

![Pasted image 20240520010158](https://github.com/Reemaa828/Linux_11_5/assets/112731236/591aa95e-efca-41e0-b40c-ecc16346c92b)

by default ps doesn’t show us very much, just the processes associated with the current  
terminal session. To see more, we need to add some **options**.
TTY is short for **teletype** and refers to the controlling terminal for the process.
The TIME field is the amount of CPU time consumed by the process.

* ***adding options to ps** `ps x`
tells ps to show  all of our processes regardless of what terminal (if any) they are controlled  
by. The presence of a ? in the TTY column indicates no controlling terminal.


**STAT is short for state and reveals the current status of the process**. 
![Pasted image 20240520021706](https://github.com/Reemaa828/Linux_11_5/assets/112731236/7ff59837-f638-4ae1-bd7d-51e5b4353b69)


different STAT


![Pasted image 20240520022318](https://github.com/Reemaa828/Linux_11_5/assets/112731236/57de75fd-d068-481c-a08e-9b3cb4bb6802)


* `ps aux` **This  gives us even more information**
![Pasted image 20240520022706](https://github.com/Reemaa828/Linux_11_5/assets/112731236/fa8146fe-92b2-4899-a267-61e4bb6838e2)

**column headers meaning**
![Pasted image 20240520022939](https://github.com/Reemaa828/Linux_11_5/assets/112731236/acf3279b-e284-417e-aac4-117924929c98)


### 2. Viewing Processes Dynamically with `top`
The top program displays a continuously updating (by default, every 3 seconds) display of the system processes listed in order of process activity.


![Pasted image 20240520023537](https://github.com/Reemaa828/Linux_11_5/assets/112731236/412f4f38-bf17-4819-9009-ffdf77fda91c)

![Pasted image 20240520023754](https://github.com/Reemaa828/Linux_11_5/assets/112731236/2c0484c6-d88f-43c3-9f0c-0dd207abd506)
![Pasted image 20240520023818](https://github.com/Reemaa828/Linux_11_5/assets/112731236/5481d835-d512-484d-b702-3197bca978bd)


### 3. Controlling process
#### 1. interrupt a process by using **CTRL+C**
 In a terminal, pressing CTRL-C interrupts a program. ==this means that we  politely asked the program to terminate==. After we pressed CTRL-C, the `xlogo` window closed and the shell prompt returned.


https://github.com/Reemaa828/Linux_11_5/assets/112731236/111a10ab-e9fc-4c2c-8c0a-a5d5f56d16b6


#### 2. Putting a Process in the Background 
we wanted to get the shell prompt back without terminating the `xlogo` program. We’ll do this by placing the program in the background.


https://github.com/Reemaa828/Linux_11_5/assets/112731236/840d201e-e548-48f7-a285-5e1b293a17b0




**a foreground (with stuff visible on the surface, like the shell prompt) and a background (with hidden stuff below the surface).**

This message is part of a shell feature called ==job control==. With this message, the shell is telling us that we have started job number 1 ([1]) and that it has PID  4483

![Pasted image 20240520035849](https://github.com/Reemaa828/Linux_11_5/assets/112731236/0d8e2e0d-a332-4e09-bd65-d7328b042238)

![Pasted image 20240520040723](https://github.com/Reemaa828/Linux_11_5/assets/112731236/29078e8e-73d4-4bed-96a2-c541d124407c)

![Pasted image 20240520040834](https://github.com/Reemaa828/Linux_11_5/assets/112731236/d02c5c61-cf72-4df4-8581-241181ebd1ce)


#### 3.Returning a Process to the Foreground
A process in the background is immune from keyboard input. To return a process to the foreground, use the `fg` command.

The command `fg` followed by a percent sign and the job number (called a jobspec) does the trick. If we have only one background job, the jobspec is optional.
#### 4.Stopping (Pausing) a Process
 Sometimes we’ll want to stop a process without terminating it.
 *We can either restore the program to the foreground, using the `fg` command, or move the program to the background with the `bg` command*


https://github.com/Reemaa828/Linux_11_5/assets/112731236/71e6d2ea-7f54-49d5-9421-42bfa6a10808

### Signals 
==**Signals are one of several ways that the operating system communicates with programs**==
#### 1. `Kill`: The kill command is used to send signals.
use `kill pid` or `kill %jobspec`
##### its default is to terminate the process.


https://github.com/Reemaa828/Linux_11_5/assets/112731236/33236a1f-b4ad-4ae0-b037-a4f2e3ae0910




use `kill -signal number PID` to send signal to a process.


![Pasted image 20240520050404](https://github.com/Reemaa828/Linux_11_5/assets/112731236/502cee5b-13cf-4b43-823a-ea7e57ba16bf)


we can use `-SIG"signal name"` or `-signal name` or `-no.` and with its PID or %jobspec


![Pasted image 20240520051341](https://github.com/Reemaa828/Linux_11_5/assets/112731236/b93b2783-c545-4dee-861c-5f62f38da0e9)

use `kill -l` to see list of  signals


![Pasted image 20240520051543](https://github.com/Reemaa828/Linux_11_5/assets/112731236/7c7c53c6-b1b1-4a07-8e74-519add894df9)

#### 2. `Killall`: Sending Signals to Multiple Processes with killall.


# Conclusion 


![processmanagement](https://github.com/Reemaa828/Linux_11_5/assets/112731236/75575609-c27f-4caf-ac6e-8a73935efc29)



