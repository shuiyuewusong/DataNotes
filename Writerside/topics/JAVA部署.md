# JAVA部署

## 更新软件包

```Bash
#centos 系统 
cd ~
wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm
sudo rpm -ivh jdk-17_linux-x64_bin.rpm

#ubuntu 系统
sudo apt update 
sudo apt install openjdk-17-jdk

```

## 将JAVA包拷贝到/home目录

## 执行命令 运行

```Bash
nohup java -jar cte-0.0.1-SNAPSHOT.jar -Dspring.config.location=application.properties &

```

## 检查输出是否正确

```Bash
tail -f nohup.out
```





