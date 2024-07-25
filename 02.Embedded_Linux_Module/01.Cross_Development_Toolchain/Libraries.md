# Table of Contents

- [Library](#library)
- [Static Library](#static-library)
	- [How to create a static library?](#how-to-create-a-static-library)
	- [Why do we use Static Linking?](#why-do-we-use-static-linking)
- [Dynamic Library](#dynamic-library)
	- [So What actually happens when dynamic linking?](#so-what-actually-happens-when-dynamic-linking)
	- [At run time (Dynamic Library)](#at-run-time-dynamic-library)
	- [What's a dynamic linker?](#whats-a-dynamic-linker)
	- [How to create a Dynamic Library?](#how-to-create-a-dynamic-library)
	- [Why do we use Dynamic Linking?](#why-do-we-use-dynamic-linking)
- [References](#references)

# Library 
A library is a collection of pre-compiled pieces of code that can be reused in a program to avoid repetition of code.The role of Libraries comes in the final step of compilation which is called ‘**Linking**’. There's two types of libraries:
1. static library 
2. dynamic library (shared library)


# Static Library
A static library is a file containing a collection of object files (*.o) that are linked into the program during the linking phase of compilation and are not relevant during runtime,
resulting in a single executable file that contains the code from both the program and the library.

![Pasted image 20240723231559.png](Pasted%20image%2020240723231559.png)

## How to create a static library?
### (`ar` Command) to create and manage static libraries
#### 1. create your code 
create source code for your library for example `add.c` and `mul.c` and compile them using `gcc`
![Screenshot from 2024-07-23 23-49-00.png](Screenshot%20from%202024-07-23%2023-49-00.png)
![Screenshot from 2024-07-23 23-40-26.png](Screenshot%20from%202024-07-23%2023-40-26.png)
![Screenshot from 2024-07-23 23-42-48.png](Screenshot%20from%202024-07-23%2023-42-48.png)

#### 2. create static library using `ar -rcs <library_name> <object files>`
`-r` to replace existing object files or insert object files
`-c` to create static library
`-s` to create achiever with a symbol table
![Screenshot from 2024-07-23 23-55-08.png](Screenshot%20from%202024-07-23%2023-55-08.png)
`ar -t <library_name> ` to show  all object files
`ar -x <library_name> ` to extract the library archiver
![Screenshot from 2024-07-24 00-08-27.png](Screenshot%20from%202024-07-24%2000-08-27.png)
`ar -d <library_name> <object_file>` to delete object file
`ar -r <library_name> <object_file>` to  add another object file
![Screenshot from 2024-07-24 00-12-18.png](Screenshot%20from%202024-07-24%2000-12-18.png)

#### 3. use the static libraries in your program
create a source code to use static libraries that you have created
![Screenshot from 2024-07-24 00-43-00.png](Screenshot%20from%202024-07-24%2000-43-00.png)
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

![Screenshot from 2024-07-24 01-13-50.png](Screenshot%20from%202024-07-24%2001-13-50.png)
## Why do we use Static Linking?
- to remove dependencies (the program have everything to run)
- libraries that are not used many times (uncommonly used )
- to make sure we are using the same version of the library

___
# Dynamic Library
Dynamic Library are pre-compiled code that are linked to the program during runtime thanks to dynamic linker.Dynamic libraries unlike static libraries are not embedded into the executable at compile time. this means there's no footprint of library in the program binary.
## So What actually happens when dynamic linking?
the program is compiled with a reference to symbols(functions/global variables)
1. when the program is loaded into the memory the loader/linker knows the dependencies of the program and loads the shared object to RAM
2. LINUX uses lazy binding, meaning it resolves the symbol when it calls the function first.
3. when the function is called, the dynamic linker resolve the symbol by pointing to actual location of the functionality that exists in the shared object.
## At run time (Dynamic Library)
When the program loads in memory (at run time), the program binary is checked by the Dynamic Linker (ld-linux.so).
• Note that **ld-linux.so** is dynamically linked by GCC with the program by default when using dynamic linking
• The Dynamic linker checks for dependencies on shared libraries
• It also tries to resolve these dependencies by locating the required libraries and updating the binary
• If the Dynamic linker fails to find a library, the program fails to run
## What's a dynamic linker?
A dynamic linker is a crucial component of an operating system that handles the process of loading and linking shared libraries at runtime. it's responsible for looking for shared object and resolving the symbol table of the program by pointing to the functionalities needed from shared object.
![Screenshot from 2024-07-25 00-47-52.png](Screenshot%20from%202024-07-25%2000-47-52.png)
### How it Works:
- **Shared Libraries:** These are libraries that multiple programs can use simultaneously. They are not part of the executable itself but are loaded into memory when the program runs.
- **Executable:** The program contains references to the shared libraries it needs in symbol table, but the actual code for these libraries is not included in the executable.
- **Loading:** When the program starts, the dynamic linker loads the necessary shared libraries into memory.
- **Linking:** The dynamic linker resolves the references in the executable to the actual code in the shared libraries, creating the connections necessary for the program to run.

### Where does dynamic linker look for the Shared object?

**Via Library Path Environment Variable:**
• It checks the environment variable LD_LIBRARY_PATH, which contain a list of directories (separated by a colon “:”) that contain the libraries

**Via Dynamic Linker Cache:**
• The dynamic linker cache is a binary file (`/etc/ld.so.cache`)
• It contains an index of libraries and their locations
• It provides a fast method for the dynamic linker to find `ld.so.cache` libraries in the specified directories
• To add a directory to the dynamic linker cache,
    - Add the directory path to the file `/etc/ld.so.conf`
• Run the utility `ldconfig` to generate the binary cache



## How to create a Dynamic Library?

### 1. create Dynamic Library
#### `gcc -shared -fpic -o <library_name> <object_files>`
![](Screenshot%20from%202024-07-25%2003-00-40.png)

### 2. use Library with your program 
#### `gcc main.c -L./ -lreema -I./Desktop`
![](Screenshot%20from%202024-07-25%2003-05-34.png)
### to resolve this error 
#### use `ldd` to show the dependencies needed for the program
![](Screenshot%20from%202024-07-25%2003-08-25.png)
#### you need to add shared library to either :
1. LD_LIBRARY_PATH
![](Screenshot%20from%202024-07-25%2003-17-24.png)
2. Dynamic Linker cache
use `ldconfig` to generate the binary cache
![](Screenshot%20from%202024-07-25%2003-40-54.png)
use `ldd <executable>` command and you will find the required library and it's location for this program
![](Screenshot%20from%202024-07-25%2003-42-53.png)

## Why do we use Dynamic Linking?
• To keep the executable binary image size smaller
• Since multiple programs may be using the same library, it makes no
sense to include it in each and every one of them
• Also, as the different programs loaded in memory, they need less
memory (since the shared object will be loaded only once in the
memory)
• Upgrade in functionality and bug fixing of the library does not require
re-building of all the programs using this library


# References
- [all about static library](https://dev.to/iamkhalil42/all-you-need-to-know-about-c-static-libraries-1o0b)
- [all about dynamic library](https://www.linkedin.com/pulse/dynamic-libraries-c-linux-agustin-flom/)
- [static and dynamic linking](https://www.youtube.com/watch?v=YtiPCPtmZrs)