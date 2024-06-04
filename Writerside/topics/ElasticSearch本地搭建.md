# ElasticSearch本地搭建

## 官方教程

[install-elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

## 导入 Elasticsearch GPG 密钥

```Bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

## 从 RPM 存储库安装

```Bash
sudo vim /etc/yum.repos.d/elasticsearch.repo

[elasticsearch]
name=Elasticsearch repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md
```

## 安装

```Bash
sudo dnf install --enablerepo=elasticsearch elasticsearch -y

```

## 安装时会输出默认密码需要记录密码

```Bash
export ELASTIC_PASSWORD="your_password"

```

![image_10.png](image_10.png)

## 多节点部署需要 进行操作

修改每个节点的配置文件

```Bash
/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node
```

## 其他节点安装后执行命令

```Bash
/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <enrollment-token>
```

## 修改配置文件配置集群内容

参考文档:[ElasticSearch集群配置文件](ElasticSearch集群配置文件.md)

## 配置系统启动时自启动

```Bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service


sudo systemctl start elasticsearch.service
sudo systemctl stop elasticsearch.service

```

## 相关日志

```Bash
sudo journalctl -f
sudo journalctl --unit elasticsearch
sudo journalctl --unit elasticsearch --since  "2016-10-30 18:17:16"
```






