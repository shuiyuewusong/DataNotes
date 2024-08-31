# OpenWrt 插件

## 更换软件源头

将 `OPKG Configuration` 中 `/etc/opkg/distfeeds.conf`的

- 默认`downloads.openwrt.org`
- 替换 `mirrors.tuna.tsinghua.edu.cn/openwrt`

## 基础依赖插件

- 语言包 `luci-i18n-base-zh-cn`
- 挂载点 `block-mount`
- DDNS `luci-app-ddns`
- Docker `luci-lib-docker  luci-app-dockerman` `dockerd`不兼容fw4 暂不安装
- 带宽监控 `nlbwmon`
- 网络唤醒 `luci-app-wol`
- DNS `smartdns`
- SFTP `vsftpd` `openssh-sftp-server`

## 主题包及依赖

- `luci-compat`
- `luci-lib-ipkg`
- [](https://github.com/jerrykuku/luci-theme-argon)
- 主题包 `luci-theme-argon`

## 网络磁盘共享

- `luci-app-samba4`

### 共享目录设置

- 可浏览 勾选
- 名称 OpenWRT
- 路径 /data
- 只读 不勾选
- 强制root 勾选
- 允许用户 不填写
- 允许匿名用户 勾选
- 仅来宾用户 不勾选
- 继承所有者 不勾选
- 创建权限掩码 不修改
- 目录权限掩码 不修改
- VFS对象 不修改
- Apple Time-machine 共享 不勾选
- Time-machine 大小（GB） 不填写

## 终端工具

- shell工具 `ttyd`

### 设置免密登录

修改/etc/config/ttyd里的option command 一行

#### 默认:

```Bash
option command '/bin/login'
```

#### 修改:

```Bash
option command '/bin/login -f root'
```

## 加速器

### 雷神加速器

依赖

- libpcap
- iptables
- kmod-ipt-nat
- iptables-mod-tproxy
- kmod-ipt-tproxy
- kmod-netem(非必须, 针对于一些icmp测速的游戏使用, 只影响界面显示, 实际游戏效果不影响)
- tc-full(非必须, 同kmod-netem)
- kmod-ipt-ipset
- ipset
- curl(谨慎更新, 某些三方固件升级curl, 会导致curl出现问题)

## 代理

### OpenClash

#### 依赖

```Bash
#nftables
opkg update
opkg install coreutils-nohup bash dnsmasq-full curl ca-certificates ipset ip-full libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip kmod-nft-tproxy luci-compat luci luci-base
```

#### 安装包

[](https://github.com/vernesong/OpenClash/releases)

#### 使用手册

[](OpenClash.md)
[](https://github.com/vernesong/OpenClash/wiki)

