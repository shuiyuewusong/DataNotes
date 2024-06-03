# ElasticSearch 简介

Elasticsearch 是一个分布式的免费开源搜索和分析引擎，适用于包括文本、数字、地理空间、结构化和非结构化数据等在内的所有类型的数据。

Elasticsearch在速度可扩展性方面都非常的出色，所以他能够用于多种用例当中：应用程序的搜索网站的搜索日志数据处理和安全分析等场景中。

![image_9.png](image_9.png)

## ElasticSearch核心概念

- cluster：
    - Cluster 也就是集群的意思。Elasticsearch 集群由一个或多个节点组成，可通过其集群名称进行标识。
- node：
    - 单个 Elasticsearch 实例，每个节点都在单独的容器、虚拟机或物理机上运行。一个集群由一个或多个 node 组成
- document：
    - 文档，ElasticSearch中的最小的数据单元
- type：
    - 类型，在ElasticSearch 6.0版本以前一个index支持多种类型，6.0版本之后一个index只能支持一种类型_doc
- index：
    - 在索引中存储相同结构的文档（document），类似数据库中的table
- shard：
    - 分片，es是分布式的所以索引会被切分为多个分片，分别分配到多个节点之上
- replica：
    - Elasticsearch 为每个索引创建一个主分片和一个副本，用以保证数据可靠性






