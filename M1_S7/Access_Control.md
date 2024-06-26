# Getting Started with Access Control Mechanism ℹ
Linux is a **multi-user operating system**, so it has security to prevent people from accessing each other’s confidential files.
## Resources and Ownerships :RiKeyFill:


![[Pasted image 20240621230323.png]]
- USER: owns resources (files in this case) and launch processes.
- Process: uses files to do the process.
- resources: have owners; by default, the user that creates the file owns it.

## Types of Access control :BoBxUniversalAccess:

## 1. DAC:
Discretionary access control, the user that owns the file will give permissions to others.
![[Pasted image 20240622002237.png]]

## 2. MAC:
Mandatory access control, the boss (Admin) gives users a clearance level and the data a confidential level. users can only access data the same level of clearance or lower. ![[idnzwknl.png]]


# Types of Users in Linux :LiUsers:
## 1. System user: 
users that are made to run processes in the  background or system services.
## 2. Human user: 
users that that interactively uses Linux via the shell and have a home directory.
## 3. Superuser/Root/Administrator: 
has all the privileges and has all the control to do anything on the system with UID=0.


# Ways to do file permissions :IbLocked: 
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


# Octal vs letters representation :OcNumber16:
![[Pasted image 20240622015123.png]]



# Files of interactions <!-- files icon by Free Icons (https://free-icons.github.io/free-icons/) -->
<svg xmlns="http://www.w3.org/2000/svg" height="1em" fill="currentColor" viewBox="0 0 512 512">
  <path
    d="M 192 384 Q 178 384 169 375 L 169 375 L 169 375 Q 160 366 160 352 L 160 64 L 160 64 Q 160 50 169 41 Q 178 32 192 32 L 336 32 L 336 32 L 336 112 L 336 112 Q 336 126 345 135 Q 354 144 368 144 L 448 144 L 448 144 Q 448 144 448 144 Q 448 145 448 145 L 448 352 L 448 352 Q 448 366 439 375 Q 430 384 416 384 L 192 384 L 192 384 Z M 368 58 L 422 112 L 368 58 L 422 112 L 368 112 L 368 112 L 368 58 L 368 58 Z M 192 0 Q 165 1 147 19 L 147 19 L 147 19 Q 129 37 128 64 L 128 352 L 128 352 Q 129 379 147 397 Q 165 415 192 416 L 416 416 L 416 416 Q 443 415 461 397 Q 479 379 480 352 L 480 145 L 480 145 Q 480 125 466 111 L 370 14 L 370 14 Q 356 0 336 0 L 192 0 L 192 0 Z M 64 112 Q 63 97 48 96 Q 33 97 32 112 L 32 384 L 32 384 Q 33 438 69 475 Q 106 511 160 512 L 368 512 L 368 512 Q 383 511 384 496 Q 383 481 368 480 L 160 480 L 160 480 Q 119 479 92 452 Q 65 425 64 384 L 64 112 L 64 112 Z"
  />
</svg>
![[Pasted image 20240622012455.png]]

>[!NOTE]
>- The default in Linux is read access to all files.
>- System Users doesn’t own home directory.
>- UID is 32 bit number.
>- UID from 1 to 999 is reserved to system users.
>- use option `-R` recursively change permissions for all files and directories in a directory.
>`chmod -R 755 <diectory>`



