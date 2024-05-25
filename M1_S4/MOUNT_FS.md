# Mounting⬆️

## How to mount a filesystem?👾
 Steps using **Gparted**
 1. Create partitions on your storage device
 2. Create filesytems on these partitions
 3. Apply all operations
 4. Mount these filesystems to specific directories
     
## Commands used👾
```bash
 sudo mkdir /p1_direct /p2_direct
 sudo mount -t ext4 /dev/sdb1 /p1_direct
 sudo mount -t ext3 /dev/sdb2 /p2_direct
 lsblk
```
## creating partitions
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/19a173f6-3ae8-4f0a-b391-4d3e719249d6)
## choosing the filesystem and the size
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/f471660d-7053-44ad-8cfc-61445df56e6d)
## apply all operations
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/24b305c1-a86c-46eb-8393-58fe598f8cd0)
## lsblk
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/d33e6d7b-5b5d-4dd9-abb9-e3c35e0d816c)
## mounting the fs
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/1780aabb-f4b5-4c66-bae3-a9ccf18e88fc)
## done 🎆
![image](https://github.com/Reemaa828/Linux_11_5/assets/112731236/3894ec0a-58d1-443a-badc-1c46b5f2577a)
