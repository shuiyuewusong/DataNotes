# NEW-API

新一代大模型网关与AI资产管理系统

[new-api](https://github.com/Calcium-Ion/new-api)

## 部署流程

采用Ubuntu
### 安装常用工具
```Bash
sudo apt install git -y
```
### 安装docker

```Bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo apt-get update
sudo apt-get install docker-compose-plugin
```

### 安装new-api

```Bash
sudo mkdir /app
sudo chmod 777 /app
cd /app
git clone https://github.com/Calcium-Ion/new-api.git
cd new-api
# 按需编辑 docker-compose.yml
# vim docker-compose.yml

sudo docker compose up -d

```
### 安装nginx 
```Bash
 sudo apt install nginx -y
 # 修改nginx 配置 路由转发
 vim  /etc/nginx/nginx.conf
 sudo nginx -t
 sudo nginx -s 
 sudo nginx -s reload
```

