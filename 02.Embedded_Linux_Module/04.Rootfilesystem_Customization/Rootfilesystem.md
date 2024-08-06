
<img src="../../images/noun-file-system-icon-5625068.svg" align="left" width="90">

# Custom Rootfilesystem
# Table of Contents

- [Result from booting without a Rootfilesystem](#result-from-booting-without-a-rootfilesystem)
- [Paths to create a Rootfilesystem](#paths-to-create-a-rootfilesystem)
- [Creating a Rootfilesystem manually](#creating-a-rootfilesystem-manually)
	- [(1. & 2.)Create the essential directories needed by kernel](#1--2create-the-essential-directories-needed-by-kernel)
	- [(3. & 4.) use `busybox` to add essential commands & init. program](#3--4-use-busybox-to-add-essential-commands--init-program)
	- [5. make an application and cross compile it](#5-make-an-application-and-cross-compile-it)
- [Adding libraries that will be needed in the target](#adding-libraries-that-will-be-needed-in-the-target)
- [Archive the rootfilesystem created and compress it.](#archive-the-rootfilesystem-created-and-compress-it)
- [Testing on qemu](#testing-on-qemu)
- [What is a `busybox`?](#what-is-a-busybox)



# Result from booting without a Rootfilesystem


![Screenshot from 2024-08-06 10-42-37.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6a6b1ce50325cc444d2c852cea0b94e6/raw/Screenshot%20from%202024-08-06%2010-42-37.png)


## Kernel Panic: it's a system error when kernel can't fix the error and unable to complete the booting.
___
# Paths to create a Rootfilesystem
![Screenshot from 2024-08-06 12-49-18.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/5892549135afef8955c5a4e1a91a8cca/raw/Screenshot%20from%202024-08-06%2012-49-18.png)

### I'm choosing the initramfs path 🤓
___
# Creating a Rootfilesystem manually

![Screenshot from 2024-08-06 13-05-12.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/fd6e1c86a3183db13a0c128cf9fdb3bc/raw/Screenshot%20from%202024-08-06%2013-05-12.png)

## (1. & 2.)Create the essential directories needed by kernel
use `man hier` to show the essential directories needed in the rootfilesystem.
![Screenshot from 2024-08-06 12-59-07.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/4941ee1a8c730fb9efda665724ce972b/raw/Screenshot%20from%202024-08-06%2012-59-07.png)

## (3. & 4.) use `busybox` to add essential commands & init. program
#### 1. use `git clone git://busybox.net/busybox.git` to download the source code 
![Screenshot from 2024-08-06 13-18-24.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/96a0c613b3db43318efc1a326d1fcfe7/raw/Screenshot%20from%202024-08-06%2013-18-24.png)

### 2. use `make ARCH=arm64 CROSS_COMPILE=<toolchain_prefix> menuconfig` to configure the busybox
![Screenshot from 2024-08-06 13-27-39.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/c17e9386918e238209507720303ccacf/raw/Screenshot%20from%202024-08-06%2013-27-39.png)
![Screenshot from 2024-08-06 13-28-22.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/fe2749f83b52a072656d198e1e528074/raw/Screenshot%20from%202024-08-06%2013-28-22.png)
>[!TIP] You have to do it two times to get the .config file 
### 3. use `make ARCH=arm64 CROSS_COMPILE=<toolchain_prefix>` to build the utilities to work on your specific target
![Screenshot from 2024-08-06 13-34-46.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/225155546da0721404dff3aa2684a2a1/raw/Screenshot%20from%202024-08-06%2013-34-46.png)


### 4. Then move all binaries to the created root filesystem
open **.config** and search for `CONFIG_PREFIX="./_install"`
![Screenshot from 2024-08-06 13-36-46.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/871df1de5526db75c90f16faf9ea02f4/raw/Screenshot%20from%202024-08-06%2013-36-46.png)
change it to the path of the created rootfilesystem to install all needed utilities into their suited directories.

![Screenshot from 2024-08-06 13-39-55.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/205ddbf3a183f492c4bcad52f2a1d9bd/raw/Screenshot%20from%202024-08-06%2013-39-55.png)

![Screenshot from 2024-08-06 13-42-20.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/9e3b40a577a548d6ade95000ff877a11/raw/Screenshot%20from%202024-08-06%2013-42-20.png)

you will find that all utilities are a soft links to executable busybox. now you have successfully installed all needed binaries ✅

## 5. make an application and cross compile it 

![Screenshot from 2024-08-06 14-02-23.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/5efa6d56ce153e58c230a22c313795cc/raw/Screenshot%20from%202024-08-06%2014-02-23.png)




# Adding libraries that will be needed in the target
without libraries the application cannot run correctly or compile correctly
![Screenshot from 2024-08-06 14-24-35.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/f273c6dcdc004e7aff9f6e8d51924582/raw/Screenshot%20from%202024-08-06%2014-24-35.png)
use ` aarch64-rpi3-linux-gnu-readelf --all /home/reema/rootffs/application/a.out | grep "Shared"` to show needed library
![Screenshot from 2024-08-06 14-11-29.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/35a5143f49a655839c5cc40319c3a0f4/raw/Screenshot%20from%202024-08-06%2014-11-29.png)

## you will find the needed libraries in the toolchain
go to sysroot and use `rsync -avh <source> <destination>` to add missing files and libraries 
![Screenshot from 2024-08-06 14-29-02.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d7d186c17796fffd85299a88363195fe/raw/Screenshot%20from%202024-08-06%2014-29-02.png)
![Screenshot from 2024-08-06 14-38-02.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/7739ef6290f0dc499346091ddb6575a1/raw/Screenshot%20from%202024-08-06%2014-38-02.png)

# Archive the rootfilesystem created and compress it.
![Screenshot from 2024-08-06 14-43-34.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/8839fca43f45acda87ce7b102aa6c979/raw/Screenshot%20from%202024-08-06%2014-43-34.png)

use `find . -print0 | cpio --null -ov --format=newc` to create an archive
`find . -print0` and `gzip <output>` to compress it.

> `find .`: This part of the command initiates a search for files starting from the current directory (represented by .).
> 
> `-print0`: This option tells find to print the full path of each found file, followed by a null character (instead of a newline). This is crucial for handling filenames with spaces or special characters correctly.
> 
> `| cpio --null -ov --format=newc`
> 
> `|`: The pipe character sends the output of the find command to the cpio command as input.
> 
> `cpio`: This is the command used to create the archive.
> 
> `--null`: This option tells cpio to expect null-terminated filenames (matching the output of find -print0).
> 
> `-o`: This option indicates that cpio should create an archive.
> 
> `-v`: This is the verbose option, providing detailed output about the archiving process.
> 
> `--format=newc`: This specifies the format of the archive to be created, which is the newc format.

![Screenshot from 2024-08-06 14-57-20.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d2170ef0aa5abb015c1f765479b3633d/raw/Screenshot%20from%202024-08-06%2014-57-20.png)

# Testing on qemu
```bash
qemu-system-aarch64 -M raspi3b -cpu cortex-a53 -m 1G -kernel Image -append " console="ttyAMA0" rdinit="/bin/sh" -initrd /home/reema/roottfs -nographic 
```
![Screenshot from 2024-08-06 15-23-53.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/700f171cc977aa2cdf6f498f5258f58c/raw/Screenshot%20from%202024-08-06%2015-23-53.png)

# What is a `busybox`?
- A busybox is a collection of UNIX utilities in a form of a single binary.
![Screenshot from 2024-08-06 15-25-46.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/a15c0fd770c3f884ac65a5317dfdfa61/raw/Screenshot%20from%202024-08-06%2015-25-46.png)

- usually these utilities are stripped down.
![Screenshot from 2024-08-06 15-27-42.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/a30fe5da98d8fd0e901a609012b29f51/raw/Screenshot%20from%202024-08-06%2015-27-42.png)

    - The implementation of utilities removes the uncommon, rarely used command options. Everything fits under 1 MB and this minimal image is the reason why it has gained popularity among **embedded system** and IoT domain.
- it even contains an init command which can be launched as PID 1.
