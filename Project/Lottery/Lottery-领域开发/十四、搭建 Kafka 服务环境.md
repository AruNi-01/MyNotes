描述：搭建MQ消息组件Kafka服务环境，并整合到SpringBoot中，完成消息的生产和消费处理

## 一、开发日志

- 搭建 Kafka 环境，配置消息主题 *注意：MQ 消息的使用不非得局限于 Kafka，也可以使用 RocketMq*
- SpringBoot 整合 Kafka，验证消息的生产和消费

## 二、Kafka 安装和配置

Apache Kafka是一个分布式发布 - 订阅消息系统和一个强大的队列，可以处理大量的数据，并使您能够将消息从一个端点传递到另一个端点。 Kafka适合离线和在线消息消费。 Kafka消息保留在磁盘上，并在群集内复制以防止数据丢失。 Kafka构建在ZooKeeper同步服务之上。 它与Apache Storm和Spark非常好地集成，用于实时流式数据分析。

以下是Kafka的几个好处：

- **可靠性** - Kafka是分布式，分区，复制和容错的。
- **可扩展性** - Kafka消息传递系统轻松缩放，无需停机。
- **耐用性** - Kafka使用分布式提交日志，这意味着消息会尽可能快地保留在磁盘上，因此它是持久的。
- **性能** - Kafka对于发布和订阅消息都具有高吞吐量。 即使存储了许多TB的消息，它也保持稳定的性能。

Kafka非常快，并保证零停机和零数据丢失。

### 1. 安装

- 使用版本：kafka_2.13-2.8.0.tgz
- 官网下载：https://kafka.apache.org/downloads
- 网盘下载：https://pan.baidu.com/s/1tNdzkmhr_mZJQvnCcTeeFw 提取码: 5niu
- 教程文档：https://www.w3cschool.cn/apache_kafka/

以 Mac 电脑为例，解压 kafka_2.13-2.8.0.tgz 到自己的目录如下：

```java
fuzhengweideMacBook-Pro: tar -xzvf kafka_2.13-2.8.0.tgz
kafka_2.13-2.8.0
kafka_2.13-2.8.0/LICENSE

fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ ls
LICENSE		bin		libs		logs
NOTICE		config		licenses	site-docs
```

- 安装步骤非常简单，直接解压即可，kafka 依赖的 Zookeeper 已经在此文件中包含

### 2. 配置 Zookeeper

**启动**

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

**关闭**

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/zookeeper-server-stop.sh -daemon config/zookeeper.properties
```

### 3. 配置 Kafka

**启动**

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/kafka-server-start.sh -daemon config/server.properties
```

**关闭**

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/kafka-server-stop.sh -daemon config/server.properties
```

### 4. 创建主题

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic Hello-Kafka
```

- **创建 `Kafka` 主题** - `Kafka` 提供了一个名为 `kafka-topics.sh` 的命令行实用程序，用于在服务器上创建主题。 打开新终端并键入以下示例。

### 5. 查看主题

```java
bin/kafka-topics.sh --list --zookeeper localhost:2181
```

- 由于我们已经创建了一个主题，它将仅列出 `Hello-Kafka` 。 假设，如果创建多个主题，您将在输出中获取主题名称。

### 6. 发送消息

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic Hello-Kafka
```

### 7. 监听消息

```java
fuzhengweideMacBook-Pro:kafka_2.13-2.8.0 fuzhengwei$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic Hello-Kafka --from-beginning
```

## 三、SpringBoot 整合 Kafka

### 1. POM

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

### 2. application.yml 配置 kafka

```java
spring:
  kafka:
    bootstrap-servers: localhost:9092
    producer:
      # 发生错误后，消息重发的次数。
      retries: 1
      #当有多个消息需要被发送到同一个分区时，生产者会把它们放在同一个批次里。该参数指定了一个批次可以使用的内存大小，按照字节数计算。
      batch-size: 16384
      # 设置生产者内存缓冲区的大小。
      buffer-memory: 33554432
      # 键的序列化方式
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      # 值的序列化方式
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
      # acks=0 ： 生产者在成功写入消息之前不会等待任何来自服务器的响应。
      # acks=1 ： 只要集群的首领节点收到消息，生产者就会收到一个来自服务器成功响应。
      # acks=all ：只有当所有参与复制的节点全部收到消息时，生产者才会收到一个来自服务器的成功响应。
      acks: 1
    consumer:
      # 自动提交的时间间隔 在spring boot 2.X 版本中这里采用的是值的类型为Duration 需要符合特定的格式，如1S,1M,2H,5D
      auto-commit-interval: 1S
      # 该属性指定了消费者在读取一个没有偏移量的分区或者偏移量无效的情况下该作何处理：
      # latest（默认值）在偏移量无效的情况下，消费者将从最新的记录开始读取数据（在消费者启动之后生成的记录）
      # earliest ：在偏移量无效的情况下，消费者将从起始位置读取分区的记录
      auto-offset-reset: earliest
      # 是否自动提交偏移量，默认值是true,为了避免出现重复数据和数据丢失，可以把它设置为false,然后手动提交偏移量
      enable-auto-commit: false
      # 键的反序列化方式
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      # 值的反序列化方式
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
    listener:
      # 在侦听器容器中运行的线程数。
      concurrency: 5
      #listner负责ack，每调用一次，就立即commit
      ack-mode: manual_immediate
      missing-topics-fatal: false
```

### 3. 生产者

```java
@Component
public class KafkaProducer {

    private Logger logger = LoggerFactory.getLogger(KafkaProducer.class);

    @Resource
    private KafkaTemplate<String, Object> kafkaTemplate;

    public static final String TOPIC_TEST = "Hello-Kafka";

    public static final String TOPIC_GROUP = "test-consumer-group";

    public void send(Object obj) {
        String obj2String = JSON.toJSONString(obj);
        logger.info("准备发送消息为：{}", obj2String);

        // 发送消息
        ListenableFuture<SendResult<String, Object>> future = kafkaTemplate.send(TOPIC_TEST, obj);
        future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
            @Override
            public void onFailure(Throwable throwable) {
                //发送失败的处理
                logger.info(TOPIC_TEST + " - 生产者 发送消息失败：" + throwable.getMessage());
            }

            @Override
            public void onSuccess(SendResult<String, Object> stringObjectSendResult) {
                //成功的处理
                logger.info(TOPIC_TEST + " - 生产者 发送消息成功：" + stringObjectSendResult.toString());
            }
        });
    }

}
```

### 4. 消费者

```java
@Component
public class KafkaConsumer {

    private Logger logger = LoggerFactory.getLogger(KafkaConsumer.class);

    @KafkaListener(topics = KafkaProducer.TOPIC_TEST, groupId = KafkaProducer.TOPIC_GROUP)
    public void topicTest(ConsumerRecord<?, ?> record, Acknowledgment ack, @Header(KafkaHeaders.RECEIVED_TOPIC) String topic) {
        Optional<?> message = Optional.ofNullable(record.value());
        if (message.isPresent()) {
            Object msg = message.get();
            logger.info("topic_test 消费了： Topic:" + topic + ",Message:" + msg);
            ack.acknowledge();
        }
    }

}
```

## 四、测试验证

测试之前需要开启 Kafka 服务

- 启动 Zookeeper：`bin/zookeeper-server-start.sh -daemon config/zookeeper.properties`
- 启动 Kafka：`bin/kafka-server-start.sh -daemon config/server.properties`

**单元测试**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class KafkaProducerTest {

    private Logger logger = LoggerFactory.getLogger(KafkaProducerTest.class);

    @Resource
    private KafkaProducer kafkaProducer;

    @Test
    public void test_send() throws InterruptedException {
        // 循环发送消息
        while (true) {
            kafkaProducer.send("你好，我是Lottery");
            Thread.sleep(3500);
        }
    }
}
```

- 在单元测试中，我们循环发送 MQ 消息，最后你会在控制台看到消费消息的结果

**测试结果**

```java
13:13:09.781  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:09.783  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:09.786  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:09.790  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:ee
13:13:09.792  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:13.155  INFO 53016 --- [           main] c.i.l.application.mq.KafkaProducer       : 准备发送消息为："你好，我是Lottery"
13:13:13.159  INFO 53016 --- [ad | producer-1] c.i.l.application.mq.KafkaProducer       : Hello-Kafka - 生产者 发送消息成功：SendResult [producerRecord=ProducerRecord(topic=Hello-Kafka, partition=null, headers=RecordHeaders(headers = [], isReadOnly = true), key=null, value=你好，我是Lottery, timestamp=null), recordMetadata=Hello-Kafka-0@22]
13:13:13.161  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:16.660  INFO 53016 --- [           main] c.i.l.application.mq.KafkaProducer       : 准备发送消息为："你好，我是Lottery"
13:13:16.664  INFO 53016 --- [ad | producer-1] c.i.l.application.mq.KafkaProducer       : Hello-Kafka - 生产者 发送消息成功：SendResult [producerRecord=ProducerRecord(topic=Hello-Kafka, partition=null, headers=RecordHeaders(headers = [], isReadOnly = true), key=null, value=你好，我是Lottery, timestamp=null), recordMetadata=Hello-Kafka-0@23]
13:13:16.669  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:20.163  INFO 53016 --- [           main] c.i.l.application.mq.KafkaProducer       : 准备发送消息为："你好，我是Lottery"
13:13:20.167  INFO 53016 --- [ad | producer-1] c.i.l.application.mq.KafkaProducer       : Hello-Kafka - 生产者 发送消息成功：SendResult [producerRecord=ProducerRecord(topic=Hello-Kafka, partition=null, headers=RecordHeaders(headers = [], isReadOnly = true), key=null, value=你好，我是Lottery, timestamp=null), recordMetadata=Hello-Kafka-0@24]
13:13:20.168  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:23.668  INFO 53016 --- [           main] c.i.l.application.mq.KafkaProducer       : 准备发送消息为："你好，我是Lottery"
13:13:23.671  INFO 53016 --- [ad | producer-1] c.i.l.application.mq.KafkaProducer       : Hello-Kafka - 生产者 发送消息成功：SendResult [producerRecord=ProducerRecord(topic=Hello-Kafka, partition=null, headers=RecordHeaders(headers = [], isReadOnly = true), key=null, value=你好，我是Lottery, timestamp=null), recordMetadata=Hello-Kafka-0@25]
13:13:23.673  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
13:13:25.196  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:dd
13:13:27.172  INFO 53016 --- [           main] c.i.l.application.mq.KafkaProducer       : 准备发送消息为："你好，我是Lottery"
13:13:27.177  INFO 53016 --- [ad | producer-1] c.i.l.application.mq.KafkaProducer       : Hello-Kafka - 生产者 发送消息成功：SendResult [producerRecord=ProducerRecord(topic=Hello-Kafka, partition=null, headers=RecordHeaders(headers = [], isReadOnly = true), key=null, value=你好，我是Lottery, timestamp=null), recordMetadata=Hello-Kafka-0@27]
13:13:27.180  INFO 53016 --- [ntainer#0-0-C-1] c.i.l.application.mq.KafkaConsumer       : topic_test 消费了： Topic:Hello-Kafka,Message:你好，我是Lottery
```

------

1. 学习 Kafka 环境配置搭建，也可以把 Kafka 配置到云服务中做集群配置
2. 学习 SpringBoot 与 Kafka 的整合使用，消息的发送和接收处理