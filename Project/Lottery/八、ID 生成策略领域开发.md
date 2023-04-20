## 1. 开发前准备

使用策略模式把三种生成ID的算法进行统一包装，由调用方决定使用哪种生成ID的策略。策略模式属于行为模式的一种，一个类的行为或算法可以在运行时进行更改。

雪花算法本章节使用的是工具包 hutool 包装好的工具类，一般在实际使用雪花算法时需要做一些优化处理，比如支持时间回拨、支持手工插入、简短生成长度、提升生成速度等。

而日期拼接和随机数工具包生成方式，都需要自己保证唯一性，一般使用此方式生成的ID，都用在单表中，本身可以在数据库配置唯一ID。那为什么不用自增ID，因为自增ID通常容易被外界知晓你的运营数据，以及后续需要做数据迁移到分库分表中都会有些麻烦。

在 domain 领域包下新增支撑领域，ID 的生成服务就放到这个领域下实现。

关于 ID 的生成因为有三种不同 ID 用于在不同的场景下；

- 订单号：唯一、大量、订单创建时使用、分库分表
- 活动号：唯一、少量、活动创建时使用、单库单表
- 策略号：唯一、少量、活动创建时使用、单库单表

## 2. 策略模式实现

通过策略模式的使用，来开发策略 ID 的服务提供。之所以使用策略模式，是因为外部的调用方会需要根据不同的场景来选择出适合的 ID 生成策略，而策略模式就非常适合这一场景的使用。

### 2.1 工程结构

```
lottery-domain
└── src
    └── main
        └── java
            └── cn.itedus.lottery.domain.support.ids
                ├── policy
                │   ├── RandomNumeric.java
                │   ├── ShortCode.java
                │   └── SnowFlake.java
                ├── IdContext.java
                └── IIdGenerator.java
```

- IIdGenerator，定义生成 ID 的策略接口。RandomNumeric、ShortCode、SnowFlake，是三种生成 ID 的策略。
- IdContext，ID 生成上下文，也就是从这里提供策略配置服务。

### 2.2 IIdGenerator 策略接口

```java
public interface IIdGenerator {

    /**
     * 获取ID，目前有两种实现方式
     * 1. 雪花算法，用于生成单号
     * 2. 日期算法，用于生成活动编号类，特性是生成数字串较短，但指定时间内不能生成太多
     * 3. 随机算法，用于生成策略ID
     *
     * @return ID
     */
    long nextId();

}
```

### 2.3 策略ID实现

```java
@Component
public class SnowFlake implements IIdGenerator {

    private Snowflake snowflake;

    @PostConstruct
    public void init() {
        // 0 ~ 31 位，可以采用配置的方式使用
        long workerId;
        try {
            workerId = NetUtil.ipv4ToLong(NetUtil.getLocalhostStr());
        } catch (Exception e) {
            workerId = NetUtil.getLocalhostStr().hashCode();
        }

        workerId = workerId >> 16 & 31;

        long dataCenterId = 1L;
        snowflake = IdUtil.createSnowflake(workerId, dataCenterId);
    }

    @Override
    public synchronized long nextId() {
        return snowflake.nextId();
    }

}
```

- 使用 hutool 工具类提供的雪花算法，提供生成 ID 服务
- 其他方式的 ID 生成可以直接参考源码

### 2.4 策略服务上下文

```java
@Configuration
public class IdContext {

    /**
     * 创建 ID 生成策略对象，属于策略设计模式的使用方式
     *
     * @param snowFlake 雪花算法，长码，大量
     * @param shortCode 日期算法，短码，少量，全局唯一需要自己保证
     * @param randomNumeric 随机算法，短码，大量，全局唯一需要自己保证
     * @return IIdGenerator 实现类
     */
    @Bean
    public Map<Constants.Ids, IIdGenerator> idGenerator(SnowFlake snowFlake, ShortCode shortCode, RandomNumeric randomNumeric) {
        Map<Constants.Ids, IIdGenerator> idGeneratorMap = new HashMap<>(8);
        idGeneratorMap.put(Constants.Ids.SnowFlake, snowFlake);
        idGeneratorMap.put(Constants.Ids.ShortCode, shortCode);
        idGeneratorMap.put(Constants.Ids.RandomNumeric, randomNumeric);
        return idGeneratorMap;
    }

}
```

- 通过配置注解 `@Configuration` 和 Bean 对象的生成 `@Bean`，来把策略生成ID服务包装到 `Map<Constants.Ids, IIdGenerator>` 对象中。

## 3. 测试验证

**cn.itedus.lottery.test.domain.SupportTest**：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SupportTest {

    private Logger logger = LoggerFactory.getLogger(SupportTest.class);

    @Resource
    private Map<Constants.Ids, IIdGenerator> idGenerator;

    @Test
    public void test_ids() {
        logger.info("雪花算法策略，生成ID：{}", idGenerator.get(Constants.Ids.SnowFlake).nextId());
        logger.info("日期算法策略，生成ID：{}", idGenerator.get(Constants.Ids.ShortCode).nextId());
        logger.info("随机算法策略，生成ID：{}", idGenerator.get(Constants.Ids.RandomNumeric).nextId());
    }

}
```

**测试结果**

```java
2023-04-11 10:54:23.321  INFO 2764 --- [           main] com.run.lottery.test.domain.SupportTest  : 雪花算法策略，生成ID：1645621285650382848
2023-04-11 10:54:23.321  INFO 2764 --- [           main] com.run.lottery.test.domain.SupportTest  : 日期算法策略，生成ID：310153087
2023-04-11 10:54:23.323  INFO 2764 --- [           main] com.run.lottery.test.domain.SupportTest  : 随机算法策略，生成ID：13168494546
```

每种 ID 策略获取到的 ID 结果都有所不同，这也是为了适合不同类型的 ID 使用，避免同样长度的ID造成混乱。比如订单ID用在活动ID生成上，就会显得有些混乱。



