# 1. MyBatisPlus概述

为什么学习它呢？MyBatisPlus可以节省我们大量工作时间，所有的CRUD代码他都可以自动化完成！

> 简介

是什么？MyBatis本来就是简化JDBC操作的！

官网：[MyBatisPlus](https://gitee.com/link?target=http%3A%2F%2Fwww.baidu.com%2Flink%3Furl%3DsHv0yNgJhx5GEetCQD7yRhsGVpOB8BOEqSNJNfkli314dBoOLi3Zoh4VKhJd6wtI%26wd%3D%26eqid%3D8a9f23960000405d0000000460b23bf1) ，简化Mybaits！ ![image-20210529210623909](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/513878b99165f98f0f8852e528d91bb2.png)

------

![image-20210529210748312](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4c9ce0a33c52b7a427fed74c9a634c14.png)

> 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作，BaseMapper
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求，以后简单的CRUD操作，不用自己编写了 ！
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用（自动帮你生成代码）
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

# 2. 快速入门

地址：[快速开始](https://gitee.com/link?target=https%3A%2F%2Fmp.baomidou.com%2Fguide%2Fquick-start.html%23%E5%88%9D%E5%A7%8B%E5%8C%96%E5%B7%A5%E7%A8%8B)

使用第三方组件：

1、导入对应的依赖

2、研究依赖如何配置

3、代码如何编写

4、提高扩展技术能力！

> 步骤

1、创建数据库 `mybatis_plus`

2、创建`user`表

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);

DELETE FROM USER;

INSERT INTO USER (id, NAME, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');

-- 真实开发中，version（乐观锁）、delete（逻辑删除）、gmt_create、gmt_modified
```

3、编写项目，初始化项目！使用`SpringBoot`初始化

4、导入依赖

```xml
<!-- 数据库驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<!-- lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
<!-- mybatis-plus --> 
<!-- mybatis-plus 是自己开发，并非官方的！ -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.0</version>
</dependency>
```

说明：我们使用Mybatis-plus可以节省我们大量的代码，尽量不要同时导入 mybatis 和 mybatis-plus！版本的差异！

5、连接数据库！这一步和mybatis相同！

```properties
# mysql 8 驱动不同com.mysql.cj.jdbc.Driver、需要增加时区的配置 serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

6、传统方式：pojo -> dao(连接mybatis，配置mapper.xml文件) -> service -> controller

6、使用了mybatis-plus 之后

- pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

- mapper，继承了BaseMapper，所有的方法都来自父类我们也可以编写自己的扩展方法

```java
//在对应的mapper上面继承基本的接口BaseMapper
@ResponseBody 		//代表持久层
public interface UserMapper extends BaseMapper<User> {
    //所有的CRUD操作都已经编写完成了
    //你不需要像以前的配置一大堆文件了
}
```

- 注意点，我们需要再主启动类上去扫描我们的mapper包下的所有接口：

```java
@MapperScan("com.run.mapper")   //扫描mapper包下的接口
@SpringBootApplication
public class MyBatisPlusApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyBatisPlusApplication.class, args);
    }

}
```

- 测试类中测试

```java
@SpringBootTest
class MybatisPlusApplicationTests {
    
    @Autowired
    private UserMapper userMapper;

    @Test
    void contextLoads() {
        //参数是一个Wrapper，条件构造器，这里我们先不用 null
        //查询全部用户
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }
}
```

- 结果 

![image-20220405103204342](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/80f9521a751e50dc88b1764cb11841f0.png)

# 3. 配置日志

现在所有的sql都是不可见的，我们希望知道他是怎么执行的，所以我们必须要看日志！

```properties
# 配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

![image-20220405103715685](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/03b49712d46bf8f4ee0aa639b8929014.png)

配置完毕日志之后，后面的学习就需要注意这个自动生成的SQL，就会喜欢上Mybatis-Plus

# 4. 扩展

## 4.1 插入操作

> Insert插入

```java
// 测试插入
@Test
public void testInsert(){
    User user = new User();
    user.setName("张三");
    user.setAge(23);
    user.setEmail("1622840727@qq.com");

    int result = userMapper.insert(user);// 帮我们自动生成id
    System.out.println(result); // 受影响的行数
    System.out.println(user); // 发现，id会自动回填
}
```

![image-20220405104047754](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c61417e09de2eced2e442aff68ed3b93.png)

**数据库插入的id默认值为：全局的唯一id**

## 4.2 主键生成策略

> 默认：ID_WORKER全局唯一id

分布式系统唯一id生成方案：[https://www.cnblogs.com/haoxinyue/p/5208136.html](https://gitee.com/link?target=https%3A%2F%2Fwww.cnblogs.com%2Fhaoxinyue%2Fp%2F5208136.html)

**雪花算法：**

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！

> 主键自增

配置主键自增：

1、实体类自增的字段上加上`@TableId(type = IdType.AUTO)`

2、数据库字段一定要是自增的！ 

![image-20220405104704219](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c0f3a6a20e0640ecc8cc878052d78688.png)

3、再次测试插入，发现id是自增的

![image-20220405105031208](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ec2e9b5d7158e38e6e448b1254190bf2.png)

> 其他的源码解释

```java
public enum IdType {
    AUTO(0),	//数据库id自增
    NONE(1),	//未设置主键
    INPUT(2),	//手动输入
    ASSIGN_ID(3),	//默认的全局唯一id
    ASSIGN_UUID(4),	//全局唯一id UUID
    ID_WORKER_STR(5); //ID_WORKER 字符串表示法
}
```

## 4.3 更新操作

数据库中的数据：

![image-20220405105535505](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e1cb8a869c95b655eb5718157266638c.png)

```java
    //测试更新
    @Test
    void testUpdate() {
        User user = new User();
        //会通过条件自动拼接SQL
        user.setId(1L);     //Id为long型
        user.setName("Gaga");
        user.setAge(3);
        user.setEmail("123123@163.com");
        //注意：updateById 参数是一个对象
        int i = userMapper.updateById(user);
        System.out.println(i);

    }
```

![image-20220405105937833](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/555a576d7602255eece936b42af785fd.png)

![image-20220405110119386](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/139f432f34ccd60461853db6061784c6.png)



根据传入的参数个数，所有的SQL都会自动帮你**动态配置**的！

## 4.4 自动填充

创建时间、修改时间！这些个操作都是自动化完成的，我们不希望手动更新！

阿里巴巴开发手册：所有的数据库表：gmt_create、gmr_modified、几乎所有的表都要配置上！而且需要自动化！

> 方式一：数据库级别（工作中不允许修改数据库）

1、在表中新增字段 `create_time`, `update_time` ![image-20210530151406140](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dbc3be89ec2ebf750331f4e05aa3ff71.png) 2、再次测试插入方法，我们需要先把实体类同步！

```java
private Date createTime;
private Date updateTime;
```

3、再次更新查看结果即可 ![image-20210530151415388](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4a6cb89da7152d85d51f0e9084b4fc8f.png)

> 方式二：代码级别

1、将`create_time`, `update_time` 设置成原始样式

2、实体类的字段属性上需要增加注解

```java
//字段添加填充内容
@TableField(fill = FieldFill.INSERT)
private LocalDateTime createTime;
@TableField(fill = FieldFill.INSERT_UPDATE)		//插入和更新时填充该字段
private LocalDateTime updateTime;
```

3、编写处理器处理注解即可！

```java
@Component  	//一定不要忘记把处理器加到IOC容器中
@Slf4j
public class MyMetaObjectHandler implements MetaObjectHandler {
    //插入时候的填充策略
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ......");
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());
        this.strictInsertFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
    }

    //更新时候的填充策略
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ......");
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
    }
}
```

4、测试插入

![image-20220405120604833](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f6c2f5f89c3ee246907bc0031bb7bee3.png)

5、测试更新、观察时间即可！

![image-20220405120704824](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3d049cddb62f39036fd61ef680d33b37.png)

**注意**：实体类的日期字段类型必须和`this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());`参数的类型一样；这里参数使用`LocalDateTime`，实体类字段的类型也要为`LocalDateTime`，不能为`Date`，否则会自动填充失败！！！

## 4.5. 乐观锁

> 乐观锁：顾名思义十分乐观，他总是认为不会出现问题，无论干什么不去上锁！如果出现了问题，再次更新值测试！
>
> 悲观锁：顾名思义十分悲观，他总是任务总是出现问题，无论干什么都会上锁！再去操作！

乐观锁实现方式：

- 取出记录，获取当前version
- 更新时，带上这个version
- 执行更新时，`set version = new version where version = oldversion`
- 如果version不对，就更新失败

```sql
乐观锁：先查询，获得版本号 version = 1
-- A 线程正在执行此操作
update user set name = "ChanV", version = version + 1
where id = 2 and version = 1

-- B 线程此时抢先完成，这个时候 version = 2，会导致 A 修改失败；避免了风险
update user set name = "ChanV", version = version + 1
where id = 2 and version = 1
```

> 测试一下MP(MybatisPlus)的乐观锁插件

1、给数据库中增加version字段，默认值为1

![image-20220405122126773](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/be44a89b5c030141a62e13380842e678.png)

 2、我们实体类加对应的字段

```java
@Version    //乐观锁version注解
private Integer version;
```

3、注册组件

```java
//扫描我们的mapper文件夹
@MapperScan("com.run.mapper")
@EnableTransactionManagement
@Configuration  //配置类
public class MyBatisPlusConfig {
    //注册乐观锁插件
    @Bean
    public OptimisticLockerInnerInterceptor optimisticLockerInnerInterceptor() {
        return new OptimisticLockerInnerInterceptor();
    }
}
```

4、测试乐观锁成功的情况，version从1变到2了：

```java
    //测试乐观锁--成功
    @Test
    void testOptimisticLocker() {
        //1.查询用户信息
        User user = userMapper.selectById(1L);
        //2.修改用户信息
        user.setName("狂神");
        user.setEmail("abc111@163.com");
        //3.执行更新
        userMapper.updateById(user);
    }
```

![image-20220405123342108](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/49361e931a805ebd620ae2c93c6de1c6.png)

5、测试乐观锁失败的情况，可以发现，name的值是插队线程的值，线程1的更新操作并没有执行成功，version只自增了一次！

```java
    //测试乐观锁--失败；多线程下
    @Test
    void testOptimisticLocker2() {
        //线程1
        User user = userMapper.selectById(1L);
        user.setName("kuangshen111");
        //模拟另一个线程执行了插队操作
        User user2 = userMapper.selectById(1L);
        user2.setName("kuangshen222");
        //插队的线程先执行更新
        userMapper.updateById(user2);
        userMapper.updateById(user);     //如果没有乐观锁，就会覆盖插队线程的值
    }
```

![image-20220405124147952](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/45aa6a0cbd8057c658479300e71441a1.png)

我们会发现，我们明明执行了更新操作，但是`update_time`并没有更新，这是为什么？

因为这里我们是先通过`selectById`获取该用户的，所以需要修改的字段本身有值，`this.strictInsertFill()`严格模式下，有值的字段是不进行填充修改的，需要自己实现`strictFillStrategy`接口才可以实现

而上面的update操作是先new一个user对象出来，是没有设置`update_time`的值的，所以执行更新操作的时候可以将时间更新。



**注意**：

- **支持的数据类型只有:int,Integer,long,Long,Date,Timestamp,LocalDateTime**
- 整数类型下 `newVersion = oldVersion + 1`
- `newVersion` 会回写到 `entity` 中
- 仅支持 `updateById(id)` 与 `update(entity, wrapper)` 方法
- **在 `update(entity, wrapper)` 方法下, `wrapper` 不能复用!!!**

## 4.6. 查询操作

```java
//测试按id查询
@Test
public void testSelectById(){
    User user = userMapper.selectById(1L);
    System.out.println(user);
}

// 测试批量查询！
@Test
public void testSelectByBatchId(){
    List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
    users.forEach(System.out::println);
}

// 按条件查询之一 使用map操作
@Test
public void testSelectBatchIds(){
    HashMap<String,Object> map = new HashMap<>();
    // 自定义要查询的内容
    map.put("name","Tom");
    map.put("age",28);

    List<User> users = userMapper.selectByMap(map);
    users.forEach(System.out::println);
}
```

## 4.7. 分页查询

分页在网站使用的十分之多！

1、原始的 limit 进行分页

2、pageHelper 第三方插件

3、MP其实也内置了分页插件

> 如何使用？

1、在配置类中添加拦截器组件即可

```java
@MapperScan("com.run.mapper")   //扫描我们的mapper文件夹
@EnableTransactionManagement
@Configuration      //配置类
public class MyBatisPlusConfig {
    //注册乐观锁插件，分页插件
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        //添加乐观锁拦截器
        mybatisPlusInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        //添加分页拦截器
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
        return mybatisPlusInterceptor;
    }

}
```

2、直接使用Page对象即可！

```java
    //测试分页查询
    @Test
    void testPage() {
        //参数1：当前页
        //参数2：页面大小
        IPage<User> iPage = new Page<>(1, 3);
        userMapper.selectPage(iPage, null);
        //将查询到的结果输出一下
        iPage.getRecords().forEach(System.out::println);
        //获取总数
        System.out.println(iPage.getTotal());
    }
```

查询第1页：

![image-20220405131814563](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6bee0d79e7317d9caa2b94641c46ce5f.png)

查询第二页：

![image-20220405131931247](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/479bfe354d00883fe1ea7afdcc9c537d.png)

## 4.8 删除操作

基本的删除操作

```java
//测试删除
@Test
public void testDeleteById(){
    userMapper.deleteById(1L);
}

//通过id批量删除
@Test
public void testDeleteBatchId(){
    userMapper.deleteBatchIds(Arrays.asList(2, 3, 4));
}

//通过map删除
@Test
public void testDeleteMap(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name", "陈伟");
    userMapper.deleteByMap(map);
}
```

在工作中会遇到一些问题：逻辑删除！

## 4.9. 逻辑删除

> 物理删除：从数据库中直接移除
>
> 逻辑删除：在数据库中没有被移除，而是通过一个字段变量来让他失效！deleted = 0 -> deleted = 1

管理员可以查看被删除的记录！防止数据的丢失，类似于回收站!

测试一下：

1、在数据表中增加一个deleted字段，默认值为0

![image-20220405133520232](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/071d05cb3f79dc70fd6d25216189361e.png)

 2、实体类中增加属性

```java
private Integer deleted;
```

3、在`application.yml`中配置！

```yaml
# 逻辑删除
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: deleted 	# 全局逻辑删除的实体字段名
      logic-delete-value: 1 	# 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 	# 逻辑未删除值(默认为 0)
```

4、测试一下删除！

```java
//测试删除
@Test
public void testDeleteById(){
    userMapper.deleteById(1L);
}
```

可以看到，实际执行的是update操作，并不是delete

![image-20220405134219004](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7c558d9a21aea9a93c83ea3f38f9a581.png)

记录依旧在数据库，但是deleted值确已经变化了

![image-20220405134257895](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/657f78fbc9e116d19d5e269b195e295c.png)

5、测试一下查找，可以发现，查找时会自动带上条件`AND deleted=0`，过滤掉已经被逻辑删除的用户

```java
//测试查找
@Test
public void testSelectById(){
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
```

![image-20220405134628767](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/aec57e016797af93412c005eb449fd5f.png)

以上的所有CRUD操作及其扩展操作，我们都必须精通掌握！会大大提高工作和写项目的效率！

## 4.10  条件构造器

条件构造器**Wappper**十分重要，官网：https://baomidou.com/pages/10c804/

我们写一些复杂的SQL就可以使用他来替代！ 

![image-20220405135752081](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/52cf5125d2ca47684325ae4fee4e136b.png)

 1、测试一

```java
@Test
void contextLoads() {
    //查询name，邮箱不为空的用户，年龄>=14
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.isNotNull("name")
        .isNotNull("email")
        .ge("age", 12);
    userMapper.selectList(wrapper).forEach(System.out::println); //和刚刚学习的map对比一下
}
```

2、测试二

```java
@Test
void test1() {
    //查询名字为狂神的用户，没有返回null；注意：如果表中有多个狂神，会报错
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.eq("name", "狂神");
    User user = userMapper.selectOne(wrapper);
    System.out.println(user);
}
```

3、测试三

```java
@Test
void test3(){
    //查询年龄在19到30岁之间的用户
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.between("age", 19, 30); //区间
    Integer count = userMapper.selectCount(wrapper);
    System.out.println(count);
}
```

4、测试四

```java
//模糊查询
@Test
void test4(){
    //查询年龄在19到30岁之间的用户
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //左和右
    wrapper.notLike("name", "b")
            .likeRight("email", "t");
    List<Map<String, Object>> maps = userMapper.selectMaps(wrapper);
    maps.forEach(System.out::println);
}
```

5、测试五

```java
@Test
void test5(){
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //id 在子查询中查出来
    wrapper.inSql("id", "select id from user where id < 3");
    List<Object> objects = userMapper.selectObjs(wrapper);
    objects.forEach(System.out::println);
}
```

6、测试六

```java
@Test
void test6(){
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //通过id进行降序排序
    wrapper.orderByDesc("id");
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}
```

## 4.11 代码自动生成器

dao、pojo、service、controller都可以自动编写完成！

`AutoGenerator` 是 MyBatis-Plus 的代码生成器，通过 `AutoGenerator` 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

导入相关依赖：

```xml
<!--注意：需要和mybatis-plus-boot-starter依赖同版本-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.0</version>
</dependency>
```

测试：

```java
public class Code {
    public static void main(String[] args) {
        //需要构建一个 代码自动生成器 对象
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();
        //配置策略

        //1、全局配置
        //注意导包: com.baomidou.mybatisplus.generator.config.GlobalConfig;
        GlobalConfig g = new GlobalConfig();
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");    //获取用户的当前项目目录
        gc.setOutputDir(projectPath + "/src/main/java");    //代码生成的位置
        gc.setAuthor("AruNi");
        gc.setOpen(false);
        gc.setFileOverride(false); 	 	//是否覆盖
        gc.setServiceName("%sService"); 	//去Service的I前缀
        gc.setIdType(IdType.ID_WORKER);
        gc.setDateType(DateType.ONLY_DATE);
        gc.setSwagger2(true);       //swagger自动生成，需要导入swagger的依赖
        mpg.setGlobalConfig(gc);

        //2、设置数据源
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("123456");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);

        //3、包的配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("auto");	//自动生成的代码在这个包里面，可以不写
        pc.setParent("com.run");
        pc.setEntity("pojo");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setController("controller");
        mpg.setPackageInfo(pc);

        //4、策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setInclude("user");    	//设置要映射的表名，可以选择多个表
        //下划线转驼峰
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setEntityLombokModel(true);    //自动lombok
        strategy.setLogicDeleteFieldName("deleted");

        //自动填充配置
        TableFill createTime = new TableFill("create_time", FieldFill.INSERT);
        TableFill updateTime = new TableFill("update_time", FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> tableFills = new ArrayList<>();
        tableFills.add(createTime);
        tableFills.add(updateTime);

        strategy.setTableFillList(tableFills);

        //乐观锁
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);
        strategy.setControllerMappingHyphenStyle(true);     //url下划线命名：localhost:8080/hello_id_2
        mpg.setStrategy(strategy);

        mpg.execute();  //执行代码构造器
    }
}
```

直接执行方法：

![image-20220405154016565](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/00fe409a503c56b6bb6b893f968a7de2.png)



`User.java`实体类：

```java
@Data
  @EqualsAndHashCode(callSuper = false)
    @ApiModel(value="User对象", description="")
public class User implements Serializable {

    //序列化
    private static final long serialVersionUID = 1L;

      @ApiModelProperty(value = "主键ID")
        @TableId(value = "id", type = IdType.AUTO)
      private Long id;

      @ApiModelProperty(value = "姓名")
      private String name;

      @ApiModelProperty(value = "年龄")
      private Integer age;

      @ApiModelProperty(value = "邮箱")
      private String email;

      @TableField(fill = FieldFill.INSERT)
      private Date createTime;

      @TableField(fill = FieldFill.INSERT_UPDATE)
      private Date updateTime;

      @ApiModelProperty(value = "乐观锁")
      @Version
    private Integer version;

      @ApiModelProperty(value = "默认为0，删除后为1")
      @TableLogic
    private Integer deleted;

}
```



service层也可以像mapper层一样，使用MP自动生成的。只需要在接口中继承`IService<XXX>`，在实现类也要继承相应的`ServiceImpl<XxxMapper, XXX>`。

例如，我们上面自动生成的service层：

`UserService`接口：

```java
public interface UserService extends IService<User> {
}
```

`UserServiceImpl`实现类：

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
}
```

此时就不用再去写service层调用mapper层的增删改查的方法了！









