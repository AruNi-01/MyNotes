当基础的工程模块创建完成以后，接着就是填充一些简单的接口，跑通整个掉用过程。

这个过程包括一个简单的 RPC 接口功能实现和测试调用，那么这里为了让功能体现出一个完整度，还会创建出一个库表在 RPC 调用的时候查询出库表中的数据并返回结果。

## 1. 创建抽奖活动表

在抽奖活动的设计和开发过程中，会涉及到的表信息包括：活动表、奖品表、策略表、规则表、用户参与表、中奖信息表等，这些都会在我们随着开发抽奖的过程中不断的添加出来这些表的创建。

那么目前我们为了先把程序跑通，可以先简单的创建出一个活动表，用于实现系统对数据库的 CRUD 操作，也就可以被 RPC 接口调用。

活动表是一个用于配置抽奖活动的总表，用于存放活动信息，包括：ID、名称、描述、时间、库存、参与次数等。

活动表（activity）建表语句如下：

```sql
CREATE TABLE `activity` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '自增ID',
  `activity_id` bigint(20) NOT NULL COMMENT '活动ID',
  `activity_name` varchar(64) CHARACTER SET utf8mb4 DEFAULT NULL COMMENT '活动名称',
  `activity_desc` varchar(128) CHARACTER SET utf8mb4 DEFAULT NULL COMMENT '活动描述',
  `begin_date_time` datetime DEFAULT NULL COMMENT '开始时间',
  `end_date_time` datetime DEFAULT NULL COMMENT '结束时间',
  `stock_count` int(11) DEFAULT NULL COMMENT '库存',
  `take_count` int(11) DEFAULT NULL COMMENT '每人可参与次数',
  `state` tinyint(2) DEFAULT NULL COMMENT '活动状态：1编辑、2提审、3撤审、4通过、5运行(审核通过后worker扫描状态)、6拒绝、7关闭、8开启',
  `creator` varchar(64) CHARACTER SET utf8mb4 DEFAULT NULL COMMENT '创建人',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_activity_id` (`activity_id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin COMMENT='活动配置';
```

## 2. POM 文件配置

按照现有工程的结构模块分层，包括：

- lottery-application，应用层，引用：`domain`
- lottery-common，通用包，引用：`无`
- lottery-domain，领域层，引用：`infrastructure`
- lottery-infrastructure，基础层，引用：`无`
- lottery-interfaces，接口层，引用：`application`、`rpc`
- lottery-rpc，RPC 接口定义层，引用：`common`

**在此分层结构和依赖引用下，各层级模块不能循环依赖**，同时 `lottery-interfaces` 作为系统的 war 包工程，在构建工程时候需要依赖于 POM 中配置的相关信息。

这里需要注意下，作为 Lottery 工程下的主 `pom.xml` 需要完成对 SpringBoot 父文件的依赖，此外还需要定义一些用于其他模块可以引入的配置信息，比如：JDK 版本、编码方式等。而其他层在依赖于工程总 `pom.xml` 后还需要配置自己的信息。

### 2.1 父工程下的 POM 配置

相对于工程下其他的模块来说，总 POM 就是这些模块的父类模块，在父类模块中一般只提供基础的定义，不提供各个Jar包的引入配置。

如果在父类 POM 中引入了所有的 Jar，那么各个模块无论是否需要这个 Jar 都会被自动引入进去，造成没必要的配置，也会影响对核心 Jar 的扰乱。

父工程的 `pom.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.run.lottery</groupId>
    <artifactId>MyLottery</artifactId>
    <!-- 父工程的 packaging 必须为 pom -->
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>

    <!-- 子模块 -->
    <modules>
        <module>lottery-application</module>
        <module>lottery-common</module>
        <module>lottery-domain</module>
        <module>lottery-infrastructure</module>
        <module>lottery-interfaces</module>
        <module>lottery-rpc</module>
    </modules>

    <!-- 定义一些用于其他模块可以引入的配置信息 -->
    <properties>
        <!-- Base -->
        <jdk.version>1.8</jdk.version>
        <sourceEncoding>UTF-8</sourceEncoding>
    </properties>

    <!-- 对 SpringBoot 父文件的依赖 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.20</version>
                <configuration>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

### 2.2 各子模块的 POM 配置

在各个模块配置中需要关注点包括：依赖父 POM 配置 `parent`、构建包类型 `packaging`、需要引入的包 `dependencies`、构建信息 `build`，之所以要配置这个是有些时候在这个模块工程下还可能会有一些差异化信息的引入。

比如，RPC 层需要使用到通用包 common，那么就需要引入该包：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.run.lottery</groupId>
        <artifactId>MyLottery</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>lottery-rpc</artifactId>
    
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.run.lottery</groupId>
            <artifactId>lottery-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>lottery-rpc</finalName>
        <plugins>
            <!-- 编译plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${jdk.version}</source>
                    <target>${jdk.version}</target>
                    <compilerVersion>1.8</compilerVersion>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

### 2.3 interfaces 的 War 包配置

ps：JAR 包是类的归档文件，通常用于调用一些封装好的 API 接口或组件库；而 War 包是 JavaWeb 程序打的包，可以理解为一个 Web 项目，里面有项目中的所有东西。

**lottery-interfaces 是整个程序的出口**，也是用于构建 War 包的工程模块，所以需要打包成 War 包，它的 `pom.xml` 如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.run.lottery</groupId>
        <artifactId>MyLottery</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>lottery-interfaces</artifactId>

    <packaging>war</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.22</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.58</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.8</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo</artifactId>
            <version>2.7.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.1</version>
        </dependency>
        <dependency>
            <groupId>com.run.lottery</groupId>
            <artifactId>lottery-infrastructure</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.run.lottery</groupId>
            <artifactId>lottery-rpc</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>Lottery</finalName>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
                <includes>
                    <include>**/**</include>
                </includes>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <directory>src/test/resources</directory>
                <filtering>true</filtering>
                <includes>
                    <include>**/**</include>
                </includes>
            </testResource>
        </testResources>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## 3. interfaces 层配置

### 3.1 MyBatis 相关

SpringBoot 与 Mybatis 的结合使用还是非常方便的，暂时我们也还没有引入分库分表路由的组件，所以只是一个引入操作数据库的简单配置。

关于 MyBatis 的 starter 我们上面以及引入了，在 SpringBoot 的使用中，你会看到各种 **xxx-starter**，它们这些组件的包装都是 **用于把一些服务交给 SpringBoot 启动时候初始化或者加载配置等操作的桥梁**。

在 `resources.application.yml` 配置文件中，配置 Spring.datasource 数据源和 MyBatis 文件的引入配置：

```yml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://127.0.0.1:3306/lottery?useUnicode=true&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver

mybatis:
  mapper-locations: classpath:/mybatis/mapper/*.xml
  config-location: classpath:/mybatis/config/mybatis-config.xml
```

### 3.2 Dubbo 相关

首先说说为什么要配置广播模式的 Dubbo，其实最早 RPC 的设计和使用都是依赖于 **注册中心**，那就是需要把服务接口信息在程序启动的时候，推送到一个统一的注册中心，在其他需要调用 RPC 接口的服务上，**通过注册中心的均衡算法来匹配可以连接的接口落到本地保存和更新**。

那么这样的标准的使用方式可以提供更大的连接数和更强的负载均衡作用，但目前我们这个以学习实践为目的的工程开发则需要尽可能减少学习成本，也就需要在开发阶段少一些引入一些额外的配置，那么目前使用广播模式就非常简单适合，以后也可以直接把 Dubbo 配置成注册中心模式。

在 `resources.application.yml` 配置文件中，引入 Dubbo 的配置：

```yml
# Dubbo 广播方式配置
dubbo:
  application:
    name: Lottery
    version: 1.0.0
  # 注册地址
  registry:
    address: N/A #multicast://224.5.6.7:1234
  # 通信协议和端口
  protocol:
    name: dubbo
    port: 20880
  # 相当于 Spring 中自动扫描包的地址，可以把此包下的所有 rpc 接口都注册到服务中
  scan:
    base-packages: com.run.lottery.rpc

```

广播模式的配置唯一区别在于注册地址，`registry.address = multicast://224.5.6.7:1234`，服务提供者和服务调用者都需要配置相同的广播地址，或者配置为 N/A 用于直连模式使用；

## 4. 定义和开发 RPC 接口

由于 **RPC** 接口在通信的过程中，需要提供接口的描述文件，也就是 **接口的定义信息**。所以这里你会看到我们会 **把所有的 RPC 接口定义都放到 `lottery-rpc` 模块下**，而 **具体的实现则在 `lottery-interfaces` 中**。

这种方式的使用让外部就只依赖这样一个 pom 配置引入的 Jar 包即可。

注意，一些基本的 DTO、Req、Res 对象就不写出来了。

为了简便本次测试，我们在 `lottery-rpc` 中定义一个抽奖活动展台的接口 IActivityBooth，里面一些与活动展台相关的方法：

```java
public interface IActivityBooth {

    ActivityRes queryActivityById(ActivityReq req);

}
```

接着，到 `lottery-interfaces` 中去实现该接口，由于需要从数据库中取数据，所以需要先在 `lottery-infrastructure` 中定义好 Dao：

![image-20230405145403777](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051454335.png)

到 `lottery-interfaces` 中去定义对于的 `mapper.xml` 文件，编写 SQL 语句，以及实现 IActivityBooth 接口：

![image-20230405145549093](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051455210.png)

IActivityBooth 接口的实现类 ActivityBooth 的实现逻辑如下：

```java
/**
 * @desc: 活动展台的接口的实现类
 * @author: AruNi_Lu
 * @date: 2023/4/5
 */
// 注意：使用 Dubbo 的注解，而不是 Spring 的，将此类交给 Dubbo 管理
@Service
public class ActivityBooth implements IActivityBooth {

    @Resource
    private IActivityDao activityDao;

    @Override
    public ActivityRes queryActivityById(ActivityReq req) {
        Activity activity = activityDao.queryActivityById(req.getActivityId());

        ActivityDto activityDto = new ActivityDto();
        activityDto.setActivityId(activity.getActivityId());
        activityDto.setActivityName(activity.getActivityName());
        activityDto.setActivityDesc(activity.getActivityDesc());
        activityDto.setBeginDateTime(activity.getBeginDateTime());
        activityDto.setEndDateTime(activity.getEndDateTime());
        activityDto.setStockCount(activity.getStockCount());
        activityDto.setTakeCount(activity.getTakeCount());

        ActivityRes activityRes = new ActivityRes(new Result(Constants.ResponseCode.SUCCESS.getCode(),
                Constants.ResponseCode.SUCCESS.getInfo()), activityDto);
        return activityRes;
    }

}
```

到此，基本的接口就定义好了，主要涉及到的模块有：

![image-20230405151712399](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051517086.png)

## 5. 搭建测试工程调用 RPC

测试之前需要把 Lottery 工程中的 lottery-rpc 进行构建打包，便于测试工程引入。

![image-20230405152047920](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051520962.png)

新建 `lottery-test` 单独的 Maven 模块引入相关依赖，例如 SpringBoot、Dubbo，还有我们自己的 lottery-rpc 等等。

接着在 `resources.application.yml` 中进行配置，这里的配置与 Dubbo 接口提供者是一样的，都需要在一个广播地址下使用：

```yml
server:
  port: 8081

# Dubbo 广播方式配置
dubbo:
  application:
    name: Lottery
    version: 1.0.0
  registry:
    address: N/A #multicast://224.5.6.7:1234
  protocol:
    name: dubbo
    port: 20880

```

单元测试类：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ApiTest {

    private Logger logger = LoggerFactory.getLogger(ApiTest.class);

    // IActivityBooth 是 RPC 接口，通过 Dubbo 的注解 @Reference 进行注入配置
    @Reference(interfaceClass = IActivityBooth.class, url = "dubbo://127.0.0.1:20880")
    private IActivityBooth activityBooth;

    @Test
    public void test_rpc() {
        ActivityReq req = new ActivityReq();
        req.setActivityId(100001L);
        ActivityRes result = activityBooth.queryActivityById(req);
        logger.info("测试结果：{}", JSON.toJSONString(result));
    }

}
```

- 如果广播模式不太稳定，可以在 @Reference 注解的 url 属性指定直连模式。后续真正的业务使用一般都是使用注册中心模式，此案例中的模式主要是为了更简单的方式学习验证。

先启动 `lottery-interfaces` 中的应用，然后执行测试方法，测试结果：

```java
2023-04-05 16:12:40.951  INFO 1368 --- [           main] com.run.lottery.test.ApiTest             : 测试结果：{"activity":{"activityDesc":"AruNi 的活动","activityId":100001,"activityName":"测试活动","beginDateTime":1680682261000,"endDateTime":1680768671000,"stockCount":5,"takeCount":1},"result":{"code":"0000","info":"成功"}}
2023-04-05 16:12:40.961  INFO 1368 --- [extShutdownHook] .b.c.e.AwaitingNonWebApplicationListener :  [Dubbo] Current Spring Boot Application is about to shutdown...
```

通过测试结果可以看到，目前通过 RPC 调用已经可以查询到分布式系统提供的数据库操作功能。

整个调用流程如下所示：

![image-20230405171213314](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051712545.png)

