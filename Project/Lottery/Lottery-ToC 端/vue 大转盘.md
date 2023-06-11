前端项目地址：https://gitcode.net/KnowledgePlanet/lottery-front

## 一、开发前准备

- 基于 IDEA 创建 vue 工程，并加入新的抽检模块
- 通过 Lottery-API 开发接口，给抽奖 vue 页面调用，这里需要配置跨域 `@CrossOrigin("*")`

下面需要在 Lottery 项目中添加 Nacos 作为服务注册与发现中心，让我们的 Lottery-API 使用 Lottery 的服务（将服务注册到 Nacos），然后前端 vue 大转盘请求 Lottery-API 即可。

### 1. 下载 Nacos

下载地址：https://github.com/alibaba/nacos/releases

进入 nacos/bin 目录，以单机模式启动 nacos：

```sh
sh startup.sh -m standalone
```



### 2. SpringBoot 整合 Nacos

在父工程的 `pom.xml` 中添加 Nacos 服务发现的依赖，顺便把 Dubbo 升级到 2.7.10 版本：

```xml
  <!-- https://mvnrepository.com/artifact/com.alibaba.boot/nacos-discovery-spring-boot-starter -->
  <dependency>
        <groupId>com.alibaba.boot</groupId>
        <artifactId>nacos-discovery-spring-boot-starter</artifactId>
        <version>0.2.7</version>
  </dependency>
```

然后在 lottery-interfaces 的 pom.xml 中引入使用 nacos。

接着配置 `application.yml`：

```yml
#......

# nacos 配置中心
nacos:
  discovery:
    server-addr: 127.0.0.1:8848

# Dubbo 广播方式配置，如果调用不稳定可以修改为直连模式：https://codechina.csdn.net/KnowledgePlanet/Lottery/-/issues/1
dubbo:
  application:
    name: Lottery
    version: 1.0.0
  # 注册地址
  registry:
    #    address: N/A
    #    address: #multicast://224.5.6.7:1234
    #    address: zookeeper://127.0.0.1:2181
    address: nacos://127.0.0.1:8848
  # 通信协议和端口
  protocol:
    name: dubbo
    # dubbo 自己起端口
    port: -1
  # 相当于 Spring 中自动扫描包的地址，可以把此包下的所有 rpc 接口都注册到服务中
  scan:
    base-packages: com.run.lottery.rpc.*

```

此时 Dubbo 中的服务就能注册到 Nacos 了。

我们启动项目，在 Nacos 地址 http://localhost:8848/nacos/ 中查看服务：

![image-20230526133818119](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305261338823.png)

## 二、Vue 工程

注意：该工程 node.js 需要降到 16.xx。

Vue 工程地址：https://gitcode.net/KnowledgePlanet/lottery-front

抽奖面板插件使用： [lucky-canvas](https://100px.net/) 

那么接下来只需要在 Lottery-API 中添加接口，给前端调用即可。

## 三、Lottery 提供 RPC 接口

在展示奖品面板时，是需要根据某个活动 ID 查询出该活动，然后在活动中会有策略 ID，在策略明细中就有大致的奖品信息（够前端展示使用了）。

所以我们先要在 Lottery 项目中提供一个查询活动奖品信息的 RPC 接口，来供 Lottery-API 调用。

### 1. rpc 层添加接口

我将这个接口放在 rpc.activity.deploy 包下（虽然是展示奖品信息，但也是活动的奖品信息，所以不合适放在 award 包下，award 包应该跟发奖相关）：

![image-20230528195320499](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305281953327.png)



### 2. interfaces 层实现 rpc 接口

在 interfaces.facade.LotteryActivityDeploy 类中添加具体实现：

```java
// 注意使用 @DubboService，将接口交给 Dubbo 管理
@DubboService
public class LotteryActivityDeploy implements ILotteryActivityDeploy {

    private Logger logger = LoggerFactory.getLogger(LotteryActivityBooth.class);

    @Resource
    private IActivityDeployProcess activityDeploy;

    @Resource
    private IMapping<ActivityVO, ActivityDTO> activityMapping;

    @Override
    public ActivityRes queryActivityListByPageForErp(ActivityPageReq req) {}

    @Override
    public ActivityAwardsRes queryActivityAwardList(ActivityAwardReq req) {
        try {
            logger.info("活动部署页活动奖品信息查询开始 activityId：{}, activityName: {}", req.getActivityId(), req.getActivityName());

            // 1. 包装入参
            ActivityAwardInfoReq activityAwardInfoReq = new ActivityAwardInfoReq();
            activityAwardInfoReq.setActivityId(req.getActivityId());
            activityAwardInfoReq.setActivityName(req.getActivityName());

            // 2. 查询结果
            ActivityAwardRich activityAwardRich = activityDeploy.queryActivityAwards(activityAwardInfoReq);
            List<AwardVO> awardVOList = activityAwardRich.getAwardVOList();

            // 3. 转换对象
            List<AwardDTO> awardDTOList = new ArrayList<>();
            for (AwardVO awardVO : awardVOList) {
                AwardDTO awardDTO = new AwardDTO();
                awardDTO.setAwardId(awardVO.getAwardId());
                awardDTO.setAwardName(awardVO.getAwardName());
                awardDTO.setAwardType(awardVO.getAwardType());
                awardDTO.setAwardContent(awardVO.getAwardContent());
                awardDTOList.add(awardDTO);
            }

            // 4. 封装数据
            ActivityAwardsRes activityAwardsRes = new ActivityAwardsRes(Result.buildSuccessResult());
            activityAwardsRes.setActivityId(activityAwardRich.getActivityId());
            activityAwardsRes.setActivityName(activityAwardRich.getActivityName());
            activityAwardsRes.setAwardDTOList(awardDTOList);

            logger.info("活动部署页活动奖品信息查询完成 activityId：{}, activityName: {}, AwardDTOList: {}", activityAwardsRes.getActivityId(), activityAwardsRes.getActivityName(), JSON.toJSON(activityAwardsRes.getAwardDTOList()));

            // 5. 返回结果
            return activityAwardsRes;
        } catch (Exception e) {
            logger.error("活动部署页活动奖品信息查询失败 activityId：{}, reqStr：{}", req.getActivityId(), JSON.toJSON(req), e);
            return new ActivityAwardsRes(Result.buildErrorResult());
        }
    }

}
```

在实现中调用了 application 层的接口。

### 3. application 层定义接口

在 application 层定义相关接口，调用 domain 层。这里直接看接口的实现类 ActivityDeployProcessImpl：

```java
@Service
public class ActivityDeployProcessImpl implements IActivityDeployProcess {

    @Resource
    private IActivityDeploy activityDeploy;

    @Override
    public ActivityInfoLimitPageRich queryActivityInfoLimitPage(ActivityInfoLimitPageReq req) {
        return activityDeploy.queryActivityInfoLimitPage(req);
    }

    @Override
    public ActivityAwardRich queryActivityAwards(ActivityAwardInfoReq req) {
        return activityDeploy.queryActivityAwards(req);
    }

}
```

这里调用了 domain 层的 IActivityDeploy 接口。

### 4. domain 层完成服务

在 domain 中完成该业务功能，这里比较简单，就是调用 Repository：

![image-20230528200536403](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305282005831.png)



这里在 domain 层的 repository 包下添加了这个 queryActivityAwards 的接口，在 infrastructure 层实现即可。

### 5. infrastructure 层实现 repository

由于已经有根据策略 ID，查询出策略详情信息（里面包含奖品的大致信息），所以直接调用，做一下对象转换即可：

![image-20230528200848433](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305282008470.png)

## 四、Lottery-API 提供 API

现在在 Lottery-API 供提供该 API，调用 RPC 接口即可，然后该 API 供前端调用。

我这里顺便把抽奖接口也提供了。由于实现很简单，就是调用接口即可，所以直接看图就懂了：

interfaces 层提供 API 给前端调用，自己调用 application 层的服务接口：

![image-20230528202157119](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305282021607.png)

application 层定义服务接口：

![image-20230528202252287](/Users/aarynlu/Library/Application Support/typora-user-images/image-20230528202252287.png)

domain 层做具体实现：

![image-20230528202525697](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305282025155.png)

实现细节：

```java
/**
 * @desc: Vue 大转盘 Service 实现类
 * @author: AruNi_Lu
 * @date: 2023-05-27
 */
@Service
public class LuckyServiceImpl implements ILuckyService {

    private Logger logger = LoggerFactory.getLogger(LuckyServiceImpl.class);

    @DubboReference(timeout = 3000)
    private ILotteryActivityDeploy lotteryActivityDeploy;

    @DubboReference(timeout = 3000)
    private ILotteryActivityBooth lotteryActivityBooth;

    @Override
    public List<PrizeVO> queryPrizes(Long activityId) {
        logger.info("查询活动的奖品列表信息 --- 开始, activityId: {}", activityId);

        // 1. 构建请求信息
        ActivityAwardReq activityAwardReq = new ActivityAwardReq();
        activityAwardReq.setActivityId(activityId);

        // 2. 执行奖品信息查询
        ActivityAwardsRes activityAwardsRes = lotteryActivityDeploy.queryActivityAwardList(activityAwardReq);

        // 3. 封装结果
        List<AwardDTO> awardDTOList = activityAwardsRes.getAwardDTOList();
        List<PrizeVO> prizeVOList = new ArrayList<>();
        boolean flag = false;
        for (AwardDTO awardDTO : awardDTOList) {
            PrizeVO prizeVO = new PrizeVO(new FontVO(awardDTO.getAwardName(), "10%", ""));
            if (flag) {
                prizeVO.setBackground("#e9e8fe");
            } else {
                prizeVO.setBackground("#b8c5f2");
            }
            flag = !flag;
            prizeVOList.add(prizeVO);
        }

        logger.info("查询活动的奖品列表信息 --- 结束, prizeList: {}", JSON.toJSON(prizeVOList));

        return prizeVOList;
    }

    @Override
    public LuckyDrawRes doDraw(LuckyDrawReq req) {
        logger.info("执行抽奖 --- 开始, uId: {}, activityId: {}", req.getuId(), req.getActivityId());

        // 1. 构建请求信息
        DrawReq drawReq = new DrawReq();
        drawReq.setuId(req.getuId());
        drawReq.setActivityId(req.getActivityId());

        // 2. 执行抽奖
        DrawRes drawRes = lotteryActivityBooth.doDraw(drawReq);
        if (!drawRes.getCode().equals(Constants.ResponseCode.SUCCESS.getCode())) {
            logger.error("执行抽奖 --- 失败, errCode: {}, errInfo: {}", drawRes.getCode(), drawRes.getInfo());
            return new LuckyDrawRes(Result.buildResult(drawRes.getCode(), drawRes.getInfo()));
        }

        // 3. 封装结果
        AwardDTO awardDTO = drawRes.getAwardDTO();
        LuckyDrawRes luckyDrawRes = new LuckyDrawRes(Result.buildResult(drawRes.getCode(), drawRes.getInfo()));
        luckyDrawRes.setUserId(awardDTO.getUserId());
        luckyDrawRes.setActivityId(awardDTO.getActivityId());
        luckyDrawRes.setAwardId(awardDTO.getAwardId());
        luckyDrawRes.setAwardName(awardDTO.getAwardName());
        luckyDrawRes.setAwardContent(awardDTO.getAwardContent());
        luckyDrawRes.setGrantType(awardDTO.getGrantType());
        luckyDrawRes.setGrantDate(awardDTO.getGrantDate());

        logger.info("执行抽奖 --- 结束, luckyDrawRes: {}", JSON.toJSON(luckyDrawRes));

        return luckyDrawRes;
    }

}
```

这样就做好了 API 的提供，当然了，这里只是简单实现，还没考虑异常出错的情况，后续完善。

## 五、Lottery-Front 调用 API

主要就是在 Lucky-Canvas 的代码中添加调用我们后端的接口，这里引入了 Element-UI 做奖品信息提示：

```vue
<template>
    <LuckyWheel
            ref="myLucky"
            width="300px"
            height="300px"
            :prizes="prizes"
            :blocks="blocks"
            :buttons="buttons"
            @start="startCallback"
            @end="endCallback"
    />
</template>

<script>
    export default {
        data() {
            return {
                blocks: [{padding: '13px', background: '#617df2'}],
                prizes: [
                    // {fonts: [{text: '0', top: '10%'}], background: '#e9e8fe'},
                    // {fonts: [{text: '1', top: '10%'}], background: '#b8c5f2'},
                    // {fonts: [{text: '2', top: '10%'}], background: '#e9e8fe'},
                    // {fonts: [{text: '3', top: '10%'}], background: '#b8c5f2'},
                    // {fonts: [{text: '4', top: '10%'}], background: '#e9e8fe'},
                    // {fonts: [{text: '5', top: '10%'}], background: '#b8c5f2'},
                ],
                buttons: [
                    {radius: '50px', background: '#617df2'},
                    {radius: '45px', background: '#afc8ff'},
                    {
                        radius: '40px', background: '#869cfa',
                        pointer: true,
                        fonts: [{text: '开始\n抽奖', top: '-20px'}]
                    },
                ],

                luckyDrawReq: {
                    uId: '',
                    activityId: -1
                },

                prize: {
                    result: {
                        code: '',
                        info: '',
                    },
                    userId: '',
                    activityId: -1,
                    awardId: '',
                    awardType: -1,
                    awardName: '',
                    awardContent: '',
                    grantType: -1,
                    grantDate: new Date()
                }
            }
        },
        created() {
            // 初始化奖品数据
            this.initPrizeList();
        },
        methods: {
            initPrizeList() {
                // 假设该活动 ID 为 100001
                const activityId = 100001;

                // 获取抽奖列表数据
                this.$http.get("http://localhost:9002/api/queryPrizeList",
                    {params: {activityId: activityId}})
                    .then((res) => {
                        this.prizes = res.data;
                }, (err) => {
                    console.log(err);
                });
            },

            // 点击抽奖按钮会触发star回调
            startCallback() {
                // 调用抽奖组件的play方法开始游戏
                this.$refs.myLucky.play();

                // 模拟请求信息
                this.luckyDrawReq.uId = '2000000001';
                this.luckyDrawReq.activityId = 100001;

                setTimeout(() => {
                    // 调用抽奖接口
                    this.$http.post("http://localhost:9002/api/drawPrize", this.luckyDrawReq, {
                        Headers: {
                            'Content-Type': 'application/json',
                        }
                    }).then((res) => {
                        if (!res.ok) {
                            this.$refs.myLucky.stop(0);
                            return;
                        }

                        // 中奖信息
                        this.prize = res.data;
                        // console.log('抽奖结束，code: ' + this.prize.result.code + ', info: ' + this.prize.result.info);

                        // 返回 code = 0000 才是抽奖成功
                        if (this.prize.result.code !== '0000') {
                            this.$refs.myLucky.stop(0);
                            this.$message({
                                showClose: true,
                                message: '抽奖失败 ~_~, errorCode: ' + this.prize.result.code + ', errorInfo: ' + this.prize.result.info,
                                type: 'error',
                                duration: 0,
                            });
                            return;
                        }

                        // 中奖索引
                        let index;
                        for (let i = 0; i < this.prizes.length; i++) {
                            if (this.prizes[i].fonts[0].text === this.prize.awardName) {
                                index = i;
                                break;
                            }
                        }

                        // 调用stop停止旋转并传递中奖索引
                        this.$refs.myLucky.stop(index);
                    }, (err) => {
                        console.log(err);
                    });
                }, 3000)
            },

            // 抽奖结束会触发end回调
            endCallback(prize) {
                // 抽奖成功才执行，失败有对应的提示信息
                if (this.prize.result.code === '0000') {
                    this.$message({
                        showClose: true,
                        message: '恭喜您💐，中奖了：' + prize.fonts[0].text + '！',
                        type: 'success',
                        duration: 0,
                    });
                }
            },

        }
    }
</script>
```

## 六、测试

启动：

- Redis、Kafka、xxl-job Admin、Nacos
- Lottery 服务；
- Lottery API 服务；
- Lottery-Front 前端；

进入前端页面：localhost:8080，此时就是初始化转盘，调用 API，查询出活动 ID 为 100001 的奖品信息：

![image-20230528203140294](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305282031515.png)

Log 信息如下：

- Lottery-API：

  ```java
  2023-05-28 20:30:44.861  INFO 3521 --- [nio-9002-exec-1] c.r.l.a.d.l.service.LuckyServiceImpl     : 查询活动的奖品列表信息 --- 开始, activityId: 100001
  2023-05-28 20:30:44.918  INFO 3521 --- [nio-9002-exec-1] c.r.l.a.d.l.service.LuckyServiceImpl     : 查询活动的奖品列表信息 --- 结束, prizeList: [{"fonts":[{"color":"","top":"10%","text":"IMac"}],"background":"#b8c5f2"},{"fonts":[{"color":"","top":"10%","text":"iphone"}],"background":"#e9e8fe"},{"fonts":[{"color":"","top":"10%","text":"ipad"}],"background":"#b8c5f2"},{"fonts":[{"color":"","top":"10%","text":"AirPods"}],"background":"#e9e8fe"},{"fonts":[{"color":"","top":"10%","text":"Book"}],"background":"#b8c5f2"},{"fonts":[{"color":"","top":"10%","text":"Keyboard"}],"background":"#e9e8fe"}]
  ```

- Lottery：

  ```java
  2023-05-28 20:30:44.865  INFO 90412 --- [20880-thread-78] c.r.l.i.facade.LotteryActivityBooth      : 活动部署页活动奖品信息查询开始 activityId：100001, activityName: null
  2023-05-28 20:30:44.915  INFO 90412 --- [20880-thread-78] c.r.l.i.facade.LotteryActivityBooth      : 活动部署页活动奖品信息查询完成 activityId：100001, activityName: 活动名, AwardDTOList: [{"awardId":"1","awardName":"IMac"},{"awardId":"2","awardName":"iphone"},{"awardId":"3","awardName":"ipad"},{"awardId":"4","awardName":"AirPods"},{"awardId":"5","awardName":"Book"},{"awardId":"6","awardName":"Keyboard"}]
  ```



此时我们点击抽奖，就会调用 API 去执行抽奖，然后返回中奖信息：

![image-20230528203606184](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305282036752.png)

Log 信息如下：

- Lottery-API：

  ```java
  2023-05-28 20:35:39.621  INFO 3521 --- [nio-9002-exec-6] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 开始, uId: 2000000001, activityId: 100001
  2023-05-28 20:35:40.118  INFO 3521 --- [nio-9002-exec-6] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 结束, luckyDrawRes: {"activityId":100001,"awardId":"4","awardName":"AirPods","awardContent":"Code","userId":"2000000001"}
  ```

- Lottery：

  ```java
  2023-05-28 20:35:39.625  INFO 90412 --- [20880-thread-83] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，开始 uId：2000000001 activityId：100001
  2023-05-28 20:35:39.921  INFO 90412 --- [20880-thread-83] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(领取活动记录) topic：lottery_activity_partake bizId：2000000001 message：{"activityId":100001,"stockCount":100,"stockSurplusCount":99,"uId":"2000000001"}
  2023-05-28 20:35:40.009  INFO 90412 --- [20880-thread-83] c.r.l.d.s.s.draw.impl.DrawExecImpl       : 执行抽奖策略 strategyId：10001，无库存排除奖品列表ID集合 awardList：["1"]
  2023-05-28 20:35:40.048  INFO 90412 --- [20880-thread-83] c.r.l.d.s.service.draw.AbstractDrawBase  : 执行策略抽奖完成【已中奖】，用户：2000000001 策略ID：10001 奖品ID：4 奖品名称：AirPods
  2023-05-28 20:35:40.081  INFO 90412 --- [20880-thread-83] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(中奖发货单) topic：lottery_invoice bizId：2000000001 message：{"awardContent":"Code","awardId":"4","awardName":"AirPods","awardType":1,"orderId":1662799798929063936,"uId":"2000000001"}
  2023-05-28 20:35:40.117  INFO 90412 --- [20880-thread-83] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，完成 uId：2000000001 activityId：100001 drawRes：{"awardDTO":{"activityId":100001,"awardContent":"Code","awardId":"4","awardName":"AirPods","awardType":1,"userId":"2000000001"},"code":"0000","info":"成功"}
  2023-05-28 20:35:40.119  INFO 90412 --- [ad | producer-1] c.r.l.a.p.d.i.ActivityDrawProcessImpl    : MQ 消息发送成功, 中奖物品发货单: InvoiceVO{uId='2000000001', orderId=1662799798929063936, awardId='4', awardType=1, awardName='AirPods', awardContent='Code', shippingAddress=null, extInfo='null'}
  2023-05-28 20:35:40.149  INFO 90412 --- [ntainer#0-0-C-1] m.c.LotteryActivityPartakeRecordListener : 消费MQ消息，异步扣减活动库存 message：{"activityId":100001,"stockCount":100,"stockSurplusCount":99,"uId":"2000000001"}
  2023-05-28 20:35:40.167  INFO 90412 --- [ntainer#1-4-C-1] c.r.l.a.m.c.LotteryInvoiceListener       : 消费MQ消息，完成 topic：lottery_invoice bizId：2000000001 发奖结果：{"code":1,"info":"发奖成功","uId":"2000000001"}
  ```



引入 Element-UI 代替弹窗提示中奖信息，如果抽奖失败的话，比如抽奖次数不足，或者活动状态不对时：

![image-20230529001446775](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305290014734.png)

Log：

- Lottery-API：

  ```java
  2023-05-29 00:10:42.104  INFO 12113 --- [io-9002-exec-10] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 开始, uId: 2000000001, activityId: 100001
  2023-05-29 00:10:44.051  INFO 12113 --- [io-9002-exec-10] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 结束, luckyDrawRes: {"result":{"code":"0000","info":"成功"},"activityId":100001,"awardId":"5","awardName":"Book","awardContent":"Code","userId":"2000000001"}
  2023-05-29 00:11:29.836  INFO 12113 --- [nio-9002-exec-1] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 开始, uId: 2000000001, activityId: 100001
  2023-05-29 00:11:30.213  INFO 12113 --- [nio-9002-exec-1] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 结束, luckyDrawRes: {"result":{"code":"0000","info":"成功"},"activityId":100001,"awardId":"4","awardName":"AirPods","awardContent":"Code","userId":"2000000001"}
  2023-05-29 00:11:58.135  INFO 12113 --- [nio-9002-exec-2] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 开始, uId: 2000000001, activityId: 100001
  2023-05-29 00:11:58.301 ERROR 12113 --- [nio-9002-exec-2] c.r.l.a.d.l.service.LuckyServiceImpl     : 执行抽奖 --- 失败, errCode: 0001, errInfo: 个人领取次数非可用
  ```

- Lottery：

  ```java
  2023-05-29 00:10:43.560  INFO 27313 --- [ntainer#0-0-C-1] m.c.LotteryActivityPartakeRecordListener : 消费MQ消息，异步扣减活动库存 message：{"activityId":100001,"stockCount":100,"stockSurplusCount":96,"uId":"2000000001"}
  2023-05-29 00:10:43.607  INFO 27313 --- [:20880-thread-3] c.r.l.d.s.s.draw.impl.DrawExecImpl       : 执行抽奖策略 strategyId：10001，无库存排除奖品列表ID集合 awardList：["1"]
  2023-05-29 00:10:43.642  INFO 27313 --- [:20880-thread-3] c.r.l.d.s.service.draw.AbstractDrawBase  : 执行策略抽奖完成【已中奖】，用户：2000000001 策略ID：10001 奖品ID：5 奖品名称：Book
  2023-05-29 00:10:43.947  INFO 27313 --- [:20880-thread-3] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(中奖发货单) topic：lottery_invoice bizId：2000000001 message：{"awardContent":"Code","awardId":"5","awardName":"Book","awardType":1,"orderId":1662853920529186816,"uId":"2000000001"}
  2023-05-29 00:10:43.959  INFO 27313 --- [:20880-thread-3] c.r.l.a.p.d.i.ActivityDrawProcessImpl    : MQ 消息发送成功, 中奖物品发货单: InvoiceVO{uId='2000000001', orderId=1662853920529186816, awardId='5', awardType=1, awardName='Book', awardContent='Code', shippingAddress=null, extInfo='null'}
  2023-05-29 00:10:44.026  INFO 27313 --- [ntainer#1-4-C-1] c.r.l.a.m.c.LotteryInvoiceListener       : 消费MQ消息，完成 topic：lottery_invoice bizId：2000000001 发奖结果：{"code":1,"info":"发奖成功","uId":"2000000001"}
  2023-05-29 00:10:44.045  INFO 27313 --- [:20880-thread-3] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，完成 uId：2000000001 activityId：100001 drawRes：{"awardDTO":{"activityId":100001,"awardContent":"Code","awardId":"5","awardName":"Book","awardType":1,"userId":"2000000001"},"code":"0000","info":"成功"}
  2023-05-29 00:11:29.864  INFO 27313 --- [:20880-thread-4] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，开始 uId：2000000001 activityId：100001
  2023-05-29 00:11:30.085  INFO 27313 --- [:20880-thread-4] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(领取活动记录) topic：lottery_activity_partake bizId：2000000001 message：{"activityId":100001,"stockCount":100,"stockSurplusCount":98,"uId":"2000000001"}
  2023-05-29 00:11:30.114  INFO 27313 --- [ntainer#0-0-C-1] m.c.LotteryActivityPartakeRecordListener : 消费MQ消息，异步扣减活动库存 message：{"activityId":100001,"stockCount":100,"stockSurplusCount":98,"uId":"2000000001"}
  2023-05-29 00:11:30.158  INFO 27313 --- [:20880-thread-4] c.r.l.d.s.s.draw.impl.DrawExecImpl       : 执行抽奖策略 strategyId：10001，无库存排除奖品列表ID集合 awardList：["1"]
  2023-05-29 00:11:30.185  INFO 27313 --- [:20880-thread-4] c.r.l.d.s.service.draw.AbstractDrawBase  : 执行策略抽奖完成【已中奖】，用户：2000000001 策略ID：10001 奖品ID：4 奖品名称：AirPods
  2023-05-29 00:11:30.204  INFO 27313 --- [:20880-thread-4] c.r.l.a.mq.producer.KafkaProducer        : 发送MQ消息(中奖发货单) topic：lottery_invoice bizId：2000000001 message：{"awardContent":"Code","awardId":"4","awardName":"AirPods","awardType":1,"orderId":1662854115740483584,"uId":"2000000001"}
  2023-05-29 00:11:30.205  INFO 27313 --- [:20880-thread-4] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，完成 uId：2000000001 activityId：100001 drawRes：{"awardDTO":{"activityId":100001,"awardContent":"Code","awardId":"4","awardName":"AirPods","awardType":1,"userId":"2000000001"},"code":"0000","info":"成功"}
  2023-05-29 00:11:30.209  INFO 27313 --- [ad | producer-1] c.r.l.a.p.d.i.ActivityDrawProcessImpl    : MQ 消息发送成功, 中奖物品发货单: InvoiceVO{uId='2000000001', orderId=1662854115740483584, awardId='4', awardType=1, awardName='AirPods', awardContent='Code', shippingAddress=null, extInfo='null'}
  2023-05-29 00:11:30.244  INFO 27313 --- [ntainer#1-4-C-1] c.r.l.a.m.c.LotteryInvoiceListener       : 消费MQ消息，完成 topic：lottery_invoice bizId：2000000001 发奖结果：{"code":1,"info":"发奖成功","uId":"2000000001"}
  2023-05-29 00:11:58.139  INFO 27313 --- [:20880-thread-5] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，开始 uId：2000000001 activityId：100001
  2023-05-29 00:11:58.300  WARN 27313 --- [:20880-thread-5] c.r.l.d.a.s.p.impl.ActivityPartakeImpl   : 个人领取次数非可用 userTakeLeftCount：0
  2023-05-29 00:11:58.300 ERROR 27313 --- [:20880-thread-5] c.r.l.i.facade.LotteryActivityBooth      : 抽奖，失败(抽奖过程异常) uId：2000000001 activityId：100001
  ```

完美走通所有链路。

## 七、问题

在测试过程中，老是显示 Redis 超时，这是因为 springboot 2.x 默认采用了 lettuce 作为连接池，但是 lettuce 是不会进行“心跳”操作的，也就是说，它不会保持连接，导致了连接超时。

所以这里将连接池改为了 jedis，在 lottery-domain 的 pom 文件中排除 lettuce，改为 jedis：

```xml
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-redis -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>redis.clients</groupId>
                    <artifactId>jedis</artifactId>
                </exclusion>
                <exclusion>
                    <artifactId>lettuce-core</artifactId>
                    <groupId>io.lettuce</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
```

yml 文件的 redis 配置：

```yml
  redis:
    database: 0
    host: 175.24.165.212      # Redis服务器地址，修改为你的地址
    port: 6379              # Redis服务器连接端口
    password: 123456          # Redis服务器连接密码（默认为空）
    timeout: 5000           # Redis服务器链接超时配置
    jedis:
      pool:
        max-active: 8
        max-wait: 2000
        max-idle: 8
        min-idle: 0
        time-between-eviction-runs: 1000
```











