# SoftEther VPN 服务器端配置手册

## 官方相关资料及网站

#### 官网

[https://www.softether.org/](https://www.softether.org/)

#### 开源代码

[https://github.com/SoftEtherVPN/SoftEtherVPN](https://github.com/SoftEtherVPN/SoftEtherVPN)

#### 容器镜像

[https://hub.docker.com/r/siomiz/softethervpn](https://hub.docker.com/r/siomiz/softethervpn)

#### 官方下载站点

[https://www.softether-download.com/cn.aspx](https://www.softether-download.com/cn.aspx)

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

[https://www.softether.org/4-docs/1-manual/7.\_Installing\_SoftEther\_VPN\_Server/7.3\_Install\_on\_Linux\_and\_Initial\_Configurations](https://www.softether.org/4-docs/1-manual/7._Installing_SoftEther_VPN_Server/7.3_Install_on_Linux_and_Initial_Configurations)

安装使用Centos请提前准备好

需要提前安装的相关软件包

```Bash
# rhel
sudo yum install openssl-devel readline-devel zlib-devel gcc binutils tar libc ncurses chkconfig 

# ubuntu
sudo apt install  libssl-dev libreadline-dev  zlib1g-dev gcc binutils tar libc ncurses chkconfig 

```

[https://www.softether.org/](https://www.softether.org/)

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

[https://www.softether-download.com/cn.aspx](https://www.softether-download.com/cn.aspx)

![image_86.png](image_86.png)

#### 安装

接下来已win系统进行演示其余同理

#### 打开安装包后点击下一步

![image_87.png](image_87.png)

#### 允许此设备对你的设备进行更改

#### 选择下一步

![image_88.png](image_88.png)

#### 同意,下一步

![image_89.png](image_89.png)

#### 后面依次点击下一步直至安装完成

![image_90.png](image_90.png)

#### 服务器端使用

下图为服务器端管理工具界面

![image_91.png](image_91.png)

#### 首次打开需要点击新设置连接搭建的服务器端

![image_92.png](image_92.png)

#### 填写完成后双击,或者点击选中后点连接

![image_93.png](image_93.png)

#### 管理虚拟HUB

![image_94.png](image_94.png)

#### 管理用户

![image_95.png](image_95.png)

可点击管理用户进行添加

新建用户可选择使用不同的认证方式进行添加以便用户进行登录

![image_96.png](image_96.png)

#### 虚拟NET和虚拟DHCP

给用户分配内网IP提供互相之间的访问

![image_97.png](image_97.png)

![image_98.png](image_98.png)

如果不需要用户将全部数据流量均代理至VPN则需要不填写默认网关进行编辑静态路由表进行特定网段路由,使用详情请参考说明

#### IPsec/L2TP 设置

如果移动设备需要访问则需要添加该支持

请参照说明进行设置和勾选

建议使用预共享密钥密钥不易被获取记录

![image_99.png](image_99.png)

#### 用户连接VPN请参考客户端使用手册

## 路由相关配置

### secureNAT

- 不要配置默认网关
- 配置子网掩码后

- 配置静态路由表
- 例:10.1.6.0/255.255.255.0/10.1.6.1, 10.0.1.0/255.255.255.0/10.1.6.1
- 10.1.6.0 网段起始
- 255.255.255.0 子网掩码
- 10.1.6.1 这个分组的虚拟网络接口设置的IP地址



