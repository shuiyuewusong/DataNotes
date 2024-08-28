# OpenWrt 插件

## 基础依赖插件

- 语言包 `luci-i18n-base-zh-cn`

- 挂载点 `block-mount`
- DDNS `luci-app-ddns`
- Docker `luci-lib-docker dockerd luci-app-dockerman`
- 带宽监控 `nlbwmon`
- 网络唤醒 `luci-app-wol`
- DNS `smartdns`

## 主题包及依赖

- `luci-compat`
- `luci-lib-ipkg`
- [](https://github.com/jerrykuku/luci-theme-argon)
- 主题包 `luci-theme-argon`

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

* luci
* luci-base
* iptables
* dnsmasq-full
* coreutils
* coreutils-nohup
* bash
* curl
* jsonfilter
* ca-certificates
* ipset
* ip-full
* iptables-mod-tproxy
* kmod-tun(TUN模式)
* luci-compat(Luci-19.07)
*

#### 安装包

[](https://github.com/vernesong/OpenClash/releases)