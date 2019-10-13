## [Kafka核心技术与实战](https://time.geekbang.org/column/intro/191)

#### 1
消息引擎系统。kafka使用二进制序列化。点对点模型和发布订阅模型。避免流量冲击和不必要的交互。
#### 2 概念
boroker：集群由多个b进程构成。topic：发布、订阅的对象。patrition：实现伸缩性，将topic划分为多个分区，分区有序，每个消息只被一个分区存储。replica：分为leader、follower，数据相同，l提供服务，f不提供。每个topic可m个part，每个part可多个repl。每个repl有一个l，剩下的是f。part可在不同的broker上。

持久化：只能追加写的log文件。日志分段，老日志删除。消费者组：多个消费者组成，同一个消息组内只一个消费者消费，同时消费多个分区。重平衡：消费者组内转移分区。

f为什么不提供服务？part分散在不同broker上，已均衡。消息和offset的一致性问题。

#### 3 消息引擎之外

分布式流处理平台kafka streams。优势：系统内可实现端到端的精确一次处理语义。只提供客户端库不提供运维特性的定位。

分布式存储，持久化存储

#### 4 种类选择

Apache：其他版本的基础

Confluent：商业化工具，跨数据中心备份，注册中心，集群监控

Cloudera：大数据框架

#### 5 版本号

scala编译器。客户端服务端同一版本。

#### 6 线上集群方案

linux优势：io模型（epoll），数据网络效率（零拷贝），社区支持。磁盘。带宽。

#### 7 重要集群配置参数

broker：log.dirs，不管log.dir，故障转移。zk：zookeeper.connect写成a,b,c/name。borker连接：listeners监听组，advertised.listeners对外组，host.name/port过时。topic：自动创建，脏选举，定期重选举。数据留存：消息保存时间，磁盘大小，接收最大消息大小。

8：topic参数：


## [Kafka流处理平台](https://www.imooc.com/learn/1043)

#### 基本概念

数据可持久化

consumer group：对同一个topic，广播给不同group，一个group中只有一个consumer可以消费该消息。

一个topic会被分散存储到多个part。同一part有多个repl。

replmanager：负责管理当前broker所有part和repl信息，处理Kafkacontroller发起的请求，副本状态得切换、添加读取消息等

stream和connectors

#### part

consumer数目小于等于part，避免同一个part被多个consumer消费。

broker group中每个broker保存topic的一个或多个part。part若过大则会跨broker。part不会被重复存储。

consumer group一一对应消费topoic，每个part仅能被一个consumer消费。consumer group可以容错、提高性能。

#### repl

broker失灵，系统将主动使其他repl提供服务。

#### 特点

消息自动平衡，避免部分被访问过多

零拷贝：网络传输持久性日志快。内核空间和用户空间间零拷贝

文件传输到网络的公共数据路径：os将数据从磁盘读入内核空间页缓存。应用将数据从内核空间读入到用户控件缓存。应用将数据写回到内核空间的socket缓存。操作系统将数据从socket缓存复制到网卡缓存。

零拷贝路径：磁盘到内核空间。数据位置和长度描述符增加到socket缓冲区。从内核复制到网卡缓冲区。