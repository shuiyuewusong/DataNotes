# 配置docker-普通用户可以使用

## docker其他用户授权启动

## 创建一个名为docker的组（如果尚未创建） {id="docker_3"}

```Bash
sudo groupadd docker
```

## 将用户添加到docker组 {id="docker_1"}

```Bash
sudo usermod -aG docker your-username
```

## 重启docker服务并重新登录 {id="docker_2"}

```Bash
sudo systemctl restart docker
```



