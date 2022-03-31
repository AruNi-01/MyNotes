# 1. SpringBoot简介

## 1.1 回顾什么是Spring

- Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson  。
- **Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

## 1.2 Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；

## 1.3 什么是SpringBoot

- 学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个Hello  Wolrld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也在不断的变化、改造吗？建议都可以去经历一遍；
- 言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置，  you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。
- 所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生  一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。
- 是的这就是Java企业级应用->J2EE->spring->springboot的过程。
- 随着 Spring  不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring  Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；
- Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring  框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring  的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如  Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。
- 简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。
- Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

真的很爽，我们快速去体验开发个接口的感觉吧！

# 2. 第一个SpringBoot程序

## 2.1 准备工作

我们将学习如何快速的创建一个Spring Boot应用，并且实现一个简单的Http请求处理。通过这个例子对Spring Boot有一个初步的了解，并体验其结构简单、开发快速的特性。

我的环境准备：

- jdk1.8
- Maven-3.6.1
- SpringBoot 最新版
- IDEA

## 2.2 创建基础项目说明

Spring官方提供了非常方便的工具让我们快速构建应用

Spring Initializr：[https://start.spring.io/](https://gitee.com/link?target=https%3A%2F%2Fstart.spring.io%2F)

**项目创建方式一：**在官网使用Spring Initializr 的 Web页面创建项目

1、打开  [https://start.spring.io/](https://gitee.com/link?target=https%3A%2F%2Fstart.spring.io%2F)

2、填写项目信息

3、点击”Generate Project“按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

**项目创建方式二：**使用 IDEA 直接创建项目

1、创建一个新项目

2、选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功

**项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml

## 2.3 pom.xml 分析

打开pom.xml，看看Spring Boot项目的依赖：

```xml
<!-- 父依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/>
</parent>

<dependencies>
    <!-- web场景启动器 -->
    <!--web依赖：tomcat,dispatcherServlet，xml...-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--spring-boot-starter：所有的springboot依赖都是使用这个开头的-->
    <!-- springboot单元测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

如上所示，主要有四个部分：

- 项目元数据：创建时候输入的Project Metadata部分，也就是Maven项目的基本元素，包括：groupId、artifactId、version、name、description等
- parent：继承`spring-boot-starter-parent`的依赖管理，控制版本与打包等内容
- dependencies：项目具体依赖，这里包含了`spring-boot-starter-web`用于实现HTTP接口（该依赖中包含了Spring MVC），官网对它的描述是：使用Spring MVC构建Web（包括RESTful）应用程序的入门者，使用Tomcat作为默认嵌入式容器。`spring-boot-starter-test`用于编写单元测试的依赖包。更多功能模块的使用将在后面逐步展开。
- build：构建配置部分。默认使用了`spring-boot-maven-plugin`，配合`spring-boot-starter-parent`就可以把Spring Boot应用打包成JAR来直接运行。

## 2.4 编写一个http接口

1、在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到

![image-20220330162157176](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/222967cd76c4e042b741c8249e79824f.png)

2、在包中新建一个HelloController类

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

3、编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！

![image-20220330160845131](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/eb426a0ab84e97b29971e3df2a36022f.png)

简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的微服务项目！



> 将项目打成jar包，点击 maven的 package

- 如果打包成功，则会在target目录下生成一个 jar 包

  ![image-20220330162331243](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dc8815b3dbd65a25fd1ca330223a9eb6.png)

- 打成了jar包后，就可以在任何地方运行了！OK

  ![image-20220330162928457](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/02ced036f0c4d049d60829bbb762216b.png)


- 浏览器运行结果：

![image-20220330162659667](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ab04187d584373593e2d61b7ae601f1d.png)





# 3. 运行原理初探

我们之前写的HelloSpringBoot，到底是怎么运行的呢，Maven项目，我们一般从`pom.xml`文件探究起

## 3.1 父依赖

> **pom.xml**

- `spring-boot-dependencies`：核心依赖在父工程中！
- 我们在写或者引入一些Springboot依赖的时候，不需要指定版本，就因为有这些版本仓库

1、其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.1.RELEASE</version>
    <relativePath/> 
    <!-- lookup parent from repository -->
</parent>
```

2、点进去，发现还有一个父依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.1.RELEASE</version>
</parent>
```

3、这里才是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；

**4、以后我们导入依赖默认是不需要写版本；但是如果导入的包没有在依赖中管理着就需要手动配置版本了；**

## 3.2 启动器 spring-boot-starter

- 依赖

  ```xml
  <dependency>        								 <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

- **springboot-boot-starter-xxx**，说白了就是Springboot的启动场景

- 比如**spring-boot-starter-web**，他就会帮我们自动导入web的所有依赖

- springboot会将所有的功能场景，都变成一个个的启动器

- 我们要使用什么功能，就只需要找到对应的启动器就好了`start`

## 3.3 主程序

### 3.3.1 默认的主启动类

```java
//@SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
@SpringBootApplication
public class SpringbootApplication {
   public static void main(String[] args) {
     //以为是启动了一个方法，没想到启动了一个服务
      SpringApplication.run(SpringbootApplication.class, args);
   }
}
```

但是**一个简单的启动类并不简单！**我们来分析一下这些注解都干了什么



> @SpringBootApplication

- 作用：标注在某个类上说明这个类是SpringBoot的主配置

- SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

- 进入这个注解：可以看到还有很多其他注解

  ```java
  @SpringBootConfiguration
  @EnableAutoConfiguration
  @ComponentScan(
      excludeFilters = {@Filter(
      type = FilterType.CUSTOM,
      classes = {TypeExcludeFilter.class}
  ), @Filter(
      type = FilterType.CUSTOM,
      classes = {AutoConfigurationExcludeFilter.class}
  )}
  )
  public @interface SpringBootApplication {
      // ......
  }
  ```



> @ComponentScan

- 这个注解在Spring中很重要 ,它对应XML配置中的元素。
- 作用：自动扫描并加载符合条件的组件或者bean ， 将这个bean定义加载到IoC容器中



> @SpringBootConfiguration

- 作用：SpringBoot的配置类 ，标注在某个类上 ， 表示这是一个SpringBoot的配置类；

- 我们继续进去这个注解查看

  ```java
  // 点进去得到下面的 @Component
  @Configuration
  public @interface SpringBootConfiguration {}
  
  @Component
  public @interface Configuration {}
  ```

- 这里的 **@Configuration**，说明这是一个spring的配置类 ，配置类就是对应Spring的xml 配置文件；

- **@Component** 这就说明，启动类本身也是Spring中的一个组件而已，负责启动应用！

- 我们回到 SpringBootApplication 注解中继续看。



> @EnableAutoConfiguration

**作用：开启自动配置功能**

- 以前我们需要自己配置的东西，而现在SpringBoot可以自动帮我们配置 ；
- `@EnableAutoConfiguration`告诉SpringBoot开启自动配置功能，这样自动配置才能生效；

点进注解继续查看：

- **@AutoConfigurationPackage ：自动配置包**

  ```java
  @Import({Registrar.class})
  public @interface AutoConfigurationPackage {
  }
  ```

  - `@import` ：Spring底层注解@import ， 给容器中导入一个组件
  - `Registrar.class` 作用：自动配置包注册，将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；
  - 这个分析完了，退到上一步，继续看

- **@Import({AutoConfigurationImportSelector.class}) ：给容器导入组件 ；**

  - **AutoConfigurationImportSelector** ：自动配置导入选择器，那么它会导入哪些组件的选择器呢？我们点击去这个类看源码：

  - ```java
    // 获取所有的配置 
    List configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
    ```

  - 获得候选的配置

    ```java
    //获得候选的配置
    protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        // 和下面的方法对应
        //这里的getSpringFactoriesLoaderFactoryClass（）方法
        //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
      List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
        
      
        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
        return configurations;
    }
    
    //和上面的类的方法loadFactoryNames里面的第一个参数对应
    //这里的getSpringFactoriesLoaderFactoryClass（）方法
    //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
    protected Class<?> getSpringFactoriesLoaderFactoryClass() {
         return EnableAutoConfiguration.class;
    }
    ```

  - 这个方法`getCandidateConfigurations()`又调用了  `SpringFactoriesLoader` 类的静态方法！我们进入`SpringFactoriesLoader`类的`loadFactoryNames() 方法`，获取所有的加载配置

    ```java
    public static List<String> loadFactoryNames(Class<?> factoryClass, @Nullable ClassLoader classLoader) {
        String factoryClassName = factoryClass.getName();
        //这里它又调用了 loadSpringFactories 方法
        return (List)loadSpringFactories(classLoader).getOrDefault(factoryClassName, Collections.emptyList());
    }
    ```

  - 我们继续点击查看 `loadSpringFactories` 方法

    - 项目资源:`META-INF/spring.factories`
    - 系统资源:`META-INF/spring.factories`
    - 从这些资源中配置了所有的nextElement（自动配置），分装成properties

    ```java
    //将所有的资源加载到配置类中（将下面的抽离出来分析，第15行）
    Properties properties = PropertiesLoaderUtils.loadProperties(resource);
    
    private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
        //获得classLoader ， 我们返回可以看到这里得到的就是EnableAutoConfiguration标注的类本身
        MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
        if (result != null) {
            return result;
        } else {
            try {
                //去获取一个资源 "META-INF/spring.factories"
                Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
                LinkedMultiValueMap result = new LinkedMultiValueMap();
                //判断有没有更多的元素，将读取到的资源循环遍历，封装成为一个Properties
                while(urls.hasMoreElements()) {
                  URL url = (URL)urls.nextElement();
                    UrlResource resource = new UrlResource(url);
                    Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                    Iterator var6 = properties.entrySet().iterator();
                    while(var6.hasNext()) {
                        Entry<?, ?> entry = (Entry)var6.next();
                        String factoryClassName = ((String)entry.getKey()).trim();
                        String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                        int var10 = var9.length;
                        for(int var11 = 0; var11 < var10; ++var11) {
                            String factoryName = var9[var11];
                            result.add(factoryClassName, factoryName.trim());
                        }
                    }
                }
                cache.put(classLoader, result);
                return result;
            } catch (IOException var13) {
                throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
            }
        }
    }
    ```

  - 发现一个多次出现的文件：`spring.factories`，全局搜索它

### 3.3.2 spring.factories

我们在`spring-boot-autoconfigure-2.6.5.jar`中打开`spring.factories` ， 看到了很多自动配置的文件；这就是自动配置根源所在！

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0b982638046fa1e11eeb4e6bd5b19bbd.png)



我们在上面的自动配置类随便找一个打开看看，比如 ：`WebMvcAutoConfiguration`

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8b4dfc599bb978e2918b980fa8177a28.png)

可以看到这些一个个的都是JavaConfig配置类，而且都注入了一些Bean，可以找一些自己认识的类，看着熟悉一下！

所以，自动配置真正实现是从classpath中搜寻所有的`META-INF/spring.factories`配置文件 ，并将其中对应的 `org.springframework.boot.autoconfigure`. 包下的配置项，通过反射实例化为对应标注了` @Configuration的JavaConfig`形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。

## 3.4 结论

1. SpringBoot在启动的时候从类路径`Maven: org.springframework.boot:spring-boot-autoconfigure:2.6.5`下的`META-INF/spring.factories`中获取`EnableAutoConfiguration`指定的值
2. 将这些值作为自动配置类导入容器，自动配置类就生效，帮我们进行自动配置工作
3. 以前我们需要自动配置的东西，现在springboot帮我们做了
4. 整合JavaEE，整体解决方案和自动配置的东西都在`spring-boot-autoconfigure-2.6.5.jar`的jar包中
5. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器中
6. 它会给容器中导入非常多的自动配置类（xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组件，并自动配置，`@Configuration`（javaConfig）
7. 有了自动配置类，免去了我们手动编写配置注入功能组件等的工作

**现在大家应该大概的了解了SpringBoot的运行原理，后面还会深化一次！**

## 3.5 Run启动

> 不简单的方法

我最初以为就是运行了一个main方法，没想到却开启了一个服务；

```java
@SpringBootApplication
public class Springboot01HellowordApplication {

    public static void main(String[] args) {
       //该方法返回一个ConfigurableApplicationContext对象
 	//参数一：应用入口的类； 参数二：命令行参数  
        SpringApplication.run(Springboot01HellowordApplication.class, args);
    }

}
```

**SpringApplication.run分析**

- 分析该方法主要分两部分
- 一是SpringApplication的实例化，
- 二是run方法的执行；



> SpringApplication

**这个类主要做了以下四件事情：**

1. 推断应用的类型是普通的Java项目还是Web项目
2. 查找并加载所有可用初始化器，设置到initializers属性中
3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类

**查看构造器**：

```java
public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
    // ......
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances();
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```



> run方法流程分析

<img src="https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dfdcde651f89e493814da32edd37eaf3.png"  />

跟着源码和这幅图就可以一探究竟了！



> 关于SpringBoot，谈谈你的理解

- 自动装配
- run()
- 全面接管SpringMVC的配置





# 4. yaml

SpringBoot使用一个全局的配置文件，使用`properties`和`yaml`都可以，但是配置文件名称是固定的`application`

- `application.properties`
  - 语法结构 ：key=value
- `application.yaml`
  - 语法结构 ：key：空格 value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了；

比如我们可以在配置文件中修改Tomcat 默认启动的端口号！

```yaml
server:
  port: 8081
```



## 4.1 yaml概述

- YAML是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）

- 这种语言以**数据**作为中心，而不是以标记语言为重点！

- 以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml

  - 传统xml配置：

    ```xml
    <server>
        <port>8081<port>
    </server>
    ```

  - yaml配置：

    ```yaml
    server：
      prot: 8080
    ```

## 4.2 yaml基础语法

**说明：语法要求严格！**

1. 空格不能省略
2. 以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。
3. 属性和值的大小写都是十分敏感的。





**字面量：普通的值  [ 数字，布尔值，字符串  ]**

- 字面量直接写在后面就可以 ， 字符串默认不用加上双引号或者单引号；`k: v`

  注意：

  - “ ” 双引号，**不会转义字符串里面的特殊字符**，特殊字符会作为本身想表示的意思；

    比如 ：`name: "kuang \n shen"`   输出 ：`kuang  换行   shen`

  - ' ' 单引号，**会转义特殊字符** ， 特殊字符最终会变成和普通字符一样输出

    比如 ：`name: ‘kuang \n shen’`   输出 ：`kuang  \n   shen`





**对象、Map（键值对）**

```yaml
#对象、Map格式
k: 
    v1:
    v2:
```

在下一行来写对象的属性和值得关系，注意缩进；比如：

```yaml
student:
    name: qinjiang
    age: 3
```

行内写法

```yaml
student: {name: qinjiang,age: 3}
```





**数组（ List、set ）**

用 - 值表示数组中的一个元素,比如：

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```





**修改SpringBoot的默认端口号**

配置文件中添加，端口号的参数，就可以切换端口；

```yaml
server:
  port: 8082
```

## 4.3 yaml注入配置文件

yaml文件更强大的地方在于，他可以给我们的**实体类直接注入匹配值**

1. 在springboot项目中的`resources`目录下新建一个文件 `application.yaml`

2. 编写一个实体类 Dog；

   ```java
   package nuc.ss.pojo;
   
   @Component  //注册bean到容器中
   public class Dog {
       private String name;
       private Integer age;
       
       //有参无参构造、get、set方法、toString()方法  
   }
   ```

3. 思考，我们原来是如何给bean注入属性值的！@Value，给狗狗类测试一下：

   ```java
   @Component //注册bean
   public class Dog {
       @Value("阿黄")
       private String name;
       @Value("18")
       private Integer age;
   }
   ```

4. 在SpringBoot的测试类下注入狗狗输出一下；

   ```java
   @SpringBootTest
   class ApplicationTests {
   
       @Autowired
       private Dog dog;
   
       @Test
       void contextLoads() {
           System.out.println(dog);
       }
   
   }
   ```

   结果成功输出，@Value注入成功，这是我们原来的办法。

   ![image-20220330203504607](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/45e2c46f999caf6930d1354616674869.png)

5. 我们在编写一个复杂一点的实体类：Person 类

   ```java
   @Component 		//注册bean到容器中
   public class Person {
       private String name;
       private Integer age;
       private Boolean happy;
       private Date birth;
       private Map<String,Object> maps;
       private List<Object> lists;
       private Dog dog;
       
       //有参无参构造、get、set方法、toString()方法  
   }
   ```

6. 我们来使用yaml配置的方式进行注入，大家写的时候注意区别和优势，我们编写一个`application.yaml`配置！

   ```yaml
   Person:
     name: AruNi
     age: 18
     happy: true
     birth: 2001/10/12
     maps: {k1: v1,k2: v2}
     lists:
       - code
       - girl
       - music
     dog:
       name: 旺财
       age: 3
   ```

7. 刚才已经把person这个对象的所有值都写好了，我们现在来注入到我们的实体类中

   ```java
   /*
   @ConfigurationProperties作用：
   将配置文件中配置的每一个属性的值，映射到这个组件中；
   告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
   参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
   */
   @Component
   @ConfigurationProperties(prefix = "person")
   public class Person {
       private String name;
       private Integer age;
       private Boolean happy;
       private Date birth;
       private Map<String,Object> maps;
       private List<Object> lists;
       private Dog dog;
   
       //有参无参构造、get、set方法、toString()方法
   }
   ```

8. 添加了`ConfigurationProperties`后，IDEA会有爆红提示(不会报错)，springboot配置注解处理器没有找到

   ![image-20220330204838949](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2cdf1126340cf9cd4d3cb7e22ede5f7e.png)

   此时可以点击Open Decumentation进入官网

   ![image-20220330205007179](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/710f22d5e45984600d0645769ca59bc1.png)

   在pom中导入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-configuration-processor</artifactId>
       <optional>true</optional>
   </dependency>
   ```

9. 确认以上配置都OK之后，我们去测试类中测试一下：

   ```java
   @SpringBootTest
   class ApplicationTests {
   
       @Autowired
       private Person person;
       
       @Test
       void contextLoads() {
           System.out.println(person);
       }
   }
   ```

10. 测试结果：

    ```text
    Person{name='AruNi', age=18, happy=true, birth=Fri Oct 12 00:00:00 CST 2001, maps={k1=v1, k2=v2}, lists=[code, girl, music], dog=Dog{name='旺财', age=3}}
    ```

**这就是yaml配置注入到实体类**





## 4.4 回顾properties配置

- 我们上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的
- 配置文件除了yml还有我们之前常用的properties

【注意】`properties`配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；settings-->FileEncodings 中配置；

![image-20220330211829040](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b479cecf33cd4cec40631b9292bb3d5f.png)

## 4.5 加载指定的配置文件

**@PropertySource ：**加载指定的配置文件

**@ConfigurationProperties**：默认从全局配置文件中获取值



1. 我们去在resources目录下新建一个`person.properties`文件

   ```properties
   name=张三
   ```

2. 然后在我们的代码中指定加载person.properties文件

   ```java
   @PropertySource(value = "classpath:person.properties")
   @Component //注册bean
   public class Person {
   
       //SPEL表达式取出配置文件中的值
       @Value("${name}")
       private String name;
   
       ......  
   }
   ```

3. 再次输出测试一下：指定配置文件绑定成功！

   ![image-20220330210523609](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dcce1c991f68d5648e48159b8ead707c.png)

## 4.6 配置文件占位符

配置文件还可以编写占位符生成随机数

```yaml
person:
  name: qinjiang${random.uuid}
  age: ${random.int}
  happy: false
  birth: 2020/07/13
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
    - girl
  dog:
    name: ${person.hi:hello}_旺财
    age: 3
```

`${person.hi:hello}`：如果person.hi存在，则就取person.hi的值，否则取值为hello

输出结果：

```text
Person{name='AruNib5aa7acf-dc70-48e7-a8ff-42ad0947b87e', age=1349162838, happy=true, birth=Fri Oct 12 00:00:00 CST 2001, maps={k1=v1, k2=v2}, lists=[code, girl, music], dog=Dog{name='hello_旺财', age=3}}
```



在person中添加hi属性：

```yaml
person:
  name: qinjiang${random.uuid}
  age: ${random.int}
  happy: false
  birth: 2020/07/13
  maps: {k1: v1,k2: v2}
  hi: hi
  lists:
    - code
    - music
    - girl
  dog:
    name: ${person.hi:hello}_旺财
    age: 3
```

输出结果：

```text
Person{name='AruNidd1dd3c2-f35c-4907-97f6-6218be725f2b', age=886385448, happy=true, birth=Fri Oct 12 00:00:00 CST 2001, maps={k1=v1, k2=v2}, lists=[code, girl, music], dog=Dog{name='hi_旺财', age=3}}
```

## 4.7 对比小结

@Value这个使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图

|                | @ConfigurationProperties | @Value     |
| -------------- | ------------------------ | ---------- |
| 功能           | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定       | 支持                     | 不支持     |
| SpEL           | 不支持                   | 支持       |
| JSR303数据校验 | 支持                     | 不支持     |
| 复杂类型封装   | 支持                     | 不支持     |

1. `@ConfigurationProperties`只需要写一次即可 ， @Value则需要每个字段都添加
2. 松散绑定：这个什么意思呢? 比如我的yml中写的`last-name`，这个和`lastName`是一样的，`-`后面跟着的字母默认是大写的。这就是松散绑定
3. JSR303数据校验，这个就是我们可以在字段是增加一层过滤器验证，可以保证数据的合法性
4. 复杂类型封装，yml中可以封装对象 ， 使用value就不支持

**结论：**

- 配置yml和配置properties都可以获取到值，**强烈推荐 yml**

- 如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value

- 如果说，我们专门编写了一个**JavaBean来和配置文件进行一一映射**，就直接**@configurationProperties**

# 5. JSR303数据校验和多环境切换

## 5.1 JSR303数据校验

### 5.1.1 如何使用

Springboot中可以用`@validated`来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。我们这里来写个注解让我们的name只能支持Email格式；

1、添加validation启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

2、用name属性来测试一个邮箱格式，在属性上添加@Email注解，并给出错误提示

```java
@Component //注册bean
@ConfigurationProperties(prefix = "person")
@Validated 		 //数据校验
public class Person {
    @Email(message="邮箱格式错误") 		//name必须是邮箱格式
    private String name;
}
```

3、运行测试，发现直接报错

```text
***************************
APPLICATION FAILED TO START
***************************

Description:

Binding to target org.springframework.boot.context.properties.bind.BindException: Failed to bind properties under 'person' to com.run.pojo.Person failed:

    Property: person.name
    Value: AruNi6b585d40-cf38-4b99-83d3-729ea08aaa10
    Origin: class path resource [application.yaml] - 2:9
    Reason: 邮箱格式错误
```

**使用数据校验，可以保证数据的正确性；**

### 5.1.2 常见参数

```text
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则
```

可以在依赖中查看：

![image-20220330213800143](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5166eab5ecf49fc95425afedef5550e6.png)

## 5.2 多环境切换

`profile`是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境

### 5.2.1 多配置文件

我们在主配置文件编写的时候，文件名可以是 `application-{profile}.properties/yml `, 用来指定多个环境版本；

**例如：**

`application-test.properties` 代表测试环境配置

`application-dev.properties` 代表开发环境配置

但是SpringBoot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

![1595484043622](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/df5895ccbfbb31138ec3a4142db42774.png)

我们需要通过`spring.profiles.active`配置来选择需要激活的环境：

```properties
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev
```

### 5.2.2 yaml的多文档模块

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，使用`---`分隔开就好，更加方便了 

```yaml
server:
  port: 8081
#选择要激活那个环境块
spring:
  profiles:
    active: test

---

server:
  port: 8083
spring:
  profiles: dev #配置环境的名称


---

server:
  port: 8084
spring:
  profiles: test  #配置环境的名称
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境，默认会使用properties配置文件的**



### 5.2.3 配置文件加载位置

**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**

可以在[官方外部配置文件说明参考文档](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2F2.3.1.RELEASE%2Freference%2Fhtml%2Fspring-boot-features.html%23boot-features-logging-color-coded-output)的Application Property Files中查看配置方式：

![image-20220330215019389](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/839809ad09ed1c12847bc4701a551668.png)

springBoot 启动会扫描以下位置的`application.properties`或者`application.yml`文件作为SpringBoot的默认配置文件：

![image-20220330215151212](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b60eaa658fa9fec8547d51f9cef0fe54.png)

```text
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；

```yaml
#配置项目的访问路径
server.servlet.context-path=/ss
```



### 5.2.4 拓展：运维小技巧

指定位置加载配置文件

我们还可以通过`spring.config.location`来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；

这种情况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

```shell
java -jar spring-boot-config.jar --spring.config.location=F:/application.properties
```

# 6. 自动装配原理

## 6.1 自动配置原理

配置文件`application`中到底能写什么？怎么写？联系---- `spring.factories`

SpringBoot官方[配置文档](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2F2.3.1.RELEASE%2Freference%2Fhtml%2Fappendix-application-properties.html%23core-properties)中有大量的配置，我们无法全部记住，所以分析其自动配置的原理，可以帮助我们更好的理解这些配置。

## 6.2 分析自动配置原理

1. SpringBoot启动的时候加载主配置类，开启了自动配置功能 `@EnableAutoConfiguration`

2. `@EnableAutoConfiguration` 作用：

   - 利用`EnableAutoConfigurationImportSelector`给容器中导入一些组件

   - 可以查看上述类中的`selectImports()`方法的内容,他返回了一个`autoConfigurationEnty`，来自`this.getAutoConfigurationEntry(annotationMetadata)`这个方法，继续来跟踪：

   - 这个方法有一个值：`List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);`叫做获取候选的配置 ，我们点击继续跟踪，进入这个类：

     ```java
     protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
             List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
             Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
             return configurations;
         }
     ```

     

     - `SpringFactoriesLoader.loadFactoryNames(......)`扫描所有jar包类路径`META-INF/spring.factories`
     - 把扫描到的这些文件的内容包装成**properties对象**
     - 从**properties对象**中获取到**EnableAutoConfiguration.class类（类名）**对应的值，然后把他们添加在容器中

   - 在类路径下,`META-INF/spring.factories`里面配置的所有`EnableAutoConfiguration`的值加入到容器中：

     ```yaml
     # Initializers
     org.springframework.context.ApplicationContextInitializer=\
     org.springframework.boot.autoconfigure.SharedMetadataReaderFactoryContextInitializer,\
     org.springframework.boot.autoconfigure.logging.ConditionEvaluationReportLoggingListener
     
     # Application Listeners
     org.springframework.context.ApplicationListener=\
     org.springframework.boot.autoconfigure.BackgroundPreinitializer
     
     # Auto Configuration Import Listeners
     org.springframework.boot.autoconfigure.AutoConfigurationImportListener=\
     org.springframework.boot.autoconfigure.condition.ConditionEvaluationReportAutoConfigurationImportListener
     
     # Auto Configuration Import Filters
     org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
     org.springframework.boot.autoconfigure.condition.OnBeanCondition,\
     org.springframework.boot.autoconfigure.condition.OnClassCondition,\
     org.springframework.boot.autoconfigure.condition.OnWebApplicationCondition
     
     # Auto Configure
     org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
     org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
     org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
     org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
     org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
     org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
     org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
     #......还有很多XXXAutoConfiguration
     
     # Failure analyzers
     org.springframework.boot.diagnostics.FailureAnalyzer=\
     org.springframework.boot.autoconfigure.diagnostics.analyzer.NoSuchBeanDefinitionFailureAnalyzer,\
     org.springframework.boot.autoconfigure.flyway.FlywayMigrationScriptMissingFailureAnalyzer,\
     org.springframework.boot.autoconfigure.jdbc.DataSourceBeanCreationFailureAnalyzer,\
     org.springframework.boot.autoconfigure.jdbc.HikariDriverConfigurationFailureAnalyzer,\
     org.springframework.boot.autoconfigure.r2dbc.ConnectionFactoryBeanCreationFailureAnalyzer,\
     org.springframework.boot.autoconfigure.session.NonUniqueSessionRepositoryFailureAnalyzer
     
     # Template availability providers
     org.springframework.boot.autoconfigure.template.TemplateAvailabilityProvider=\
     org.springframework.boot.autoconfigure.freemarker.FreeMarkerTemplateAvailabilityProvider,\
     org.springframework.boot.autoconfigure.mustache.MustacheTemplateAvailabilityProvider,\
     org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAvailabilityProvider,\
     org.springframework.boot.autoconfigure.thymeleaf.ThymeleafTemplateAvailabilityProvider,\
     org.springframework.boot.autoconfigure.web.servlet.JspTemplateAvailabilityProvider
     ```

     每一个这样的`xxxAutoConfiguration`类都是容器中的一个**组件，都加入到容器中；用他们来做自动配置；**

3. 每一个自动配置类进行自动配置功能；

4. 我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

   ```java
   //表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
   @Configuration 
   
   //启动指定类的ConfigurationProperties功能；
   //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
   //并把HttpProperties加入到ioc容器中
   @EnableConfigurationProperties({HttpProperties.class}) 
   
   //Spring底层@Conditional注解
   //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
   //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
   @ConditionalOnWebApplication(
       type = Type.SERVLET
   )
   
   //判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
   @ConditionalOnClass({CharacterEncodingFilter.class})
   
   //判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
   //如果不存在，判断也是成立的
   //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
   @ConditionalOnProperty(
       prefix = "spring.http.encoding",
       value = {"enabled"},
       matchIfMissing = true
   )
   
   public class HttpEncodingAutoConfiguration {
       //他已经和SpringBoot的配置文件映射了
       private final Encoding properties;
       //只有一个有参构造器的情况下，参数的值就会从容器中拿
       public HttpEncodingAutoConfiguration(HttpProperties properties) {
           this.properties = properties.getEncoding();
       }
   
       //给容器中添加一个组件，这个组件的某些值需要从properties中获取
       @Bean
       @ConditionalOnMissingBean //判断容器没有这个组件？
       public CharacterEncodingFilter characterEncodingFilter() {
           CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
           filter.setEncoding(this.properties.getCharset().name());
           filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
           filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
           return filter;
       }
       //。。。。。。。
   }
   ```





**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；

- 这些组件的属性是从对应的**properties类**中获取的，这些类里面的每一个属性又是**和配置文件绑定**的；

- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；

- 配置文件能配置什么就可以参照某个功能对应的这个属性类

  ```java
  //从配置文件中获取指定的值和bean的属性进行绑定
  @ConfigurationProperties(prefix = "spring.http") 
  public class HttpProperties {
      // .....
  }
  ```

我们去配置文件里面试试前缀，看提示！

![1595493884773](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a1ad762f7f4b8917e8aab361eb79c7df.png)

**这就是自动装配的原理！**

## 6.3 精髓

1. SpringBoot启动会加载大量的自动配置类

2. 我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中

3. 我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可

   **`xxxAutoConfigurartion`：自动配置类**，给容器中添加组件

   **`xxxProperties`：封装配置文件中相关属性**

## 6.4 @Conditional

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

| @Conditional扩展注解            | 作用(判断是否满足当前指定条件)                   |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnJava              | 容器中存在指定Bean ;                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean ;                           |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean ,或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**

## 6.5 自动配置类是否生效

**我们可以在application.properties通过启用 `debug=true`属性；**

**在控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**

```yaml
#开启springboot的调试类
debug=true 
```

- **Positive matches:（自动配置类启用的：正匹配）**
- **Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**
- **Unconditional classes: （没有条件的类）**
- **【演示：运行项目查看输出的日志】**



# 7. 自定义Starter

我们分析完毕了源码以及自动装配的过程，我们可以尝试自定义一个启动器！

## 7.1 说明

启动器模块是一个空jar文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

**命名归约**：

官方命名：

- 前缀：spring-boot-starter-xxx
- 比如：spring-boot-starter-web....

自定义命名：

- xxx-spring-boot-starter
- 比如：mybatis-spring-boot-starter

## 7.2 编写启动器

1. 在IDEA中新建一个空项目 spring-boot-starter-diy

2. 新建一个普通Maven模块：kuang-spring-boot-starter

3. 新建一个SpringBoot模块：kuang-spring-boot-starter-autoconfigure

4. 点击apply即可，基本结构

5. 在我们的starter中导入autoconfigure的依赖

   ```xml
   <!-- 启动器 -->
   <dependencies>
       <!--  引入自动配置模块 -->
       <dependency>
           <groupId>com.kuang</groupId>
           <artifactId>kuang-spring-boot-starter-autoconfigure</artifactId>
           <version>0.0.1-SNAPSHOT</version>
       </dependency>
   </dependencies>
   ```

6. 将autoconfigure项目下多余的文件都删掉，`pom.xml`中只留下一个starter，这是所有的启动器基本配置！

7. 我们编写一个自己的服务

   ```java
   package nuc.ss;
   
   public class HelloService {
   
       HelloProperties helloProperties;
   
       public HelloProperties getHelloProperties() {
           return helloProperties;
       }
   
       public void setHelloProperties(HelloProperties helloProperties) {
           this.helloProperties = helloProperties;
       }
   
       public String sayHello(String name){
           return helloProperties.getPrefix() + name + helloProperties.getSuffix();
       }
   
   }
   ```

8. 编写`HelloProperties` 配置类

   ```java
   package nuc.ss;
   
   import org.springframework.boot.context.properties.ConfigurationProperties;
   
   // 前缀 kuang.hello
   @ConfigurationProperties(prefix = "kuang.hello")
   public class HelloProperties {
   
       private String prefix;
       private String suffix;
   
       public String getPrefix() {
           return prefix;
       }
   
       public void setPrefix(String prefix) {
           this.prefix = prefix;
       }
   
       public String getSuffix() {
           return suffix;
       }
   
       public void setSuffix(String suffix) {
           this.suffix = suffix;
       }
   }
   ```

9. 编写自动配置类并注入bean，测试

   ```java
   package nuc.ss;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   @Configuration
   @ConditionalOnWebApplication //web应用生效
   @EnableConfigurationProperties(HelloProperties.class)
   public class HelloServiceAutoConfiguration {
   
       @Autowired
       HelloProperties helloProperties;
   
       @Bean
       public HelloService helloService(){
           HelloService service = new HelloService();
           service.setHelloProperties(helloProperties);
           return service;
       }
   
   }
   ```

10. 在resources编写一个自己的 `META-INF\spring.factories`

    ```yaml
    # Auto Configure
    org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
    nuc.ss.HelloServiceAutoConfiguration
    ```

11. 编写完成后，可以安装到maven仓库中！

## 7.3 测试自定义启动器

1. 新建一个SpringBoot 项目

2. 导入我们自己写的启动器

   ```xml
   <dependency>
       <groupId>nuc.ss</groupId>
       <artifactId>ss-spring-boot-starter</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```

3. 编写一个 `HelloController`  进行测试我们自己的写的接口！

   ```java
   package nuc.ss.controller;
   
   @RestController
   public class HelloController {
   
       @Autowired
       HelloService helloService;
   
       @RequestMapping("/hello")
       public String hello(){
           return helloService.sayHello("zxc");
       }
   
   }
   ```

4. 编写配置文件 `application.properties`

   ```properties
   ss.hello.prefix="ppp"
   ss.hello.suffix="sss"
   ```

5. 启动项目进行测试，结果成功 !



# 8. Web静态资源处理

## 8.1 简介

其实SpringBoot的东西用起来非常简单，因为SpringBoot最大的特点就是自动装配。

**使用SpringBoot的步骤：**

1、创建一个SpringBoot应用，选择我们需要的模块，SpringBoot就会默认将我们的需要的模块自动配置好

2、手动在配置文件中配置部分配置项目就可以运行起来了

3、专注编写业务代码，不需要考虑以前那样一大堆的配置了

要熟悉掌握开发，之前学习的自动配置的原理一定要搞明白

比如SpringBoot到底帮我们配置了什么？我们能不能修改？我们能修改哪些配置？我们能不能扩展？

- 向容器中自动配置组件 ：**Autoconfiguration**
- 自动配置类，封装配置文件的内容：**Properties**

## 8.2 静态资源处理

### 8.2.1 静态资源映射规则

**首先，我们搭建一个普通的SpringBoot项目，回顾一下HelloWorld程序**！

写请求非常简单，那我们要引入我们前端资源，我们项目中有许多的静态资源，比如css，js等文件，这个SpringBoot怎么处理呢？

如果我们是一个web应用，我们的main下会有一个webapp，我们以前都是将所有的页面导在这里面的，对吧！但是我们现在的pom呢，打包方式是为jar的方式，那么这种方式SpringBoot能不能来给我们写页面呢？当然是可以的，但是SpringBoot对于静态资源放置的位置，是有规定的！

**静态资源映射规则**：

- SpringBoot中，SpringMVC的web配置都在 `WebMvcAutoConfiguration` (spring.factories中) 这个配置类里面；

- 我们可以去看看 `WebMvcAutoConfigurationAdapter` 中有很多配置方法；

- 有一个方法：`addResourceHandlers` 添加资源处理

  ```java
  @Override
  public void addResourceHandlers(ResourceHandlerRegistry registry) {
      if (!this.resourceProperties.isAddMappings()) {
          // 已禁用默认资源处理
          logger.debug("Default resource handling disabled");
          return;
      }
      // 缓存控制
      Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
      CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
      // webjars 配置
      if (!registry.hasMappingForPattern("/webjars/**")) {
          customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
                                               .addResourceLocations("classpath:/META-INF/resources/webjars/")
                                               .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
      }
      // 静态资源配置
      String staticPathPattern = this.mvcProperties.getStaticPathPattern();
      if (!registry.hasMappingForPattern(staticPathPattern)) {
          customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                                               .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                                               .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
      }
  }
  ```

  读一下源代码：比如所有的` /webjars/**` ， 都需要去 `classpath:/META-INF/resources/webjars/` 找对应的资源；

> 什么是webjars

Webjars本质就是以jar包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。

### 8.2.2 第一种静态资源映射规则

使用SpringBoot需要使用Webjars，我们可以去搜索一下：

网站：[https://www.webjars.org](https://gitee.com/link?target=https%3A%2F%2Fwww.webjars.org)

要使用jQuery，我们只要要引入jQuery对应版本的pom依赖即可！

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

导入完毕，查看webjars目录结构，并访问`jquery.js`文件

![image-20220331101345101](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fde22d82a3215824fe8bc6835d315691.png)

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：

http://localhost:8080/webjars/jquery/3.4.1/jquery.js![image-20220331101506669](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/558447143af0078da01d837c84f6cee5.png)

### 8.2.3 第二种静态资源映射规则

1、那我们项目中要是使用自己的静态资源该怎么导入呢？我们看下一行代码；

![image-20220331103730892](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/944078383e143e347c435a25d637f34a.png)

2、我们去找`staticPathPattern`发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 `resourceProperties` 这个类，我们可以点进去看一下分析：

```java
// 进入方法
public String[] getStaticLocations() {
    return this.staticLocations;
}
// 找到对应的值
private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
// 找到路径
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
    "classpath:/META-INF/resources/",
  	"classpath:/resources/", 
    "classpath:/static/", 
    "classpath:/public/" 
};
```

3、ResourceProperties 可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。

4、所以得出结论，以下四个目录存放的静态资源可以被我们识别：

```java
"classpath:/META-INF/resources/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
```

5、我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件；

![1595517831392](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f025be63d00ac2dc4e6106aedd7210c2.png)

6、比如我们访问 [http://localhost:8080/1.js](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2F1.js) , 他就会去这些文件夹中寻找对应的静态资源文件；

![1595517869049](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/73d66c6f1aa13c7bfb4a261971e7ad95.png)

### 8.2.4 自定义静态资源路径

我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在`application.properties`中配置；

```java
spring.resources.static-locations=classpath:/mystatic/,classpath:/mypath/
```

注意：一旦自定义了静态资源路径，原来的的就失效了，一般不会这么干，默认的就够用了！！！

自定义资源路径后再访问刚刚的静态资源，发现访问不到了。

![1595518276475](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fea0465faf93025276fc9e6ca3ba212b.png)

### 8.2.5 总结

1. 在springboot，我们可以使用以下方式处理静态资源
   - webjars：访问方式是`localhost:8080/webjars/xxxx`
   - public，static，/**，resources：访问方式是`localhost:8080/xxxx`
2. 优先级：resources > static（默认） > public

## 8.3 首页处理

静态资源文件夹说完后，我们继续向下看源码！可以看到一个欢迎页的映射，就是我们的首页！

```java
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                           FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
        new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
        this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
    return welcomePageHandlerMapping;
}
```

点进去继续看

```java
private Optional<Resource> getWelcomePage() {
    String[] locations = getResourceLocations(this.resourceProperties.getStaticLocations());
    // ::是java8 中新引入的运算符
    // Class::function的时候function是属于Class的，应该是静态方法。
    // this::function的funtion是属于这个对象的。
    // 简而言之，就是一种语法糖而已，是一种简写
    return Arrays.stream(locations).map(this::getIndexHtml).filter(this::isReadable).findFirst();
}
// 欢迎页就是一个location下的的 index.html 而已
private Resource getIndexHtml(String location) {
    return this.resourceLoader.getResource(location + "index.html");
}
```

截图说明：

![image-20220331110603307](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7acc6bc8f9e3e01b65c5cf87fec79f62.png)

继续往下看，找到`getWelcomePage()`方法：

![image-20220331111500520](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/661a1c254e26a74037b56fbe78adea71.png)



- 欢迎页，静态资源文件夹下的所有 index.html 页面；被 /** 映射。

- 比如我访问  [http://localhost:8080/](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2F) ，就会找静态资源文件夹下的 `index.html`

- 新建一个 `index.html` ，在我们上面的3个目录中任意一个；然后访问测试  [http://localhost:8080/](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2F)  看结果：

  ![image-20220331111620574](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/73cb64a3a2b8a866dc550796b3bedaaa.png)

## 8.4 网站图标

**欢迎页面(Welcome Page)**

> Spring Boot supports both static and templated welcome pages. It first looks for an `index.html` file in the configured static content locations. If one is not found, it then looks for an `index template`. If either is found, it is automatically used as the welcome page of the application.

**自定义应用图标（Custom Facicon）**

> Spring Boot looks for a `favicon.ico` in the configured  static content locations and the root of the classpath (in that order).  If such a file is present, it is automatically used as the favicon of  the application.



首页图标

与其他静态资源一样，SpringBoot在配置的静态内容位置中查找 `favicon.ico`。如果存在这样的文件，它将自动用作应用程序的favicon。

1. 自己放一个图标在静态资源目录static下

   ![image-20220331113207608](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d03e362d0fa778fd34c02109063e57ab.png)

2. 清除浏览器缓存`Ctrl + F5`！刷新网页，发现图标已经变成自己的了

   ![image-20220331113225439](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4ae4ca6249b98f5a2fa6041a88c89773.png)



# 9. Thymeleaf模板引擎

## 9.1 模板引擎

- 前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。
- jsp支持非常强大的功能，包括能写Java代码，但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，**他现在默认是不支持jsp的**。
- 那不支持jsp，如果我们直接用纯静态页面的方式，那给我们开发会带来非常大的麻烦，那怎么办呢？

**SpringBoot推荐你可以来使用模板引擎：**

模板引擎，我们其实大家听到很多，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot给我们推荐的Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢我们来看一下这张图：

![1595555521951](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d83f39e64ba251ccfab25fdee7945822.png)

模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。

我们呢，就来看一下这个模板引擎，那既然要看这个模板引擎。首先，我们来看SpringBoot里边怎么用。

## 9.2 引入Thymeleaf

怎么引入呢，对于SpringBoot来说，什么事情不都是一个start的事情嘛，我们去在项目中引入一下。给大家三个网址：

- Thymeleaf 官网：[https://www.thymeleaf.org/](https://gitee.com/link?target=https%3A%2F%2Fwww.thymeleaf.org%2F)
- Thymeleaf 在Github 的主页：[https://github.com/thymeleaf/thymeleaf](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fthymeleaf%2Fthymeleaf)
- Spring官方文档：找到我们对应的版本[https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter ](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2F2.2.5.RELEASE%2Freference%2Fhtmlsingle%2F%23using-boot-starter)

找到对应的pom依赖：注意对应Spring的版本

```xml
<!--thymeleaf-->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
```

Maven会自动下载jar包，我们可以去看下下载的东西；

![image-20220331114615254](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1128f4c3744a8b4f14fe1360a7b30a5d.png)

## 9.3 Thymeleaf分析

前面呢，我们已经引入了Thymeleaf，那这个要怎么使用呢？

我们首先得按照SpringBoot的自动配置原理看一下我们这个Thymeleaf的自动配置规则，在按照那个规则，我们进行使用。

我们去找一下Thymeleaf的自动配置类：ctrl+n 全局搜索`ThymeleafProperties`

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";

	private boolean checkTemplate = true;

	private boolean checkTemplateLocation = true;

	private String prefix = DEFAULT_PREFIX;

	private String suffix = DEFAULT_SUFFIX;

	private String mode = "HTML";

	private Charset encoding = DEFAULT_ENCODING;
        
        /.......
```

我们可以在其中看到默认的前缀和后缀！

我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。

使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

**测试**

1. 编写一个TestController

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/test")
       public String test() {
           //解析，自动加上前缀和后缀：classpath:/templates/leaf.html
           return "leaf";
       }
   }
   ```

2. 编写一个测试页面  test.html 放在 templates 目录下

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
       <h1>Thymeleaf页面</h1>
   </body>
   </html>
   ```

3. 启动项目请求测试

   ![image-20220331120015992](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9019b29b4dd37675c86e028c2b6b64a1.png)

## 9.4 Thymeleaf语法

要学习语法，还是参考官网文档最为准确，我们找到对应的版本看一下；

Thymeleaf 官网：[https://www.thymeleaf.org/](https://gitee.com/link?target=https%3A%2F%2Fwww.thymeleaf.org%2F) 

在线文档：[https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html](https://gitee.com/link?target=https%3A%2F%2Fwww.thymeleaf.org%2Fdoc%2Ftutorials%2F3.0%2Fusingthymeleaf.html)

### 9.4.1 Thymeleaf入门

**我们做个最简单的练习 ：我们需要查出一些数据，在页面中展示**

1. 修改测试Controller请求，增加数据传输；

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/test")
       public String test(Model model) {
           //存入数据
           model.addAttribute("msg", "Hello, Thymeleaf");
           //classpath:/templates/leaf.html
           return "leaf";
       }
   }
   ```

2. 我们要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示。

   我们可以去官方文档的#3中看一下命名空间拿来过来：

   ```html
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   ```

3. 我们去编写下前端页面

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Thymeleaf</title>
   </head>
   <body>
       <h1>Thymeleaf页面</h1>
   
       <!--所有的html标签都可以被thymeleaf接管，用th:标签名即可-->
       <!--th:text就是将div中的内容设置为它指定的值，和之前学习的Vue一样-->
       <div th:text="${msg}"></div>
   </body>
   </html>
   ```

4. 启动测试

   ![image-20220331124537066](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2a6736a8cf26bfe4a8d569fe3db66f26.png)

**OK，入门搞定，接下来认真研究一下Thymeleaf的使用语法！**

### 9.4.2 Thymeleaf语法

**1、我们可以使用任意的 th:attr 来替换Html中原生属性的值！**

**2、我们能写哪些表达式呢？**

```
Simple expressions:（表达式语法）
Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：#18
         #ctx : the context object.
         #vars: the context variables.
         #locale : the context locale.
         #request : (only in Web Contexts) the HttpServletRequest object.
         #response : (only in Web Contexts) the HttpServletResponse object.
         #session : (only in Web Contexts) the HttpSession object.
         #servletContext : (only in Web Contexts) the ServletContext object.

    3）、内置的一些工具对象：
　　　　　　#execInfo : information about the template being processed.
　　　　　　#uris : methods for escaping parts of URLs/URIs
　　　　　　#conversions : methods for executing the configured conversion service (if any).
　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
　　　　　　#numbers : methods for formatting numeric objects.
　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
　　　　　　#objects : methods for objects in general.
　　　　　　#bools : methods for boolean evaluation.
　　　　　　#arrays : methods for arrays.
　　　　　　#lists : methods for lists.
　　　　　　#sets : methods for sets.
　　　　　　#maps : methods for maps.
　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
==================================================================================

  Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
  Message Expressions: #{...}：获取国际化内容
  Link URL Expressions: @{...}：定义URL；
  Fragment Expressions: ~{...}：片段引用表达式

Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
      
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
    
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
    
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
    
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
    
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
    
Special tokens:
    No-Operation: _
```





**练习测试：**

1、 我们编写一个Controller，放一些数据

```java
@Controller
public class TestController {
    @RequestMapping("/test")
    public String test(Map<String, Object> map) {
        //使用Map存数据，前端一样可以取出
        map.put("msg", "<h1>Hello, Thymeleaf</h1>");
        map.put("users", Arrays.asList("张三", "李四", "王五"));
        return "leaf";
    }
}
```

2、测试页面取出数据

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Thymeleaf</title>
</head>
<body>
    <h1>Thymeleaf语法</h1>
    <hr>
    <!--不转义-->
    <div th:text="${msg}"></div>
    <!--转义-->
    <div th:utext="${msg}"></div>

    <hr>
    <h1>遍历数据</h1>
    <!--th:each每次遍历都会生成当前这个标签：官网#9-->
    <h2 th:each="user:${users}" th:text="${user}"></h2>
    <hr>
    <!--行内写法：官网#12-->
    <h2 th:each="user:${users}">[[ ${user} ]]</h2>

</body>
</html>
```

3、启动项目测试

![image-20220331130102379](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2ed0cff5fe693b165c40b4364f19c634.png)

**我们看完语法，很多样式，我们即使现在学习了，也会忘记，所以我们在学习过程中，需要使用什么，根据官方文档来查询，才是最重要的，要熟练使用官方文档！**





# 10. MVC自动配置原理

## 10.1 官网阅读

在进行项目编写前，我们还需要知道一个东西，就是SpringBoot对我们的SpringMVC还做了哪些配置，包括如何扩展，如何定制。

只有把这些都搞清楚了，我们在之后使用才会更加得心应手。途径一：源码分析，途径二：官方文档！

地址 ：[https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2F2.2.5.RELEASE%2Freference%2Fhtmlsingle%2F%23boot-features-spring-mvc-auto-configuration)

```text
SpringMVC Auto-configuration
// Spring Boot为Spring MVC提供了自动配置，它可以很好地与大多数应用程序一起工作。
SpringBoot provides auto-configuration for Spring MVC that works well with most applications.
// 自动配置在Spring默认设置的基础上添加了以下功能：
The auto-configuration adds the following features on top of Spring’s defaults:
// 包含视图解析器
Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
// 支持静态资源文件夹的路径，以及webjars
Support for serving static resources, including support for WebJars 
// 自动注册了Converter：
// 转换器，这就是我们网页提交数据到后台自动封装成为对象的东西，比如把"1"字符串自动转换为int类型
// Formatter：【格式化器，比如页面给我们了一个2019-8-10，它会给我们自动格式化为Date对象】
Automatic registration of Converter, GenericConverter, and Formatter beans.
// HttpMessageConverters
// SpringMVC用来转换Http请求和响应的的，比如我们要把一个User对象转换为JSON字符串，可以去看官网文档解释；
Support for HttpMessageConverters (covered later in this document).
// 定义错误代码生成规则的
Automatic registration of MessageCodesResolver (covered later in this document).
// 首页定制
Static index.html support.
// 图标定制
Custom Favicon support (covered later in this document).
// 初始化数据绑定器：帮我们把请求数据绑定到JavaBean中！
Automatic use of a ConfigurableWebBindingInitializer bean (covered later in this document).

/*
如果您希望保留SpringBoot MVC功能，并且希望添加其他MVC配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己
的@configuration类，类型为webmvcconfiguer，但不添加@EnableWebMvc。如果希望提供
RequestMappingHandlerMapping、RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver的自定义
实例，则可以声明WebMVCregistrationAdapter实例来提供此类组件。
*/
If you want to keep Spring Boot MVC features and you want to add additional MVC configuration 
(interceptors, formatters, view controllers, and other features), you can add your own 
@Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide 
custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or 
ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

// 如果您想完全控制Spring MVC，可以添加自己的@Configuration，并用@EnableWebMvc进行注释。
If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc.
```

我们来仔细对照，看一下它怎么实现的，它告诉我们SpringBoot已经帮我们自动配置好了SpringMVC，然后自动配置了哪些东西呢？

## 10.2 内容协商视图解析器

**ContentNegotiatingViewResolver**

- 自动配置了ViewResolver，就是我们之前学习的SpringMVC的视图解析器；

- 即根据方法的返回值取得视图对象（View），然后由视图对象决定如何渲染（转发，重定向）。

- 我们去看看这里的源码：我们找到 `WebMvcAutoConfiguration` ， 然后搜索`ContentNegotiatingViewResolver`。找到如下方法！

  ```java
  @Bean
  @ConditionalOnBean(ViewResolver.class)
  @ConditionalOnMissingBean(name = "viewResolver", value = ContentNegotiatingViewResolver.class)
  public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory) {
      ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();
      resolver.setContentNegotiationManager(beanFactory.getBean(ContentNegotiationManager.class));
      // ContentNegotiatingViewResolver使用所有其他视图解析器来定位视图，因此它应该具有较高的优先级
      resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
      return resolver;
  }
  ```

- 我们可以点进这类看看！找到对应的解析视图的代码；

  ```java
  @Nullable // 注解说明：@Nullable 即参数可为null
  public View resolveViewName(String viewName, Locale locale) throws Exception {
      RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
      Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
      List<MediaType> requestedMediaTypes = this.getMediaTypes(((ServletRequestAttributes)attrs).getRequest());
      if (requestedMediaTypes != null) {
          // 获取候选的视图对象
          List<View> candidateViews = this.getCandidateViews(viewName, locale, requestedMediaTypes);
          // 选择一个最适合的视图对象，然后把这个对象返回
          View bestView = this.getBestView(candidateViews, requestedMediaTypes, attrs);
          if (bestView != null) {
              return bestView;
          }
      }
      // .....
  }
  ```

- 我们继续点进去看，他是怎么获得候选的视图的呢？

  getCandidateViews中看到他是把所有的视图解析器拿来，进行while循环，挨个解析！

  ```java
  Iterator var5 = this.viewResolvers.iterator();
  ```

  所以得出结论：**ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的**

- 我们再去研究下他的组合逻辑，看到有个属性viewResolvers，看看它是在哪里进行赋值的！

  ```java
  protected void initServletContext(ServletContext servletContext) {
      // 这里它是从beanFactory工具中获取容器中的所有视图解析器
      // ViewRescolver.class 把所有的视图解析器来组合的
      Collection<ViewResolver> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(this.obtainApplicationContext(), ViewResolver.class).values();
      ViewResolver viewResolver;
      if (this.viewResolvers == null) {
          this.viewResolvers = new ArrayList(matchingBeans.size());
      }
      // ...............
  }
  ```

- 既然它是在容器中去找视图解析器，我们是否可以猜想，我们就可以去实现一个视图解析器了呢？





**自定义视图解析器**：

我们可以自己给容器中去添加一个视图解析器；这个类就会帮我们自动的将它组合进来

1. 我们在我们的主程序中去写一个视图解析器来试试；

   ```java
   //扩展 springmvc      DispatchServlet
   @Configuration
   public class MyMvcConfig implements WebMvcConfigurer {
       // public interface ViewResolver 实现了视图解析器接口的类，我们就可以吧它看做视图解析器
   
       @Bean
       public ViewResolver myViewResolver() {
           return new MyViewResolver();
       }
       // 自定义了一个自己的视图解析器
       public static class MyViewResolver implements ViewResolver {
   
           @Override
           public View resolveViewName(String s, Locale locale) throws Exception {
   
               return null;
           }
       }
   }
   ```

2. 我们给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

   ![1595564720595](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f0c4c188c426083c5874ce441d3db6ed.png)

3. 我们启动我们的项目，然后随便访问一个页面，看一下Debug信息；

   找到this(就是DispatcherServlet)

   ![1595564823239](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/829c52112cc328abbb395eb3bc0008e9.png)

   找到视图解析器（viewResolvers），我们看到我们自己定义的就在这里了；

   ![1595564942873](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/40b901100bc52e8e652f3b370cc23ef1.png)

- 所以说，我们如果想要使用自己定制化的东西，我们只需要给容器中添加这个组件就好了！剩下的事情SpringBoot就会帮我们做了！

## 10.3 转换器和格式化器

- 在`WebMvcAutoConfiguration`中找到格式化转换器：

  ```java
  @Bean
  @Override
  public FormattingConversionService mvcConversionService() {
      // 拿到配置文件中的格式化规则
      WebConversionService conversionService = 
          new WebConversionService(this.mvcProperties.getDateFormat());
      addFormatters(conversionService);
      return conversionService;
  }
  ```

- 点击去：可以看到在我们的Properties文件中，我们可以进行自动配置它！

  - 2.2.x之前版本

    ```java
    public String getDateFormat() {
        return this.dateFormat;
    }
    
    /**
    * Date format to use. For instance, `dd/MM/yyyy`. 官方默认的
     */
    private String dateFormat;
    ```

  - 2.2.x之后的版本

  ```java
  public String getDateFormat() {
      return this.format.getDate();
  }
  
  public String getDate() {
      return this.date;
  }
  
  /**
  	* Date format to use, for example `dd/MM/yyyy`.默认的
  */
  private String date;
  ```

- 如果配置了自己的格式化方式，就会注册到Bean中生效，我们可以在配置文件中配置日期格式化的规则：

  - 2.2.x版本之前的

    配置文件：

    ```properties
    # 配置文件
    spring.nvc.date-format=
    ```

    源码：

    ```java
    @Deprecated
    public void setDateFormat(String dateFormat) {
        this.dateFormat = dateFormat;
    }
    ```

  - 2.2.x版本之后的

    配置文件：

    ```properties
    spring.nvc.date=
    ```

    源码：

    ```java
    @Deprecated
    public void setDateFormat(String dateFormat) {
        this.format.setDate(dateFormat);
    }
    
    public void setDate(String date) {
        this.date = date;
    }
    ```

其余的就不一一举例了，可以自己去多研究探讨。

## 10.4 修改SpringBoot的默认配置

- 这么多的自动配置，原理都是一样的，通过这个WebMVC的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。
- SpringBoot的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；
- SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；
- 如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！





> 扩展使用SpringMVC

- 官方文档如下：

  If you want to keep Spring Boot MVC features and you want to add  additional MVC configuration (interceptors, formatters, view  controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` **but without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

- 我们要做的就是编写一个`@Configuration`注解类，并且类型要为`WebMvcConfigurer`，一定**不能标注**`@EnableWebMvc`注解，否则默认配置类全部失效；

- 我们去自己写一个；新建一个包叫config，写一个类MyMvcConfig；

  ```java
  // 如果我们要扩展springmvc，官方建议使用@Configuration
  //因为类型要求为WebMvcConfigurer，所以我们实现其接口
  //扩展 springmvc      DispatchServlet
  //@EnableWebMvc		 //这个注解就是导入了一个类，DelegatingWebMvcConfiguration，从容器中获取所有的webMvcConfig
  
  @Configuration
  public class MyMvcConfig implements WebMvcConfigurer {
  
      @Override
      public void addViewControllers(ViewControllerRegistry registry) {
          // 浏览器发送/test2，就会跳转到test页面；
          registry.addViewController("/test2").setViewName("test");
      }
  }
  ```

- 在resources的templates下新建`test.html`，我们去浏览器访问一下：

  ![image-20220331212750840](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bd8851c21937956b4a5abe5f804f8100.png)

**确实也跳转过来了！所以说，我们要扩展SpringMVC，官方就推荐我们这么去使用，既保SpringBoot留所有的自动配置，也能用我们扩展的配置！**

我们可以去分析一下原理：

1. `WebMvcAutoConfiguration` 是 SpringMVC的自动配置类，里面有一个类`WebMvcAutoConfigurationAdapter`

2. 这个类上有一个注解，在做其他自动配置时会导入：`@Import(EnableWebMvcConfiguration.class)`

3. 我们点进`EnableWebMvcConfiguration`这个类看一下，它继承了一个父类：`DelegatingWebMvcConfiguration`

   这个父类中有这样一段代码：

   ```java
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
       private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
       
     // 从容器中获取所有的webmvcConfigurer
       @Autowired(required = false)
       public void setConfigurers(List<WebMvcConfigurer> configurers) {
           if (!CollectionUtils.isEmpty(configurers)) {
               this.configurers.addWebMvcConfigurers(configurers);
           }
       }
   }
   ```

4. 我们可以在这个类中去寻找一个我们刚才设置的viewController当做参考，发现它调用了一个

   ```java
   protected void addViewControllers(ViewControllerRegistry registry) {
       this.configurers.addViewControllers(registry);
   }
   ```

5. 我们点进去看一下

   ```java
   public void addViewControllers(ViewControllerRegistry registry) {
       Iterator var2 = this.delegates.iterator();
   
       while(var2.hasNext()) {
           // 将所有的WebMvcConfigurer相关配置来一起调用！包括我们自己配置的和Spring给我们配置的
           WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
           delegate.addViewControllers(registry);
       }
   
   }
   ```

**得出结论**：所有的`WebMvcConfiguration`都会被作用，不止Spring自己的配置类，我们自己的配置类当然也会被调用；

## 10.5 全面接管SpringMVC

- 官方文档：

  ```text
  If you want to take complete control of Spring MVC
  you can add your own @Configuration annotated with @EnableWebMvc.
  ```

- 全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

- 只需在我们的配置类中要加一个`@EnableWebMvc`。

- 如果我们全面接管了SpringMVC了，之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

- 不加注解之前，访问首页：

  ![image-20220331213206709](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dcebce26ee1d24ecf012b9f50ec5eb6f.png)

- 给配置类加上注解：`@EnableWebMvc`

  我们发现所有的SpringMVC自动配置都失效了：

  ![image-20220331213312358](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/52342131894e6a68ad5c73a59976f2f0.png)

**当然，我们开发中，不推荐使用全面接管SpringMVC**

> 为什么加了一个注解，自动配置就失效了

1. 这里发现它是导入了一个类，我们可以继续进去看

   ```java
   @Import({DelegatingWebMvcConfiguration.class})
   public @interface EnableWebMvc {
   }
   ```

2. 它继承了一个父类 WebMvcConfigurationSupport

   ```java
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
     // ......
   }
   ```

3. 我们来回顾一下Webmvc自动配置类

   ```java
   @Configuration(proxyBeanMethods = false)
   @ConditionalOnWebApplication(type = Type.SERVLET)
   @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
   // 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
   @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
   @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
   @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
       ValidationAutoConfiguration.class })
   public class WebMvcAutoConfiguration {
       
   }
   ```

**总结：**

- `@EnableWebMvc`将`WebMvcConfigurationSupport`组件导入进来了；
- 导入的`WebMvcConfigurationSupport`只是SpringMVC最基本的功能！
- **在springboot中，有非常多的xxxxconfigure帮助我们进行扩展配置，只要看到这个东西就要注意了**





