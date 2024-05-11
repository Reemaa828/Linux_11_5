# Session One
## commands learned
```
$ uname -a
$ which uname
$ cd
$ ls
$ ls | grep "uname"
$ echo $PATH
$ man getuid
```

## how to create a new command in linux
firstly make a cpp application and run it using this command:
```
$ g++ main.cpp -o userid
```
then to run this code/command we should change location to a system variable type path 
```
$ sudo cp userid /usr/bin/
```
and voilà you have created a command!

