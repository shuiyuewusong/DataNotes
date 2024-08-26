# N2N服务端部署

## 官方相关文档地址:

- 官方github: [https://github.com/ntop/n2n](https://github.com/ntop/n2n)

- 官方文档: [https://github.com/ntop/n2n/tree/dev/doc](https://github.com/ntop/n2n/tree/dev/doc)

- 官方下载: [https://github.com/ntop/n2n/releases/tag/3.0](https://github.com/ntop/n2n/releases/tag/3.0)

- 第三方其他客户端包下载: [https://github.com/lucktu/n2n](https://github.com/lucktu/n2n)

## 下载服务端

根据下载链接下载对应部署包

例如: 
- n2n-3.0.0-1038.x86_64.rpm
- n2n-3.0.0-1038 代表版本号
- x86_64 代表服务器系统架构
- rpm 代表rbp包管理器
- 相关文件内容请自行学习
- n2n_3.0.0-1038.x86_64.rpm


将下载好地安装包上传到服务器中

## 安装服务端

由于我使用ubuntu操作系统采用deb安装包进行演示

```Bash
sudo dpkg -i n2n_3.0.0-1038_amd64.deb
```

安装完成后 可使用命令进行启动

## 修改配置文件

```Bash
#进入到目录
cd /etc/n2n/
编辑配置文件
vim supernode.conf

```

常见情况下一般添加2个命令

```Bash
-p=5011
-c=/app/n2n/community.list
```

community.list 文件作用 创建相应的分区并保存密码.这主要是为了安全考虑

```Bash
vim community.list
```

```
#社区/频道名称
game
#固定语法使用客户端工具生成具体生成方式可见客户端文档
* name1 G5EItPg1RSFXAw5JdzQGJ7bGzpxhweVM
* name2 qpBVJLDVdBaOBJsgY2n80RjWk2mjNdKC
* name3 7tTHq9Uvj8dkSCsH1ux2xzqKETysybuD
* name3 jgPfCQVfpBMQsAmrxbcOVNvt9h9CFCkw
* name3 6YSY5qHymS5UcNV8aJPjA18pb9fbSVqr
* name3 oy6DsPRysaLj2vbz7reW3v8qQyi511Qi
* name3 83nhp6E1qETSyzfHqzfp408V9KCybGE3
* name3 Tkyq4U40BIBdlAWVAktFsGiXSZoakqGX
* name3 StTN8SttR8Z5lQW3cMLuS9FMv3hjvkn6
* name3 jopXW5CXlunCtdoj61IBJjrcGedw4PzR

```

其余命令参考如下:

- `-p <local port>`固定的本地UDP端口，默认为7654
- `-F <fed name>`超级节点的联合名称，默认为'*联合会'
- `-l <host:port>`ip地址或名称，以及已知超节点的端口
- `-M `禁用所有MAC和IP地址欺骗保护|非用户名密码身份验证社区
- `-V <version text> `发送最多19个字母的自定义超级节点版本字符串长度到边缘，在其管理端口输出中可见
- `-c <path> `包含允许的社区的文件
- `-a <net-net/n> `自动ip地址服务的子网范围，例如。'-a 192.168.0.0-192.168.255.0/24'，默认值
  至“10.128.255.0-10.255.255.0/24”
- `-f `不要分叉并作为守护进程运行，而是在前台运行
- `-t<port> `管理UDP端口，用于一台机器上的多个超级节点，|默认为5645
- `--management_...password <pw> `  管理端口密码，默认为“n2n”…密码＜pw＞|
- `-v `使更详细，根据需要重复
- `-u＜UID＞ `删除权限时要使用的数字用户ID
- `-g<GID> `删除权限时要使用的数字组ID

> 从技术上讲，所有参数都是可选的，但超级节点是必须的
> 需要至少一个参数才能运行。例如-v或-f，否则无法执行

## 启动服务端

```Bash
#启动服务端
sudo systemctl start supernode
#设置服务端开机自启
sudo systemctl enable supernode
```