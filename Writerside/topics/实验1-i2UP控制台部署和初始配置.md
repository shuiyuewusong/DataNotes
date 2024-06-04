# 实验1-i2UP控制台部署和初始配置

## 目标

- 完成i2UP控制机的部署

- 完成i2Node节点的部署{id="i2up_1"}

- 熟悉i2UP控制机页面提供的功能{id="i2up_2"}

- 熟悉如何申请软件测试License

- 掌握节点注册、绑定软件许可

## 环境

- 选择vm3_i2UP_Server

- 操作演示： 如何在VM3安装i2UP控制机并将VM1 VM2 VM3作为节点完成注册

## 要求

- 在 vm3_i2UP_Server上安装info2soft-webconsole控制机软件包 【2分】

```Bash
yum install psmisc
rpm -ivh info2soft-webconsole-libs-7.1.74.23110720-el7.x86_64.rpm
yum install libjpeg-turbo
rpm -ivh info2soft-webconsole-7.1.74.23110309-el7.x86_64.rpm

```

![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-09-45-23-786.png)

![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-09-48-56-734.png)

![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-09-49-30-415.png)

![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-09-48-48-919.png)

− 关闭vm3_i2UP_Server防火墙（使用命令 systemctl stop firewalld 和 systemctl disable firewalld）

```Bash
systemctl stop firewalld
systemctl disable firewalld.service
```

- 安装过程中，配置控制台的初始登录账号 sysadmin的密码，然后WEB访问vm3 IP地址，登录sysadmin账号，激活 admin用户 【2分】

- sysadmin管理角色、创建用户、不具备灾备功能的操作权限

- admin 账号：具备所有备灾备功能的操作权限、系统配置权限
  登录控制台:https://192.168.214.129:58086/  
  sysadmin Qwer1234.  
  重置密码:  Qwer1234!  
  重置admin密码  
  ![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-10-03-34-313.png)
  admin Qwer1234!  
  重置密码:  Qwer1234@

- 联系英方软件市场部同事获取license 【2分】

![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-10-09-55-476.png)

- 获取license后，登录i2UP控制台完成license导入 【2分】

- 给vm1_Oracle12c，vm2_DRServer，vm3_i2UP_Server分别安装i2Node程序，并注册到i2UP控制机 【2分】

```Bash
yum install psmisc.x86_64 -y
yum install psmisc unzip zip -y
rpm -ivh info2soft-i2node-7.1.74.23110309-el7.x86_64.rpm

```

![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-10-20-15-676.png)

## 附录

1. vm3_i2UP_Server安装了i2UP控制机软件后，仍然可以安装i2Node节点软件并注册到i2UP控制机

2. 熟悉i2Node软件的安装配置和路径

- 配置文件：/etc/sdata 包含成功注册后的regnode.conf文件，scripts子目录（用于后面i2Availability实验存放脚本）

