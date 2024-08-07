## Session Pre-requisites 
```
sudo apt install libncurses5-dev flex bison # for menuconfig tool.
sudo apt install subversion libssl-dev
sudo apt-get install qemu-system
sudo apt-get install device-tree-compiler
```
---------------------------------------------------------------------------------------------
## Session Goals:
1. Download/Choose Kernel.
2. Understand kernel versioning system [stable-mainline-long-term-support]
3. Understand kernel source code tree.
4. Understand Device tree.
5. Understand menuconfig/kconfig/kbuild.
6. Compiling Kernel for raspi3/4
7. Identify Kernel size.
8. Compiling device tree.
9. Booting kernel on Qemu.
---------------------------------------------------------------------------------------------
## 1. Actions to Build Kernel
---------------------------------------------------------------------------------------------
- Choose Kernel Version.
- Downloading Source code.
- Understand Kernel config.
- Identify Kernel Version.
- **Compiling** kernel for specific target.
- Build artifcats [Targets]:
  - VMLinux: elf version.
  - zImage: compressed version (u-boot understand it through `bootz` command)
  - uImage: for uBoot.
---------------------------------------------------------------------------------------------
### 1. Download/Choose Kernel.
a. from vendor
```
source: https://www.raspberrypi.com/documentation/computers/linux_kernel.html\

git clone --depth=1 -b rpi-4.19.y https://github.com/raspberrypi/linux.git # raspberrypi linux version.
[Kernel.org](https://www.kernel.org/) # Main branches [Kernel Archives]

```

b. from kernel main branch.

```
1. [Kernel.org](https://www.kernel.org/) # Main branches [Kernel Archives]
2. wget <version link from kernel.org>
3. tar xf <compressed filename>
```

---------------------------------------------------------------------------------------------
### 2. Understand Kernel source code structure.
![Screenshot 2023-10-14 at 10 55 21 AM](https://github.com/embeddedlinuxworkshop/M2-S3/assets/139722851/55da2354-2750-45fa-a703-6e75d47753de)

---------------------------------------------------------------------------------------------
### 3. Understanding Kernel config mechanism.
1. menuconfig ---> read KConfig ---> output .config

```
make ARCH=<target> menuconfig

# choose configs
make ARCH=<target> <filename>
```
3. Kbuild     ---> read .config ---> add parameters inside code as Macros.

5. build :) 
---------------------------------------------------------------------------------------------
### 4. Identify Kernel Version.

```
make ARCH=<target> kernelrelease 
```
---------------------------------------------------------------------------------------------
### 5. Compiling kernel for specific target.
0. Most Important Targets:
   
a. VMLinux
b. zImage
c. uImage.
d. distclean

---------------------------------------------------------------------------------------------

2. Most Used *make targets*.
---------------------------------------------------------------------------------------------
a. distclean

```
make ARCH=<target> distclean
```
b. vmLinux: elf

```
make -j 4 ARCH=<target> CROSS_COMPILE=`prefix`

# output
> main directory for Linux.
```

c. zImage: compressed.

```
make -j4 ARCH=<target> CROSS_COMPILE=`prefix` Image

# output
<linux_directory>/arch/<target>/boot
```

d. uImage: zImage + uBoot header.

```
make -j4 ARCH=<target> CROSS_COMPILE=`prefix` LOADADDR=0x80008000 uImage

# output
<linux_directory>/arch/<target>/boot
```
---------------------------------------------------------------------------------------------
