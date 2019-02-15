---
title: NoSQL数据库介绍
date: 2019-02-15
---

NoSQL 数据库介绍以及 NoSQL 数据库的选择

<!-- more -->

### 基本概念

NoSQL 指的是**非关系型的数据库**。NoSQL 最常见的解释是 non-relational，有时也称作 Not Only SQL 的缩写，是**对不同于传统的关系型数据库的数据库管理系统的统称**。主要用于**超大规模数据**的存储。（例如 Google 或Facebook 每天为他们的用户收集万亿比特的数据）。这些类型的数据存储**不需要固定的模式**，**无需多余操作就可以横向扩展**。MySQL 和 NoSQL 都有各自的特点和使用的应用场景，两者的紧密结合将会给 web2.0 的数据库发展带来新的思路。**让关系数据库关注在关系上，NoSQL 关注在存储上**。

### RDBMS vs NoSQL

RDBMS

- 高度组织化结构化数据
- 结构化查询语言 (SQL)
- 数据和关系都存储在单独的表中
- 数据操纵语言，数据定义语言
- 严格的一致性
- 基础事务

NoSQL

- 代表着不仅仅是SQL
- 没有声明性查询语言
- 没有预定义的模式 -键 - 值对存储，列存储，文档存储，图形数据库
- 最终一致性，而非ACID属性
- 非结构化和不可预知的数据
- CAP定理
- 高性能，高可用性和可伸缩性

### 常见的 NoSQL 数据库

目前 NoSQL 类型的数据库至少有225种，详细内容见[官网](http://nosql-database.org/) ，下面对常见的几种 NoSQL 数据库进行简单介绍

#### 键值存储数据库

这一类数据库主要会使用到一个哈希表，这个表中有一个特定的键和一个指针指向特定的数据。Key/value模型对于IT系统来说的优势在于**简单、易部署**。

##### Redis

redis是一个基于内存的 key-value 存储系统，同时提供了更加丰富的数据结构和运算的能力，成功用法是替代memcached，通过 checkpoint 和 commit log 提供了快速的宕机恢复，同时支持 replication 提供读可扩展和高可用。

适用场景

适用于数据变化快且数据库大小可预见（适合内存容量）的应用程序

相关网址

- 官网

  <https://redis.io/>

- github

  <https://github.com/antirez/redis>

##### LevelDB

一个 google 实现的非常高效的 key-value 数据库，LevelDB 是单进程的服务，性能非常之高，在一台4核Q6600的CPU机器上，每秒钟写数据超过40w，而随机读的性能每秒钟超过10w。此处随机读是完全命中内存的速度，如果是不命中速度大大下降。

特性

- 键值都是string类型

- 基于磁盘，数据量不受限于内存大小

- 模型单一简单，数据落盘高可靠

- LSM模型天然写优化

- 顺序写盘的方式非常适合于ssd

- 支持数据压缩等操作，这对于减小存储空间以及增快IO效率都有直接的帮助

- 可以指定排序规则

- 不足是仅提供了一个库，需要自己封装server端

相关网址

- 官网

  <http://leveldb.org/>

- github

  <https://github.com/google/leveldb>

##### RocksDB

RocksDB 是使用 C++ 编写的嵌入式kv存储引擎，其键值均允许使用二进制流。由 Facebook 基于 levelDB 开发，提供向后兼容的 levelDB API。

特性

- 可用于存储键和值,可以是任意大小的字节流
- 支持原子读和写
- 具有高度灵活的配置功能,可以通过配置使其运行在各种各样的生产环境,包括纯内存,Flash,硬盘或HDFS
- 支持各种压缩算法，并提供了便捷的生产环境维护和调试工具
- 为快速而又低延迟的存储设备（例如闪存或者高速硬盘）而特殊优化处理。RocksDB将最大限度的发挥闪存和RAM的高度率读写性能
- 前缀迭代器：应用可以指定一个键值前缀，配置一个前缀提取器。针对每个键值前缀，RockDB都会其来存储bloom。通过bloom，迭代器在扫描指定前缀的键值的时候，就可以避免扫描那些没有这种前缀键值的文件了
- 数据库中的所有数据都是逻辑上排好序的

相关网址

- 官网

  <https://rocksdb.org/>

- github

  <https://github.com/facebook/rocksdb>

- 中文网站

  <https://rocksdb.org.cn/>

#### 文档型数据库

文档型数据库的灵感是来自于 Lotus Notes 办公软件的，而且它同第一种键值存储相类似。该类型的数据模型是版本化的文档，半结构化的文档以特定的格式存储，比如 JSON。文档型数据库可 以看作是**键值数据库的升级版**，允许之间嵌套键值。而且文档型数据库**比键值数据库的查询效率更高**。

##### MongoDB

MongoDB 是一个基于分布式文件存储的数据库，介于关系数据库和非关系数据库之间，**是非关系数据库当中功能最丰富，最像关系数据库的**。MongoDB 最大的特点是他支持的**查询语言非常强大**，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还**支持对数据建立索引并指定索引有效期**。MongoDB 支持 RUBY，PYTHON，JAVA，C++，PHP，C# 等多种语言。

MongoDB 最吸引人的莫过于提供了 sql 接口，是目前 NoSQL 里最像 mysql 的，只是没有 ACID 的特性，发展很快，支持了索引等特性，上手容易，对于数据量远超内存限制的场景来说，还需要慎重。

特性

- BSON（一种JSON的扩展）存储
- 支持服务器端js脚本
- 支持聚合的管道
- 支持MapReduce
- 支持的查询语言非常强大
- 面向集合存储，易存储对象类型的数据
- 支持完全索引，包含内部对象
- 单个文档大小有限制、内存要求较大、非事务
- 可以使用不同的存储引擎，如rocksdb
- 实时的插入、更新与查询的需求，并具备应用程序实时数据存储所需的复制及高度伸缩性

相关网址

- 官网

  <https://www.mongodb.com/>

- github

  <https://github.com/mongodb/mongo>

- Elasticsearch、MongoDB和Hadoop比较

  <https://www.jianshu.com/p/2c7b0c76fa04>

#### 列存储数据库

这部分数据库通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是**指向了多个列。这些列是由列家族来安排的**。

##### HBase

HBase(Hadoop Database)，是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统，利用HBase技术可在廉价PC Server上搭建起大规模结构化存储集群。HBase**是Google Bigtable的开源实现**，类似Google Bigtable利用GFS作为其文件存储系统，HBase**利用Hadoop HDFS作为其文件存储系统**；Google运行MapReduce来处理Bigtable中的海量数据，HBase同样**利用Hadoop MapReduce来处理HBase中的海量数据**；Google Bigtable利用 Chubby作为协同服务，HBase**利用Zookeeper作为协同服务**。

Pig和Hive还为HBase提供了**高层语言支持**，使得在HBase上进行数据统计处理变的非常简单。Sqoop则为HBase提供了**方便的RDBMS数据导入功能**，使得传统数据库数据向HBase中迁移变的非常方便。

特性

- 强一致性读写：HBase不是“Eventual Consistentcy（最终一致性）”数据存储，这让它很适合高速计数聚合类任务
- 自动分片（Automatic sharding）：HBase表通过region分布在集群中，数据增长时，region会自动分割并重新分布
- RegionServer自动故障转移
- Hadoop/HDFS集成：HBase支持开箱即用HDFS作为它的分布式文件系统
- MapRecue：HBase通过MapRecue支持大并发处理
- 可查询历史版本
- Java客户端API：HBase支持易于使用的Java API进行编程访问
- Thrift/REST API：HBase也支持Thrift和Rest作为非Java前端访问
- Block Cache和Bloom Filter：对于大容量查询优化，HBase支持Block Cache和Bloom Filter
- 运维管理：HBase支持JMX提供内置网页用于运维

适用场景

1. 写密集型应用，每天写入量巨大，而相对读数量较小的应用，比如IM的历史消息，游戏的日志等等
2. 不需要复杂查询条件来查询数据的应用，HBase**只支持基于rowkey的查询**，对于HBase来说，单条记录或者小范围的查询是可以接受的，大范围的查询由于分布式的原因，可能在性能上有点影响，而对于像SQL的join等查询，HBase无法支持
3. 对性能和可靠性要求非常高的应用，由于HBase**本身没有单点故障，可用性非常高**

相关网址

- 官网

  <https://hbase.apache.org/>

- github

  <https://github.com/apache/hbase>

- 一张图看懂HBase

  <https://blog.csdn.net/u011331430/article/details/79036441>

##### Accumulo

Apache Accumulo 是一个可靠的、可伸缩的、高性能的排序分布式的 Key-Value 存储解决方案，基于单元访问控制以及可定制的服务器端处理。使用 Google BigTable 设计思路，基于Apache Hadoop、Zookeeper 和 Thrift 构建。由美国国家安全局开发，并于2011年发布的。

Accumulo 支持**高效存储和结构多样化**，包括范围查询，为 MapReduce 的 job 提供 input 和 output 支持。 Accumulo 优点是**自动负载均衡和分片，数据压缩和安全机制**。

特性

- 使用HDFS存储数据，zk保证一致性
- 对数据保护的**安全性高**
- 支持ACID原则
- 读写性能强

和HBase差别不是很大，关键特性在于安全性，可以控制每一列是否允许用户进行查询，除政府之外的企业用的比较少

相关网址

- 官网

  <https://accumulo.apache.org/>

- github

  <https://github.com/apache/accumulo>

- Leveldb/Rocksdb/Accumulo简单比较

  <https://www.jianshu.com/p/4c57cd82ccde>

##### Cassandra

一个大规模可扩展的分布式开源 NoSQL 数据库，完美适用于跨数据中心/云端的结构化数据、半结构化数据和非结构化数据，同时，Cassandra 高可用、线性可扩展、高性能、无单点。类似 amazon 的 Dynamo，和 Dynamo不同之处在于，Cassandra 结合了 Google Bigtable 的 ColumnFamily 的数据模型

在关系型数据库中，数据模式的修改成本很高，而这却降低了查询模式的修改成本；Cassandra 则与之相反，**改变查询模式要比改变其数据模式代价更高**

例如：银行业，金融业（虽然对于金融交易不是必须的，但这些产业对数据库的要求会比它们更大）写比读更快，所以一个自然的特性就是实时数据分析

相关网址

- 官网

  <http://cassandra.apache.org/>

- github

  <https://github.com/apache/cassandra>

- HBase和Cassandra比较

  <https://yq.aliyun.com/articles/25706>

##### Kudu

Kudu 实现的是既可以实现数据的快速插入与实时更新，也可以实现数据的快速分析。Kudu 的定位不是取代HBase，而是以降低写的性能为代价，提高了批量读的性能，使其能够实现快速在线分析。设计上借鉴了 HBase。

Kudu 试图在 OLAP 与 OLTP 之间，寻求一个最佳的结合点，从而在一个系统的一份数据中，既能支持 OLTP 型实时读写能力又能支持 OLAP 型分析；Kudu 作为一个新的分布式存储系统期望有效提升 CPU 的使用率，而低 CPU 使用率恰是 HBase/Cassandra 等系统的最大问题

适用于需要进行实时分析的系统中，按行进行读取时效率较差

相关网址

- 官网

  <https://kudu.apache.org/>

- github

  <https://github.com/apache/kudu>

- kudu论文

  <https://kudu.apache.org/kudu.pdf>

- Kudu：一个融合低延迟写入和高性能分析的存储系统

  <https://www.jianshu.com/p/a6c0fdec3d7b>

- Kudu vs HBase

  <https://bigdata.163.com/product/article/15>

---
列存对于 OLAP 非常友好，但在写入的时候压力可能会比较大，如果一个 table 有很多 column，写入性能影响会非常明显。行存则是对于 OLTP 比较友好，但在读取的时候会将整行数据全读出来，在一些分析场景下压力会有点大。但无论列存还是行存，都是为满足不同的业务场景而服务的，有一些数据库使用的是行列混存

#### 图形数据库

图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它是**使用灵活的图形模型**，并且能够扩展到多个服务器上。NoSQL数据库没有标准的查询语言(SQL)，因此进行数据库查询需要制定数据模型。许多NoSQL数据库都有REST式的数据接口或者查询API。

#####  Neo4j

Neo4j 是目前最流行的图形数据库，支持完整的事务，在属性图中，图是由顶点（Vertex），边（Edge）和属性（Property）组成的，顶点和边都可以设置属性，顶点也称作节点，边也称作关系，每个节点和关系都可以由一个或多个属性。Neo4j 创建的图是用顶点和边构建一个有向图，其查询语言 cypher 已经成为事实上的标准。

应用场景：适用于图形一类数据。这是Neo4j与其他nosql数据库的最显著区别

例如：社会关系，公共交通网络，地图及网络拓谱

相关网址

- 官网

  <https://neo4j.com/>

- github

  <https://github.com/neo4j/neo4j>

- 在线演示

  <http://console.neo4j.org/>

##### OrientDB

OrientDB 是在2011年发布的新一代分布式 NoSQL 数据库，能够处理 Graph、Document、Key-Value、GeoSpatial 和 Reactive 五种模型，是目前最流行的一款多模型数据库。

在 OrientDB 中，任何类型的数据都是可搜索的，用户域的建模支持面向对象的概念，可以很容易地扩展。 每个模型不只是一个层，而是共存于一个引擎中。可选**无模式、全模式或混合模式**。

相关网址

- 官网

  <https://orientdb.com/>

- github

  <https://github.com/orientechnologies/orientdb>

- orientdb vs neo4j

  <https://orientdb.com/orientdb-vs-neo4j/>

##### JanusGraph 

JanusGraph 是 Titan1.0.0 版本的延续，Titan 是从2012年开始开发，到2016年停止维护的一个分布式图数据库。JanusGraph 继承了 Titan 的全部功能并做了进一步的改进，并支持 Hadoop 2 和 Tinkerpop 3.2.3，采用 Gremlin 图查询语言。

相关网址

- 官网

  <http://janusgraph.org/>

- github

  <https://github.com/JanusGraph/janusgraph>

- 中文文档

  <https://xiuxiuing.gitee.io/blog/2018/11/13/JanusGraph/>

- JanusGraph学习手册

  <https://blog.csdn.net/xd2ss/article/details/83930009>

### NoSQL数据库选择

[选择合适的NoSQL数据库](http://www.dataversity.net/choose-right-nosql-database-application/)

![NoSQL数据库选择](http://assets.processon.com/chart_image/5c1c4fcbe4b0bcd70c413c6b.png?_=1545365110359)