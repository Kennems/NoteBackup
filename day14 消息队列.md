# day14 消息队列

面对：

- 系统奔溃
- 服务处理能力有限
- 链路耗时长尾
- 日志如何处理

解决方案：

- 解耦
- 削峰
- 异步
- 日志处理

消息队列（MQ），指保存消息的一个容器，本质是某个队列。但这个队列需要支持高吞吐，高并发，并且高可用。

## 消息队列-Kafka

创建集群-》新增Topic-》编写生产者逻辑-》编写消费者逻辑。

Topic：逻辑队列，不同Topic可以建立不同的Topic。

Cluster：物理集群，每个集群中可以建立多个不同的Topic。

Consumer：消费者，负责消费Topic中的消息。

ConsumerGroup：消费者组，不同组Consumer消费进度互不干涉。

Offset：消息在Partition内的相对位置信息，可以理解为唯一ID，在Partition内部严格递增。

每个分片有多个Replica，Leader Replica将会从ISR中选出。

## Kafka架构

![image-20230209195211461](D:\文档\文本\学习笔记\typora_imag\image-20230209195211461.png)

ZooKeeper：负责存储器群员信息，包括分区信息等。

### Producer-批量发送

### Producer-数据压缩

通过压缩，减少消息大小，目前支持Snappy，Gzip，LZ4，ZSTD压缩算法。

### Broker-数据的存储

### Broker-消息文件结构

数据路径：/Topic/Partition/Segment/（log|index|timeindex|...)

### Broker-磁盘结构

移动磁头找到对应磁道，磁盘转动，找到对应扇区，最后写入。寻到成本较高，因此顺序写可以减少寻道所带来的时间成本。

### Broker-顺序写

采用顺序写的方式进行写入，以提高写入效率。

### Broker-如何找到消息

Consumer通过发送FetchRequest请求消息数据，Broker会将指定Offset处的消息，按照时间窗口和消息大小窗口发送给Consumer。

### Broker偏移量索引文件

二分找到目标文件。

### Broker时间戳索引文件

二分找到小于目标时间戳最大的索引位置，再通过寻找offset的方式找到最终数据。

### Broker-传统数据拷贝

### Broker-零拷贝

### Consumer-消息的接收端

### Consumer-Low Level

通过动手进行分配，哪一个Consumer消费哪一个Partition完全由业务来决定。

### Consumer-Hign Level

### Consumer Rebalance

### Kafka问题总结

1. 运维成本高
2. 对于负载不均衡的场景，解决方案复杂
3. 没有自己的缓存，完全依赖Page Cache
4. Controller和Coordinator和Broker在同一进程中，大量IO会造成其性能下降。

## 消息队列-BMG

兼容Kafka协议，存算分离，云原生消息队列。

架构图

![image-20230209203313561](D:\文档\文本\学习笔记\typora_imag\image-20230209203313561.png)

运维操作对比

![image-20230209203403154](C:\Users\zg\AppData\Roaming\Typora\typora-user-images\image-20230209203403154.png)

### HDFS写文件流程

随机选择一定数量的DataNode进行写入。

### Broker-Partition 状态机

![image-20230209203945905](D:\文档\文本\学习笔记\typora_imag\image-20230209203945905.png)

保证对于任意分片在同一时刻只能在一个Broker上存活。

### Broker-写文件流程

![image-20230209204043742](D:\文档\文本\学习笔记\typora_imag\image-20230209204043742.png)

### Broker-写文件 Failover

如果DataNode节点挂了或者其他原因导致我们写文件失败。

### Proxy

### 多机房部署

### 泳道消息

BOE：Bytedance Offline Environment, 是一套完全独立的线下机房环境。

PPE：Product Preview Environment， 产品预览环境。

BOE：多人同时测试，需要等待上一个人测试完成。每多一个测试人员，都需要重新搭建一个相同配置的Topic，造成人力和资源的浪费。

PPE：对于PPE的消费者来说，资源没有生产环境多，所以无法承受生产环境的流量。

解决主干泳道隔离问题以及用到资源重复创建问题。

### Databus

- 简化消息队列客户端复杂度
- 解耦业务与Topic
- 缓解集群压力，提高吞吐。

### Mirror

使用Mirror通过最终一致的方式，解决跨Region读写问题。

### Index

直接在BMQ中将数据结构化，配置索引DDL，异步构建索引后，通过Index Query服务读出数据。

### Parquet

Apache Parquet是Hadoop生态圈中一种新型列式存储格式，它可以兼容Hadoop生态圈中大多数计算框架（Hadoop，Spark等），被多种查询引擎支持（Hive，Impala，Drill等）。

直接在BMQ中将数据结构化，通过Parquet Engine，可以使用不同的方式构建Parquet格式文件。

## 消息队列-RocketMQ

### RocketMQ高级特性

#### 事务场景

#### 事务消息

#### 延迟发送

#### 延迟消息

#### 处理失败

#### 消费重试和死信队列

## 今日总结

今天学习了什么是消息队列，消息队列的应用场景，常用的消息队列——BMG和RocketMQ及其特性，完善了自身对高吞吐，高并发，高可用需求的解决能力。