---
layout: post
title: Spark之初识Spark
date: 2017-09-14
tag: spark
---

毕业论文需要，外加为了以后更好的工作，也为了自己学习更多的知识，挖了一个Spark的坑。现在开始填坑。
---

### 1.Spark简介

Spark是加州大学伯克利分校AMP实验室（Algorithms, Machines, and People Lab）开发通用内存并行计算框架。Spark在2013年6月进入Apache成为孵化项目，8个月后成为Apache顶级项目，速度之快足见过人之处，Spark以其先进的设计理念，迅速成为社区的热门项目，围绕着Spark推出了Spark SQL、Spark Streaming、MLLib和GraphX等组件，也就是BDAS（伯克利数据分析栈），这些组件逐渐形成大数据处理一站式解决平台。从各方面报道来看Spark抱负并非池鱼，而是希望替代Hadoop在大数据中的地位，成为大数据处理的主流标准，不过Spark还没有太多大项目的检验，离这个目标还有很大路要走。

Spark使用Scala语言进行实现，它是一种面向对象、函数式编程语言，能够像操作本地集合对象一样轻松地操作分布式数据集（Scala 提供一个称为 Actor 的并行模型，其中Actor通过它的收件箱来发送和接收非同步信息而不是共享数据，该方式被称为：Shared Nothing 模型）。在Spark官网上介绍，它具有运行速度快、易用性好、通用性强和随处运行等特点。

### 2.Spark术语

- 2.1 Spark运行模式

运行环境 | 模式 | 描述
---|---|---
Local | 本地模式 | 常用于本地开发测试，本地还分为local单线程和local-cluster多线程;
Standalone | 集群模式 | 典型的Mater/slave模式，不过也能看出Master是有单点故障的；Spark支持 ZooKeeper来实现HA
On yarn | 集群模式 | 运行在yarn资源管理器框架之上，由yarn负责资源管理，Spark负责任务调度和计算
On mesos | 集群模式 | 运行在mesos资源管理器框架之上，由mesos负责资源管理，Spark负责任务调度和计算
On cloud | 集群模式 | 比如AWS的EC2，使用这个模式能很方便的访问Amazon的S3;Spark支持多种分布式存储系统：HDFS和S3

- 2.2 Spark常用术语

术语 | 描述
---|---
Application | Spark的应用程序，包含一个Driver program和若干Executor
SparkContext | Spark应用程序的入口，负责调度各个运算资源，协调各个Worker Node上的Executor
Driver Program | 运行Application的main()函数并且创建SparkContext
Executor | 是为Application运行在Worker node上的一个进程，该进程负责运行Task，并且负责将数据存在内存或者磁盘上。每个Application都会申请各自的Executor来处理任务
Cluster Manager | 在集群上获取资源的外部服务(例如：Standalone、Mesos、Yarn)
Worker Node | 集群中任何可以运行Application代码的节点，运行一个或多个Executor进程
Task | 运行在Executor上的工作单元
Job | SparkContext提交的具体Action操作，常和Action对应
Stage | 每个Job会被拆分很多组task，每组任务被称为Stage，也称TaskSet
RDD | 是Resilient distributed datasets的简称，中文为弹性分布式数据集;是Spark最核心的模块和类
DAGScheduler | 根据Job构建基于Stage的DAG，并提交Stage给TaskScheduler
TaskScheduler | 将Taskset提交给Worker node集群运行并返回结果
Transformations | 是Spark API的一种类型，Transformation返回值还是一个RDD，所有的Transformation采用的都是懒策略，如果只是将Transformation提交是不会执行计算的

### 生态系统
Spark生态圈也称为BDAS（伯克利数据分析栈），是伯克利APMLab实验室打造的，力图在算法（Algorithms）、机器（Machines）、人（People）之间通过大规模集成来展现大数据应用的一个平台。伯克利AMPLab运用大数据、云计算、通信等各种资源以及各种灵活的技术方案，对海量不透明的数据进行甄别并转化为有用的信息，以供人们更好的理解世界。该生态圈已经涉及到机器学习、数据挖掘、数据库、信息检索、自然语言处理和语音识别等多个领域。

Spark生态圈以Spark Core为核心，从HDFS、Amazon S3和HBase等持久层读取数据，以MESS、YARN和自身携带的Standalone为资源管理器调度Job完成Spark应用程序的计算。 这些应用程序可以来自于不同的组件，如Spark Shell/Spark Submit的批处理、Spark Streaming的实时处理应用、Spark SQL的即席查询、BlinkDB的权衡查询、MLlib/MLbase的机器学习、GraphX的图处理和SparkR的数学计算等等。


![img](http://images0.cnblogs.com/blog/107289/201508/032218378456765.jpg)
 

- 3.1 Spark Core
- - 提供了有向无环图（DAG）的分布式并行计算框架，并提供Cache机制来支持多次迭代计算或者数据共享，大大减少迭代计算之间读取数据局的开销，这对于需要进行多次迭代的数据挖掘和分析性能有很大提升
-  - 在Spark中引入了RDD (Resilient Distributed Dataset) 的抽象，它是分布在一组节点中的只读对象集合，这些集合是弹性的，如果数据集一部分丢失，则可以根据“血统”对它们进行重建，保证了数据的高容错性；
- -  移动计算而非移动数据，RDD Partition可以就近读取分布式文件系统中的数据块到各个节点内存中进行计算
- - 使用多线程池模型来减少task启动开稍
- - 采用容错的、高可伸缩性的akka作为通讯框架

- 3.2 Spark Streaming

SparkStreaming是一个对实时数据流进行高通量、容错处理的流式处理系统，可以对多种数据源（如Kdfka、Flume、Twitter、Zero和TCP 套接字）进行类似Map、Reduce和Join等复杂操作，并将结果保存到外部文件系统、数据库或应用到实时仪表盘。
Spark Streaming是将流式计算分解成一系列短小的批处理作业。这里的批处理引擎是Spark Core，也就是把Spark Streaming的输入数据按照batch size（如1秒）分成一段一段的数据（Discretized Stream），每一段数据都转换成Spark中的RDD（Resilient Distributed Dataset），然后将Spark Streaming中对DStream的Transformation操作变为针对Spark中对RDD的Transformation操作，将RDD经过操作变成中间结果保存在内存中。整个流式计算根据业务的需求可以对中间的结果进行叠加或者存储到外部设备。下图显示了Spark Streaming的整个流程。

![img](http://images0.cnblogs.com/blog/107289/201508/032218395172222.jpg)

- 3.3 Spark SQL
    Shark是SparkSQL的前身，它发布于3年前，那个时候Hive可以说是SQL on Hadoop的唯一选择，负责将SQL编译成可扩展的MapReduce作业，鉴于Hive的性能以及与Spark的兼容，Shark项目由此而生。

Shark即Hive on Spark，本质上是通过Hive的HQL解析，把HQL翻译成Spark上的RDD操作，然后通过Hive的metadata获取数据库里的表信息，实际HDFS上的数据和文件，会由Shark获取并放到Spark上运算。Shark的最大特性就是快和与Hive的完全兼容，且可以在shell模式下使用rdd2sql()这样的API，把HQL得到的结果集，继续在scala环境下运算，支持自己编写简单的机器学习或简单分析处理函数，对HQL结果进一步分析计算。
Spark SQL允许开发人员直接处理RDD，同时也可查询例如在 Apache Hive上存在的外部数据。Spark SQL的一个重要特点是其能够统一处理关系表和RDD，使得开发人员可以轻松地使用SQL命令进行外部查询，同时进行更复杂的数据分析。除了Spark SQL外，Michael还谈到Catalyst优化框架，它允许Spark SQL自动修改查询方案，使SQL更有效地执行。

![img](http://images0.cnblogs.com/blog/107289/201508/032218485482429.jpg)

- 3.4 SparkR
SparkR是AMPLab发布的一个R开发包，使得R摆脱单机运行的命运，可以作为Spark的job运行在集群上，极大得扩展了R的数据处理能力。

SparkR的几个特性：
1. 提供了Spark中弹性分布式数据集（RDD）的API，用户可以在集群上通过R shell交互性的运行Spark job。
2. 支持序化闭包功能，可以将用户定义函数中所引用到的变量自动序化发送到集群中其他的机器上。
3.  SparkR还可以很容易地调用R开发包，只需要在集群上执行操作前用includePackage读取R开发包就可以了，当然集群上要安装R开发包。

![img](http://images0.cnblogs.com/blog/107289/201508/032219059708673.jpg)