# Tasks on System Call💻

## Exercise1️⃣
 ### Trace System call for ` ps` & `cd` & `ls` commands
>[!important]
>* you can trace external executables directly but not build-in commands in bash cause it doesnt have seperate directories the process is performed in the shell itself, none of the builtins to Bash are traceable. strace can only trace actual executables, whereas the builtins are not.
>* *trick* we can use `strace bash` to indirectly trace build-in commands



![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/b4064191-efcd-455b-81da-4e97208e1fee)

![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/a24c2191-9c44-49c1-be2b-7b9451ca3966)

## Exercise2️⃣
 ### for timestamp use `strace -r`
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/73ca9697-5dc4-46ae-81af-154658725aca)





