# Session One
## Commands learned
```
  uname -a
  which uname
  cd
  ls
  ls | grep "uname"
  echo $PATH
  man getuid
```

## Commands explanation 
  ### Command is an application with a location and it has an inputs and outputs
   * use `ls` for displaying contents of directory
   * use `cd` for changing directory or to move from current directory to another
   * use `which` for telling or showing you the path of file
   * use `ls | grep "file"` for finding a specific file
   * use `man` to show you the documents of glibc or use it as a manual/reference for glibc
   * use 'sudo' to give permission
   * use `cp` to copy something to another
   * use `rm` to remove a file


## How to create a new command in linux
  * first make a cpp application and run it using this command:
```
$ g++ main.cpp -o userid
```
  * then to run this code/command we should change location to a system variable type path 
```
$ sudo cp userid /usr/bin/
```
  and voilà you have created a command!
  
  ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/59ffa8a3-3fa0-4a8b-84b5-ed785d30c9d8)



