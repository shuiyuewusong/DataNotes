# ElasticSearch集群配置文件

## elasticsearch.yml

通过 dnf/yum 安装的es文件位于:

```Bash
/etc/elasticsearch/elasticsearch.yml
```

可通过vim 等命令编辑

## 配置内容及含义 

```yaml
# 配置集群名称 *必填*
#Use a descriptive name for your cluster
cluster.name: my-application
# 配置节点名称 *必填*
#Use a descriptive name for the node
node.name: ES-1
#对外暴露ip范围 如果在内网则可以直接使用0.0.0.0  可以不填写
#address here to expose this node on the network
network.host: 0.0.0.0
#配置ES集群相关ip 如果是默认端口则可以不填写端口号 一般情况下会自动生成不需要补充
#Pass an initial list of hosts to perform discovery when this node is started
discovery.seed_hosts: [ "10.0.5.17:9300", "10.0.5.15:9300","10.0.5.16:9300" ]
#配置ES集群相关主节点名 一般情况下不需要填写
#Bootstrap the cluster using an initial set of master-eligible nodes
cluster.initial_master_nodes: [ "ES-1", "ES-2","ES-3" ]

```








