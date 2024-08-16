# Initramfs and Disk Image via statically linked Busybox

>[!TIP]
>-  make sure the kernel version and the kernel header files are compatible and lower than 6.8
>  - [error encountered by busybox for kernel versions](http://lists.busybox.net/pipermail/busybox-cvs/2024-January/041752.html)
>  ![Screenshot from 2024-08-14 18-00-56.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/be21cc2e228465e01ce2b0fc58b34a5d/raw/Screenshot%20from%202024-08-14%2018-00-56.png)

> ![Screenshot from 2024-08-14 18-01-30.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/25dd9a6323991957bd768f89a0c5e852/raw/Screenshot%20from%202024-08-14%2018-01-30.png)




# Ways for transferring the rootfs 

![Screenshot from 2024-08-14 16-46-09.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/2d63e484e908b04772410db6cdfe7926/raw/Screenshot%20from%202024-08-14%2016-46-09.png)

# Creating a initramfs
##### 1. You need to configure your kernel with `CONFIG_BLK_DEV_INITRD` to support initramfs.

![Screenshot from 2024-08-14 16-51-07.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/af0db9e8a857ef95b02db34d0a891da8/raw/Screenshot%20from%202024-08-14%2016-51-07.png)

##### 2. making a statically linked busybox
1. install busybox
```bash
git clone https://github.com/mirror/busybox.git
```
![Screenshot from 2024-08-14 16-59-57.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/ac5da981afec7535a4016f2cb9b59052/raw/Screenshot%20from%202024-08-14%2016-59-57.png)

2. open menuconfig and make these choices
   
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/7f1683739a95593383d24ef6b92903bd/raw/image.png)
change this so all the utilities be installed to initramfs

![Screenshot from 2024-08-14 17-07-56.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/c165e461815970be9f3a141c70f775de/raw/Screenshot%20from%202024-08-14%2017-07-56.png)

![Screenshot from 2024-08-14 17-07-51.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/02806eeb07a269d6000e52ca0946106f/raw/Screenshot%20from%202024-08-14%2017-07-51.png)


![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/5ec5a6778df202b163aec307c7dbc95d/raw/image.png)

3. Build the busybox and install it 
```bash
make ARCH=arm CROSS_COMPILE=arm-cortexa9_neon-linux-musleabihf-
make ARCH=arm CROSS_COMPILE=arm-cortexa9_neon-linux-musleabihf- install
#This will copy the binary to the directory configured in CONFIG_PREFIX and
#create all the symbolic links to it.
```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/997eb45e848f497d4c8afed91270da1b/raw/image.png)

![Screenshot from 2024-08-14 20-21-47.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/7c53b40abff683b463d567fa4eecff1f/raw/Screenshot%20from%202024-08-14%2020-21-47.png)

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/bee946b3a88548b26f558ca2f1e5ece1/raw/image.png)
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/3eafab1678d53ff909356585438ab798/raw/image.png)

4. populate the modules of kernel in initramfs by using `sudo make ARCH=arm CROSS_COMPILE=arm-cortexa9_neon-linux-musleabihf-INSTALL_MOD_PATH=/home/reema/initramfs modules_install`

![Screenshot from 2024-08-14 21-07-46.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/a8b120261c213fbc6f2606bf42d63ff9/raw/Screenshot%20from%202024-08-14%2021-07-46.png)

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/9f4ee3588dff85e721ace37982a1e747/raw/image.png)

5. make these directories and files for init process  
>- Init begins by reading the configuration file `/etc/inittab`
>- The first line runs a shell script, rcS, when init is started. The second line prints the message Please press Enter to activate this console to the console, and starts a
>shell when you press Enter. The leading - before /bin/ash means that it will be a
>login shell, which sources /etc/profile and $HOME/.profile before giving the
>shell prompt

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/56af0715cbf30ecbd2928fd62f7a65c3/raw/image.png)

6. populate the libraries or shared objects from toolchain to initramfs
`sudo rsync -avh ../sysroot/ $HOME/initramfs`

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/c28eed9f8c56454103b4aaaae93e1985/raw/image.png)

7. make sure the owner is root
   
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/4eed87b660ff729a53fd048d0d70d105/raw/image.png)

8. then mount on virtual sd 
```bash
losetup -f --show --partscan ./loop.img 
cp -rp ./initramfs ./rootfs
sudo mount /dev/loop24p2 ./rootfs
sudo mount /dev/loop24p1 ./boot
```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/252328e93f72678f4091b5ad97c834f5/raw/image.png)

##### Standalone initramfs
1. create a boot ramdisk as a standalone `cpio` archive, *it means that you have two files to deal with instead of one and not all bootloaders have the facility to load a separate ramdisk.*
   
![Screenshot from 2024-08-14 22-25-02.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/eecebc9a977dad86029db61f62804af6/raw/Screenshot%20from%202024-08-14%2022-25-02.png)


# Boot and Test on QEMU standalone initrams
## first method 
```bash
qemu-system-arm -M vexpress-a9 -m 128M -kernel ./boot/zImage -nographic  -initrd ./initramfs.cpio.gz -append "console=ttyAMA0 rdinit=/bin/sh" -dtb ./boot/vexpress-v2p-ca9.dtb 
```

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/41e72c139cf507bf97390517487b3b6f/raw/image.png)
![Screenshot from 2024-08-15 01-20-38.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/dd23568616ee8642ac1974ef97abb262/raw/Screenshot%20from%202024-08-15%2001-20-38.png)

## Second method
```bash
mkimage -A arm -O linux -T ramdisk -d initramfs.cpio.gz uRamdisk
fatload mmc 0:1 0x60000000 zImage                           
3904744 bytes read in 2285 ms (1.6 MiB/s)
uboot_Test=> fatload mmc 0:1 0x64000000 vexpress-v2p-ca9.dtb
14129 bytes read in 28 ms (492.2 KiB/s)
uboot_Test=> fatload mmc 0:1 0x66000000 uRamdisk            
23070765 bytes read in 13666 ms (1.6 MiB/s)
uboot_Test=> editenv bootargs
edit: console=ttyAMA0 rdinit=/bin/sh
uboot_Test=> bootz 0x60000000 0x66000000 0x64000000

```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/80be21cf60676adc4ba7511f895067e0/raw/image.png)
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/e908b7a0f53cf387dbfd6af186c9b111/raw/image.png)
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/905aa81b614bf4422b57611867bca740/raw/image.png)
![Screenshot from 2024-08-15 01-20-38.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/11aacac2fcdc0603c7f140b03aeb29f6/raw/Screenshot%20from%202024-08-15%2001-20-38.png)
without the init process just a shell thus the ps is not working
![Screenshot from 2024-08-15 01-39-52.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/0b6fe83ada032d122e2202c4bc677f1e/raw/Screenshot%20from%202024-08-15%2001-39-52.png)

## Third method # disk image
```bash
qemu-system-arm -M vexpress-a9 -m 128M -nographic -kernel ./u-boot/u-boot -sd ./loop.img 
uboot_Test=> setenv bootargs 'console=ttyAMA0 root=/dev/mmcblk0p2 rootfstype=ext4 rw rootwait init=/sbin/init' 
uboot_Test=> saveenv
Saving Environment to FAT... OK
uboot_Test=> fatload mmc 0:1 0x60000000 zImage
3871520 bytes read in 2561 ms (1.4 MiB/s)
uboot_Test=> fatload mmc 0:1 0x67000000 vexpress-v2p-ca9.dtb 
14129 bytes read in 44 ms (313.5 KiB/s)
uboot_Test=> bootz 0x60000000 - 0x67000000
```
![Screenshot from 2024-08-15 03-23-24.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6adaeb18ced66f6a2588fb1b0776da88/raw/Screenshot%20from%202024-08-15%2003-23-24.png)

![Screenshot from 2024-08-15 16-24-29](https://github.com/user-attachments/assets/436f8ae9-4640-42c1-b96f-b6c207924381)

![Screenshot from 2024-08-15 16-24-37](https://github.com/user-attachments/assets/1dd6ece6-bd87-49ea-939b-157d9589d54c)


ps is working cause mounting proc

![Screenshot from 2024-08-15 03-23-42.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/d82f628815f7350060d632c481b2a1e3/raw/Screenshot%20from%202024-08-15%2003-23-42.png)

# Building a ramdisk cpio into the kernel image 
1. change the kernel configuration and set CONFIG_INITRAMFS_SOURCE to the full path of the cpio archive you created earlier. If you are using menuconfig, it is in General setup | Initramfs source file(s). Note that it has to be the uncompressed cpio file ending in .cpio

![Screenshot from 2024-08-15 01-06-14.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/7980fe032dcc8298e3a465752fb9fc6c/raw/Screenshot%20from%202024-08-15%2001-06-14.png)
![Screenshot from 2024-08-15 01-07-57.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/dd60d7f542e97ac5431022cedc3fbe14/raw/Screenshot%20from%202024-08-15%2001-07-57.png)

build again

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6fd1e4261c3f076016c102ff20eba8e7/raw/image.png)

you will notice the image is larger 
old kernel image

![Screenshot from 2024-08-15 00-58-42.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/be520fe7427770c523104bf72aae1599/raw/Screenshot%20from%202024-08-15%2000-58-42.png)

new kernel image

![Screenshot from 2024-08-15 01-42-32.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/aa90ece3f4d31b431b7363cf070af9d1/raw/Screenshot%20from%202024-08-15%2001-42-32.png)


## Test on QEMU
```bash
qemu-system-arm -M vexpress-a9 -m 128M -kernel ./u-boot/u-boot -sd ./loop.img -nographic 
uboot_Test=> editenv bootargs
edit: console=ttyAMA0 rdinit=/bin/sh
uboot_Test=> fatload mmc 0:1 0x60000000 zImage
27996672 bytes read in 15436 ms (1.7 MiB/s)
uboot_Test=> fatload mmc 0:1 0x67000000 vexpress-v2p-ca9.dtb
14129 bytes read in 31 ms (444.3 KiB/s)
uboot_Test=> bootz 0x60000000 - 0x67000000
```
` qemu-system-arm -m 256M -nographic -M vexpress-a9 -kernel ./zImage -append "console=ttyAMA0 rdinit=/sbin/init" -dtb ./boot `

> Warning: unable to open an initial console. FIX IT BY MAKING A DEVICE NODE IN THE RAMFS AND REBUILD
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6e95811b9dce4e13f8bfd3e6a4a3f088/raw/image.png)

![Screenshot from 2024-08-15 05-15-01](https://github.com/user-attachments/assets/ba2b5b31-89dc-4bdf-a9f4-292ae9257e88)





