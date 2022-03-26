# Mybatis

## 1. 简介

### 1.1 什么是Mybatis

- MyBatis 是一款优秀的**持久层框架**
- 它支持自定义 SQL、存储过程以及高级映射
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录
- MyBatis本是apache的一个开源项目iBatis，2010年这个项目由apache software foundation迁移到了google code，并且改名为MyBatis
- 2013年11月迁移到Github
- iBATIS一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架
- iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAOs）



### 1.2 如何获得Mybatis

- maven仓库
- Github：https://github.com/mybatis/mybatis-3
- 中文文档：https://mybatis.org/mybatis-3/zh/index.html



### 1.3 持久化

数据持久化

- **持久化**就是**将程序的数据在持久状态和瞬时状态转化的过程**
- 内存：**断电即失**
- 数据库(JDBC)，IO文件持久化

为什么需要持久化？

- 有一些对象不能丢掉
- 内存太贵



### 1.4 持久层

- 完成持久化工作的代码块
- 层界限十分明显



### 1.5 为什么需要Mybatis

- 方便
- 传统的JDBC代码太复杂了，简化框架
- 帮助程序猿将数据存入到数据库中

**优点：**

- 简单易学
- 灵活
- 解除sql与程序代码的耦合：sql和代码的分离，提高了可维护性。
- 提供映射标签，支持对象与数据库的orm字段关系映射。
- 提供对象关系映射标签，支持对象关系组建维护。
- 提供xml标签，支持编写动态sql。





## 2. 第一个Mybatis程序

**思路：**搭建环境-->导入Mybatis-->编写代码-->测试



1. 搭建数据库

   ```sql
   CREATE DATABASE `mybatis`;
   
   USE `mybatis`;
   
   DROP TABLE IF EXISTS `user`;
   
   CREATE TABLE `user` (
   `id` int(20) NOT NULL,
   `name` varchar(30) DEFAULT NULL,
   `pwd` varchar(30) DEFAULT NULL,
   PRIMARY KEY (`id`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   
   insert  into `user`(`id`,`name`,`pwd`) values (1,'狂神','123456'),(2,'张三','abcdef'),(3,'李四','987654');
   ```

2. 在pom文件中导入MyBatis相关 jar 包

   ```xml
   <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
   </dependency>
   <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
   </dependency>
   ```

3. 编写MyBatis核心配置文件`mybatis-config.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="123456"/>
              </dataSource>
          </environment>
      </environments>
       
       <!--每个Mapper.xml都需要在核心配置文件中注册-->
      <mappers>
          <mapper resource="com/kuang/dao/userMapper.xml"/>
      </mappers>
   </configuration>
   ```

4. 编写MyBatis工具类

   ```java
   public class MybatisUtils {
   
      private static SqlSessionFactory sqlSessionFactory;
   
       //获取sqlSessionFactory对象
      static {
          try {
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
         } catch (IOException e) {
              e.printStackTrace();
         }
     }
   
      //获取SqlSession实例；可以在此开启自动提交sqlSessionFactory.openSession(true);
      public static SqlSession getSession(){
          return sqlSessionFactory.openSession();
     }
   }
   ```

5. 创建实体类

   ```java
   @Data
   public class User {
       
       private int id;  //id
       private String name;   //姓名
       private String pwd;   //密码 
   }
   ```

6. 编写Mapper接口类

   ```java
   public interface UserMapper {
       List<User> selectUser();
   }
   ```

7. 编写Mapper.xml配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.kuang.dao.UserMapper">
    <select id="selectUser" resultType="com.kuang.pojo.User">
     	select * from mybatis.user
    </select>
   </mapper>
   ```

8. 编写测试类

   ```java
   public class MyTest {
      @Test
      public void selectUser() {
          SqlSession session = MybatisUtils.getSession();
          //方法一:
          //List<User> users = session.selectList("com.kuang.mapper.UserMapper.selectUser");
          //方法二:
          UserMapper mapper = session.getMapper(UserMapper.class);
          List<User> users = mapper.selectUser();
   
          for (User user: users){
              System.out.println(user);
         }
          session.close();
     }
   }
   ```

9. 运行测试





> 问题说明

**可能出现问题说明：Maven静态资源过滤问题**

```xml
 <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```



> 注意：增删改需要提交事务



### 2.1 练习

根据 密码 和 名字 查询用户

思路一：直接在方法中传递参数

1、在接口方法的参数前加 @Param属性

2、Sql语句编写的时候，直接取@Param中设置的值即可，不需要单独设置参数类型

```xml
//通过密码和名字查询用户
User selectUserByNP(@Param("username") String username,@Param("pwd") String pwd);

/*
    <select id="selectUserByNP" resultType="com.kuang.pojo.User">
      select * from user where name = #{username} and pwd = #{pwd}
    </select>
*/
```



思路二：使用万能的Map

1、在接口方法中，参数直接传递Map；

```java
User selectUserByNP2(Map<String,Object> map);
```

2、编写sql语句的时候，需要传递参数类型，参数类型为map

```xml
<select id="selectUserByNP2" parameterType="map" resultType="com.kuang.pojo.User">
  select * from user where name = #{username} and pwd = #{pwd}
</select>
```

3、在使用方法的时候，Map的 key 为 sql中取的值即可，没有顺序要求！

```java
Map<String, Object> map = new HashMap<String, Object>();
map.put("username","小明");
map.put("pwd","123456");
User user = mapper.selectUserByNP2(map);
```

总结：如果参数过多，我们可以考虑直接使用Map实现，如果参数比较少，直接传递参数即可



## 3. 配置解析

### 3.1 核心配置文件

- mybatis-config.xml 系统核心配置文件
- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。
- 能配置的内容如下：**注意元素节点的顺序！顺序不对会报错**

```xml
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
        environment（环境变量）
            transactionManager（事务管理器）
            dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
<!-- 注意元素节点的顺序！顺序不对会报错 -->
```

我们可以阅读 mybatis-config.xml 上面的dtd的头文件！



### 3.2 environments元素

```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

- 配置MyBatis的多套运行环境，将SQL映射到多个**不同的数据库**上，必须指定其中一个为默认运行环境（通过default指定）

- 子元素节点：**environment**

- 具体的一套环境，通过设置id进行区别，id保证唯一！

  - 子元素节点：transactionManager - [ 事务管理器 ]

    ```
    <!-- 语法 -->
    <transactionManager type="[ JDBC | MANAGED ]"/>
    ```

  - 这两种事务管理器类型都不需要设置任何属性。

  

  - 子元素节点：**数据源（dataSource）**

  - dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

  - 数据源是必须配置的。

  - 有三种内建的数据源类型

    ```
    type="[UNPOOLED|POOLED|JNDI]"）
    ```

    - unpooled：这个数据源的实现只是每次被请求时打开和关闭连接。
    - **pooled**：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得**并发 Web 应用快速响应**请求的流行处理方式。
    - jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

  - 数据源也有很多第三方的实现，比如dbcp，c3p0，druid等等....



### 3.3 Properties优化

数据库这些属性都是可外部配置且可动态替换的，既可以在典型的 Java 属性文件中配置，亦可通过 properties 元素的子元素来传递。具体的官方文档

我们来优化我们的配置文件

第一步：在资源目录下新建一个`db.properties`

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatisuseSSL=true&useUnicode=true&characterEncoding=utf8
username=root
password=123456
```

第二步：将文件导入properties 配置文件

```xml
<configuration>
    <!--导入properties文件-->
    <properties resource="db.properties"/>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

- 在`properties`中也还可以添加`property`来添加一些属性配置，但是如果外部文件中和内部文件有同一个字段，优先使用外部配置文件的。
- 新特性：使用占位符



### 3.4 typeAliases优化

**类型别名**是为 Java 类型设置一个短的名字。它**只和XML配置**有关，存在的意义仅在于用来**减少类完全限定名的冗余**。

```xml
<!--配置别名，注意标签的顺序-->
<typeAliases>
    <typeAlias type="com.kuang.pojo.User" alias="User"/>
</typeAliases>
```

当这样配置时，`User`可以用在任何使用`com.kuang.pojo.User`的地方。



也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

```xml
<typeAliases>
    <package name="com.kuang.pojo"/>
</typeAliases>
```

每一个在包 `com.kuang.pojo` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的**首字母小写的类名**来作为它的别名。



若有注解，则别名为其注解值。见下面的例子：

```java
@Alias("user")
public class User {
    ...
}
```

Mybatis中还有一些默认的类型别名！在官网中可以查看





### 3.5 mappers元素

**mappers**

- MyBatis 是基于 sql 映射配置的框架，sql 语句都写在 Mapper 配置文件中，当构建 SqlSession 类之后，就需要去读取 Mapper 配置文件中的 sql 配置。而 mappers 标签就是用来配置需要加载的 sql 映射配置文件路径的。



mappers 标签下有许多 mapper 标签，每一个 mapper 标签中配置的都是一个独立的映射配置文件的路径，配置方式有以下几种：

1. **相对路径配置(推荐使用)：**

   - `mapper标签`，通过`resource`属性引入`classpath路径`的相对资源

   ```xml
   <mappers>
     <mapper resource="org/mybatis/builder/PostMapper.xml"/>
   </mappers>
   ```

   

2. **接口所在包：**

   - `package标签`，通过`name`属性指定`mapper接口`所在的包名，此时对应的`映射文件必须与接口位于同一路径下`，并且`名称相同`

   ```xml
   <mappers>
     <package name="org.mybatis.builder"/>
   </mappers>
   ```

   

3. **类注册引入：**

   - `mapper标签`，通过`class`属性指定mapper接口名称，此时对应的`映射文件必须与接口位于同一路径下`，并且`名称相同`

   ```xml
   <mappers>
     <mapper class="org.mybatis.builder.AuthorMapper"/>
   </mappers>
   ```

   

4. **使用URL绝对路径方式引入(不用)：**

   - mapper标签，通过url引入网络资源或者本地磁盘资源

   ```xml
   <mappers>
     <mapper url="file:///var/mappers/AuthorMapper.xml"/>
   </mappers>
   ```

   



**`Mapper.xml`文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
    	·····SQL语句
</mapper>
```

- namespace中文意思：命名空间，作用如下：

- - namespace的命名必须跟某个接口同名
  - 接口中的**方法**与映射文件中**sql语句id**应该一一对应

- - namespace和子元素的id联合保证唯一  , 区别不同的mapper
  - 绑定DAO接口
  - namespace命名规则 : 包名+类名





### 3.6 其他配置浏览

**设置**

- 设置（settings）相关 => 查看帮助文档

- - 懒加载
  - 日志实现
  - 缓存开启关闭

- 一个配置完整的 settings 元素的示例如下：

  ```xml
  <settings>
    <setting name="cacheEnabled" value="true"/>
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="multipleResultSetsEnabled" value="true"/>
    <setting name="useColumnLabel" value="true"/>
    <setting name="useGeneratedKeys" value="false"/>
    <setting name="autoMappingBehavior" value="PARTIAL"/>
    <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
    <setting name="defaultExecutorType" value="SIMPLE"/>
    <setting name="defaultStatementTimeout" value="25"/>
    <setting name="defaultFetchSize" value="100"/>
    <setting name="safeRowBoundsEnabled" value="false"/>
    <setting name="mapUnderscoreToCamelCase" value="false"/>
    <setting name="localCacheScope" value="SESSION"/>
    <setting name="jdbcTypeForNull" value="OTHER"/>
    <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
  </settings>
  ```





### 3.7 生命周期和作用域

**作用域（Scope）和生命周期**

不同作用域和生命周期类是至关重要的，因为错误的使用会导致非常严重的并发问题。

我们可以先画一个流程图，分析一下Mybatis的执行过程！



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718619.png)



**作用域理解**

**SqlSessionFactoryBuilder：**

- `SqlSessionFactoryBuilder` 的作用在于创建 `SqlSessionFactory`，创建成功后，`SqlSessionFactoryBuilder` 就失去了作用。
- 最佳作用域：**局部变量**



**SqlSessionFactory：**

- `SqlSessionFactory` 可以被认为是一个数据库连接池，它的作用是创建 `SqlSession` 接口对象。
- 因为 MyBatis 的本质就是 Java 对数据库的操作，所以 `SqlSessionFactory` 的生命周期存在于**整个 MyBatis 的应用之中**，所以一旦创建了 SqlSessionFactory，就要长期保存它，直至不再使用 MyBatis 应用，所以可以认为 SqlSessionFactory 的生命周期就等同于 MyBatis 的应用周期。
- 最佳作用域：**应用作用域**



**SqlSession：**

- 连接到连接池的一个请求
- SqlSession的实例不是线程安全的，因此是不能被共享的
- 最佳作用域：**请求或方法**
- 用完之后需要赶紧关闭，否则资源会被占用



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718134.png)

每个Mapper对应一个具体的业务！！！





## 4. ResultMap

### 4.1 问题引出

实体类中的属性名和表中的字段名不一致。

示例：

1、查看之前的数据库的字段名

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718376.png)

2、Java中的实体类设计

```java
public class User {

    private int id;  //id
    private String name;   //姓名
    private String password;   //密码和数据库不一样！
    
    //构造
    //set/get
    //toString()
}
```

3、UserMapper接口

```java
//根据id查询用户
User selectUserById(int id);
```

4、`UserMapper.xml`映射文件

```xml
<select id="selectUserById" resultType="user">
    select * from user where id = #{id}
</select>
```

5、测试

```java
@Test
public void testSelectUserById() {
    SqlSession session = MybatisUtils.getSession();  //获取SqlSession连接
    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selectUserById(1);
    System.out.println(user);
    session.close();
}
```

**结果:**

```
User{id=1, name='狂神', password='null'}
```

查询出来发现 password 为空, 说明出现了问题！



**分析：**

- `select * from user where id = #{id}` 可以看做

  `select  id,name,pwd  from user where id = #{id}`

- mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的set方法设值 , 由于找不到setPwd() , 所以password返回null ; 【自动映射】



> 解决方案

方案一：为列名指定别名 , 别名和java实体类的属性名一致 .

```xml
<select id="selectUserById" resultType="User">
    select id , name , pwd as password from user where id = #{id}
</select>
```

**方案二：使用结果集映射->ResultMap** 【推荐】

```xml
<resultMap id="UserMap" type="User">
    <!-- id为主键 -->
    <id column="id" property="id"/>
    <!-- column是数据库表的字段(列名) , property是对应实体类的属性名 -->
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>

<select id="selectUserById" resultMap="UserMap">
    select id , name , pwd from user where id = #{id}
</select>
```





### 4.2 ResultMap概念

**自动映射：**

- `ResultMap` 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBC `ResultSets` 数据提取代码中解放出来。
- `ResultMap` 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的**关系**就行了。



你已经见过简单映射语句的示例了，但并没有显式指定 `resultMap`。比如：

```xml
<select id="selectUserById" resultType="map">
select id , name , pwd
    from user
    where id = #{id}
</select>
```

上述语句只是简单地将所有的列映射到 `HashMap` 的键上，这由 `resultType` 属性指定。虽然在大部分情况下都够用，但是 HashMap 不是一个很好的模型。

你的程序更可能会使用 JavaBean 或 POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。此时当数据库表中的字段名可能与Bean中的属性名不一致，那么`ResultMap`就可以建立他们之间的对应关系。



**手动映射：**

1、返回值类型为resultMap

```xml
<select id="selectUserById" resultMap="UserMap">
    select id , name , pwd from user where id = #{id}
</select>
```

2、编写resultMap，实现手动映射！

```xml
<resultMap id="UserMap" type="User">
    <!-- id为主键 -->
    <id column="id" property="id"/>
    <!-- column是数据库表的列名 , property是对应实体类的属性名 -->
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>
```

如果世界总是这么简单就好了。但是肯定不是的，数据库中，存在一对多，多对一的情况，我们之后会使用到一些高级的结果集映射，`association`，`collection`这些，我们将在之后讲解。







## 5. 日志工厂

思考：我们在测试SQL的时候，要是能够在控制台输出 SQL 的话，是不是就能够有更快的排错效率？

如果一个 数据库相关的操作出现了问题，我们可以根据输出的SQL语句快速排查问题。

对于以往的开发过程，我们会经常使用到debug模式来调节，跟踪我们的代码执行过程。但是现在使用Mybatis是基于接口，配置文件的源代码执行过程。因此，我们必须选择日志工具来作为我们开发，调节程序的工具。

Mybatis内置的日志工厂提供日志功能，具体的日志实现有以下几种工具：

- SLF4J
- Log4J(掌握)
- Log4J 2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING（掌握）
- NO_LOGGING

具体选择哪个日志实现工具，在设置中设定



### 5.1 标准日志STDOUT_LOGGING

```
<settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

测试，可以看到控制台有大量的输出！我们可以通过这些输出来判断程序到底哪里出了Bug

```
Logging initialized using 'class org.apache.ibatis.logging.stdout.StdOutImpl' adapter.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
Opening JDBC Connection
Created connection 1646464088.
Setting autocommit to false on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@62230c58]
==>  Preparing: select * from mybatis.user
==> Parameters: 
<==    Columns: id, name, pwd
<==        Row: 1, 狂神, 123456
<==        Row: 2, 张三, abcdef
<==        Row: 3, 李四, 987654
<==        Row: 4, xiaolu, 123123
<==      Total: 4
User(id=1, name=狂神, pwd=123456)
User(id=2, name=张三, pwd=abcdef)
User(id=3, name=李四, pwd=987654)
User(id=4, name=xiaolu, pwd=123123)
Resetting autocommit to true on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@62230c58]
Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@62230c58]
Returned connection 1646464088 to pool.

Process finished with exit code 0

```





### 5.2 Log4j

**简介：**

- Log4j是Apache的一个开源项目
- 通过使用Log4j，我们可以控制日志信息输送的目的地：控制台，文本，GUI组件....
- 我们也可以控制每一条日志的输出格式；
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。最令人感兴趣的就是，这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。

**使用步骤：**

1、导入log4j的包

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

2、配置文件`log4j.properties`编写

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/kuang.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

3、setting设置日志实现

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

4、在程序中使用Log4j进行输出！

```java
public class UserMapperTest {
    //注意导包：org.apache.log4j.Logger
    static Logger logger = Logger.getLogger(UserMapperTest.class);

    @Test
    public void test() {
        logger.info("info：进入selectUser方法");
        logger.debug("debug：进入selectUser方法");
        logger.error("error: 进入selectUser方法");
        SqlSession sqlSession = MybatisUtil.getSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.selectAllUser();

        for (User user : userList) {
            System.out.println(user);
        }
        sqlSession.close();
    }
}
```

5、测试，看控制台输出！

- 使用Log4j 输出日志

  ```
  [com.run.dao.UserMapperTest]-info：进入selectUser方法
  [com.run.dao.UserMapperTest]-debug：进入selectUser方法
  [com.run.dao.UserMapperTest]-error: 进入selectUser方法
  [org.apache.ibatis.logging.LogFactory]-Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
  [org.apache.ibatis.logging.LogFactory]-Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
  [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
  [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
  [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
  [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
  [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Opening JDBC Connection
  [org.apache.ibatis.datasource.pooled.PooledDataSource]-Created connection 375457936.
  [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Setting autocommit to false on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@16610890]
  [com.run.dao.UserMapper.selectAllUser]-==>  Preparing: select * from mybatis.user
  [com.run.dao.UserMapper.selectAllUser]-==> Parameters: 
  [com.run.dao.UserMapper.selectAllUser]-<==      Total: 4
  User(id=1, name=狂神, pwd=123456)
  User(id=2, name=张三, pwd=abcdef)
  User(id=3, name=李四, pwd=987654)
  User(id=4, name=xiaolu, pwd=123123)
  [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Resetting autocommit to true on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@16610890]
  [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@16610890]
  [org.apache.ibatis.datasource.pooled.PooledDataSource]-Returned connection 375457936 to pool.
  
  Process finished with exit code 0
  ```

  

- 可以看到还生成了一个日志的文件 【需要修改file的日志级别】

  ![image-20220318173302318](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717621.png)





## 6. 结果分页

### 6.1 limit实现分页

**思考：为什么需要分页？**

如果查询大量数据的时候，我们往往使用分页进行查询，也就是每次处理小部分数据，这样对数据库压力就在可控范围内。

**使用Limit实现分页**

```sql
#语法
SELECT * FROM table LIMIT stratIndex，pageSize

SELECT * FROM table LIMIT 5,10; // 检索记录行 6-15  

#为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1：   
SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last.  

#如果只给定一个参数，它表示返回最大的记录行数目：   
SELECT * FROM table LIMIT 5; //检索前 5 个记录行  
 
#换句话说，LIMIT n 等价于 LIMIT 0,n。 
```

**步骤：**

1、修改`Mapper.xml`文件

```xml
<select id="selectUser" parameterType="map" resultType="user">
    select * from user limit #{startIndex},#{pageSize}
</select>
```

2、Mapper接口，参数为map

```java
//选择全部用户实现分页
List<User> selectUser(Map<String,Integer> map);
```

3、在测试类中传入参数测试

- 推断：起始位置 =  （当前页面 - 1 ） * 页面大小

```java
//分页查询 , 两个参数startIndex , pageSize
@Test
public void testSelectUser() {
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    int currentPage = 1; 	  //第几页
    int pageSize = 2; 		 //每页显示几个
    Map<String,Integer> map = new HashMap<String,Integer>();
    map.put("startIndex", (currentPage - 1) * pageSize);
    map.put("pageSize", pageSize);

    List<User> users = mapper.selectUser(map);

    for (User user: users){
        System.out.println(user);
    }
    session.close();
}
```

输出：

```
User(id=1, name=狂神, pwd=123456)
User(id=2, name=张三, pwd=abcdef)
```





### 6.2 RowBounds分页(了解)

我们除了使用Limit在SQL层面实现分页，也可以使用RowBounds在Java代码层面实现分页，当然此种方式作为了解即可。我们来看下如何实现的！

**步骤：**

1、mapper接口

```java
//选择全部用户RowBounds实现分页
List<User> getUserByRowBounds();
```

2、mapper文件

```xml
<select id="getUserByRowBounds" resultType="user">
  select * from user
</select>
```

3、测试类

在这里，我们需要使用RowBounds类

```java
@Test
public void testUserByRowBounds() {
    SqlSession session = MybatisUtils.getSession();

    int currentPage = 2;  //第几页
    int pageSize = 2;  //每页显示几个
    RowBounds rowBounds = new RowBounds((currentPage-1)*pageSize,pageSize);

    //通过session.**方法进行传递rowBounds，[此种方式现在已经不推荐使用了]
    List<User> users = session.selectList("com.kuang.dao.UserMapper.getUserByRowBounds", null, rowBounds);

    for (User user: users){
        System.out.println(user);
    }
    session.close();
}
```





### 6.3 PageHelper

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718398.png)

了解即可，可以自己尝试使用

官方文档：https://pagehelper.github.io/





## 7. 使用注解开发

> 面向接口编程

- 大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程
- **根本原因** :  **解耦** , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好



**关于接口的理解**

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类：

- - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

- 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。



**三个面向区别**

- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .

- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .

- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构

  

### 7.1 利用注解开发

- **mybatis最初配置信息是基于 XML，映射语句(SQL)也是定义在 XML 中的。而到MyBatis 3提供了新的基于注解的配置。不幸的是，Java 注解的的表达力和灵活性十分有限。最强大的 MyBatis 映射并不能用注解来构建**
- sql 类型主要分成 ：
  - @select ()
  - @update ()
  - @Insert ()
  - @delete ()




**注意：**利用注解开发就不需要mapper.xml映射文件了 .

1、我们在我们的接口中添加注解

```java
//查询全部用户
@Select("select id,name,pwd password from user")
public List<User> getAllUser();
```

2、在mybatis的核心配置文件中注入

```xml
<!--使用class绑定接口-->
<mappers>
    <mapper class="com.kuang.mapper.UserMapper"/>
</mappers>
```

3、我们去进行测试

```java
@Test
public void testGetAllUser() {
    SqlSession session = MybatisUtils.getSession();
    //本质上利用了jvm的动态代理机制
    UserMapper mapper = session.getMapper(UserMapper.class);

    List<User> users = mapper.getAllUser();
    for (User user : users){
        System.out.println(user);
    }

    session.close();
}
```

4、利用Debug查看本质

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718089.png)

5、本质上利用了jvm的动态代理机制

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718650.png)

6、Mybatis详细的执行流程(源代码)

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718027.png)



### 7.2 注解实现增删改

改造MybatisUtils工具类的getSession( ) 方法，重载实现。

```java
   //获取SqlSession连接
   public static SqlSession getSession(){
       return getSession(true); //事务自动提交
   }
  
   public static SqlSession getSession(boolean flag){
       return sqlSessionFactory.openSession(flag);
   }
```

【注意】确保实体类和数据库字段对应



**查询：**

1、编写接口方法注解

```java
//根据id查询用户
@Select("select * from user where id = #{id}")
User selectUserById(@Param("id") int id);
```

2、测试

```java
@Test
public void testSelectUserById() {
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    User user = mapper.selectUserById(1);
    System.out.println(user);

    session.close();
}
```



**新增：**

1、编写接口方法注解

```java
//添加一个用户
@Insert("insert into user (id,name,pwd) values (#{id},#{name},#{pwd})")
int addUser(User user);
```

2、测试

```java
@Test
public void testAddUser() {
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    User user = new User(6, "秦疆", "123456");
    mapper.addUser(user);

    session.close();
}
```



**修改：**

1、编写接口方法注解

```java
//修改一个用户
@Update("update user set name=#{name},pwd=#{pwd} where id = #{id}")
int updateUser(User user);
```

2、测试

```java
@Test
public void testUpdateUser() {
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    User user = new User(6, "秦疆", "zxcvbn");
    mapper.updateUser(user);

    session.close();
}
```



**删除：**

1、编写接口方法注解

```java
//根据id删除用
@Delete("delete from user where id = #{id}")
int deleteUser(@Param("id")int id);
```

2、测试

```java
@Test
public void testDeleteUser() {
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    mapper.deleteUser(6);
    
    session.close();
}
```

【注意点：增删改一定记得对事务的处理】



### 7.3 关于@Param

@Param注解用于给方法参数起一个名字。以下是总结的使用原则：

- 在方法只接受一个参数的情况下，可以不使用@Param。
- 在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。
- 如果参数是 JavaBean ， 则不能使用@Param。
- 不使用@Param注解时，参数只能有一个，并且是Javabean。



### 7.4 #与$的区别

- \#{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符? 【推荐使用】

  ```sql
  INSERT INTO user (name) VALUES (#{name});
  INSERT INTO user (name) VALUES (?);
  ```

- ${} 的作用是直接进行字符串替换

  ```sql
  INSERT INTO user (name) VALUES ('${name}');
  INSERT INTO user (name) VALUES ('kuangshen');
  ```



**使用注解和配置文件协同开发，才是MyBatis的最佳实践！**



## 8. 复杂查询

### 8.1 多对一

多对一的理解：

- 多个学生对应一个老师
- 如果对于学生这边，就是一个多对一的现象，即从学生这边关联一个老师！



> 数据库设计

![image-20220319094933576](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717623.png)

```sql
CREATE TABLE `teacher` (
  `id` INT(10) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
  `id` INT(10) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  `tid` INT(10) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `fktid` (`tid`),
  CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8


INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```



> 搭建测试环境

1、创建相关pojo

```java
@Data //GET,SET,ToString，有参，无参构造
public class Teacher {
    private int id;
    private String name;
}
```

```java
@Data
public class Student {
    private int id;
    private String name;
    //多个学生可以是同一个老师，即多对一
    private Teacher teacher;
}
```



2、编写实体类对应的Mapper接口 【两个】

- **无论有没有需求，都应该写上，以备后来之需！**

```java
public interface StudentMapper {
}
```

```java
public interface TeacherMapper {
}
```



3、编写Mapper接口对应的 mapper.xml配置文件 【两个】

- **无论有没有需求，都应该写上，以备后来之需！**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.run.dao.StudentMapper">

</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.run.dao.TeacherMapper">

</mapper>
```



#### 8.1.1 按查询嵌套处理

1、给StudentMapper接口增加方法

```java
//获取所有学生及对应老师的信息
public List<Student> getStudents();
```



2、编写对应的`Mapper.xml`文件

需求：获取所有学生及对应老师的信息
思路：

1. 获取所有学生的信息
2. 根据获取的学生信息的老师tid -> 获取该老师的信息
3. 这样学生的结果集中应该包含老师，该如何处理呢，数据库中一般使用关联查询
   1. 做一个结果集映射`resultMap`：studentAndTeacher
   2. studentAndTeacher结果集的类型为 Student
   3. 复杂的属性，单独处理，association用于对象，collection用于集合
   4. 学生实体和数据库表的映射关系：学生**实体**中老师的属性为teacher(property)，对应**数据库**中为tid(column)
   5. `javaType`：该关联对象复杂属性在Student实体类中的类型
   6. `select`：关联下一个子查询语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.run.dao.StudentMapper">

    <!--查询所有学生-->
    <select id="getStudents" resultMap="studentAndTeacher">
        select * from mybatis.student
    </select>

    <resultMap id="studentAndTeacher" type="student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--复杂的属性，单独处理，association用于对象，collection用于集合-->
        <association property="teacher" column="tid" javaType="teacher" select="getTeacher"/>
    </resultMap>

    <!--查询学生表中tid所对应的教师-->
    <select id="getTeacher" resultType="teacher">
        select * from mybatis.teacher where id = #{tid}
    </select>
    
</mapper>
```



3、编写完毕去Mybatis配置文件中，注册Mapper！



4、测试

```java
    @Test
    public void StudentTeacher() {
        SqlSession sqlSession = MybatisUtil.getSession(true);
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        List<Student> studentList = mapper.getStudents();

        for (Student student: studentList) {
            System.out.println(student);
        }
    }
```

输出：

```
Student(id=1, name=小明, teacher=Teacher(id=1, name=秦老师))
Student(id=2, name=小红, teacher=Teacher(id=1, name=秦老师))
Student(id=3, name=小张, teacher=Teacher(id=1, name=秦老师))
Student(id=4, name=小李, teacher=Teacher(id=1, name=秦老师))
Student(id=5, name=小王, teacher=Teacher(id=1, name=秦老师))

Process finished with exit code 0
```







#### 8.1.2 按结果嵌套处理

除了上面这种方式，还可以按照结果进行嵌套处理。

1、接口方法编写

```java
public List<Student> getStudents2();
```



2、编写对应的`mapper.xml`文件

```xml
<!--
按查询结果嵌套处理
思路：
    1. 直接查询出结果，进行结果集的映射
-->
<select id="getStudents2" resultMap="StudentTeacher2" >
    select s.id sid, s.name sname , t.name tname
    from student s,teacher t
    where s.tid = t.id
</select>

<resultMap id="StudentTeacher2" type="Student">
    <id property="id" column="sid"/>
    <result property="name" column="sname"/>
    <!--关联对象property 关联对象在Student实体类中的属性-->
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```



3、去mybatis-config文件中注入【此处应该处理过了】



4、测试

```java
@Test
    public void StudentTeacher2() {
        SqlSession sqlSession = MybatisUtil.getSession(true);
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        List<Student> studentList = mapper.getStudents2();

        for (Student student: studentList) {
            System.out.println(student);
        }
    }
```

输出：

```
Student(id=1, name=小明, teacher=Teacher(id=0, name=秦老师))
Student(id=2, name=小红, teacher=Teacher(id=0, name=秦老师))
Student(id=3, name=小张, teacher=Teacher(id=0, name=秦老师))
Student(id=4, name=小李, teacher=Teacher(id=0, name=秦老师))
Student(id=5, name=小王, teacher=Teacher(id=0, name=秦老师))

Process finished with exit code 0
```





> 小结

- 按照**查询进行嵌套**处理就像SQL中的**子查询**

- 按照**结果进行嵌套**处理就像SQL中的**联表查询**





### 8.2 一对多

一对多的理解：

- 一个老师拥有多个学生
- 如果对于老师这边，就是一个一对多的现象，即从一个老师下面拥有一群学生（集合）！

> 实体类编写

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

```java
@Data 
public class Teacher {
    private int id;
    private String name;
    //一个老师多个学生
    private List<Student> students;
}
```

..... 和之前一样，搭建测试的环境！



#### 8.2.1 按结果嵌套处理

1、TeacherMapper接口编写方法

```java
//获取指定老师，及老师下的所有学生
public Teacher getTeacher(int id);
```

2、编写接口对应的`Mapper.xml`配置文件

 思路:

1. 从学生表和老师表中查出学生id，学生姓名，老师姓名
2. 对查询出来的操作做结果集映射
   1. 集合使用collection
   2. JavaType和ofType都是用来指定对象类型的
   3. JavaType是用来指定pojo中属性的类型
   4. ofType指定的是映射到list集合属性中pojo的类型

```xml
<mapper namespace="com.run.dao.TeacherMapper">

    <select id="getTeacher" resultMap="teacherAndStudent">
        select s.id sid, s.name sname, t.name tname, t.id tid
        from mybatis.student s, mybatis.teacher t
        where s.tid = t.id and t.id = #{tid}
    </select>
    <resultMap id="teacherAndStudent" type="teacher">
        <id property="id" column="tid"/>
        <result property="name" column="tname"/>
        <!--在集合的泛型信息中，用ofType-->
        <!--实体类中的students对应的类型为Student-->
        <collection property="students" ofType="student">
            <id property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>
```

3、将Mapper文件注册到`mybatis-config.xml`文件中

```xml
<mappers>
        <mapper class="com.run.dao.TeacherMapper"/>
</mappers>
```

4、测试

```java
public class MapperTest {

    @Test
    public void teacherTest() {
        SqlSession sqlSession = MybatisUtil.getSession(true);
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);

        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher);

        sqlSession.close();
    }
}
```

输出：

```
Teacher(id=1, name=秦老师, students=[Student(id=1, name=小明, tid=1), Student(id=2, name=小红, tid=1), Student(id=3, name=小张, tid=1), Student(id=4, name=小李, tid=1), Student(id=5, name=小王, tid=1)])

Process finished with exit code 0
```





#### 8.2.2 按查询嵌套处理

1、TeacherMapper接口编写方法

```java
public Teacher getTeacher2(int id);
```

2、编写接口对应的Mapper配置文件

```xml
    <select id="getTeacher2" resultMap="teacherAndStudent2">
        select * from mybatis.teacher where id = #{tid}
    </select>
    <resultMap id="teacherAndStudent2" type="teacher">
        <!--column的值为Teacher里的id，传给下面的getStudentByTid-->
        <collection property="students" javaType="ArrayList" ofType="student" column="id" select="getStudentByTid"/>
    </resultMap>
    <select id="getStudentByTid" resultType="student">
        select * from mybatis.student where tid = #{tid}
    </select>
```

3、将Mapper文件注册到`mybatis-config.xml`文件中

4、测试

```java
    @Test
    public void teacherTest2() {
        SqlSession sqlSession = MybatisUtil.getSession(true);
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);

        Teacher teacher = mapper.getTeacher2(1);
        System.out.println(teacher);

        sqlSession.close();
    }
```

输出：

```
Teacher(id=0, name=秦老师, students=[Student(id=1, name=小明, tid=1), Student(id=2, name=小红, tid=1), Student(id=3, name=小张, tid=1), Student(id=4, name=小李, tid=1), Student(id=5, name=小王, tid=1)])

Process finished with exit code 0
```





> 小结

1. 关联-association：处理一对一，多对一

2. 集合-collection：处理一对多

3. JavaType和ofType都是用来指定对象类型的

   - JavaType是用来指定pojo中属性的类型

   - ofType指定的是映射到list集合属性中pojo的类型。泛型中的约束类型

**注意说明：**

1. 保证SQL的可读性，尽量通俗易懂
2. 根据实际要求，尽量编写性能更高的SQL语句
3. 注意属性名和字段不一致的问题
4. 注意一对多和多对一 中：字段和属性对应的问题
5. 尽量使用Log4j，通过日志来查看自己的错误





## 9. 动态SQL

> 介绍

动态SQL：**根据不同的查询条件 , 生成不同的Sql语句.**



我们之前写的 SQL 语句都比较简单，如果有比较复杂的业务，我们需要写复杂的 SQL 语句，往往需要拼接，而拼接 SQL ，稍微不注意，由于引号，空格等缺失可能都会导致错误。

那么怎么去解决这个问题呢？这就要使用 mybatis 动态SQL，通过 if, choose, when, otherwise, trim, where, set, foreach等标签，可组合成非常灵活的SQL语句，从而在提高 SQL 语句的准确性的同时，也大大提高了效率。



> 搭建环境

**新建一个数据库表：blog**

字段：id，title，author，create_time，views

```sql
CREATE TABLE `blog` (
  `id` varchar(50) NOT NULL COMMENT '博客id',
  `title` varchar(100) NOT NULL COMMENT '博客标题',
  `author` varchar(30) NOT NULL COMMENT '博客作者',
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `views` int(30) NOT NULL COMMENT '浏览量'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

1、创建Mybatis基础工程



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251718509.png)



2、IDutil工具类，UUID用于随机生成ID(根据时间来生成的，不会有重复)

```java
public class IDUtil {

    public static String genId(){
        //把随机ID里的 '-' 去掉
        return UUID.randomUUID().toString().replaceAll("-","");
    }
}
```



3、实体类编写  【注意set方法作用】

```java
public class Blog {

    private String id;
    private String title;
    private String author;
    private Date createTime;
    private int views;
    //set，get....
}
```



4、编写Mapper接口及xml文件

```java
public interface BlogMapper {
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.run.dao.BlogMapper">

</mapper>
```



5、`mybatis-config.xml`核心配置文件，开启**下划线-驼峰**自动转换(create_time ==》createTime)

```xml
<settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
<!--注册Mapper.xml-->
<mappers>
  <mapper resource="mapper/BlogMapper.xml"/>
</mappers>
```



6、插入初始数据

编写接口

```java
//新增一个博客
int addBlog(Blog blog);
```

sql配置文件

```xml
<insert id="addBlog" parameterType="blog">
    insert into blog (id, title, author, create_time, views)
    values (#{id},#{title},#{author},#{createTime},#{views});
</insert>
```

初始化博客方法

```java
public class MyTest {
    @Test
    public void addInitBlog() {
        SqlSession sqlSession = MybatisUtil.getSession(true);
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);

        Blog blog = new Blog();
        blog.setId(IDUtil.getId());
        blog.setTitle("Mybatis如此简单");
        blog.setAuthor("狂神说");
        blog.setCreateTime(new Date());
        blog.setViews(9999);

        mapper.addBlog(blog);

        blog.setId(IDUtil.getId());
        blog.setTitle("Java如此简单");
        mapper.addBlog(blog);

        blog.setId(IDUtil.getId());
        blog.setTitle("Spring如此简单");
        mapper.addBlog(blog);

        blog.setId(IDUtil.getId());
        blog.setTitle("微服务如此简单");
        mapper.addBlog(blog);


        sqlSession.close();
    }
}
```

初始化数据完毕！

![image-20220319141319642](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717624.png)





### 9.1 If

**需求：根据作者名字和博客名字来查询博客！如果作者名字为空，那么只根据博客名字查询，反之，则根据作者名来查询**

1、编写接口类

```java
//需求1
List<Blog> queryBlogIf(Map map);
```

2、编写SQL语句

```xml
<!--需求1：
根据作者名字和博客名字来查询博客！
如果作者名字为空，那么只根据博客名字查询，反之，则根据作者名来查询
select * from blog where title = #{title} and author = #{author}
-->
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from blog where
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```

3、测试

```java
@Test
public void testQueryBlogIf(){
    SqlSession session = MybatisUtils.getSession();
    BlogMapper mapper = session.getMapper(BlogMapper.class);

    HashMap<String, String> map = new HashMap<String, String>();
    map.put("title","Mybatis如此简单");
    map.put("author","狂神说");
    List<Blog> blogs = mapper.queryBlogIf(map);

    System.out.println(blogs);

    session.close();
}
```

如果 author 等于 null，那么查询语句为 `select * from user where 1=1 and title=#{title}`，但是如果title为空呢？那么查询语句为 `select * from user where and author=#{author}`，这是错误的 SQL 语句，如何解决呢？请看下面的 where 语句！



### 9.2 Where

修改上面的SQL语句；

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </where>
</select>
```

**这个 “where” 标签会知道如果它包含的标签中有参数传入的话，它就插入一个 ‘where’。**

**此外，如果标签返回的内容是以AND 或OR 开头的，则它会自动剔除掉。**



### 9.3 Set

同理，上面的对于查询 SQL 语句包含 where 关键字，如果在进行**更新操作**的时候，含有 set 关键词，我们怎么处理呢？

1、编写接口方法

```java
int updateBlog(Map map);
```

2、sql配置文件

```xml
<!--注意set是用的逗号隔开-->
<update id="updateBlog" parameterType="map">
    update blog
      <set>
          <if test="title != null">
              title = #{title},
          </if>
          <if test="author != null">
              author = #{author}
          </if>
      </set>
    where id = #{id}
</update>
```

注意：除了最后一个`if`，前面的`if`中在语句末尾加一个`,`，而当最后一句`if`没执行的时候，也不用担心，`set`会自动移除掉最后一个`,`

通过**trim**可以自定义*where* 元素的功能：[Mybatis-trim/set/where](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)



3、测试

```java
@Test
public void testUpdateBlog(){
    SqlSession session = MybatisUtils.getSession();
    BlogMapper mapper = session.getMapper(BlogMapper.class);

    HashMap<String, String> map = new HashMap<String, String>();
    map.put("title","动态SQL");
    map.put("author","秦疆");
    map.put("id","9d6a763f5e1347cebda43e2a32687a77");

    mapper.updateBlog(map);

    session.close();
}
```



### 9.4 Choose、When、Otherwise

有时候，我们不想用到所有的查询条件，只想选择其中的一个，查询条件有一个满足即可，使用 choose 标签可以解决此类问题，类似于 Java 的 switch 语句

1、编写接口方法

```java
List<Blog> queryBlogChoose(Map map);
```

2、sql配置文件

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <choose>
            <when test="title != null">
                 title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```

传入了 “title” 就按 “title” 查找，传入了 “author” 就按 “author” 查找的情形。

若两者都没有传入，就返回 views = #{views}的 BLOG（这个例子中至少要传一个views，在实际开发中，可以将`otherwise`中默认定义一些属性值，这样当没有传入参数时，可以按开发人员默认设定的条件返回）。

**注意：**如果传入的参数有多个，只会选择匹配第一个，然后就退出choose了。



3、测试类

```java
@Test
public void testQueryBlogChoose(){
    SqlSession session = MybatisUtils.getSession();
    BlogMapper mapper = session.getMapper(BlogMapper.class);

    HashMap<String, Object> map = new HashMap<String, Object>();
    map.put("title","Java如此简单");
    map.put("author","狂神说");
    map.put("views",9999);
    List<Blog> blogs = mapper.queryBlogChoose(map);

    System.out.println(blogs);

    session.close();
}
```



### 9.5 SQL片段

有时候可能某个 SQL 语句我们用的特别多，为了增加代码的重用性，简化代码，我们需要将这些**SQL代码抽取出来**，然后使用时**直接调用**。

**提取SQL片段`<sql id="">`：**

```xml
<sql id="if-title-author">
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</sql>
```

**引用SQL片段`<include refid="">`：**

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <!-- 引用 sql 片段，如果refid 指定的不在本文件中，那么需要在前面加上 namespace -->
        <include refid="if-title-author"></include>
        <!-- 在这里还可以引用其他的 sql 片段 -->
    </where>
</select>
```

注意：

1. 最好基于 **单表**来定义 sql 片段，**提高**片段的可**重用性**
2. 在 sql 片段中**不要包括 where**



### 9.6 Foreach

- collection: 指定输入对象中的集合属性
- item: 每次遍历生成的对象
- open: 开始遍历时的拼接字符串
- close: 结束时拼接的字符串
- separator: 遍历对象之间需要拼接的字符串



将数据库中前三个数据的id修改为1,2,3；

需求：我们需要查询 blog 表中 id 分别为1,2,3的博客信息

1、编写接口

```java
List<Blog> queryBlogForeach(Map map);
```

2、编写SQL语句

```xml
<select id="queryBlogForeach" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <!--
        collection: 指定输入对象中的集合属性
        item: 每次遍历生成的对象
        open: 开始遍历时的拼接字符串
        close: 结束时拼接的字符串
        separator: 遍历对象之间需要拼接的字符串

        SQL: select * from blog where 1=1 and (id=1 or id=2 or id=3)
      -->
        
        <foreach collection="ids"  item="id" open="and (" close=")" separator="or">
            id=#{id}
        </foreach>
    </where>
</select>
```

其实就是把SQL: `select * from blog where 1=1 and (id=1 or id=2 or id=3)`拼接起来。

传入的ids集合中，里面有id，就把对应的id填进sql语句中。



3、测试

```java
@Test
public void testQueryBlogForeach(){
    SqlSession session = MybatisUtils.getSession();
    BlogMapper mapper = session.getMapper(BlogMapper.class);
    
    HashMap map = new HashMap();
    List<Integer> ids = new ArrayList<Integer>();
    ids.add(1);
    ids.add(2);
    ids.add(3);
    //相当于Map<String, List<Integer>>
    map.put("ids",ids);

    List<Blog> blogs = mapper.queryBlogForeach(map);

    System.out.println(blogs);

    session.close();
}
```

小结：其实动态 sql 语句的编写往往就是一个拼接的问题

建议：

- 首先要写原生的 sql 语句出来
- 通过 mybatis 动态sql 对照着改

**动态SQL在开发中大量的使用，一定要熟练掌握！**





## 10. 缓存

> 简介

1、什么是缓存 [ Cache ]？

- 存在内存中的临时数据。
- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2、为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率。

3、什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据。



### 10.1 Mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**

- - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存



#### 10.1.1 一级缓存

一级缓存也叫本地缓存：

- 与数据库同一次会话期间查询到的数据会放在本地缓存中。
- 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；



**示例：**

1、在mybatis中加入日志，方便测试结果

2、编写接口方法

```java
//根据id查询用户
User queryUserById(@Param("id") int id);
```

3、接口对应的Mapper文件

```xml
<select id="queryUserById" resultType="user">
    select * from user where id = #{id}
</select>
```

4、测试

```java
@Test
public void testQueryUserById(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);
    User user2 = mapper.queryUserById(1);
    System.out.println(user2);
    System.out.println(user==user2);

    session.close();
}
```

5、结果分析

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717562.png)



> 一级缓存失效的四种情况

一级缓存是SqlSession级别的缓存，是一直开启的，我们关闭不了它；

一级缓存失效情况：没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！

1、sqlSession不同

```java
@Test
public void testQueryUserById(){
    SqlSession session = MybatisUtils.getSession();
    SqlSession session2 = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    UserMapper mapper2 = session2.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);
    User user2 = mapper2.queryUserById(1);
    System.out.println(user2);
    System.out.println(user==user2);

    session.close();
    session2.close();
}
```

观察结果：发现发送了两条SQL语句！

结论：**每个sqlSession中的缓存相互独立**

2、sqlSession相同，查询条件不同

```java
@Test
public void testQueryUserById(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    UserMapper mapper2 = session.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);
    User user2 = mapper2.queryUserById(2);
    System.out.println(user2);
    System.out.println(user==user2);

    session.close();
}
```

观察结果：发现发送了两条SQL语句！很正常的理解

结论：**当前缓存中，不存在这个数据**

3、sqlSession相同，两次查询之间执行了增删改操作！

增加方法

```java
//修改用户
int updateUser(Map map);
```

编写SQL

```xml
<update id="updateUser" parameterType="map">
    update user set name = #{name} where id = #{id}
</update>
```

测试

```java
@Test
public void testQueryUserById(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);

    HashMap map = new HashMap();
    map.put("name","kuangshen");
    map.put("id",4);
    mapper.updateUser(map);

    User user2 = mapper.queryUserById(1);
    System.out.println(user2);

    System.out.println(user==user2);

    session.close();
}
```

观察结果：查询在中间执行了增删改操作后，重新执行了

结论：**因为增删改操作可能会对当前数据产生影响**

4、sqlSession相同，手动清除一级缓存

```java
@Test
public void testQueryUserById(){
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);

    session.clearCache();//手动清除缓存

    User user2 = mapper.queryUserById(1);
    System.out.println(user2);

    System.out.println(user==user2);

    session.close();
}
```

一级缓存其实就是一个map





#### 10.1.2 二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存

- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；

- 工作机制

- - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；



> 使用步骤

1、开启全局缓存 【`mybatis-config.xml`】

```xml
<setting name="cacheEnabled" value="true"/>
```

2、去每个mapper.xml中配置使用二级缓存`<cache/>`，这个配置非常简单；【`xxxMapper.xml`】

```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。



3、代码测试

- 所有的实体类先实现序列化接口
- 测试代码

```java
@Test
public void testQueryUserById(){
    SqlSession session = MybatisUtils.getSession();
    SqlSession session2 = MybatisUtils.getSession();

    UserMapper mapper = session.getMapper(UserMapper.class);
    UserMapper mapper2 = session2.getMapper(UserMapper.class);

    User user = mapper.queryUserById(1);
    System.out.println(user);
    //先关闭会话，只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中
    session.close();

    User user2 = mapper2.queryUserById(1);
    System.out.println(user2);
    
    System.out.println(user==user2);

    session2.close();
}
```

![image-20220319193828718](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717625.png)

可以看到，使用了二级缓存后，即使是不同的sqlsession创建的mapper对象，去执行相同的sql语句，在第二次执行的时候是到缓存中去取结果的！





> 结论

- 只要开启了二级缓存，在**同一个Mapper**(上述例子都是取`UserMapper.class`)中的查询，可以在二级缓存中拿到数据
- 查出的所有数据都会被默认先放在一级缓存中
- 只有会话**提交**或者**关闭**以后，**一级缓存中的数据才会转到二级缓存中**





### 10.2 缓存原理图

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717334.png)

**执行顺序：**

1. 先判断二级缓存是否开启，如果没开启，再判断一级缓存是否开启，如果没开启，直接查数据库
2. 如果一级缓存关闭，即使二级缓存开启也没有数据，因为二级缓存的数据从一级缓存获取
3. 一般不会关闭一级缓存
4. 二级缓存默认不开启
5. 如果二级缓存关闭，直接判断一级缓存是否有数据，如果没有就查数据库
6. 如果二级缓存开启，先判断二级缓存有没有数据，如果有就直接返回；如果没有，就查询一级缓存，如果有就返回，没有就查询数据库

**综上：先查二级缓存，再查一级缓存，再查数据库；即使在一个sqlSession中，也会先查二级缓存；一个namespace中的查询更是如此**





### 10.3 EhCache

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251717330.png)

第三方缓存实现--EhCache: 查看百度百科

Ehcache是一种广泛使用的Java分布式缓存，用于通用缓存；

要在应用程序中使用Ehcache，需要引入依赖的jar包

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.1.0</version>
</dependency>
```

在mapper.xml中使用对应的缓存即可

```xml
<mapper namespace = “com.kuang.mapper.BlogMapper” > 
    <cache type = “org.mybatis.caches.ehcache.EhcacheCache” /> 
</mapper>
```

编写ehcache.xml文件，如果在加载时未找到/ehcache.xml资源或出现问题，则将使用默认配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
       user.home – 用户主目录
       user.dir  – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
     -->
    <diskStore path="./tmpdir/Tmp_EhCache"/>
    
    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>
 
    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
    <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
     -->
    <!--
      name:缓存名称。
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统当机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->

</ehcache>
```

EhCache了解即可，在后面会使用redis来处理缓存

合理的使用缓存，可以让我们程序的性能大大提升！























