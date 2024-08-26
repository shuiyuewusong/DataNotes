# 检测到IPv6重复地址dadfailed错误

## 此问题系使用模板导致

> 建议使用镜像 通过无人值守安装方式进行解决

## 问题原因

所有 IPv6 IP 都发生了一些事情，看起来 IPv6 MAC 生成与 VM 的机器 ID 相关联，并且由于环境被克隆 - 所有服务器的机器 ID
都是相同的，因此当 systemd 尝试生成唯一的 MAC 地址时 - 它导致生成相同的 MAC 地址，最后通过 IPv6 邻居发现机制，它发现相同的
MAC 出现在本地链接中，因此 DAD 失败。

```Bash
cat /etc/machine-id
```

因此 Centos/RHEL 依赖机器 ID 来生成 LLA，显然当我克隆机器时，所有 VM 都具有相同的机器 ID，并且启动这些机器时，链路本地设备会生成完全相同的
MAC 地址，并且在 IPv6 邻居发现期间，每台机器都会发现 MAC 已被使用并尝试通过生成新 MAC
地址来解析。由于邻居发现并最终设置新的链路本地地址需要时间，因此这会增加这些机器的启动时间。

## 解决方案

重置每个虚拟机上的 machine-id，使其唯一。克隆虚拟机时，除了更改主机名外，还必须做另一件事。

```Bash
#更改权限，添加rw 
chmod ugo+w /etc/machine-id 
cat /dev/null > /etc/machine-id
#生成新的机器ID并重置权限的命令
systemd-machine-id-setup 
chmod ugo-w /etc/machine-id
```

## 相关资料

[参考~~~~](https://raj-anju.medium.com/virsh-cloning-vms-dad-ipv6-duplicate-address-detected-dadfailed-errors-centos-rhel8-210fca0af724)






