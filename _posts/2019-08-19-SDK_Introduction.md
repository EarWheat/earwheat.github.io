---
layout: post
title: SDK Introduction
date: 2019-08-09 
tag: 研发
---

# 远程调用--API、SDK、RPC

##1.API
Thrift最初由Facebook研发，主要用于各个服务之间的RPC通信，支持跨语言，常用的语言比如C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, and OCaml都支持。Thrift是一个典型的CS（客户端/服务端）结构，客户端和服务端可以使用不同的语言开发。
RPC(Remote Procedure Call)，远程过程调用，大部分的RPC框架都遵循如下三个开发步骤：
```
1. 定义一个接口说明文件：描述了对象(结构体)、对象成员、接口方法等一系列信息；
2. 通过RPC框架所提供的编译器，将接口说明文件编译成具体的语言文件；
3. 在客户端和服务器端分别引入RPC编译器所生成的文件，即可像调用本地方法一样调用服务端代码；
```
通信过程如下
![image](https://img-blog.csdn.net/20170207141803075?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGloYW8yMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##2.RPC
![image](https://images2017.cnblogs.com/blog/1167080/201708/1167080-20170815134327975-1466613495.png)

第一部分（Your Code）：

简单总结：开发者的业务逻辑代码

 

第二部分（ServiceClient）

Thrift自动生成的代码,包含Processor、Tserver、ServiceClient

TServer负责接收Client的请求，并将请求转发到Processor进行处理。TServer主要任务就是高效的接受Client的请求，特别是在高并发请求的情况下快速完成请求。

Processor(或者TProcessor)负责对Client的请求做出相应，包括RPC请求转发，调用参数解析和用户逻辑调用，返回值写回等处理步骤。Processor是服务器端从Thrift框架转入用户逻辑的关键流程。Processor同时也负责向Message结构中写入数据或者读出数据。

ServiceClient就是客户端，包含可以调用的请求方法和发送客户端请求

第三部分：

TProtocol主要负责结构化数据组装成Message，或者从Message结构中读出结构化数据。TProtocol将一个有类型的数据转化为字节流以交给TTransport进行传输，或者从TTransport中读取一定长度的字节数据转化为特定类型的数据。如int32会被TBinaryProtocol Encode为一个四字节的字节数据，或者TBinaryProtocol从TTransport中取出四个字节的数据Decode为int32。

 

第四部分：

TTransport负责以字节流方式发送和接收Message，是底层IO模块在Thrift框架中的实现，每一个底层IO模块都会有一个对应TTransport来负责Thrift的字节流(Byte Stream)数据在该IO模块上的传输。例如TSocket对应Socket传输，TFileTransport对应文件传输。

 

第五部分：

底层IO模块，负责实际的数据传输，包括Socket，文件，或者压缩数据流等。

 

通过这个协议栈，可以得出结论，使用Thrift只需要做三件事：

1.通过IDL定义数据结构和服务

2.利用代码生成工具生成代码

3.编写你的业务逻辑

##3.使用Thrift
Step1:编写IDL文件
Step2:使用Thrift命令执行IDL文件
```
thrift --gen java user.thrift
```
Step3:将生成的gen-java引入项目
Step4:编写业务逻辑