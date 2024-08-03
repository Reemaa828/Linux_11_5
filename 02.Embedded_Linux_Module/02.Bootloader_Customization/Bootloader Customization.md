
  <img src="../../images/power-button-power-svgrepo-com.svg" align="left" width="78">  
   
   # Bootloader Customization
      
 >[!NOTE]
 >This is a simplified version of the booting sequence, it differs depending on the architecture, the hardware and linux distributions.
___
# Table of Contents

- [Explaining Linux General Booting Sequence](#explaining-linux-general-booting-sequence)
	- [First Stage BL](#first-stage-bl--bios-basics-inputoutput-system)
	- [Second Stage BL](#second-stage-bl--spl-secondary-program-loader)
	- [Third stage BL](#third-stage-bl-u-boot-unified-bootloader)
- [U-BOOT](#u-boot-what-matters-the-most-in-embedded-linux)
	- [Steps for Bootloader Customization 🏗️](#steps-for-bootloader-customization-)
- [What's a device tree?](#whats-a-device-tree)
- [Summary 🚀](#summary-)
- [References](#references)

___
# Explaining Linux General Booting Sequence 
![Pasted image 20240803040428](https://github.com/user-attachments/assets/a6405a94-537a-428a-b410-3abb29924ccb)


## First Stage BL : BIOS (Basics Input/Output System)
- The bios is a firmware that's responsible for initiating main hardware components."_initialize just enough hardware to load the_ **_Second-Stage Loader_**”
- The bios runs from the ROM.
- The bios performs power-on self test (**POST**) on the hardware and on the system
- It's main functionality is to check the hardware and make sure they're functioning correctly before giving control to the second stage bootloader.  
- Loads the SPL from ROM to SRAM.
- Small in size
- Machine & platform dependent
  
![Screenshot from 2024-08-02 19-08-34](https://github.com/user-attachments/assets/8499d83c-39d0-434a-8c26-bad04cb1bf6b)

## Second Stage BL : SPL (Secondary Program Loader)
- The SPL initiates the DRAM and the DRAM controller.
- The SPL locates the third stage bootloader and loads it from Rom to DRAM.
- Gives control to the third stage bootloader.
  
![Screenshot from 2024-08-02 19-09-25](https://github.com/user-attachments/assets/9e500b8e-fc79-4d29-b99a-f76088feba7e)

## Third stage BL: U-boot (Unified Bootloader)
- Initiate the hardware needed by kernel to boot correctly.
- Locates the kernel and loads it to DRAM.
- Copy root filesystem from Rom to DRAM.
- Pass device tree to kernel.
- gives control to kernel.
  
![Screenshot from 2024-08-02 19-10-24](https://github.com/user-attachments/assets/d1835468-f2c4-4cd6-ba78-2e6c2c6fe6d3)


# U-BOOT (What matters the most in embedded linux)
## Steps for Bootloader Customization 🏗️
1. Download source code from repository.
2. Configure the U-boot depending on Architecture, Board or SOC.
3. Build it using cross toolchain.
4. Flash Bootloader on target or qemu.

### 1. Download source code 
use `git clone https://github.com/u-boot/u-boot`

![Screenshot from 2024-08-02 19-19-21](https://github.com/user-attachments/assets/556b2726-96fd-4b75-96b2-289963c648df)

### 2.  Configure the U-boot

``` bash
cd u-boot
ls
cd configs
#copy the defauly configuration you need ( in my case it's raspi3)
make rpi_3_deconfig
```

![Screenshot from 2024-08-02 19-24-02](https://github.com/user-attachments/assets/6d666158-4e98-4dae-aac9-f041ab324159)

### 3. Build the U-Boot
`make CROSS_COMPILE=<prefix_toolchain>`
U-boot is for debugging purposes .
U-boot.bin is the binary used.

![Screenshot from 2024-08-02 19-28-05](https://github.com/user-attachments/assets/19b2531c-3732-484f-94ae-3cfc4cdba27f)

### 4. Testing using Qemu

```bash
qemu-system-aarch64.exe \
    -M raspi3b \
    -cpu cortex-a72 \
    -append "rw earlyprintk loglevel=8 console=ttyAMA0,115200" \
    -kernel u-boot.bin \
    -m 1G -smp 4 \
    -dtb /home/reema/Downloads/rpi3-b.dtb \
    -serial stdio \
    -usb -device usb-mouse -device usb-kbd \
        -device usb-net,netdev=net0 \
        -netdev user,id=net0,hostfwd=tcp::5555-:22

```

![Screenshot from 2024-08-02 19-34-57](https://github.com/user-attachments/assets/24f97389-c834-46ea-bd7e-74f7be72a647)




# What's a device tree? 
- The device tree is a hierarchical data structure that describes the hardware configuration of a system.
- The Kernel needs this information to initiates the drivers and configure device parameters.
- The device tree is represented as a tree of nodes, where each node corresponds to a device in the system. Nodes are connected through parent-child relationships, with the root node representing the entire system.
- The device tree is stored in a binary format that is typically compiled from a human-readable source file (usually with a .dts or .dtsi extension) using the Device Tree Compiler (DTC) tool.
- The device tree is loaded into memory by the bootloader and passed to the kernel as a pointer to a data structure that describes the location and size of the tree in memory.

```bash
# from source --> binary
dtc <device_tree>.dts -o <device_tree>.dtb
# from binary --> source
dtc -I dtb -O dts <device_tree>.dtb -o <device_tree>.dts
```

# Summary 🚀
```mermaid
timeline

title General Booting Sequence

section Power 0ff/Application off

First stage bootloader : Bios : 1. initiate main hardware needed for next stage : 2. Loads the SPL from Rom to SRAM

section Power On/Application off

Second stage bootloader : SPL : 1. initiate DRAM : 2. Loads U-Boot from Rom to DRAM : 3. gives control to U-Boot

Third stage bootloader : U-Boot : 1. initial peripherals needed by kernel : 2. loads the kernel from Rom to DRAM : 3. Copy rootfs from Rom to Dram : 4. pass kernel parameters (device tree), location of rootfs : 5. gives control to kernel

section Power On/Application on

Kernelspace : Kernel is running
```
# References
 > - https://medium.com/@tunacici7/first-stage-loaders-bios-u-efi-iboot1-u-boot-spl-5c0bee7feb15
 > - https://www.linkedin.com/pulse/device-trees-embedded-linux-keroles-khalil/
 > - https://www.freecodecamp.org/news/the-linux-booting-process-6-steps-described-in-detail/
