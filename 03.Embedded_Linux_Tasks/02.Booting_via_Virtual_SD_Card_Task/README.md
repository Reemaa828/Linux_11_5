# Booting via virtual SD card
# Table of Contents

- [Build tool-chain suitable for your machine](#build-tool-chain-suitable-for-your-machine)
- [Build Bootloader for your machine](#build-bootloader-for-your-machine)
- [Testing on QEMU](#testing-on-qemu)
- [Adding Fake image and Fake dtb to virtual sd](#adding-fake-image-and-fake-dtb-to-virtual-sd)


# Build tool-chain suitable for your machine
```bash
./bin/ct-ng list-samples
./bin/ct-ng arm-cortexa9_neon-linux-gnueabihf
./bin/ct-ng show-arm-cortexa9_neon-linux-gnueabihf
./bin/ct-ng build
```
![Screenshot from 2024-08-11 00-27-18.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/527fad4279dcf97985fb832852d631bb/raw/Screenshot%20from%202024-08-11%2000-27-18.png)
![Screenshot from 2024-08-11 16-03-06.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/0cf619c6bfd5563fba6f3832c9d909cc/raw/Screenshot%20from%202024-08-11%2016-03-06.png)

# Build Bootloader for your machine
```bash
cd ./configs
make <configs>
make menuconfig
make CROSS_COMPILE=<toolchaiin_prefix>

```
**The customer requirement are like following**:

- [x] Support **editenv**.
![Screenshot from 2024-08-11 00-46-14.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/ed6d55a65d29194a5dea8c7ac5524761/raw/Screenshot%20from%202024-08-11%2000-46-14.png)

- [x] Support **bootd**.
![Screenshot from 2024-08-11 01-10-44.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/0d41f699460f41fdfed6dff6352e63ba/raw/Screenshot%20from%202024-08-11%2001-10-44.png)

- [x] Store the environment variable inside file call **uboot.env**.
![Screenshot from 2024-08-11 01-11-42.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/011fadfb58152f820f1ec8bd3868cca0/raw/Screenshot%20from%202024-08-11%2001-11-42.png)

- [x] Unset support of **Flash**
![Screenshot from 2024-08-11 01-11-42.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/b2dc2830f68e86e6d451080a0ac9eccb/raw/Screenshot%20from%202024-08-11%2001-11-42.png)

- [x] Support **FAT file system**
  - [x] Configure the FAT interface to **mmc**
  - [x] Configure the partition where the fat is store to **0:1**
  ![Screenshot from 2024-08-11 01-15-09.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/b6c7bb70f1dd32659c792d7eca2cf707/raw/Screenshot%20from%202024-08-11%2001-15-09.png)

then build bootloader with the specified toolchain with the configuration
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/430bc1d8615236c489931e488ee85132/raw/image.png)



# Testing on QEMU
```bash
qemu-system-arm -M vexpress-a9 -m 128M -nographic -kernel u-boot -sd ../loop.img 

```
![Screenshot from 2024-08-11 16-39-36.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6b6a8da1f6b84e98119f499a3e4dda25/raw/Screenshot%20from%202024-08-11%2016-39-36.png)


# Adding Fake image and Fake dtb to virtual sd
---to be contuined
