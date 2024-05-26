# New filesystem🗃️
## Drive: A storage device.
## Partition: splits the storage devices in sectors and gets treated in Linux as two different device

## Super block: a fixed position in the storage device which contains metadata of filesystems (number of inodes, filesystem type, blocks, state).

## Inode: contain metadata of files(ownerships, permissions, size) but not actual filename or data.
*The number of Inode limits the total number of files/directories that can be stored in the file system*
****metadata is data about data***
![[Pasted image 20240526094806.png]]
**use `stat .` for directory state
use `stat <file_name>` for file state
![[Pasted image 20240526100809.png]]
use `findmnt -D -t nosquashfs` to excluding specialized compressed filesystems  
use `lsblk -e 7` to exclude loop devices
![[Pasted image 20240526101322.png]]
use `sudo fdisk -l` to show partitions of block devices and other info about each partition
![[Pasted image 20240526101751.png]]
use `sudo parted --list` for listing partitions
![[Pasted image 20240526102215.png]]
![[Pasted image 20240526103729.png]]
# Linux Links 🔗
## 1. Hard Link
It's a copy of the original file, If the original file is deleted the hard links will not be affected.
Some specs:
1) same inode number
2) same file size
3) different name of the original file
## 2. Soft Link aka Symbolic Link
**it's a shortcut of the original file, If the original file is deleted the soft links becomes useless**
Some specs:
1) different inode number
2) small file size
3) it's a pointer to the original file

![[Pasted image 20240526115156.png]]
use `ln <file_name> <hardlink>` to make a hard-link, you will notice same inode and size
![[Pasted image 20240526115846.png]]
use `ln -s <file_name> <softlink>` to make a symbolic link, you will notice different id,size
![[Pasted image 20240526124207.png]]
use `truncate --size -10M file2.txt` to shrink to this size and `truncate --size +10M file2.txt` to add
![[Pasted image 20240526131259.png]]
