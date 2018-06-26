---
layout: post
title: Kafka Demo
date: 2018-06-26 
tag: Kafka
---

# Apache Kafka - 简介
在大数据中，使用了大量的数据。 关于数据，我们有两个主要挑战。第一个挑战是如何收集大量的数据，第二个挑战是分析收集的数据。 为了克服这些挑战，您必须需要一个消息系统。
Kafka专为分布式高吞吐量系统而设计。 Kafka往往工作得很好，作为一个更传统的消息代理的替代品。 与其他消息传递系统相比，Kafka具有更好的吞吐量，内置分区，复制和固有的容错能力，这使得它非常适合大规模消息处理应用程序。

常用的Kafka为**发布 - 订阅消息系统**

在发布 - 订阅系统中，消息被保留在主题中。 与点对点系统不同，消费者可以订阅一个或多个主题并使用该主题中的所有消息。 在发布 - 订阅系统中，消息生产者称为发布者，消息使用者称为订阅者。 一个现实生活的例子是Dish电视，它发布不同的渠道，如运动，电影，音乐等，任何人都可以订阅自己的频道集，并获得他们订阅的频道时可用。
![image](https://www.w3cschool.cn/attachments/tuploads/apache_kafka/publish_subscribe_messaging_system.jpg)
# Apache Kafka 基础
在深入了解Kafka之前，您必须了解主题，经纪人，生产者和消费者等主要术语。 下图说明了主要术语，表格详细描述了图表组件。
![image](https://www.w3cschool.cn/attachments/tuploads/apache_kafka/fundamentals.jpg)
在上图中，主题配置为三个分区。 分区1具有两个偏移因子0和1.分区2具有四个偏移因子0,1,2和3.分区3具有一个偏移因子0.副本的id与承载它的服务器的id相同。

假设，如果主题的复制因子设置为3，那么Kafka将创建每个分区的3个相同的副本，并将它们放在集群中以使其可用于其所有操作。 为了平衡集群中的负载，每个代理都存储一个或多个这些分区。 多个生产者和消费者可以同时发布和检索消息。

- **Topics（主题）**

属于特定类别的消息流称为主题。 数据存储在主题中。
主题被拆分成分区。 对于每个主题，Kafka保存一个分区的数据。 每个这样的分区包含不可变有序序列的消息。 分区被实现为具有相等大小的一组分段文件。
- **Partition（分区）**

主题可能有许多分区，因此它可以处理任意数量的数据。
- **Partition offset（分区偏移）**

每个分区消息具有称为 offset 的唯一序列标识。
- **Replicas of partition（分区备份）**

副本只是一个分区的备份。 副本从不读取或写入数据。 它们用于防止数据丢失。
- **Brokers（经纪人）**

代理是负责维护发布数据的简单系统。 每个代理中的每个主题可以具有零个或多个分区。 假设，如果在一个主题和N个代理中有N个分区，每个代理将有一个分区。

假设在一个主题中有N个分区并且多于N个代理(n + m)，则第一个N代理将具有一个分区，并且下一个M代理将不具有用于该特定主题的任何分区。

假设在一个主题中有N个分区并且小于N个代理(n-m)，每个代理将在它们之间具有一个或多个分区共享。 由于代理之间的负载分布不相等，不推荐使用此方案。
- **Kafka Cluster（Kafka集群）**

Kafka有多个代理被称为Kafka集群。 可以扩展Kafka集群，无需停机。 这些集群用于管理消息数据的持久性和复制。
- **Producers（生产者）**

生产者是发送给一个或多个Kafka主题的消息的发布者。 生产者向Kafka经纪人发送数据。 每当生产者将消息发布给代理时，代理只需将消息附加到最后一个段文件。 实际上，该消息将被附加到分区。 生产者还可以向他们选择的分区发送消息。
- **Consumers（消费者）**

Consumers从经纪人处读取数据。 消费者订阅一个或多个主题，并通过从代理中提取数据来使用已发布的消息。
- **Leader（领导者）**

 Leader 是负责给定分区的所有读取和写入的节点。 每个分区都有一个服务器充当Leader
。
- **Follower（追随者）**

跟随领导者指令的节点被称为Follower。 如果领导失败，一个追随者将自动成为新的领导者。 跟随者作为正常消费者，拉取消息并更新其自己的数据存储。

# Apache Kafka - 集群架构

Kafka的集群图。
![image](https://7n.w3cschool.cn/attachments/tuploads/apache_kafka/cluster_architecture.jpg)

组件和说明

- **Broker（代理）**

Kafka集群通常由多个代理组成以保持负载平衡。 Kafka代理是无状态的，所以他们使用ZooKeeper来维护它们的集群状态。 一个Kafka代理实例可以每秒处理数十万次读取和写入，每个Broker可以处理TB的消息，而没有性能影响。 Kafka经纪人领导选举可以由ZooKeeper完成。


- **ZooKeeper**

ZooKeeper用于管理和协调Kafka代理。 ZooKeeper服务主要用于通知生产者和消费者Kafka系统中存在任何新代理或Kafka系统中代理失败。 根据Zookeeper接收到关于代理的存在或失败的通知，然后产品和消费者采取决定并开始与某些其他代理协调他们的任务。

- **Producers（制片人）**

生产者将数据推送给经纪人。 当新代理启动时，所有生产者搜索它并自动向该新代理发送消息。 Kafka生产者不等待来自代理的确认，并且发送消息的速度与代理可以处理的一样快。

- **Consumers（消费者）**
 
因为Kafka代理是无状态的，这意味着消费者必须通过使用分区偏移来维护已经消耗了多少消息。 如果消费者确认特定的消息偏移，则意味着消费者已经消费了所有先前的消息。 消费者向代理发出异步拉取请求，以具有准备好消耗的字节缓冲区。 消费者可以简单地通过提供偏移值来快退或跳到分区中的任何点。 消费者偏移值由ZooKeeper通知。

# Apache Kafka - 基本操作

## 启动ZooKeeper

```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

要启动Kafka Broker，请键入以下命令 -


```
bin/kafka-server-start.sh config/server.properties
```

启动Kafka Broker后，在ZooKeeper终端上键入命令 jps ，您将看到以下响应 -


```
821 QuorumPeerMain
928 Kafka
931 Jps
```
现在你可以看到两个守护进程运行在终端上，QuorumPeerMain是ZooKeeper守护进程，另一个是Kafka守护进程。

## 创建Kafka主题

Kafka提供了一个名为 kafka-topics.sh 的命令行实用程序，用于在服务器上创建主题。 打开新终端并键入以下示例。


```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 
--partitions 1 --topic topic-name
```

要获取Kafka服务器中的主题列表，可以使用以下命令 


```
bin/kafka-topics.sh --list --zookeeper localhost:2181
```
## 启动生产者以发送消息


```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic topic-name
```
从上面的语法，生产者命令行客户端需要两个主要参数 -

代理列表 - 我们要发送邮件的代理列表。 在这种情况下，我们只有一个代理。 Config / server.properties文件包含代理端口ID，因为我们知道我们的代理正在侦听端口9092，因此您可以直接指定它。

主题名称 - 以下是主题名称的示例。


```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic Hello-Kafka
```

## 启动消费者以接收消息
与生产者类似，在 config / consumer.proper-ties 文件中指定了缺省使用者属性。 打开一个新终端并键入以下消息消息语法。


```
bin/kafka-console-consumer.sh --zookeeper localhost:2181 —topic topic-name 
--from-beginning

```


最后，您可以从制作商的终端输入消息，并看到他们出现在消费者的终端。 到目前为止，您对具有单个代理的单节点群集有非常好的了解。 现在让我们继续讨论多个代理配置。