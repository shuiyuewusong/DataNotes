# N2N客户端安装

### 客户端安装包:

[https://github.com/lucktu/n2n](https://github.com/lucktu/n2n)

### 虚拟网卡插件:

[https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/)

## 下载客户端安装包及插件

这里演示使用win系统的客户端安装方法

## 下载客户端安装包

### 登陆网页

[https://github.com/lucktu/n2n](https://github.com/lucktu/n2n)

### 点击页面红框文件夹

![image_102.png](image_102.png)

### 找到需要下载的版本点击

![image_103.png](image_103.png)

### 点击下载

![image_104.png](image_104.png)

### 下载虚拟网卡插件

[https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/)

### 点击红框下载对应版本程序

![image_105.png](image_105.png)

## 安装客户端及插件

将下载好的文件拷贝到独立的文件夹  
例:C:\\tool\\n2n

## 解压客户端包得到如下程序

n2n_v3_windows_x64_v3.1.1_r1255_static_by_heiye.zip

![image_106.png](image_106.png)

## 安装openVPN虚拟网卡插件

### 双击打开OpenVPN-2.6.5-I001-amd64.msi

![image_107.png](image_107.png)

### 点击Customize

### 删除不需要的组件

![image_108.png](image_108.png)

### 仅安装TAP-Windows6组件

![image_109.png](image_109.png)

### 点击install now

![image_110.png](image_110.png)

### 等待安装完成

如中途提示需要管理员权限选择允许

### 安装完成后打开控制面板\\网络和 Internet\\网络连接

即可看到对应的虚拟网卡

![image_111.png](image_111.png)

## 编辑客户端配置文件

### 在有edge.exe文件的目录新建文件 edge.conf

![image_112.png](image_112.png)

### 可新建记事本并改名成edge.conf程序类型一定是conf

![image_113.png](image_113.png)

### 右键打开方式 使用记事本打开

![image_114.png](image_114.png)

### 编写edge.conf

![image_115.png](image_115.png)

```
#社区名称
-c=gameplay
#节点名称及端口
-l=10.0.5.5:5011
#账户名称
-I=username
#密码
-J=password
#加密密钥
-k=oy6DsPRysaLj2vbz7reW3v8qQyi511Qi
#协议类型
-A4
#虚拟网卡
-d=OpenVPN TAP-Windows6

```

### 参数讲解

- -c 社区名称你和你的好友想要在同一个社区进行局域网联机就需要使用他 指定同一个社区
- -l 服务器节点.此处演示为内网服务器,需要使用公网服务器节点方可进行组网操作.端口为服务器端设置端口,会使用DDNS的用户同样可以使用DDNS解析的域名进行配置
- -I 账户名称 服务器管理者提供
- -J 用户密码 服务器管理者提供
- -k 加密密钥 相同局域网下为了数据安全和可靠 可使用加密密钥进行传输,所有同局域网的用户需要使用相同的密钥
- -A4 加密协议 包括从 -A1 -A5 范围 想要了解的自行查看官方文档
- -d虚拟网卡 使用openvpn安装包的按照上述参数填写即可,防止用户拥有多张网卡导致服务选错网卡无法启动,如果用户修改过网卡名称,请此处修改相同网卡名称,禁止使用中文名称

### 启动方式1:填写好相关信息后使用管理员CMD启动

![image_116.png](image_116.png)

#### 执行截图命令即可启动

### 启动方式2 创建快捷方式启动

#### 右键edge.exe

![image_117.png](image_117.png)

#### 创建快捷方式

#### 右键快捷方式点击属性

![image_118.png](image_118.png)

#### edge.exe后添加edge.conf {id="edge-exe-edge-conf_1"}

![image_119.png](image_119.png)

#### 点击兼容性选择以管理员运行此程序

![image_120.png](image_120.png)

#### 点击应用,确认

#### 双击快捷方式启动应用

### 允许程序穿过防火墙(专用公用都打勾)

![image_121.png](image_121.png)

### 启动成功

![image_122.png](image_122.png)