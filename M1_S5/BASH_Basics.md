# Bash Scripting Language🐚
## 🏗️ Basic Structure of a Bash Script
1. shebang
2. inputs
3. outputs
4. types of variables
5. check conditions
6. functions
### 💥 Shebang
Shebang (#!): A seqeunce of character followed by the path of interpreter which indicates which interpreter which be used.
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/476fe714-20bc-4e92-9638-a2be4177b2b0)

### ➡️ Inputs
1. Before you run the script
   - Accessing posiitonal parameters inside the script:
     - use `$0` for script's name
     - use `$1` for the first argument passed to script, `$2` for the second argument passed to script, `$...
     - use `$#` to get the numbers of arguments
     - use `$@` to get all arguments

2. After you run the script
   - use `read <variable_name>` to hold the script and wait for an input

![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/2671450d-9412-4370-b94c-24909aae46b4)

### ⚙️ Types of variables
```mermaid
flowchart TB
    A(Shell Variables) --> B(enviroment variable)
    A --> C(local variable)
    A --> D(Special variable)
```
1. local variable operations:
   - declare a variable
   ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/67fdc1f8-e738-4744-afdd-a35887c87666)
   - assign a varaible
   ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/4c38656a-b69f-4974-a4b7-7f5f0a6e9195)
   - access a variable
   - delete a variable
   ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/604a12fd-7e6e-4dee-b182-ee5b09d11901)


2. Enviroment Variable operations:
   - declare a variable: two ways
     ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/dbd2888a-0ff7-4307-a270-64a680e16dd0)
   - change value
     ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/6241315e-9f50-4fd8-b5e8-1f9f7e018368)
   - access a variable
   - delete a variable
   - print all variable
   ![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/2ed735e9-537e-4a8c-ab8a-8c481b963245)



## Ways to run a Bash Script?
```mermaid
flowchart TB
    A(Bash Script) --> B(Run normally)
    A --> C(Source)
    A --> D(Schedule)
```

