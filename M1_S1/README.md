# Session One 👆
## Commands Learned 👾
```
  uname -a
  which uname
  cd
  ls
  ls | grep "uname"
  echo $PATH
  man getuid
```
___

## Commands Explanation 👾
  ### Command is an application with a location and it has inputs and outputs
   * use `ls` for displaying contents of directory
     * ys
      * b
      -bbb
   * use `cd` for changing directory or to move from current directory to another
   * use `which` for telling or showing you the path of file
   * use `ls | grep "file"` for finding a specific file
   * use `man` to show you the documents of glibc or use it as a manual/reference for glibc
   * use `sudo` to give permission
   * use `cp` to copy something to another
   * use `rm` to remove a file
   * use `id --user` its the same as getuid() syscall
   * use `cat /proc/version` show the version of os
   * use `cat /proc/cpuinfo | grep "model name"` show cores
   * use `cat /proc/$$/status | head -n6` without head -n6 it shows all , with it; it shows only first 6; $$ means current process
   * use `lscpu` to get architecture info or use `uname -m`
   * use `grep MemTotal /proc/meminfo` ram size
   * use `grep VmallocTotal /proc/memibfo` virtual memory size
   * use `ip link` for ip address  
   * use `ls -al /sys/devices` an overview of the devices on your Linux system
   * use `strace ls` tell us the behind scene syscalls
   * use `strace -c` overview of syscalls
___


## How to Create a New Command in Linux 👾
  * first make a cpp application and run it using this command:
```
$ g++ main.cpp -o userid
```
  * then to run this code/command we should change location to a system variable type path 
```
$ sudo cp userid /usr/bin/
```
  and **_voilà_** you have created a command!
  
  ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/59ffa8a3-3fa0-4a8b-84b5-ed785d30c9d8)
  ___



