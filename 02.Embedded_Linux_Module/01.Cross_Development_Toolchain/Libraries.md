# Library 
A library is a collection of pre-compiled pieces of code that can be reused in a program to avoid repetition of code.The role of Libraries comes in the final step of compilation which is called ‘**Linking**’. There's two types of libraries:
1. static library 
2. dynamic library (shared library)


# Static Library
A static library is a file containing a collection of object files (*.o) that are linked into the program during the linking phase of compilation and are not relevant during runtime,
resulting in a single executable file that contains the code from both the program and the library.

![[Pasted image 20240723231559.png]]

## How to create a static library?
### (`ar` Command) to create and manage static libraries
#### 1. create your code 
create source code for your library for example `add.c` and `mul.c` and compile them using `gcc`
![[Screenshot from 2024-07-23 23-49-00.png]]
![[Screenshot from 2024-07-23 23-40-26.png]]
![[Screenshot from 2024-07-23 23-42-48.png]]

#### 2. create static library using `ar -rcs <library_name> <object files>`
`-r` to replace existing object files or insert object files
`-c` to create static library
`-s` to insert index 
![[Screenshot from 2024-07-23 23-55-08.png]]
`ar -t <library_name> ` to show  all object files
`ar -x <library_name> ` to extract the library archiver
![[Screenshot from 2024-07-24 00-08-27.png]]
`ar -d <library_name> <object_file>` to delete object file
`ar -r <library_name> <object_file>` to  add another object file
![[Screenshot from 2024-07-24 00-12-18.png]]

#### 3. use the static libraries in your program
create a source code to use static libraries that you have created
![[Screenshot from 2024-07-24 00-43-00.png]]
#### 4. compile your program with static libraries and run it
```bash
g++ main.cpp -L. -lreema -I./Desktop 
#or
ld -d n main.cpp -L. -lreema -I./Desktop 
#or
g++ -static main.cpp -L. -lreema -I./Desktop 
```
`-L` : Specifies the path to the given libraries ('.' referring to the current directory)  
`-l`: Specifies the library name without the "lib" prefix and the ".a" suffix, because the linker attaches these parts back to the name of the library to create a name of a file to look for.

![[Screenshot from 2024-07-24 01-13-50.png]]
## Why do we use Static Linking?
- to remove dependencies (the program have everything to run)
- libraries that are not used many times (uncommonly used )
- to make sure we are using the same version of the library
___
# Dynamic Library

