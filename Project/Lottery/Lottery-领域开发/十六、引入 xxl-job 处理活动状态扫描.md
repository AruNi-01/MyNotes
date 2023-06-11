## 1. 前言

描述：引入 XXL-JOB，分布式任务调度平台，处理需要使用定时任务解决的场景。

搭建 XXL-JOB 分布式任务调度环境，这里需要在官网：https://github.com/xuxueli/xxl-job/ 下载运行包，按照 Java SpringBoot 修改一些基本配置，项目启动即可。

配置 XXL-JOB 的基础使用环境，导入库表、配置文件、验证官网管理，测试任务启动运行

解决第一个分布式任务场景问题，扫描抽奖活动状态，把审核通过的活动扫描为活动中，把已过期活动中的状态扫描为关闭。后续章节我们还会使用分布式任务调度系统解决其他场景问题。

## 2. 搭建分布式任务调度环境（XXL-JOB）

XXL-JOB 是一个分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展。现已开放源代码并接入多家公司线上产品线，开箱即用。

功能：

1、简单：支持通过Web页面对任务进行CRUD操作，操作简单，一分钟上手；

2、动态：支持动态修改任务状态、启动/停止任务，以及终止运行中任务，即时生效；

3、调度中心HA（中心式）：调度采用中心式设计，“调度中心”自研调度组件并支持集群部署，可保证调度中心HA；

4、执行器HA（分布式）：任务分布式执行，任务"执行器"支持集群部署，可保证任务执行HA；

5、注册中心: 执行器会周期性自动注册任务, 调度中心将会自动发现注册的任务并触发执行。同时，也支持手动录入执行器地址；

。。。。。。等等 30+ 。。。。。。

### 2.1 下载 xxl-job

1. 下载：[2.3.0](https://github.com/xuxueli/xxl-job/releases/tag/2.3.0)
2. 打开：使用 IDEA 打开下载的 xxl-job，该项目由 Java 编写而成，直接在 IDEA 中运行即可
3. 导表：把 xxl-job 中的 `doc\db\tables_xxl_job.sql` 导入到自己的数据库中
4. 启动：xxl-job-admin 是用于管理分布式任务调度的后台，一切配置完后，启动 xxl-job-admin。另外你需要配 `application.properties` 修改数据库链接参数和日志文件夹
5. 案例：xxl-job-executor-samples 是一组 job 任务案例，运行后可以在分布式任务调度后台管理任务，配置、启动、关闭
6. 核心：xxl-job-core

### 2.2 快速入门

https://www.xuxueli.com/xxl-job/#%E4%BA%8C%E3%80%81%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8



## 3. 任务扫描活动状态

### 1. 引入POM

```xml
<!-- xxl-job-core https://github.com/xuxueli/xxl-job/-->
<dependency>
    <groupId>com.xuxueli</groupId>
    <artifactId>xxl-job-core</artifactId>
    <version>2.3.0</version>
</dependency>
```

- 把需要使用 xxl-job 的包，引入对应的 POM 配置

### 2. 配置 application.yml

```java
# xxl-job
# 官网：https://github.com/xuxueli/xxl-job/
# 地址：http://localhost:8081/xxl-job-admin 【需要先启动 xxl-job】
# 账号：admin
# 密码：123456
xxl:
  job:
    admin:
      addresses: http://127.0.0.1:8081/xxl-job-admin
    executor:
      address:
      appname: lottery-job
      ip:
      port: 9999
      logpath: /Users/aarynlu/MyLottery/data/applogs/xxl-job/jobhandler
      logretentiondays: 50
    accessToken:
```

- 配置中主要包括你的服务地址，应用名称、日志路径等

### 3. 任务初始类

**cn.itedus.lottery.application.worker.LotteryXxlJobConfig**

```java
@Configuration
public class LotteryXxlJobConfig {

    private Logger logger = LoggerFactory.getLogger(LotteryXxlJobConfig.class);

    @Value("${xxl.job.admin.addresses}")
    private String adminAddresses;

    @Value("${xxl.job.accessToken}")
    private String accessToken;

    @Value("${xxl.job.executor.appname}")
    private String appname;

    @Value("${xxl.job.executor.address}")
    private String address;

    @Value("${xxl.job.executor.ip}")
    private String ip;

    @Value("${xxl.job.executor.port}")
    private int port;

    @Value("${xxl.job.executor.logpath}")
    private String logPath;

    @Value("${xxl.job.executor.logretentiondays}")
    private int logRetentionDays;

    @Bean
    public XxlJobSpringExecutor xxlJobExecutor() {
        logger.info(">>>>>>>>>>> xxl-job config init.");

        XxlJobSpringExecutor xxlJobSpringExecutor = new XxlJobSpringExecutor();
        xxlJobSpringExecutor.setAdminAddresses(adminAddresses);
        xxlJobSpringExecutor.setAppname(appname);
        xxlJobSpringExecutor.setAddress(address);
        xxlJobSpringExecutor.setIp(ip);
        xxlJobSpringExecutor.setPort(port);
        xxlJobSpringExecutor.setAccessToken(accessToken);
        xxlJobSpringExecutor.setLogPath(logPath);
        xxlJobSpringExecutor.setLogRetentionDays(logRetentionDays);

        return xxlJobSpringExecutor;
    }

    /**********************************************************************************************
     * 针对多网卡、容器内部署等情况，可借助 "spring-cloud-commons" 提供的 "InetUtils" 组件灵活定制注册IP；
     *
     *      1、引入依赖：
     *          <dependency>
     *             <groupId>org.springframework.cloud</groupId>
     *             <artifactId>spring-cloud-commons</artifactId>
     *             <version>${version}</version>
     *         </dependency>
     *
     *      2、配置文件，或者容器启动变量
     *          spring.cloud.inetutils.preferred-networks: 'xxx.xxx.xxx.'
     *
     *      3、获取IP
     *          String ip_ = inetUtils.findFirstNonLoopbackHostInfo().getIpAddress();
     **********************************************************************************************/

}
```

- 这里需要启动一个任务执行器，通过配置 @Bean 对象的方式交给 Spring 进行管理。

### 4. 开发活动扫描任务

**com.run.lottery.application.worker.LotteryXxlJob**

```java
@Component
public class LotteryXxlJob {

    private Logger logger = LoggerFactory.getLogger(LotteryXxlJob.class);

    @Resource
    private IActivityDeploy activityDeploy;

    @Resource
    private IStateHandler stateHandler;

    @XxlJob("lotteryActivityStateJobHandler")
    public void lotteryActivityStateJobHandler() throws Exception {
        logger.info("扫描活动状态 Begin");

        List<ActivityVO> activityVOList = activityDeploy.scanToDoActivityList(0L);
        if (activityVOList.isEmpty()) {
            logger.info("扫描活动状态 End. 暂无符合需要扫描的活动列表");
            return;
        }

        // 由于数据库每次只扫描 10 条数据，所以需要进行循环扫描
        while (!activityVOList.isEmpty()) {
            for (ActivityVO activityVO : activityVOList) {
                Integer state = activityVO.getState();
                switch (state) {
                    // 活动状态为审核通过，在临近活动开启时间前，审核活动为活动中。在使用活动的时候，需要依照活动状态核时间两个字段进行判断和使用。
                    case 4:
                        Result state4Result = stateHandler.doing(activityVO.getActivityId(), Constants.ActivityState.PASS);
                        logger.info("扫描活动状态为活动中 结果：{} activityId：{} activityName：{} creator：{}", JSON.toJSONString(state4Result), activityVO.getActivityId(), activityVO.getActivityName(), activityVO.getCreator());
                        break;
                    // 扫描时间已过期的活动，从活动中状态变更为关闭状态【这里也可以细化为2个任务来处理，也可以把时间判断放到数据库中操作】
                    case 5:
                        if (activityVO.getEndDateTime().before(new Date())){
                            Result state5Result = stateHandler.close(activityVO.getActivityId(), Constants.ActivityState.DOING);
                            logger.info("扫描活动状态为关闭 结果：{} activityId：{} activityName：{} creator：{}", JSON.toJSONString(state5Result), activityVO.getActivityId(), activityVO.getActivityName(), activityVO.getCreator());
                        }
                        break;
                    default:
                        break;
                }
            }

            // 获取集合中最后一条记录，继续扫描后面10条记录
            ActivityVO activityVO = activityVOList.get(activityVOList.size() - 1);
            activityVOList = activityDeploy.scanToDoActivityList(activityVO.getId());
        }

        logger.info("扫描活动状态 End");
    }

}
```

在任务扫描中，主要把已经审核通过的活动和已过期的活动中状态进行变更操作；

- 审核通过 -> 扫描为活动中
- 活动中已过期时间 -> 扫描为活动关闭

### 5. 配置抽奖系统任务调度执行器

![image-20230511222053380](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305112220817.png)

- 只有配置了任务执行器，才能执行当前这个实例中的任务。

- 另外在有些业务体量较大的场景中，需要把任务开发为新工程并单独部署。

### 6. 配置任务

这里我们把已经开发了的任务 `LotteryXxlJob#lotteryActivityStateJobHandler` 配置到任务调度中心，如下：

![image-20230511222132807](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305112221428.png)

配置完成后，就可以启动任务了。



## 4. 测试

**准备数据**

- 确保数据库中有可以扫描的活动数据，比如可以把活动数据从活动中扫描为结束，也就是把状态5变更为7

启动任务，就会按照你指定的规则进行改任务的调度，我们这里就让他执行一次测试，输出：

```java
2023-05-11 22:16:15.186  INFO 89496 --- [Pool-1300584958] c.xxl.job.core.executor.XxlJobExecutor   : >>>>>>>>>>> xxl-job regist JobThread success, jobId:4, handler:com.xxl.job.core.handler.impl.MethodJobHandler@208f0007[class com.run.lottery.application.worker.LotteryXxlJob#lotteryActivityStateJobHandler]
2023-05-11 22:16:15.191  INFO 89496 --- [4-1683814575186] c.r.l.application.worker.LotteryXxlJob   : 扫描活动状态 Begin
2023-05-11 22:16:16.239  INFO 89496 --- [4-1683814575186] c.r.l.application.worker.LotteryXxlJob   : 扫描活动状态为关闭 结果：{"code":"0000","info":"活动关闭成功"} activityId：100001 activityName：活动名 creator：null
2023-05-11 22:16:16.264  INFO 89496 --- [4-1683814575186] c.r.l.application.worker.LotteryXxlJob   : 扫描活动状态为关闭 结果：{"code":"0000","info":"活动关闭成功"} activityId：100002 activityName：活动名02 creator：null
2023-05-11 22:16:16.288  INFO 89496 --- [4-1683814575186] c.r.l.application.worker.LotteryXxlJob   : 扫描活动状态 End
```

此时调度器就执行了一次该任务，把活动状态为 5 的已过期的活动，扫描为关闭了。

下一节我们会继续开发分布式任务调度，完成发奖数据MQ补偿处理。





