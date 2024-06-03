# ElasticSearch节点类型

一个Elasticsearch实例代表了一个ES 节点，如果不通过 node.roles 设置节点的角色，es默认节点拥有以下所有角色。
为了达到节约资源并且能更有效的管理集群，在做集群规划时需要根据业务实际需求对elasticsearch的节点做节点角色设置，elasticsearch节点角色列表如下所示：

- Master
    - 功能介绍：Master节点负责轻量级集群范围操作，例如创建、删除索引，跟踪哪些节点是集群中节点以及分片分配到哪个节点等。主节点必须存在path.data目录，用于存放集群元数据信息。
    - 角色配置： node.roles: [ master ]
- Data
    - 功能介绍：保存索引数据，处理数据相关工作，如搜索和聚和。
    - 角色配置：node.roles: [ data ]
- Data_content
    - 功能介绍：内容数据节点容纳用户创建的内容，支持搜索和聚和等操作。
    - 角色配置：node.roles: [ data_content ]
- Data_frozen
    - 功能介绍：冻结节点
    - 角色配置： node.roles: [ data_frozen ]
- Data_hot
    - 功能介绍：该角色的nodes会根据数据进入ES的时间存储时序数据，hot层对数据读写要求较高，通常使用配置高的机器。
    - 角色配置：node.roles: [ data_hot ]
- Data_warm
    - 功能介绍：暖数据节点存储不再定期更新但仍在查询的索引。查询量的频率通常低于索引处于热层时的频率。性能较低的硬件通常可用于此层中的节点
    - 角色配置：node.roles: [ data_warm ]
- Data_clod
    - 功能介绍：冷数据节点存储访问频率较低的只读索引。此层使用性能较低的硬件，并且可以利用可搜索的快照索引来最小化所需的资源节点。
    - 角色配置： node.roles: [ data_cold ]
- Ingest
    - 功能介绍：Ingest node可以在文档索引之前执行ingest pipeline，进行数据清洗、字段填充等工作，对于数据预处理较为复杂，负载较重的ingest来说，应该单独部署ingest
      node
    - 角色配置：node.roles: [ ingest ]
- Ml
    - 功能介绍：机器学习节点运行作业并处理机器学习API请求。
    - 角色配置：node.roles: [ ml ]
      注意：一般开启ml角色的节点，推荐同时开启remote_cluster_client角色。
    - 角色配置：node.roles: [ ml,remote_cluster_client ]
- Remode_cluster_client
    - 功能介绍：远程集群客户端节点充当跨集群客户端并连接到 远程集群。连接后，您可以使用跨集群搜索来搜索远程集群。您还可以使用跨集群复制在集群之间同步数据。
    - 角色配置： node.roles: [ remote_cluster_client ]

- Transform
    - 功能介绍：根据我们需求自动生成另外一个索引，在新生成的索引中生成一个新的字段对原有索引中的某个值进行统计计算等操作。
    - 角色配置：node.roles: [ transform,remote_cluster_client ]
- Voting_only
    - 功能介绍：只能参与主节点的投票选举环节，但是自己不能被选举为master。
    - 角色配置：仅作为投票节点（node.roles: [ master,voting_only ]
      ），既是数据节点，也是仅投票节点（node.roles:[ data,master,voting_only ]）
- coordinating
    - 功能介绍：类似智能负载均衡器，该类型节点加入集群后会和其他节点一样，利用集群状态将请求路由到适当的地方，一般大型集群可以做单独的Coordinating配置。
    - 角色配置： node.roles: [  ]

