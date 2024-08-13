

# Building kernel for Vexpress-a9 
```bash
cd linux/
make ARCH=arm CROSS_COMPILE=<toolchain_prefix> vexpress_defconfig
make ARCH=arm CROSS_COMPILE=<toolchain_prefix> menuconfig
```
For all the next board this configuration must be checked
- [x]  Enable **devtmpfs**

![Screenshot from 2024-08-13 15-40-00.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/3a45af502c8fe0c02a210dfa2909a89a/raw/Screenshot%20from%202024-08-13%2015-40-00.png)

- [x]  Change kernel compression to **XZ**

![Screenshot from 2024-08-13 15-42-59.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/48107ec1918d165cd22716fdc4c15203/raw/Screenshot%20from%202024-08-13%2015-42-59.png)

- [x]  Change your kernel local version to your name and append on it -v1.0

![Screenshot from 2024-08-13 15-46-45.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/4cd4927a6e689b8c6d0a93f0466ffbc4/raw/Screenshot%20from%202024-08-13%2015-46-45.png)

build the kernel `make -j 8 ARCH=arm CROSS_COMPILE=<toolchain_prefix> zImage dtbs modules`
>- **-j 8:** This option tells make to use up to 8 parallel jobs for building, which can significantly speed up the compilation process.
>- **ARCH=arm:** Sets the architecture to ARM.
>- **CROSS_COMPILE:** Specifies the prefix for the cross-compiler used to build code for the target architecture. In this case, it's `arm-cortexa9_neon-linux-musleabihf-`.
>- **zImage:** This is the target to build. It's a compressed kernel image suitable for bootloaders like U-Boot.
>- **dtbs:** This target instructs the build system to build the device tree blobs (DTBs). Device tree is a data structure that describes the hardware board.
>- **modules:** This is another target to build. It creates kernel modules that can be loaded dynamically at runtime.
![Screenshot from 2024-08-13 15-53-58.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/1f2aeb3230e2bb8736ad4c0f65e9b686/raw/Screenshot%20from%202024-08-13%2015-53-58.png)

install the modules to root filesystem of your target `make -j 8 ARCH=arm CROSS_COMPILE=<toolchain_prefix> INSTALL_MOD_PATH=<PATH_OF_TARGET_FS> module_install` 
![Screenshot from 2024-08-13 16-21-37.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/11e30d0efdfc75d59804b3e5997373e6/raw/Screenshot%20from%202024-08-13%2016-21-37.png)


# Booting kernel using virtual sd

## Steps 
### 1. copy these file to boot partition of virtual sd card
`zimage` & `device tree` to virtual sd card
![Screenshot from 2024-08-13 16-37-00.png](https://itg.singhinder.com/?url=https://gist.githubusercontent.com/Reemaa828/153b40ffd89f8ea82b5c4e33109c1e61/raw/Screenshot%20from%202024-08-13%2016-37-00.png)

### 2. Test on qemu
```bash
qemu-system-arm -M vexpress-a9 -m 128M -kernel ./u-boot/u-boot -nographic -sd ./loop.img 

setenv bootargs console=ttyAMA0
saveenv
fatload mmc 0:1 0x60000000 zImage
fatload mmc 0:1 0x65000000 vexpress-v2p-ca9.dtb
bootz 0x60000000 - 0x65000000
```

![Screenshot from 2024-08-13 23-30-46.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/5f8683052cccd1b0a4e00ee99f615ecc/raw/Screenshot%20from%202024-08-13%2023-30-46.png)

![Screenshot from 2024-08-13 23-34-06.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/f99acca3c6d2b47a531610770af26ee4/raw/Screenshot%20from%202024-08-13%2023-34-06.png)

![Screenshot from 2024-08-13 23-46-09.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/45fd6b8b29da6981fd5e84321dcee40d/raw/Screenshot%20from%202024-08-13%2023-46-09.png)


# Booting kernel via TFTP
## Steps
### 1. copy zImage and device tree to /srv/tftp

