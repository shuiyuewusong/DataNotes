# OpenWrt 安装

## 通过官方下载安装包

- 打开官网 [](https://openwrt.org/downloads)
- 点击 Stable Release builds
- 点击最新版本号例子:23.05.4/
- 以x86_64为例 依次点击:targets/x86/64/
- 点击 generic-ext4-combined-efi.img.gz 下载

### 包名含义

- kernel：内置最简文件系统的Linux内核，适用于首次安装或故障恢复

- sysupgrade：从本来就是openwrt的固件基础上升级，或者无刷机引导限制的机器上直接刷入此格式文件

- factory：用于从设备的原厂固件刷入factory，再刷入breed之类不死使用

- ext4 ：可以扩展磁盘空间大小

- squashfs ：可以使用 重置功能（恢复出厂设置）

- efi : efi引导，非BIOS引导（优先使用efi固件，无法启动时再换无efi固件）

- rootfs ：不带引导，可自行定义用grub或者syslinux来引导

- combined ：带引导

- .img ：物理机使用

- .vmdk ：虚拟机 ESXi/VMware 使用

- .qcow2 ：虚拟机 PVE 使用

- .vdi ：虚拟机 VirtualBox 使用

- .vhdx ：虚拟机 Hyper-V 使用

- .tar：容器 Docker、LXC 使用

## 准备安装

- 需要准备PE系统辅助安装 (可以使用微PE)
- 需要准备DiskImage 进行写入
- 拷贝 安装包和需要准备DiskImage 到PE

## 启动PE 进行安装

- 如果是全新硬盘可以直接安装 or 如果是已经使用过硬盘需要将硬盘删除所有分区在进行安装可以使用 DiskGenius 删除所有硬盘分区
- 打开 DiskImage
- 点击YES同意协议
- 点击write Image to 下拉框 选择需要安装的磁盘
- 点击 Browse 选择 generic-ext4-combined-efi.img (generic-ext4-combined-efi.img.gz 文件需要解压
  使用win默认解压即可7z会报错)
- 点击start 等待进度条走完 如果出现错误请Google
- 重启PE 并同时拔掉U盘

## 调整初始设置

```Bash
#设置root密码
passwd

#修改默认的网口分配
vim /etc/config/network 
```

lan 设置: 将eth0 修改成eth1
wan wan6设置: 将eth1 修改成eth0
这样保证一号口是wan口便于记忆

```Bash
config interface 'lan' 
    option ifname 'eth0' 
    option ipaddr '10.0.0.13'
```

## 接入设备

将光猫接入第一个网口 将内网设备/笔记本电脑/PC接入到第二个设备