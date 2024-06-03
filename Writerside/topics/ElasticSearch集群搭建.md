# ElasticSearch搭建Docker单节点

## 启动单节点集群

### 安装 Docker

### 创建一个新的docker网络 {id="docker_1"}

```Bash
docker network create elastic
```

### 拉取 Elasticsearch Docker 镜像

```Bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.13.4
```

### 可选：为您的环境安装 Cosign。然后使用 Cosign 验证 Elasticsearch 映像的签名

```Bash
wget https://artifacts.elastic.co/cosign.pub
cosign verify --key cosign.pub docker.elastic.co/elasticsearch/elasticsearch:8.13.4
```

该cosign命令以 JSON 格式打印检查结果和签名负载：

```Bash
Verification for docker.elastic.co/elasticsearch/elasticsearch:8.13.4 --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - The signatures were verified against the specified public key
```

### 启动 Elasticsearch 容器

```Bash
docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.13.4
```

> 使用-m标志为容器设置内存限制。这样就无需手动设置 JVM 大小。

## 添加更多节点

### 使用现有节点为新节点生成注册令牌,注册令牌有效期为 30 分钟。

```Bash
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node
```

### 启动一个新的 Elasticsearch 容器。将注册令牌作为环境变量包含在内。 {id="elasticsearch_1"}

```Bash
docker run -e ENROLLMENT_TOKEN="<token>" --name es02 --net elastic -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.13.4
```

### 调用cat nodes API来验证节点是否已添加到集群

```Bash
curl --cacert http_ca.crt -u elastic:$ELASTIC_PASSWORD https://localhost:9200/_cat/nodes
```

## 运行 Kibana

### 拉取 Kibana Docker 镜像

```Bash
docker pull docker.elastic.co/kibana/kibana:8.13.4
```

### 可选：验证 Kibana 图像的签名。 {id="kibana_1"}

```Bash
wget https://artifacts.elastic.co/cosign.pub
cosign verify --key cosign.pub docker.elastic.co/kibana/kibana:8.13.4
```

### 启动 Kibana 容器。 {id="kibana_2"}

```Bash
docker run --name kib01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.13.4
```

### Kibana 启动时，会将生成的一个唯一链接输出到终端。要访问 Kibana，请在 Web 浏览器中打开此链接。

### 在您的浏览器中，输入启动 Elasticsearch 时生成的注册令牌。 {id="elasticsearch_2"}

要重新生成令牌，请运行：

```Bash
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

### elastic使用启动 Elasticsearch 时生成的密码以用户身份登录 Kibana

要重新生成密码，请运行：

```Bash
docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

## 删除容器

要删除容器及其网络，请运行：

```Bash
# Remove the Elastic network
docker network rm elastic

# Remove Elasticsearch containers
docker rm es01
docker rm es02

# Remove the Kibana container
docker rm kib01
```












