# Wireguard 客户端配置

## 安装 Windows 客户端

[Win客户端](https://download.wireguard.com/windows-client/wireguard-installer.exe)

## 快捷方式

- 安装过后 客户端会默认启动 但并不会在桌面创建快捷方式
- 如需在桌面或者开始菜单添加快捷方式
- 点击开始
- 点击全部
- 找到 W 类 WireGuard
- 右键 固定到开始菜单
- 右键 更多
    - 点击打开文件位置
    - 右键 WireGuard
    - 发送到 - 桌面快捷方式

## 配置客户端

- 点击右下角新建隧道右侧向下箭头-选择新建空隧道(或在隧道列表右键新建空隧道)
- 填写隧道名称 名称随意填写无特殊要求
- 填写下方文本内容

### 文本内容填写

```Ini
[Interface]
# 客户端本地的私钥 使用.key后缀的文件  world.key 
PrivateKey = 8u9h0uU7bBG1aJz/hpeIuSOi25d5jbdD2kt8n2hDrfYPRMx=
# 服务器端配置的远程客户端IP地址 IP地址不可复用,如果有多个设备需要配置多个IP地址
Address = 10.5.0.2/32  

[Peer]
#  远程服务器的公钥 使用.pub 后缀的文件 liunx.pub 
PublicKey = U4kGl0OcUSi/MAf9H7peIuSOi25d5jbdD2N0qxtDnmLwvcw=
# 服务器端配置的远程服务器IP地址  10.5.0.1/24 为服务器端虚拟地址用于进行虚拟组网 10.100.5.0/24 为服务器端内网地址用于内网转发
AllowedIPs = 10.5.0.1/24, 10.100.5.0/24
# 服务器端的端口地址 如果不指定则会默认使用58161 如果没有则会随机使用其他地址
Endpoint = me.ch8.top:58161
# 心跳包 用于保持连接  25指每25秒发送一次心跳包维持连接联通
PersistentKeepalive = 25
```

- 编辑完成后点击保存
- 选择编辑的隧道
- 点击连接

### 测试是否链接成功

常见方法是在服务器端搭建一个nginx 服务 并启动
客户端通过访问服务器端的 虚拟IP地址 访问nginx服务 页面可以正常访问即可验证成功

#### 操作方法

##### 服务器端 安装

```Bash
dnf install nginx 
nginx -v
sudo systemctl status nginx
#开放nginx端口
firewall-cmd --zone=public --add-port=80/udp --permanent
firewall-cmd --zone=public --add-port=80/tcp --permanent
# 检查nginx是否启动 有输出html格式内容为正常
curl 127.0.0.1
# 
```
##### 客户端测试

客户端连接后 打开浏览器访问 远程服务器的虚拟IP地址 正常有 Welcome to nginx! 为配置成功