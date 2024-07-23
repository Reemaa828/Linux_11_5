# What's a Toolchain? 🪛
A set of development tools used with source code or binaries generated from source code.
It's used in number of operations:
- compilation
- preparing libraries
- reading binaries
- debugging
  
it normally contains these tools:
1. **compiler**: generate object files from source code.
2. **linker**: link object files together to build executable.
3. **library archiver**: to group a set of object files to a library file.
4. **debugger**: to debug the binary file while running.
Most popular toolchain is the **GNU toolchain** which is part of GNU project.

# Machines involved 🎰
- **Config machine:** the machine configuring the toolchain components.
- **Build machine**: the machine building the toolchain components.
- **Host machine**: the machine running the toolchain.
- **Target**: the machine running the program that toolchain generated.
We can most of the time assume that the config machine and the build machine are the same.
# Types of Toolchain
1. **Native Toolchain:** build == host == target (**“native”**)
3. **Cross-platform Toolchain:** build == host != target (**“cross”**)
4. **Cross-Native Toolchain**: build != host == target (**“cross-native”**)
5. **Canadian Toolchain**: build != host != target (**“canadian”**)
# Toolchain Components 
<img src="https://github.com/user-attachments/assets/9ce7019a-7bd5-483d-a34d-abb7853b8d5f" width="500">


## 1. GCC: GNU Compiler Collection
The **compiler** is software that converts a program written in a high-level language (Source Language) to a low-level language (Object/Target/Machine Language/0, 1’s).
![Pasted image 20240723005654](https://github.com/user-attachments/assets/1228320c-e43f-40bb-b88e-b22f53b2a151)

use `g++ <file_name>` to perform the compilation process and get executable
![Screenshot from 2024-07-23 01-06-45 1](https://github.com/user-attachments/assets/35e105c9-f288-4ad1-9c01-5418592d1eab)

