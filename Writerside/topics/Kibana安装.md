# Kibana安装

## 官方教程

[Install Kibana](https://www.elastic.co/guide/en/kibana/8.13/install.html)

## 导入 Elastic PGP 密钥

```Bash
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

## 从 RPM 存储库安装

```Bash
vim /etc/yum.repos.d/kibana.repo

[kibana-8.x]
name=Kibana repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

## 安装

```Bash
sudo dnf install kibana 
```

## 启动 Elasticsearch 并为 Kibana 生成注册令牌

```Bash
/usr/share/elasticsearch/binbin/elasticsearch-create-enrollment-token -s kibana
```

## 运行 Kibana 并配置自启动

```Bash
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable kibana.service

sudo systemctl start kibana.service
#sudo systemctl stop kibana.service
```

## 启动kibana 后需要输入 生成的注册令牌 {id="kibana_1"}

启动 Kibana 并输入注册令牌以将 Kibana 与 Elasticsearch 安全地连接。








