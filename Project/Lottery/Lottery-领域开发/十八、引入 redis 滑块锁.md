## 1. 开发前准备

本章引入 Redis 到抽奖系统，设计颗粒度更细的滑动库存编号分布式锁，处理活动秒杀流程。

在抽奖系统中引入 Redis 模块，优化用户参与抽奖活动。因为只要有大量的用户参与抽奖，那么这个就属于秒杀场景。所以需要使用 Redis 分布式锁的方式来处理集中化库存扣减的问题，否则在 TPS 达到 1k-2k，就会把数据库拖垮。

在设计秒杀流程时，优化锁的颗粒度力度，不要把锁直接放到活动编号上，这样在极端临界情况下会出现秒杀解锁失败，导致库存有剩余但不能下单的情况。所以需要增加锁的颗粒度，**以滑动库存剩余编号的方式进行加锁**，例如 100001_1、100001_2、100001_3，以此类推，具体看代码实现。

增加缓存扣减库存后，发送 MQ 消息进行异步更新数据库中活动库存，做最终数据一致性处理。这一部分如果你的系统并发体量较大，还需要把 MQ 的数据不要直接对库更新，而是更新到缓存中，再由任务最后阶段同步，以此减少对数据库表的操作。

## 2. 扣减流程

![image-20230520102122247](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305201021490.png)

- 优化活动领域，活动参与流程中的库存扣减操作，这部分我们原来是使用数据库行级锁处理的库存扣减，但因为会存在并发问题所以这里优化为 Redis 分布式锁进行处理。
- 活动领取完成后，其实这个时候只是把缓存的库存扣掉了，但数据库中的库存并没有扣减，所以我们需要发送一个 MQ 消息，来对数据库中的库存进行处理。因为 MQ 可以消峰因此在降低 MQ 分片的情况下，消费效率有所下降，并不会对数据库造成压力，保证最终数据一致性即可。但也有例外，所以我们提到可以使用定时任务来更新数据库库存。

## 3. 配置 Redis 云服务环境

我这里使用 Docker 部署。

### 3.1 拉取 Redis 镜像

我们这里选择的是 6.2.6 版本：

```sh
☁  ~  docker pull redis:6.2.6
# ......

☁  ~  docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        6.2.6     3c3da61c4be0   13 months ago   113MB
```

### 3.2 创建挂载的文件

我们现在只需要把镜像 run 起来，就能得到一个运行的 redis 容器了。

不过我们需要提前将文件挂载好，比如配置文件，持久化文件，保存到我们的宿主机上来，否则容器挂了我们的数据也丢了。（**挂载**：即将宿主的文件和容器内部目录相关联，相互绑定，在宿主机内修改文件的话也随之修改容器内部文件）

- 配置文件 `redis.conf`：在宿主机自己创建文件，将配置文件的内容粘贴进去。用于 redis 实例的启动；

  > 在 Redis 官网可找到配置文件的内容：https://redis.io/docs/management/config/

- 持久化文件夹 `data`：自己创建即可。

我创建的文件如下：

```sh
☁  redis-lottery-volume  pwd
/home/redis-lottery-volume
☁  redis-lottery-volume  ls
data  redis.conf
```

### 3.3 启动 Redis 容器

现在只需要把 redis 镜像 run 起来，就可得到 redis 容器了，在 run 镜像的时候，把文件给挂载上去即可。

启动命令：

```sh
docker run -p 6379:6379 --name redis-lottery -v /home/redis-lottery-volume/redis.conf:/etc/redis/redis.conf -v /home/redis-lottery-volume/data:/data -d redis:6.2.6 redis-server /etc/redis/redis.conf
```

- `-p 6379:6379` 将 6379 端口映射出去
- `--name` 给这个容器取一个名字
- `-v` 数据卷挂载
  - `/home/redis-lottery-volume/redis.conf:/etc/redis/redis.conf` 将 liunx 路径下的redis.conf 和容器内 redis 下的 redis.conf 挂载在一起；
  - `/home/redis-lottery-volume/data:/data` 这个同上。
- `-d` 表示后台启动 redis
- `redis-server /etc/redis/redis.conf` 以配置文件启动 redis，加载容器内的 conf 文件，最终找到的是挂载的目录，也就是 liunx下的 `/home/redis-lottery-volume/redis.conf`

> 注意：`redis.conf` 中的 daemonize 不要设置为 yes，因为 docker 是以后台进程启动的 redis，redis 不能再以后台进程启动了，否则会出错。

启动成功：

![image-20230520115646124](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305201156616.png)

查看容器运行日志：`docker logs --since 30m redis-lottery`

进入容器：

```sh
☁  redis-lottery-volume  docker exec -it redis-lottery /bin/bash
root@fdeed99aefbd:/data# redis-cli

127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> ping
PONG
```

## 4. 功能开发

### 4.1 滑块库存锁的设计

![image-20230520170001920](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305201700354.png)

- 如图所示，即使是使用 Redis 分布式锁，我们也不希望把锁的颗粒度放的太粗，否则还是会出现活动有库存但不能秒杀，提示“活动过于火爆”；

- 那么我们就需要 **按照活动编号把库存锁的颗粒度缩小，实际操作也并不复杂，只是把 `活动ID+库存扣减后的值` 一起作为分布式锁的Key，这样就缩小了锁的颗粒度**。

### 4.2 滑块库存锁的实现

在活动领域层的领取活动抽象类 `BaseActivityPartake` 添加方法 `subtractionActivityStockByRedis`、`recoverActivityCacheStockByRedis`，分别用作 Redis 库存扣减和解锁 Key 的处理。

> 库存扣减操作

`ActivityRepository#subtractionActivityStockByRedis`：

```java
    @Override
    public StockResult subtractionActivityStockByRedis(String uId, Long activityId, Integer stockCount) {
        // 1. 获取抽奖活动库存 Key
        String stockKey = Constants.RedisKey.KEY_LOTTERY_ACTIVITY_STOCK_COUNT(activityId);

        // 2. 扣减库存（增加库存使用量），目前占用库存数（incr(stockKey, 1) 返回值为 incr 后的新 stock 值）
        Integer stockUsedCount = (int) redisUtil.incr(stockKey, 1);

        // 3. 超出库存判断（库存使用量 > 库存数量），进行回复原始库存
        if (stockUsedCount > stockCount) {
            redisUtil.decr(stockKey, 1);
            return new StockResult(Constants.ResponseCode.OUT_OF_STOCK.getCode(), Constants.ResponseCode.OUT_OF_STOCK.getInfo());
        }

        // 4. 以活动库存占用编号，生成对应加锁 key，细化锁的粒度
        String stockTokenKey = Constants.RedisKey.KEY_LOTTERY_ACTIVITY_STOCK_COUNT_TOKEN(activityId, stockUsedCount);
        boolean lockToken = redisUtil.setNx(stockTokenKey, 350L);
        if (!lockToken) {
            logger.info("抽奖活动{}用户秒杀{}扣减库存，分布式锁失败：{}", activityId, uId, stockTokenKey);
            return new StockResult(Constants.ResponseCode.ERR_TOKEN.getCode(), Constants.ResponseCode.ERR_TOKEN.getInfo());
        }

        return new StockResult(Constants.ResponseCode.SUCCESS.getCode(), Constants.ResponseCode.SUCCESS.getInfo(), stockTokenKey, stockCount - stockUsedCount);
    }
```

代码中的注释就是整个操作流程，创建 Key、占用库存、判断库存、**以占用库存的编号做为加锁key**、调用 setNx 加一个分布式锁，最终完成整个秒杀扣减库存的动作。

这里还有一些其他的实现方式，例如：`lua脚本`、zk、jvm层，都可以处理，但经过验证 lua脚本会有一定的耗时，并发较高时会有问题。

秒杀的设计原则是保证不超卖，但不一定保证100个库存完全消耗掉，因为可能会有一些锁失败的情况。不过在库存数据最终一致性任务处理下，基本保证`3个9`到`4个9`的可用率还是没问题的。

如果说你的库存秒杀tps已经到10级以上，*一般单key在10万以下是可靠的*，那么这个时候就需要进行库存分片，把路由思想用到 Redis 使用上，不同的用户进入 Redis 进行秒杀处理时，把他的库存操作路由到属于他的分组上进行处理，那么这样就可以横向扩展了。

**注意**：这里需要修改一下 ActivityRepository 类下的 queryActivityBill 方法，在查询活动账单时，活动库存需要先从 redis 中获取，因为现在 redis 中才是最新的数据：

```java
    @Override
    public ActivityBillVO queryActivityBill(PartakeReq req) {
        // 查询活动信息
        Activity activity = activityDao.queryActivityById(req.getActivityId());

        // 从缓存中获取库存
        Object usedStockCountObj =  redisUtil.get(Constants.RedisKey.KEY_LOTTERY_ACTIVITY_STOCK_COUNT(req.getActivityId()));

        // 查询领取次数
        UserTakeActivityCount userTakeActivityCountReq = new UserTakeActivityCount();
        userTakeActivityCountReq.setuId(req.getuId());
        userTakeActivityCountReq.setActivityId(req.getActivityId());
        UserTakeActivityCount userTakeActivityCount = userTakeActivityCountDao.queryUserTakeActivityCount(userTakeActivityCountReq);

        // 封装结果信息
        ActivityBillVO activityBillVO = new ActivityBillVO();
        activityBillVO.setuId(req.getuId());
        activityBillVO.setActivityId(req.getActivityId());
        activityBillVO.setActivityName(activity.getActivityName());
        activityBillVO.setBeginDateTime(activity.getBeginDateTime());
        activityBillVO.setEndDateTime(activity.getEndDateTime());
        activityBillVO.setTakeCount(activity.getTakeCount());
        activityBillVO.setStockCount(activity.getStockCount());
        activityBillVO.setStockSurplusCount(null == usedStockCountObj ? activity.getStockSurplusCount() : activity.getStockCount() - Integer.parseInt(String.valueOf(usedStockCountObj)));
        activityBillVO.setStrategyId(activity.getStrategyId());
        activityBillVO.setState(activity.getState());
        activityBillVO.setUserTakeLeftCount(null == userTakeActivityCount ? null : userTakeActivityCount.getLeftCount());

        return activityBillVO;
    }
```



> 并发锁删除处理

`ActivityRepository#recoverActivityCacheStockByRedis`：

```java
    @Override
    public void recoverActivityCacheStockByRedis(Long activityId, String tokenKey, String code) {
        // 删除分布式锁 Key
        redisUtil.del(tokenKey);
    }
```

秒杀完毕后，接下来的流程是用户记录落库，但可能这个时候会发生失败的情况，因为需要在失败时恢复缓存的库存。不过这个情况不是事务性的，因此可能会恢复失败，也就是保证不超卖，但不能保证一定完全消耗库存。*当然这个部分具体看你希望的是什么，比如你可以牺牲掉一些性能，考虑 lua 脚本和给整个流程分布式事务锁。*

如果最后的操作是成功的，那么正常删除掉这个加锁的 Key 就可以了，因为下一个用户获取的到的库存滑块又是新的了，旧 Key 已经没有用了。

### 4.3 活动秒杀流程处理

`BaseActivityPartake#doPartake`：

![image-20230520211834375](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305202118970.png)

在领取活动的模板方法中，优化掉原来直接使用数据库行级锁的流程，把 Redis 库存的扣减添加到这里。

扣减库存后，在各个以下的流程节点中，如果有流程失败则进行缓存库存的恢复操作。

### 4.4 发送 MQ 消息，处理数据一致性

由于我们使用 Redis 代替数据库库存，那么在缓存的库存处理后，还需要 **把数据库中的库存处理为和缓存一致**，这样在后续运营这部分数据时才能保证一定的运营可靠性。

`ActivityProcessImpl#doDrawProcess`：

![image-20230520214728460](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305202147736.png)

申请新的 MQ Topic：`bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic lottery_activity_partake`

这里需要注意，MQ 的发送，只发生在用户首次领取活动时，如果是已经领取活动但因为抽奖等流程失败，二次进入此流程，则不会发送 MQ 消息。

### 4.5 活动领取记录 MQ 消费

`LotteryActivityPartakeRecordListener`：

```java
    @KafkaListener(topics = "lottery_activity_partake", groupId = "lottery")
    public void onMessage(ConsumerRecord<?, ?> record, Acknowledgment ack, @Header(KafkaHeaders.RECEIVED_TOPIC) String topic) {
        Optional<?> message = Optional.ofNullable(record.value());

        // 1. 判断消息是否存在
        if (!message.isPresent()) {
            return;
        }

        // 2. 转化对象（或者也可以重写 Serializer<T>）
        ActivityPartakeRecordVO activityPartakeRecordVO = JSON.parseObject((String) message.get(), ActivityPartakeRecordVO.class);
        logger.info("消费MQ消息，异步扣减活动库存 message：{}", message.get());

        // 3. 更新数据库库存【实际场景业务体量较大，可能也会由于MQ消费引起并发，对数据库产生压力，所以如果并发量较大，
        //                 可以把库存记录缓存中，并使用定时任务进行处理缓存和数据库库存同步，减少对数据库的操作次数】
        activityPartake.updateActivityStock(activityPartakeRecordVO);
    }
```

消费 MQ 消息的流程就比较简单了，接收到 MQ 进行更新数据库处理即可。**不过我们这里更新数据库并不是直接对数据库进行库存扣减操作，而是把从缓存拿到的库存最新镜像更新到数据库中**。它的 SQL = `UPDATE activity SET stock_surplus_count = #{stockSurplusCount} WHERE activity_id = #{activityId} AND stock_surplus_count > #{stockSurplusCount}`

更新数据库库存【实际场景业务体量较大，可能也会由于MQ消费引起并发，对数据库产生压力，所以如果并发量较大，可以把库存记录缓存中，并使用定时任务进行处理缓存和数据库库存同步，减少对数据库的操作次数】

## 5. 测试

确保：kafka 开启、xxl-job 开启、redis 服务开启

确保：活动状态可用、活动库存充足、活动时间有效

记得在 `application.yml` 中配置 Redis 的配置信息，在 `lottery-domain` 中的 support 包下添加 redis 的配置类。

单元测试 `ActivityProcessTest#test_doDrawProcess`：

```java
    @Test
    public void test_doDrawProcess() {
        DrawProcessReq req = new DrawProcessReq();
        req.setuId("AruNi");
        req.setActivityId(100001L);
        DrawProcessResult drawProcessResult = activityProcessImpl.doDrawProcess(req);

        logger.info("请求入参：{}", JSON.toJSONString(req));
        logger.info("测试结果：{}", JSON.toJSONString(drawProcessResult));
    }
```

测试结果：

```java
2023-05-20 23:34:42.631  INFO 21916 --- [           main] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(领取活动记录) topic：lottery_activity_partake bizId：AruNi message：{"activityId":100001,"stockCount":100,"stockSurplusCount":99,"uId":"AruNi"}

2023-05-20 23:34:42.686  INFO 21916 --- [ntainer#0-0-C-1] m.c.LotteryActivityPartakeRecordListener : 消费MQ消息，异步扣减活动库存 message：{"activityId":100001,"stockCount":100,"stockSurplusCount":99,"uId":"AruNi"}
2023-05-20 23:34:42.757  INFO 21916 --- [           main] c.r.l.d.s.s.draw.impl.DrawExecImpl       : 执行抽奖策略 strategyId：10001，无库存排除奖品列表ID集合 awardList：["1"]
2023-05-20 23:34:42.806  INFO 21916 --- [           main] c.r.l.d.s.service.draw.AbstractDrawBase  : 执行策略抽奖完成【已中奖】，用户：AruNi 策略ID：10001 奖品ID：3 奖品名称：ipad
2023-05-20 23:34:42.854  INFO 21916 --- [           main] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(中奖发货单) topic：lottery_invoice bizId：AruNi message：{"awardContent":"Code","awardId":"3","awardName":"ipad","awardType":1,"orderId":1659945754401308672,"uId":"AruNi"}
2023-05-20 23:34:42.860  INFO 21916 --- [ad | producer-1] c.r.l.a.p.impl.ActivityProcessImpl       : MQ 消息发送成功, 中奖物品发货单: InvoiceVO{uId='AruNi', orderId=1659945754401308672, awardId='3', awardType=1, awardName='ipad', awardContent='Code', shippingAddress=null, extInfo='null'}
2023-05-20 23:34:42.865  INFO 21916 --- [           main] c.r.l.t.application.ActivityProcessTest  : 请求入参：{"activityId":100001,"uId":"AruNi"}
2023-05-20 23:34:42.880  INFO 21916 --- [           main] c.r.l.t.application.ActivityProcessTest  : 测试结果：{"code":"0000","drawAwardVO":{"awardContent":"Code","awardId":"3","awardName":"ipad","awardType":1,"uId":"AruNi"},"info":"成功"}
```

从测试结果已经可以看到，异步扣减库存的流程已经生效，同时你可以去检查数据库中活动库存是否已经异步更新。*因为我们有一个更新条件判断是否更新数据比剩余库存数据最新，否则不会更新*。









