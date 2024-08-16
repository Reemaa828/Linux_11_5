# Creating a network filesystem

# Table of Contents

- [What's a NFS?](#whats-a-nfs)
- [Requirements](#requirements)
- [Host Side](#host-side)
	- [Steps](#steps)
- [Client Side](#client-side)
	- [Steps](#steps)
	- [on qemu](#on-qemu)
- [DONE 🥂](#done-)

# What's a NFS?
NFS allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files.
- it's where a server can export part of its filesystem so that this can be mounted by a client on the local network. This way, changes made to files on the server side are immediately seen by the client, and vice-versa
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/cef10a036f01c887fb222aa03f5602aa/raw/image.png)


# Requirements 
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/acf9aaca365f8ef2dfe325775e7ccc4a/raw/image.png)


# Host Side
## Steps
### 1. Install NFS Server
`sudo apt-get install nfs-kernel-server`

and to **confirm** it 

`systemctl status nfs-kernel-server ` or `ss -tupl | grep "2049"

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/cdae3602197d00fb4bb615bb00d2c370/raw/image.png)

it's working but it exited cause the export file (the file that will contain the files that i plan on sharing) is empty

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/26558e0d4003e60f3274e0ce3bfb8f7d/raw/image.png)

this directory is responsible for defining the files you want to share over network 

### 2. Create Root NFS Directory
```bash
mkdir /srv/nfs-share
sudo chown nobody:nogroup ./nfs-share/
sudo chmod 777  nobody:nogroup ./nfs-share/
```
`
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/ac86e7b4478f21b3bd8916dc90a8747b/raw/image.png)
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/c123279bfd7b3913e7b1592add32bfc6/raw/image.png)

options 

![Screenshot from 2024-08-17 00-33-08.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/97ce69731d4baccbd9f38cb8fd9784fb/raw/Screenshot%20from%202024-08-17%2000-33-08.png)

![Screenshot from 2024-08-17 00-41-40.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d3e2cd6cfbf5c85d1874228c0310c7ff/raw/Screenshot%20from%202024-08-17%2000-41-40.png)
>- rw
>Read-write access: Clients can read and write to the exported file system.
>- sync
Data integrity: Ensures data is written to disk on the server before acknowledging the write operation to the client. This provides stronger data consistency but can impact performance.
>- no_root_squash
Root user preservation: Allows the root user on the client to be treated as the root user on the server. This is a significant security risk.

### 3. Refresh the service 
`systemctl restart nfs-kernel-server` or `exportfs -a`

to **confirm** it

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/bfa17a2ddaefdbea367996f5a8688d76/raw/image.png)

### 4. Copy your real rootfs to new nfs
` sudo  cp -rp ./rf1/ /srv/nfs-share/`

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/67624b3bebc823c4fd59c9464c9b166e/raw/image.png)

# Client Side
## Steps
### 1. make sure that the host IP align with the script to establish a `tab` network interface

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/0b917042e00fdd58e1fb80db9209cafa/raw/image.png)

## on qemu
```bash
sudo qemu-system-arm -M vexpress-a9 -m 256M -kernel ../u-boot/u-boot -nographic -sd ../loop.img -net tap,script=/home/reema/qemu-ifup  -net nic 
uboot_Test=> editenv bootargs
edit: console=ttyAMA0 root=/dev/nfs nfsroot=192.168.1.16:/srv/nfs-share,nfsvers=3,tcp rw init=/sbin/init ip=192.168.1.33:::::eth0
uboot_Test=> saveenv
Saving Environment to FAT... OK
uboot_Test=> editenv serverip 
edit: 192.168.1.16
uboot_Test=> editenv ipaddr   
edit: 192.168.1.33
uboot_Test=> fatload mmc 0:1 0x60000000 zImage
3870984 bytes read in 2272 ms (1.6 MiB/s)
uboot_Test=> fatload mmc 0:1 0x67000000 vexpress-v2p-ca9.dtb
14129 bytes read in 28 ms (492.2 KiB/s)
uboot_Test=> bootz 0x60000000 - 0x67000000
Kernel image @ 0x60000000 [ 0x000000 - 0x3b1108 ]

```

![Screenshot from 2024-08-17 02-23-48.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/1dbab92067a8ca1df5a328a6fe63e091/raw/Screenshot%20from%202024-08-17%2002-23-48.png)

![Screenshot from 2024-08-17 02-23-26.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/9c221ef9dc28f5fc2deb74529259e04c/raw/Screenshot%20from%202024-08-17%2002-23-26.png)

# DONE 🥂
