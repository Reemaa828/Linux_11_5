
# System Call Introduction💻

## What's a syscall?
Syscall is the **interface** between the application and kernel space, when the application request a service or access to a specific resource it makes this request thru a system call . syscall is a **software interrupt**.

## Two ways to invoke a syscall

![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/dda4ada4-16c2-45f2-9d67-5407f96aec83)



Directly invoking a syscall is rarely used but in this case is when the c standard library function does not implement a nice wrapper function for you or *new stack in the kernel is made so you have to make your own functions.*

in other cases most system calls have corresponding C library wrapper functions which perform the
steps required in order to invoke the system call.  Thus, making a system call looks the same as invoking a normal library function.
___
## Sequence of Operation

1) invoke function from the kernel in the userspace by using glibc
2) save context
3) switch from user mode to kernel mode
4) call function by using systable(table that maps syscall to its function in the kernel)
5) return result 
6) restore context
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/74c464c5-06d5-48e0-87a3-9f1ee6bdf682)

## Return result
The syscall return zero if its a success and return a negative error number if it fails, The C library wrapper hides this detail from the caller: when a system call returns a negative value, the wrapper copies the absolute value into the _[errno](https://man7.org/linux/man-pages/man3/errno.3.html)_ variable, and returns -1 as the return value of the wrapper.

___
## Conclusion
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/6a46b143-7447-43ff-b5ab-032b40d47d81)


