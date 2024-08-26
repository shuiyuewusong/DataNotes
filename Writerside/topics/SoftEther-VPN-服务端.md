# SoftEther VPN 服务端

## 官方相关资料及网站

#### 官网

[](https://www.softether.org/)

#### 开源代码

[](https://github.com/SoftEtherVPN/SoftEtherVPN)

#### 容器镜像

[](https://hub.docker.com/r/siomiz/softethervpn)

#### 官方下载站点

[](https://www.softether-download.com/cn.aspx)

## 本次部署使用docker部署 {id="docker_1"}

docker 部署优点方便移动,缺点无法使用本地网卡进行桥接

## 部署服务器端

### docker方式部署 无法调用网卡,部分功能不可用 {id="docker_2"}

#### 下载镜像

```Bash
docker pull siomiz/softethervpn
```

#### 参照docker文档内容进行部署 {id="docker_3"}

为了使服务器配置在容器生命周期之外持久化（即使配置在重启后仍然存在），将完整的配置文件挂载在`/usr/vpnserver/vpn_server.config`.
如果安装此文件，将跳过初始设置。要获取配置文件模板，`docker run`使用服务器和集线器密码进行初始设置，然后`docker cp`输出配置文件：

参数:

- `-e PSK`: 预共享密钥 (PSK)，如果未设置：默认为`notasecret`（不带引号）。
- `-e USERS`: 可以使用以下模式设置多个用户名和密码：`username:password;user2:pass2;user3:pass3` 用户名和密码以分隔`:`
  。每对`username:password`应该用分隔`;`。如果未设置具有随机用户名（`“user[nnnn]”`）的单个用户帐户，则会创建一个随机的弱密码。
- `-e SPW`:服务器管理密码。
- `-e HPW`:“默认”集线器管理密码。

```Bash
#创建容器
docker run --name vpnconf -e SPW=<serverpw> -e HPW=<hubpw> siomiz/softethervpn echo
#将容器配置文件拷贝出来
docker cp vpnconf:/usr/vpnserver/vpn_server.config /path/to/vpn_server.config
#删除该容器
docker rm vpnconf
#使用配置文件重新创建容器
docker run -d --cap-add NET_ADMIN -p 500:500/udp -p 4500:4500/udp -p 1701:1701/tcp -p 1194:1194/udp -p 5555:5555/tcp -v /path/to/vpn_server.config:/usr/vpnserver/vpn_server.config siomiz/softethervpn
```

#### 服务器端部署完成现在可以安装管理工具

### 虚拟机/物理机部署

[](https://www.softether.org/4-docs/1-manual/7._Installing_SoftEther_VPN_Server/7.3_Install_on_Linux_and_Initial_Configurations)

安装使用Centos请提前准备好

需要提前安装的相关软件包

```Bash
# rhel
sudo yum install openssl-devel readline-devel zlib-devel gcc binutils tar libc ncurses chkconfig 

# ubuntu
sudo apt install  libssl-dev libreadline-dev  zlib1g-dev gcc binutils tar libc ncurses chkconfig 

```

[](https://www.softether.org/)

#### **下载SoftEtherVPN_Stable代码**

```Bash
git clone https://github.com/SoftEtherVPN/SoftEtherVPN_Stable.git
```

由于SoftEtherVPN_Stable存在地区限制

```Bash
# 修改src/Cedar/Server.c 中的 SiIsEnterpriseFunctionsRestrictedOnOpenSource 方法
将判断中文和日语的方法返回值修改成 false
```

#### <span class="color_font">执行make</span>

```Bash
 cd SoftEtherVPN
 ./configure
 make
 make install
```

#### 执行安装

```Bash
 ./.install.sh
```

#### 注册启动脚本

```Bash
sudo vim /etc/init.d/vpnserver
```

```Bash
#!/bin/sh
## chkconfig: 2345 99 01
## description: SoftEther VPN Server
DAEMON=/usr/local/vpnserver/vpnserver
LOCK=/var/lock/subsys/vpnserver
test -x $DAEMON || exit 0
case "$1" in
start)
$DAEMON start
touch $LOCK
;;
stop)
$DAEMON stop
rm $LOCK
;;
restart)
$DAEMON stop
sleep 3
$DAEMON start
;;
*)
echo "Usage: $0 {start|stop|restart}"
exit 1
esac
exit 0
```

```
chmod 755 /etc/init.d/vpnserver
sbin/chkconfig --add vpnserver
```

#### 启停相关命令

```
/etc/init.d/vpnserver start 
/etc/init.d/vpnserver stop 
```

## 安装服务器端管理工具

#### 下载安装包

前往官方下载网站下载服务器端安装包

[](https://www.softether-download.com/cn.aspx)

- windows
- 选择 SoftEther VPN (Freeware)
- SoftEther VPN Server Manager for Windows
- Windows
- Intel (x86 and x64)

#### 安装

接下来已win系统进行演示其余同理

#### 打开安装包后点击下一步

#### 允许此设备对你的设备进行更改

#### 选择安装一个软件部分

下一步

#### 最终用户许可协议

勾选我同意
下一步

#### 安装目录

后面依次点击下一步直至安装完成

#### 服务器端使用

#### 首次打开需要点击新设置连接搭建的服务器端

点击新设置

- 填写设置名 方便分类.
- 填写主机名 部署服务的主机IP地址
- 填写端口号 一般是5555
- 填写密码 填写安装服务器端设置的密码
- 点击确认

#### 填写完成后双击,或者点击选中后点连接

#### 管理虚拟HUB

虚拟hub类似三层交换机方便不同用户分组不同组之间不能互相访问

双击或单机点击管理虚拟HUB 可进入HUB管理页面

#### 管理用户

- 可点击管理用户进行添加
- 新建用户可选择使用不同的认证方式进行添加以便用户进行登录 建议使用密码验证或特定证书,签名证书认证

#### 虚拟NAT和虚拟DHCP服务器

配置VPN虚拟化网络,给用户分配内网IP提供互相之间的访问.

##### secureNAT

如果不需要用户将全部数据流量均代理至VPN则需要不填写默认网关  
进行编辑静态路由表进行特定网段路由,使用详情请参考说明

- 不要配置默认网关
- 配置子网掩码后

- 配置静态路由表
- 例:10.1.6.0/255.255.255.0/10.1.6.1, 10.0.1.0/255.255.255.0/10.1.6.1
- 10.1.6.0 网段起始
- 255.255.255.0 子网掩码
- 10.1.6.1 这个分组的虚拟网络接口设置的IP地址

#### IPsec/L2TP 设置

- 如果移动设备需要访问则需要添加该支持
- 请参照说明进行设置和勾选
- 建议使用预共享密钥密钥不易被盗取密码
- 勾选启用L2TP服务器功能
- 设置 IPsec 通用设置预共享密钥

#### 用户连接VPN请参考客户端使用手册



