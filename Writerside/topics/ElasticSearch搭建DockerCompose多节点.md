# ElasticSearch搭建DockerCompose多节点

## 配置并启动集群

### 安装 Docker Compose

[Docker Compose](https://docs.docker.com/compose/install/)
如果您使用的是 Docker Desktop，则会自动安装 Docker Compose。请确保为 Docker Desktop 分配至少 4GB
内存。您可以通过转到“设置”>“资源”来调整 Docker Desktop 中的内存使用情况。

### 为项目创建或导航到空目录

### 下载并保存以下文件在项目目录中：

[.env](https://github.com/elastic/elasticsearch/blob/8.13/docs/reference/setup/install/docker/.env)

[docker-compose.yml](https://github.com/elastic/elasticsearch/blob/8.13/docs/reference/setup/install/docker/docker-compose.yml)

### 在文件中，为和 变量.env指定密码

```ELASTIC_PASSWORD KIBANA_PASSWORD```

密码必须是字母数字，不能包含特殊字符，例如 !或@。文件中包含的 bash 脚本docker-compose.yml仅适用于字母数字字符。示例：

```Bash
# 'elastic'用户的密码（至少 6 个字符)
ELASTIC_PASSWORD=changeme

# 'kibana_system' 用户的密码（至少 6 个字符）
KIBANA_PASSWORD=changeme
```

### 在.env文件中，设置STACK_VERSION为当前 Elastic Stack 版本

```Bash
# Elastic产品版本
STACK_VERSION=8.13.4
```

### 默认情况下，Docker Compose 配置9200在所有网络接口上公开端口

为了避免将端口暴露9200给外部主机，请在文件 中设置ES_PORT为。这可确保 Elasticsearch 只能从主机访问。127.0.0.1:9200.env

```Bash
# 向主机公开 Elasticsearch HTTP API 的端口
#ES_PORT=9200
ES_PORT=127.0.0.1:9200
```

### 要启动集群，请从项目目录运行以下命令。

```Bash
docker compose up -d
```

### 集群启动后，在 Web 浏览器中 打开 即可访问 Kibana。

http://localhost:5601

### elastic使用ELASTIC_PASSWORD您之前设置的 用户 身份登录 Kibana

## 停止并删除集群

### 要停止集群，请运行docker-compose down。使用 重新启动集群时，Docker 卷中的数据将被保留和加载docker-compose up。

```Bash
docker compose down
```

### 要在停止集群时删除网络、容器和卷，请指定以下-v选项
```Bash
docker compose down -v
```








