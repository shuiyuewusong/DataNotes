# Nexus 私有化 Maven 中央仓库

## 官方文档
 [docker](https://hub.docker.com/r/sonatype/nexus3/#running)  
 [install](https://help.sonatype.com/en/install-nexus-repository-with-postgresql.html)  
 

## docker 安装部署步骤
```Bash
# 创建存储卷
docker volume create --name nexus-data
# 启动服务 
docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3

```

## 查看日志是否启动完成

```Bash
docker logs -f nexus
```
## 进入容器 查看 初始密码

```Bash
docker exec -it nexus bash 
cd /nexus-data
cat admin.password
```
##  访问网页
账户: admin  
密码 上述密码  
登陆后 默认修改密码 

```Bash
http://10.0.1.1:8081

```