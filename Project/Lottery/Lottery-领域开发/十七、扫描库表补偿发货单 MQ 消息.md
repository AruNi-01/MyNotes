## 1. 开发前准备

描述：分布式任务调度，扫描抽奖发货单消息状态，对于未发送 MQ 或者发送失败的 MQ，进行补偿发送处理

因为需要扫描库表，也就是循环的方式把每个库下的多张表中的每条用户记录，都进行扫描。所以需要在分库分表组件中，提供出可以设置路由到的库和表，这样就可以满足我们扫描的动作了。这部分可以看最新的 [db-router-spring-boot-starter](https://gitcode.net/KnowledgePlanet/db-router-spring-boot-starter)

在 application 应用层下的 worker 包 LotteryXxlJob 中，添加关于扫描库表补偿消息发送的任务，并在开发完成后把任务配置到 xxl-job 任务调度后台中。关于任务的配置，我们在上一个章节，已经讲述过。

## 2. 任务流程

![image-20230511225047360](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305112250329.png)

我们的任务流程，完成的就是整个抽奖活动中，关于中奖结果落库后，进行 MQ 后。出现问题时，进行补偿消息发送处理的部分。

在 MQ 消息补偿的过程中，会把发送失败的消息和迟迟没有发送的消息，都进行补偿，已保障全流程的可靠性。

## 3. 功能实现

### 3.1 消息补偿任务

`LotteryXxlJob#lotteryOrderMQStateJobHandler`：

```java
    @XxlJob("lotteryOrderMQStateJobHandler")
    public void lotteryOrderMQStateJobHandler() throws Exception {
        // 验证参数
        String jobParam = XxlJobHelper.getJobParam();
        if (null == jobParam) {
            logger.info("扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 错误 params is null");
            return;
        }

        // 获取分布式任务配置参数信息 参数配置格式：1,2,3 也可以是指定扫描一个，也可以配置多个库，按照部署的任务集群进行数量配置，均摊分别扫描效率更高
        String[] params = jobParam.split(",");
        logger.info("扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 开始 params：{}", JSON.toJSONString(params));

        if (params.length == 0) {
            logger.info("扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 结束 params is null");
            return;
        }

        // 获取分库分表配置下的分表数
        int tbCount = dbRouter.tbCount();

        // 循环获取指定扫描库
        for (String param : params) {
            // 获取当前任务扫描的指定分库
            int dbIdx = Integer.parseInt(param);

            // 判断配置指定扫描库数，是否存在
            if (dbIdx > dbRouter.dbCount()) {
                logger.info("扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 结束 dbIdx not exist");
                continue;
            }

            // 循环扫描对应的表
            for (int i = 0; i < tbCount; i++) {
                // 扫描库表数据
                List<InvoiceVO> invoiceVOList = activityPartake.scanInvoiceMqState(dbIdx, i);
                logger.info("扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 扫描库：{} 扫描表：{} 扫描数：{}", dbIdx, i, invoiceVOList.size());

                // 补偿 MQ 消息
                for (InvoiceVO invoiceVO : invoiceVOList) {
                    ListenableFuture<SendResult<String, Object>> future = kafkaProducer.sendLotteryInvoice(invoiceVO);
                    future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
                        @Override
                        public void onSuccess(SendResult<String, Object> stringObjectSendResult) {
                            // MQ 消息发送完成，更新数据库表 user_strategy_export.mq_state = 1
                            activityPartake.updateInvoiceMqState(invoiceVO.getuId(), invoiceVO.getOrderId(), Constants.MQState.COMPLETE.getCode());
                        }

                        @Override
                        public void onFailure(Throwable throwable) {
                            // MQ 消息发送失败，更新数据库表 user_strategy_export.mq_state = 2 【等待定时任务扫码补偿MQ消息】
                            activityPartake.updateInvoiceMqState(invoiceVO.getuId(), invoiceVO.getOrderId(), Constants.MQState.FAIL.getCode());
                        }
                    });
                }
            }
        }

        logger.info("扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 完成 param：{}", JSON.toJSONString(params));
    }
```

这一部分就是使用任务扫描库表的操作，在 activityPartake.scanInvoiceMqState 方法中，会设定路由，如下：

```java
    @Override
    public List<InvoiceVO> scanInvoiceMqState(int dbCount, int tbCount) {
        try {
            // 设置路由
            dbRouter.setDBKey(dbCount);
            dbRouter.setTBKey(tbCount);
            // 查询数据
            return userTakeActivityRepository.scanInvoiceMqState();
        } finally {
            dbRouter.clear();
        }
    }
```

在执行 `scanInvoiceMqState` 方法时，首先通过 `dbRouter.setDBKey(dbCount)` 和 `dbRouter.setTBKey(tbCount)` 方法来设置当前操作的分库分表信息。然后，执行查询操作，查询对应的数据。最后，在方法执行完成后，通过 `dbRouter.clear()` 方法来清除当前操作的分库分表信息，以便于下一次操作能够正确地指定分库分表。

另外这里有一点是关于分布式设计的思考，一般我们运行在线上的任务都是由多个实例共同完成，所以这里我们配置里一个任务的参数，已达到可以满足每个任务实例只跑自己需要扫描的库表。

## 4. 任务配置

在 xxl-job 调度台中添加我们的任务：

![image-20230512000829623](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305120008918.png)

在新增任务中，添加我们的扫描任务。注意这里有一个任务参数配置，用于配置这个任务负责扫描的库表。如果你希望一个任务只处理一个库表，可以在编写任务的时候，提供多个任务方法，方法实现内容只调用一份即可。

## 5. 测试

启动 zk、kafka，启动 xxl-job，启动 lottery 。

修改库表中，user_strategy_export_001~004 中任意一个表的 MQ 状态为 2 表示发送 MQ 失败。

```java
2023-05-12 22:43:15.074  INFO 87121 --- [5-1683902595047] c.r.l.application.worker.LotteryXxlJob   : 扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 开始 params：["1"]
2023-05-12 22:43:15.188  INFO 87121 --- [5-1683902595047] c.r.l.application.worker.LotteryXxlJob   : 扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 扫描库：1 扫描表：0 扫描数：0
2023-05-12 22:43:15.209  INFO 87121 --- [5-1683902595047] c.r.l.application.worker.LotteryXxlJob   : 扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 扫描库：1 扫描表：1 扫描数：1
2023-05-12 22:43:15.211  INFO 87121 --- [5-1683902595047] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息 topic：lottery_invoice bizId：xiaofuge message：{"awardContent":"Code","awardId":"3","awardName":"ipad","awardType":1,"orderId":1454351703703945216,"uId":"xiaofuge"}
2023-05-12 22:43:15.258  INFO 87121 --- [5-1683902595047] c.r.l.application.worker.LotteryXxlJob   : 扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 扫描库：1 扫描表：2 扫描数：0
2023-05-12 22:43:15.283  INFO 87121 --- [5-1683902595047] c.r.l.application.worker.LotteryXxlJob   : 扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 扫描库：1 扫描表：3 扫描数：0
2023-05-12 22:43:15.283  INFO 87121 --- [5-1683902595047] c.r.l.application.worker.LotteryXxlJob   : 扫描用户抽奖奖品发放 MQ 状态[Table = 2*4] 完成 param：["1"]
```

可以看到，我们的任务成功扫描到了 mq_state = 2 的记录，并且该消息发送成功，mq_state 已经改回 1 了。

学习对于流程中，一些非正常情况下的逻辑补偿机制，调用接口、发送 MQ、更新库表都可能会有失败的情况，这些情况就需要一个完善的逻辑处理来进行补偿操作。

在分布式设计情看下，我们会更多的考虑用多个实例来的调度来解决集中的问题，以此提升系统的可靠性。

















