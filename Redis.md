## [Redis深度历险](https://book.douban.com/subject/30386804/)

#### 概念

远程，基于内存，非关系数据库

缓存，队列，数据存储

#### 数据类型

string，list，set，hash，sortset。以整个对象为单位，所有数据结构都可以设置过期时间。字符串重置字符串后，过期时间将消失。

hash：一个应用场景是存储用户信息，相较于string得序列化，可以实现取部分字段

sortset：分数，值，排名。value必须全局唯一，分数相同则字典排序

#### 应用

分布式锁set if not exist

* 过期参数
* 超时问题，随机数检查。lua脚本最优
* 多重锁。麻烦

单组消费者的简单延时队列

* 轮询，阻塞读写（超时问题）
* zset实现延时队列解决锁冲突
* zrem移除，保证仅有一个消费者消费

位图

* 计数和第一个出现
* bitfield取部分位，子命令incrby范围自增

hyperloglog用于UV去重

* pfadd、pfcount、pfmerge

布隆过滤器

* 设置错误率，估计空间

简单限流

* 用户再一定时间内的某种操作的次数，zset实现滑动窗口计数

漏斗限流

* 流水速率，上一次漏水时间，无法保证原子性
* redis-cell限流模块，cl.throttle

Geohash

* 映射一维
* 即为set结构
* geoadd，geodist，geopos，geohash，georadiusbymember，georadius
* 集群环境中zset单个数据量不宜超过1mb。建议geo使用单独的redis实例

scan和key保存方式

* 找特定前缀的key，keys。O（n），会卡顿崩溃
* scan，有limit，可匹配，不需要保存游标状态
* 结果可能有重复
* 遍历过程中的数据修改不一定能遍历到
* key存放在hashtable结构中
* 槽，高位进位加法遍历，扩容缩容时的合理顺序，可能重复的原因
* 渐进式rehash，新旧同时存在，需要都扫描
* scan还可扫描其他容器集合
* 大key影响应能，官方自带扫描

#### 原理

线程IO模型

* 非阻塞IO：不等待立即写完返回
* 事件轮询：由操作系统返回可用的读写符[redis封装多路IO复用](https://mp.weixin.qq.com/s/ApDUI3uWJIUtHcpjJzI_6w?)
* 指令队列：每个客户端套接字关联一个指令队列
* 响应队列：同上。队列为空时，将写符从待检查符中移出，避免不需要写的也加入轮询。
* 定时任务：定时任务被记录在最小堆上。每次循环执行到点任务，将下一个任务的时间差作为睡眠时间。nginx和node事件处理类似。

通信协议

* RESP：直观的文本协议

持久化

* 快照rdb和指令日志aof
* rdb原理：避免阻塞业务，使用cow（copy on write）机制实现rdb持久化
* fork：glibc的fork产生一个子进程。产生时和父进程共享代码段和数据段。子进程持久化过程中，不会修改现有的内存数据，只读出写到磁盘。cow在父进程修改时，将复制分离数据段页面，对副本修改。
* aof：先执行指令，再将日志存盘。
* 瘦身指令bgrewriteaof：将内存中的对象遍历生成指令，再将操作期间增量写入结尾。
* fsyno：内存缓存手动写入磁盘，周期性执行
* 运维：rdb和aof都耗资源，所以主节点不进行持久化操作，
* 混合持久化

管道

* 压力测试
* 改变了读写顺序

事务

* multi、exec、discard
* 事务执行失败后会继续执行，不会回滚
* 事务+管道
* watch乐观锁，仅在关键变量变化时失败

pubsub

* 阻塞消费者
* 模式匹配订阅
* 缺点：断线期间不会重发，消息不会持久化
* stream是持久化的消息队列

小对象压缩

* 32位优化
* ziplist，intset。会自动升级
* 内存不会立即被回收，按页回收
* 页内分配算法

#### 集群

主从同步

* 异步同步，不满足C
* 主从同步，从从同步
* 环形数组增量同步，快照同步
* 主节点无盘复制发送快照
* wait命令保证C

sentinel选举主节点

* 限制主从延迟的参数

codis集群

* hash后取余实现
* 通过zookeeper向codis proxy同步槽位关系
* 扩容时，通过改造后新增的slotsscan命令，移动槽位的key。若此时有请求，则先迁出该key，再发送请求
* 自动均衡slot
* 代价：不同实例间不可事务和rename等，单个key的value不宜过大，网络开销略大
* 优点：比redis cluster简单，因为将一致性交由zookeeper或etcd
* mget执行过程：mapreduce

cluster集群

* slot信息存储在每个节点中，客户端可以直接定位，客户端slot信息缓存校验，节点将集群配置持久化
* slot定位：取余，允许强制将key挂载到特定slot
* 错误访问跳转指引
* 迁移：序列化，迁移，反序列化，返回OK，删除。迁移过程是同步的。
* 容错、网络抖动
* 可能下线和确定下线
* slot迁移的请求过程
* 集群变更感知的过程

#### 拓展

stream持久化消息队列

* 竞争消费组内消息
* 消费者数据结构pel正处理消息
* 独立消费

info指令

* Server 服务器运行的环境参数
* Clients 客户端相关信息
* Memory 服务器运行内存统计数据
* Persistence 持久化信息
* Stats 通用统计数据
* Replication 主从复制相关信息
* CPU CPU 使用情况
* Cluster 集群信息
* KeySpace 键值对统计数量信息

redlock分布式锁

* 加锁未释放时主从切换
* redlock向多实例加锁，投票决定

过期策略

* 避免回收带来的卡顿
* 有过期时间的key会放到字典中，定时遍历，惰性删除
* 定时扫描策略：随机选，删除过期，若比例过则再选。时间上限25ms。key集中过期风险大。增加随机时间避免集中过期。
* 从节点通过aof被动过期

LRU

* 内存不足策略
* 不继续服务写请求、过期时间lru、过期时间ttl、过期时间随机、lru、全随机
* 懒惰处理，随机采样

阅读golanglru源码

懒惰删除

* redis有几个用于处理耗时操作的异步线程
* 大key删除，unlink
* 清空数据库 flushall async
* 以上使用线程安全的异步队列
* aof sync

jedis

* try-with-resource避免未释放资源回连接池
* 封装上一条，使用回调类传入逻辑
* lambda表达式简化
* holder类包装绕过对变量的限制
* catch中增加重试功能

保护

* 指令重命名
* 指定客户ip设置密码
* lua脚本攻击
* ssl spiped

#### 源码

字符串

* 带长度信息的字节数组
* cap和len机制
* emb和raw：44分隔，与头结构共同占用一个64B的最小内存
* 对象共有头结构：type、encoding、lru、refcount、ptr
* 类java：1mb下翻倍，1mb上加1mb

字典

* 渐进和定时任务rehash
* 扩容缩容策略

压缩列表ziplist

* 定长区域保存结构体，结构体针对内容有优化
* 对结构体内容修改可能导致连锁更新，因为长度由下一个结构记录
* intset实现少整数set

快速列表qucklist

* qucklist双向链表，用于替代导致碎片化的双向链表

跳表

* 层数随机
* 更新值将先删除再插入
* score值相等时将比较value值：便于相等时的查询
* 指针后继中有span跨度属性，rank值由此算出

紧凑列表listpack

* 类似ziplist
* 无最后元素位置，通过总字节和后置的最后字节长度计算出
* 长度不仅为1或5，而是1到5，类utf8
* 类别内容本身可以实现小数据存储
* 无须级联更新。
* 尚仅在stream中使用

基数树rax
* cluster中存储slot和key的关系

LFU&LRU
* LRU:通过对时间戳取模算折返得出其空闲时间
* LFU:未看懂

懒惰删除的变化
* 单线程：类似scan，需要控制速度防止内存占用或阻塞服务，服务繁忙时性能下降严重
* 单独回收线程：需要修改原有的对象共享机制。共享机制还存在吗？

字典遍历
* 允许rehash
* 未完全看懂
