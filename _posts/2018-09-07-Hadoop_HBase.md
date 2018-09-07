---
layout: post
title: HBase简介
date: 2018-08-17 
tag: 大数据
---

## HBase


### 简介
HBase是一个分布式的、面向列的开源数据库，该技术来源于 Fay Chang 所撰写的Google论文“Bigtable：一个结构化数据的分布式存储系统”。就像Bigtable利用了Google文件系统（File System）所提供的分布式数据存储一样，HBase在Hadoop之上提供了类似于Bigtable的能力。HBase是Apache的Hadoop项目的子项目。HBase不同于一般的关系数据库，它是一个适合于非结构化数据存储的数据库。另一个不同的是HBase基于列的而不是基于行的模式。

HBase – Hadoop Database，是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统，利用HBase技术可在廉价PC Server上搭建起大规模结构化存储集群。

HBase利用Hadoop HDFS作为其文件存储系统。通过运行MapReduce来处理Bigtable中的海量数据，HBase同样利用Hadoop MapReduce来处理HBase中的海量数据；

### 访问接口
1. Native Java API，最常规和高效的访问方式，适合Hadoop MapReduce Job并行批处理HBase表数据
2. HBase Shell，HBase的命令行工具，最简单的接口，适合HBase管理使用
3. Thrift Gateway，利用Thrift序列化技术，支持C++，PHP，Python等多种语言，适合其他异构系统在线访问HBase表数据
4. REST Gateway，支持REST 风格的Http API访问HBase, 解除了语言限制
5. Pig，可以使用Pig Latin流式编程语言来操作HBase中的数据，和Hive类似，本质最终也是编译成MapReduce Job来处理HBase表数据，适合做数据统计
6. Hive，当前Hive的Release版本尚没有加入对HBase的支持，但在下一个版本Hive 0.7.0中将会支持HBase，可以使用类似SQL语言来访问HBase


### 数据模型
HBase数据模型Table & Column Family

1、行键Row Key：主键是用来检索记录的主键，访问hbasetable中的行。

 

2、列族Column Family：Table在水平方向有一个或者多个ColumnFamily组成，一个ColumnFamily中可以由任意多个Column组成，即ColumnFamily支持动态扩展，无需预先定义Column的数量以及类型，所有Column均以二进制格式存储，用户需要自行进行类型转换。

 

3、列column：由Hbase中的列族ColumnFamily + 列的名称（cell）组成列。

 

4、单元格cell：HBase中通过row和columns确定的为一个存贮单元称为cell。

 

5、版本version：每个 cell都保存着同一份数据的多个版本。版本通过时间戳来索引。

## Hbase shell操作
1、status:查询服务器状态：


```
//status

hbase(main):005:0* status
1 active master, 0 backup masters, 1 servers, 0 dead, 3.0000 average load

```

2、create:创建数据库表，创建命令可看帮助help 'create'
语法：create <table>, {NAME =><family>, VERSIONS => <VERSIONS>}

例示：create 'product',{NAME => 'computer', VERSIONS => 5},{ NAME => 'food' , VERSIONS => 3}

描述：创建一张名叫'product'数据库表，并且创建两个列族，分别为：'computer'、'food'



```
hbase(main):007:0> create 'product',{NAME => 'computer', VERSIONS => 5},{ NAME => 'food' , VERSIONS => 3}
0 row(s) in 0.7770 seconds

=> Hbase::Table - product
```

3.list:查看已经存在的表


```
hbase(main):008:0> list
TABLE                                                                           
product                                                                         
1 row(s) in 0.0060 seconds

=> ["product"]
```

4、describe:查看表结构描述


```
hbase(main):009:0> describe 'product'
Table product is ENABLED                                                        
product                                                                         
COLUMN FAMILIES DESCRIPTION                                                     
{NAME => 'computer', BLOOMFILTER => 'ROW', VERSIONS => '5', IN_MEMORY => 'false'
, KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER'
, COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE =>
 '65536', REPLICATION_SCOPE => '0'}                                             
{NAME => 'food', BLOOMFILTER => 'ROW', VERSIONS => '3', IN_MEMORY => 'false', KE
EP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', CO
MPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65
536', REPLICATION_SCOPE => '0'}                                                 
2 row(s) in 0.0450 seconds

```

5、alter:修改表

用法：修改表结构必须先disable，再修改表，修改完成后，再enable表。

disable 'product'

alter 'product',{NAME => 'food',VERSIONS=> 3}

enable 'product'

   

 

6、drop:删除表

用法：首先disable，然后drop。

disable 'product'

drop 'product'

7、put:插入数据

语法：put <table>,<rowkey>,<family:column>,<value>,<timestamp>

例示：put 'product','rowkey001',  'computer:name', 'ThinkPad E550'

再次插入数据：put'product', 'rowkey001',  'computer:price', 4199

描述：给表'product'的添加一行记录：rowkey是'rowkey001'，family name：'computer'，column name：'name'，value：'ThinkPad E550'，timestamp：系统默认


8、get:查询数据

   语法：get<table>,<rowkey>,[<family:column>,....]

   查询表product，rowkey001中的computer下的cell的值

   例示：get 'product', 'rowkey001'
   
   
9、scan:扫描表

   语法：scan <table>,{COLUMNS => [ <family:column>,.... ], LIMIT => num}

    Hbase存储数据是以二进制形式保存的，读取是没有问题的

   例示：scan 'product'