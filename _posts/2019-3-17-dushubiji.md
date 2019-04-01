---
layout: post
title: "大型网站系统与Java中间件实践"
date: 2019-3-17
tag: 摘要 
---

[TOC]

### 分布式系统

#### 定义：

1. 组件分布在网络计算机上
2. 组件之间仅仅通过消息传递通信并协调行动

#### 网络IO实现的方式：

1. BIO方式：阻塞的方式实现，一个线程处理一个socket
2. NIO方式：非阻塞的方式实现，基于事件驱动思想，采用reactor模式
   1. Reactor会管理所有的handler
   2. 通过Reactor对所有的客户端的socket事件处理，然后派发到不同的线程
   3. ![Reactor模式在通信中的应用](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/%E5%A4%A7%E5%9E%8B%E7%BD%91%E7%AB%99%E6%9E%B6%E6%9E%84/1.jpg)
3. AIO方式
   1. 异步IO，采用Proactor模式。
   2. 在动作发生的时候发出通知，比如在进行读/写操作时，只需要调用相应的read/write方法，并传入需要的CompletionHandler，动作完成之后，会调用CompletionHandler
   3. NIO的通知发生在动作之前，选择器发现这些是件后调用handler处理

#### 分布式系统的难点

1. 缺乏全局时钟
2. 面对故障独立性
3. 处理单点故障
4. 事务的挑战

### 大型网站架构演进过程

#### 应用服务器负载，走向集群

当增加服务器应用之后出现了几个问题：

1. 服务器访问的问题。
   1. 通过DNS解决
   2. 增加负载均衡设备，如：nginx
2. session问题

##### 负载均衡

在引用了负载均衡之后，在浏览器和服务器之间加了一层保护屏障——负载均衡器

![负载均衡结构](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/%E5%A4%A7%E5%9E%8B%E7%BD%91%E7%AB%99%E6%9E%B6%E6%9E%84/2.jpg)

##### session问题

http协议的无状态，所以在每次会话时需要携带会话标识，在集群中无法保障每次请求都在一台机器上，所以会导致无法识别会话

1. Session Stricky

   保证同一个会话请求在同一个服务器处理，需要负载均衡器根据会话标识进行请求转发

   - 如果有一台机器坏掉，所有数据丢失


- 会话标识是应用层信息，开销比较大


- 负载均衡器变成有状态的节点，内存消耗大

1. Session Replication

   在服务器之间增加了会话数据同步，保证服务器之间session的一致性。通过应用容器来完成session复制

   - 同步数据造成网络带宽的开销
   - 每台机器用于保存session数据占用严重

2. Session数据集中存储

   将session数据集中存储起来，不同的web服务器从同样的地方获取session。可以使用数据库或者其他的分布式存储系统存储sessioin

   - 读写session存在时延和不稳定性
   - 如果存储session的机器坏掉会影响使用应用

3. Cookie

   通过cookie携带session数据

   - cookie长度的限制
   - 安全性
   - 消耗带宽
   - 性能变低

#### 数据库读写分离

增加一个读库来分担数据库的压力。mysql中支持master+slave结构，提供异步数据复制机制，会有延迟，提供完全镜像方式的复制，保证数据一致性

#### 数据库拆表

##### 垂直拆分

把数据库中的不同业务拆分到不同的数据库中

##### 水平拆分

把一个表中的数据拆分到两个数据库中

**缓存**

1. 数据缓存

   使用缓存可以减轻数据库的读压力，像可以使用redis作为缓存数据库。数据缓存可以加速应用在相应请求时的数据读取速度

2. 页面缓存

   ESI，将处理放在apache中进行。apache对服务器返回的响应结果进行处理，找到ESI标签，去缓存中获取标签对应的数据

#### 引入分布式存储系统

分布式存储系统提供了一个高容量、高并发访问、数据冗余容灾的支持

#### 拆分应用

- 根据业务特性把应用拆分

最终分布式系统的大体架构如下图所示：

![分布式系统架构](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/%E5%A4%A7%E5%9E%8B%E7%BD%91%E7%AB%99%E6%9E%B6%E6%9E%84/3.jpg)

### Java中间件

中间件：为软件应用提供了操作系统所提供的服务之外的服务

这里就不自己画图了，引书上的图：

![引入中间件的结构图](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/%E5%A4%A7%E5%9E%8B%E7%BD%91%E7%AB%99%E6%9E%B6%E6%9E%84/4.jpg)

如图所示，在webapp和service之间，通过服务框架解决了集群之间的通信问题；在应用和数据库之间，通过分布式数据层可以让应用更方便的访问已经被分库分表的数据库节点；数据复制/迁移帮助我们更好的根据业务需求完成数据的分布。

### 服务框架

### 消息中间件

JMS：JavaEE中关于消息的规范。

消息中间件的特点：异步和解耦

#### 如何保证消息发送的一致性

一致性是指产生消息的业务动作与消息发送的一致

#### JMS如何保证消息的一致性

Queue模型和Topic模型

JMS几个重要元素的关系：

ConnectionFactory——》Connection——》Session——》Message

Destination+Session——》MessageProducer

Destination+Session——》MessageConsumer

JMS中引入了XA系列的接口，保证了分布式事务的支持。  

![发送消息的正向流程](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/%E5%A4%A7%E5%9E%8B%E7%BD%91%E7%AB%99%E6%9E%B6%E6%9E%84/5.jpg)

针对异常情况可能遇到的三种状态：

1. 业务操作未进行，消息未存入
2. 业务操作未进行，消息存储未待处理
3. 业务操作成功，消息存储，状态未待处理

可以看出，只需要了解业务操作结果，就可以保证一致性

![检查业务的反向流程](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/%E5%A4%A7%E5%9E%8B%E7%BD%91%E7%AB%99%E6%9E%B6%E6%9E%84/6.jpg)

解决业务操作与发送消息的一致性的解决方案就是发送消息的正向流程与检查业务的反向流程结合

- JMS Queue模型：是点对点的，消息在queue上是所有应用共享的，一个应用消费废掉了其他应用就收不到了
- JMS Topic模型：是发布/订阅，在接收消息的时候，每个应用可以独立收到所有到达Topic的消息


- 持久订阅：订阅关系一旦建立，除非显式的取消订阅，否则订阅关系一直存在
  - 消息发送端的可靠性：发送者需要准确将消息发送给应用，消息中间件及时明确的给出回复
  - 消息存储的可靠性
    - 基于文件的消息存储
    - 采用数据库作为消息存储 
    - 基于双机内存的消息存储
- 非持久订阅：每次消息接收者启动就会重新建立订阅关系

#### 消息系统的扩容

