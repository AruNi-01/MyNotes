# 1. Spring Cloud Alibaba 概览

## 1.1 Spring Cloud Alibaba 是什么

Spring Cloud Alibaba 简称 SCA，官网：https://sca.aliyun.com/zh-cn/

Spring Cloud Alibaba 致力于提供 **微服务开发的一站式解决方案**。此项目 **包含开发分布式应用服务的必需组件**，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里分布式应用解决方案，通过阿里中间件来迅速搭建分布式应用系统。

## 1.2 Spring Cloud Alibaba 的定位

![spring-cloud](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701519046.png)

Spring Cloud Alibaba 是阿里巴巴结合自身丰富的微服务实践而推出的微服务开发的一站式解决方案，是 Spring Cloud 第二代实现的主要组成部分。吸收了 Spring Cloud Netflix 微服务框架的核心架构思想，并进行了高性能改进。自 Spring Cloud Netflix 进入停更维护后，Spring Cloud Alibaba 逐渐代替它成为主流的微服务框架。

## 1.3 Spring Cloud Alibaba 有哪些组件

一个微服务全景图如下所示：

![image-20231107082716683](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311070827994.png)

其中含有 Spring Cloud Alibaba 的主要组件有：

- Nacos：服务注册与配置中心；
- Dubbo：RPC 框架；
- Sentinel：服务治理，包括服务限流、降级、熔断等；
- Seata：分布式事务解决方案；
- RocketMQ：分布式消息队列。

分层结构如下：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701519055.png)

# 2. Nacos 服务注册和配置中心

## 2.1 什么是 Nacos

[Nacos](https://nacos.io) 即 Naming and Configuration Service，是一个动态服务注册与发现、配置管理和服务管理的平台。

Nacos 的关键特性包括：

- **服务发现和服务健康监测**：
    - Nacos 支持基于 DNS 和基于 RPC 的服务发现。服务提供者使用 [原生SDK](https://nacos.io/zh-cn/docs/sdk.html)、[OpenAPI](https://nacos.io/zh-cn/docs/open-api.html)、或一个[独立的Agent TODO](https://nacos.io/zh-cn/docs/other-language.html)注册 Service 后，服务消费者可以使用[DNS TODO](https://nacos.io/zh-cn/docs/xx) 或[HTTP&API](https://nacos.io/zh-cn/docs/open-api.html)查找和发现服务。
    - Nacos 提供对服务的实时的健康检查，阻止向不健康的主机或服务实例发送请求。还提供了统一的健康检查仪表盘，帮助您根据健康状态管理服务的可用性及流量。
- **动态配置服务**：
    - 动态配置服务可以让您以中心化、外部化和动态化的方式管理所有环境的应用配置和服务配置。
    - 动态配置消除了配置变更时重新部署应用和服务的需要，让配置管理变得更加高效和敏捷。
    - Nacos 提供了一个简洁易用的UI ([控制台样例 Demo](http://console.nacos.io/nacos/index.html)) 帮助您管理所有的服务和应用的配置。
- **动态 DNS 服务**：
    - 动态 DNS 服务支持权重路由，让您更容易地实现中间层负载均衡、更灵活的路由策略、流量控制以及数据中心内网的简单DNS解析服务。



## 2.2 基本名词解释

Nacos 的基本架构：

![nacos_arch.jpg](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701519064.jpeg)

核心专业术语：

- **服务 (Service)**：服务是指一个或一组软件功能（例如特定信息的检索或一组操作的执行），其目的是不同的客户端可以为不同的目的重用（例如通过跨进程的网络调用）。Nacos 支持主流的服务生态，如 Kubernetes Service、gRPC|Dubbo RPC Service 或者 Spring Cloud RESTful Service。
- **服务注册中心 (Service Registry)**：服务注册中心，它是服务，其实例及元数据的数据库。服务实例在启动时注册到服务注册表，并在关闭时注销。服务和路由器的客户端查询服务注册表以查找服务的可用实例。服务注册中心可能会调用服务实例的健康检查 API 来验证它是否能够处理请求。
- **服务提供方 (Service Provider)**：是指提供可复用和可调用服务的应用方。
- **服务消费方 (Service Consumer)**：是指会发起对某个服务调用的应用方。
- **名字服务 (Naming Service)**：提供分布式系统中所有对象(Object)、实体(Entity)的“名字”到关联的元数据之间的映射管理服务，例如 ServiceName -> Endpoints Info, DNS Domain Name -> IP List, 服务发现和 DNS 就是名字服务的 2 大场景。
- **配置服务 (Configuration Service)**：在服务或者应用运行过程中，提供动态配置或者元数据以及配置管理的服务提供者。

## 2.3 安装运行 Nacos

Nacos 依赖 Java 环境，确保机器上有 JDK 1.8+ 环境。

下面提供两种方式安装 Nacos：

- 下载编译后的压缩包：从 [最新稳定版本](https://github.com/alibaba/nacos/releases) 下载 `nacos-server-$version.zip` 包，解压后执行：

    ```sh
    sh startup.sh -m standalone	# Linux
    
    startup.cmd -m standalone	# Win
    ```

- Docker 方式：

    ```sh
    # 1. 克隆并进入项目
    git clone https://github.com/nacos-group/nacos-docker.git
    cd nacos-docker
    #  2. 启动
    docker-compose -f example/standalone-derby.yaml up
    ```

启动后访问 ip:8848/naocs，即可进入管理页面，默认用户名密码都为 nacos。

## 2.4 Nacos 服务注册与发现

SCA Nacos 官网的 [快速开始](https://sca.aliyun.com/zh-cn/docs/2022.0.0.0/user-guide/nacos/quick-start#nacos-%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%8E%E5%8F%91%E7%8E%B0)。

### 2.4.1 基于 Nacos 的服务提供者

在父项目中新建子 Module `cloudalibaba-provider-payment9001`。

在父 pom 文件中添加 SCA 依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.run.sca</groupId>
    <artifactId>SpringCloudAlibaba</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <modules>
        <module>cloudalibaba-provider-payment9001</module>
    </modules>

    <!-- 同一管理 jar 包版本 -->
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <mysql.version>5.1.47</mysql.version>
        <druid.version>1.1.16</druid.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
    </properties>

    <!-- 子模块继承之后，提供作用：锁定版本+子module不用写groupId和version  -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
                <optional>true</optional>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

本模块的 pom 文件，引入 nacos：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.run.sca</groupId>
        <artifactId>SpringCloudAlibaba</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>cloudalibaba-provider-payment9001</artifactId>

    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--日常通用jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

配置 yml 文件：

```yaml
server:
  port: 9001

spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848   # 配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: '*'  # Spring Actuator 监控，将所有可用的端点都公开出来，供外部访问。

```

主启动类，加上Spring Cloud 注解 `@EnableDiscoveryClient`，启用服务注册和发现的功能：

```java
package com.run.sca;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * @desc:
 * @author: AruNi_Lu
 * @date: 2023/11/7
 */
@EnableDiscoveryClient
@SpringBootApplication
public class PaymentMain9001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain9001.class, args);
    }
}
```

业务类 controller：

```java
package com.run.sca.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

/**
 * @desc:
 * @author: AruNi_Lu
 * @date: 2023/11/7
 */
@RestController
public class PaymentController {
    
    @Value("${server.port}")
    private String serverPort;
    
    @GetMapping(value = "/payment/nacos/{id}")
    public String getPayment(@PathVariable("id") Integer id) {
        return "nacos registry serverPort: " + serverPort + "\t id: " + id;
    }
    
}
```

启动项目，服务就会自动注册到 nacos 了，nacos 控制台：

![image-20231107120940800](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071209870.png)

为了演示 nacos 负载均衡的功能，复制上面项目，端口为 9002。启动后 nacos 中的该服务实例数就变为 2 了：

![image-20231107121658863](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071217007.png)

访问以下服务接口 http://localhost:9001/payment/nacos/1001，成功返回响应：

![image-20231107121829483](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071218801.png)

### 2.4.2 基于 Nacos 的服务消费者

创建 Module 与前面类似。

yml 配置文件：

```yaml
server:
  port: 83

spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848


# 消费者将要去访问的微服务名称(注册成功进 nacos 的微服务提供者)
service-url:
  nacos-user-service: http://nacos-payment-provider   # 与提供者配置文件的服务名对应

```

Nacos 之所以能进行负载均衡的原因，是因为它集成了 Ribbon：

![image-20231107123241662](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071232797.png)

Spring 配置类，配置负载均衡：

```java
package com.run.sca.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * @desc:
 * @author: AruNi_Lu
 * @date: 2023/11/7
 */
@Configuration
public class ApplicationContextBean {

    // RestTemplate 配合 Ribbon 做负载均衡
    // 通过 RestTemplate 发送的请求将会基于负载均衡策略分发到多个实例上
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}

```

业务类 controller：

```java
package com.run.sca.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

/**
 * @desc:
 * @author: AruNi_Lu
 * @date: 2023/11/7
 */
@RestController
public class OrderController {

    @Resource
    private RestTemplate restTemplate;

    @Value("${service-url.nacos-user-service}")
    private String serverURL;

    @GetMapping(value = "/consumer/payment/nacos/{id}")
    public String getPayment(@PathVariable("id") Integer id) {
        return restTemplate.getForObject(serverURL + "/payment/nacos/" + id, String.class);
    }

}

```

启动项目，访问：http://localhost:83/consumer/payment/nacos/1002，多次访问，发现访问的 provider 端口在 9001 和 9002 中轮询，即实现了负载均衡：

![image-20231107125748977](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071257075.png)

## 2.5 Nacos 配置中心

SCA Nacos 官网的 [快速开始](https://sca.aliyun.com/zh-cn/docs/2022.0.0.0/user-guide/nacos/quick-start#nacos-%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83)。

### 2.5.1 基础配置

新建 Module `cloudalibaba-config-nacos-client3377`。

pom 文件引入 nacos 的 config 和 discovery 依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.run.sca</groupId>
        <artifactId>SpringCloudAlibaba</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>cloudalibaba-config-nacos-client3377</artifactId>

    <dependencies>
        <!--nacos-config-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
        <!--nacos-discovery-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--日常通用jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

yml 配置文件：

- Nacos 在项目初始化时，**要保证先从配置中心进行配置拉取**，拉取配置之后，才能保证项目的正常启动；
- SpringBoot 中配置文件的加载是存在优先级顺序的，`bootstrap.yml` 优先级高于 `application.yml`。

因此，我们需要先在 bootstrap 配置文件中进行项目的配置：

```yaml
# nacos配置
server:
  port: 3377

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848   # Nacos 服务注册中心地址
      config:
        server-addr: localhost:8848   # Nacos 作为配置中心地址
        file-extension: yaml  # 指定 yaml 格式的配置


# Nacos 配置规则：${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}
# 对应到该项目就是：nacos-config-client-dev.yaml

```

application 配置文件：

```yaml
spring:
  profiles:
    active: dev
    
```

业务类 controller：

- Nacos 通过 SpringCloud 原生注解 `@RefreshScope` 实现配置自动更新；

```java
package com.run.sca.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @desc:
 * @author: AruNi_Lu
 * @date: 2023/11/7
 */
@RestController
@RefreshScope   // 控制器类上加 @RefreshScope 使当前类下的配置支持 Nacos 的动态刷新
public class ConfigClientController {

    // 要从 Nacos 配置文件中获取的信息
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/config/info")
    public String getConfigInfo() {
        return configInfo;
    }

}
```

在 Nacos 中添加配置之前，需要先了解以下其配置规则，因为 **Naocs 中的 dataid 与 SpringBoot 配置文件有关**：

在 Nacos Spring Cloud 中，`dataId` 的完整格式如下：

```text
${prefix}-${spring.profiles.active}.${file-extension}
```

- `prefix` 默认为 `spring.application.name` 的值，也可以通过配置项 `spring.cloud.nacos.config.prefix `来配置。
- `spring.profiles.active` 即为当前环境对应的 profile，详情可以参考 [Spring Boot 文档](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html#boot-features-profiles)。 **注意：当 `spring.profiles.active` 为空时，对应的连接符 `-` 也将不存在，dataId 的拼接格式变成 `${prefix}.${file-extension}`**；
- `file-exetension` 为配置内容的数据格式，可以通过配置项 `spring.cloud.nacos.config.file-extension` 来配置。目前只支持 `properties` 和 `yaml` 类型。

所以根据该项目的配置，dataid 就是 `nacos-config-client-dev.yaml`。

![image-20231107133616388](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071336206.png)

了解之后，就可以到 Nacos 中新建配置了：

![image-20231107133838985](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071339508.png)

 启动项目，访问 http://localhost:3377/config/info，获取配置信息：

![image-20231107134339077](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071343059.png)

现在在 Nacos 中修改配置信息，重新访问，即可获取最新的配置信息：

![image-20231107134516242](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071345125.png)



### 2.5.2 分类配置

分类配置要解决的问题：

- 实际项目中一般会有 dev、test、prod 环境，如何确保环境启动时能正确读取到 Nacos 上对应的配置文件呢？
- 一个项目分为很多微服务，每个微服务又有对应的 dev、test、pre、prod 环境，如何对应它们各自的配置文件？

Nacos 采用 Namespace + Group + DataID 来区分：

- Namespace 可以用于区分部署环境。
- Group 和 DataID 逻辑上区分两个目标对象。

![image-20231107135519559](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071355376.png)

- **Namespace 主要用来实现隔离**。比方说我们现在有三个环境：开发、测试、生产环境，我们就可以创建三个 Namespace，不同的 Namespace 之间是隔离的；
- **Group 可以把不同的微服务划分到同一个分组里面去**；
- **Service 就是微服务**；一个 Service 可以包含多个 Cluster（集群），Cluster 是对指定微服务的一个虚拟划分。比方说为了容灾，将 Service 微服务分别部署在了杭州机房和广州机房，这时就可以给杭州机房的 Service 微服务起一个集群名称（HZ），给广州机房的 Service 微服务起一个集群名称（GZ），还可以尽量让同一个机房的微服务互相调用，以提升性能。
- **Instance 就是微服务的实例**。

默认情况：

- Namespace=public；
- Group=DEFAULT_GROUP；
- 默认Cluster 是 DEFAULT。





**DataID 方案**

通过 DataID 中的 `${spring.profile.active}` 就能切换不同部署环境的配置文件。例如 `nacos-config-client-dev.yaml`、`nacos-config-client-test.yaml`，在 Nacos 中建立对应的配置即可。

**Group 方案**：通过 Group 实现环境区分。

在新建配置时指定 Group 即可：

![image-20231107142047361](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071420766.png)

接着在 SpringBoot 配置文件中添加 `group` 配置即可：

![image-20231107142252344](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071422303.png)

**Namespace 方案**：

Nacos 新建 dev/test Namespace：

![image-20231107142417706](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071424836.png)

 此时服务列表有对应的 Namespace，下面的服务是隔离的：

![image-20231107142614750](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071426574.png)

接着按照 SpringBoot 配置文件来编写 dev Namespace 下的配置：

![image-20231107142704564](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071427160.png)

SpringBoot 配置文件中添加 `namespace`，值为 Namespace 的 ID：

![image-20231107142846082](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311071428164.png)

## 2.6 Nacos 持久化配置

Nacos 默认自带一个嵌入式数据库 derby 实现数据的存储，但不方便观察数据存储的基本情况。后续 Nacos 支持了 MySQL 数据源，切换步骤如下：

1. 安装 MySQL 数据库，版本要求：5.6.5+
2. 新建名为 nacos_config 的数据库，初始化表数据：`nacos/conf` 目录下的 `mysql-schema.sql`
3. 修改 `nacos/conf` 目录下的 `application.properties` 文件，增加支持 MySQL 数据源配置（目前只支持 MySQL ），添加 MySQL 数据源的 URL、用户名和密码。

```properties
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```

重启 Nacos，后续所有的数据都会写到 MySQL 中。

## 2.7 Nacos 集群

Nacos 官方推荐我们将所有服务列表放到一个 VIP（Virtual IP）下，然后挂到一个域名下，即 http://nacos.com:port/openAPI 域名 + SLB 模式（内网SLB，不可暴露到公网，以免带来安全风险）。这样可读性好，而且换 ip 方便。

![deployDnsVipMode.jpg](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2Fassets-2023_11_25-1700879543.jpg)

部署 Nacos 集群推荐使用 Linux 系统，并且需要 3 个或以上的 Nacos 节点才能构成集群。

### 2.7.1 Linux 中配置 Nacos 集群

**1、准备工作**

首先在 Linux 中下载配置好 JDK、Maven、Nacos，并且按照上面 Nacos 持久化配置，将 MySQL 作为数据源（Linux 操作不便可以在 PC 上连接 MySQL 操作）。

**2、集群配置 `cluster.conf`**

在目录 nacos/conf 下有配置文件 `cluster.conf`，配置集群信息：

```sh
# ip:port
200.8.9.16:8847
200.8.9.17:8848
200.8.9.18:8849
```

**3、修改 naocs 启动脚本 `startup.sh`**

启动单机版都是直接执行 `./startup.sh` 即可，但集群启动的时候，我们希望可以按不同的端口号启动，例如 `./startup.sh -p 3333` 启动 3333 端口的 nacos。所以需要修改启动脚本来支持。

编辑 `startup.sh`，需要修改三个地方：

![image-20231125140619220](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_25-1700892389.png)



启动三个 Nacos 节点：

```sh
./startup.sh -p 3333/4444/5555
```

编辑 Nginx 配置文件做代理：

```sh
upstream cluster {
  server 127.0.0.1:3333;
  server 127.0.0.1:4444;
  server 127.0.0.1:5555;
}

server {
  listen 1111;
  server_name localhost;

  location / {
    #root html;
    #index index.html;
    proxy_pass http://cluster;    # 代理到自定义的 upstream cluster
  }

  error_page 500 502 503 504 /50x.html;
  location = /50x.html {
    root html;
  }
}
```

启动 Nginx，访问 ip:1111/nacos，即可访问集群节点。

> 云服务器记得添加端口到防火墙规则。

这样在项目中，client 端配置 nacos 就只用配 port=1111 这个节点即可使用集群，实现了 Nacos 的高可用：

![image-20231125141042358](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_25-1700892646.png)

# 3. Sentinel 服务治理

## 3.1 什么是 Sentinel

Sentinel 是面向分布式的 **流量治理** 组件，主要以流量为切入点，从 **流量路由、流量控制、流量整形、熔断降级、系统自适应过载保护、热点流量防护** 等多个维度来帮助开发者 **保障微服务的稳定性**。

**流量控制**：任意时间到来的请求往往是随机不可控的，而系统的处理能力是有限的。我们需要 **根据系统的处理能力对流量进行控制**。Sentinel 作为一个调配器，可以根据需要 **把随机的请求调整成合适的 “形状”**：

![arch](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_25-1700911310.jpg)

流量控制有以下几个角度：

- 资源的调用关系，例如资源的调用链路，资源和资源之间的关系；
- 运行指标，例如 QPS、线程池、系统负载等；
- 控制的效果，例如直接限流、冷启动、排队等。

Sentinel 的设计理念是让您自由选择控制的角度，并进行灵活组合。





**熔断降级**：由于调用关系的复杂性，如果 **调用链路中的某个资源出现了不稳定（表现为 timeout，异常比例升高），最终会导致请求发生堆积**。所以这时需要 **对该资源的调用进行限制，并让请求快速失败，避免影响到其它的资源，最终产生雪崩的效果**。

![image](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_25-1700911512.png)

**系统负载保护**：防止雪崩，是系统防护中重要的一环。**当系统负载较高的时候，如果还持续让请求进入，可能会导致系统崩溃，无法响应**。在集群环境下，网络负载均衡会把本应这台机器承载的流量转发到其它的机器上去。如果这个时候 **其它的机器也处在一个边缘状态的时候，这个增加的流量就会导致这台机器也崩溃，最后导致整个集群不可用**。针对这个情况，Sentinel 提供了对应的保护机制，**让系统的入口流量和系统的负载达到一个平衡，保证系统在能力范围之内处理最多的请求**。





## 3.2 快速开始

### 3.2.1 启动 Dashboard

Sentinel 的使用分为两个部分:

- 核心库（Java 客户端）：不依赖任何框架/库，能够运行于 Java 8 及以上的版本的运行时环境。
- 控制台（Dashboard）：Dashboard 主要负责管理推送规则、监控、管理机器信息等。

下面我们先下载 Dashboard，看看 Sentinel 的管理界面长什么样子。下载地址：https://github.com/alibaba/Sentinel/releases，直接下载 jar 包运行即可：

```sh
java -jar .\sentinel-dashboard-1.8.6.jar
```

### 3.2.2 初始化工程

先把 Nacos8848 启动起来：`./startup.cmd -m standalone`。

新建 cloudalibaba-sentinel-service8401 Module，pom 文件如下：

```xml
    <dependencies>
        <!-- SpringCloud Alibaba sentinel -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <!-- SpringCloud Alibaba sentinel-datasource-nacos 后续做持久化用 -->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>

        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--日常通用jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

yaml 文件：

```yaml
server:
  port: 8401

spring:
  application:
    name: sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848   # 配置Nacos地址

    sentinel:
      transport:
        dashboard: localhost:8080   # 配置 Sentinel Dashboard 地址，
        port: 8719  # 默认 8719 端口，用于与 Sentinel Dashboard 交互

management:
  endpoints:
    web:
      exposure:
        include: '*'  # Spring Actuator 监控，将所有可用的端点都公开出来，供外部访问。

```

controller：

```java
@RestController
public class FlowLimitController {

    @GetMapping("/testA")
    public String testA() {
        return "------------testA";
    }

    @GetMapping("/testB")
    public String testB() {
        return "------------testB";
    }

}

```



启动项目，访问 Sentinel Dashboard，发现还是没应用，这是因为它采用的是懒加载，我们先去访问一下controller 层服务的接口，然后再查看 Dashboard，可以发现，Sentinel8080 正在监控微服务 sentinel-service8401：

![image-20231125230315716](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_25-1700924597.png)

![image-20231125230404273](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_25-1700924645.png)

## 3.3 流控规则

在 Dashboard 看看流控设置有哪些：

![image-20231127235111062](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_27-1701100289.png)

我们新增一个 QPS 单机阈值 = 1，流控效果快速失败，这意味着如果每秒请求量大于 1 时，后续的请求都会直接失败：

![image-20231127235333821](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_27-1701100416.png)

现在来看看流控规则中的配置项：

- **资源名**：唯一名称，默认请求路径

- **针对来源**：Sentinel 可以 **针对调用者进行限流**，填写微服务名，默认default（不区分来源)

- **阈值类型/单机阈值**：

  - QPS（每秒钟的请求数量）：当调用该 api 的 QPS 达到间值的时候，进行限流。
  - 线程数：当调用该 api 的线程数达到阈值的时候，进行限流

- **是否集群**：不需要集群

- **流控模式**：

  - 直接：api 达到限流条件时，直接限流。
  - 关联：当与之关联的资源达到阈值时，就限流自己（别人达到阈值，限流自己，比如支付接口崩了，让下单接口限流）。
  - 链路：只记录指定链路上的流量（指定资源从入口资源进来的流量，如果达到阈值，就进行限流）【api 级别的针对来源，多个 api 访问同一个其他的 api】

- **流控效果**：

  - 快速失败：直接失败，抛异常。

  - WarmUp：当阈值达到 `设定阀值 / codeFactor(默认为 3)` 时，开始在设定的预热时长时间内，逐渐升至设置的 QPS 阈值。

    > 场景：秒杀系统在开启的瞬间，突增的大量流量可能直接把系统打死，预热方式就是把为了保护系统，慢慢的把流量放进来，慢慢的把阀值增长到设置的阀值。

  - 排队等待：匀速排人，让请求以匀速的速度通过（对应漏桶算法），阈值类型必须设置为 QPS，否则无效。设置的超时时间到之后还没排到则请求失败。

    > 场景：主要用于处理间隔性突发的流量，例如消息队列。例如在某一秒有大量的请求到来，而接下来的几秒则处于空闲状态，我们希望系统能够在接下来的空闲期间逐渐处理这些请求，而不是在第一秒直接拒绝多余的请求。

## 3.4 熔断规则

现代微服务架构都是分布式的，由非常多的服务组成。不同服务之间相互调用，组成复杂的调用链路。以上的问题在链路调用中会产生放大的效果。复杂链路上的某一环不稳定，就可能会层层级联，最终导致整个链路都不可用。因此我们需要对不稳定的 **弱依赖服务调用** 进行熔断降级，暂时切断不稳定调用，避免局部不稳定因素导致整体的雪崩。熔断降级作为保护自身的手段，通常在客户端（调用端）进行配置。

例如，支付的时候，可能需要远程调用银联提供的 API；查询某个商品的价格，可能需要进行数据库查询。然而，这个被依赖服务的稳定性是不能保证的。如果依赖的服务出现了不稳定的情况，请求的响应时间变长，那么调用服务的方法的响应时间也会变长，线程会产生堆积，最终可能耗尽业务自身的线程池，服务本身也变得不可用。

![chain](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701134637.png)

Sentinel 熔断降级会在调用链路中某个 **资源出现不稳定状态时**（例如调用超时或异常比例升高），对这个资源的调用 **进行限制**，让请求 **快速失败**，**避免影响到其它的资源而导致级联错误**。当资源被降级后，在接下来的熔断降级时间内，对该资源的调用都自动熔断（默认行为是抛出 DegradeException）。

来看看 Sentinel 熔断配置都有哪些：

![image-20231128092514601](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701134715.png)

熔断降级规则（DegradeRule）包含下面几个重要的属性：

|       Field        | 说明                                                         | 默认值     |
| :----------------: | :----------------------------------------------------------- | :--------- |
|      resource      | 资源名，即规则的作用对象                                     |            |
|       grade        | 熔断策略，支持慢调用比例/异常比例/异常数策略                 | 慢调用比例 |
|       count        | 慢调用比例模式下为慢调用临界 RT（超出该值计为慢调用）；异常比例/异常数模式下为对应的阈值 |            |
|     timeWindow     | 熔断时长，单位为 s                                           |            |
|  minRequestAmount  | 熔断触发的最小请求数，请求数小于该值时即使异常比率超出阈值也不会熔断（1.7.0 引入） | 5          |
|   statIntervalMs   | 统计时长（单位为 ms），如 60*1000 代表分钟级（1.8.0 引入）   | 1000 ms    |
| slowRatioThreshold | 慢调用比例阈值，仅慢调用比例模式有效（1.8.0 引入）           |            |

熔断策略：

- **慢调用比例 (`SLOW_REQUEST_RATIO`)**：选择以慢调用比例作为阈值，需要设置允许的慢调用 RT（即最大的响应时间），**请求的响应时间大于该值则统计为慢调用**。当 **单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值**，则接下来的熔断时长内请求会自动被熔断。**经过熔断时长后熔断器会进入探测恢复状态**（HALF-OPEN 状态），若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断。
- **异常比例 (`ERROR_RATIO`)**：**当单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且异常的比例大于阈值**，则接下来的熔断时长内请求会自动被熔断。**经过熔断时长后熔断器会进入探测恢复状态**（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。异常比率的阈值范围是 `[0.0, 1.0]`，代表 0% - 100%。
- **异常数 (`ERROR_COUNT`)**：**当单位统计时长内的异常数目超过阈值之后会自动进行熔断**。**经过熔断时长后熔断器会进入探测恢复状态**（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。

## 3.5 热点参数限流

何为热点？热点即经常访问的数据。很多时候我们希望统计某个热点数据中访问频次最高的 Top K 数据，并对其访问进行限制。比如：

- 商品 ID 为参数，统计一段时间内最常购买的商品 ID 并进行限制
- 用户 ID 为参数，针对一段时间内频繁访问的用户 ID 进行限制

热点参数限流会 **统计传入参数中的热点参数**，并根据配置的限流阈值与模式，对包含热点参数的资源调用进行限流。热点参数限流可以看做是一种特殊的流量控制，**仅对包含热点参数的资源调用生效**。

![Sentinel Parameter Flow Control](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701136569.png)

Sentinel 利用 **LRU 策略** 统计最近最常访问的热点参数，结合 **令牌桶算法** 来进行参数级别的流控。

热点规则：

![image-20231128101056103](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701137457.png)

例子：

```java
    @GetMapping("/testHotParams")
    @SentinelResource(value = "testHotParams", blockHandler = "handle_testHotParams")
    public String testHotParams(@RequestParam(value = "p1", required = false) String p1,
                                @RequestParam(value = "p2", required = false) String p2) {
        return "------------testHotParams";
    }

    // 配置热点参数限流触发时的处理方法，不配置则直接将异常信息打到页面
    public String handle_testHotParams(String p1, String p2, BlockException exception) {
        return "------------handle_testHotParams(兜底处理)";
    }
```

热点规则：

![image-20231128101511019](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701137712.png)

当传入参数 idx 为 0（即 p1），1s 内 QPS 超过 1 时，就会进行限流。

**参数例外项**：参数例外项可 **额外配置该热点参数的参数值和限流阈值**，该参数值的阈值不受上面阈值的限制。我们接着上面的配置，来新增参数例外项：

![image-20231128103151607](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701138712.png)

此时当请求 testHotParams 时，当参数 idx = 0（p1）不为 "5" 时，QPS 阈值只有 1，等于 "5" 时，阈值可以到 200。

注意：@SentinelResource 只管在 Sentinel 配置的出错，运行出错该走异常走异常。

## 3.6 系统自适应保护

Sentinel 系统自适应保护从 **整体维度对应用入口流量** 进行控制，结合应用的 Load、总体平均 RT、入口 QPS 和线程数等几个维度的监控指标，让系统的入口流量和系统的负载达到一个平衡，让系统尽可能跑在最大吞吐量的同时保证系统整体的稳定性。

系统保护规则是应用整体维度的，而不是资源维度的，并且**仅对入口流量生效**。入口流量指的是进入应用的流量（`EntryType.IN`），比如 Web 服务或 Dubbo 服务端接收的请求，都属于入口流量。

系统规则支持以下的阈值类型：

- **Load**（仅对 Linux/Unix-like 机器生效）：当系统 load1（系统负载）超过阈值，且系统当前的并发线程数超过系统容量时才会触发系统保护。系统容量由系统的 `maxQps * minRt` 计算得出。设定参考值一般是 `CPU cores * 2.5`。
- **CPU usage**（1.5.0+ 版本）：当系统 CPU 使用率超过阈值即触发系统保护（取值范围 0.0-1.0）。
- **RT**：当单台机器上所有入口流量的平均 RT 达到阈值即触发系统保护，单位是毫秒。
- **线程数**：当单台机器上所有入口流量的并发线程数达到阈值即触发系统保护。
- **入口 QPS**：当单台机器上所有入口流量的 QPS 达到阈值即触发系统保护。

## 3.7 @SentinelResource

Sentinel 提供了 `@SentinelResource` 注解用于 **定义资源**，并提供了 AspectJ 的扩展用于 **自动定义资源、处理 `BlockException` 等**。

> **注意**：注解方式埋点不支持 private 方法。

`@SentinelResource` 用于定义资源，并提供可选的异常处理和 fallback 配置项。 `@SentinelResource` 注解包含以下属性：

- `value`：资源名称，必需项（不能为空），Dashboard 中可以配置资源名，也可以配置 URL。
- `entryType`：entry 类型，可选项（默认为 `EntryType.OUT`）
- `blockHandler` / `blockHandlerClass`：`blockHandler` 对应处理 `BlockException` 的函数名称，可选项。
  - blockHandler 函数访问范围需要是 `public`，返回类型需要与原方法相匹配，参数类型需要和原方法相匹配并且最后加一个额外的参数，类型为 `BlockException`。
  - blockHandler 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `blockHandlerClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。
- `fallback` / `fallbackClass`：fallback 函数名称，可选项，用于在抛出异常的时候提供 fallback 处理逻辑。fallback 函数可以针对所有类型的异常（除了 `exceptionsToIgnore` 里面排除掉的异常类型）进行处理。fallback 函数签名和位置要求：
  - 返回值类型必须与原函数返回值类型一致；
  - 方法参数列表需要和原函数一致，或者可以额外多一个 `Throwable` 类型的参数用于接收对应的异常。
  - fallback 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `fallbackClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。
- `defaultFallback`（since 1.6.0）：默认的 fallback 函数名称，可选项，通常用于通用的 fallback 逻辑（即可以用于很多服务或方法）。默认 fallback 函数可以针对所以类型的异常（除 `exceptionsToIgnore` 里面排除掉的异常类型）进行处理。若同时配置了 fallback 和 defaultFallback，则只有 fallback 会生效。defaultFallback 函数签名要求：
  - 返回值类型必须与原函数返回值类型一致；
  - 方法参数列表需要为空，或者可以额外多一个 `Throwable` 类型的参数用于接收对应的异常。
  - `defaultFallback` 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `fallbackClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。
- `exceptionsToIgnore`（since 1.6.0）：用于指定哪些异常被排除掉，不会计入异常统计中，也不会进入 fallback 逻辑中，而是会原样抛出。

> 注：1.6.0 之前的版本 fallback 函数只针对降级异常（`DegradeException`）进行处理，**不能针对业务异常进行处理**。

`blockHandler` 和 `fallback`：

- **`blockHandler`** 是专门(只)处理 sentinel **流控降级规则抛出的 BlockException**，**其他异常均会直接抛出**，不会进入blockHandler方法处理。

- **`fallback` 默认处理所有的异常**，其中包括 BlockEcxeption（BlockException 是 Exception 的子类）。

- 特别地，**若 blockHandler 和 fallback 都进行了配置**，则被限流降级而抛出 `BlockException` 时 **只会进入 `blockHandler` 处理逻辑**。若未配置 `blockHandler`、`fallback` 和 `defaultFallback`，则被限流降级时会将 `BlockException` **直接抛出**。

示例：

```java
public class TestService {

    // 对应的 `handleException` 函数需要位于 `ExceptionUtil` 类中，并且必须为 static 函数.
    @SentinelResource(value = "test", blockHandler = "handleException", blockHandlerClass = {ExceptionUtil.class})
    public void test() {
        System.out.println("Test");
    }

    // 原函数
    @SentinelResource(value = "hello", blockHandler = "exceptionHandler", fallback = "helloFallback")
    public String hello(long s) {
        return String.format("Hello at %d", s);
    }
    
    // Fallback 函数，函数签名与原函数一致或加一个 Throwable 类型的参数.
    public String helloFallback(long s) {
        return String.format("Halooooo %d", s);
    }

    // Block 异常处理函数，参数最后多一个 BlockException，其余与原函数一致.
    public String exceptionHandler(long s, BlockException ex) {
        // Do some log here.
        ex.printStackTrace();
        return "Oops, error occurred at " + s;
    }
}
```

## 3.8 持久化

Dashboard 的推送规则方式是通过 API 将规则推送至客户端并直接更新到内存中，一旦重启应用，Sentinel 配置的规则就会丢失：

![Original push rules from Sentinel Dashboard](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701175389.png)

这种做法的好处是简单，无依赖；坏处是应用重启规则就会消失，仅用于简单测试，不能用于生产环境。生产环境肯定是需要持久化的，可以将规则持久化到某些持久层中，例如数据库。

我们这里演示一下持久化到 Nacos 配置中心里去。在 cloudalibaba-sentinel-service8401 pom 文件中添加依赖：

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

yaml 配置文件，配置 sentinel 的 datasource：

```yaml
server:
  port: 8401

spring:
  application:
    name: sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848   # 配置Nacos地址

    sentinel:
      transport:
        dashboard: localhost:8080   # 配置 Sentinel Dashboard 地址
        port: 8719  # 默认 8719 端口，用于与 Sentinel Dashboard 交互
      datasource:
        ds1:
          nacos:
            server-addr: localhost:8848
            namespace: public
            data-id: cloudalibaba-sentinel-service
            group-id: DEFAULT_GROUP
            data-type: json
            rule-type: flow   # 流控规则

management:
  endpoints:
    web:
      exposure:
        include: '*'  # Spring Actuator 监控，将所有可用的端点都公开出来，供外部访问。

```

在 Nacos 中添加流控规则的配置（其他规则的配置可自行搜索）：

```json
[
    {
        "resource": "/rateLimit/byUrl",
        "limitApp": "default",
        "grade": 1,
        "count": 1,
        "strategy": 0,
        "controlBehavior": 0,
        "clusterMode": false
    }
]

/*
resource：资源名称；
limitApp：来源应用；
grade：阈值类型，0 表示线程数，1 表示 QPS；
count：单机阈值；
strategy：流控模式，0 表示直接，1 表示关联，2 表示链路；
controlBehavior：流控效果，0 表示快速失败，1 表示 Warm Up，2 表示排队等待；
clusterMode：是否集群。
*/
```

![image-20231128203245098](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701175005.png)

查看 Sentinel Dashboard，发现已经有了该流控规则：

![image-20231128203627668](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701521469.png)

快速访问 http://localhost:8401/rateLimit/byUrl，流控规则也是可以生效的。

现在我们重启 sentinel-service8401，发现 Dashboard 中的流控规则还是存在的（注意 Dashboard 是懒加载，得访问一次接口流控规则才会显示）。

通过将规则写在 Nacos 配置中心里可以达到持久化的目的，但是无疑过于繁琐，其实，Sentinel 有三种规则管理及推送模式，可以在 Sentinel Dashboard 中直接配置规则，然后应用中再进行持久化（Pull 模式）：
![Push rules from Sentinel Dashboard to local file](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701175698.png)

或者引入一个远程配置中心，通过 Dashboard 将配置写入远程配置中心，然后再将配置推送到应用（Push 模式）：

![Remote push rules to config center](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_28-1701175711.png)

实现要涉及到改造 Sentinel 客户端应用，具体内容看官方文档：https://github.com/alibaba/Sentinel/wiki/%E5%9C%A8%E7%94%9F%E4%BA%A7%E7%8E%AF%E5%A2%83%E4%B8%AD%E4%BD%BF%E7%94%A8-Sentinel。

持久化到 MySQL：https://www.cnblogs.com/cdfive2018/p/9838577.html。

# 4. Seata 分布式事务

## 4.1 分布式事务问题

单体应用被拆分成微服务应用，原来的三个模块被拆分成三个独立的应用，分别使用三个独立的数据源，业务操作需要调用三个服务来完成。例如，用户购买商品的业务逻辑，整个业务逻辑由 3 个微服务提供支持：

- 仓储服务：对给定的商品扣除仓储数量。
- 订单服务：根据采购需求创建订单。
- 帐户服务：从用户帐户中扣除余额。

![Architecture](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_29-1701262804.png)

此时每个服务内部的数据一致性由本地事务来保证，但是 **全局的数据一致性问题没法保证**。所以 **一次业务操作需要跨多个数据源或跨多个系统进行远程调用**，就会产生分布式事务问题。



## 4.2 什么是 Seata

Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。

在学习 Seata 之前，先来了解一下一个分布式事务处理过程的 **一个 ID + 三个组件**：

- **Transaction ID（XID）事务 ID**：全局唯一的事务 ID；
- **Transaction Coordinator（TC）事务协调者**：维护全局和分支事务的状态，驱动全局事务提交或回滚；
- **Transaction Manager（TM）事务管理器**：定义全局事务的范围，开始全局事务、提交或回滚全局事务；
- **Resource Manager（RM）资源管理器**：管理分支事务处理的资源，与 TC 交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。

一个分布式事务的处理过程如下（**重要**）：

1. **TM 向 TC 申请开启一个全局事务，全局事务创建成功并生成一个全局唯一的 XID**；

   > **XID 在微服务调用链路的 context 中传播**。

2. **RM 向 TC 注册分支事务，将其纳入 XID 对应全局事务的管辖**；

3. **TM 向 TC 发起针对 XID 的全局提交或回滚决议**；

4. **TC 调度 XID 下管辖的全部分支事务完成提交或回滚请求**。

![image-20231129211819932](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_29-1701263904.png)

Seata 的使用是非常简单的，当把 Seata 部署好之后，直接使用 `@GlobalTransactional` 注解到业务方法上即可。在后续搭建 Demo 的时候就深有体会了。

## 4.3 Seata 部署

Seata 分 TC、TM 和 RM 三个角色，TC（Server端）为单独服务端部署，TM 和 RM（Client端）由业务系统集成。

### 4.3.1 启动 Server

Server 端存储模式（store.mode）现有 file、db、redis、raft 四种（后续将引入 mongodb）：

- file 模式：无需改动，直接启动即可；
- db 模式：高可用模式，全局事务会话信息通过 db 共享，相应性能差些；
- redis 模式：Seata-Server 1.3 及以上版本支持，性能较高，存在事务信息丢失风险，请提前配置合适当前场景的 redis 持久化配置。

**一、启动包**

去[官网](https://github.com/seata/seata/releases)下载 Seata-Server，Seata 新版本的配置文件有所更变，推荐直接下载最新版本（我下载的 v1.7.0，最好与 client 端 pom 文件中的 seata 版本一致），最新版的部署指南：https://seata.io/zh-cn/docs/ops/deploy-guide-beginner.html。

seata 的存储模式、配置中心、注册中心都可以自定义，我们下面的演示存储模式为 db，配置中心和注册中心都是 Nacos。

资源目录 `.\seata\script` 下的文件介绍：

- client：存放 client 端 sql 脚本 (包含 undo_log 表) ，参数配置（v2.0.0 已移除，在[源码](https://github.com/seata/seata/tree/develop/script/client)中获取）

- config-center：各个配置中心参数导入脚本，config.txt(包含server和client，原名nacos-config.txt)为通用参数文件

- server：server端数据库脚本 (包含 lock_table、branch_table 与 global_table) 及各个容器配置

**二、建表 — db 模式**

全局事务会话信息由3块内容构成，全局事务 --> 分支事务 --> 全局锁，对应表 global_table、branch_table、lock_table。SQL 文件在 `.\seata\script\server\db` 目录下。

**三、修改 store.mode 为 db，修改数据库连接属性配置**

启动包：seata-->conf-->application.yml，修改 store.mode="db"。

```yaml
seata:
  config:
    # support: nacos, consul, apollo, zk, etcd3
    type: file
  registry:
    # support: nacos, eureka, redis, zk, consul, etcd3, sofa
    type: file
  store:
    # support: file 、 db 、 redis 、 raft
    mode: db
    # 配置 db
    db:
      datasource: druid
      db-type: mysql
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/seata?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
      user: root
      password: 123456
      min-conn: 10
      max-conn: 100
      global-table: global_table
      branch-table: branch_table
      lock-table: lock_table
      distributed-lock-table: distributed_lock
      query-limit: 1000
      max-wait: 5000
  #  server:
  #    service-port: 8091 #If not configured, the default is '${server.port} + 1000'
```

**四、修改注册中心**

与上面 db 配置文件类似，修改 seata.registry：

```yaml
seata:
  config:
    # support: nacos, consul, apollo, zk, etcd3
    type: file
  registry:
    # support: nacos 、 eureka 、 redis 、 zk  、 consul 、 etcd3 、 sofa
    type: nacos
    nacos:
      application: seata-server
      server-addr: 127.0.0.1:8848
      group: SEATA_GROUP
      namespace:
      cluster: default
      username: 
      password: 
      context-path:
      ##if use MSE Nacos with auth, mutex with username/password attribute
      #access-key:
      #secret-key:
```

**五、修改配置中心**

修改 seata.config：

```yaml
seata:
  config:
    # support: nacos 、 consul 、 apollo 、 zk  、 etcd3
    type: file
    nacos:
      server-addr: 127.0.0.1:8848
      group: SEATA_GROUP
      namespace:
      username: 
      password: 
      context-path:
      data-id: seataServer.properties
      ##if use MSE Nacos with auth, mutex with username/password attribute
      #access-key:
      #secret-key:
```

> Tips：后续配置 client 时，确保 client 与 serve r的注册处于同一个 namespace 和 group，不然会找不到服务。

启动 Nacos，接着将 Seata 的配置上传到 Nacos。

通过 dataId 配置：

1. 从 v1.4.2 版本开始，已支持从一个 Nacos dataId 中获取所有配置信息，只需要额外添加一个 dataId 配置项。

2. 首先需要在 nacos 新建配置，此处 dataId 为 seataServer.properties，配置内容参考 `.\seata\script\config-center` 中的 config.txt 并按需修改保存。

   我们这里主要修改一个事务分组 `service.vgroupMapping.aaryn_test_tx_group` 和 存储模式 db `store.mode` 的内容：

   ```properties
   #Transaction routing rules configuration, only for the client
   # service.vgroupMapping.default_tx_group=default	# 指定事务分组至集群映射关系（等号右侧的集群名需要与Seata-server注册到Nacos的cluster保持一致）
   # 修改成自定义的事务分组
   service.vgroupMapping.aaryn_test_tx_group=default
   
   #Transaction storage configuration, only for the server. The file, db, and redis configuration values are optional.
   store.mode=db
   
   #These configurations are required if the `store mode` is `db`. If `store.mode,store.lock.mode,store.session.mode` are not equal to `db`, you can remove the configuration block.
   store.db.datasource=druid
   store.db.dbType=mysql
   store.db.driverClassName=com.mysql.cj.jdbc.Driver
   store.db.url=jdbc:mysql://localhost:3306/seata?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
   store.db.user=root
   store.db.password=123456
   store.db.minConn=5
   store.db.maxConn=30
   store.db.globalTable=global_table
   store.db.branchTable=branch_table
   store.db.distributedLockTable=distributed_lock
   store.db.queryLimit=100
   store.db.lockTable=lock_table
   store.db.maxWait=5000
   
   ```

Nacos 新建配置，注意 Data ID 为  seataServer.properties，Group 为 SEATA_GROUP：

![image-20231202005345686](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701449626.png)

**六、启动**

> 先启动 Nacos。

命令启动：`seata-server.sh -h 127.0.0.1 -p 8091 -m db`，win 执行执行 `.\seata-server.bat`

- -h: 注册到注册中心的 ip    
- -p: Server rpc 监听端口   
- -m: 全局事务会话信息存储模式，file、db、redis，优先读取启动参数 (Seata-Server 1.3 及以上版本支持 redis)    
- -n: Server node，多个 Server 时，需区分各自节点，用于生成不同区间的 transactionId，以免冲突    
- -e: 多环境配置参考 http://seata.io/en-us/docs/ops/multi-configuration-isolation.html

启动后查看 Nacos 的服务列表，可以看到 seata-server 已经注册进来了：

![image-20231130150533386](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_30-1701327934.png)

### 4.3.2 业务系统集成 Client

分布式事务业务示例说明：这里我们会创建三个服务，一个订单服务，一个库存服务，一个账户服务。

当用户下单时，会在订单服务中创建一个订单，然后通过远程调用库存服务来扣减下单商品的库存，再通过远程调用账户服务来扣减用户账户里面的余额，最后在订单服务中修改订单状态为已完成。

该操作跨越三个数据库，有两次远程调用，很明显会有分布式事务问题。

**一、数据库准备**

新建三个库，在每个库中新建对应的表：

- seata_account 库，t_account 用户表：

  ```SQL
  CREATE TABLE t_account (
  
    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'id',
  
    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
  
    `total` DECIMAL(10,0) DEFAULT NULL COMMENT '总额度',
  
    `used` DECIMAL(10,0) DEFAULT NULL COMMENT '已用余额',
  
    `residue` DECIMAL(10,0) DEFAULT '0' COMMENT '剩余可用额度'
  
  ) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
  
  
  INSERT INTO seata_account.t_account(`id`, `user_id`, `total`, `used`, `residue`)  VALUES ('1', '1', '1000', '0', '1000');
  ```

- seata_storage 库，t_storage 库存表：

  ```sql
  CREATE TABLE t_storage (
  
   `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  
   `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
  
   `total` INT(11) DEFAULT NULL COMMENT '总库存',
  
   `used` INT(11) DEFAULT NULL COMMENT '已用库存',
  
   `residue` INT(11) DEFAULT NULL COMMENT '剩余库存'
  
  ) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
  
   
  INSERT INTO seata_storage.t_storage(`id`, `product_id`, `total`, `used`, `residue`) VALUES ('1', '1', '100', '0', '100');
  ```

- seata_order 库，t_order 订单表：

  ```sql
  CREATE TABLE t_order (
  
    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  
    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
  
    `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
  
    `count` INT(11) DEFAULT NULL COMMENT '数量',
  
    `money` DECIMAL(11,0) DEFAULT NULL COMMENT '金额',
  
    `status` INT(1) DEFAULT NULL COMMENT '订单状态：0：创建中；1：已完结' 
  
  ) ENGINE=INNODB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;
  ```

除此之外，每个库下都要建立一个回滚日志表 undo_log（[官方 SQL 地址](https://github.com/seata/seata/blob/2.x/script/client/at/db/mysql.sql)）：

```sql
-- for AT mode you must to init this sql for you business database. the seata server not need it.
CREATE TABLE IF NOT EXISTS `undo_log`
(
    `branch_id`     BIGINT       NOT NULL COMMENT 'branch transaction id',
    `xid`           VARCHAR(128) NOT NULL COMMENT 'global transaction id',
    `context`       VARCHAR(128) NOT NULL COMMENT 'undo_log context,such as serialization',
    `rollback_info` LONGBLOB     NOT NULL COMMENT 'rollback info',
    `log_status`    INT(11)      NOT NULL COMMENT '0:normal status,1:defense status',
    `log_created`   DATETIME(6)  NOT NULL COMMENT 'create datetime',
    `log_modified`  DATETIME(6)  NOT NULL COMMENT 'modify datetime',
    UNIQUE KEY `ux_undo_log` (`xid`, `branch_id`)
) ENGINE = InnoDB AUTO_INCREMENT = 1 DEFAULT CHARSET = utf8mb4 COMMENT ='AT transaction mode undo table';
ALTER TABLE `undo_log` ADD INDEX `ix_log_created` (`log_created`);
```

所有的数据库表：

![image-20231130132224847](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_11_30-1701321762.png)

**二、业务模块搭建**

业务需求：下订单 -> 减库存 -> 扣余额 -> 改（订单）状态。

我们在学习 Seata 时，都使用新版。所以先改下 父 pom，使用 springboot3、jdk17，可直接使用 [Cloud Native App Initializer (aliyun.com)](https://start.aliyun.com/) 生成：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.run.sca</groupId>
    <artifactId>SpringCloudAlibaba</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <modules>
        <module>cloudalibaba-provider-payment9001</module>
        <module>cloudalibaba-provider-payment9002</module>
        <module>cloudalibaba-consumer-order83</module>
        <module>cloudalibaba-config-nacos-client3377</module>
        <module>cloudalibaba-sentinel-service8401</module>
        <module>cloudalibaba-seata-order-service2001</module>
    </modules>

    <!-- 同一管理 jar 包版本 -->
    <properties>
        <java.version>17</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>3.0.2</spring-boot.version>
        <spring-cloud-alibaba.version>2022.0.0.0-RC2</spring-cloud-alibaba.version>
        <spring-cloud.version>2022.0.0-RC2</spring-cloud.version>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.18.30</lombok.version>
        <mysql.version>8.0.33</mysql.version>
    </properties>

    <!-- 子模块继承之后，提供作用：锁定版本+子module不用写groupId和version  -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring-cloud-alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring-boot.version}</version>
                <configuration>
                    <mainClass>com.run.sca.SpringCloudAlibaba.SpringCloudAlibabaApplication</mainClass>
                    <skip>true</skip>
                </configuration>
                <executions>
                    <execution>
                        <id>repackage</id>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>
```

新建 cloudalibaba-seata-order-service2001 订单服务模块，pom：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.run.sca</groupId>
        <artifactId>SpringCloudAlibaba</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>cloudalibaba-seata-order-service2001</artifactId>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!--nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--seata-->
        <dependency>
            <groupId>io.seata</groupId>
            <artifactId>seata-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>io.seata</groupId>
                    <artifactId>seata-spring-boot-starter</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--feign，Hoxton.M2 RELEASED 版本之后不再使用 ribbon，而是 spring-cloud-loadbalancer-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>
        <!--web-actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--mysql-druid-->
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.16</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>3.0.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

</project>
```

yml 配置文件：

```yaml
server:
  port: 2001

spring:
  application:
    name: seata-order-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/seata_order?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: 123456

seata:
    # 自定义事务组名称需要与 seata 配置中心中的配置对应（service.vgroupMapping.aaryn_test_tx_group=default）
  tx-service-group: aaryn_test_tx_group
  service:
    # 虚拟组和分组的映射
    vgroup-mapping:
      aaryn_test_tx_group: default
#  enable-auto-data-source-proxy: true   # seata 自动代理数据源，从而管理事务（默认开启）
  # 配置中心
  config:
    type: nacos
    nacos:
      server-addr: 127.0.0.1:8848
      group: "SEATA_GROUP"
      namespace: ""
      dataId: "seataServer.properties"
      username:
      password:
  # 注册中心
  registry:
    type: nacos
    nacos:
      application: seata-server
      server-addr: 127.0.0.1:8848
      group: "SEATA_GROUP"  # 请确保client与server的注册处于同一个namespace和group，不然会找不到服务。
      namespace: ""
      username:
      password:

logging:
  level:
    io:
      seata: info

mybatis:
  mapperLocations: classpath:mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true

```

SpringApplication 启动类：

```java
@EnableDiscoveryClient
@SpringBootApplication
@MapperScan({"com.run.sca.dao"})
@EnableFeignClients
public class SeataOrderService2001 {
    public static void main(String[] args) {
        SpringApplication.run(SeataOrderService2001.class, args);
    }
}
```

新建 domain 包，创建 Order 实体和返回的结果 CommonResult：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Order
{
    private Long id;

    private Long userId;

    private Long productId;

    private Integer count;

    private BigDecimal money;

    /**
     * 订单状态：0：创建中；1：已完结
     */
    private Integer status;
}

@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T>
{
    private Integer code;
    private String  message;
    private T       data;

    public CommonResult(Integer code, String message)
    {
        this(code,message,null);
    }
}
```

新建 dao 包，创建 OrderDao：

```java
@Mapper
public interface OrderDao {

    /**
     * 创建订单
     */
    void create(Order order);

    /**
     * 修改订单金额
     */
    void update(@Param("userId") Long userId, @Param("status") Integer status);
}
```

`resources/mapper/OrderMapper.xml`：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="com.run.sca.dao.OrderDao">

    <insert id="create">
        INSERT INTO `t_order` (`id`, `user_id`, `product_id`, `count`, `money`, `status`)
        VALUES (NULL, #{userId}, #{productId}, #{count}, #{money}, 0);
    </insert>

    <update id="update">
        UPDATE `t_order`
        SET status = 1
        WHERE user_id = #{userId}
          AND status = #{status};
    </update>
    
</mapper>
```

service 包下的 OrderServcie 服务：

```java
public interface OrderService {

    /**
     * 创建订单
     */
    void create(Order order);
}


@Service
@Slf4j
public class OrderServiceImpl implements OrderService {
    
    @Resource
    private OrderDao orderDao;

    @Resource
    private AccountService accountService;

    @Resource
    private StorageService storageService;


    /**
     * 创建订单->调用库存服务扣减库存->调用账户服务扣减账户余额->修改订单状态
     * 简单说：下订单->减库存->减余额->改状态
     */
    @Override
    public void create(Order order) {
        log.info("------->下单开始");

        // 1. 本应用创建订单
        orderDao.create(order);

        // 2. 远程调用库存服务扣减库存
        log.info("------->order-service中扣减库存开始");
        storageService.decrease(order.getProductId(), order.getCount());
        log.info("------->order-service中扣减库存结束");

        // 3. 远程调用账户服务扣减余额
        log.info("------->order-service中扣减余额开始");
        accountService.decrease(order.getUserId(), order.getMoney());
        log.info("------->order-service中扣减余额结束");

        // 4. 修改订单状态为已完成
        log.info("------->order-service中修改订单状态开始");
        orderDao.update(order.getUserId(), 0);
        log.info("------->order-service中修改订单状态结束");

        log.info("------->下单结束");
    }
}
```

service 包下的 account、storage 服务，通过 Feign 远程调用：
```java
/**
 * @desc: 使用 Feign 调用 account 服务
 * @author: AarynLu
 * @date: 2023-11-30 16:10
 */
@FeignClient(value = "seata-account-service")
public interface AccountService {

    /**
     * 扣减账户余额
     */
    //@RequestMapping(value = "/account/decrease", method = RequestMethod.POST, produces = "application/json; charset=UTF-8")
    @PostMapping("/account/decrease")
    CommonResult decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money);

}


/**
 * @desc: 使用 Feign 调用 storage 服务
 * @author: AarynLu
 * @date: 2023-11-30 16:10
 */
@FeignClient(value = "seata-storage-service")
public interface StorageService {

    /**
     * 扣减库存
     */
    @PostMapping(value = "/storage/decrease")
    CommonResult decrease(@RequestParam("productId") Long productId, @RequestParam("count") Integer count);

}
```

controller：

```java
@RestController
public class OrderController {

    @Resource
    private OrderService orderService;

    /**
     * 创建订单
     */
    @GetMapping("/order/create")
    public CommonResult create(Order order) {
        orderService.create(order);
        return new CommonResult(200, "订单创建成功!");
    }

}
```

到此，订单服务就创建完毕了。其他两个服务也类似，就不详细写出来了，具体源码看：[AruNi-01/SpringCloudAlibaba (github.com)](https://github.com/AruNi-01/SpringCloudAlibaba)。

**三、测试**

将 Nacos、Seata-Server、还有三个服务启动，先来正常走一波流程，看看服务有没有问题，三个库表的初始数据：

![image-20231201234147144](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_01-1701445835.png)

访问：http://localhost:2001/order/create?userId=1&productId=1&count=10&money=100。

![image-20231201234748116](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_01-1701445670.png)

访问后库表数据：

![image-20231201234901784](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_01-1701445743.png)

可以发现三个库表的数据是满足一致性的，接下来手动制造异常，并且也 **不给 OrderServcie 的 `create(Order order)` 方法加 `@GlobalTransactional` 注解**。把 seata-account-service 服务的扣款设置超时异常：

```java
@Service
@Slf4j
public class AccountServiceImpl implements AccountService {

    @Resource
    AccountDao accountDao;

    /**
     * 扣减账户余额
     */
    @Override
    public void decrease(Long userId, BigDecimal money) {
        log.info("------->account-service中扣减账户余额开始");
        // sleep 10s 模拟超时异常（在调用方设置了 OpenFeign 请求处理超时时间是 3s），全局事务回滚
        try {
            TimeUnit.SECONDS.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        accountDao.decrease(userId, money);
        log.info("------->account-service中扣减账户余额结束");
    }
}
```

接着在 seata-order-service 的 yml 配置文件中配置向 seata-account-service 发出的请求，超时时间为 3s：

```yaml
spring:
  application:
    name: seata-order-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    openfeign:
      client:
        config:
          seata-account-service:  # 服务提供者名称
            read-timeout: 3000    # 配置 seata-account-service 服务的请求超时时间为 3s
```

重启两个服务，再次访问订单服务，按预期出现超时异常：

![image-20231202002624126](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701447985.png)

请求完后 **马上** 再来看看库表数据，发现 account 没有扣钱，订单生成了，status=0，表示未付款，而库存却减了：

![image-20231202002913765](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701448154.png)

过一会儿再刷新库表数据看看，会发现 account 又扣款成功了，这是因为 Feign 有重试机制，但是 order 的 status 依旧为 0，这就表示用户还是没付款，但又扣款了：

![image-20231202003306765](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701448387.png)

此时就需要使用 `@GlobalTransactional` 来保证全局事务了，在 order-service 的 create 方法上加上此注解：

```java
    @Override
    // Seata 开启分布式事务
    @GlobalTransactional(name = "seata-order-service:createOrder", rollbackFor = Exception.class)        
    public void create(Order order) {
```

再来访问，虽然还是超时，但是发现库表的数据都回滚了，三个库表的数据都没有变化：

![image-20231202004031616](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701448832.png)

来看看 seata 数据库和 seata-server（读取的 seata 数据库），都有对应的回滚记录：

![image-20231202004353858](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/Java%2FSpringCloudAlibaba.assets-2023_12_02-1701449035.png)

