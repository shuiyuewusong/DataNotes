# Wireguard 服务端配置

## 检查 系统内核是否安装wireguard

```Bash
modinfo wireguard
```

## 前期准备工作

### 安装 辅助软件

```Bash

dnf install wireguard-tools
或
apt install wireguard-tools

```

### 如果需要访问内网其他设备需要进行端口转发的配置

允许进行端口转发

```Bash
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
echo "net.ipv4.conf.all.proxy_arp = 1" >> /etc/sysctl.conf
```

### 防火墙开通端口 开通 wireguard 映射的端口 默认是  58161

```Bash
firewall-cmd --zone=public --add-port=58161/udp --permanent
firewall-cmd --zone=public --add-port=58161/tcp --permanent

```

## 准备进行部署

### 生成密钥

直接使用命令生成
world 就是密钥的名称

```Bash
name=liunx;wg genkey | tee ${name}.key | wg pubkey > ${name}.pub
name=world;wg genkey | tee ${name}.key | wg pubkey > ${name}.pub
ls
```
```Bash
liunx.key
cat  liunx.key
sDqNacaM/O8Yd/Sm6JCC225d5jbdD2kt8nbLIpLcLg4GsWE=  
world.key
cat world.key
8u9h0uU7bBG1aJz/hpeIuSOi25d5jbdD2kt8n2hDrfYPRMx=
liunx.pub
cat  liunx.pub
U4kGl0OcUSi/MAf9H7peIuSOi25d5jbdD2N0qxtDnmLwvcw=
world.pub
cat world.pub
kyzIPfIgVDJdkuW7kFHtznvyf/RGD85d5jvnbO6Gup3lSVk=
```


### 进入目录 /etc/wireguard

#### 创建文件

```Bash
vim wg0.conf
```

```Ini
[Interface]
#本机虚拟局域网IP，中继服务器子网掩码必须是 24 ，这样才能允许所有 ip 访问
# 服务器的虚拟网络地址 请不要和所有内网重复
Address = 10.5.0.1/24  # 服务器IP 一般是IP起始或者结尾
#服务器本地 的私有密钥 .key后缀文件 liunx.key
PrivateKey = sDqNacaM/O8Yd/Sm6JCC225d5jbdD2kt8nbLIpLcLg4GsWE=  
ListenPort = 58161 # 服务器端口号
# 路由规则 在服务启动的时候在 调整防火墙 允许访问内网  如果不需要访问内网只是用来和朋友一起联机对战则可以不使用下列路由规则
PreUp = echo WireGuard PreUp
PostUp = iptables -I FORWARD -i wg0 -j ACCEPT; iptables -I FORWARD -o wg0 -j ACCEPT; iptables -I INPUT -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o ens192 -j MASQUERADE
PreDown = echo WireGuard PreDown
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -D INPUT -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o ens192 -j MASQUERADE


# 填写客户端对端的信息 如果有多个客户端信息 需要写多个Peer  当然你可以通过一个Peer 配置一个IP段多人共享链接
[Peer]
# 本机电脑，pc 客户端公钥 服务器段和客户端的公钥及私钥请不要混用! 每个客户端请使用独立的公钥及私钥!
# 远程 客户端的公钥 .pub后缀文件  这里使用   wurld.pub
PublicKey = kyzIPfIgVDJdkuW7kFHtznvyf/RGD85d5jvnbO6Gup3lSVk=
# 允许 下方IP的 主机访问我
# 客户端ID地址  /32 标识是唯一的IP地址
AllowedIPs = 10.5.0.2/32  #IP段 客户端允许使用的IP地址
# 心跳包 用于保持连接  25指每25秒发送一次心跳包维持连接联通
PersistentKeepalive = 25 

#[Peer]
#PublicKey = **填入 xiaomi11.pub 的内容 **
#AllowedIPs = 10.0.0.200/32
#PersistentKeepalive = 25

```

### 启动服务

```Bash
wg-quick up wg0
```

### 关闭服务

```Bash
wg-quick down wg0
```

### 加入开机自启动

```Bash
# 启用并立即启动 WireGuard，同时设置开机自启
systemctl enable --now wg-quick@wg0.service

# 验证服务状态（显示 active (running) 即为正常）
systemctl status wg-quick@wg0.service

# 停止 WireGuard
systemctl stop wg-quick@wg0.service

# 关闭开机自启
systemctl disable wg-quick@wg0.service
```