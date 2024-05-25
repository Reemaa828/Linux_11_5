# Task 🅰️
## Exercise 1️⃣: Basic Navigation
* Use `ls` to list all files and directories in the current directory.
* Use `cd` to navigate to a specific directory.
* Use `pwd` to print the current working directory.
### Commands used
```shell
ls
cd
pwd
```
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/2be505d1-0058-4ed8-93ff-a8393347194c)


## Exercise 2️⃣: File and Directory Operations
* Create a directory named "practice" in the current directory using `mkdir`.
* Create an empty file named "file.txt" within the "practice" directory using `touch`.
* Copy "file.txt" to a new file "file_backup.txt" using `cp`.
* Move "file_backup.txt" to another directory using `mv`.
* Rename "file.txt" to "new_file.txt" using `mv`.
* Delete the "new_file.txt" using `rm`.
### Commands used
```shell
mkdir practise
touch file.txt
cp file.txt file_backup.txt
mv file_backup.txt /
mv file.txt /new_file.txt
rm new_file.txt
```
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/f134f4df-ff6d-4c8f-ad14-2a7e510506b8)

## Exercise 3️⃣: File Viewing and Editing
* Create a text file using `echo` or a text editor like `nano`.
* View the contents of the file using `cat`.
* View the contents of the file using `less`.
* Edit the file using `nano` or another text editor.
* Redirect the output of a command (e.g., ls) to a file using.
### Commands used
```shell
echo "reem" > file.txt
cat file.txt
less file.txt
nano file.txt
ls > file.txt
```
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/51875779-b3e8-4b5f-af45-5453587ee85c)

## Exercise 4️⃣: File Permissions
* Create a file and set specific permissions using `chmod`.
* Check the permissions of the file using `ls -l`.
* Change the owner and group of the file using `chown`.
* Verify the changes using `ls -l`.
### Commands used
```shell
ls -l
chmod u-w file.txt
chown root file.txt
```
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/458a88ea-3397-418a-8ddb-bd7550d51799)

## Exercise 5️⃣: User and Group Management
- Create a new user using `useradd`.
- Set a password for the new user using `passwd`.
- Create a new group using `groupadd`.
- Add the user to the newly created group using `usermod`.
### Commands used
```shell
useradd riri
passwd riri
groupadd rii
usermod --append --groups rii riri
```
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/9cadedca-ff5a-4fdc-b920-6846d74e81df)
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/8f99d91f-81e0-4c2f-b6bf-0844f433ac9b)

