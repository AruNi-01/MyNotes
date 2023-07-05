# 一、Kafka 概述

## 1.1 定义

**Kafka 传统定义**：Kafka 是一个 **分布式** 的基于 **发布/订阅模式** 的消息队列(Message Queue)，主要应用于大数据实时处理领域。

**发布/订阅**：消息的发布者不会将消息直接发送给特定的订阅者，而是 **将发布的消息分为不同的类别**，订阅者 **只接收感兴趣的消息**。

**Kafka 最新定义**：Kafka 是一个开源的 **分布式事件流平台**(Event Streaming Platform)，被数千家公司用于高性能数据管道、流分析、数据集成和关键任务应用。

## 1.2 MQ 应用场景

传统的消息队列的主要应用场景包括：**缓存/削峰、解耦和异步通信**。

**缓冲/削峰**：有助于控制和优化数据流经过系统的速度，**解决生产消息和消费消息的处理速度不一致的情况**。

![image-20230617125549360](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306171255372.png)

**解耦**：**允许你独立的扩展或修改两边的处理过程**，只要确保它们遵守同样的接口约束。

![image-20230617125638019](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306171256972.png)

**异步通信**：允许用户把一个消息放入队列，但并不立即处理它，然后在需要的时候再去处理它们，以 **增加响应速度**。

![image-20230617125724924](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306171257387.png)

## 1.3 MQ 的两种模式

**1、点对点模式**：

- **消费者** 主动拉取数据，**消息收到后清除消息**

![image-20230617125927830](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306171259126.png)

**2、发布/订阅模式**：

- 可以有多个 topic 主题(浏览、点赞、收藏、评论等) 
- **消费者消费数据之后，不删除数据。每个消费者都可以消费到数据**

![image-20230617130142977](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306171301030.png)

## 1.4 Kafka 基础架构

Kafka 基础架构的演进：

- 为方便扩展，并提高吞吐量，**一个 topic 分为多个 partition** 
- 配合分区的设计，提出 **消费者组** 的概念，**组内每个消费者并行消费**
- 为提高可用性，**为每个 partition 增加若干副本**
- **ZK 中记录谁是 leader**，Kafka2.8.0 以后也可以配置不采用 ZK

![image-20230617130658294](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306171306464.png)

- **Producer**：消息生产者，就是向 Kafka broker 发消息的客户端。 
- **Consumer**：消息消费者，向 Kafka broker 取消息的客户端。
- **Consumer Group**(**CG**)：消费者组，由多个 consumer 组成。**消费者组内每个消费者负责消费不同分区的数据，一个分区只能由一个 组内的消费者 进行消费；消费者组之间互不影响**。所有的消费者都属于某个消费者组，即 **消费者组是逻辑上的一个订阅者**。
- **Broker**：**一台 Kafka 服务器就是一个 broker**。一个集群由多个 broker 组成。**一个 broker 可以容纳多个 topic**。
- **Topic**：可以理解为 **一个队列**，生产者和消费者面向的都是一个 topic。
- **Partition**：为了实现 **高吞吐和高扩展**：
  - **一个非常大的 topic 可以分布到多个 broker(即服务器)上，增加了可扩展性**；
  - **一个 topic 可以分为多个 partition，使得生产者具有并行写入的能力，消费者具有并行消费的能力**；
  - **每个 partition 都是一个有序的队列**。
- **Replica**：副本。一个 topic 的 **每个 partition 都有若干个副本**，一个 Leader 和若干个 Follower。
- **Leader**：每个分区多个副本的 “主”，**生产者发送数据的对象，以及消费者消费数据的对象都是 Leader**。
- **Follower**：每个分区多个副本中的 “从”，实时从 Leader 中同步数据，保持和 Leader 数据的同步。**Leader 发生故障时，某个 Follower 会成为新的 Leader**。



# 二、Kafka 快速入门











