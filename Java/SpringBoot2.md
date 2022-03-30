# 10. 整合MyBatis

官方文档：[http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/](https://gitee.com/link?target=http%3A%2F%2Fmybatis.org%2Fspring-boot-starter%2Fmybatis-spring-boot-autoconfigure%2F)

Maven仓库地址：[https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.3](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Forg.mybatis.spring.boot%2Fmybatis-spring-boot-starter%2F2.1.3)

![image-20200728083023851](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728083023851.png)

## 10.1 整合步骤

1. 导入 MyBatis 所需要的依赖

   ```
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.1.3</version>
   </dependency>
   ```

2. 配置数据库连接信息（不变）

   ```
   spring:
     datasource:
       username: root
       password: admin
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

4. **创建实体类，导入 Lombok！**

   `User.java`

   ```
   package nuc.ss.pojo;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

5. **创建mapper目录以及对应的 Mapper 接口**

   `UserMapper.java`

   ```
   package nuc.ss.mapper;
   
   import nuc.ss.pojo.User;
   import org.apache.ibatis.annotations.Mapper;
   import org.springframework.stereotype.Repository;
   
   import java.util.List;
   
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

6. **对应的Mapper映射文件**

   `UserMapper.xml`

   ```
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

7. **maven配置资源过滤问题**

   ```
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

8. **编写部门的 UserController 进行测试！**

   ```
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

**启动项目访问进行测试！**



# 11. Web开发静态资源处理

## 11.1 简介

其实SpringBoot的东西用起来非常简单，因为SpringBoot最大的特点就是自动装配。

**使用SpringBoot的步骤：**

1、创建一个SpringBoot应用，选择我们需要的模块，SpringBoot就会默认将我们的需要的模块自动配置好

2、手动在配置文件中配置部分配置项目就可以运行起来了

3、专注编写业务代码，不需要考虑以前那样一大堆的配置了。

要熟悉掌握开发，之前学习的自动配置的原理一定要搞明白！

比如SpringBoot到底帮我们配置了什么？我们能不能修改？我们能修改哪些配置？我们能不能扩展？

- 向容器中自动配置组件 ：*** Autoconfiguration
- 自动配置类，封装配置文件的内容：***Properties

没事就找找类，看看自动装配原理！

## 11.2 静态资源处理

### 11.2.1 静态资源映射规则

**首先，我们搭建一个普通的SpringBoot项目，回顾一下HelloWorld程序！**

写请求非常简单，那我们要引入我们前端资源，我们项目中有许多的静态资源，比如css，js等文件，这个SpringBoot怎么处理呢？

如果我们是一个web应用，我们的main下会有一个webapp，我们以前都是将所有的页面导在这里面的，对吧！但是我们现在的pom呢，打包方式是为jar的方式，那么这种方式SpringBoot能不能来给我们写页面呢？当然是可以的，但是SpringBoot对于静态资源放置的位置，是有规定的！

**我们先来聊聊这个静态资源映射规则：**

- SpringBoot中，SpringMVC的web配置都在 WebMvcAutoConfiguration 这个配置类里面；

- 我们可以去看看 WebMvcAutoConfigurationAdapter 中有很多配置方法；

- 有一个方法：addResourceHandlers 添加资源处理

  ```
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

### 11.2.2 什么是webjars

Webjars本质就是以jar包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。

### 11.2.3 第一种静态资源映射规则

使用SpringBoot需要使用Webjars，我们可以去搜索一下：

网站：[https://www.webjars.org](https://gitee.com/link?target=https%3A%2F%2Fwww.webjars.org)

要使用jQuery，我们只要要引入jQuery对应版本的pom依赖即可！

```
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

导入完毕，查看webjars目录结构，并访问Jquery.js文件！

![1595506633980](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595506633980.png)

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：[http://localhost:8080/webjars/jquery/3.4.1/jquery.js](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fwebjars%2Fjquery%2F3.4.1%2Fjquery.js)

![1595506019658](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595506019658.png)

### 11.2.4 第二种静态资源映射规则

1、那我们项目中要是使用自己的静态资源该怎么导入呢？我们看下一行代码；

![1595516976999](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595516976999.png)

2、我们去找`staticPathPattern`发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 `resourceProperties` 这个类，我们可以点进去看一下分析：

```
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

```
"classpath:/META-INF/resources/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
```

5、我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件；

![1595517831392](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595517831392.png)

6、比如我们访问 [http://localhost:8080/1.js](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2F1.js) , 他就会去这些文件夹中寻找对应的静态资源文件；

![1595517869049](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595517869049.png)

### 11.2.5 自定义静态资源路径

我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在application.properties中配置；

```
spring.resources.static-locations=classpath:/coding/,classpath:/ss/
```

![1595518276475](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595518276475.png)

### 11.2.6 总结

1. 在springboot，我们可以使用一下方式处理静态资源
   - webjars `localhost:8080/webjars/`
   - public，static，/**，resources  `localhost:8080/`
2. 优先级：resources > static（默认） > public

## 11.3 首页处理

静态资源文件夹说完后，我们继续向下看源码！可以看到一个欢迎页的映射，就是我们的首页！

```
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

```
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

截图说明

![1595550098734](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595550098734.png)

- 欢迎页，静态资源文件夹下的所有 index.html 页面；被 /** 映射。

- 比如我访问  [http://localhost:8080/](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2F) ，就会找静态资源文件夹下的 index.html

- 新建一个 index.html ，在我们上面的3个目录中任意一个；然后访问测试  [http://localhost:8080/](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2F)  看结果！

  ![1595550394178](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595550394178.png)

1、**关于网站图标说明**：

**欢迎页面(Welcome Page)**

> Spring Boot supports both static and templated welcome pages. It first looks for an `index.html` file in the configured static content locations. If one is not found, it then looks for an `index template`. If either is found, it is automatically used as the welcome page of the application.

**自定义应用图标（Custom Facicon）**

> Spring Boot looks for a `favicon.ico` in the configured  static content locations and the root of the classpath (in that order).  If such a file is present, it is automatically used as the favicon of  the application.

2、首页图标

**2.2.x之前的版本（如2.1.7）springboot是这样**

与其他静态资源一样，Spring Boot在配置的静态内容位置中查找 favicon.ico。如果存在这样的文件，它将自动用作应用程序的favicon。

1. 关闭SpringBoot默认图标

   ```
   #关闭默认图标
   spring.mvc.favicon.enabled=false
   ```

2. 自己放一个图标在静态资源目录下，我放在 public 目录下

   ![1595554357772](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595554357772.png)

3. 清除浏览器缓存`Ctrl + F5`！刷新网页，发现图标已经变成自己的了！

**2.2.x之后的版本（如2.3.0）直接执行2和3就可以了**

![1595554118099](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot10%EF%BC%9AWeb%E5%BC%80%E5%8F%91%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E5%A4%84%E7%90%86.assets/1595554118099.png)



# 12. Thymeleaf模板引擎

## 12.1 模板引擎

- 前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。
- jsp支持非常强大的功能，包括能写Java代码，但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，**他现在默认是不支持jsp的**。
- 那不支持jsp，如果我们直接用纯静态页面的方式，那给我们开发会带来非常大的麻烦，那怎么办呢？

**SpringBoot推荐你可以来使用模板引擎：**

模板引擎，我们其实大家听到很多，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot给我们推荐的Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢我们来看一下这张图：

![1595555521951](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot11%EF%BC%9AThymeleaf%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E.assets/1595555521951.png)

模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。

我们呢，就来看一下这个模板引擎，那既然要看这个模板引擎。首先，我们来看SpringBoot里边怎么用。

## 12.2 引入Thymeleaf

怎么引入呢，对于springboot来说，什么事情不都是一个start的事情嘛，我们去在项目中引入一下。给大家三个网址：

- Thymeleaf 官网：[https://www.thymeleaf.org/](https://gitee.com/link?target=https%3A%2F%2Fwww.thymeleaf.org%2F)
- Thymeleaf 在Github 的主页：[https://github.com/thymeleaf/thymeleaf](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fthymeleaf%2Fthymeleaf)
- Spring官方文档：找到我们对应的版本[https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter ](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2F2.2.5.RELEASE%2Freference%2Fhtmlsingle%2F%23using-boot-starter)

找到对应的pom依赖：可以适当点进源码看下本来的包！

```
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Maven会自动下载jar包，我们可以去看下下载的东西；

## 12.3 Thymeleaf分析

前面呢，我们已经引入了Thymeleaf，那这个要怎么使用呢？

我们首先得按照SpringBoot的自动配置原理看一下我们这个Thymeleaf的自动配置规则，在按照那个规则，我们进行使用。

我们去找一下Thymeleaf的自动配置类：`ThymeleafProperties`

```
@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
    private Charset encoding;
}
```

我们可以在其中看到默认的前缀和后缀！

我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。

使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

**测试**

1. 编写一个TestController

   ```
   @Controller
   public class TestController {
       
       @RequestMapping("/test")
       public String test1(){
           //classpath:/templates/test.html
           return "test";
       }
       
   }
   ```

2. 编写一个测试页面  test.html 放在 templates 目录下

   ```
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>Test页面</h1>
   </body>
   </html>
   ```

3. 启动项目请求测试

![1595557596160](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot11%EF%BC%9AThymeleaf%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E.assets/1595557596160.png)

## 12.4 Thymeleaf 语法学习

要学习语法，还是参考官网文档最为准确，我们找到对应的版本看一下；

Thymeleaf 官网：[https://www.thymeleaf.org/](https://gitee.com/link?target=https%3A%2F%2Fwww.thymeleaf.org%2F) ， 简单看一下官网！我们去下载Thymeleaf的官方文档！在线文档：[https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html](https://gitee.com/link?target=https%3A%2F%2Fwww.thymeleaf.org%2Fdoc%2Ftutorials%2F3.0%2Fusingthymeleaf.html)

### 12.4.1 Thymeleaf入门

**我们做个最简单的练习 ：我们需要查出一些数据，在页面中展示**

1. 修改测试请求，增加数据传输；

   ```
   @RequestMapping("/t1")
   public String test1(Model model){
       //存入数据
       model.addAttribute("msg","Hello,Thymeleaf");
       //classpath:/templates/test.html
       return "test";
   }
   ```

2. 我们要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示。

   我们可以去官方文档的#3中看一下命名空间拿来过来：

   ```
    xmlns:th="http://www.thymeleaf.org"
   ```

3. 我们去编写下前端页面

   ```
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>狂神说</title>
   </head>
   <body>
   <h1>测试页面</h1>
   
   <!--th:text就是将div中的内容设置为它指定的值，和之前学习的Vue一样-->
   <div th:text="${msg}"></div>
   </body>
   </html>
   ```

4. 启动测试！

   ![1595558424003](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot11%EF%BC%9AThymeleaf%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E.assets/1595558424003.png)

**OK，入门搞定，我们来认真研习一下Thymeleaf的使用语法！**

### 12.4.2 Thymeleaf语法

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

```
@RequestMapping("/test2")
public String test2(Map<String,Object> map){
    //存入数据
    map.put("msg","<h1>Hello</h1>");
    map.put("users", Arrays.asList("qinjiang","kuangshen"));
    //classpath:/templates/test.html
    return "test";
}
```

2、测试页面取出数据

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div>
    <h1>Test页面</h1>
    <!--不转义-->
    <div th:text="${msg}"></div>
    <!--转义-->
    <div th:utext="${msg}"></div>

    <hr>
	<!--遍历数据-->
	<!--th:each每次遍历都会生成当前这个标签：官网#9-->
    <h3 th:each="user:${users}" th:text="${user}"></h3>
    <hr>
     <!--行内写法：官网#12-->
    <h3 th:each="user:${users}">[[ ${user} ]]</h3>
</div>
</body>
</html>
```

3、启动项目测试！

![1595560097876](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot11%EF%BC%9AThymeleaf%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E.assets/1595560097876.png)

**我们看完语法，很多样式，我们即使现在学习了，也会忘记，所以我们在学习过程中，需要使用什么，根据官方文档来查询，才是最重要的，要熟练使用官方文档！**





# 13. MVC自动配置原理

## 13.1 官网阅读

在进行项目编写前，我们还需要知道一个东西，就是SpringBoot对我们的SpringMVC还做了哪些配置，包括如何扩展，如何定制。

只有把这些都搞清楚了，我们在之后使用才会更加得心应手。途径一：源码分析，途径二：官方文档！

地址 ：[https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2F2.2.5.RELEASE%2Freference%2Fhtmlsingle%2F%23boot-features-spring-mvc-auto-configuration)

```
Spring MVC Auto-configuration
// Spring Boot为Spring MVC提供了自动配置，它可以很好地与大多数应用程序一起工作。
Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
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
如果您希望保留Spring Boot MVC功能，并且希望添加其他MVC配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己
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

## 13.2 内容协商视图解析器

**ContentNegotiatingViewResolver**

- 自动配置了ViewResolver，就是我们之前学习的SpringMVC的视图解析器；

- 即根据方法的返回值取得视图对象（View），然后由视图对象决定如何渲染（转发，重定向）。

- 我们去看看这里的源码：我们找到 `WebMvcAutoConfiguration` ， 然后搜索`ContentNegotiatingViewResolver`。找到如下方法！

  ```
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

  ```
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

  ```
  Iterator var5 = this.viewResolvers.iterator();
  ```

  所以得出结论：**ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的**

- 我们再去研究下他的组合逻辑，看到有个属性viewResolvers，看看它是在哪里进行赋值的！

  ```
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

**自定义视图解析器**

我们可以自己给容器中去添加一个视图解析器；这个类就会帮我们自动的将它组合进来；**我们去实现一下**

1. 我们在我们的主程序中去写一个视图解析器来试试；

   ```
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

   ![1595564720595](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot12%EF%BC%9AMVC%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595564720595.png)

3. 我们启动我们的项目，然后随便访问一个页面，看一下Debug信息；

   找到this(就是DispatcherServlet)

   ![1595564823239](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot12%EF%BC%9AMVC%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595564823239.png)

   找到视图解析器（viewResolvers），我们看到我们自己定义的就在这里了；

   ![1595564942873](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot12%EF%BC%9AMVC%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595564942873.png)

- 所以说，我们如果想要使用自己定制化的东西，我们只需要给容器中添加这个组件就好了！剩下的事情SpringBoot就会帮我们做了！

## 13.3 转换器和格式化器

- 在`WebMvcAutoConfiguration`中找到格式化转换器：

  ```
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

    ```
    public String getDateFormat() {
        return this.dateFormat;
    }
    
    /**
    * Date format to use. For instance, `dd/MM/yyyy`. 默认的
     */
    private String dateFormat;
    ```

  - 2.2.x之后的版本

  ```
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

    配置文件

    ```
    # 配置文件
    spring.nvc.date-format=
    ```

    源码

    ```
    @Deprecated
    public void setDateFormat(String dateFormat) {
        this.dateFormat = dateFormat;
    }
    ```

  - 2.2.x版本之后的

    配置文件

    ```
    spring.nvc.date=
    ```

    源码

    ```
    @Deprecated
    public void setDateFormat(String dateFormat) {
        this.format.setDate(dateFormat);
    }
    
    public void setDate(String date) {
        this.date = date;
    }
    ```

其余的就不一一举例了，大家可以下去多研究探讨即可！

## 13.4 修改SpringBoot的默认配置

- 这么多的自动配置，原理都是一样的，通过这个WebMVC的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。

- SpringBoot的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；

- SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；

- 如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！

- **扩展使用SpringMVC**  官方文档如下：

  If you want to keep Spring Boot MVC features and you want to add  additional MVC configuration (interceptors, formatters, view  controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` **but without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

- 我们要做的就是编写一个`@Configuration`注解类，并且类型要为`WebMvcConfigurer`，还不能标注`@EnableWebMvc`注解；我们去自己写一个；

- 我们新建一个包叫config，写一个类MyMvcConfig；

  ```
  // 如果我们要扩展springmvc，官方建议我们这样去做@Configuration
  //应为类型要求为WebMvcConfigurer，所以我们实现其接口
  //扩展 springmvc      DispatchServlet
  //@EnableWebMvc //这玩意就是导入了一个类，DelegatingWebMvcConfiguration，从容器中获取所有的webMvcConfig
  @Configuration
  public class MyMvcConfig implements WebMvcConfigurer {
  
      @Override
      public void addViewControllers(ViewControllerRegistry registry) {
          // 浏览器发送/test ， 就会跳转到test页面；
          registry.addViewController("/test2").setViewName("test");
      }
  }
  ```

- 我们去浏览器访问一下：

  ![1595584684727](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot12%EF%BC%9AMVC%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595584684727.png)

**确实也跳转过来了！所以说，我们要扩展SpringMVC，官方就推荐我们这么去使用，既保SpringBoot留所有的自动配置，也能用我们扩展的配置！**

我们可以去分析一下原理：

1. `WebMvcAutoConfiguration` 是 SpringMVC的自动配置类，里面有一个类`WebMvcAutoConfigurationAdapter`

2. 这个类上有一个注解，在做其他自动配置时会导入：`@Import(EnableWebMvcConfiguration.class)`

3. 我们点进`EnableWebMvcConfiguration`这个类看一下，它继承了一个父类：`DelegatingWebMvcConfiguration`

   这个父类中有这样一段代码：

   ```
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

   ```
   protected void addViewControllers(ViewControllerRegistry registry) {
       this.configurers.addViewControllers(registry);
   }
   ```

5. 我们点进去看一下

   ```
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

## 13.5 全面接管SpringMVC

- 官方文档：

  ```
  If you want to take complete control of Spring MVC
  you can add your own @Configuration annotated with @EnableWebMvc.
  ```

- 全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

- 只需在我们的配置类中要加一个`@EnableWebMvc`。

- 我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

- 不加注解之前，访问首页：

  ![1595587645715](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot12%EF%BC%9AMVC%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595587645715.png)

- 给配置类加上注解：`@EnableWebMvc`

  我们发现所有的SpringMVC自动配置都失效了！回归到了最初的样子；

  ![1595587533039](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot12%EF%BC%9AMVC%E8%87%AA%E5%8A%A8%E9%85%8D%E7%BD%AE%E5%8E%9F%E7%90%86.assets/1595587533039.png)

**当然，我们开发中，不推荐使用全面接管SpringMVC**

思考问题？为什么加了一个注解，自动配置就失效了！我们看下源码：

1. 这里发现它是导入了一个类，我们可以继续进去看

   ```
   @Import({DelegatingWebMvcConfiguration.class})
   public @interface EnableWebMvc {
   }
   ```

2. 它继承了一个父类 WebMvcConfigurationSupport

   ```
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
     // ......
   }
   ```

3. 我们来回顾一下Webmvc自动配置类

   ```
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



# 14. 页面国际化

有的时候，我们的网站会去涉及中英文甚至多语言的切换，这时候我们就需要学习国际化了！

## 14.1 准备工作

先在IDEA中统一设置properties的编码问题！

![1595594403624](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595594403624.png)

编写国际化配置文件，抽取页面需要显示的国际化页面消息。我们可以去登录页面查看一下，哪些内容我们需要编写国际化的配置！

## 14.2 配置文件编写

1. 我们在resources资源文件下新建一个i18n（internationalization缩写）目录，存放国际化配置文件

2. 建立一个`login.properties`文件，还有一个`login_zh_CN.properties`；发现IDEA自动识别了我们要做国际化操作；文件夹变了！

   ![1595604595071](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595604595071.png)

3. 我们可以在这上面去新建一个文件；

   ![1595604664699](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595604664699.png)

   弹出如下页面：我们再添加一个英文的；

   ![1595605422294](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595605422294.png)

   这样就快捷多了！

   ![1595605442883](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595605442883.png)

4. **接下来，我们就来编写配置，我们可以看到idea下面有另外一个视图；**

   ![1595605469344](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595605469344.png)

   这个视图我们点击 + 号就可以直接添加属性了；我们新建一个login.tip，可以看到边上有三个文件框可以输入

   ![1595605530046](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595605530046.png)

   然后依次添加其他页面内容即可！

   ![1595605552329](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595605552329.png)

   然后去查看我们的配置文件；

   login.properties ：

   默认

   ```
   login.btn=登录
   login.password=密码
   login.remember=记住我
   login.tip=请登录
   login.username=用户名
   ```

   英文：

   ```
   login.btn=Sign in
   login.password=Password
   login.remember=Remember me
   login.tip=Please sign in
   login.username=Username
   ```

   中文：

   ```
   login.btn=登录
   login.password=密码
   login.remember=记住我
   login.tip=请登录
   login.username=用户名
   ```

   OK，配置文件步骤搞定！

## 14.3 配置文件生效探究

我们去看一下SpringBoot对国际化的自动配置！这里又涉及到一个类：`MessageSourceAutoConfiguration`

里面有一个方法，这里发现SpringBoot已经自动配置好了管理我们国际化资源文件的组件 `ResourceBundleMessageSource`；

```
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

我们真实 的情况是放在了i18n目录下，所以我们要去配置这个messages的路径；

```
spring.messages.basename=i18n.login
```

## 14.4 配置页面国际化值

去页面获取国际化的值，查看Thymeleaf的文档，找到message取值操作为：#{...}。我们去页面测试下：

IDEA还有提示，非常智能的！

![1595606566975](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595606566975.png)

我们可以去启动项目，访问一下，发现已经自动识别为中文的了！

![1595606666498](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595606666498.png)

**但是我们想要更好！可以根据按钮自动切换中文英文！**

## 14.5 配置国际化解析

在Spring中有一个国际化的Locale （区域信息对象）；里面有一个叫做`LocaleResolver `（获取区域信息对象）的解析器！

我们去我们webmvc自动配置文件，寻找一下！看到SpringBoot默认配置：

```
@Bean
@ConditionalOnMissingBean
@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
public LocaleResolver localeResolver() {
    // 容器中没有就自己配，有的话就用用户配置的
    if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
        return new FixedLocaleResolver(this.mvcProperties.getLocale());
    }
    // 接收头国际化分解
    AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
    localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
    return localeResolver;
}
```

`AcceptHeaderLocaleResolver` 这个类中有一个方法

```
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

那假如我们现在想点击链接让我们的国际化资源生效，就需要让我们自己的Locale生效！

我们去自己写一个自己的`LocaleResolver`，可以在链接上携带区域信息！

修改一下前端页面的跳转连接：

```
<!-- 这里传入参数不需要使用 ？使用 （key=value）-->
<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
```

我们去写一个处理的组件类！

```
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

为了让我们的区域化信息能够生效，我们需要再配置一下这个组件！在我们自己的`MvcConofig`下添加`bean`；

```
@Bean
public LocaleResolver localeResolver(){
    return new MyLocaleResolver();
}
```

**我们重启项目，来访问一下，发现点击按钮可以实现成功切换！搞定收工！**

![1595666066757](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot13%EF%BC%9A%E9%A1%B5%E9%9D%A2%E5%9B%BD%E9%99%85%E5%8C%96.assets/1595666066757.png)

## 14.6 小结

1. 首页配置：
   - 注意点，所有页面的静态资源都需要使用thymeleaf接管
   - url:@{}
2. 页面国际化
   - 我们需要配置i18n文件
   - 我们如果需要在项目中进行按钮自动切换，我们需要定义一个组件`LocalResolver`
   - 记得将自己写的组件配置到spring容器`@Bean`
   - \#{}





# 15. Swagger

- 了解Swagger的概念及作用
- 了解前后端分离
- 在springboot中集成swagger

## 15.1 Swagger简介

- 号称世界上最流行的API框架
- Restful Api 文档在线自动生成器 => **API 文档 与API 定义同步更新**
- 直接运行，在线测试API
- 支持多种语言 （如：Java，PHP等）
- 官网：[https://swagger.io/](https://gitee.com/link?target=https%3A%2F%2Fswagger.io%2F)



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

- 前后端集成联调，前端或者后端无法做到“及时协商，尽早解决”，最终导致问题集中爆发

**解决方案**

- 首先定义schema [ 计划的提纲 ]，并实时跟踪最新的API，降低集成风险；
- 早些年：指定word计划文档；
- 前后端分离：
  - 前端测试后端接口：postman
  - 后端提供接口，需要实时更新最新的消息及改动

## 15.2 SpringBoot集成Swagger

**SpringBoot集成Swagger** => **springfox**，两个jar包

- **[Springfox-swagger2](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Fio.springfox%2Fspringfox-swagger2)**
- [springfox-swagger-ui](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Fio.springfox%2Fspringfox-swagger-ui)

**使用Swagger**

要求：jdk 1.8 + 否则swagger2无法运行

步骤：

1. 新建一个SpringBoot-web项目

2. 添加Maven依赖（注意：2.9.2版本之前，之后的不行）

   ```
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

4. 要使用Swagger，我们需要编写一个配置类-SwaggerConfig来配置 Swagger

   ```
   @Configuration //配置类
   @EnableSwagger2// 开启Swagger2的自动配置
   public class SwaggerConfig {  
   }
   ```

5. 访问测试 ：[http://localhost:8080/swagger-ui.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fswagger-ui.html) ，可以看到swagger的界面；

   ![image-20200731132229265](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731132229265.png)

## 15.3 配置Swagger

1. Swagger实例Bean是Docket，所以通过配置Docket实例来配置Swaggger。

   ```
   @Bean //配置docket以配置Swagger具体参数
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2);
   }
   ```

2. 可以通过apiInfo()属性配置文档信息

   ```
   //配置文档信息
   private ApiInfo apiInfo() {
      Contact contact = new Contact("联系人名字", "http://xxx.xxx.com/联系人访问链接", "联系人邮箱");
      return new ApiInfo(
              "Swagger学习", // 标题
              "学习演示如何配置Swagger", // 描述
              "v1.0", // 版本
              "http://terms.service.url/组织链接", // 组织链接
              contact, // 联系人信息
              "Apach 2.0 许可", // 许可
              "许可链接", // 许可连接
              new ArrayList<>()// 扩展
     );
   }
   ```

3. Docket 实例关联上 apiInfo()

   ```
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
   }
   ```

4. 重启项目，访问测试 [http://localhost:8080/swagger-ui.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fswagger-ui.html)  看下效果；

   ![image-20200731161851136](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731161851136.png)

## 15.4 配置扫描接口

1. 构建Docket时通过select()方法配置怎么扫描接口。

   ```
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2)
         .apiInfo(apiInfo())
         .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
         .apis(RequestHandlerSelectors.basePackage("nuc.ss.swagger.controller"))
         .build();
   }
   ```

2. 重启项目测试，由于我们配置根据包的路径扫描接口，所以我们只能看到一个类

   ![image-20200731165837391](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731165837391.png)

3. 除了通过包路径配置扫描接口外，还可以通过配置其他方式扫描接口，这里注释一下所有的配置方式：

   ```
   basePackage(final String basePackage) // 根据包路径扫描接口
   any() // 扫描所有，项目中的所有接口都会被扫描到
   none() // 不扫描接口
   // 通过方法上的注解扫描，如withMethodAnnotation(GetMapping.class)只扫描get请求
   withMethodAnnotation(final Class<? extends Annotation> annotation)
   // 通过类上的注解扫描，如.withClassAnnotation(Controller.class)只扫描有controller注解的类中的接口
   withClassAnnotation(final Class<? extends Annotation> annotation)
   ```

4. 除此之外，我们还可以配置接口扫描过滤：

   ```
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2)
         .apiInfo(apiInfo())
         .select()
         .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
          // 配置如何通过path过滤,即这里只扫描请求以/ss开头的接口
         .paths(PathSelectors.ant("/ss/**"))
         .build();
   }
   ```

5. 这里的可选值还有

   ```
   any() // 任何请求都扫描
   none() // 任何请求都不扫描
   regex(final String pathRegex) // 通过正则表达式控制
   ant(final String antPattern) // 通过ant()控制
   ```

## 15.5 配置Swagger开关

1. 通过enable()方法配置是否启用swagger，如果是false，swagger将不能在浏览器中访问了

   ```
   @Bean
   public Docket docket() {
      return new Docket(DocumentationType.SWAGGER_2)
         .apiInfo(apiInfo())
         .enable(false) //配置是否启用Swagger，如果是false，在浏览器将无法访问
         .select()
         .apis(RequestHandlerSelectors.basePackage("nuc.ss.swagger.controller"))
         .paths(PathSelectors.ant("/ss/**"))
         .build();
   }
   ```

![image-20200731190614381](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731190614381.png)

1. 如何动态配置当项目处于test、dev环境时显示swagger，处于prod时不显示？

   ```
   @Bean
   public Docket docket(Environment environment) {
      // 设置要显示swagger的环境
      Profiles of = Profiles.of("dev", "test");
      // 判断当前是否处于该环境
      // 通过 enable() 接收此参数判断是否要显示
      boolean b = environment.acceptsProfiles(of);
      
      return new Docket(DocumentationType.SWAGGER_2)
         .apiInfo(apiInfo())
         .enable(b)
         .select()
         .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
         .paths(PathSelectors.ant("/ss/**"))
         .build();
   }
   ```

2. 可以在项目中增加配置文件

   - dev测试环境

     ```
     server.port=8081
     ```

     ![image-20200731193109826](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731193109826.png)

     项目运行结果

     ![image-20200731193425090](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731193425090.png)

   - pro测试环境

     ```
     server.port=8082
     ```

     ![image-20200731194455510](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731194455510.png)

     项目运行结果

     ![image-20200731194559290](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731194559290.png)

## 15.6 配置API分组

1. 如果没有配置分组，默认是default。通过groupName()方法即可配置分组：

   ```
   @Bean
   public Docket docket(Environment environment) {
      return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
         .groupName("狂神") // 配置分组
          // 省略配置....
   }
   ```

2. 重启项目查看分组

   ![image-20200731195354714](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731195354714.png)

3. 如何配置多个分组？配置多个分组只需要配置多个docket即可：

   ```
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

   ![image-20200731195543102](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731195543102.png)

## 15.7 实体配置

1. 新建一个实体类

   ```
   //@Api("注释")
   @ApiModel("用户实体")
   public class User {
       @ApiModelProperty("用户名")
       private String username;
       @ApiModelProperty("密码")
       private String password;
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   
       public String getPassword() {
           return password;
       }
   
       public void setPassword(String password) {
           this.password = password;
       }
   }
   ```

2. 只要这个实体在**请求接口**的返回值上（即使是泛型），都能映射到实体项中：

   ```
   @RestController
   public class HelloController {
   
       //   /error默认错误请求
       @GetMapping("/hello")
       public String hello() {
           return "hello";
       }
   
       //只要我们的接口中，返回值中存在实体类，他就会被扫描到Swagger中
       @PostMapping("/user")
       public User user() {
           return new User();
       }
   }
   ```

3. 重启查看测试

   ![image-20200731200413725](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731200413725.png)

注：并不是因为@ApiModel这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而@ApiModel和@ApiModelProperty这两个注解只是为实体添加注释的。

- @ApiModel为类添加注释
- @ApiModelProperty为类属性添加注释

**总结：**

- 我们可以通过Swagger给一些比较难理解的接口或者属性，增加注释信息
- 接口文档实时更新
- 可以在线测试

Swagger是一个优秀的工具，几乎所有大公司都有使用它

【注意点】：在正式发布的时候，关闭Swagger！！！

- 出于安全考虑
- 而且节省内存

## 15.8 常用注解

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

   ```
   @RestController
   public class HelloController {
       ......
       @ApiOperation("Hello控制接口")
       @GetMapping("/hello")
       public String hello2(@ApiParam("用户名") String username) {
           return "hello" + username;
       }
       
       @ApiOperation("get测试")
       @GetMapping("/get")
       public User hello2(@ApiParam("用户") User user) {
           return user;
       }
   }
   ```

   ![image-20200731201755001](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731201755001.png)

2. 进行try it out测试

   ![image-20200731202958255](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731202958255.png)

   测试结果

   ![image-20200731203034702](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731203034702.png)

**总结：**

1. 这样的话，可以给一些比较难理解的属性或者接口，增加一些配置信息，让人更容易阅读！
2. 相较于传统的Postman或Curl方式测试接口，使用swagger简直就是傻瓜式操作，不需要额外说明文档(写得好本身就是文档)而且更不容易出错，只需要录入数据然后点击Execute，如果再配合自动化框架，可以说基本就不需要人为操作了。
3. Swagger是个优秀的工具，现在国内已经有很多的中小型互联网公司都在使用它，相较于传统的要先出Word接口文档再测试的方式，显然这样也更符合现在的快速迭代开发行情。当然了，提醒下大家在正式环境要记得关闭Swagger，一来出于安全考虑二来也可以节省运行时内存。

## 15.9 拓展：其他皮肤

我们可以导入不同的包实现不同的皮肤定义：

1、默认的  **访问 [http://localhost:8080/swagger-ui.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fswagger-ui.html)**

```
<dependency> 
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>2.9.2</version>
</dependency>
```

![image-20200731204929854](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731204929854.png)

2、bootstrap-ui  **访问 [http://localhost:8080/doc.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdoc.html)**

```
<!-- 引入swagger-bootstrap-ui包 /doc.html-->
<dependency>
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>swagger-bootstrap-ui</artifactId>
   <version>1.9.1</version>
</dependency>
```

![image-20200731205550845](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731205550845.png)

3、Layui-ui  **访问 [http://localhost:8080/docs.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdocs.html)**

```
<!-- 引入swagger-ui-layer包 /docs.html-->
<dependency>
   <groupId>com.github.caspar-chen</groupId>
   <artifactId>swagger-ui-layer</artifactId>
   <version>1.1.3</version>
</dependency>
```

- 我这个测试没成功(Layui-ui)

4、mg-ui  **访问 [http://localhost:8080/document.html](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fdocument.html)**

```
<!-- 引入swagger-ui-layer包 /document.html-->
<dependency>
   <groupId>com.zyplayer</groupId>
   <artifactId>swagger-mg-ui</artifactId>
   <version>1.0.6</version>
</dependency>
```

![image-20200731205723914](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200731205723914.png)





# 16. 异步、定时、邮件任务

## 16.1 异步任务

1. 创建一个service包

2. 创建一个类AsyncService

   异步处理还是非常常用的，比如我们在网站上发送邮件，后台会去发送邮件，此时前台会造成响应不动，直到邮件发送完毕，响应才会成功，所以我们一般会采用多线程的方式去处理这些任务。

   编写方法，假装正在处理数据，使用线程设置一些延时，模拟同步等待的情况；

   ```
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

3. 编写controller包

4. 编写AsyncController类

   我们去写一个Controller测试一下

   ```
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

5. 访问[http://localhost:8080/hello进行测试，3秒后出现OK，这是同步等待的情况。](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A8080%2Fhello%E8%BF%9B%E8%A1%8C%E6%B5%8B%E8%AF%95%EF%BC%8C3%E7%A7%92%E5%90%8E%E5%87%BA%E7%8E%B0OK%EF%BC%8C%E8%BF%99%E6%98%AF%E5%90%8C%E6%AD%A5%E7%AD%89%E5%BE%85%E7%9A%84%E6%83%85%E5%86%B5%E3%80%82)

   问题：我们如果想让用户直接得到消息，就在后台使用多线程的方式进行处理即可，但是每次都需要自己手动去编写多线程的实现的话，太麻烦了，我们只需要用一个简单的办法，在我们的方法上加一个简单的注解即可，如下：

6. 给hello方法添加@Async注解；

   ```
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

   ```
   @EnableAsync //开启异步注解功能
   @SpringBootApplication
   public class SpringbootTaskApplication {
   
      public static void main(String[] args) {
          SpringApplication.run(SpringbootTaskApplication.class, args);
     }
   
   }
   ```

7、重启测试，网页瞬间响应，后台代码依旧执行！

## 16.2 邮件任务

邮件发送，在我们的日常开发中，也非常的多，Springboot也帮我们做了支持

- 邮件发送需要引入spring-boot-start-mail
- SpringBoot 自动配置MailSenderAutoConfiguration
- 定义MailProperties内容，配置在application.yml中
- 自动装配JavaMailSender
- 测试邮件发送

**测试：**

1. 引入pom依赖

   ```
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail</artifactId>
   </dependency>
   ```

   看它引入的依赖，可以看到 jakarta.mail

   ```
   <dependency>
      <groupId>com.sun.mail</groupId>
      <artifactId>jakarta.mail</artifactId>
      <scope>compile</scope>
   </dependency>
   ```

2. 查看自动配置类：MailSenderAutoConfiguration

   ![image-20200801104504309](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801104504309.png)

   这个类中存在bean，JavaMailSenderImpl

   ![img](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801104504309.png)

   然后我们去看下配置文件

   ```
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

3. 配置文件：

   ```
   spring.mail.username=1710841251@qq.com
   spring.mail.password=你的qq授权码
   spring.mail.host=smtp.qq.com
   # qq需要配置ssl
   spring.mail.properties.mail.smtp.ssl.enable=true
   ```

   获取授权码：在QQ邮箱中的设置->账户->开启pop3和smtp服务

   ![image-20200801105503766](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801105503766.png)

4. Spring单元测试

   ```
   @Autowired
   JavaMailSenderImpl javaMailSender;
   @Test//邮件设置1：一个简单的邮件
   void contextLoads() {
       SimpleMailMessage mailMessage = new SimpleMailMessage();
       mailMessage.setSubject("狂神，你好");
       mailMessage.setText("谢谢你的狂神说Java系列课程");
   
       mailMessage.setTo("24736743@qq.com");
       mailMessage.setFrom("1710841251@qq.com");
       javaMailSender.send(mailMessage);
   }
   
   @Test// 一个复杂的邮件
   void contextLoads2() throws MessagingException {
       MimeMessage mimeMessage = javaMailSender.createMimeMessage();
       //组装
       MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);
   
       //正文
       helper.setSubject("狂神，你好~plus");
       helper.setText("<p style='color:red'>谢谢你的狂神说Java系列课程</p>", true);
   
       //附件
       helper.addAttachment("1.jpg", new File(""));
       helper.addAttachment("2.jpg", new File(""));
   
       helper.setTo("24736743@qq.com");
       helper.setFrom("1710841251@qq.com");
   
       javaMailSender.send(mimeMessage);
   
   }
   ```

   ![image-20200801112646650](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801112646650.png)

查看邮箱，邮件接收成功！

我们只需要使用Thymeleaf进行前后端结合即可开发自己网站邮件收发功能了！

## 16.3 定时任务

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，Spring为我们提供了异步执行任务调度的方式，提供了两个接口。

- TaskExecutor接口（任务执行者）
- TaskScheduler接口（任务调度者）

两个注解：

- @EnableScheduling——开启定时功能的注解
- @Scheduled——什么时候执行

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

1、创建一个ScheduledService

我们里面存在一个hello方法，他需要定时执行，怎么处理呢？

```
@Service
public class ScheduledService {

    // 在一个特定的时间执行这个方法——Timer
    //cron表达式
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

```
@EnableAsync //开启异步注解功能
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {

   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }

}
```

3、我们来详细了解下cron表达式；

[http://www.bejson.com/othertools/cron/](https://gitee.com/link?target=http%3A%2F%2Fwww.bejson.com%2Fothertools%2Fcron%2F)

4、常用的表达式

```
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





# 17. Dubbo和Zookeeper集成

## 17.1 分布式理论

### 17.1.1 什么是分布式系统？

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。

### 17.1.2 Dubbo文档

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图

![img](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/dubbo-architecture-roadmap.jpg)

### 17.1.3 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

![image-20200801133522303](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133522303.png)

适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

**缺点：**

1、性能扩展比较难

2、协同开发问题

3、不利于升级维护

### 17.1.4 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

![image-20200801133625663](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133625663.png)

通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

缺点：公用模块无法重复利用，开发性的浪费

### 17.1.5 分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服务框架(RPC)**是关键。

![image-20200801133710784](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133710784.png)

### 17.1.6 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心**(SOA)[ Service Oriented Architecture]是关键。

![image-20200801133801873](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133801873.png)

## 17.2 什么是RPC

RPC【Remote Procedure  Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：[https://www.jianshu.com/p/2accc2840a1b](https://gitee.com/link?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2F2accc2840a1b)

**RPC基本原理**

![image-20200801134404631](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801134404631.png)

![image-20200801134657756](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801134657756.png)

RPC两个核心模块：通讯，序列化。



## 17.3 Dubbo

Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

dubbo官网 [http://dubbo.apache.org/zh-cn/index.html](https://gitee.com/link?target=http%3A%2F%2Fdubbo.apache.org%2Fzh-cn%2Findex.html)

1.了解Dubbo的特性

2.查看官方文档

**dubbo基本概念**

![dubbo-architucture](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/dubbo-architecture.jpg)

![img](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/architecture.png)

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

## 17.4 Dubbo环境搭建

点进dubbo官方文档，推荐我们使用Zookeeper 注册中心

![image-20200801140529370](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801140529370.png)

什么是zookeeper呢？可以查看[官方文档](https://gitee.com/link?target=https%3A%2F%2Fzookeeper.apache.org%2F)

## 17.5 安装Zookeeper

1. 下载zookeeper ：[地址](https://gitee.com/link?target=https%3A%2F%2Fmirror.bit.edu.cn%2Fapache%2Fzookeeper%2F)， 我们下载3.6.1， 最新版！解压zookeeper

   下载带bin的

   ![image-20200801173958146](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801173958146.png)

2. 运行/bin/zkServer.cmd ，初次运行会报错，没有zoo.cfg配置文件；

   可能遇到问题：闪退 !

   解决方案：编辑zkServer.cmd文件末尾添加pause 。这样运行出错就不会退出，会提示错误信息，方便找到原因。

   ![image-20200801172648723](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801172648723.png)

   ![image-20200801173023636](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801173023636.png)

3. 修改zoo.cfg配置文件

   - 将conf文件夹下面的zoo_sample.cfg复制一份改名为zoo.cfg即可。

   - 注意几个重要位置：

     dataDir=./  临时数据存储的目录（可写相对路径）

     clientPort=2181  zookeeper的端口号

   - 修改完成后再次启动zookeeper

     ![image-20200801174251778](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801174251778.png)

4. 使用zkCli.cmd测试

   - ls /：列出zookeeper根下保存的所有节点

     ![image-20200801175746901](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801175746901.png)

     报错别着急，多试几次就好了，我试了三次o(╥﹏╥)o，看报错信息

     ![image-20200801175928795](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801175928795.png)

   - create –e /kuangshen 123：创建一个kuangshen节点，值为123

     ![image-20200801180120251](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801180120251.png)

   - get /kuangshen：获取/kuangshen节点的值

     ![image-20200801180225001](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801180225001.png)

   - 我们再来查看一下节点：ls /

     ![image-20200801180302890](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801180302890.png)

## 17.6 安装Dubbo-admin

- dubbo本身并不是一个服务软件。它其实就是一个jar包，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。
- 但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

我们这里来安装一下：

**1、下载dubbo-admin**

地址 ：[https://github.com/apache/dubbo-admin/tree/master](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fapache%2Fdubbo-admin%2Ftree%2Fmaster)

**2、解压进入目录**

修改 dubbo-admin\src\main\resources \application.properties 指定zookeeper地址

```
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

**3、在项目目录下**打包dubbo-admin

```
mvn clean package -Dmaven.test.skip=true
```

**第一次打包的过程有点慢，需要耐心等待！直到成功！**

![image-20200801181825286](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801181825286.png)

4、执行 dubbo-admin\target 下的dubbo-admin-0.0.1-SNAPSHOT.jar

```
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

【注意：zookeeper的服务一定要打开！】

执行完毕，我们去访问一下 [http://localhost:7001/](https://gitee.com/link?target=http%3A%2F%2Flocalhost%3A7001%2F) ， 这时候我们需要输入登录账户和密码，我们都是默认的root-root；

![image-20200801183043114](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801183043114.png)

登录成功后，查看界面

![image-20200801183113485](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801183113485.png)

安装完成！

**总结：**

- zookeeper :注册中心
- dubbo-admin:是一 个监控管理后台-查看我们注册了哪些服务，哪些服务被消费了（可以不用）
- Dubbo: jar包

## 17.7 SpringBoot + Dubbo + zookeeper

### 17.7.1 框架搭建

**1. 启动zookeeper ！**

**2. IDEA创建一个空项目；**

**3.创建一个模块，实现服务提供者：provider-server ， 选择web依赖即可**

**4.项目创建完毕，我们写一个服务，比如卖票的服务；**

编写接口

```
package nuc.ss.service;

public interface TicketService {
   public String getTicket();
}
```

编写实现类

```
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

```
package nuc.ss.service;

public interface UserService {
   //我们需要去拿去注册中心的服务
}
```

**需求：现在我们的用户想使用买票的服务，这要怎么弄呢 ？**

### 17.7.2 服务提供者

**1、将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导包**

**我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包**

```
<!-- Dubbo Spring Boot Starter -->
<dependency>
   <groupId>org.apache.dubbo</groupId>
   <artifactId>dubbo-spring-boot-starter</artifactId>
   <version>2.7.3</version>
</dependency>    
```

**zookeeper的包我们去maven仓库下载，zkclient；**

```
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
```

**【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；**

```
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

```
server.port=8001

#当前应用名字
dubbo.application.name=provider-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#扫描指定包下服务
dubbo.scan.base-packages=nuc.ss.service
```

**3、在service的实现类中配置服务注解，发布服务！注意导包问题**

```
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

**逻辑理解 ：应用启动起来，dubbo就会扫描指定的包下带有@component注解的服务，将它发布在指定的注册中心中！**

**4、运行测试**

![image-20200801204543375](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801204543375.png)

![image-20200801205620564](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801205620564.png)

![image-20200801205708690](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801205708690.png)

### 17.7.3 服务消费者

**1、导入依赖，和之前的依赖一样；**

```
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

```
server.port=8002

#当前应用名字
dubbo.application.name=consumer-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3. 本来正常步骤是需要将服务提供者的接口打包，然后用pom文件导入，我们这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；**

![image-20200801211028698](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801211028698.png)

**4. 完善消费者的服务类**

```
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

```
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

### 17.7.4 启动测试

**1. 开启zookeeper**

**2. 打开dubbo-admin实现监控【可以不用做】**

**3. 开启服务者**

**4. 消费者消费测试，结果：**

![image-20200801221551593](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801221551593.png)

**监控中心 ：**

![image-20200801222026747](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801222026747.png)

**ok , 这就是SpingBoot + dubbo + zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想；**





# 18. SpringSecurity

## 18.1 安全简介

1、在 Web  开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。

2、市面上存在比较有名的：Shiro，Spring Security ！

3、这里需要阐述一下的是，每一个框架的出现都是为了解决某一问题而产生了，那么Spring Security框架的出现是为了解决什么问题呢？

4、首先我们看下它的官网介绍：Spring Security官网地址

```
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

## 18.2 实战测试

### 18.2.1 实验环境搭建

1. 新建一个初始的springboot项目web模块，thymeleaf模块

2. 导入静态资源

   ![image-20200728130501139](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728130501139.png)

3. controller跳转！

   ```
   package nuc.ss.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RequestMapping;
   
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

   首页

   ![image-20200728130703899](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728130703899.png)

   登录

   ![image-20200728130726820](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728130726820.png)

   详情

   ![image-20200728130751338](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728130751338.png)

### 18.2.2 认识SpringSecurity

Spring Security 是针对Spring项目的安全框架，也是Spring  Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入  spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- `WebSecurityConfigurerAdapter`：自定义Security策略
- `AuthenticationManagerBuilder`：自定义认证策略
- `@EnableWebSecurity`：开启WebSecurity模式

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。

**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

**“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

### 18.2.3 认证和授权

目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能

1. 引入 Spring Security 模块

   ```
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

   ![image-20200728175946604](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728175946604.png)

2. 编写 Spring Security 配置类

   - 参考官网：[https://spring.io/projects/spring-security](https://gitee.com/link?target=https%3A%2F%2Fspring.io%2Fprojects%2Fspring-security)

   - 查看我们自己项目中的版本，找到对应的帮助文档：[https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5](https://gitee.com/link?target=https%3A%2F%2Fdocs.spring.io%2Fspring-security%2Fsite%2Fdocs%2F5.3.0.RELEASE%2Freference%2Fhtml5)

   - servlet-applications 8.16.4

     ```
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

     ![image-20200728175824524](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728175824524.png)

3. 编写基础配置类

   ![image-20200728180624787](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728180624787.png)

   ```
   package nuc.ss.config;
   
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   
   @EnableWebSecurity// 开启WebSecurity模式
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
   
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           super.configure(http);
       }
   }
   ```

4. 定制请求的授权规则

   看源码

   ![image-20200728190605894](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728190605894.png)

   仿写

   ```
   //链式编程
   @Override
   protected void configure(HttpSecurity http) throws Exception {
       // 首页所有人都可以访问，功能也只有对应有权限的人才能访问到
       // 请求授权的规则
   
       http.authorizeRequests()
           .antMatchers("/").permitAll()
           .antMatchers("/level1/**").hasRole("vip1")
           .antMatchers("/level2/**").hasRole("vip2")
           .antMatchers("/level3/**").hasRole("vip3");
   
   }
   ```

5. 测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！

   ![image-20200728185841148](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728185841148.png)

6. 在configure()方法中加入以下配置，开启自动配置的登录功能！

   ```
   // 开启自动配置的登录功能
   // /login 请求来到登录页
   // /login?error 重定向到这里表示登录失败
   http.formLogin();
   ```

7. 测试一下：发现，没有权限的时候，会跳转到登录的页面！

   ![image-20200728190113670](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728190113670.png)

8. 查看刚才登录页的注释信息；

   我们可以定义认证规则，重写configure的另一个方法

   ![image-20200728190303746](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728190303746.png)

   源码：

   ![image-20200728190840458](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728190840458.png)

   仿写

   ```
   // 认证，springboot 2.1.x 可以直接使用
   // 密码编码： PasswordEncoder
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   
       //这些数据正常应该中数据库中读
   
       auth.inMemoryAuthentication()
           .withUser("kuangshen").password("123456").roles("vip2","vip3")
           .and()
           .withUser("root").password("123456").roles("vip1","vip2","vip3")
           .and()
           .withUser("guest").password("123456").roles("vip1");
   }
   ```

9. 测试，我们可以使用这些账号登录进行测试！发现会报错！

   `There is no PasswordEncoder mapped for the id “null”`

   ![image-20200728204515545](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728204515545.png)

   ![image-20200728204424570](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728204424570.png)

10. 原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码

    ```
    // 认证，springboot 2.1.x 可以直接使用
    // 密码编码： PasswordEncoder
    // 在spring Secutiry 5.0+ 新增了很多加密方法
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    
        //这些数据正常应该中数据库中读
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
            .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
            .and()
            .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
            .and()
            .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
    }
    ```

11. 测试，发现，登录成功，并且每个角色只能访问自己认证下的规则！搞定

### 18.2.4 权限控制和注销

1. 开启自动配置的注销的功能

   ```
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
      //....
      //开启自动配置的注销的功能
      // /logout 注销请求
      http.logout();
   }
   ```

2. 我们在前端，增加一个注销的按钮，`index.html `导航栏中

   ```
   <!--注销-->
   <a class="item" th:href="@{/logout}">
       <i class="sign-out icon"></i> 注销
   </a>
   ```

3. 我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！

   ![image-20200728210246562](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728210246562.png)

   ![image-20200728210323067](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728210323067.png)

4. 但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

   源码：

   ![image-20200728211500366](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728211500366.png)

   ```
   // .logoutSuccessUrl("/"); 注销成功来到首页
   http.logout().logoutSuccessUrl("/");
   ```

5. 测试，注销完毕后，发现跳转到首页OK

6. 我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！还有就是，比如kuangshen这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？

   我们需要结合thymeleaf中的一些功能

   `sec：authorize="isAuthenticated()"`:是否认证登录！来显示不同的页面

   Maven依赖：

   ```
   <!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
   <dependency>
      <groupId>org.thymeleaf.extras</groupId>
      <artifactId>thymeleaf-extras-springsecurity5</artifactId>
      <version>3.0.4.RELEASE</version>
   </dependency>
   ```

   - 整合包4（springsecurity4）——springboot版本2.0.9
   - 整合包5（springsecurity5）——springboot版本之后

7. 修改我们的前端页面

   导入命名空间

   ```
   <html lang="en" xmlns:th="http://www.thymeleaf.org"
         xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">
   ```

   修改导航栏，增加认证判断

   ```
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

     ![image-20200728213100804](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728213100804.png)

   - 登录

     ![image-20200728213235625](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728213235625.png)

9. 点击注销产生的问题

   - 整合包4（springsecurity4）

     ![image-20200728220414292](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728220414292.png)

   - 整合包5（springsecurity5）（不算问题，需要点击确定，才能回到首页）

     ![image-20200728220517534](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728220517534.png)

     ![image-20200728220531678](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728220531678.png)

   解决问题：

   - 它默认防止csrf跨站请求伪造，因为会产生安全问题
   - 将请求改为post表单提交
   - 在spring security中关闭csrf功能`http.csrf().disable();`

   再次点击注销按钮之后（直接退出到首页）

   ![image-20200728220835347](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728220835347.png)

10. 我们继续将下面的角色功能块认证完成！

    ```
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

11. 测试一下！

- 用户首页未登录

  ![image-20200728221453455](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728221453455.png)

- 某个用户登录

  ![image-20200728221536116](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728221536116.png)

- 权限控制和注销搞定！

### 18.2.5 记住我

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单

1. 开启记住我功能

   ```
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

     ![image-20200728222312694](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728222312694.png)

   - 我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

     ![image-20200728222406216](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728222406216.png)

     思考：如何实现的呢？其实非常简单

     我们可以查看浏览器的cookie

     ![image-20200728222706154](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728222706154.png)

3. 我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

   ![image-20200728223559077](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728223559077.png)

4. cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，具体的原理我们在JavaWeb阶段都讲过了，这里就不在多说了！

### 18.2.6 定制登录页

现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1. 在刚才的登录页配置后面指定 loginpage

   ```
   protected void configure(HttpSecurity http) throws Exception {
       //......
   
       // 没有权限默认会到登录页面,需要开启登录的页面
       // /login页面
       http.formLogin().loginPage("/toLogin");
   
       //......
   }
   ```

2. 然后前端也需要指向我们自己定义的 login请求

   ```
   <div sec:authorize="!isAuthenticated()">
       <a class="item" th:href="@{/toLogin}">
           <i class="address card icon"></i> 登录
       </a>
   </div>
   ```

3. 我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:

   在 loginPage()源码中的注释上有写明：

   ![image-20200728224246393](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728224246393.png)

4. 这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！

   ![image-20200728224831116](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728224831116.png)

   ```
   protected void configure(HttpSecurity http) throws Exception {
       //......
   
       // 没有权限默认会到登录页面,需要开启登录的页面
       // /login页面
       http.formLogin()
         .usernameParameter("username")
         .passwordParameter("password")
         .loginPage("/toLogin")
         .loginProcessingUrl("/login"); // 登陆表单提交请求
   
       //......
   }
   ```

5. 在登录页增加记住我的多选框

   ```
   <input type="checkbox" name="remember"> 记住我
   ```

6. 后端验证处理！

   ```
   protected void configure(HttpSecurity http) throws Exception {
       //......
       //开启记住我功能: cookie,默认保存两周,自定义接收前端的参数
       http.rememberMe().rememberMeParameter("remember");
   }
   ```

7. 测试，OK





> 完整配置代码

```
package nuc.ss.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

// AOP:拦截器
@EnableWebSecurity  // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //链式编程
    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 首页所有人都可以访问，功能也只有对应有权限的人才能访问到
        // 请求授权的规则
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        // 没有权限默认会到登录页面,需要开启登录的页面
        // /login页面
        http.formLogin()
                .usernameParameter("username")
                .passwordParameter("password")
                .loginPage("/toLogin")
                .loginProcessingUrl("/login");

        //注销,开启了注销功能,跳到首页
        http.logout().logoutSuccessUrl("/");

        // 防止网站工具：get，post
        http.csrf().disable();//关闭csrf功能，登录失败肯定存在的原因

        //开启记住我功能: cookie,默认保存两周,自定义接收前端的参数
        http.rememberMe().rememberMeParameter("remember");


    }

    // 认证，springboot 2.1.x 可以直接使用
    // 密码编码： PasswordEncoder
    // 在spring Secutiry 5.0+ 新增了很多加密方法
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        //这些数据正常应该中数据库中读
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
    }


}
```





