# web小说reader

官方文档地址
[https://github.com/hectorqin/reader](https://github.com/hectorqin/reader)

## Linux部署

```Bash
# 安装JDK
sudo yum install java-21-openjdk*
# 解压文件
unzip reader-server-$version.zip
# 检查端口是否开放
```

![image_123.png](image_123.png)

```Bash
#执行命令
# -i inviteCode 设置多用户模式下的邀请码，默认 以配置文件 conf/application.properties 为准
# -k secureKey 设置多用户模式下的管理密码，默认 以配置文件 conf/application.properties 为准
./bin/startup.sh -i  xxxxxxxxxx   -k  xxxxxxxxxx

# web端 http://localhost:8080/
# 接口地址 http://localhost:8080/reader3/
conf/app** 文件可以修改其他配置
例如修改端口号
```

## 已知问题

grep 命令 -Eo O大写了需要手动改成小写

![image_124.png](image_124.png)