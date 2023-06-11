## 1. 开发前准备

描述：使用MQ消息的特性，把用户抽奖到发货到流程进行解耦。这个过程中包括了消息的发送、库表中状态的更新、消息的接收消费、发奖状态的处理等。

在数据库表 `user_strategy_export` 添加字段 `mq_state` 这个字段用于发送 MQ 成功更新库表状态，如果 MQ 消息发送失败则需要通过定时任务补偿 MQ 消息。PS：你可以使用本章节分支下的 sql 更新自己的库表。

启动 kafka 新增 topic：lottery_invoice 用于发货单消息，**当抽奖完成后则发送一个发货单，再异步处理发货流程**，这个部分就是MQ的解耦流程使用。

在 `ActivityProcessImpl#doDrawProcess` 活动抽奖流程编排中补全用户抽奖后，发送 MQ 触达异步奖品发送的流程。

## 2. 创建主题（Topic）

在开发前我们先启动 Kafka 并新增加一个 MQ 的 Topic，命令语句如下：

```sh
# 启动zk：
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
#启动kafka：
bin/kafka-server-start.sh -daemon config/server.properties
#创建topic：
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic lottery_invoice
```

## 3. 流程

本章节开发的MQ流程：

![image-20230508210237936](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305082102310.png)

- 从用户发起抽奖到中奖后开始，就是MQ处理发奖的流程

- 因为 MQ 消息的发送是不具备事务性的，也就是说你在发送 MQ 时可能会失败，哪怕成功率是3个9到4个9，也会有一定的概率触发发送失败。所以我们在 MQ 发送完成后需要知道是否发送成功，进行库表状态更新，如果发送失败则需要使用 worker 来补偿 MQ 发送。PS：这部分内容我们会在后续章节实现。

- 最后 MQ 发送完成到消费，也是可能有失败的，比如处理失败、更新库表失败等，但无论是什么失败都需要保证 MQ 进行重试处理。而保证 MQ 消息重试的前提就是服务的幂等性，否则你在重试的过程中就造成了流程异常，比如更新次数多了、数据库插入多了、给用户发奖多了等等，尤其是发生资损是更可怕的。

## 4. MQ 服务

关于 MQ 的使用，无论是 Kafka 还是 RocketMQ，基本方式都是类似的，一个生产消息，一个监听消息，只不过在一些符合各自业务场景下，做了细分的优化，但这并不会影响你的使用。

### 4.1 生产消息

```java
@Component
public class KafkaProducer {

    private Logger logger = LoggerFactory.getLogger(KafkaProducer.class);

    @Resource
    private KafkaTemplate<String, Object> kafkaTemplate;

    /**
     * MQ主题：中奖发货单
     */
    public static final String TOPIC_INVOICE = "lottery_invoice";

    /**
     * 发送中奖物品发货单消息
     *
     * @param invoice 发货单
     */
    public ListenableFuture<SendResult<String, Object>> sendLotteryInvoice(InvoiceVO invoice) {
        String objJson = JSON.toJSONString(invoice);
        logger.info("发送MQ消息 topic：{} bizId：{} message：{}", TOPIC_INVOICE, invoice.getuId(), objJson);
        return kafkaTemplate.send(TOPIC_INVOICE, objJson);
    }

}
```

- 我们会把所有的生产消息都放到 KafkaProducer 中，并对外提供一个可以发送 MQ 消息的方法。
- 因为我们配置的类型转换为 StringDeserializer 所以发送消息的方式是 JSON 字符串，当然这个编解码器是可以重写的，满足你发送其他类型的数据。

### 4.2 消费消息

```java
@Component
public class LotteryInvoiceListener {

    private Logger logger = LoggerFactory.getLogger(LotteryInvoiceListener.class);

    @Resource
    private DistributionGoodsFactory distributionGoodsFactory;

    @KafkaListener(topics = "lottery_invoice", groupId = "lottery")
    public void onMessage(ConsumerRecord<?, ?> record, Acknowledgment ack, @Header(KafkaHeaders.RECEIVED_TOPIC) String topic) {
        Optional<?> message = Optional.ofNullable(record.value());

        // 1. 判断消息是否存在
        if (!message.isPresent()) {
            return;
        }

        // 2. 处理 MQ 消息
        try {
            // 1. 转化对象（或者你也可以重写Serializer<T>）
            InvoiceVO invoiceVO = JSON.parseObject((String) message.get(), InvoiceVO.class);

            // 2. 获取发送奖品工厂，执行发奖
            IDistributionGoods distributionGoodsService = distributionGoodsFactory.getDistributionGoodsService(invoiceVO.getAwardType());
            DistributionRes distributionRes = distributionGoodsService.doDistribution(new GoodsReq(invoiceVO.getuId(), invoiceVO.getOrderId(), invoiceVO.getAwardId(), invoiceVO.getAwardName(), invoiceVO.getAwardContent()));

            Assert.isTrue(Constants.AwardState.SUCCESS.getCode().equals(distributionRes.getCode()), distributionRes.getInfo());

            // 3. 打印日志
            logger.info("消费MQ消息，完成 topic：{} bizId：{} 发奖结果：{}", topic, invoiceVO.getuId(), JSON.toJSONString(distributionRes));

            // 4. 消息消费完成
            ack.acknowledge();
        } catch (Exception e) {
            // 发奖环节失败，消息重试。所有到环节，发货、更新库，都需要保证幂等。
            logger.error("消费MQ消息，失败 topic：{} message：{}", topic, message.get());
            throw e;
        }
    }

}
```

- 每一个 MQ 消息的消费都会有一个对应的 XxxListener 来处理消息体，如果你使用一些其他的 MQ 可能还会看到一些抽象类来处理 MQ 消息集合。
- 在这个 LotteryInvoiceListener 消息监听类中，主要就是通过消息中的发奖类型获取到对应的奖品发货工厂，处理奖品的发送操作。

**注意**：在奖品发送操作中，记得补全 `DistributionBase#updateUserAwardState` 更新奖品发送状态的操作。

## 5. 抽奖流程解耦

```java
public DrawProcessResult doDrawProcess(DrawProcessReq req) {
    // 1. 领取活动
    
    // 2. 执行抽奖

    // 3. 结果落库

    // 4. 发送MQ，触发发奖流程
    InvoiceVO invoiceVO = buildInvoiceVO(drawOrderVO);
    ListenableFuture<SendResult<String, Object>> future = kafkaProducer.sendLotteryInvoice(invoiceVO);
    future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
        @Override
        public void onSuccess(SendResult<String, Object> stringObjectSendResult) {
            // 4.1 MQ 消息发送完成，更新数据库表 user_strategy_export.mq_state = 1
            activityPartake.updateInvoiceMqState(invoiceVO.getuId(), invoiceVO.getOrderId(), Constants.MQState.COMPLETE.getCode());
        }
        @Override
        public void onFailure(Throwable throwable) {
            // 4.2 MQ 消息发送失败，更新数据库表 user_strategy_export.mq_state = 2 【等待定时任务扫码补偿MQ消息】
            activityPartake.updateInvoiceMqState(invoiceVO.getuId(), invoiceVO.getOrderId(), Constants.MQState.FAIL.getCode());
        }
    });

    // 5. 返回结果
    return new DrawProcessResult(Constants.ResponseCode.SUCCESS.getCode(), Constants.ResponseCode.SUCCESS.getInfo(), drawAwardVO);
}
```

- 在 ActivityProcessImpl#doDrawProcess 方法中，主要补全的就是关于 MQ 的处理，这里我们会调动 kafkaProducer.sendLotteryInvoice 发送一个中奖结果的发货单。
- 消息发送完毕后进行回调处理，更新数据库中 MQ 发送的状态，如果有 MQ 发送失败则更新数据库 mq_state = 2 **这里还有可能在更新库表状态的时候失败**，但没关系这些都会被 worker 补偿处理掉，一种是发送 MQ 失败，另外一种是 MQ 状态为 0 但很久都没有发送 MQ 那么也可以触发发送。
- 现在 **从用户领取活动、执行抽奖、结果落库，到 发送MQ处理后续发奖的流程就解耦了**，因为用户只需要知道自己中奖了，但发奖到货是可以等待的，毕竟发送虚拟商品的等待时间并不会很长，而实物商品走物流就更可以接收了。所以对于这样的流程进行解耦是非常有必要的，否则你的程序逻辑会让用户在界面等待更久的时间。

## 6. 测试

测试之前需要开启 Kafka 服务

- 启动 Zookeeper：`bin/zookeeper-server-start.sh -daemon config/zookeeper.properties`
- 启动 Kafka：`bin/kafka-server-start.sh -daemon config/server.properties`

**单元测试(发送消息)**

**cn.itedus.lottery.test.application.KafkaProducerTest**

```java
@Test
public void test_send() throws InterruptedException {
    InvoiceVO invoice = new InvoiceVO();
    invoice.setuId("fustack");
    invoice.setOrderId(1444540456057864192L);
    invoice.setAwardId("3");
    invoice.setAwardType(Constants.AwardType.DESC.getCode());
    invoice.setAwardName("Code");
    invoice.setAwardContent("苹果电脑");
    invoice.setShippingAddress(null);
    invoice.setExtInfo(null);
    kafkaProducer.sendLotteryInvoice(invoice);

    while (true){
        Thread.sleep(10000);
    }
}
```

- 你可以自己主动发送消息的方式进行测试 MQ 的接收和处理过程

**单元测试(流程测试)**

**cn.itedus.lottery.test.application.ActivityProcessTest**

```java
@Test
public void test_doDrawProcess() {
    DrawProcessReq req = new DrawProcessReq();
    req.setuId("xiaofuge");
    req.setActivityId(100001L);
    DrawProcessResult drawProcessResult = activityProcess.doDrawProcess(req);
    logger.info("请求入参：{}", JSON.toJSONString(req));
    logger.info("测试结果：{}", JSON.toJSONString(drawProcessResult));
}
```

输出：

```java
2023-05-08 23:07:28.657  INFO 24724 --- [           main] c.r.l.d.s.s.draw.impl.DrawExecImpl       : 执行抽奖策略 strategyId：10001，无库存排除奖品列表ID集合 awardList：["1"]
2023-05-08 23:07:28.698  INFO 24724 --- [           main] c.r.l.d.s.service.draw.AbstractDrawBase  : 执行策略抽奖完成【已中奖】，用户：AruNi 策略ID：10001 奖品ID：5 奖品名称：Book
2023-05-08 23:07:28.787  INFO 24724 --- [           main] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息 topic：lottery_invoice bizId：AruNi message：{"awardContent":"Code","awardId":"5","awardName":"Book","awardType":1,"orderId":1655590246068387840,"uId":"AruNi"}

2023-05-08 23:07:28.867  INFO 24724 --- [           main] c.r.l.t.application.ActivityProcessTest  : 请求入参：{"activityId":100001,"uId":"AruNi"}
2023-05-08 23:07:28.875  INFO 24724 --- [           main] c.r.l.t.application.ActivityProcessTest  : 测试结果：{"code":"0000","drawAwardVO":{"awardContent":"Code","awardId":"5","awardName":"Book","awardType":1,"uId":"AruNi"},"info":"成功"}
2023-05-08 23:07:28.875  INFO 24724 --- [ad | producer-1] c.r.l.a.p.impl.ActivityProcessImpl       : MQ 消息发送成功, 中奖物品发货单: InvoiceVO{uId='AruNi', orderId=1655590246068387840, awardId='5', awardType=1, awardName='Book', awardContent='Code', shippingAddress=null, extInfo='null'}

2023-05-08 23:07:28.946  INFO 24724 --- [ntainer#0-0-C-1] c.r.l.a.m.c.LotteryInvoiceListener       : 消费MQ消息，完成 topic：lottery_invoice bizId：AruNi 发奖结果：{"code":1,"info":"发奖成功","uId":"AruNi"}
```

![image-20230508231346857](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305082313116.png)

全流程测试是业务服务自己发送MQ，再由消费方来处理 MQ 消息。

从测试结果可以看到，用户 AruNi 执行抽奖中奖后，已经触发了 MQ 消息的发送，并进行了消费，最终奖品发放完成。



















