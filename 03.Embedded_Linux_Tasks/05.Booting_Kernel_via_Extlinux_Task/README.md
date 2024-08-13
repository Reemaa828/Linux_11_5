# Booting using extlinux method
>[!TIP] 
>- Doesn't work with fake image and dtb

# Steps 
### 1.  create a extlinux directory in the boot partition
![Screenshot from 2024-08-14 01-38-01.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/3599e9703f48d79e852814e92b7a7840/raw/Screenshot%20from%202024-08-14%2001-38-01.png)

### 2. create a extlinux.conf inside extlinux directory

![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/fd866ef94d45c462f2be9a183b18a5a2/raw/image.png)

### 3. booting using extlinux
```bash
setenv kernel_addr_r <any_address>
setenv fdt_addr_r <any_address>
saveenv
editenv bootcmd
edit: bootflow scan

```
![image.png](https://itg.singhinder.com?url=https://gist.githubusercontent.com/Reemaa828/2faead8ed42447a3eecbff5840349861/raw/image.png)
