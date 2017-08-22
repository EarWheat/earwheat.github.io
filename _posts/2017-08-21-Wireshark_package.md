---
layout: post
title: 计算机网络流量之捕获分析数据包
date: 2017-08-21
tag: 计算网络
---

# 计算机网络流量之捕获分析数据包

---
工具：Wireshark

操作系统:Ubuntu16.04

---

此篇博客针对分析网络流量捕获的数据包，Wireshark的下载安装请自行百度。

## 1.抓包
选择一个接口准备开始抓取数据包，如下图：
![image](http://img.blog.csdn.net/20170821172010377?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 2.数据包详解
 在Wireshark中关于数据包的叫法有三个术语，分别是帧、包、段。下面通过分析一个数据包，来介绍这三个术语。在Wireshark中捕获的一个数据包，如图：
 ![image](http://img.blog.csdn.net/20150716143332537?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
 接下来咱们看一个HTTP的数据包来进一步认识分析数据包。
 ![image](http://img.blog.csdn.net/20170821221108713?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 wireshark一共有三个小窗口，从上到下分别是①抓取到的数据包分组②某个数据包的各层协议③包的具体信息（十六进制）

 咱们把注意力放到第二个窗口上（中间窗口）。
-  Frame：物理层的数据帧概况。
-  Ethernet II：数据链路层以太网帧头部信息。
-  Internet Protocol Version 4：互联网层IP包头部信息。
-  Transmission Control Protocol：传输层的数据段头部信息，此处是TCP协议。
-  Hypertext Transfer Protocol：应用层的信息，此处是HTTP协议。

另附上一层OSI七层模型与TCP/IP协议的五层模型对比图便于理解。![image](http://www.ha97.com/wp-content/uploads/image/2010/09/162558wtA.jpg)![image](http://www.ha97.com/wp-content/uploads/image/2010/09/162603uh3.gif)
## 各层次模型详解
### 1.物理层数据帧概况


字段 | 意义
---|---
Frame 190: 364 bytes on wire (2912 bits), 364 bytes captured (2912 bits) on interface 0   | 190号帧，2912个字，364字节，实际捕获364字节（2912bits）
Interface id ：0 (any）   | 接口的id号
Encapsulation type： Linux cooked-mode capture (25)   | 封装类型
Arrival Time Aug 22, 2017 22:07:42.167911635 CST   | 捕获时间
[Time shift for this packet: 0.000000000 seconds]   |
Epoch Time: 1503410862.167911635 seconds   | 
[Time delta from previous captured frame: 0.000097773 seconds]   | 此包与前一包的时间间隔
[Time delta from previous displayed frame: 0.000000000 seconds]   | 此包与前一显示帧的时间间隔
[Time since reference or first frame: 13.524021107 seconds]   | 此包与第一帧的时间间隔
Frame Number: 190   | 帧序号
Frame Length: 364 bytes (2912 bits)   | 帧长度
Capture Length: 364 bytes (2912 bits)   | 捕获长度
[Frame is marked: False]   | 此帧是否做了标记：否
[Frame is ignored: False]  | 此帧是否被忽略：否
[Protocols in frame: sll:ethertype:ip:tcp:http]   | 帧内封装的协议层次结构
[Coloring Rule Name: HTTP]   | 着色标记的协议名称
[Coloring Rule String: http......]   | 着色规则显示的字符串

### 2.数据链路层以太网帧头部信息

字段 | 意义
---|---
Ethernet II, Src: Giga-Byt_c8:4c:89(1c:6f:65:c8:4c:89),Dst: Tp-LinkT_f9:3c:c0 (6c:e8:73:f9:3c:c0)   | 
Destination: Tp-LinkT_f9:3c:c0 (6c:e8:73:f9:3c:c0)  | 目的MAC地址
Source: Giga-Byt_c8:4c:89 (1c:6f:65:c8:4c:89)  | 源MAC地址
Type: IP (0x0800)  |
### 3. 互联网层IP包头部信息 

字段 | 意义
---|---
Internet Protocol Version 4, Src: 192.168.0.104 (192.168.0.104), Dst: 61.182.140.146 (61.182.140.146)  | 
Version: 4   | 互联网协议IPv4
Header length: 20 bytes   | IP包头部长度
Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00: Not-ECT (Not ECN-Capable Transport))    | 差分服务字段
Total Length: 254   | IP包的总长度
Identification: 0x5bb5 (23477)   | 标志字段
Flags: 0x02 (Don't Fragment)   | 标记字段
Fragment offset: 0   | 分的偏移量
Time to live: 64    | 生存期TTL
Protocol: TCP (6)   | 此包内封装的上层协议为TCP
Header checksum: 0x52ec [validation disabled]                                               | 头部数据的校验和
Source: 192.168.0.104 (192.168.0.104)   | 源IP地址
Destination: 61.182.140.146 (61.182.140.146)   | 目标IP地址
### 4. 传输层TCP数据段头部信息

字段 | 意义
---|---
Transmission Control Protocol, Src Port: 51833 (51833), Dst Port: http (80), Seq: 1, Ack: 1, Len: 214 |
Source port: 51833 (51833)                                                                                 | 源端口号
Destination port: http (80)                                                                       | 目标端口号
Sequence number: 1    (relative sequence number)                                  | 序列号（相对序列号）
[Next sequence number: 215    (relative sequence number)]       | 下一个序列号
Acknowledgment number: 1    (relative ack number)                        | 确认序列号
Header length: 20 bytes                                                                              | 头部长度
Flags: 0x018 (PSH, ACK)                                                                     | TCP标记字段
Window size value: 64800                                                                                | 流量控制的窗口大小
Checksum: 0x677e [validation disabled]                                                  | TCP数据段的校验和

