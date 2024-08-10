
<img src="../../01.Kernel_Module/7.Network_Stack/images/chip.svg" align="left" />

# Emulating SD Card
![Screenshot from 2024-08-10 20-30-16.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/e200a9fdd146246ef45677479fdd9334/raw/Screenshot%20from%202024-08-10%2020-30-16.png&w=700&h=100)
# Table of Contents

- [What does it mean to have virtual SD card?](#what-does-it-mean-to-have-virtual-sd-card)
- [What's a partition?](#whats-a-partition)
- [What's a loop device ?](#whats-a-loop-device-)
	- [How to create a loop device?](#how-to-create-a-loop-device)
- [dd (disk destroyer) command](#dd-disk-destroyer-command)
	- [Usage of dd command](#usage-of-dd-command)
	- [Example coping files](#example-coping-files)
- [Steps for emulating a virtual SD card](#steps-for-emulating-a-virtual-sd-card)

![Screenshot from 2024-08-10 20-35-13.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/dd3af8ccd11d9ae7fb691f911eedda52/raw/Screenshot%20from%202024-08-10%2020-35-13.png)




# What does it mean to have virtual SD card?
- it's just a file on your hard disk
- it's not a partition taken from hard disk or anything.
- you can free space just by deleting the virtual SD card
# What's a partition?
It's a disk management technique of logically dividing the storage device into sections.
There're three types:
1. **primary partition**: it's a bootable partition, it store bootloader, the operating system and other system files. 
2. **Extended partition:** can be used to create multiple logical partitions on a single hard drive. A hard drive can only have one extended partition.
3. **Logical partition:** is a section of a hard drive that is used to store data. It is created within an extended partition and cannot be bootable
# What's a loop device ?
- A loop device is a virtual or pseudo-device which enables a regular file to be accessed as a block device.
- This loop device can then be used to create a new file system. The file system can be mounted, and its contents can be accessed using normal file system APIs.
## How to create a loop device?
#### 1. create a file with the size required
`dd if=/dev/zero of=./loop bs=1M count=512`
![Screenshot from 2024-08-10 17-21-00.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/a38f65d1065150b082954c58d2168b15/raw/Screenshot%20from%202024-08-10%2017-21-00.png)
#### 2. use `losetup -f` to show unused loop devices
![Screenshot from 2024-08-10 17-30-07.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/f6097fbc98d1ec7aa2e1be2b0380ea64/raw/Screenshot%20from%202024-08-10%2017-30-07.png)

#### 3. Creating partitions on loop devices using `cfdisk` or `fdisk`
`cfdisk <file_that_will_be_attached_to_loop_device>`
##### 1. select `dos`
![Screenshot from 2024-08-10 19-12-02.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/108ae1ec837e1bd8db6a2976eae0cdbe/raw/Screenshot%20from%202024-08-10%2019-12-02.png)
##### 2. select `new` to create a partitions on file created
![Screenshot from 2024-08-10 19-12-17.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/4d9a1c3c1d5e9a08dd71a170c2797c9b/raw/Screenshot%20from%202024-08-10%2019-12-17.png)

| Partition Size | Partition Format | Bootable  |
| :------------: | :--------------: | :-------: |
|    `200 MB`    |     `FAT 16`     | ***Yes*** |
|    `300 MB`    |     `Linux`      | ***No***  |

![Screenshot from 2024-08-10 19-56-29.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/2a3b3cadb6a80fb9c1027b178812488b/raw/Screenshot%20from%202024-08-10%2019-56-29.png)

##### 3. attach it to a loop device `losetup -f --show --partscan ./loop `
![Screenshot from 2024-08-10 19-38-07.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/73a24e241326f68a002d57755ea74a92/raw/Screenshot%20from%202024-08-10%2019-38-07.png)
##### 4. make filesytems on each partitions `mkfs.vfat -F 16 -n boot <loop_device>` `mkfs.ext4 -L rootfs <loop_Device>`
![Screenshot from 2024-08-10 19-41-14.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/41b1a8462964f82b74722e0c7070d802/raw/Screenshot%20from%202024-08-10%2019-41-14.png)
##### 5. mounting both partitions `sudo mount <loop_Device> <directory>` by creating a directory then mounting it.
![Screenshot from 2024-08-10 19-49-07.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/09059deafe450246975125def028724f/raw/Screenshot%20from%202024-08-10%2019-49-07.png)

##### 6. confirm it use `fdisk -l` or `lsblk` or `df`
![Screenshot from 2024-08-10 19-46-08.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/f47c91d63f3857d323d6abe7705ca331/raw/Screenshot%20from%202024-08-10%2019-46-08.png)

## Voilà! 🥂
# dd (disk destroyer) command
- clone files/partitions/disks from one place to another.
- one to one situation
- the basic syntax `dd option=<file> option=<file> option=<file>...`
   - `if`:input file 
   - `of`: output file
   - `bs`:block size 
      - the default is 512 bytes.
      - the size is in bytes
  - `skip`: to skip these bytes based on `bs` size from input file to start reading
  - `seek`: to skip these bytes based on `bs`size from output file to start writing 
  - `count`: to copy blocks according to these counts *if not used till the end of file*
## Usage of dd command
- making blank files
- clone disk/partitions 
## Example coping files
1. copying all `file1` to `file2` `dd if=./file1 of=./file2`
   
![Screenshot from 2024-08-10 16-04-59.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/2569f5af1d2b1ac173dac7a9dc412919/raw/Screenshot%20from%202024-08-10%2016-04-59.png)

![Screenshot from 2024-08-10 16-05-45.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/472f79cf3235afe9310e6323bc58f82f/raw/Screenshot%20from%202024-08-10%2016-05-45.png)

2. copying only two characters `dd if=./file1 of=./file2 bs=2 count=1`

![Screenshot from 2024-08-10 16-13-55.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/5eddb31b76ec046f1db0d2a72b5ae08f/raw/Screenshot%20from%202024-08-10%2016-13-55.png)

3. use `xxd` to see the hex representation of file
   
![Screenshot from 2024-08-10 16-14-57.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/7e82bb95a34eb85a49c06b8877cd85f0/raw/Screenshot%20from%202024-08-10%2016-14-57.png)

4. skipping `hello` from `input file` and skipping `he` from `output file`

![Screenshot from 2024-08-10 16-12-53.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/ab5bc77babe81744bbc06072f54e6bdd/raw/Screenshot%20from%202024-08-10%2016-12-53.png)


# Steps for emulating a virtual SD card
1. `dd if=/dev/zero of=sd.img bs=1M count=1024` making a 1G file by writing 1 mega bytes of zeros 1024 times.
   
![Screenshot from 2024-08-10 16-28-10.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/fa83ada5a12e6233aa2520d6cfc3fcdf/raw/Screenshot%20from%202024-08-10%2016-28-10.png)

![Screenshot from 2024-08-10 16-26-33.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/bb27d8361e23fac1ff23a0d173572ffc/raw/Screenshot%20from%202024-08-10%2016-26-33.png)

then continue with [How to create a loop device?](#how-to-create-a-loop-device)
