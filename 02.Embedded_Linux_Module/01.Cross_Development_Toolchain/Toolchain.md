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


## GCC: GNU Compiler Collection
