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

![image-20220330162157176](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330162157176.png)

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

![image-20220330160845131](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330160845131.png)

简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的微服务项目！



> 将项目打成jar包，点击 maven的 package

- 如果打包成功，则会在target目录下生成一个 jar 包

  ![image-20220330162331243](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330162331243.png)

- 打成了jar包后，就可以在任何地方运行了！OK

  ![image-20220330162928457](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330162928457.png)


- 浏览器运行结果：

![image-20220330162659667](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330162659667.png)





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

![](D:\Snipaste截图\Snipaste_2022-03-30_17-41-00.png)



我们在上面的自动配置类随便找一个打开看看，比如 ：`WebMvcAutoConfiguration`

![](D:\Snipaste截图\Snipaste_2022-03-30_17-42-42.png)

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

<img src="D:\Snipaste截图\2020033020092699.png"  />

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

   ![image-20220330203504607](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330203504607.png)

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

   ![image-20220330204838949](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330204838949.png)

   此时可以点击Open Decumentation进入官网

   ![image-20220330205007179](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330205007179.png)

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

![image-20220330211829040](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330211829040.png)

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

   ![image-20220330210523609](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330210523609.png)

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

![image-20220330213800143](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330213800143.png)

## 5.2 多环境切换

`profile`是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境

### 5.2.1 多配置文件

我们在主配置文件编写的时候，文件名可以是 `application-{profile}.properties/yml `, 用来指定多个环境版本；

**例如：**

`application-test.properties` 代表测试环境配置

`application-dev.properties` 代表开发环境配置

但是SpringBoot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

![1595484043622](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot04%EF%BC%9AJSR303%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%8F%8A%E5%A4%9A%E7%8E%AF%E5%A2%83%E5%88%87%E6%8D%A2.assets/1595484043622.png)

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

![image-20220330215019389](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330215019389.png)

springBoot 启动会扫描以下位置的`application.properties`或者`application.yml`文件作为SpringBoot的默认配置文件：

![image-20220330215151212](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220330215151212.png)

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

   - 利用EnableAutoConfigurationImportSelector给容器中导入一些组件

   - 可以查看selectImports()方法的内容,他返回了一个autoConfigurationEnty,来自`this.getAutoConfigurationEntry(autoConfigurationMetadata,annotationMetadata)`这个方法我们继续来跟踪：

   - 这个方法有一个值：`List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);`叫做获取候选的配置 ，我们点击继续跟踪

     - `SpringFactoriesLoader.loadFactoryNames()`
     - 扫描所有jar包类路径下`META-INF/spring.factories`
     - 把扫描到的这些文件的内容包装成properties对象
     - 从properties中获取到EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加在容器中

   - 在类路径下,`META-INF/spring.factories`里面配置的所有EnableAutoConfiguration的值加入到容器中：

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

- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；

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

![1595493884773](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot05%EF%BC%9A%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595493884773.png)

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
- **【演示：查看输出的日志】**

# 7. 自定义Starter

我们分析完毕了源码以及自动装配的过程，我们可以尝试自定义一个启动器！

## 7.1 说明

启动器模块是一个 空 jar 文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

**命名归约：**

官方命名：

- 前缀：spring-boot-starter-xxx
- 比如：spring-boot-starter-web....

自定义命名：

- xxx-spring-boot-starter
- 比如：mybatis-spring-boot-starter

## 7.2 编写启动器

1. 在IDEA中新建一个空项目 spring-boot-starter-diy

2. 新建一个普通Maven模块：kuang-spring-boot-starter

3. 新建一个Springboot模块：kuang-spring-boot-starter-autoconfigure

4. 点击apply即可，基本结构

5. 在我们的 starter 中 导入  autoconfigure 的依赖！

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

6. 将 autoconfigure 项目下多余的文件都删掉，Pom中只留下一个 starter，这是所有的启动器基本配置！

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

9. 编写我们的自动配置类并注入bean，测试

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



# 8. 整合JDBC

## 8.1 创建测试项目测试数据源

1. 我去新建一个项目测试：springboot-data-jdbc ; 引入相应的模块！基础模块

   ![1595741635349](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot07%EF%BC%9A%E6%95%B4%E5%90%88JDBC.assets/1595741635349.png)

2. 项目建好之后，发现自动帮我们导入了如下的启动器：

   ```
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

   ```
   spring:
     datasource:
       username: root
       password: admin
       #?serverTimezone=UTC解决时区的报错
       url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
   ```

4. 配置完这一些东西后，我们就可以直接去使用了，因为SpringBoot已经默认帮我们进行了自动配置；去测试类测试一下

   ```
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

结果：我们可以看到他默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置

我们来全局搜索一下，找到数据源的所有自动配置都在 ：DataSourceAutoConfiguration文件：

```
@Import(
    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
)
protected static class PooledDataSourceConfiguration {
    protected PooledDataSourceConfiguration() {
    }
}
```

这里导入的类都在 DataSourceConfiguration 配置类下，可以看出 Spring Boot 2.2.5  默认使用HikariDataSource 数据源，而以前版本，如 Spring Boot 1.5 默认使用  org.apache.tomcat.jdbc.pool.DataSource 作为数据源；

**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；**

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate

## 8.2 JDBCTemplate

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

## 8.3 测试

编写一个Controller，注入 jdbcTemplate，编写测试方法进行访问测试；

```
package nuc.ss.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
import java.util.Map;

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





# 9. 整合Druid

## 9.1 Druid简介

- Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。
- Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。
- Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。
- Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。
- Spring Boot 2.0 以上默认使用 Hikari 数据源，可以说 Hikari 与 Driud 都是当前 Java Web 上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成 Druid 数据源，如何实现数据库监控。
- Github地址：[https://github.com/alibaba/druid/](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Falibaba%2Fdruid%2F)

**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

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

## 9.2 配置数据源

1. 添加上 Druid 数据源依赖，这个依赖可以从Maven仓库官网[Maven Respository](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Fcom.alibaba%2Fdruid)中获取

   ```
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.23</version>
   </dependency>
   ```

   ![image-20200727215315060](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727215315060.png)

2. 切换数据源；之前已经说过 Spring Boot 2.0 以上默认使用 `com.zaxxer.hikari.HikariDataSource `数据源，但可以通过 `spring.datasource.type` 指定数据源。

   ```
   spring:
     datasource:
       username: root
       password: 123456
       url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
   ```

3. 数据源切换之后，在测试类中注入 DataSource，然后获取到它，输出一看便知是否成功切换；

   ![image-20200727222109497](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727222109497.png)

4. 切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

   ```
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
       #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
       #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
       filters: stat,wall,log4j
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

5. 导入Log4j 的依赖

   ```
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

6. 现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

   ```
   package nuc.ss.config;
   
   import com.alibaba.druid.pool.DruidDataSource;
   import org.springframework.boot.context.properties.ConfigurationProperties;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   import javax.sql.DataSource;
   
   @Configuration
   public class DruidConfig {
   
       /*
          将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
          绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
          @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
          前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
        */
       @ConfigurationProperties(prefix = "spring.datasource")
       @Bean
       public DataSource druidDataSource() {
           return new DruidDataSource();
       }
   
   }
   ```

7. 去测试类中测试一下；看是否成功！

   ```
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

   ![image-20200727233746228](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727233746228.png)

## 9.3 配置Druid数据源监控

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。

所以第一步需要设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；

```
//配置 Druid 监控管理后台的Servlet；
//内置 Servlet 容器时没有web.xml文件，所以使用 Spring Boot 的注册 Servlet 方式
@Bean
public ServletRegistrationBean statViewServlet() {
    ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

    // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet 
    // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
    Map<String, String> initParams = new HashMap<>();
    initParams.put("loginUsername", "root"); //后台管理界面的登录账号
    initParams.put("loginPassword", "admin"); //后台管理界面的登录密码

    //后台允许谁可以访问
    //initParams.put("allow", "localhost")：表示只有本机可以访问
    //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
    initParams.put("allow", "");
    //deny：Druid 后台拒绝谁访问
    //initParams.put("kuangshen", "192.168.1.20");表示禁止此ip访问

    //设置初始化参数
    bean.setInitParameters(initParams);
    return bean;
}
```

配置完毕后，我们可以选择访问 ：[http://localhost:8080/druid/login.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdruid%2Flogin.html)

![image-20200727233409312](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727233409312.png)

进入之后

![image-20200727233436583](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727233436583.png)

**配置 Druid web 监控 filter 过滤器**

```
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

















