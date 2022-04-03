# 12. 整合JDBC

## 12.1 创建测试项目测试数据源

1. 我去新建一个项目测试：springboot-data-jdbc ; 引入相应的模块！基础模块

   ![image-20220402124510910](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402124510910.png)

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

   ![image-20220402130148743](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402130148743.png)

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

   ![image-20220402131634755](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402131634755.png)

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

![image-20200727233409312](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727233409312.png)

进入之后

![image-20200727233436583](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200727233436583.png)

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

   ![image-20220402151410667](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402151410667.png)

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

   ![image-20220402154524753](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402154524753.png)


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

   ![image-20220402155719261](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402155719261.png)

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

   ![image-20220402160154398](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402160154398.png)

6. 在`configure()`方法中加入以下配置，开启自动配置的登录功能！

   ```java
   // 开启自动配置的登录功能
   //   /login 请求来到登录页
   //   /login?error 重定向到这里表示登录失败
   http.formLogin();
   ```

7. 随便点击一个需要权限的level，发现没有权限的时候，会跳转到登录的页面：

   ![image-20220402160536691](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402160536691.png)

8. 我们可以自定义认证规则，重写configure的另一个方法，参数为`AuthenticationManagerBuilder`：

   先查看源码：

   ![image-20220402161204046](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402161204046.png)

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

   ![image-20220402161700260](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402161700260.png)

   ![image-20220402161636119](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402161636119.png)

   

   

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

   ![image-20220402162956586](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402162956586.png)

4. 但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

   看源码：

   ![image-20220402163401739](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402163401739.png)

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

     ![image-20220402165034257](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402165034257.png)

   - 登录

     ![image-20220402165018635](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402165018635.png)

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

      ![image-20220402165550336](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402165550336.png)


    - 某个用户登录：
    
      ![image-20220402165644511](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402165644511.png)


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

     ![image-20220402165834723](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402165834723.png)

   - 我们登录之后关闭浏览器，然后重新打开浏览器访问，发现用户依旧存在！

     ![image-20220402165913705](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402165913705.png)思考：如何实现的呢？其实非常简单

     我们可以查看浏览器的Cookies：

     ![image-20220402170213104](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402170213104.png)

3. 我们点击注销的时候，可以发现，Spring Security 帮我们自动删除了这个cookie(置为空)：

   ![image-20220402170511522](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402170511522.png)

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

   ![image-20220402171827509](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220402171827509.png)

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

![image-20220403132629590](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403132629590.png)
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

![image-20220403132736544](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403132736544.png)
● `subject`: 应用代码直接交互的对象是Subject, 也就是说Shiro的对外API核心就是Subject, Subject代表了当前的用户，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject,如网络爬虫，机器人等，与Subject的所有交互都会委托给SecurityManager; Subject其实是一个门面， SecurityManageer 才是实际的执行者
● `SecurityManager`: 安全管理器，即所有与安全有关的操作都会与SercurityManager交互, 并且它管理着所有的Subject,可以看出它是Shiro的核心，它负责与Shiro的其他组件进行交互，它相当于SpringMVC的DispatcherServlet的角色
● `Realm`: Shiro从Realm获取安全数据 (如用户,角色，权限)，就是说SecurityManager要验证用户身份，那么它需要从Realm获取相应的用户进行比较，来确定用户的身份是否合法;也需要从Realm得到用户相应的角色、权限，进行验证用户的操作是否能够进行，可以把Realm看DataSource;

### 16.1.3 Shiro架构(内部)

![image-20220403132756268](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403132756268.png)
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

   ![image-20220403135721889](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403135721889.png)

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

   ![image-20220403171551122](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403171551122.png)

   

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

![image-20220403181857060](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403181857060.png)

3、授权

- 对`subject`进行授权，调用方法`isPermitted(“permission串”)`，递交给`SecurityManager`
- `SecurityManager`将权限检测操作委托给`Authorizer`授权管理器对象
- `Authorizer`执行`Realm`（自定义的Realm）从数据库查询权限数据并封装
- `Authorizer`对用户授权信息进行判定(判断用户访问资源时需要什么权限，假如用户所具有的权限包含这个资源访问时所需要的权限，那么用户就可以访问这个资源了)

![image-20220403181951827](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220403181951827.png)

# 17. Swagger

- 了解Swagger的概念及作用
- 了解前后端分离
- 在springboot中集成swagger

## 17.1 Swagger简介

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

## 17.2 SpringBoot集成Swagger

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

## 17.3 配置Swagger

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

## 17.4 配置扫描接口

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

## 17.5 配置Swagger开关

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

## 17.6 配置API分组

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

## 17.7 实体配置

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

## 17.9 拓展：其他皮肤

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





# 18. 异步、定时、邮件任务

## 18.1 异步任务

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

## 18.2 邮件任务

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

## 18.3 定时任务

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





# 19. Dubbo和Zookeeper集成

## 19.1 分布式理论

### 19.1.1 什么是分布式系统？

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。

### 19.1.2 Dubbo文档

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图

![img](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/dubbo-architecture-roadmap.jpg)

### 19.1.3 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

![image-20200801133522303](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133522303.png)

适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

**缺点：**

1、性能扩展比较难

2、协同开发问题

3、不利于升级维护

### 19.1.4 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

![image-20200801133625663](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133625663.png)

通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

缺点：公用模块无法重复利用，开发性的浪费

### 19.1.5 分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服务框架(RPC)**是关键。

![image-20200801133710784](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133710784.png)

### 19.1.6 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心**(SOA)[ Service Oriented Architecture]是关键。

![image-20200801133801873](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801133801873.png)

## 19.2 什么是RPC

RPC【Remote Procedure  Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：[https://www.jianshu.com/p/2accc2840a1b](https://gitee.com/link?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2F2accc2840a1b)

**RPC基本原理**

![image-20200801134404631](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801134404631.png)

![image-20200801134657756](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801134657756.png)

RPC两个核心模块：通讯，序列化。



## 19.3 Dubbo

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

## 19.4 Dubbo环境搭建

点进dubbo官方文档，推荐我们使用Zookeeper 注册中心

![image-20200801140529370](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801140529370.png)

什么是zookeeper呢？可以查看[官方文档](https://gitee.com/link?target=https%3A%2F%2Fzookeeper.apache.org%2F)

## 19.5 安装Zookeeper

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

## 19.6 安装Dubbo-admin

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

## 19.7 SpringBoot + Dubbo + zookeeper

### 19.7.1 框架搭建

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

### 19.7.2 服务提供者

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

### 19.7.3 服务消费者

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

### 19.7.4 启动测试

**1. 开启zookeeper**

**2. 打开dubbo-admin实现监控【可以不用做】**

**3. 开启服务者**

**4. 消费者消费测试，结果：**

![image-20200801221551593](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801221551593.png)

**监控中心 ：**

![image-20200801222026747](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200801222026747.png)

**ok , 这就是SpingBoot + dubbo + zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想；**
