# 1. SpringBoot简介

## 1.1 回顾什么是Spring

- Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson  。
- **Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

## 1.2 Spring是如何简化Java开发的

^d98971


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





# 11. 员工管理系统

## 11.1 准备工作

1. 导入相关静态资源

   ![image-20220331215243683](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220331215243683.png)

2. pojo层：部门类、员工类

   ```java
   //部门类
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Department {
       private Integer id;
       private String departmentName;
   }
   
   //员工类
   @Data
   @NoArgsConstructor
   public class Employee {
       private Integer id;
       private String lastName;
       private String email;
       private Integer gender;     //0女 1男
       private Date birth;
       private  Department department;
   
       //自定义有参，birth缺省，下面自动创建
       public Employee(Integer id, String lastName, String email, Integer gender, Department department) {
           this.id = id;
           this.lastName = lastName;
           this.email = email;
           this.gender = gender;
           this.birth = new Date();
           this.department = department;
       }
   }
   ```

3. dao层：

   DepartmentDao: 

   ```java
   @Repository     //交给Spring托管
   public class DepartmentDao {
   
       //模拟数据库中的数据
       @Autowired      //自动装配
       private static Map<Integer, Department> departments = null;
       static {
           departments = new HashMap<Integer, Department>();   //创建一个部门
           departments.put(101, new Department(101, "教学部"));
           departments.put(102, new Department(102, "市场部"));
           departments.put(103, new Department(103, "教研部"));
           departments.put(104, new Department(104, "运营部"));
           departments.put(105, new Department(105, "后勤部"));
       }
   
       //获得所有部门信息
       public Collection<Department> getDepartments() {
           return departments.values();
       }
   
       //通过id获得部门
       public Department getDepartmentById(Integer id) {
           return departments.get(id);
       }
   
   }
   ```

   EmployeeDao: 

   ```java
   @Repository
   public class EmployeeDao {
   
       //模拟数据库中的数据
       private static Map<Integer, Employee> employees = null;
   
       @Autowired
       private DepartmentDao departmentDao;    //员工所属部门
   
       static {
           employees = new HashMap<Integer, Employee>();   //创建一个部门
           employees.put(1001, new Employee(1001, "小AA", "444444@qq.com", 1, new Department(101, "教学部")));
           employees.put(1002, new Employee(1002, "小BB", "555555@qq.com", 0, new Department(102, "市场部")));
           employees.put(1003, new Employee(1003, "小CC", "666666@qq.com", 1, new Department(103, "教研部")));
           employees.put(1004, new Employee(1004, "小DD", "777777@qq.com", 0, new Department(104, "运营部")));
           employees.put(1005, new Employee(1005, "小EE", "888888@qq.com", 1, new Department(105, "后勤部")));
       }
   
       //增加一个员工
       //主键自增
       private static Integer initId = 1006;
       public void save(Employee employee) {
           if (employee.getId() == null) {
               employee.setId(initId++);
           }
   
           employee.setDepartment(departmentDao.getDepartmentById(employee.getDepartment().getId()));
   
           employees.put(employee.getId(), employee);
       }
   
   
       //获得所有员工信息
       public Collection<Employee> getAll() {
           return employees.values();
       }
   
       //通过id获得员工
       public Employee getEmployeeById(Integer id) {
           return employees.get(id);
       }
   
       //通过id删除员工
       public void delete(Integer id) {
           employees.remove(id);
       }
   }
   ```





## 11.2 首页展示

1. 可以在controller里面配置，也可以实现WebMvcConfiguration接口配置，这里采用第二种，方便回顾。在config包下新建MyMvcConfig：

   ```java
   @Configuration
   public class MyMvcConfig implements WebMvcConfigurer {
       @Override
       public void addViewControllers(ViewControllerRegistry registry) {
           //可以配置多个
           registry.addViewController("/").setViewName("index");
           registry.addViewController("/index.html").setViewName("index");
       }
   }
   ```

2. 将静态文件中相关标签全部改成Thymeleaf的模板，示例如下：

   ![image-20220401123042971](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401123042971.png)

3. 启动项目，查看首页：

   ![image-20220331225852489](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220331225852489.png)

## 11.3 页面国际化

有的时候，我们的网站会去涉及中英文甚至多语言的切换，这时候我们就需要学习国际化了！

### 11.3.1 准备工作

先在IDEA中统一设置properties的编码问题！

![image-20220331213516288](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220331213516288.png)

编写国际化配置文件，抽取页面需要显示的国际化页面消息。

### 11.3.2 配置文件编写

1. 我们在resources资源文件下新建一个`i18n`（internationalization缩写）目录，存放国际化配置文件

2. 建立一个`login.properties`文件，还有一个`login_zh_CN.properties`；发现IDEA自动识别了我们要做国际化操作；文件夹变了！

   ![image-20220401102524302](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401102524302.png)

3. 我们可以在这上面去新建一个文件；

   ![image-20220401102553866](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401102553866.png)

   弹出如下页面：我们再添加一个英文的；

   ![image-20220401102620870](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401102620870.png)

   这样就快捷多了！

   ![image-20220401102643052](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401102643052.png)

4. **接下来，我们就来编写配置，我们可以看到idea下面有另外一个视图**

   ![image-20220401105424073](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401105424073.png)

   这个视图我们点击 + 号就可以直接添加属性了；我们新建一个`login.tip`，可以看到边上有三个文件框可以输入

   ![image-20220401105512451](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401105512451.png)

   然后依次添加其他页面内容即可！

   

   去查看我们的配置文件`login.properties` ：

   默认：

   ```properties
   login.btn=登录
   login.password=密码
   login.remember=记住我
   login.tip=请登录
   login.username=用户名
   ```

   英文：

   ```properties
   login.btn=Login in
   login.password=Password
   login.remember=Remember me
   login.tip=Please Login in
   login.username=Username
   ```

   中文：

   ```properties
   login.btn=登录
   login.password=密码
   login.remember=记住我
   login.tip=请登录
   login.username=用户名
   ```

   配置文件步骤搞定！

### 11.3.3 配置文件生效探究

我们去看一下SpringBoot对国际化的自动配置！这里又涉及到一个类：`MessageSourceAutoConfiguration`

里面有一个方法，这里发现SpringBoot已经自动配置好了管理我们国际化资源文件的组件 `ResourceBundleMessageSource`；

```java
// 获取 properties 传递过来的值进行判断
@Bean
public MessageSource messageSource(MessageSourceProperties properties) {
    ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
    if (StringUtils.hasText(properties.getBasename())) {
        // 设置国际化文件的基础名（去掉语言国家代码的）
        messageSource.setBasenames(
            StringUtils.commaDelimitedListToStringArray(
                                       StringUtils.trimAllWhitespace(properties.getBasename())));
    }
    if (properties.getEncoding() != null) {
        messageSource.setDefaultEncoding(properties.getEncoding().name());
    }
    messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale());
    Duration cacheDuration = properties.getCacheDuration();
    if (cacheDuration != null) {
        messageSource.setCacheMillis(cacheDuration.toMillis());
    }
    messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat());
    messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessage());
    return messageSource;
}
```

我们真实的情况是放在了i18n目录下，所以我们要去`application.properties`配置这个messages的路径：

```properties
#配置文件的真实位置
spring.messages.basename=i18n.login
```

### 11.3.4 配置页面国际化值

去页面获取国际化的值，查看Thymeleaf的文档，找到message取值操作为：`#{...}`。我们去index首页进行修改：

![image-20220401111528571](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401111528571.png)

我们可以去启动项目，访问一下，发现已经自动识别为中文的了！

![image-20220401111547450](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401111547450.png)

**接下来取配置根据按钮自动切换中文英文**

### 11.3.5 配置国际化解析

在Spring中有一个国际化的Locale （区域信息对象）；里面有一个叫做`LocaleResolver `（获取区域信息对象）的解析器！

我们去我们webmvc自动配置文件，寻找一下！看到SpringBoot默认配置：

```java
@Bean
@ConditionalOnMissingBean
@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
public LocaleResolver localeResolver() {
    // 用户配置了就用用户的，没有就用下面默认的
    if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
        return new FixedLocaleResolver(this.mvcProperties.getLocale());
    }
    // 接收头国际化分解
    AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
    localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
    return localeResolver;
}
```

`AcceptHeaderLocaleResolver` 这个类中有一个方法，判断返回什么语言

```java
public Locale resolveLocale(HttpServletRequest request) {
    Locale defaultLocale = this.getDefaultLocale();
    // 默认的就是根据请求头带来的区域信息获取Locale进行国际化
    if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
        return defaultLocale;
    } else {
        Locale requestLocale = request.getLocale();
        List<Locale> supportedLocales = this.getSupportedLocales();
        if (!supportedLocales.isEmpty() && !supportedLocales.contains(requestLocale)) {
            Locale supportedLocale = this.findSupportedLocale(request, supportedLocales);
            if (supportedLocale != null) {
                return supportedLocale;
            } else {
                return defaultLocale != null ? defaultLocale : requestLocale;
            }
        } else {
            return requestLocale;
        }
    }
}
```

那假如我们现在想点击链接让我们自己的国际化资源生效，就需要让我们自己的Locale生效！

我们去自己写一个自己的`LocaleResolver`，可以在链接上携带区域信息！

修改一下前端页面的跳转连接：

```html
<!-- 这里传入参数不需要使用 ？使用 （key=value）-->
<a class="btn btn-sm" th:href="@{/index.html(language='zh_CN')}">中文</a>
			<a class="btn btn-sm" th:href="@{/index.html(language='en_US')}">English</a>
```

去config包下新建MyLocaleResolver类，实现LocaleResolver接口，仿照源码写一个处理的组件类：

```java
package nuc.ss.component;

import org.springframework.util.StringUtils;
import org.springframework.web.servlet.LocaleResolver;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

//可以在链接上携带区域信息
public class MyLocaleResolver implements LocaleResolver {

 //解析请求
 @Override
 public Locale resolveLocale(HttpServletRequest request) {

     String language = request.getParameter("l");
     Locale locale = Locale.getDefault(); // 如果没有获取到就使用系统默认的
     //如果请求链接不为空
     if (!StringUtils.isEmpty(language)){
         //分割请求参数
         String[] split = language.split("_");
         //国家，地区
         locale = new Locale(split[0],split[1]);
     }
     return locale;
 }

 @Override
 public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

 }
}
```

为了让我们的区域化信息能够生效，我们需要在配置类中注入一下这个组件！在我们自己的`MyMvcConofig`下添加`bean`；

```java
@Bean
public LocaleResolver localeResolver(){
    return new MyLocaleResolver();
}
```

**我们重启项目，来访问一下，发现点击按钮可以实现成功切换**

![image-20220401114001820](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401114001820.png)

### 11.3.6 小结

1. 首页配置：
   - 注意点，所有页面的静态资源都需要使用Thymeleaf接管
   - url： `@{}`
2. 页面国际化
   - 我们需要配置`i18n`文件
   - 我们如果需要在项目中进行按钮自动切换，我们需要定义一个组件`LocalResolver`
   - 记得将自己写的组件配置到spring容器`@Bean`
   - 值：`#{}`



## 11.4 登录功能

### 11.4.1 登录

1. 在`index.html`首页中的form表单上添加一个事件，让用户点击登录的时候把信息传入对应的后台。

   ```html
   <form class="form-signin" th:action="@{/user/login}">
   ......
   </form>
   ```

2. 编写对应的controller层：`LoginController`

   ```java
   @Controller
   public class LoginController {
   
       @RequestMapping("/user/login")
       public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model) {
           //具体的业务
           if (!StringUtils.isEmpty(username) && "123456".equals(password)) {
               //重定向到main.html（dashboard页面）
               return "redirect:/main.html";
           } else {
               //登录失败将信息传递回去
               model.addAttribute("error", "用户名或密码错误");
               return "index";
           }
       }
   }
   ```

3. 因为登录成功之后重定向到`main.html`页面，所有去`MyMvcConfig`中配置它的路径：

   ```java
   //登录成功页面
   registry.addViewController("/main.html").setViewName("dashboard");
   ```

4. 登录失败页面提示：

   ```html
   <!--如果error为空，则不显示提示-->
   <p style="color: red" th:text="${error}" th:if="${! #strings.isEmpty(error)}"></p>
   ```

5. 测试，可以发现登录成功后url是`main.html`，实际还是dashboard页面：

   ![image-20220401124241604](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401124241604.png)



### 11.4.2 登录拦截器

登录写好后，发现在地址栏输入`mian.html`时，无论已经登录与否，都能跳转到主页面，所以需要做登录拦截。

1. 要写登录拦截器，首先需要登录成功后用户的Session，所以先去`LoginController`类中将用户信息存入Session：

   ```java
   @RequestMapping("/user/login")
       public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model, HttpSession session) {
           //具体的业务
           if (!StringUtils.isEmpty(username) && "123456".equals(password)) {
               //存入Session
               session.setAttribute("loginUser", username);
               //重定向到main.html（dashboard页面）
               return "redirect:/main.html";
           } else {
               //登录失败将信息传递回去
               model.addAttribute("error", "用户名或密码错误");
               return "index";
           }
       }
   ```

2. 在config包下新建`LoginHandlerInterception`类，实现HandlerInterception接口：

   ```java
   public class LoginHandlerInterception implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //获取用户Session
           Object loginUser = request.getSession().getAttribute("loginUser");
   
           if (loginUser == null) {
               request.setAttribute("error", "请先登录才能进主页哦！");
               request.getRequestDispatcher("/index.html").forward(request, response);
               return false;
           } else {
               return true;     //放行
           }
       }
   }
   ```

3. 去`MyMvcConfig`类中配置拦截器，重写`addInterceptors`方法

   ```java
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           //拦截全部请求路径，除了excludePathPatterns中的登录页面和静态资源
           registry.addInterceptor(new LoginHandlerInterception())
                   .addPathPatterns("/**")
                   .excludePathPatterns("/index.html", "/", "/user/login", "/static/*", "/img/*");
       }
   ```

4. 测试：

   ![image-20220401135154331](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401135154331.png)

5. 登录成功后，可以通过session拿到用户的名字，在主页中显示，在`dashboard.html`中对应位置获取即可：

   ```html
   <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>
   ```

6. 测试：

   ![image-20220401135736113](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401135736113.png)



## 11.5 查询功能

在写查询功能之前，先思考以下，当我们点击管理员工时，页面的左边栏和上边栏应该是保持不变的，只需要将员工列表对应的页面插入到中间位置即可。所以我们要先处理公共部分。

### 11.5.1 提取公共部分

1. 在templates文件夹下新建commons文件夹，创建一个`commons.html`，存放公共的页面。：

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   
   <!--头部导航栏-->
   <!--th:fragment是将多个地方出现的元素块用fragment包起来使用，方便在其他地方直接调用此元素块-->
   <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar">
       <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>
       <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
       <ul class="navbar-nav px-3">
           <li class="nav-item text-nowrap">
               <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">注销</a>
           </li>
       </ul>
   </nav>
   
   <!--侧边栏-->
   <nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar">
       <div class="sidebar-sticky">
           <ul class="nav flex-column">
               <li class="nav-item">
                   <a class="nav-link" th:href="@{/index.html}">
                       首页 <span class="sr-only">(current)</span>
                   </a>
               </li>
               
               <!--中间还有很多li标签-->
               
               <li class="nav-item">
                   <a class="nav-link" th:href="@{/emps}">
                       员工管理
                   </a>
               </li>
           </ul>
       </div>
   </nav>
   
   </html>
   ```

2. 在`dashboard.html`主页面和`list.html`员工列表页面中的对应位置将topbar和sidebar抽取过来，下面给出主页面的示例：

   ```html
   <body>
   
   		<!--将commons文件下的commons.html中的topbar调用过来-->
   		<div th:replace="~{commons/commons::topbar}"></div>
   
   		<div class="container-fluid">
   			<div class="row">
   
   				<!--将公共部分的sidebar调用过来-->
   				<div th:replace="~{commons/commons::sidebar}"></div>
   
   				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
   
   <!-- .......... -->
   ```

3. 提取公共部分已完成。



### 11.5.2 员工列表查询

1. 在templates目录下新建employee目录，将`list.html`放入，方便管理

2. 编写`EmployeeController`类，因为现在用的是伪数据库数据，所以暂时忽略Service层，让Controller层之间调Dao层：

   ```java
   @Controller
   public class EmployeeController {
   
       @Autowired
       EmployeeDao employeeDao;
   
       @RequestMapping("/emps")
       public String list(Model model) {
           Collection<Employee> employees = employeeDao.getAll();
           model.addAttribute("emps", employees);
           return "employee/list";
       }
   
   }
   ```

3. 在公共部分`commons.html`页面中对应位置编写此请求：

   ```html
   	   <li class="nav-item">
                   <a class="nav-link" th:href="@{/emps}">
                       员工管理
                   </a>
               </li>
   ```

4. 先测试一下：

   ![image-20220401145922383](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401145922383.png)

5. 在点击员工管理后，确实跳转到了`list.html`页面，但是员工管理的位置没有高亮，先解决一下这个问题。

6. 在其他页面调用公共页面的时候，可以在其他页面携带一个参数再调用公共页面，这样就可以在公共页面中直到是哪个页面调用了它。修改`dashboard.html`和`list.html`中调用公共页面的代码：

   ```html
   <!--将公共部分的sidebar调用过来，并且携带一个active参数过去，方便公共部分页面直到是谁调用了它-->
   <div th:replace="~{commons/commons::sidebar(active='main.html')}"></div>
   ```

   

   ```html
   <!--将公共部分的sidebar调用过来-->
   <div th:replace="~{commons/commons::sidebar(active='list.html')}"></div>
   ```

7. 在公共页面的对应位置处接收其他页面传过来的参数，然后做判断，看是哪个页面调用了它，就给哪个页面的高亮激活：

   ```html
   <!--首页位置-->
   <!--取其他页面传过来的active，如果与之对应，则激活高亮，否则显示默认-->
   <a th:class="${active=='main.html'? 'nav-link active':'nav-link'}" th:href="@{/index.html}">
       首页 <span class="sr-only">(current)</span>
   </a>
   
   <!--员工管理位置-->
   <a th:class="${active=='list.html'?'nav-link active':'nav-link'}" th:href="@{/emps}">
        员工管理
   </a>
   ```

8. 测试，可以发现，刚进入登录页面，是首页高亮，当点击员工管理时，员工管理高亮：

   ![image-20220401152205641](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401152205641.png)

   ![image-20220401152224972](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401152224972.png)

   

> 表格修改，数据展示

好了，现在开始修改`list.html`页面中的表格，并且把后端传过来的员工展示出来。

```html
<table class="table table-striped table-sm">
    <thead>
        <tr>
            <th>id</th>
            <th>lastName</th>
            <th>email</th>
            <th>gender</th>
            <th>birth</th>
            <th>department</th>
            <th>operate</th>
        </tr>
    </thead>
    <tbody>
        <!--将后端传过来的emps遍历出来-->
        <tr th:each="emp:${emps}">
            <td th:text="${emp.getId()}"></td>
            <td th:text="${emp.getLastName()}"></td>
            <td th:text="${emp.getEmail()}"></td>
            <td th:text="${emp.getGender()==0?'女':'男'}"></td>
            <td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
            <td th:text="${emp.getDepartment().getDepartmentName()}"></td>
            <td>
               <a class="btn btn-sm btn-primary">update</a>
           	  <a class="btn btn-sm btn-danger">delete</a>
            </td>
        </tr>
    </tbody>
</table>
```

测试：

![image-20220401154227488](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401154227488.png)



## 11.6 添加员工

1. 在`list.html`页面适当位置处写一个添加员工的链接：

   ```html
   <!--这里的请求方式为get-->
   <a class="btn btn-sm btn-success" th:href="@{/emp}">addEmployee</a>
   ```

2. 编写对应的controller：

   ```java
       //添加员工
       @GetMapping("/emp")       //使用Restful风格，这里用get请求
       public String toAddPage(Model model) {
           //查询出所有的部门信息，方便添加的时候选择
           Collection<Department> departments = departmentDao.getDepartments();
           model.addAttribute("departments", departments);
           return "employee/add";
       }
       @PostMapping("/emp")    //这里也使用emp路径，但是请求方式不同，这就是Restful
       public String addEmp(Employee employee) {
           employeeDao.save(employee);
           //重定向到emps，查询所有员工信息
           return "redirect:/emps";
       }
   ```

3. 添加员工页面`add.html`中的关键部分代码：

   ```html
   <!--将commons文件下的commons.html中的topbar调用过来-->
   <div th:replace="~{commons/commons::topbar}"></div>
   
   <div class="container-fluid">
       <div class="row">
           <!--将公共部分的sidebar调用过来-->
           <div th:replace="~{commons/commons::sidebar(active='list.html')}"></div>
           <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
               
               <!--这里的请求方式为post-->
               <form th:action="@{/emp}" method="post">
                   <div class="form-group">
                       <label>LastName</label>
                       <input type="text" name="lastName" class="form-control" placeholder="lastName">
                   </div>
                   <div class="form-group">
                       <label>Email</label>
                       <input type="email" name="email" class="form-control" placeholder="email">
                   </div>
                   <div class="form-group">
                       <label>Gender</label><br>
                       <div class="form-check form-check-inline">
                           <input class="form-check-input" type="radio" name="gender" value="1">
                           <label class="form-check-label">男</label>
                       </div>
                       <div class="form-check form-check-inline">
                           <input class="form-check-input" type="radio" name="gender" value="0">
                           <label class="form-check-label">女</label>
                       </div>
                   </div>
                   <div class="form-group">
                       <label>Birth</label>
                       <input type="text" name="birth" class="form-control" placeholder="birth">
                   </div>
                   <div class="form-group">
                       <label>department</label>
                       <!--这里要传该部门的id给后端-->
                       <select class="form-control" name="department.id">
                           <option th:each="dept:${departments}" th:text="${dept.getDepartmentName()}" th:value="${dept.getId()}"></option>
                       </select>
                   </div>
                   <button type="submit" class="btn btn-primary">添加</button>
               </form>
               
           </main>
       </div>
   </div>
   ```

4. birth格式优化：Spring默认的Date格式为`yyyy/MM/dd`，如果想要自己自定义一个，可以去`application.properties`里配置，配置完后Spring默认的格式就失效了

   ```properties
   #时间格式化
   spring.mvc.format.date=yyyy-MM-dd
   ```

5. 测试：

   ![image-20220401164646710](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401164646710.png)

   ![image-20220401164714897](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401164714897.png)



## 11.7 修改员工

1. 在`list.html`页面的修改链接中添加请求：

   ```html
   <!--将这个员工的id传给后端-->
   <a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.getId()}">update</a>
   ```

2. 编写controller层：

   ```java
        //修改员工
       @GetMapping("/emp/{id}")    //虽然还是emp路径，但参数不同，Restful
       public String toUpdatePage(@PathVariable("id")Integer id, Model model) {
           //查询员工的信息传给修改页面
           Employee employee = employeeDao.getEmployeeById(id);
           model.addAttribute("emp", employee);
           Collection<Department> departments = departmentDao.getDepartments();
           model.addAttribute("departments", departments);
           return "employee/update";
       }
   
       @PostMapping("/updateEmp")
       public String updateEmp(Employee employee) {
           employeeDao.save(employee);
           return "redirect:/emps";
       }
   ```

3. `update.html`页面关键部分代码：

   ```html
   <form th:action="@{/updateEmp}" method="post">
       <!--隐藏域-->
       <input type="hidden" name="id" th:value="${emp.getId()}">
       <div class="form-group">
           <label>LastName</label>
           <input type="text" name="lastName" th:value="${emp.getLastName()}" class="form-control">
       </div>
       <div class="form-group">
           <label>Email</label>
           <input type="email" name="email" th:value="${emp.getEmail()}" class="form-control">
       </div>
       <div class="form-group">
           <label>Gender</label><br>
           <div class="form-check form-check-inline">
               <input class="form-check-input" type="radio" name="gender" th:checked="${emp.getGender()==1}" value="1">
               <label class="form-check-label">男</label>
           </div>
           <div class="form-check form-check-inline">
               <input class="form-check-input" type="radio" name="gender" th:checked="${emp.getGender()==0}" value="0">
               <label class="form-check-label">女</label>
           </div>
       </div>
       <div class="form-group">
           <label>Birth</label>
           <input type="text" name="birth" th:value="${#dates.format(emp.getBirth(), 'yyyy-MM-dd')}" class="form-control">
       </div>
       <div class="form-group">
           <label>department</label>
           <!--这里要传该部门的id给后端-->
           <select class="form-control" name="department.id">
               <option th:each="dept:${departments}" th:text="${dept.getDepartmentName()}"
               th:selected="${dept.getId()==emp.getDepartment().getId()}" th:value="${dept.getId()}">
               </option>
           </select>
       </div>
       <button type="submit" class="btn btn-primary">修改</button>
   </form>
   ```

4. 测试，修改功能完成！



## 11.8 删除功能

1. 在`list.html`页面的删除链接中添加请求：

   ```html
   <a class="btn btn-sm btn-danger" th:href="@{/deleteEmp/}+${emp.getId()}">delete</a>
   ```

2. 编写controller层：

   ```java
       //删除员工
       @GetMapping("/deleteEmp/{id}")
       public String deleteEmp(@PathVariable("id") Integer id) {
           employeeDao.delete(id);
           return "redirect:/emps";
       }
   ```

3. 测试，删除功能完成！



## 11.9 404和注销

### 11.9.1 404页面处理

1. 在templates目录下新建error目录，将`404.html`添加进去

2. Spring会帮我们自动扫描到404页面，直接去测试

   ![image-20220401181044013](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220401181044013.png)





### 11.9.2 注销功能

1. 在`commons.html`的头部导航栏中编写注销链接的请求：

   ```html
   <a class="nav-link" th:href="@{/user/logout}">注销</a>
   ```

2. 在`LoginController`类中编写注销功能：

   ```java
   @RequestMapping("/user/logout")
   public String logout(HttpSession session) {
       session.removeAttribute("loginUser");
       return "redirect:/index.html";
   }
   ```

3. 测试，登录成功后点击注销，跳转回首页，不能直接进主页面了。



# 12. 整合JDBC

## 12.1 创建测试项目测试数据源

1. 我去新建一个项目测试：springboot-data-jdbc ; 引入相应的模块！基础模块

   ![image-20220402124510910](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d506a30bf512d56633d3022bb56785de.png)

2. 项目建好之后，发现自动帮我们导入了如下的启动器：

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-jdbc</artifactId>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <scope>runtime</scope>
   </dependency>
   ```

3. 编写yaml配置文件连接数据库；

   ```yaml
   spring:
     datasource:
       username: root
       password: 123456
       #?serverTimezone=UTC解决时区的报错
       url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
   ```

4. 配置完这一些东西后，我们就可以直接去使用了，因为SpringBoot已经默认帮我们进行了自动配置；去测试类测试一下

   ```java
   @SpringBootTest
   class SpringbootDataJdbcApplicationTests {
   
       //DI注入数据源
       @Autowired
       DataSource dataSource;
   
       @Test
       public void contextLoads() throws SQLException {
           //看一下默认数据源
           System.out.println(dataSource.getClass());
           //获得连接
           Connection connection = dataSource.getConnection();
           System.out.println(connection);
           //关闭连接
           connection.close();
       }
   }
   ```

结果：我们可以看到他默认给我们配置的数据源为 : `class com.zaxxer.hikari.HikariDataSource`，我们并没有手动配置。

我们来全局搜索一下，找到数据源的所有自动配置都在 ：`DataSourceAutoConfiguration`文件：

```java
@Import(
    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
)
protected static class PooledDataSourceConfiguration {
    protected PooledDataSourceConfiguration() {
    }
}
```

这里导入的类都在 `DataSourceConfiguration` 配置类下，可以看出 SpringBoot 2.2.5  默认使用`HikariDataSource`数据源，而以前版本，如 SpringBoot 1.5 默认使用  `org.apache.tomcat.jdbc.pool.DataSource` 作为数据源；

**HikariDataSource号称JavaWeb当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀**；

**可以使用 `spring.datasource.type` 指定自定义的数据源类型，只为要使用的连接池实现的完全限定名**。

有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate

## 12.2 JDBCTemplate

1. 有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；
2. 即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。
3. 数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。
4. Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用
5. JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。

## 12.3 测试

编写一个Controller，注入 jdbcTemplate，编写测试方法进行访问测试；

```java
@RestController
public class JDBCController {

    @Autowired
    JdbcTemplate jdbcTemplate;

    // 查询数据库的所有信息
    // 没有实体类，获取数据库的东西，怎么获取？ Map
    @GetMapping("/userList")
    public List<Map<String,Object>> userList() {
        String sql = "select * from user";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }

    @GetMapping("/addUser")
    public String addUser() {
        String sql = "insert into mybatis.user(id, name, pwd) values(7,'小明','123456')";
        jdbcTemplate.update(sql);
        return "update-ok";
    }

    @GetMapping("/updateUser/{id}")
    public String updateUser(@PathVariable("id") int id) {
        String sql = "update mybatis.user set name  = ?,pwd = ? where id = " + id;
        //封装
        Object[] objects = new Object[2];

        objects[0] = "小明2";
        objects[1] = "aaaaaaa";

        jdbcTemplate.update(sql,objects);
        return "update-ok";
    }

    @GetMapping("/deleteUser/{id}")
    public String deleteUser(@PathVariable("id") int id) {
        String sql = "delete from mybatis.user where id = ?";
        jdbcTemplate.update(sql,id);
        return "update-ok";
    }
}
```

测试请求，结果正常；

到此，CURD的基本操作，使用 JDBC 就搞定了。





# 13. 整合Druid

## 13.1 Druid简介

- Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。
- Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了C3P0、DBCP 等DB池的优点，同时加入了日志监控。
- Druid 可以很好的监控DB池连接和SQL的执行情况，天生就是针对监控而生的 DB 连接池。
- Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。
- Spring Boot 2.0 以上默认使用Hikari数据源，可以说Hikari与Driud都是当前 Java Web上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成Druid数据源，如何实现数据库监控。
- Github地址：[https://github.com/alibaba/druid/](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Falibaba%2Fdruid%2F)

**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下**：

| **配置**                      | **缺省值**         | **说明**                                                     |
| ----------------------------- | ------------------ | ------------------------------------------------------------ |
| name                          |                    | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一个名字，格式是：“DataSource-” + System.identityHashCode(this) |
| jdbcUrl                       |                    | 连接数据库的url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username                      |                    | 连接数据库的用户名                                           |
| password                      |                    | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用ConfigFilter。详细看这里：[https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Falibaba%2Fdruid%2Fwiki%2F%E4%BD%BF%E7%94%A8ConfigFilter) |
| driverClassName               | 根据url自动识别    | 这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName(建议配置下) |
| initialSize                   | 0                  | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive                     | 8                  | 最大连接池数量                                               |
| maxIdle                       | 8                  | 已经不再使用，配置了也没效果                                 |
| minIdle                       |                    | 最小连接池数量                                               |
| maxWait                       |                    | 获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。 |
| poolPreparedStatements        | false              | 是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql下建议关闭。 |
| maxOpenPreparedStatements     | -1                 | 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100 |
| validationQuery               |                    | 用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会其作用。 |
| validationQueryTimeout        |                    | 单位:秒，检测连接是否有效的超时时间。底层调用jdbc Statement对象的void setQueryTimeout(int seconds)方法 |
| testOnBorrow                  | true               | 申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testOnReturn                  | false              | 归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testWhileIdle                 | false              | 建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效 |
| timeBetweenEvictionRunsMillis | 1分钟 ( 1.0.14 )   | 有两个含义： 1) Destroy线程会检测连接的间隔时间 2) testWhileIdle的判断依据，详细看testWhileIdle属性的说明 |
| numTestsPerEvictionRun        |                    | 不再使用，一个DruidDataSource只支持一个EvictionRun           |
| minEvictableIdleTimeMillis    | 30分钟 ( 1.0.14 )  | 连接保持空闲而不被驱逐的最长时间                             |
| connectionInitSqls            |                    | 物理连接初始化的时候执行的sql                                |
| exceptionSorter               | 根据dbType自动识别 | 当数据库抛出一些不可恢复的异常时，抛弃连接                   |
| filters                       |                    | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的filter:stat日志用的filter:log4j防御sql注入的filter:wall |
| proxyFilters                  |                    | 类型是List<com.alibaba.druid.filter.Filter>，如果同时配置了filters和proxyFilters，是组合关系，并非替换关系 |

## 13.2 配置数据源

1. 添加上 Druid 数据源依赖，这个依赖可以从Maven仓库官网[Maven Respository](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Fcom.alibaba%2Fdruid)中获取

   ```xml
   <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.0.9</version>
   </dependency>
   ```

   

2. 切换数据源；之前已经说过 Spring Boot 2.0 以上默认使用 `com.zaxxer.hikari.HikariDataSource `数据源，但可以通过 `spring.datasource.type` 指定数据源。

   ```yaml
   spring:
     datasource:
       username: root
       password: 123456
       url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
   ```

3. 数据源切换之后，在测试类中注入DataSource，然后获取到它，输出一看便知是否成功切换；

   ![image-20220402130148743](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6d69750c2d72881e194affcb96f47e9e.png)

4. 切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

   ```yaml
   spring:
     datasource:
       username: root
       password: 123456
       #?serverTimezone=UTC解决时区的报错
       url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource
   
       #Spring Boot 默认是不注入这些属性值的，需要自己绑定
       #druid 数据源专有配置
       initialSize: 5
       minIdle: 5
       maxActive: 20
       maxWait: 60000
       timeBetweenEvictionRunsMillis: 60000
       minEvictableIdleTimeMillis: 300000
       validationQuery: SELECT 1 FROM DUAL
       testWhileIdle: true
       testOnBorrow: false
       testOnReturn: false
       poolPreparedStatements: true
   
       #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
       filters: stat,wall,log4j
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

5. 导入Log4j 的依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

6. 现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 SpringBoot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

   ```java
   @Controller
   public class DruidConfig {
       
       /*
          @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 yaml全局配置文件中
          前缀为spring.datasource的属性值注入到com.alibaba.druid.pool.DruidDataSource的同名参数中
          @Bean：注入到Spring容器中
        */
       @ConfigurationProperties(prefix = "spring.datasource")
       @Bean
       public DataSource druidDataSource() {
           return new DruidDataSource();
       }
       
   }
   ```

7. 去测试类中测试一下；看是否成功！

   ```java
   @SpringBootTest
   class SpringbootDataJdbcApplicationTests {
   
       //DI注入数据源
       @Autowired
       DataSource dataSource;
   
       @Test
       public void contextLoads() throws SQLException {
           //看一下默认数据源
           System.out.println(dataSource.getClass());
           //获得连接
           Connection connection =   dataSource.getConnection();
           System.out.println(connection);
   
           DruidDataSource druidDataSource = (DruidDataSource) dataSource;
           System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
           System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());
   
           //关闭连接
           connection.close();
       }
   }
   ```

8. 输出结果 ：可见配置参数已经生效！

   ![image-20220402131634755](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/649bc5db3d6dddaedf3127a2247a611c.png)

## 13.3 配置Druid数据源监控

Druid 数据源具有监控的功能，并提供了一个web界面方便用户查看，类似安装路由器 时，人家也提供了一个默认的web页面。

所以第一步需要设置 Druid 的后台管理页面，比如登录账号、密码等；去`DruidConfig`类中配置后台管理：

```java
    //配置Druid监控管理后台的Servlet；
    //内置Servlet容器时没有web.xml文件，所以使用SpringBoot的注册Servlet方式
    @Bean
    public ServletRegistrationBean statViewServlet() {
        //如果报错需要导入servlet-api的依赖
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

        // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet
        // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
        Map<String, String> initParams = new HashMap<>();
        initParams.put("loginUsername", "root"); //后台管理界面的登录账号
        initParams.put("loginPassword", "123456"); //后台管理界面的登录密码

        //后台允许谁可以访问
        //initParams.put("allow", "localhost")：表示只有本机可以访问
        //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
        initParams.put("allow", "");
        //deny：Druid 后台拒绝谁访问
        //initParams.put("kuangshen", "192.168.1.20");  表示禁止此ip访问

        //设置初始化参数
        bean.setInitParameters(initParams);
        return bean;
    }
```

配置完毕后，我们可以选择访问 ：[http://localhost:8080/druid/login.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdruid%2Flogin.html)

![image-20200727233409312](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/969d6375a26d92f41a216311f65e1c84.png)

进入之后

![image-20200727233436583](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/167e541339bfda23f6f021d9645ac1aa.png)

**`DruidConfig`类中配置Druid web监控filter过滤器**

```java
//配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.setFilter(new WebStatFilter());

    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);

    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```

# 14. 整合MyBatis

官方文档：[http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/](https://gitee.com/link?target=http%3A%2F%2Fmybatis.org%2Fspring-boot-starter%2Fmybatis-spring-boot-autoconfigure%2F)

Maven仓库地址：[https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.3](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Forg.mybatis.spring.boot%2Fmybatis-spring-boot-starter%2F2.1.3)

## 14.1 整合步骤

1. 导入 MyBatis 所需要的依赖

   ```xml
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.1.3</version>
   </dependency>
   ```

2. 导入Druid依赖，配置数据库连接信息（不变）

   ```yaml
   spring:
     datasource:
       username: root
       password: 123456
       #?serverTimezone=UTC解决时区的报错
       url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource
   
       #Spring Boot 默认是不注入这些属性值的，需要自己绑定
       #druid 数据源专有配置
       initialSize: 5
       minIdle: 5
       maxActive: 20
       maxWait: 60000
       timeBetweenEvictionRunsMillis: 60000
       minEvictableIdleTimeMillis: 300000
       validationQuery: SELECT 1 FROM DUAL
       testWhileIdle: true
       testOnBorrow: false
       testOnReturn: false
       poolPreparedStatements: true
   
       #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
       #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
       #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
       filters: stat,wall,log4j
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

3. **测试数据库是否连接成功！**

4. **导入Lombok，创建实体类**：`User.java`

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

5. **创建mapper包以及对应的Mapper接口**：`UserMapper.java`

   ```java
   // 这个注解表示了这是一个 mybatis 的 mapper 类
   @Mapper
   @Repository
   public interface UserMapper {
   
       List<User> queryUserList();
   
       User queryUserById(int id);
   
       int addUser(User user);
   
       int updateUser(User user);
   
       int deleteUser(int id);
   }
   ```

6. 在`application.yaml`中配置mybatis，起别名和`mapper.xml`的路径等：

   ```yaml
   #配置mybatis
   mybatis:
     # 配置XML映射文件中指定的实体类别名路径
     type-aliases-package: com.run.pojo
     # 配置MyBatis的xml配置文件路径
     mapper-locations: classpath:mybatis/mapper/*.xml
     # 开启驼峰uName自动映射到u_name
     map-underscore-to-camel-case: true
   ```

7. **在`resources`目录下新建mybatis目录，创建mapper目录，编写对应的Mapper映射文件**：`UserMapper.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--namespace=绑定一个对应的Dao/Mapper接口-->
   <mapper namespace="nuc.ss.mapper.UserMapper">
       
       <select id="queryUserList" resultType="User">
           select * from mybatis.user;
       </select>
   
       <select id="queryUserById" resultType="User">
           select * from mybatis.user where id = #{id};
       </select>
   
       <insert id="addUser" parameterType="User">
           insert into mybatis.user (id, name, pwd) values (#{id},#{name},#{pwd});
       </insert>
   
       <update id="updateUser" parameterType="User">
           update mybatis.user set name=#{name},pwd = #{pwd} where id = #{id};
       </update>
   
       <delete id="deleteUser" parameterType="int">
           delete from mybatis.user where id = #{id}
       </delete>
   </mapper>
   ```

8. **maven配置资源过滤问题**

   ```xml
   <resources>
       <resource>
           <directory>src/main/java</directory>
           <includes>
               <include>**/*.xml</include>
           </includes>
           <filtering>true</filtering>
       </resource>
   </resources>
   ```

9. **编写 UserController 进行测试！**

   ```java
   @RestController
   public class UserController {
       @Autowired
       private UserMapper userMapper;
   
       @GetMapping("/queryUserList")
       public List<User> queryUserList() {
           List<User> userList = userMapper.queryUserList();
   
           for (User user : userList) {
               System.out.println(user);
           }
   
           return userList;
       }
       
        //添加一个用户
       @GetMapping("/addUser")
       public String addUser() {
           userMapper.addUser(new User(7,"阿毛","123456"));
           return "ok";
       }
   
       //修改一个用户
       @GetMapping("/updateUser")
       public String updateUser() {
           userMapper.updateUser(new User(7,"阿毛","123456"));
           return "ok";
       }
   
       @GetMapping("/deleteUser")
       public String deleteUser() {
           userMapper.deleteUser(7);
   
           return "ok";
       }
   }
   ```

10. 启动项目测试。



# 15. SpringSecurity

## 15.1 安全简介

1、在 Web  开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。

2、市面上存在比较有名的：Shiro，Spring Security ！

3、这里需要阐述一下的是，每一个框架的出现都是为了解决某一问题而产生了，那么Spring Security框架的出现是为了解决什么问题呢？

4、首先我们看下它的官网介绍：Spring Security官网地址

```text
Spring Security is a powerful and highly customizable  authentication and access-control framework. It is the de-facto standard for securing Spring-based applications.
Spring Security is a framework that focuses on providing both  authentication and authorization to Java applications. Like all Spring  projects, the real power of Spring Security is found in how easily it  can be extended to meet custom requirements
```

5、Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它实际上是保护基于spring的应用程序的标准。

6、Spring Security是一个框架，侧重于为Java应用程序提供身份验证和授权。与所有Spring项目一样，Spring安全性的真正强大之处在于它可以轻松地扩展以满足定制需求

7、从官网的介绍中可以知道这是一个权限框架。想我们之前做项目是没有使用框架是怎么控制权限的？对于权限 一般会细分为功能权限，访问权限，和菜单权限。代码会写的非常的繁琐，冗余。

8、怎么解决之前写权限代码繁琐，冗余的问题，一些主流框架就应运而生而Spring Scecurity就是其中的一种。

9、Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套  Web 应用安全性的完整解决方案。一般来说，Web  应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。

- 用户认证指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。
- 用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

10、对于上面提到的两种应用情景，Spring Security 框架都有很好的支持。

- 在用户认证方面，Spring Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID 和 LDAP 等。
- 在用户授权方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

## 15.2 实战测试

### 15.2.1 实验环境搭建

1. 新建一个初始的springboot项目web模块，thymeleaf模块，在pom文件中添加thymeleaf依赖

2. 导入静态资源

   ![image-20220402151410667](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c8ae3a5f3a16250a07e2ae1a02dd71b4.png)

3. controller层跳转页面：

   ```java
   @Controller
   public class RouterController {
   
       @RequestMapping({"/","/index"})
       public String index() {
           return "index";
       }
   
       @RequestMapping("/toLogin")
       public String toLogin() {
           return "views/login";
       }
   
       @RequestMapping("/level1/{id}")
       public String level1(@PathVariable("id") int id) {
           return "views/level1/" + id;
       }
   
       @RequestMapping("/level2/{id}")
       public String level2(@PathVariable("id") int id) {
           return "views/level2/" + id;
       }
   
       @RequestMapping("/level3/{id}")
       public String level3(@PathVariable("id") int id) {
           return "views/level3/" + id;
       }
   }
   ```

4. 测试实验环境是否OK！

   ![image-20220402154524753](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/be3fdd901d0c603955fe2618e7dc2e3d.png)


### 15.2.2 认识SpringSecurity

Spring Security 是针对Spring项目的安全框架，也是Spring  Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入  spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- `WebSecurityConfigurerAdapter`：自定义Security策略
- `AuthenticationManagerBuilder`：自定义认证策略
- `@EnableWebSecurity`：开启WebSecurity模式

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。

**“认证”（Authentication）**

- 身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

- 身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

**“授权” （Authorization）**

- 授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

### 15.2.3 认证和授权

目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能

1. 引入 Spring Security 模块

   ```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

2. 编写 Spring Security 配置类

   - 参考官网：[https://spring.io/projects/spring-security](https://gitee.com/link?target=https%3A%2F%2Fspring.io%2Fprojects%2Fspring-security)

   - 查看我们自己项目中的版本，找到对应的帮助文档：[https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-security%2Fsite%2Fdocs%2F5.3.0.RELEASE%2Freference%2Fhtml5)

   - servlet-applications 8.16.4

     ```java
     @EnableWebSecurity
     public class Config extends WebSecurityConfigurerAdapter {
         @Override
         protected void configure(HttpSecurity http) throws Exception {
             http
                 .apply(customDsl())
                     .flag(true)
                     .and()
                 ...;
         }
     }
     ```

3. 新建SecurityConfig类，编写基础配置；需要继承`WebSecurityConfigurerAdapter`，重写configure方法，注意选择参数是`http:HttpSecurity`的：

   ```java
   @EnableWebSecurity// 开启WebSecurity模式
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
   
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           super.configure(http);
       }
   }
   ```

4. 定制请求的授权规则。先点击`super.configure(http)`看源码：

   ![image-20220402155719261](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a276d043a6b764d44229050f10d3ff45.png)

   自己仿写，首页所有人都可以访问，功能页只有对应有权限的人才能访问到：

   ```java
   @EnableWebSecurity
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           http.authorizeRequests()
                   .antMatchers("/").permitAll()
                   .antMatchers("/level1/**").hasRole("vip1")
                   .antMatchers("/level2/**").hasRole("vip2")
                   .antMatchers("/level3/**").hasRole("vip3");
       }
   }
   ```

5. 测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！

   ![image-20220402160154398](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4138ffea9416154ffb12813e13ce30ae.png)

6. 在`configure()`方法中加入以下配置，开启自动配置的登录功能！

   ```java
   // 开启自动配置的登录功能
   //   /login 请求来到登录页
   //   /login?error 重定向到这里表示登录失败
   http.formLogin();
   ```

7. 随便点击一个需要权限的level，发现没有权限的时候，会跳转到登录的页面：

   ![image-20220402160536691](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9de0304274beb73232b2be213d393868.png)

8. 我们可以自定义认证规则，重写configure的另一个方法，参数为`AuthenticationManagerBuilder`：

   先查看源码：

   ![image-20220402161204046](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c0fa36fc90420288347fce54ac5971af.png)

   自己仿写：

   ```java
       @Override
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
           //这些用户正常应该从数据库中读取，这里用内存模拟
           auth.inMemoryAuthentication()
                   .withUser("AruNi").password("123456").roles("vip2", "vip3")
                   .and()
                   .withUser("root").password("123456").roles("vip1", "vip2", "vip3")
                   .and()
                   .withUser("guest").password("123456").roles("vip1");
       }
   ```

9. 测试，我们可以使用这些账号登录进行测试，发现会报错！

   ![image-20220402161700260](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/27ae252e7544c25f08a137c37aba4661.png)

   ![image-20220402161636119](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dfb90d8deaf4911f56004469927ffe96.png)

   

   

10. 原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码：

    ```java
    //密码编码： PasswordEncoder
        //在spring Secutiry 5.0+ 新增了很多加密方法
        @Override
        protected void configure(AuthenticationManagerBuilder auth) throws Exception {
            //这些用户正常应该从数据库中读取，这里用内存模拟
            auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                    .withUser("AruNi").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2", "vip3")
                    .and()
                    .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                    .and()
                    .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
        }
    ```

11. 测试，发现，登录成功，并且每个角色只能访问自己认证下的规则！

### 15.2.4 权限控制和注销

1. 开启自动配置的注销的功能，在登录权限下面写即可

   ```java
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
      //....
      http.formLogin();
   
       //开启自动配置的注销的功能
       //  /logout 注销请求
       http.logout();
   }
   ```

2. 我们在前端相应位置增加一个注销的按钮，`index.html `导航栏中

   ```html
   <!--注销-->
   <a class="item" th:href="@{/logout}">
       <i class="sign-out icon"></i> 注销
   </a>
   ```

3. 我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！

   ![image-20220402162956586](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3e2f71ffee114a42b1f868048f3b6519.png)

4. 但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

   看源码：

   ![image-20220402163401739](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/412babe2019ad8d58081345ab978dda3.png)

   在`http.logout()`后再添加方法：

   ```java
   // .logoutSuccessUrl("/"); 注销成功来到首页
   http.logout().logoutSuccessUrl("/");
   ```

5. 测试，注销完毕后，发现跳转到首页

6. 我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！用户只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？

   我们需要结合thymeleaf中的一些功能

   `sec：authorize="isAuthenticated()"`:是否认证登录！来显示不同的页面

   Maven导入整合包的依赖：

   ```xml
   	<dependency>
               <groupId>org.thymeleaf.extras</groupId>
               <artifactId>thymeleaf-extras-springsecurity5</artifactId>
           </dependency>
   ```

7. 修改我们的前端页面

   导入命名空间

   ```html
   <html lang="en" xmlns:th="http://www.thymeleaf.org"
         xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">
   ```

   修改导航栏，增加认证判断

   ```html
   <!--登录注销-->
   <div class="right menu">
   
       <!--如果未登录-->
       <div sec:authorize="!isAuthenticated()">
           <a class="item" th:href="@{/login}">
               <i class="address card icon"></i> 登录
           </a>
       </div>
   
       <!--如果已登录-->
       <div sec:authorize="isAuthenticated()">
           <a class="item">
               <i class="address card icon"></i>
               用户名：<span sec:authentication="principal.username"></span>
               角色：<span sec:authentication="principal.authorities"></span>
           </a>
       </div>
   
       <div sec:authorize="isAuthenticated()">
           <a class="item" th:href="@{/logout}">
               <i class="sign-out  icon"></i> 注销
           </a>
       </div>
   </div>
   ```

8. 重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面；

   - 未登录

     ![image-20220402165034257](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8d8c03bfad5bc3d38540e2a5ff3aed2c.png)

   - 登录

     ![image-20220402165018635](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/55de4e96710af5d239710087873779a2.png)

9. 我们继续将下面的角色功能块认证完成，在每个板块的位置添加`sec:authorize="hasRole(' ')"`

   ```html
   <!--菜单根据用户的角色动态的实现-->
   <div class="column"  sec:authorize="hasRole('vip1')">
       <div class="ui raised segment">
           <div class="ui">
               <div class="content">
                   <h5 class="content">Level 1</h5>
                   <hr>
                   <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
                   <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
                   <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
               </div>
           </div>
       </div>
   </div>
   
   <div class="column"  sec:authorize="hasRole('vip2')">
       <div class="ui raised segment">
           <div class="ui">
               <div class="content">
                   <h5 class="content">Level 2</h5>
                   <hr>
                   <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
                   <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
                   <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
               </div>
           </div>
       </div>
   </div>
   
   <div class="column"  sec:authorize="hasRole('vip3')">
       <div class="ui raised segment">
           <div class="ui">
               <div class="content">
                   <h5 class="content">Level 3</h5>
                   <hr>
                   <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
                   <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
                   <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
               </div>
           </div>
       </div>
   </div>
   ```

10. 测试一下！

    - 用户首页未登录，发现什么页看不见：

      ![image-20220402165550336](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/34a89ef6ad10d9d5caf23b2413687985.png)

- 某个用户登录：

  ![image-20220402165644511](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220402165644511.png)


​      


权限控制和注销搞定

### 15.2.5 记住我

1. 开启记住我功能：

   ```java
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
   //。。。。。。。。。。。
      //开启记住我功能: cookie,默认保存两周
      http.rememberMe();
   }
   ```

2. 我们再次启动项目测试一下

   - 发现登录页多了一个记住我功能

     ![image-20220402165834723](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/84f69d91720a8aceeb7ff3c0e55337c7.png)

   - 我们登录之后关闭浏览器，然后重新打开浏览器访问，发现用户依旧存在！

     ![image-20220402165913705](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/af08ed4aebcbb38b77f31f7ea79252ee.png)思考：如何实现的呢？其实非常简单

     我们可以查看浏览器的Cookies：

     ![image-20220402170213104](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ae27652a50b9608a485a12e06979d79e.png)

3. 我们点击注销的时候，可以发现，Spring Security 帮我们自动删除了这个cookie(置为空)：

   ![image-20220402170511522](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4bce8e7ea2d45fcbe6461517ceec1ea7.png)

4. cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie

### 15.2.6 定制登录页

现在这个登录页面都是Spring Security 默认的，怎么样可以使用我们自己写的Login界面呢？

1. 在刚才的登录页配置后面指定 loginpage 即可：

   ```java
   protected void configure(HttpSecurity http) throws Exception {
       //......
   
       // 没有权限默认会到登录页面,需要开启登录的页面
       // /login页面
       http.formLogin().loginPage("/toLogin");
   
       //......
   }
   ```

2. 然后前端也需要指向我们自己定义的 login请求

   ```html
   <div sec:authorize="!isAuthenticated()">
       <a class="item" th:href="@{/toLogin}">
           <i class="address card icon"></i> 登录
       </a>
   </div>
   ```

3. - `login.html` 表单的提交请求及方式，**方式必须为post**
   - 登录需要将这些信息发送到哪里
   - 接收登录的用户名和密码的参数！

   在 `loginPage()`源码中的注释上有写明：

   ![image-20220402171827509](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/00ba31f03b769379ec261385ac0a65cd.png)

   进一步修改：

   ```java
   protected void configure(HttpSecurity http) throws Exception {
       //......
   
       //开启自动配置的登录功能
       //  /login 请求来到登录页
       //  /login?error 重定向到这里表示登录失败
       http.formLogin()
       .usernameParameter("username")
       .passwordParameter("password")
       .loginPage("/toLogin")		//去登陆页面请求
       .loginProcessingUrl("/login");      //登录页面，表单提交时请求的地址
   
       //......
   }
   ```

4. 在登录页增加记住我的多选框

   ```html
   <div class="field">
   	<input type="checkbox" name="remember"> 记住我
   </div>
   ```

5. 后端验证处理！

   ```java
   protected void configure(HttpSecurity http) throws Exception {
       //......
       //开启记住我功能: cookie,默认保存两周，自定义接收前端的参数
       http.rememberMe().rememberMeParameter("remember");
   }
   ```

6. 测试，可能会发现注销功能失败了，这是因为我们使用了自己的登陆页面后，存在csrf攻击（跨站伪造攻击），只需要在configure方法中关闭csrf来防止即可：

   ```java
   //关闭csrf，防止网址攻击：get，post
   http.csrf().disable();
   ```





> 完整配置代码

```java
//通过AOP实现拦截器
@EnableWebSecurity      //开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //关闭csrf，防止网址攻击：get，post
        http.csrf().disable();

        //请求权限规则
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        //开启自动配置的登录功能
        //  /login 请求来到登录页
        //  /login?error 重定向到这里表示登录失败
        http.formLogin()
                .usernameParameter("username")
                .passwordParameter("password")
                .loginPage("/toLogin")  //去登陆页面请求
                .loginProcessingUrl("/login");      //登录页面，表单提交时请求的地址


        //开启自动配置的注销的功能
        //  /logout 注销请求
        http.logout().logoutSuccessUrl("/");

        //开启记住我功能: cookie,默认保存两周，自定义接收前端的参数
        http.rememberMe().rememberMeParameter("remember");

    }

    //认证
    //密码编码： PasswordEncoder
    //在spring Secutiry 5.0+ 新增了很多加密方法
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //这些用户正常应该从数据库中读取，这里用内存模拟
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("AruNi").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2", "vip3")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
    }
}
```

# 16. Shiro

## 16.1 什么是Shiro?

● `Apache Shiro`是一个Java 的安全(权限)框架。
● `Shiro`可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE环境，也可以用在JavaEE环境。
● `Shiro`可以完成，认证，授权，加密，会话管理，Web集成，缓存等.
● 下载地址:`http://shiro.apache.org/`

### 16.1.1 有哪些功能

![image-20220403132629590](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/42318faf7b43111fc2f62f55ee8f7d3b.png)
● `Authentication`: 身份认证、登录，验证用户是不是拥有相应的身份;
● `Authorization`: 授权,即权限验证，验证某个已认证的用户是否拥有某个权限，即判断用户能否进行什么操作，如:验证某个用户是否拥有某个角色，或者细粒度的验证某个用户对某个资源是否具有某个权限!
● `Session Manager`: 会话管理，即用户登录后就是第一次会话，在没有退出之前，它的所有信息都在会话中;会话可以是普通的JavaSE环境，也可以是Web环境;
● `Cryptography`: 加密,保护数据的安全性，如密码加密存储到数据库中，而不是明文存储;
● `Web Support`: Web支持，可以非常容易的集成到Web环境;
● `Caching`: 缓存，比如用户登录后，其用户信息，拥有的角色、权限不必每次去查,这样可以提高效率
● `Concurrency`: Shiro支持多线程应用的并发验证，即，如在-个线程中开启另-一个线程,能把权限自动的传播过去
● `Testing`: 提供测试支持;
● `RunAs`: 允许一个用户假装为另-一个用户(如果他们允许)的身份进行访问;
● `Remember Me`: 记住我，这个是非常常见的功能，即一次登录后， 下次再来的话不用登录了

### 16.1.2 Shiro架构(外部)

![image-20220403132736544](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6d3aa7d8079c3e3bb557ef3d93ca6d5c.png)
● `subject`: 应用代码直接交互的对象是Subject, 也就是说Shiro的对外API核心就是Subject, Subject代表了当前的用户，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject,如网络爬虫，机器人等，与Subject的所有交互都会委托给SecurityManager; Subject其实是一个门面， SecurityManageer 才是实际的执行者
● `SecurityManager`: 安全管理器，即所有与安全有关的操作都会与SercurityManager交互, 并且它管理着所有的Subject,可以看出它是Shiro的核心，它负责与Shiro的其他组件进行交互，它相当于SpringMVC的DispatcherServlet的角色
● `Realm`: Shiro从Realm获取安全数据 (如用户,角色，权限)，就是说SecurityManager要验证用户身份，那么它需要从Realm获取相应的用户进行比较，来确定用户的身份是否合法;也需要从Realm得到用户相应的角色、权限，进行验证用户的操作是否能够进行，可以把Realm看DataSource;

### 16.1.3 Shiro架构(内部)

![image-20220403132756268](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6033c29cc20158e9c4aa901343cf4fa5.png)
● `Subject`: 任何可以与应用交互的用户;
● `Security Manager`:相当于SpringMVC中的DispatcherSerlet; 是Shiro的心脏， 所有具体的交互都通过Security Manager进行控制，它管理者所有的Subject, 且负责进行认证,授权，会话，及缓存的管理。
● `Authenticator`:负责Subject认证， 是-一个扩展点，可以自定义实现;可以使用认证策略(Authentication Strategy)，即什么情况下算用户认证通过了;
● `Authorizer`:授权器，即访问控制器，用来决定主体是否有权限进行相应的操作;即控制着用户能访问应用中
的那些功能;
● `Realm`: 可以有一个或者多个的realm, 可以认为是安全实体数据源，即用于获取安全实体的，可以用JDBC实现，也可以是内存实现等等，由用户提供;所以- -般在应用中都需要实现自己的realm
● `SessionManager`:管理Session生 命周期的组件,而Shiro并不仅仅可以用在Web环境，也可以用在普通的JavaSE环境中
● `CacheManager`: 缓存控制器，来管理如用户，角色，权限等缓存的;因为这些数据基本上很少改变,放到缓存中后可以提高访问的性能;
● `Cryptography`:密码模块，Shiro 提高了一些常见的加密组件用于密码加密， 解密等

## 16.2 Shiro快速开始

创建一个普通`maven`项目`springboot-shiro`, 然后删除src目录, 这样的话就可以在这个项目里新建很多model

1. 在`springboot-shiro`里新建model `shiro-01-helloshiro`，父依赖`pom.xml`

   ```xml
       <dependencies>
           <dependency>
               <groupId>org.apache.shiro</groupId>
               <artifactId>shiro-core</artifactId>
               <version>1.9.0</version>
           </dependency>
           <!-- Shiro uses SLF4J for logging.  We'll use the 'simple' binding
                in this example app.  See http://www.slf4j.org for more info. -->
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
               <version>1.7.21</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>jcl-over-slf4j</artifactId>
               <version>1.7.21</version>
           </dependency>
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
               <scope>runtime</scope>
           </dependency>
       </dependencies>
   ```

2. 在`resources`目录下新建`log4j.properties`和`shiro.ini`，在IDEA安装ini插件：

   log4j：

   ```properties
   log4j.rootLogger=INFO, stdout
   
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n
   
   # General Apache libraries
   log4j.logger.org.apache=WARN
   
   # Spring
   log4j.logger.org.springframework=WARN
   
   # Default Shiro logging
   log4j.logger.org.apache.shiro=INFO
   
   # Disable verbose logging
   log4j.logger.org.apache.shiro.util.ThreadContext=WARN
   log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
   ```

   shiro：

   ```ini
   # -----------------------------------------------------------------------------
   # Users and their (optional) assigned roles
   # username = password, role1, role2, ..., roleN
   # -----------------------------------------------------------------------------
   [users]
   root = secret, admin
   guest = guest, guest
   presidentskroob = 12345, president
   darkhelmet = ludicrousspeed, darklord, schwartz
   lonestarr = vespa, goodguy, schwartz
   
   # -----------------------------------------------------------------------------
   # Roles with assigned permissions
   # roleName = perm1, perm2, ..., permN
   # -----------------------------------------------------------------------------
   [roles]
   admin = *
   schwartz = lightsaber:*
   goodguy = winnebago:drive:eagle5
   ```

3. 在java包下新建`QuickStar`类：

   ```java
   public class QuickStar {
       private static final transient Logger log = LoggerFactory.getLogger(QuickStar.class);
   
       public static void main(String[] args) {
           log.info("My First Apache Shiro Application");
   
           Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
           SecurityManager securityManager = factory.getInstance();
           SecurityUtils.setSecurityManager((SecurityManager) securityManager);
   
           // get the currently executing user:
           //获取当前用户对象 Subject
           Subject currentUser = SecurityUtils.getSubject();
   
           // Do some stuff with a Session (no need for a web or EJB container!!!)
           //通过当前用户拿到session
           Session session = currentUser.getSession();
           session.setAttribute("someKey", "aValue");
           String value = (String) session.getAttribute("someKey");
           if (value.equals("aValue")) {
               log.info("Retrieved the correct value! [" + value + "]");
           }
   
           // let's login the current user so we can check against roles and permissions:
           //判断当前用户是否被认证
           if (!currentUser.isAuthenticated()) {
               //token：令牌
               UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
               token.setRememberMe(true);
               try {
                   currentUser.login(token);		//执行登录操作
               } catch (UnknownAccountException uae) {
                   log.info("There is no user with username of " + token.getPrincipal());
               } catch (IncorrectCredentialsException ice) {
                   log.info("Password for account " + token.getPrincipal() + " was incorrect!");
               } catch (LockedAccountException lae) {
                   log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                           "Please contact your administrator to unlock it.");
               }
               // ... catch more exceptions here (maybe custom ones specific to your application?
               catch (AuthenticationException ae) {
                   //unexpected condition?  error?
               }
           }
   
           //say who they are:
           //print their identifying principal (in this case, a username):
           log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
   
           //test a role:
           //当前用户的角色
           if (currentUser.hasRole("schwartz")) {
               log.info("May the Schwartz be with you!");
           } else {
               log.info("Hello, mere mortal.");
           }
   
           //test a typed permission (not instance-level)
           //当前用户的权限，粗粒度
           if (currentUser.isPermitted("lightsaber:wield")) {
               log.info("You may use a lightsaber ring.  Use it wisely.");
           } else {
               log.info("Sorry, lightsaber rings are for schwartz masters only.");
           }
   
           //a (very powerful) Instance Level permission:
           //当前用户的权限，细粒度
           if (currentUser.isPermitted("winnebago:drive:eagle5")) {
               log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                       "Here are the keys - have fun!");
           } else {
               log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
           }
   
           //all done - log out!
           //注销
           currentUser.logout();
   
           System.exit(0);
       }
   }
   ```

4. 运行：

   ![image-20220403135721889](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4e5ef51887292600daf92c4e1f0f48d0.png)

**总结主要方法**：

- 获取当前用户对象：`Subject currentUser = SecurityUtils.getSubject();`
- 通过当前用户拿到session：`Session session = currentUser.getSession();`
- 判断当前用户是否被认证：`currentUser.isAuthenticated()`
- 获取当前用户token中的principal(uuid/主键/唯一信息)：`token.getPrincipal()`
- 当前用户拥有的角色：`currentUser.hasRole("XXXX")`
- 当前用户拥有的权限：`currentUser.isPermitted("XXX:XX")`
- 注销：`currentUser.logout();`





## 16.3 SpringBoot整合Shiro

### 16.3.1 环境搭建

1. 新建一个springboot项目，勾选web，thymeleaf模块，在`pom.xml`中添加`shiro-spring`的依赖：

   ```xml
   <dependency>
       <groupId>org.apache.shiro</groupId>
       <artifactId>shiro-spring</artifactId>
       <version>1.4.0</version>
   </dependency>
   ```

2. 新建config包，创建`ShiroConfig`类和`UserRealm`类，因为创建realm对象时需要自定义类：

   `UserRealm.java`继承`AuthorizingRealm`：

   ```java
   public class UserRealm extends AuthorizingRealm {
       
       //授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
           System.out.println("执行了=》授权doGetAuthorizationInfo");
           return null;
       }
   
       //认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
           System.out.println("执行了=》认证doGetAuthenticationInfo");
           return null;
       }
       
   }
   ```

   `ShiroConfig.java`里面的三个方法关联性很强，倒叙写会顺畅一些：

   ```java
   @Configuration
   public class ShiroConfig {
   
       //3. ShiroFilterFactoryBean
       @Bean
       public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager) {
           ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
           //设置安全管理器，关联DefaultWebSecurityManager
           bean.setSecurityManager(securityManager);
           return bean;
       }
   
       //2. DefaultWebSecurityManager
       @Bean(name = "securityManager")
       public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm) {
           DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
           //关联UserRealm
           securityManager.setRealm(userRealm);
           return securityManager;
       }
   
       //1. 创建 realm对象， 需要自定义类
       @Bean(name = "userRealm")
       public UserRealm userRealm() {
           return new UserRealm();
       }
       
   }
   ```

3. 新建页面和controller，进行环境测试：

   templates目录下进行`index.html`首页：

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>首页</h1>
   <p th:text="${msg}"></p>
   <hr>
   
   <a th:href="@{/user/add}">add</a> |
   <a th:href="@{/user/update}">update</a>
   </body>
   </html>
   ```

   templates目录下新建user文件夹，创建`add.html`和`update.html`：

   ```html
   add.html
   <body>
   <h1>add</h1>
   </body>
   
   update.html
   <body>
   <h1>update</h1>
   </body>
   ```

   创建controller：

   ```java
   @Controller
   public class MyController {
   
       @RequestMapping({"/index", "/"})
       public String toIndex(Model model) {
           model.addAttribute("msg", "Hello Shiro");
           return "index";
       }
   
       @RequestMapping("/user/add")
       public String add() {
           return "user/add";
       }
   
       @RequestMapping("/user/update")
       public String update() {
           return "user/update";
       }
   }
   ```

4. 环境搭建完毕，接下来开始权限设定。



### 16.3.2 登录拦截

1. 在`ShiroConfig.java`的`ShiroFilterFactoryBean`方法中添加shiro的内置过滤器：

   ```java
       //3. ShiroFilterFactoryBean
       @Bean
       public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager) {
           ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
           //设置安全管理器，关联DefaultWebSecurityManager
           bean.setSecurityManager(securityManager);
   
           //添加Shiro内置过滤器
           /*
               anon：无需认证就可以访问
               authc：必须认证了才能访问
               user：必须拥有 记住我 功能才能访问
               perms：拥有对某个资源的权限才能访问
               role：拥有某个角色权限才能访问
            */
           //拦截
           Map<String, String> filterMap = new LinkedHashMap<>();
           filterMap.put("/user/*", "authc");
           bean.setFilterChainDefinitionMap(filterMap);
   
           //设置登录请求
           bean.setLoginUrl("/toLogin");
   
           return bean;
       }
   ```

2. 编写登录页面`login.heml`

3. 在controller里添加去登陆的请求处理：

   ```java
       @RequestMapping("/toLogin")
       public String toLogin() {
           return "login";
       }
   ```

4. 进行测试，发现点击add或者update会跳转到登录页面



### 16.3.3 实现用户认证

1. 在controller里面编写登录请求处理：

   ```java
       @RequestMapping("/login")
       public String login(String username, String password, Model model) {
           //获取当前用户
           Subject subject = SecurityUtils.getSubject();
           //封装用户的登录数据
           UsernamePasswordToken token = new UsernamePasswordToken(username, password);
   
           //执行登录方法，如果没有异常就成功
           try {
               subject.login(token);
               return "index";
           } catch (UnknownAccountException e) {   //用户名不存在
               model.addAttribute("error", "用户名错误");
               return "login";
           } catch (IncorrectCredentialsException e) {     //密码错误
               model.addAttribute("error", "密码错误");
               return "login";
           }
       }
   ```

2. 在`UserRealm.java`的认证方法中编写是否存在该用户：

   ```java
       //认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
           System.out.println("执行了=》认证doGetAuthenticationInfo");
   
           //实际的用户名密码从数据库中取，这里模拟一下
           String name = "root";
           String password = "123456";
   
           //将AuthenticationToken转为controller中用的UsernamePasswordToken
           UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;
   
           //用户名认证
           if (!userToken.getUsername().equals(name)) {
               return null;    //用户名不对会自动抛出UnknownAccountException异常
           }
   
           //密码认证，shiro帮我们做，有这个方法有3个参数，暂时只用中间这个对比密码的参数
           return new SimpleAuthenticationInfo("", password, "");
           
       }
   ```

3. 登录页面`login.html`：

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http//www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>登录</h1>
   <hr>
   <p th:text="${error}" style="color: red"></p>
   <form th:action="@{/login}">
       <p>用户名：<input type="text" name="username"></p>
       <p>密码：<input type="text" name="password"></p>
       <p><input type="submit"></p>
   </form>
   </body>
   </html>
   ```

4. 测试，登录无论是用户名还是密码错误都会有相应的提示。登录成功后可以进add和update页面。



### 16.3.4 整合MyBatis

1. 在pom文件中添加数据库相关的依赖：

   ```xml
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
               <version>1.0.9</version>
           </dependency>
   
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
   
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>2.1.4</version>
           </dependency>
   ```

2. 配置`application.yaml`：

   ```yaml
   spring:
     datasource:
       username: root
       password: 123456
       #?serverTimezone=UTC解决时区的报错
       url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource
   
       #Spring Boot 默认是不注入这些属性值的，需要自己绑定
       #druid 数据源专有配置
       initialSize: 5
       minIdle: 5
       maxActive: 20
       maxWait: 60000
       timeBetweenEvictionRunsMillis: 60000
       minEvictableIdleTimeMillis: 300000
       validationQuery: SELECT 1 FROM DUAL
       testWhileIdle: true
       testOnBorrow: false
       testOnReturn: false
       poolPreparedStatements: true
   
       #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
       filters: stat,wall,log4j
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

3. 配置`application.properties`：

   ```properties
   mybatis.type-aliases-package=com.run.pojo
   mybatis.mapper-locations=classpath:mapper/*xml
   ```

4. pojo包下的User实体类：

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

5. 编写dao层，mapper包下的`UserMapper.java`接口：

   ```java
   @Mapper
   public interface UserMapper {
       User queryUserByName(String name);
   }
   ```

   resources目录下新建mapper文件夹，添加`UserMapper.xml`：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--namespace=绑定一个对应的Dao/Mapper接口-->
   <mapper namespace="com.run.mapper.UserMapper">
   
       <select id="queryUserByName" parameterType="String" resultType="com.run.pojo.User">
           select * from mybatis.user where name = #{name}
       </select>
   
   </mapper>
   ```

6. 编写service层，新建`UserService.java`接口

   ```java
   public interface UserService {
       User queryUserByName(String name);
   }
   ```

   `UserServiceImpl.java`：

   ```java
   @Service
   public class UserServiceImpl implements UserService {
   
       @Autowired
       UserMapper userMapper;
   
       @Override
       public User queryUserByName(String name) {
           return userMapper.queryUserByName(name);
       }
   }
   ```

7. 在`UserRealm.java`中修改：

   ```java
   public class UserRealm extends AuthorizingRealm {
   
       @Autowired
       UserServiceImpl userService;
   
       //授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
           System.out.println("执行了=》授权doGetAuthorizationInfo");
           return null;
       }
   
       //认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
           System.out.println("执行了=》认证doGetAuthenticationInfo");
   
           //将AuthenticationToken转为controller中用的UsernamePasswordToken
           UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;
   
           //从真实数据库中查询
           User user = userService.queryUserByName(userToken.getUsername());
   
           //用户名认证
           if (user == null) {
               return null;    //用户名不对会自动抛出UnknownAccountException异常
           }
   
           //密码认证，shiro帮我们做，有这个方法有3个参数，暂时只用中间这个对比密码的参数
           return new SimpleAuthenticationInfo("", user.getPwd(), "");
   
       }
   
   }
   ```

8. 测试，发现数据库中存在的用户才能登录。

9. Shiro也可以像Spring Security一样实现密码加密，例如md5加密，md5盐值加密(会绑定用户的用户名信息等，更加安全)等。

   

### 16.3.5 实现请求授权

1. 在`shiroConfig.java`类中的`ShiroFilterFactoryBean`方法中编写权限控制：

   ```java
       //3. ShiroFilterFactoryBean
       @Bean
       public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager) {
           ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
           //设置安全管理器，关联DefaultWebSecurityManager
           bean.setSecurityManager(securityManager);
   
           //添加Shiro内置过滤器
           /*
               anon：无需认证就可以访问
               authc：必须认证了才能访问
               user：必须拥有 记住我 功能才能访问
               perms：拥有对某个资源的权限才能访问
               role：拥有某个角色权限才能访问
            */
   
           Map<String, String> filterMap = new LinkedHashMap<>();
           //授权，未授权的用户会跳转到未授权页面。注意：写在拦截前面
           //有user:add权限才能进/user/add
           filterMap.put("/user/add", "perms[user:add]");
           filterMap.put("/user/update", "perms[user:update]");
   
           //拦截
           filterMap.put("/user/*", "authc");
   
           bean.setFilterChainDefinitionMap(filterMap);
   
           //设置登录请求
           bean.setLoginUrl("/toLogin");
           //设置未授权页面
           bean.setUnauthorizedUrl("/noauth");
   
           return bean;
       }
   ```

2. controller层处理未授权页面：

   ```java
       @RequestMapping("/noauth")
       @ResponseBody
       public String noauth() {
           return "未经授权不得访问此页面";
       }
   ```

3. 此时在数据库的user表中添加一个perms字段，用于存放用户的权限，例如给狂神update权限，给xiaoluadd权限，如下图：

   ![image-20220403171551122](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/160f7159242c432f72a7457f0a55fd68.png)

   

4. 在`UserRealm.java`类中的授权方法中赋予权限

   ```java
   public class UserRealm extends AuthorizingRealm {
   
       @Autowired
       UserServiceImpl userService;
   
       //授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
           System.out.println("执行了=》授权doGetAuthorizationInfo");
   
           SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
   
           //拿到当前登录的这个对象
           Subject subject = SecurityUtils.getSubject();
           //拿到User对象，下面认证方法中的SimpleAuthenticationInfo(第一个参数principal把user传递过来了)
           User currentUser = (User) subject.getPrincipal();
           //从数据库中获取该用户的权限，赋予相应的权限
           info.addStringPermission(currentUser.getPerms());
   
           return info;
       }
   
       //认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
           System.out.println("执行了=》认证doGetAuthenticationInfo");
   
           //将AuthenticationToken转为controller中用的UsernamePasswordToken
           UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;
   
           //从真实数据库中查询
           User user = userService.queryUserByName(userToken.getUsername());
   
           //用户名认证
           if (user == null) {
               return null;    //用户名不对会自动抛出UnknownAccountException异常
           }
   
           //密码认证，shiro帮我们做，第一个参数传递当前登录的用户，第二个参数传递他的密码
           return new SimpleAuthenticationInfo(user, user.getPwd(), "");
   
       }
   
   }
   ```

5. 通过测试可以发现，当登录狂神用户时，只能进update不能进add，当登录xiaolu用户时，只能进add不能进update。



### 16.3.6 整合Thymeleaf

权限控制应该和前端相结合，有什么权限的用户在他的主页就显示什么权限，没有该权限就不显示出来。

1. pom文件中添加相关依赖：

   ```xml
   	<dependency>
               <groupId>com.GitHub.theborakompanioni</groupId>
               <artifactId>thymeleaf-extras-shiro</artifactId>
               <version>2.1.0</version>
           </dependency>
   ```

2. 在`ShiroConfig.java`中添加`ShiroDialect`方法，用来整合shiro thymeleaf：

   ```java
       //ShiroDialect：用来整合shiro thymeleaf
       @Bean
       public ShiroDialect getShiroDialect() {
           return new ShiroDialect();
       }
   ```

3. 在`index.html`首页中修改，注意导入`xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro"`：

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org"
         xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>首页</h1>
   <p th:text="${msg}"></p>
   <hr>
   
   <!--若没有被认证，就显示登录-->
   <div shiro:notAuthenticated>
       <a th:href="@{/toLogin}">登录</a>
   </div>
   
   <div shiro:hasPermission="user:add">
       <a th:href="@{/user/add}">add</a>
   </div>
   
   <div shiro:hasPermission="user:update">
       <a th:href="@{/user/update}">update</a>
   </div>
   
   </body>
   </html>
   ```

4. 测试，发现未登录的时候只显示登录链接，用户登录后，有什么权限就只显示什么权限。



> shiro相关JSP标签

```jsp
<shiro:authenticated>        登录之后
<shiro:notAuthenticated>        不在登录状态时
<shiro:guest>            用户在没有RememberMe时
<shiro:user>            用户在RememberMe时
<shiro:hasAnyRoles name="abc,123" >    在有abc或者123角色时
<shiro:hasRole name="abc">        拥有角色abc
<shiro:lacksRole name="abc">        没有角色abc
<shiro:hasPermission name="abc">    拥有权限资源abc
<shiro:lacksPermission name="abc">    没有abc权限资源
<shiro:principal>        显示用户身份名称
<shiro:principal property="username"/>         显示用户身份中的属性值
```

## 16.4 总结

**Shiro 三大要素**

`subject` -> `ShiroFilterFactoryBean`
`securityManager` -> `DefaultWebSecurityManager`
`realm`
实际操作中对象创建的顺序 ： `realm -> securityManager -> subject`



> 流程梳理

1、用户进入首页点击跳转，Shiro内置过滤器进行拦截，看过滤器的设置，未认证跳转到登录页面，未授权跳转到未授权界面

2、认证

- 用户进入登录页面，输入用户名密码准备进行认证，点击登录按钮后，会请求/login，
- 首先调用`Subject.login(token)` 进行登录，其会自动委托给`SecurityManager`，
- `SecurityManager`负责真正的身份验证逻辑；它会委托给`Authenticator` 进行身份验证；
- `Authenticator` 才是真正的身份验证者，`Authenticator` 会把相应的token 传入`Realm`，从`Realm` 获取身份验证信息，如果没有返回/抛出异常表示身份验证失败了，如果有就返回`AuthenticationInfo`验证信息，此信息中包含了身份（pricipals）及凭证，也就是账号密码。

![image-20220403181857060](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/457a78197f0d5b9d69a45c8198bd8be1.png)

3、授权

- 对`subject`进行授权，调用方法`isPermitted(“permission串”)`，递交给`SecurityManager`
- `SecurityManager`将权限检测操作委托给`Authorizer`授权管理器对象
- `Authorizer`执行`Realm`（自定义的Realm）从数据库查询权限数据并封装
- `Authorizer`对用户授权信息进行判定(判断用户访问资源时需要什么权限，假如用户所具有的权限包含这个资源访问时所需要的权限，那么用户就可以访问这个资源了)

![image-20220403181951827](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/69bd56e8003d87d1bf61a317a8a3c3ff.png)

# 17. Swagger

- 了解Swagger的概念及作用
- 了解前后端分离
- 在springboot中集成swagger



**前后端分离**

Vue+SpringBoot

后端时代：前端只用管理静态页面；html==>后端。模板引擎JSP=>后端才是主力

**前后端分离时代**

- 前端 -> 前端控制层、视图层
  - 伪造后端数据，json。已经存在了，不需要后端，前端工程队依旧能够跑起来
- 后端 -> 后端控制层、服务层、数据访问层
- 前后端通过API进行交互
- 前后端相对独立且松耦合

**产生的问题**

- 前后端集成联调，前端或者后端无法做到 “及时协商，尽早解决”，最终导致问题集中爆发

**解决方案**

- 首先定义schema [ 计划的提纲 ]，并实时跟踪最新的API，降低集成风险
- 早些年：指定word计划文档
- 前后端分离：
  - 前端测试后端接口：postman
  - 后端提供接口，需要实时更新最新的消息及改动

## 17.1 Swagger简介

- 号称世界上最流行的API框架
- Restful Api 文档在线自动生成器 => **API 文档 与API 定义同步更新**
- 直接运行，在线测试API
- 支持多种语言 （如：Java，PHP等）
- 官网：[https://swagger.io/](https://gitee.com/link?target=https%3A%2F%2Fswagger.io%2F)

## 17.2 SpringBoot集成Swagger

**SpringBoot集成Swagger** => **springfox**，两个jar包

- [Springfox-swagger2](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Fio.springfox%2Fspringfox-swagger2)
- [springfox-swagger-ui](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Fio.springfox%2Fspringfox-swagger-ui)

**使用Swagger**

要求：jdk 1.8 + ，否则swagger2无法运行

步骤：

1. 新建一个SpringBoot-web项目

2. 添加Maven依赖（注意：2.9.2版本之前）

   ```xml
   <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>2.9.2</version>
   </dependency>
   
   <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.9.2</version>
   </dependency>
   ```

3. 编写HelloController，测试确保运行成功！

4. 要使用Swagger，我们需要编写一个配置类`SwaggerConfig`来配置 Swagger

   ```java
   @Configuration 		//配置类
   @EnableSwagger2		// 开启Swagger2的自动配置
   public class SwaggerConfig {  
   }
   ```

5. 访问测试 ：[http://localhost:8080/swagger-ui.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fswagger-ui.html) ，可以看到swagger的界面，若报错或者进不去此页面，降springboot版本；

   ![image-20220404110035005](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/aafc42ddad81e7ab9dd1dc6bf470fd73.png)

## 17.3 配置Swagger

1. Swagger实例Bean是`Docket`，所以通过配置Docket实例来配置Swaggger。

   ```java
   @Configuration 		//配置类
   @EnableSwagger2		// 开启Swagger2的自动配置
   public class SwaggerConfig {
   
       @Bean       //配置docket的Bean实例，以配置Swagger具体参数
       public Docket docket() {
           return new Docket(DocumentationType.SWAGGER_2);
       }
       
   }
   ```

2. 可以通过`apiInfo()`属性配置文档信息

   ```java
   //配置Swagger文档信息
       private ApiInfo apiInfo() {
           //作者信息
           Contact contact = new Contact("联系人名字", "http://xxx.xxx.com/联系人访问链接", "联系人邮箱");
           return new ApiInfo(
                   "Swagger学习",        // 标题
                   "学习演示如何配置Swagger",      // 描述
                   "v1.0", // 版本
                   "http://terms.service.url/组织链接",        // 组织链接
                   contact,        // 联系人信息
                   "Apach 2.0 许可",         // 许可
                   "许可链接",         // 许可连接
                   new ArrayList<>()       // 扩展
           );
       }
   ```

3. `Docket` 实例关联上 `apiInfo()`，覆盖默认的apiInfo：

   ```java
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
   }
   ```

4. 重启项目，访问测试 [http://localhost:8080/swagger-ui.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fswagger-ui.html)  看下效果；

   ![image-20220404111517626](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/21c96bea67b0a7153c43f551547fd111.png)

## 17.4 配置扫描接口

1. 构建`Docket`时通过`select()`方法配置怎么扫描接口。

   ```java
       @Bean       //配置docket的Bean实例，以配置Swagger具体参数
       public Docket docket() {
           return new Docket(DocumentationType.SWAGGER_2)
                   .apiInfo(apiInfo())
                   .select()
                   // 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
                   .apis(RequestHandlerSelectors.basePackage("com.run.swagger.controller"))
                   .build();
       }
   ```

2. 重启项目测试，由于我们配置根据包的路径扫描接口，所以我们只能看到一个类

   ![image-20220404112331553](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3a02416d861386aeca56d34d5355fc8d.png)

3. 除了通过包路径配置扫描接口外，还可以通过配置其他方式扫描接口，这里注释一下所有的配置方式；这些方法在`apis中的RequestHandlerSelectors`中通过`.`使用：

   ```java
   basePackage(final String basePackage) 		// 根据包路径扫描接口
   any() 		// 扫描所有，项目中的所有接口都会被扫描到
   none() 		// 不扫描接口
   // 通过方法上的注解扫描，如withMethodAnnotation(GetMapping.class)只扫描get请求
   withMethodAnnotation(final Class<? extends Annotation> annotation)
   // 通过类上的注解扫描，如.withClassAnnotation(Controller.class)只扫描有controller注解的类中的接口
   withClassAnnotation(final Class<? extends Annotation> annotation)
   ```

4. 除此之外，我们还可以配置接口扫描过滤：

   ```java
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2)
         .apiInfo(apiInfo())
         .select()
         .apis(RequestHandlerSelectors.basePackage("com.run.swagger.controller"))
          // 配置如何通过path过滤,即这里只扫描请求以/user开头的接口
         .paths(PathSelectors.ant("/user/**"))
         .build();
   }
   ```

   - `paths的PathSelectors`中的可选值还有

   ```java
   any() // 任何请求都扫描
   none() // 任何请求都不扫描
   regex(final String pathRegex) // 通过正则表达式控制
   ant(final String antPattern) // 通过ant()控制
   ```

## 17.5 配置Swagger开关

1. 通过`enable()`方法配置是否启用swagger，如果是false，swagger将不能在浏览器中访问了

   ```java
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2)
         .apiInfo(apiInfo())
         .enable(false) 		//配置是否启用Swagger，如果是false，在浏览器将无法访问
         .select()
         .apis(RequestHandlerSelectors.basePackage("nuc.ss.swagger.controller"))
         .paths(PathSelectors.ant("/ss/**"))
         .build();
   }
   ```

![image-20220404113026018](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a05828ce523ac72625e823dde7c5b18b.png)

如何动态配置当项目处于dev环境时显示swagger，处于pro时不显示？

- 在`Docket`方法中配置：

  ```java
      @Bean       //配置docket的Bean实例，以配置Swagger具体参数
      public Docket docket(Environment environment) {
          // 设置要显示swagger的环境，可以配置多个，里面test并没有写
          Profiles of = Profiles.of("dev", "test");
          // 判断当前是否处于该环境
          boolean b = environment.acceptsProfiles(of);
          // 下面通过 enable() 接收此参数判断是否要显示
  
          return new Docket(DocumentationType.SWAGGER_2)
                  .apiInfo(apiInfo())
                  .enable(b)
                  .select()
                  // 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
                  .apis(RequestHandlerSelectors.basePackage("com.run.swagger.controller"))
                  .build();
      }
  ```

  

可以在项目中增加配置文件

- dev测试环境：降主配置文件`application.properties`设置成dev环境，dev环境的端口为8081

  ![image-20220404115207467](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8c23ee9a8d41d872bbd40a05e4873f4f.png)

  

  项目运行结果，可以发现，走8081端口，进入了dev环境：

  ![image-20220404115421402](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5731c3bbff9c1c5890c9c20e136ec8fb.png)

- pro测试环境：降主配置文件`application.properties`设置成pro环境，pro环境的端口为8082

  ![image-20220404115524869](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f46e321eca19ead48b16de866938305b.png)

  

  项目运行结果，因为不是dev环境，所以进不去Swagger页面

  ![image-20220404115616468](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0eef18acba022ae7533ad00525798538.png)

## 17.6 配置API分组

1. 如果没有配置分组，默认是default。通过`groupName()`方法即可配置分组：

   ```java
   @Bean
   public Docket docket(Environment environment) {
      return new Docket(DocumentationType.SWAGGER_2)
                   .apiInfo(apiInfo())
                   .groupName("AruNi")     //配置分组
                   //.enable(b)
                   .select()
                   // 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
                   .apis(RequestHandlerSelectors.basePackage("com.run.swagger.controller"))
                   .build();
   }
   ```

2. 重启项目查看分组

   ![image-20220404122110998](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/102d870450de26e62d1935ff4128206d.png)

3. 如何配置多个分组？配置多个分组只需要配置多个`Docket`即可：

   ```java
   @Bean
   public Docket docket1(){
      return new Docket(DocumentationType.SWAGGER_2).groupName("group1");
   }
   @Bean
   public Docket docket2(){
      return new Docket(DocumentationType.SWAGGER_2).groupName("group2");
   }
   @Bean
   public Docket docket3(){
      return new Docket(DocumentationType.SWAGGER_2).groupName("group3");
   }
   ```

4. 重启项目查看即可

   ![image-20220404122220500](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cc520ef5d190b4685911201ede1b2066.png)

## 17.7 实体配置

1. 新建一个实体类，private属性不会在Swagger的实体中显示

   ```java
   //@Api("注释")
   @ApiModel("用户实体")
   public class User {
       @ApiModelProperty("用户名")
       public String username;
       @ApiModelProperty("密码")
       public String password;
   
       //set, get方法，有参无参构造器
   }
   ```

2. 只要这个实体在**请求接口**的返回值上（即使是泛型），都能映射到实体项中：

   ```java
   @RestController
   public class HelloController {
   
       @GetMapping("/hello")
       public String hello() {
           return "hello";
       }
   
       //只要接口中，返回值中存在实体类，他就会被扫描到Swagger中
       @PostMapping("/user")
       public User user() {
           return new User();
       }
       
   }
   ```

3. 重启查看测试

   ![image-20220404123150755](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/692c6e4ab2fb0b29e5b4e537c78d628c.png)

注：并不是因为@ApiModel这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而@ApiModel和@ApiModelProperty这两个注解只是为实体添加注释的。

- @ApiModel为类添加注释
- @ApiModelProperty为类属性添加注释

**总结：**

- 我们可以通过Swagger给一些比较难理解的接口或者属性，增加注释信息
- 接口文档实时更新
- 可以在线测试

Swagger是一个优秀的工具，几乎所有大公司都有使用它

【注意点】：在项目正式发布的时候，关闭Swagger！！！

- 出于安全考虑
- 而且节省内存

## 17.8 常用注解

Swagger的所有注解定义在io.swagger.annotations包下

下面列一些经常用到的，未列举出来的可以另行查阅说明：

| Swagger注解                                            | 简单说明                                             |
| ------------------------------------------------------ | ---------------------------------------------------- |
| @Api(tags = "xxx模块说明")                             | 作用在模块类上                                       |
| @ApiOperation("xxx接口说明")                           | 作用在接口方法上                                     |
| @ApiModel("xxxPOJO说明")                               | 作用在模型类上：如VO、BO                             |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) | 作用在类方法和属性上，hidden设置为true可以隐藏该属性 |
| @ApiParam("xxx参数说明")                               | 作用在参数、方法和字段上，类似@ApiModelProperty      |

我们也可以给请求的接口配置一些注释

1. 在HelloController控制类中的接口添加api接口注释

   ```java
   @Api(tags = "HelloController模块")
   @RestController
   public class HelloController {
   
       @GetMapping("/hello")
       public String hello() {
           return "Hello Swagger";
       }
   
       //只要接口中，返回值中存在实体类，他就会被扫描到Swagger中
       @PostMapping("/user")
       public User user() {
           return new User();
       }
   
       @ApiOperation("Hi控制接口")
       @GetMapping("/hi")
       public String hi(@ApiParam("用户名") String username) {
           return "Hi" + username;
       }
   
       @ApiOperation("get测试")
       @GetMapping("/get")
       public User hello2(@ApiParam("用户") User user) {
           return user;
       }
   }
   ```

2. 运行项目，可以看见在HelloController模块中，有对应的请求方法：

   ![image-20220404124658297](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/399996141796ea8707b2de9bbde79a2d.png)

3. 在`/get`中进行try it out测试

   ![image-20220404124820937](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1c47efe3e7af4bd1a89de4e154729dd8.png)

   点击try it out后，填入账号密码，进行测试：

   ![image-20220404124920698](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/326a2cec8302c8c5c859f63c8e69bbab.png)

   测试结果，可以看到测试成功，返回了user对象中的信息，状态码为200：

   ![image-20220404125332238](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fb68b477947b70b4225c99a4eb9488d8.png)

**总结：**

1. 这样的话，可以给一些比较难理解的属性或者接口，增加一些配置信息，让人更容易阅读！
2. 相较于传统的Postman或Curl方式测试接口，使用swagger简直就是傻瓜式操作，不需要额外说明文档(写得好本身就是文档)而且更不容易出错，只需要录入数据然后点击Execute，如果再配合自动化框架，可以说基本就不需要人为操作了。
3. Swagger是个优秀的工具，现在国内已经有很多的中小型互联网公司都在使用它，相较于传统的要先出Word接口文档再测试的方式，显然这样也更符合现在的快速迭代开发行情。当然了，提醒下大家在正式环境要记得关闭Swagger，一来出于安全考虑二来也可以节省运行时内存。

## 17.9 拓展：其他皮肤

我们可以导入不同的包实现不同的皮肤定义：

1、默认的  **访问 [http://localhost:8080/swagger-ui.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fswagger-ui.html)**

```xml
<dependency> 
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>2.9.2</version>
</dependency>
```

![image-20220404125716852](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8f1a3670fe259a83e4501f432e97b071.png)

2、bootstrap-ui  **访问 [http://localhost:8080/doc.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdoc.html)**

```xml
<!-- 引入swagger-bootstrap-ui包 /doc.html-->
<dependency>
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>swagger-bootstrap-ui</artifactId>
   <version>1.9.1</version>
</dependency>
```

![image-20220404125937979](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0a952c790aec8982144807059d639c82.png)

3、Layui-ui  **访问 [http://localhost:8080/docs.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdocs.html)**

```xml
<!-- 引入swagger-ui-layer包 /docs.html-->
<dependency>
   <groupId>com.github.caspar-chen</groupId>
   <artifactId>swagger-ui-layer</artifactId>
   <version>1.1.3</version>
</dependency>
```

- Layui已经停止维护了，可能访问不成功

4、mg-ui  **访问 [http://localhost:8080/document.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdocument.html)**

```xml
<!-- 引入swagger-ui-layer包 /document.html-->
<dependency>
   <groupId>com.zyplayer</groupId>
   <artifactId>swagger-mg-ui</artifactId>
   <version>1.0.6</version>
</dependency>
```

![image-20220404130143149](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9fb7657db70c2a257f6e2c83174b8ad6.png)





# 18. 异步、定时、邮件任务

## 18.1 异步任务

异步处理还是十分常用的，比如我们在网站上发送邮件，后台会去发送邮件，此时前台会造成响应不动，直到邮件发送完毕，响应才会成功，所以我们一般会采用多线程的方式去处理这些任务。

1. 新建SpringBoot-Web项目

2. 创建一个service包，创建一个类AsyncService

   编写方法，假装正在处理数据，使用线程设置一些延时，模拟同步等待的情况；

   ```java
   @Service
   public class AsyncService {
   
      public void hello(){
          try {
              Thread.sleep(3000);
         } catch (InterruptedException e) {
              e.printStackTrace();
         }
          System.out.println("业务进行中....");
     }
   }
   ```

3. 新建controller包，编写AsyncController类

   我们去写一个Controller测试一下

   ```java
   @RestController
   public class AsyncController {
   
      @Autowired
      AsyncService asyncService;
   
      @GetMapping("/hello")
      public String hello(){
          asyncService.hello();
          return "OK";
     }
   
   }
   ```

4. 访问http://localhost:8080/hello进行测试，浏览器必须转3秒后，才显示OK，然后控制台打印出业务进行中...

   如果想让用户直接得到消息，就在后台使用多线程的方式进行处理即可，但是每次都需要自己手动去编写多线程的实现的话，太麻烦了，我们只需要用一个简单的办法，在我们的方法上加一个简单的注解即可，如下：

5. 给hello方法添加@Async注解；

   ```java
   //告诉Spring这是一个异步方法
   @Async
   public void hello(){
      try {
          Thread.sleep(3000);
     } catch (InterruptedException e) {
          e.printStackTrace();
     }
      System.out.println("业务进行中....");
   }
   ```

   SpringBoot就会自己开一个线程池，进行调用！但是要让这个注解生效，我们还需要在主程序上添加一个注解@EnableAsync ，开启异步注解功能；

   ```java
   @EnableAsync //开启异步注解功能
   @SpringBootApplication
   public class SpringbootTaskApplication {
   
      public static void main(String[] args) {
          SpringApplication.run(SpringbootTaskApplication.class, args);
     }
   
   }
   ```

7、重启测试，网页瞬间响应OK，后台的业务进行中...是3秒过后才在控制台输出，所以后台代码依旧执行。

## 18.2 邮件任务

邮件发送，在我们的日常开发中，也非常的多，Springboot也帮我们做了支持

- 邮件发送需要引入`spring-boot-start-mail`
- SpringBoot 自动配置`MailSenderAutoConfiguration`
- 定义`MailProperties`内容，配置在`application.yml`中
- 自动装配`JavaMailSender`
- 测试邮件发送

**测试：**

1. 引入pom依赖

   ```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail</artifactId>
   </dependency>
   ```

   点击进去看它引入的依赖，可以看到 `jakarta.mail`

   ```xml
   <dependency>
      <groupId>com.sun.mail</groupId>
      <artifactId>jakarta.mail</artifactId>
      <scope>compile</scope>
   </dependency>
   ```

2. 查看自动配置类：`MailSenderAutoConfiguration`，可以发现这个类中没有注册bean，点进`MailSenderJndiConfiguration.class`查看导入的其他类，这个类中存在bean，`JavaMailSenderImpl`：

   ![image-20220404145845221](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7056ba7fb14621b6b4deaea30da3ca86.png)

   然后点进`MailProperties`去看下配置文件

   ```java
   @ConfigurationProperties(prefix = "spring.mail")
   public class MailProperties {
   
   	private static final Charset DEFAULT_CHARSET = StandardCharsets.UTF_8;
   	private String host;
   	private Integer port;
   	private String username;
   	private String password;
   	private String protocol = "smtp";
   	private Charset defaultEncoding = DEFAULT_CHARSET;
   	private Map<String, String> properties = new HashMap<>();
   	private String jndiName;
   	//set、get方法省略。。。
   }
   ```

3. 自己在`application.properties`中配置Mail配置文件：

   ```properties
   spring.mail.username=1710841251@qq.com
   spring.mail.password=你的qq授权码
   spring.mail.host=smtp.qq.com
   # QQ需要配置ssl加密验证
   spring.mail.properties.mail.smtp.ssl.enable=true
   ```

   获取授权码：在QQ邮箱中的设置->账户->开启pop3和smtp服务

   ![image-20220404150506323](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9372c8da7e7f4946879b53c0beff55f5.png)

4. 在SpringBoot单元测试中测试：

   ```java
   @SpringBootTest
   class SpringBootTaskApplicationTests {
   
       @Autowired
       JavaMailSenderImpl mailSender;
   
       @Test
       void contextLoads() {
           SimpleMailMessage mailMessage = new SimpleMailMessage();
           //邮件主题
           mailMessage.setSubject("SpringBoot如何编写邮件");
           //邮件内容
           mailMessage.setText("只需要导入一个spring-boot-starter-mail依赖，您就可以轻松的编写邮件啦！！！");
           //发送给谁
           mailMessage.setTo("1298911600@qq.com");
           mailMessage.setFrom("1298911600@qq.com");
           //发送
           mailSender.send(mailMessage);
       }
       
   }
   ```

   这样一个简单的邮件就发送成功了，查看邮箱：

   ![image-20220404151909904](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/52cbeb75aba47d856916fdeebb53fbd1.png)

5. 发送复杂的邮件

   ```java
   @SpringBootTest
   class SpringBootTaskApplicationTests {
   
       @Autowired
       JavaMailSenderImpl mailSender;
   
       //复杂的邮件
       @Test
       void contextLoads2() throws MessagingException {
           MimeMessage mimeMessage = mailSender.createMimeMessage();
           //组装，第二个参数开启多文件发送
           MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);
   
           //正文
           helper.setSubject("SpringBoot如何发送复杂邮件");
           //第二个参数开启支持html标签
           helper.setText("<a href=https://spring.io/ style='color:green'>点击前往官网学习！</a>", true);
   
           //附件
           helper.addAttachment("Spring.jpg", new File("D:\\shejie\\Spring.jpg"));
       
           helper.setTo("1298911600@qq.com");
           helper.setFrom("1298911600@qq.com");
           
           mailSender.send(mimeMessage);
       }
   
   }
   ```

   查看邮箱：

   ![image-20220404153926297](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/35da9bbff0cd2f65bef21a644b61a223.png)

## 18.3 定时任务

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，Spring为我们提供了异步执行任务调度的方式，提供了两个接口。

- TaskExecutor接口（任务执行者）
- TaskScheduler接口（任务调度者）

两个注解：

- `@EnableScheduling`——开启定时功能的注解
- `@Scheduled`——什么时候执行

**cron表达式：**

| 字段 | 允许值                | 允许特殊字符    |
| ---- | --------------------- | --------------- |
| 秒   | 0-59                  | , - * /         |
| 分   | 0-59                  | , - * /         |
| 小时 | 0-23                  | , - * /         |
| 日期 | 1-31                  | , - * / ? L W C |
| 月份 | 1-12                  | , - * /         |
| 星期 | 0-1或SUN-SAT 0,7是SUN | , - * / ? L W C |

| 特殊字符 | 代表含义                   |
| -------- | -------------------------- |
| ,        | 枚举                       |
| -        | 区间                       |
| *        | 任意                       |
| /        | 步长                       |
| ?        | 日/星期冲突匹配            |
| L        | 最后                       |
| W        | 工作日                     |
| C        | 和calendar练习后计算过的值 |
| #        | 星期，4#2 第二个星期三     |

**测试步骤：**

1、创建一个`ScheduledService`类

我们里面存在一个hello方法，他需要定时执行，怎么处理呢？

```java
@Service
public class ScheduledService {

    //cron表达式，以空格分隔
    // 秒 分 时 日 月 周几

    /*
        0 49 11 * * ?   每天的11点49分00秒执行
        0 0/5 11,12 * * ?   每天的11点和12点每个五分钟执行一次
        0 15 10 ? * 1-6     每个月的周一到周六的10点15分执行一次
        0/2 * * * * ?     每2秒执行一次
     */
    @Scheduled(cron = "0/2 * * * * ?")
    public void hello() {
        System.out.println("hello,你被执行了");
    }
}
```

2、这里写完定时任务之后，我们需要在主程序上增加@EnableScheduling 开启定时任务功能

```java
@EnableAsync 		//开启异步注解功能
@EnableScheduling 		//开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {

   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }

}
```

3、运行项目，发现hello方法每2秒执行一次，定时任务成功！

- cron在线生成：[http://www.bejson.com/othertools/cron/](https://gitee.com/link?target=http%3A%2F%2Fwww.bejson.com%2Fothertools%2Fcron%2F)

> 常用cron表达式

```text
（1）0/2 * * * * ?   表示每2秒 执行任务
（1）0 0/2 * * * ?   表示每2分钟 执行任务
（1）0 0 2 1 * ?   表示在每月的1日的凌晨2点调整任务
（2）0 15 10 ? * MON-FRI   表示周一到周五每天上午10:15执行作业
（3）0 15 10 ? 6L 2002-2006   表示2002-2006年的每个月的最后一个星期五上午10:15执行作
（4）0 0 10,14,16 * * ?   每天上午10点，下午2点，4点
（5）0 0/30 9-17 * * ?   朝九晚五工作时间内每半小时
（6）0 0 12 ? * WED   表示每个星期三中午12点
（7）0 0 12 * * ?   每天中午12点触发
（8）0 15 10 ? * *   每天上午10:15触发
（9）0 15 10 * * ?     每天上午10:15触发
（10）0 15 10 * * ?   每天上午10:15触发
（11）0 15 10 * * ? 2005   2005年的每天上午10:15触发
（12）0 * 14 * * ?     在每天下午2点到下午2:59期间的每1分钟触发
（13）0 0/5 14 * * ?   在每天下午2点到下午2:55期间的每5分钟触发
（14）0 0/5 14,18 * * ?     在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发
（15）0 0-5 14 * * ?   在每天下午2点到下午2:05期间的每1分钟触发
（16）0 10,44 14 ? 3 WED   每年三月的星期三的下午2:10和2:44触发
（17）0 15 10 ? * MON-FRI   周一至周五的上午10:15触发
（18）0 15 10 15 * ?   每月15日上午10:15触发
（19）0 15 10 L * ?   每月最后一日的上午10:15触发
（20）0 15 10 ? * 6L   每月的最后一个星期五上午10:15触发
（21）0 15 10 ? * 6L 2002-2005   2002年至2005年的每月的最后一个星期五上午10:15触发
（22）0 15 10 ? * 6#3   每月的第三个星期五上午10:15触发
```

# 19. Dubbo和Zookeeper集成

## 19.1 分布式理论

### 19.1.1 什么是分布式系统？

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。



随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/23791576e8f92918933e5ec1909d4538.jpeg)

### 19.1.2 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

![image-20200801133522303](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b53b931eba09b6b309e8083d1fbdc4d5.png)

适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

**缺点：**

1、性能扩展比较难

2、协同开发问题

3、不利于升级维护

### 19.1.3 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

![image-20200801133625663](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/db4f4b956f6bc3564b0179d5053855b1.png)

通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

缺点：公用模块无法重复利用，开发性的浪费

### 19.1.4 分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服务框架(RPC)**是关键。

![image-20200801133710784](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/28ce18804e556da83272c56e6d4af10f.png)

### 19.1.5 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心**(SOA)[ Service Oriented Architecture]是关键。

![image-20200801133801873](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fd5d4ac8e6f2f547444deda813480e74.png)

## 19.2 什么是RPC

RPC【Remote Procedure  Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：[https://www.jianshu.com/p/2accc2840a1b](https://gitee.com/link?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2F2accc2840a1b)

**RPC基本原理**

![image-20220404161600156](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2b4e8fb8fe70058822f631b080f25f2c.png)

![image-20200801134657756](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2533494e3b47af7b1a725031b7696001.png)

RPC两个核心模块：

- 通讯
- 序列化

## 19.3 Dubbo

Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

dubbo官网 [http://dubbo.apache.org/zh-cn/index.html](https://gitee.com/link?target=http%3A%2F%2Fdubbo.apache.org%2Fzh-cn%2Findex.html)

1. 了解Dubbo的特性
2. 查看官方文档

**dubbo基本概念**

![image-20220404163957402](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/05fb6f668e7b47c68f37e0e4dfcb7b86.png)

**服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

**服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

**调用关系说明**

- 服务容器负责启动，加载，运行服务提供者。
- 服务提供者在启动时，向注册中心注册自己提供的服务。
- 服务消费者在启动时，向注册中心订阅自己所需的服务。
- 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
- 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
- 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

## 19.4 Dubbo环境搭建

dubbo官方文档，推荐我们使用Zookeeper 注册中心

什么是zookeeper呢？可以查看[官方文档](https://gitee.com/link?target=https%3A%2F%2Fzookeeper.apache.org%2F)

## 19.5 安装Zookeeper

1. 下载zookeeper ：[地址](https://gitee.com/link?target=https%3A%2F%2Fmirror.bit.edu.cn%2Fapache%2Fzookeeper%2F)， 下载带bin的

2. 运行/bin/zkServer.cmd ，初次运行会报错，没有`zoo.cfg`配置文件；

   可能遇到问题：闪退 !

   解决方案：编辑`zkServer.cmd`文件末尾添加pause 。这样运行出错就不会退出，会提示错误信息，方便找到原因。

   ![image-20220404175253688](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b033bf19c258299959b1364f17cd953e.png)

3. 修改`zoo.cfg`配置文件

   - 将conf文件夹下面的`zoo_sample.cfg`复制一份改名为`zoo.cfg`即可。

   - 注意几个重要位置：

     `dataDir=./` ：临时数据存储的目录（可写相对路径）

     `clientPort=2181` ：zookeeper的端口号

   - 修改完成后再次启动zookeeper

     ![image-20220404180651111](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/742466c79333f5c4c545374bf43f8201.png)

4. 使用`zkCli.cmd`测试

   - `ls /`：列出zookeeper根下保存的所有节点

     ![image-20220404180741993](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5f5fe841ae851a1dcbdc84973da3e511.png)

   - `create –e /AruNi 123`：创建一个AruNi节点，值为123

     ![image-20220404180836309](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fe4349c9d15ceda09ba3e47ed59dc2a4.png)

   - `get /AruNi`：获取 /AruNi节点的值

     ![image-20220404180922172](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fe5d55f1e155bfe4dee52193194c283d.png)

   - 我们再来查看一下节点：ls /

     ![image-20220404180948329](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4ce422656de11d3afef418495051949e.png)

## 19.6 安装Dubbo-admin

- dubbo本身并不是一个服务软件。它其实就是一个jar包，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。
- 但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

我们这里来安装一下：

**1、下载dubbo-admin**

地址 ：[https://github.com/apache/dubbo-admin/tree/master](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fapache%2Fdubbo-admin%2Ftree%2Fmaster)

**2、解压进入目录**

修改 `dubbo-admin-server\src\main\resources \application.properties` 指定zookeeper地址`dubbo.registry.address`，若没有修改过，是2181就行。

```properties
server.port=7001
spring.velocity.cache=false
spring.velocity.charset=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest

dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3、在项目目录下**打包dubbo-admin，需要maven环境

```shell
mvn clean package -Dmaven.test.skip=true
```

**第一次打包的过程有点慢，需要耐心等待！直到成功！**

4、执行 dubbo-admin\target 下的`dubbo-admin-0.0.1-SNAPSHOT.jar`

```
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

【注意：zookeeper的服务一定要打开！】

执行完毕，我们去访问一下 [http://localhost:7001/](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A7001%2F) ， 这时候我们需要输入登录账户和密码，我们都是默认的root-root；

![image-20200801183043114](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4ca84c7b5728810ed3b2fbaa41c9ec3e.png)

登录成功后，查看界面

![image-20200801183113485](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/755e0e3c808b59fc518cd9307e66d98c.png)

安装完成！

**总结：**

- zookeeper :注册中心
- dubbo-admin:是一 个监控管理后台-查看我们注册了哪些服务，哪些服务被消费了（可以不用）
- Dubbo: jar包

## 19.7 SpringBoot + Dubbo + zookeeper

### 19.7.1 框架搭建

**1. 启动zookeeper ！**

**2. IDEA创建一个空项目；**

**3.创建一个模块，实现服务提供者：provider-server ， 选择web依赖即可**

**4.项目创建完毕，我们写一个服务，比如卖票的服务；**

编写接口

```java
package nuc.ss.service;

public interface TicketService {
   public String getTicket();
}
```

编写实现类

```java
package nuc.ss.service;

public class TicketServiceImpl implements TicketService {
    @Override
    public String getTicket() {
        return "《狂神说Java》";
    }
}
```

**5.创建一个模块，实现服务消费者：consumer-server ， 选择web依赖即可**

**6.项目创建完毕，我们写一个服务，比如用户的服务；**

编写service

```java
package nuc.ss.service;

public interface UserService {
   //我们需要去拿去注册中心的服务
}
```

**需求：现在我们的用户想使用买票的服务，这要怎么弄呢 ？**

### 19.7.2 服务提供者

**1、将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导包**

**我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包**

```xml
<!-- Dubbo Spring Boot Starter -->
<dependency>
   <groupId>org.apache.dubbo</groupId>
   <artifactId>dubbo-spring-boot-starter</artifactId>
   <version>2.7.3</version>
</dependency>    
```

**zookeeper的包我们去maven仓库下载，zkclient；**

```xml
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
```

**【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；**

```xml
<!-- 引入zookeeper -->
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-framework</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-recipes</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.zookeeper</groupId>
   <artifactId>zookeeper</artifactId>
   <version>3.4.14</version>
   <!--排除这个slf4j-log4j12-->
   <exclusions>
       <exclusion>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-log4j12</artifactId>
       </exclusion>
   </exclusions>
</dependency>
```

**2、在springboot配置文件中配置dubbo相关属性！**

```properties
server.port=8001

#当前应用名字
dubbo.application.name=provider-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#扫描指定包下服务
dubbo.scan.base-packages=nuc.ss.service
```

**3、在service的实现类中配置服务注解，发布服务！注意导包问题**

```java
package nuc.ss.service;

import org.apache.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;

@Service    //可以被扫描到，在项目一启动就自动注册到注册中心
@Component  //使用Dubbo后尽量不要用Service注解
public class TicketServiceImpl implements TicketService {
    @Override
    public String getTicket() {
        return "《狂神说Java》";
    }
}
```

**逻辑理解 ：应用启动起来，dubbo就会扫描指定的包下带有`@component`注解的服务，将它发布在指定的注册中心中！**

**4、运行测试**

![image-20200801204543375](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8969774122417431ba64628643e975f6.png)

![image-20200801205620564](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fce2c168367cccb5b134df0589635f38.png)

![image-20200801205708690](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5ea693118ba2da05c41262a053f89ea3.png)

### 19.7.3 服务消费者

**1、导入依赖，和之前的依赖一样；**

```xml
<!--dubbo-->
<!-- Dubbo Spring Boot Starter -->
<dependency>
   <groupId>org.apache.dubbo</groupId>
   <artifactId>dubbo-spring-boot-starter</artifactId>
   <version>2.7.3</version>
</dependency>
<!--zookeeper-->
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
<!-- 引入zookeeper -->
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-framework</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-recipes</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.zookeeper</groupId>
   <artifactId>zookeeper</artifactId>
   <version>3.4.14</version>
   <!--排除这个slf4j-log4j12-->
   <exclusions>
       <exclusion>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-log4j12</artifactId>
       </exclusion>
   </exclusions>
</dependency>
```

2、**配置参数**

```properties
server.port=8002

#当前应用名字
dubbo.application.name=consumer-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3. 本来正常步骤是需要将服务提供者的接口打包，然后用pom文件导入，我们这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；**

![image-20200801211028698](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3c4582f51e70a7ea2d422f2a4fe86955.png)

**4. 完善消费者的服务类**

```java
package nuc.ss.service;

import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

@Service //注入到容器中
public class UserService {

    // 想拿到provider-server提供的票，要去注册中心拿到服务
    @Reference //引用，Pom坐标，可以定义路径相同的接口名
            TicketService ticketService;

    public void bugTicket(){
        String ticket = ticketService.getTicket();
        System.out.println("在注册中心买到"+ticket);
    }

}
```

**5. 测试类编写；**

```java
@SpringBootTest
public class ConsumerServerApplicationTests {

   @Autowired
   UserService userService;

   @Test
   public void contextLoads() {

       userService.bugTicket();

  }

}
```

### 19.7.4 启动测试

**1. 开启zookeeper**

**2. 打开dubbo-admin实现监控**【可以不用做】

**3. 开启服务者**

**4. 消费者消费测试，结果**：

![image-20200801221551593](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/16160f428abf351d56d1ea6b56738114.png)

**监控中心**：

![image-20200801222026747](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f002c47c60ce97a4743413b5b621c6d7.png)

**这就是SpingBoot + dubbo + zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想**
