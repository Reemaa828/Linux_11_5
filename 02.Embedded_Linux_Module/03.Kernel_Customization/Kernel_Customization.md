

<img src="../../images/linux-svgrepo-com.svg" align="left" width="82">

# Kernel Customization
![Screenshot from 2024-08-01 06-13-05](https://github.com/user-attachments/assets/4b948b64-3c3d-4dd5-9d78-3de2bd3bc9b2)

# Table of Contents 

- [Steps for Kernel Customization 📃](#steps-for-kernel-customization-)
- [Getting Started 🚀](#getting-started-)
  
- [Steps using Main Branch Repository 🏗️](#steps-using-main-branch-repository-)
	- [1. Downloading Kernel Source code](#1-downloading-kernel-source-code)
	- [2. Configure Kernel](#2-configure-kernel)
	- [3. Build Kernel](#3-build-kernel)
- [Steps using vendor path🏗️](#steps-using-vendor-path)
	

# Steps for Kernel Customization 📃
1. Downloading kernel source code either from repository or from vendor. (uncustomized kernel).
2. Configure the kernel according to your target specifications using menuconfig. (customized kernel)`
3. Build Kernel using the cross platform toolchain. (image)
4. Booting kernel to the target or qemu.
# Getting Started 🚀

``` mermaid 
flowchart TD

A(Source code) --> B(Main Branch repo ) --> C(https://github.com/torvalds/linux/releases/tag/v6.4)

A --> D(Vendor branch) --> E( https://www.raspberrypi.com/documentation/computers/linux_kernel.html)
```
# Steps using Main Branch Repository 🏗️
##  1. Downloading Kernel Source code  

you should use the same version or older as your toolchain kernel header files version.
```bash
#Navigate to your crosstool directory and display the configuration.
cd crosstool-ng/
bin/ct-ng show-config
```
![Screenshot from 2024-08-01 07-46-03](https://github.com/user-attachments/assets/47ebecf6-78d5-42c3-a9c1-2a5f2a20b489)

go to the repo and to the tags to choose the right version of the source code

![Screenshot from 2024-08-01 07-49-11](https://github.com/user-attachments/assets/e075ac95-3202-49ef-913e-fa2cd520f0ed)

use `tar xf <compressed source code> --directory <directory__path>` to extract the file.

![Screenshot from 2024-08-01 07-55-30](https://github.com/user-attachments/assets/53ad1a42-dfbb-4066-b754-6940c7b96ac9)

## 2. Configure Kernel

![Pasted image 20240801083014](https://github.com/user-attachments/assets/daa7797b-297e-4ecf-8d22-c738a05fa1c9)


- The menuconfig reads the Kconfig (which is the default configuration of every directory in the kernel source). if a directory have Kconfig then this directory can be customized.
  
use `make ARCH=<architectutre_target> CROSS_COMPILE=<toolchain_prefix> menuconfig`

![Screenshot from 2024-08-01 08-39-46](https://github.com/user-attachments/assets/e75d8e57-219c-4a5d-8598-b86b23842730)

***kconfig file is a hierarchical file based on " key value pair " ,  all of these files in the Kconfig can be configured***

![Screenshot from 2024-08-01 08-37-53](https://github.com/user-attachments/assets/a0afaea4-2e79-4e39-9f15-c8978eeb6d48)

- after  menuconfig reads the Kconfig of every directory, you can change the configurations according to the specifications of your board or the requirements of your project.

-  then load configuration to generate .config file by clicking `save`

![Screenshot from 2024-08-01 08-40-37](https://github.com/user-attachments/assets/8b0f9079-0328-4267-ad7e-d185d6a3d203)


>[!NOTE]
why choose the architecture configuration for the kernel customization❓  
>The kernel repo itself doesn't directly contain board-specific or Soc-specific configurations and specifying the architecture provides a more general and flexible approach to kernel configuration.

## 3. Build Kernel

When Kbuild customized the kernel source code according to the .config file, it's time to build it to create the **image** that must be compatible with the boot-loader.

- use `make ARCH=arm64 CROSS_COMPILE=<prefix_of_toolchain> <target> `
- use `make ARCH=arm64 CROSS_COMPILE=<prefix_of_toolchain> ` for vmlinux
- use `make ARCH=arm64 CROSS_COMPILE=<prefix_of_toolchain> Image` for zimage
- use `make ARCH=arm64 CROSS_COMPILE=<prefix_of_toolchain> LOADADDR=<ADDRESS> uImage` for uImage

![Screenshot from 2024-08-01 09-47-28](https://github.com/user-attachments/assets/69c7a788-1da8-417b-86e8-6db36443ea81)

### what `<target>` should be used when generating the image ❓

### the `image` must be boot-loader compatible . 
### The four types of `<target>` :
### vmlinux

- Uncompressed elf file.
- Primarily used for debugging purposes.
- Not directly bootable.

### zImage

- Compressed version of vmlinux .
- Suitable for older bootloaders with limited memory.
- Primarily used for x86 architectures.

### uImage

- Compressed kernel image with a header containing information about the image, such as compression type, image size, and load address.
- More flexible than zImage and can be used on various architectures.
- Commonly used with modern bootloaders like U-Boot.

### distclean
- not an image but its a command used in the build process.
- it removes configuration, generated config files and resets the build environment to start from scratch.


____
# Steps using vendor path 🏗️

It's the straightforward path because of the vendor documentation.
[vendor steps to cross compiler the kernel and build it](https://www.raspberrypi.com/documentation/computers/linux_kernel.html#cross-compile-the-kernel)

steps for raspi3+ arm64  
```bash
sudo apt install git
git clone --depth=1 --branch <branch> https://github.com/raspberrypi/linux
```
branch names from the vendor git repo

![Screenshot from 2024-08-01 10-05-16](https://github.com/user-attachments/assets/c95103b6-751a-4ac2-99e1-d577068cf4a4)


```bash
sudo apt install bc bison flex libssl-dev make libc6-dev libncurses5-dev
cd linux
KERNEL=kernel8
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcm2711_defconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image modules dtbs
```

## it's done without configuring anything in the kernel because it's already customized for our board 😼

![Screenshot from 2024-08-01 10-21-22](https://github.com/user-attachments/assets/37fd74c7-256f-4939-b0dd-38353fa963ae)
















