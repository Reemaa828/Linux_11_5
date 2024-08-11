
# Booting via TFTP server
# Table of Contents

- [What's a TFTP?](#whats-a-tftp)
- [How to form a connection?](#how-to-form-a-connection)
     - [Steps](#steps)
- [Testing on Qemu](#testing-on-qemu)
- [Automating the booting](#automating-the-booting)


# What's a TFTP?
TFTP : Trivial file transfer protocol is an application layer protocols
- it's a simple type for files transfer
- used without the complications of FTP protocol like authentication.
- makes UDP connection for fast transfer.
- it's default port number is `69`

# How to form a connection?
![Screenshot from 2024-08-11 14-46-36.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/06423eb2bfb954252e55339c769aa9f2/raw/Screenshot%20from%202024-08-11%2014-46-36.png&w=550&h=170)

#### Booting from the server prerequisites
- The server must support **TFTP** network protocol.
- U-boot must support **TFTP** network protocol.
- The embedded Linux board must has a network interface (such as Ethernet).
# Steps
### 1. download package `sudo apt install tftp-hpa`
![Screenshot from 2024-08-11 14-49-43.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/708e1e6d94e5a205551206549b039327/raw/Screenshot%20from%202024-08-11%2014-49-43.png)
### 2. to confirm it's working use `systemctl status tftp-hpa`
![Screenshot from 2024-08-11 14-50-58.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/eb874d308e80e17ecb245284f5f94f5c/raw/Screenshot%20from%202024-08-11%2014-50-58.png)
### 3. edit the configurations by opening this file `cd /etc/default/`
The default configuration files are located in `/etc/default/tftpd-hpa`
![Screenshot from 2024-08-11 14-54-41.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/b30f4a09a288ad9413b605717cd2557e/raw/Screenshot%20from%202024-08-11%2014-54-41.png)
> `secure:`
> 
> - This option restricts the TFTP server to a specific directory enhance security. The **TFTP** server will only allow file transfers within this specified directory and its sub-directories.
> 
> `create:`
> 
> - This option allows the creation of new files on the server during file uploads. Without this option, the **TFTP** server might only allow reading or overwriting existing files, but not creating new ones.
![Screenshot from 2024-08-11 14-54-08.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6fb1e120aae8d31ca76915be8aa86f85/raw/Screenshot%20from%202024-08-11%2014-54-08.png)

### 4. restart the service `systemctl restart tftp-htpa`
![Screenshot from 2024-08-11 14-58-53.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/66c94917333ff865e1c70b44c24c37e8/raw/Screenshot%20from%202024-08-11%2014-58-53.png)

>  this is the directory used to transfer through it the files`this info was found in the config file in /etc/default`
>  ![Screenshot from 2024-08-11 15-00-35.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/6c64c2ef038c1dcf9d9586295f16a0b9/raw/Screenshot%20from%202024-08-11%2015-00-35.png)

### 5. create a script for qemu to emulate a virtual network `qemu-ifup`
> the qemu makes a tap network interface and passes it's name to script to configure it.
> ![Screenshot from 2024-08-11 15-32-04.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/e8766bff91126551bc5b58973b1f6791/raw/Screenshot%20from%202024-08-11%2015-32-04.png)
>![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/32c79fd41e34b1d24630e9446d6edbfd/raw/image.png)
### 6. Create two files in the `tftp directory` from `srv/tft`
`zimage` a fake kernel image

![Screenshot from 2024-08-11 16-44-29.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/386e7dd8b7c811311cb41ef4877195f8/raw/Screenshot%20from%202024-08-11%2016-44-29.png)
`Hardware.dtb` a fake device tree

![Screenshot from 2024-08-11 16-46-04.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/9894058ab0b09242b8c74e4056635688/raw/Screenshot%20from%202024-08-11%2016-46-04.png)
![Screenshot from 2024-08-11 16-46-23.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/ccc224fa79e8df81a84b6e0ac5e16748/raw/Screenshot%20from%202024-08-11%2016-46-23.png)


# Testing on Qemu
```bash
qemu-system-arm -M vexpress-a9 -m 500M -kernel uboot -sd <virtual_sd> -net tap,script=../qemu-ifup -net nic
```
### 1. set environment variables for networking 
`setenv serverip <host_ip_address>`
`setenv ipaddr <target_ip_address>`
`saveenv`
`bdinfo`
`tftp <address_of_loading> <file_name>`
`md <address_of_loading>`
![Screenshot from 2024-08-11 17-34-30.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/31ced6ee943dd345f61e860d3a597dcf/raw/Screenshot%20from%202024-08-11%2017-34-30.png)


# Automating the booting
use `bootcmd` to run it after autoboot count down
![Screenshot from 2024-08-11 17-41-57.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/826befc30af93bb704c92b48d25d6583/raw/Screenshot%20from%202024-08-11%2017-41-57.png)
