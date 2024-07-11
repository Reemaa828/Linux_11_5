# Getting Started with Access Control Mechanism ℹ
Linux is a **multi-user operating system**, so it has security to prevent people from accessing each other’s confidential files.
## Resources and Ownerships 🧑‍🦲


![Pasted image 20240621230323](https://github.com/Reemaa828/Linux_11_5/assets/112731236/2e48965f-5230-4da8-b8fc-985460b18126)
- USER: owns resources (files in this case) and launch processes.
- Process: uses files to do the process.
- resources: have owners; by default, the user that creates the file owns it.

## Types of Access control 🔐

## 1. DAC:
Discretionary access control, the user that owns the file will give permissions to others.
![Pasted image 20240622002237](https://github.com/Reemaa828/Linux_11_5/assets/112731236/654faf4e-aeb2-41bc-abba-7f82ca9cb63c)


## 2. MAC:
Mandatory access control, the boss (Admin) gives users a clearance level and the data a confidential level. users can only access data the same level of clearance or lower.![idnzwknl](https://github.com/Reemaa828/Linux_11_5/assets/112731236/587edeb6-e836-4344-8622-3a2ed403e612)



# Types of Users in Linux :accessibility:
## 1. System user: 
users that are made to run processes in the  background or system services.
## 2. Human user: 
users that that interactively uses Linux via the shell and have a home directory.
## 3. Superuser/Root/Administrator: 
has all the privileges and has all the control to do anything on the system with UID=0.


# Ways to do file permissions ❗ 
## 1. Scopes of Permission
### - User: 
The owner of the file.
### - Group:
Has one or more members.
### - Other: 
The category for everyone else.
## 2. Types of access
### - Read -r: 
  - for files: you can view the contents of the file.
  - for directory: you can view the sub-directories and files that exists in the directory
### - Write -w:
  - for files: you can delete and edit the file.
  - for directory: you can create, delete and rename files in the directory
### - Execute -x:
  - for files: you can execute the file if it has read permission too. 
  - for directory: you can view its use `ls`and `cd`
### - Set user id/Set group id -s:
it gives **process/executable files** the same user id of its owner and owner's privileges even if the process is launched by another user.
### - Sticky bit -t:
it works on ***directories*** only, it prevents non-root users from deleting files in the directory unless its the owner of the file.


# Octal vs letters representation 🔢
![Pasted image 20240622015123](https://github.com/Reemaa828/Linux_11_5/assets/112731236/01a439db-1569-4f36-9ee3-4c18e8599d6d)




# Files of interactions 🗃️
![Pasted image 20240622012455](https://github.com/Reemaa828/Linux_11_5/assets/112731236/11815e99-ccb8-4401-859c-00eb307d7c4c)


>[!NOTE]
>- The default in Linux is read access to all files.
>- System Users doesn’t own home directory.
>- UID is 32 bit number.
>- UID from 1 to 999 is reserved to system users.
>- use option `-R` recursively change permissions for all files and directories in a directory.
>`chmod -R 755 <diectory>`



