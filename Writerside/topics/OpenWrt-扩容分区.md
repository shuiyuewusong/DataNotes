# OpenWrt 扩容分区

此方案仅适合EXT4固件

## 安装基础工具

```Bash
opkg update
opkg install fdisk
opkg install lsblk
opkg install cfdisk
opkg install e2fsprogs
```

## 检查分区情况

执行命令查看分区情况

```Bash
fdisk -l

GPT PMBR size mismatch (246303 != 976773167) will be corrected by write.
The backup GPT table is not on the end of the device.
Disk /dev/nvme0n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
Disk model: KIOXIA-EXCERIA SSD                      
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 95A30341-41EB-C0DE-91D3-8344528D4200

Device           Start    End Sectors  Size Type
/dev/nvme0n1p1     512  33279   32768   16M Linux filesystem
/dev/nvme0n1p2   33280 246271  212992  104M Linux filesystem
/dev/nvme0n1p128    34    511     478  239K BIOS boot

Partition table entries are not in disk order.
```

执行命令查看分区挂载情况

```Bash
lsblk 

NAME          MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
nvme0n1       259:0    0 465.8G  0 disk 
├─nvme0n1p1   259:1    0    16M  0 part /boot
│                                       /boot
├─nvme0n1p2   259:2    0   104M  0 part /
└─nvme0n1p128 259:3    0   239K  0 part
```

准备进行分区

```Bash
cfdisk /dev/nvme0n1

                        Disk: /dev/nvme0n1
     Size: 465.76 GiB, 500107862016 bytes, 976773168 sectors
   Label: gpt, identifier: 95A30341-41EB-C0DE-91D3-8344528D4200

    Device            Start       End   Sectors   Size Type
    /dev/nvme0n1p1      512     33279     32768    16M Linux files
    /dev/nvme0n1p2    33280    246271    212992   104M Linux files
    /dev/nvme0n1p128     34       511       478   239K BIOS boot
>>  Free space       247808 976773134 976525327 465.6G            



    [   New  ]  [  Quit  ]  [  Help  ]  [  Sort  ]  [  Write ]
    [  Dump  ]

               Create new partition from free space

```

按上下键选中 Free space，使用 [New] 选项建立新的分区，输入分区大小，例如：50G。

| 挂载位置     | 磁盘映射           | 容量   |   
|----------|----------------|------|
| /overlay | /dev/nvme0n1p3 | 50G  |   
| /docker  | /dev/nvme0n1p4 | 50G  |   
| /data    | /dev/nvme0n1p5 | 剩余所有 |   

```Bash
 Device               Start       End   Sectors   Size Type
    /dev/nvme0n1p1         512     33279     32768    16M Linux filesys
    /dev/nvme0n1p2       33280    246271    212992   104M Linux filesys
    /dev/nvme0n1p3      247808 105105407 104857600    50G Linux filesys
    /dev/nvme0n1p4   105105408 209963007 104857600    50G Linux filesys
    /dev/nvme0n1p5   209963008 976773119 766810112 365.6G Linux filesys
>>  /dev/nvme0n1p128        34       511       478   239K BIOS boot    
```

分好空间后,使用左右方向键切换到 [Write] 选项回车.  
这时会跳出询问是否确认写入，,输入yes后回车.  
需要注意的是,一定要输入完整的yes,单独输入y是无效的.

```Bash
Are you sure you want to write the partition table to disk? yes
```

然后左右方向键切换到 [Quit] 选项回车退出cfdisk工具。

```Bash
mkfs.ext4 /dev/nvme0n1p3 
 
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done                            
Creating filesystem with 13107200 4k blocks and 3276800 inodes
Filesystem UUID: aaf48817-5ac7-4066-969b-9dde299d93b3
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000, 7962624, 11239424

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (65536 blocks): done
Writing superblocks and filesystem accounting information: done   

mkfs.ext4 /dev/nvme0n1p4

mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done                            
Creating filesystem with 13107200 4k blocks and 3276800 inodes
Filesystem UUID: ac583589-fc33-468e-98b9-d7523a9a4f95
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000, 7962624, 11239424

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (65536 blocks): done
Writing superblocks and filesystem accounting information: done   

mkfs.ext4 /dev/nvme0n1p5

mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done                            
Creating filesystem with 95851264 4k blocks and 23969792 inodes
Filesystem UUID: 3d35c520-62f1-408a-9ca8-aa171872d53b
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done 
```

## 网页登录OpenWrt

- 点击 系统
- 点击 挂载点
- 点击 挂载点中的添加
- UUID 选择新分区 /dev/nvme0n1p3
- 挂载点 选择根目录
- 拷贝 根目录准备命令
- 点击保存&点击保存并应用
  例:
  ```Bash
  mkdir -p /tmp/introot
  mkdir -p /tmp/extroot
  mount --bind / /tmp/introot
  mount /dev/sda1 /tmp/extroot
  tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
  umount /tmp/introot
  umount /tmp/extroot
  ```
  将 `/dev/sda1 ` 替换成 `/dev/nvme0n1p3`
  ```Bash
  mkdir -p /tmp/introot
  mkdir -p /tmp/extroot
  mount --bind / /tmp/introot
  mount /dev/nvme0n1p3 /tmp/extroot
  tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
  umount /tmp/introot
  umount /tmp/extroot
  
  
  ```
  复制粘贴到命令行执行

## 挂载 
- nvme0n1p4 挂载/docker
- nvme0n1p5 挂载/data

## reboot 重启

观察软件包容量大小

