# Creating & Managing Dual Rootfs
# Table of Contents

- [Steps](#steps)
	- [1. Unmount and detach all files from loop device](#1-unmount-and-detach-all-files-from-loop-device)
	- [2. Create a third partition for a second rootfs](#2-create-a-third-partition-for-a-second-rootfs)
	- [3. make a script to control dual rootfs](#3-make-a-script-to-control-dual-rootfs)
	- [4. make `rootfs1` and `rootfs2` then mount them](#4-make-rootfs1-and-rootfs2-then-mount-them)
	- [5. edit `init.d/rcS` to match the dual rootfs sequence](#5-edit-initdrcs-to-match-the-dual-rootfs-sequence)
	- [6. Testing on QEMU](#6-testing-on-qemu)
- [Errors that faced me](#errors-that-faced-me)
	- [1. devtmpfs is not mounted after i did it in the configuration](#1-devtmpfs-is-not-mounted-after-i-did-it-in-the-configuration)
	- [2. busybox is statically linked do not include library files as it will form conflict](#2-busybox-is-statically-linked-do-not-include-library-files-as-it-will-form-conflict)
	- [3. Error in filesystem running `bootmanager` script for second time](#3-error-in-filesystem-running-bootmanager-script-for-second-time)
   
# Steps

## 1. Unmount and detach all files from loop device 
```bash
sudo umount /dev/loop24p1
sudo umount /dev/loop24p2
sudo losetup -d /dev/loop24
```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d626be981b4d77670485fb940409a5fa/raw/image.png)

## 2. Create a third partition for a second rootfs
```bash
cfdisk ./loop.img 
sudo losetup -f --show --partscan ./loop.img
sudo mkfs.vfat -F 16 -n boot /dev/loop24p1
sudo mkfs.ext4 -L rootfs1 /dev/loop24p2
sudo mkfs.ext4 -L rootfs2 /dev/loop24p3
sudo mount /dev/loop24p1 ./boot
```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/44aeb77cff6627dbd8af9b2cb9605256/raw/image.png)
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/f33cf612129b2af65fa684b92a1aaa17/raw/image.png)

## 3. make a script to control dual rootfs
```bash
#!/bin/sh
echo "Select an option:"
echo "1) rootfs_one"
echo "2) rootfs_two"
echo "3) quit"
read -r choice

case $choice in
    1)
        mkdir  /mnt/rootfs_one
        mount dev/mmcblk0p2 /mnt/rootfs_one
        /usr/sbin/chroot /mnt/rootfs_one
        ;;
    2)
        mkdir  /mnt/rootfs_two
        mount  /dev/mmcblk0p3 /mnt/rootfs_two
        /usr/sbin/chroot /mnt/rootfs_two
        ;;
    3)
        echo "Quitting..."
        break
        ;;
    *)
        echo "Wrong selection"
        ;;
esac

```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/64e8e5d23227cf2178ba3f8e56b91ca9/raw/image.png)

## 4. make `rootfs1` and `rootfs2` then mount them
```bash
mkdir initramfs2
rsync -avh ../initramfs/* ../initramfs2
sudo mount /dev/loop24p2 ./initramfs
sudo mount /dev/loop24p2 ./initramfs2
```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/add058a1dae1ac069f6af839ed3bad1a/raw/image.png)

## 5. edit `init.d/rcS` to match the dual rootfs sequence
```bash
#!/bin/sh
# mount a filesystem of type `proc` to /proc
mount -t proc nodev /proc
# mount a filesystem of type `sysfs` to /sys
mount -t sysfs nodev /sys
# you can create `/dev` and execute `mdev -s` if you missed the `devtmpfs` configuration
sh /bootmanager.sh 

```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/abdcbb500a05b968b907169af119d432/raw/image.png)

## 6. Testing on QEMU
```bash
 cd ./boot/
sudo qemu-system-arm -M vexpress-a9 -m 256M -kernel ../u-boot/u-boot -nographic -sd ../loop.img 
uboot_Test=> setenv bootargs 'console=ttyAMA0 root=/dev/mmcblk0p2 rootfstype=ext4 rw rootwait init=/sbin/init' 
uboot_Test=> saveenv
Saving Environment to FAT... OK
fatload mmc 0:1 0x60000000 zImage
3870984 bytes read in 2656 ms (1.4 MiB/s)
uboot_Test=> fatload mmc 0:1 0x67000000 vexpress-v2p-ca9.dtb 
14129 bytes read in 28 ms (492.2 KiB/s)
uboot_Test=> bootz 0x60000000 - 0x67000000

```

![Screenshot from 2024-08-16 18-07-41.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/0c20691f4aa5492f6d673d2b531b2121/raw/Screenshot%20from%202024-08-16%2018-07-41.png)


![Screenshot from 2024-08-16 18-07-49.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/991304422668bab1cc679f0313880f1d/raw/Screenshot%20from%202024-08-16%2018-07-49.png)

![Screenshot from 2024-08-16 18-07-57.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/03cc55bfbba82ecfd51f853489320197/raw/Screenshot%20from%202024-08-16%2018-07-57.png)



# Errors that faced me
## 1. devtmpfs is not mounted after i did it in the configuration
if you noticed the sequence you will see that the fs is mounted then devtmpfs after choosing another one, the devtmpfs is not mounted.

![Screenshot from 2024-08-16 18-12-48.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d9118d7a9c89f748eedb5393c9c4aac8/raw/Screenshot%20from%202024-08-16%2018-12-48.png)

solution mount devtmpfs in userspace
```bash
mount -t devtmpfs anysha /dev
```
## 2. busybox is statically linked do not include library files as it will form conflict

difference between the rootfs with library (didn't work error)

![Screenshot from 2024-08-16 18-48-25.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/5502d6569c5d3141d5dd7dabe0be8c14/raw/Screenshot%20from%202024-08-16%2018-48-25.png)

without library

![Screenshot from 2024-08-16 18-45-47.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/14969cfa7974e730714e51fece32bd83/raw/Screenshot%20from%202024-08-16%2018-45-47.png)

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/735611e1a424dc8718d47f493402f3ad/raw/image.png)

## 3. Error in filesystem running `bootmanager` script for second time
```bash
does error have to do with this script error : EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mkdir: deleted inode referenced: 25613

mkdir: can't create directory '/mnt/rootfs_two': No error information

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mount: deleted inode referenced: 25613

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mount: deleted inode referenced: 25613

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mount: deleted inode referenced: 25613

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mount: deleted inode referenced: 25613

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mount: deleted inode referenced: 25613

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm mount: deleted inode referenced: 25613

mount: mounting /dev/mmcblk0p3 on /mnt/rootfs_two failed: No error information

EXT4-fs error (device mmcblk0p2): ext4_lookup:1853: inode #25607: comm chroot: deleted inode referenced: 25613
```
fix by running `fsck`

![Screenshot from 2024-08-16 20-06-21.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/88cc1a97fd065fe16c9f51fe569edef5/raw/Screenshot%20from%202024-08-16%2020-06-21.png)


