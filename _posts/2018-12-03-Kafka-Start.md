---
layout: post
title: Kafka启动
date: 2018-12-03 
tag: Kafka
---

# Kafka启动过程记录

## 1.启动zookeeper

kafka用到zookeeper，因此如果您的机器上没有zookeeper服务，则需要先启动zookpeer服务


```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

查看日志最后几行为

```
[2018-12-04 10:44:35,222] INFO binding to port 0.0.0.0/0.0.0.0:2181 (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2018-12-04 10:44:35,980] INFO Accepted socket connection from /127.0.0.1:50044 (org.apache.zookeeper.server.NIOServerCnxnFactory)
[2018-12-04 10:44:36,014] INFO Client attempting to renew session 0x1676444e3770035 at /127.0.0.1:50044 (org.apache.zookeeper.server.ZooKeeperServer)
[2018-12-04 10:44:36,019] INFO Established session 0x1676444e3770035 with negotiated timeout 6000 for client /127.0.0.1:50044 (org.apache.zookeeper.server.ZooKeeperServer)
[2018-12-04 10:44:38,861] INFO Processed session termination for sessionid: 0x1676444e3770035 (org.apache.zookeeper.server.PrepRequestProcessor)
[2018-12-04 10:44:38,861] INFO Creating new log file: log.a46 (org.apache.zookeeper.server.persistence.FileTxnLog)
[2018-12-04 10:44:38,869] INFO Closed socket connection for client /127.0.0.1:50044 which had sessionid 0x1676444e3770035 (org.apache.zookeeper.server.NIOServerCnxn)
```

可以看到zookeeper服务的端口为2181

## 2.启动Kafka


```
bin/kafka-server-start.sh config/server.properties
```

查看日志


```
[2018-05-21 12:21:07,901] INFO Registered kafka:type=kafka.Log4jController MBean (kafka.utils.Log4jControllerRegistration$)
[2018-05-21 12:21:09,417] INFO starting (kafka.server.KafkaServer)
[2018-05-21 12:21:09,419] INFO Connecting to zookeeper on localhost:2181 (kafka.server.KafkaServer)
...
[2018-05-21 12:21:15,955] INFO [KafkaServer id=0] started (kafka.server.KafkaServer)
```

其中kafka的端口为9092


## 3.创建一个主题


```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

Created topic "test".
```

可以通过list topic命令查看所有的主题


```
bin/kafka-topics.sh --list --zookeeper localhost:2181
```

## 4.发送消息

Kafka带有一个命令行客户端，它将从文件或标准输入中获取输入，并将其作为消息发送到Kafka集群。默认情况下，每行将作为单独的消息发送。 

## 5.启动生产者

```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```

输入几条消息发送到服务器

```
>This is a message
>This is another message
```

## 6.启动消费者
消费者可以将消息转储到标准输出 
打开第四个个终端

```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

```

然后就可以在命令行看到生产者发送的消息了


```
This is a message
This is another message
```

