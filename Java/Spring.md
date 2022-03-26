# Spring

## 1. Spring概述

![image-20220321222854686](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251737260.png)

### 1.1 简介

Spring : 春天 ---> 给软件行业带来了春天

2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。

2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

很难想象Rod Johnson的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。

Spring理念 : 使现有技术更加实用，本身就是一个大杂烩，整合现有的框架技术

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects



### 1.2 优点

1、Spring是一个开源免费的框架, 容器  

2、Spring是一个轻量级的框架, 非侵入式的 

**3、控制反转 IoC  , 面向切面 AOP**

4、对事物的支持 , 对框架整合的支持

.......



一句话概括：

**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**



### 1.3 组成

![Spring主要模块](https://images.xiaozhuanlan.com/photo/2019/e0c60b4606711fc4a0b6faf03230247a.png)

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .



![image-20220321223150304](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251737262.png)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **Spring Core核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用*控制反转*（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring Context 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。



### 1.4 拓展

**Spring Boot与Spring Cloud**

- Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务;
- Spring Cloud是基于Spring Boot实现的；
- Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；
- Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 , Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。

![image-20220321223053184](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251737263.png)





## 2. IoC

**IoC（Inverse of Control:控制反转）** 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。不过， IoC 并非 Spring 特有，在其他语言中也有应用。

**为什么叫控制反转？**

- **控制** ：指的是对象创建（实例化、管理）的权力
- **反转** ：控制权交给外部环境（Spring 框架、IoC 容器）



### 2.1 IoC理论推导

新建一个空白的maven项目

> 分析实现

我们先用我们原来的方式写一段代码 .

1、先写一个UserDao接口

```java
public interface UserDao {
    public void getUser();
}
```

2、再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("获取用户数据");
    }
}
```

3、然后去写UserService的接口

```java
public interface UserService {
    public void getUser();
}
```

4、最后写Service的实现类

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao = new UserDaoImpl();

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```

5、测试一下

```java
@Test
public void test(){
    UserService service = new UserServiceImpl();
    service.getUser();
}
```

这是我们原来的方式 , 那我们现在修改一下。

把Userdao的实现类增加一个

```java
public class UserDaoMySqlImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("MySql获取用户数据");
    }
}
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao = new UserDaoMySqlImpl();

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```

在假设, 我们再增加一个Userdao的实现类 .

```java
public class UserDaoOracleImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("Oracle获取用户数据");
    }
}
```

那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现。假设我们的这种需求非常大, 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码。这种设计的耦合性太高了, 牵一发而动全身。



**那我们如何去解决呢 ?** 

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用**set进行动态实现值的注入**！

我们去代码里修改下

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao;
    // 利用set实现
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```

现在去我们的测试类里 , 进行测试 ;

```java
@Test
public void test(){
    UserServiceImpl service = new UserServiceImpl();
    service.setUserDao( new UserDaoMySqlImpl() );
    service.getUser();
    //那我们现在又想用Oracle去实现呢
    service.setUserDao( new UserDaoOracleImpl() );
    service.getUser();
}
```

大家发现了区别没有 ? 可能很多人说没啥区别。但是同学们 , 他们已经发生了根本性的变化，很多地方都不一样了。仔细去思考一下，以前所有东西都是由程序去进行控制创建，而现在是由我们自行控制创建对象 , 把主动权交给了调用者 . 程序不用去管怎么创建，怎么实现了。它只负责提供一个接口 。

这种思想 , 从本质上解决了问题 ，我们程序员**不再去管理对象的创建**了，更多的去关注业务的实现。耦合性大大降低 ，这也就是IoC的原型 !





#### 2.1.1 IoC本质

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。

没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，所谓控制反转就是：获得依赖对象的方式反转了。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251738286.png)

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。



Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![查看源图像](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251738036.png)

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection, DI）**





## 3. 快速上手Spring

### 3.1 Hello Spring

> 导入Jar包

注 : spring 需要导入commons-logging进行日志记录。我们利用maven , 他会自动下载对应的依赖项

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.10.RELEASE</version>
</dependency>
```



> 编写代码

1、编写一个Hello实体类

```java
public class Hello {
    private String str;

    public String getStr() {
        return str;
    }
    public void setStr(String str) {
        this.str = str;
    }

    public void show(){
        System.out.println("Hello, "+ str);
    }
}
```

2、编写我们的spring文件 , 这里我们命名为beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--bean就是Java对象 , 由Spring创建和管理-->
    <!--
	Java中：
	类型 	  变量名   =      new	类型();
	Hello    hello	=	new	  Hello();

	Spring中：
	id = 变量名
	class = new的对象
	property：相当于给对象中的属性赋值
    -->
    <bean id="hello" class="com.run.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>

</beans>
```

3、我们可以去进行测试了 .

```java
public class MyTest {
    public static void main(String[] args) {
        //获取Spring的上下文对象，解析beans.xml文件，生成管理相应的Bean对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在Spring中管理了，我们要使用直接去里面取即可
        //getBean：参数是spring配置文件中bean的id
        Hello hello = (Hello) context.getBean("hello");
        hello.show();
    }
}
```

输出：

```
Hello, Spring

Process finished with exit code 0
```



> 思考

- Hello 对象是谁创建的 ?  
  - hello 对象是由Spring创建的
- Hello 对象的属性是怎么设置的 ?  
  - hello 对象的属性是由Spring容器设置的



这个过程就叫控制反转 :

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
- 反转 : 程序本身不创建对象 , 而变成被动的接收对象

**依赖注入：就是利用set方法来进行注入的，`beans.xml文件中property标签设置值的时候就调用了setStr()方法`**

IoC是一种编程思想，由主动的编程变成被动的接收

可以通过newClassPathXmlApplicationContext去浏览一下底层源码



> 修改之前的案例一

![image-20220321180621889](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251737264.png)

我们在案例一中， 新增一个Spring配置文件`beans.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="MysqlImpl" class="com.kuang.dao.impl.UserDaoMySqlImpl"/>
    <bean id="OracleImpl" class="com.kuang.dao.impl.UserDaoOracleImpl"/>
    <bean id="SqlserverImpl" class="com.kuang.dao.impl.UserDaoSqlserverImpl"/>
    
    <bean id="ServiceImpl" class="com.kuang.service.impl.UserServiceImpl">
        <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写-->
        <!--引用另外一个bean , 不是用value 而是用 ref-->
        <property name="userDao" ref="OracleImpl"/>
    </bean>

</beans>
```

测试！

```java
@Test
public void test2(){
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    //需要什么，就getBean什么
    UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("ServiceImpl");
    serviceImpl.getUser();
}
```

OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 例如，需要Mysql的对象，在`property`中修改`ref = "MysqlImpl"`即可；需要添加一个Sqlserver的对象时，创建实体类后直接在配置文件中添加一个bean即可。

所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 ! 





### 3.2 IoC创建对象方式

#### 3.2.1 无参构造方法(默认)

1、`User.java`

```java
public class User {

    private String name;

    public User() {
        System.out.println("user无参构造方法");
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("name="+ name );
    }

}
```

2、`beans.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.kuang.pojo.User">
        <property name="name" value="kuangshen"/>
    </bean>

</beans>
```

3、测试类

```java
@Test
public void test(){
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    //在执行getBean的时候, user已经创建好了 , 通过无参构造
    User user = (User) context.getBean("user");
    //调用对象的方法 .
    user.show();
}
```

输出：

```
user无参构造方法
name=kuangshen

Process finished with exit code 0
```

结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！





#### 3.2.2 有参构造方法

1、`UserT.java`

```java
public class UserT {

    private String name;

    public UserT(String name) {
        this.name = name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("name="+ name );
    }

}
```



2、`beans.xml` 有三种方式编写

```xml
<!-- 第一种根据index参数下标设置 -->
<bean id="userT" class="com.kuang.pojo.UserT">
    <!-- index指构造方法 , 下标从0开始 -->
    <constructor-arg index="0" value="kuangshen2"/>
</bean>

<!-- 第二种根据参数名字设置 -->
<bean id="userT" class="com.kuang.pojo.UserT">
    <!-- name指参数名 -->
    <constructor-arg name="name" value="kuangshen2"/>
</bean>

<!-- 第三种根据参数类型设置-->
<bean id="userT" class="com.kuang.pojo.UserT">
    <constructor-arg type="java.lang.String" value="kuangshen2"/>
</bean>
```



3、测试

```java
@Test
public void testT(){
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    UserT user = (UserT) context.getBean("userT");
    user.show();
}
```

结论：在配置文件加载的时候。其中管理的对象都已经初始化了！



### 3.3 Spring配置

> alias 别名

alias 设置别名，区分大小写。 为bean设置别名 , 可以设置多个别名

```xml
<!--设置别名：在获取Bean的时候可以使用别名获取-->
<alias name="userT" alias="userNew"/>
```



> bean

- **id**：是bean的标识符，要唯一，如果没有配置id，name就是默认标识符 
  - 如果配置id，又配置了name，那么name是别名
- **name**：可以设置多个别名,可以用逗号, 分号, 空格隔开
- 如果不配置id和name,可以根据`applicationContext.getBean(.class)`获取对象;
- **class**：是bean的全限定名=包名+类名

```xml
<!--bean就是java对象,由Spring创建和管理-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.kuang.pojo.Hello">
    <property name="name" value="Spring"/>
</bean>
```



> import

团队的合作通过import来实现，它可以将多个配置文件，导入合成一个，在这个配置文件中，可以获取所有的bean。

```xml
<import resource="{path}/beans.xml"/>
```





## 4. 依赖注入（DI）

> 概念

- 依赖注入（Dependency Injection, DI）。
- 依赖：指Bean对象的创建依赖于容器，Bean对象的依赖资源
- 注入：指Bean对象所依赖的资源 , 由容器来设置和装配



**IoC和DI的理解：**

如果一个类A的功能实现需要借助于类B，那么就称类B是类A的依赖，如果在类A的内部去实例化类B，那么两者之间会出现较高的耦合， 一旦类B出现了问题，类A也需要进行改造,如果这样的情况较多，每个类之间都有很多依赖，那么就会出现牵一发而动全身的情况，程序会极难维护，组很容易出现问题。要解决这个问题，就要把A类对B类的控制权抽离出来，交给一个第三方去做，把控制权反转给第三方，就称作**控制反转**(IoC Inversion of Control)。

控制反转是一种思想，是能够解决问题的一种可能的结果，而**依赖注入**(Dependency Injection)就是其最典型的**实现方法**。由第三方(我们称作IoC容器)来控制依赖，把他通过构造函数、属性或者工厂模式等方法，注入到类A内，这样就极大程度的对类A和类B进行了解耦。



### 4.1 构造器注入

之前的案例已经讲过了



### 4.2 Set 注入（重点）

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 isXX开头

测试pojo类：

复杂类型：`Address.java`

```java
 public class Address {
 
     private String address;
 
     public String getAddress() {
         return address;
     }
 
     public void setAddress(String address) {
         this.address = address;
     }
 }
```

真实对象：`Student.java`

```java
 package com.kuang.pojo;
 
 import java.util.List;
 import java.util.Map;
 import java.util.Properties;
 import java.util.Set;
 
 public class Student {
 
     private String name;
     private Address address;
     private String[] books;
     private List<String> hobbys;
     private Map<String,String> card;
     private Set<String> games;
     private String wife;
     private Properties info;
 
     //get/set方法.....
 
     public void show(){
         System.out.println("name="+ name
                 + ",address="+ address.getAddress()
                 + ",books="
         );
         for (String book:books){
             System.out.print("<<"+book+">>\t");
         }
         System.out.println("\n爱好:"+hobbys);
 
         System.out.println("card:"+card);
 
         System.out.println("games:"+games);
 
         System.out.println("wife:"+wife);
 
         System.out.println("info:"+info);
 
     }
 }
```

#### 1、**常量注入**

```xml
 <bean id="student" class="com.kuang.pojo.Student">
     <property name="name" value="小明"/>
 </bean>
```

测试：

```java
 @Test
 public void test01(){
     ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
 
     Student student = (Student) context.getBean("student");
 
     System.out.println(student.getName());		//小明
 
 }
```

#### 2、**Bean注入** 

注意点：这里的值是一个引用，ref

```xml
 <bean id="addr" class="com.kuang.pojo.Address">
     <property name="address" value="重庆"/>
 </bean>
 
 <bean id="student" class="com.kuang.pojo.Student">
     <property name="name" value="小明"/>
     <property name="address" ref="addr"/>
 </bean>
```

#### 3、**数组注入**

```xml
 <bean id="student" class="com.kuang.pojo.Student">
     <property name="name" value="小明"/>
     <property name="address" ref="addr"/>
     <property name="books">
         <array>
             <value>西游记</value>
             <value>红楼梦</value>
             <value>水浒传</value>
         </array>
     </property>
 </bean>
```

#### 4、**List注入**

```xml
 <property name="hobbys">
     <list>
         <value>听歌</value>
         <value>看电影</value>
         <value>爬山</value>
     </list>
 </property>
```

#### 5、**Map注入**

```xml
 <property name="card">
     <map>
         <entry key="中国邮政" value="456456456465456"/>
         <entry key="建设" value="1456682255511"/>
     </map>
 </property>
```

#### 6、**set注入**

```xml
 <property name="games">
     <set>
         <value>LOL</value>
         <value>BOB</value>
         <value>COC</value>
     </set>
 </property>
```

#### 7、**Null注入**

null与 "" 空字符串不一样

```xml
 <property name="wife"><null/></property>
```

#### 8、**Properties注入**

```xml
 <property name="info">
     <props>
         <prop key="学号">20190604</prop>
         <prop key="性别">男</prop>
         <prop key="姓名">小明</prop>
     </props>
 </property>
```



测试：

```java
 @Test
 public void test01(){
     ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
 
     Student student = (Student) context.getBean("student");
 
     student.show();
 }
```

结果：

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251739060.png)





### 4.3 p命名和c命名注入

- p命名：properties命名空间，属性依然要设置set方法
- c命名：Constructor命名空间，就是构造器注入，属性依然要设置set方法
- 两种方式都需要在头文件引入相关约束文件



示例：

`User.java` ：【注意：这里无有参构造器！】

```java
 public class User {
     private String name;
     private int age;
 
     public void setName(String name) {
         this.name = name;
     }
 
     public void setAge(int age) {
         this.age = age;
     }
 
     @Override
     public String toString() {
         return "User{" +
                 "name='" + name + '\'' +
                 ", age=" + age +
                 '}';
     }
 }
```

1、P命名空间注入 : 需要在头文件中加入约束文件：` xmlns:p="http://www.springframework.org/schema/p"`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       
       xmlns:p="http://www.springframework.org/schema/p"
       
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    
    <!--P(属性: properties)命名空间 , 属性依然要设置set方法-->
    <bean id="user" class="com.kuang.pojo.User" p:name="狂神" p:age="18"/>

</beans>
```



2、c 命名空间注入 : 需要在头文件中加入约束文件：` xmlns:c="http://www.springframework.org/schema/c"`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       
       xmlns:c="http://www.springframework.org/schema/c"
       
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    
    <!-- C(构造: Constructor)命名空间 , 属性依然要设置set方法-->
    <bean id="user" class="com.kuang.pojo.User" c:name="狂神" c:age="18"/>

</beans>
```

发现问题：爆红了，刚才我们没有写有参构造！

解决：把有参构造器加上，因为**c** 就是所谓的**构造器注入**！

测试代码：

```java
 @Test
 public void test02(){
     ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
     User user = (User) context.getBean("user");
     System.out.println(user);
 }
```



## 5. Bean的作用域

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲，bean就是由IoC容器初始化、装配及管理的对象 .

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251739883.png)

几种作用域中，request、session作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。

### Singleton（单例）

当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。

Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，他都存在了，**每次获取到的对象都是同一个对象**。

注意：Singleton作用域是Spring中的缺省作用域（默认）。要在XML中将bean定义成singleton，可以这样配置：

```xml
 <bean id="ServiceImpl" class="cn.csdn.service.ServiceImpl" scope="singleton">
```

测试：

```java
 @Test
 public void test03(){
     ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
     User user = (User) context.getBean("user");
     User user2 = (User) context.getBean("user");
     System.out.println(user==user2);		//true
 }
```



### Prototype（原型）

当一个bean的作用域为Prototype，表示一个bean定义对应**多个对象实例**。

Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会**创建一个新的bean实例**。

Prototype是原型类型，它在我们**创建容器**的时候并**没有实例化**，而是当我们**获取bean的时候才会去创建一个对象**，而且我们**每次获取到的对象都不是同一个对象**。

在XML中将bean定义成prototype，可以这样配置：

```xml
 <bean id="account" class="com.foo.DefaultAccount" scope="prototype"/>  
  或者
 <bean id="account" class="com.foo.DefaultAccount" singleton="false"/> 
```



根据经验，对**有状态**的bean应该使用**prototype**作用域，而对**无状态**的bean则应该使用**singleton**作用域。



**有状态对象(Stateful Bean)** ：有实例变量的对象，可以保存数据，是**非线程安全**的。每个用户有自己特有的一个实例，在用户的生存期内，bean保持了用户的信息，即“有状态”；一旦用户灭亡（调用结束或实例结束），bean的生命期也告结束。即每个用户最初都会得到一个初始的bean。 

**无状态对象(Stateless Bean)**：没有实例变量的对象，不能保存数据，是不变类，是**线程安全**的。bean一旦实例化就被加进会话池中，各个用户都可以共用。即使用户已经消亡，bean 的生命期也不一定结束，它可能依然存在于会话池中，供其他用户调用。由于没有特定的用户，那么也就不能保持某一用户的状态，所以叫无状态bean。但无状态会话bean 并非没有状态，如果它有自己的属性（变量），那么这些变量就会受到所有调用它的用户的影响。



### Request、Session、Application

这几种作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。





## 6. Bean的自动装配

> 自动装配说明

- 自动装配是使用spring满足bean依赖的一种方法
- spring会在应用上下文中为某个bean寻找其依赖的bean



Spring中bean有三种装配机制，分别是：

1. 在Xml中显式配置
2. 在Java中显式配置
3. 隐式的Bean发现机制和自动装配

这里我们主要讲第三种：自动化的装配Bean



Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描(component scanning)：Spring会自动发现应用上下文中所创建的bean
2. 自动装配(autowiring)：Spring自动满足bean之间的依赖，也就是我们说的IoC/DI

组件扫描和自动装配组合发挥巨大威力，使得显示的配置降低到最少





> 测试环境搭建

1、新建一个项目

2、新建两个实体类，Cat  Dog  都有一个叫的方法

```java
public class Cat {
    public void shout() {
        System.out.println("miao~");
    }
}
public class Dog {
    public void shout() {
        System.out.println("wang~");
    }
}
```

3、新建一个People类

```java
public class People {
    private Cat cat;
    private Dog dog;
    private String name;
}
```

4、编写Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.run.pojo.Dog"/>
    <bean id="cat" class="com.run.pojo.Cat"/>

    <bean id="user" class="com.run.pojo.People">
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
        <property name="name" value="AruNi"/>
    </bean>

</beans>
```

5、测试

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        People people =(People) context.getBean("people");
        people.getCat().shout();
        people.getDog().shout();
    }
}
```

结果正常输出，环境OK



### 6.1 Xml实现自动装配

#### byName

由于在手动配置Xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

采用自动装配将避免这些错误，并且使配置简单化。

测试：

1、修改bean配置，增加一个属性  `autowire="byName"`

```xml
    <bean id="people" class="com.run.pojo.People" autowire="byName">
        <property name="name" value="AruNi"/>
    </bean>
```

2、再次测试，结果依旧成功输出！

3、我们将 cat 的bean id修改为 catXXX

4、再次测试， 执行时报空指针java.lang.NullPointerException。**因为按byName规则找不对应set方法，真正的setCat就没执行**，对象就没有初始化，所以调用时就会报空指针错误。

**小结：**

当一个bean节点带有 `autowire = "byName"`属性时

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat
2. 去spring容器中寻找是否有此字符串名称id的对象
3. 如果有，就取出注入；如果没有，就报空指针异常



#### byType

使用`autowire = "byType"`首先需要保证：**同一类型的对象，在spring容器中唯一**。如果不唯一，会报不唯一的异常：`NoUniqueBeanDefinitionException`



测试：

1、将user的bean配置修改一下 ： `autowire = "byType"`

2、测试，正常输出

3、再注册一个cat 的bean对象！

```xml
    <bean id="dog" class="com.run.pojo.Dog"/>
    <bean id="cat" class="com.run.pojo.Cat"/>
    <bean id="cat2" class="com.run.pojo.Cat"/>

    <bean id="people" class="com.run.pojo.People" autowire="byType">
        <property name="name" value="AruNi"/>
    </bean>
```

4、测试，报错：`NoUniqueBeanDefinitionException`

5、删掉cat2，将cat的bean名称改掉！测试！因为是按类型装配，所以并不会报异常，也不影响最后的结果。甚至将id属性去掉，也不影响结果。



**小结：**

- byName：保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
- byType：保证bean的class唯一，并且这个bean需要和自动注入的属性的类型一致





### 6.1 注解实现自动装配

JDK1.5开始支持注解，spring2.5开始全面支持注解。



准备工作：利用注解的方式注入属性。

1、在Spring配置文件中引入`context文件头`(3条)

```xml
xmlns:context="http://www.springframework.org/schema/context"

http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
```

2、==开启注解支持==

```xml
<context:annotation-config/>
```



#### @Autowired

- @Autowired是**按类型byType**自动转配的，不支持id匹配。

- 需要导入spring-aop的包（spring-mvc里面有）

  

测试：

1、将People类中使用@Autowired注解 (无set方法也可以)

```java
@Data
public class People {
    @Autowired
    private Cat cat;

    @Autowired
    private Dog dog;
    private String name;
}
```

2、此时配置文件内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean id="dog" class="com.run.pojo.Dog"/>
    <bean id="cat" class="com.run.pojo.Cat"/>
    <bean id="people" class="com.run.pojo.People"/>

</beans>
```

3、测试，成功输出结果！



**扩展：**

`@Autowired(required = false) ` 说明：false，对象可以为null；true，对象必须存对象，不能为null。默认为true

```java
//如果允许对象为null，设置required = false, 默认为true
@Autowired(required = false)
private Cat cat;
```

`@Nullable`：字段标记了这个注解，说明这个字段额可以为null



#### @Qualifier

- `@Autowired`是根据**类型**自动装配的，加上`@Qualifier`则可以根据**byName**的方式自动装配
- `@Qualifier`不能单独使用

测试实验步骤：

1、配置文件修改内容，保证类型存在对象。且名字不为类的默认名字！

```xml
<bean id="dog1" class="com.kuang.pojo.Dog"/>
<bean id="dog2" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>
```

2、没有加Qualifier测试，直接报错

3、在属性上添加Qualifier注解

```java
@Autowired
@Qualifier(value = "cat2")
private Cat cat;

@Autowired
@Qualifier(value = "dog2")
private Dog dog;
```

测试，成功输出！



如果`@Autowired`自动装配的环境比较复杂，自动装配无法通过一个注解`@Autowired`完成的时候，可以使用`@Qualifier(value = "xxx")`配合使用！



#### @Resource

- `@Resource`是Java官方的注解



自动装配的过程：

- `@Resource`如果有指定的name属性，先按**该属性**进行**byName方式**查找装配
- 其次再进行**默认**的**byName**方式进行装配
- 如果以上都不成功，则按byType的方式自动装配
- 都不成功，则报异常



实体类：

```java
public class People {
    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

beans.xml

```xml
<bean id="dog" class="com.run.pojo.Dog"/>
<bean id="cat1" class="com.run.pojo.Cat"/>
<bean id="cat2" class="com.run.pojo.Cat"/>

<bean id="People" class="com.run.pojo.People"/>
```

测试：结果OK

配置文件2：beans.xml ， 删掉cat2

```xml
<bean id="dog" class="com.run.pojo.Dog"/>
<bean id="cat1" class="com.run.pojo.Cat"/>
```

实体类上只保留注解

```java
@Resource
private Cat cat;
@Resource
private Dog dog;
```

结果：OK

结论：先进行byName查找，失败；再进行byType查找，成功。



#### 小结

`@Autowired`与`@Resource`异同：

- `@Autowired`与`@Resource`都可以用来装配bean。都可以写在字段上，或写在setter方法上。

- `@Autowired`默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：`@Autowired(required=false) `，如果我们想使用名称装配可以结合`@Qualifier`注解进行使用

- `@Resource`（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

它们的作用相同都是用注解方式注入对象，但执行顺序不同。`@Autowired`先byType，`@Resource`先byName。







## 7. 使用注解开发

> 说明

在spring4之后，想要使用注解形式，必须得要引入aop的包(spring-mvc中有)

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251741504.png)



在`applicationContext.xml`配置文件当中，还得要引入一个context约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
```





### 7.1 Bean的实现`@Component()`

我们之前都是使用 bean 的标签进行bean注入，但是实际开发中，我们一般都会使用注解！

1、在`applicationContext.xml`中配置扫描哪些包下的注解

```xml
<!--指定注解扫描包-->
<context:component-scan base-package="com.run.pojo"/>

<!--开启注解支持-->
<context:annotation-config/>
```

2、在指定包下编写类，增加注解

```java
@Component("user")
//相当于配置文件中的 <bean id="user" class="注解类"/>
public class User {
    public String name = "AruNi";
}
```

3、测试

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user =(User) context.getBean("user");
        System.out.println(user.name);		//AruNi
    }
}
```



### 7.2 属性注入`Value()`

使用注解注入属性

1、可以不用提供set方法，直接在直接名上添加@value("值")

```java
@Component("user")
//相当于配置文件中的 <bean id="user" class="注解类"/>
public class User {
    @Value("润")
    //相当于配置文件中的 <property name="name" value="润"/>
    public String name;
}
```

2、如果提供了set方法，在set方法上添加@value("值");

```java
@Component("user")
//相当于配置文件中的 <bean id="user" class="注解类"/>
public class User {

    //相当于配置文件中的 <property name="name" value="润"/>
    public String name;

    @Value("润")
    public void setName(String name) {
        this.name = name;
    }
}
```



### 7.3 衍生注解

为了**更好的进行分层**，Spring可以使用其它三个注解，**功能一样**，目前使用哪一个功能都一样。

**`@Component`三个衍生注解：**

- `@Controller`：web层(servlet层)
- `@Service`：service层
- `@Repository`：dao层

写上这些注解，就相当于将这个类交给Spring管理装配了！



### 7.4 自动装配注解

在Bean的自动装配已经讲过了



### 7.5 作用域`@scope`

- **singleton**：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- **prototype**：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

```
@Controller("user")
@Scope("prototype")
public class User {
    @Value("run")
    public String name;
}
```



> 小结

**XML与注解比较**

- XML可以适用任何场景 ，结构清晰，维护方便
- 注解不是自己提供的类使用不了，开发简单方便                               

​                                        

**XML与注解整合开发** ：推荐最佳实践                                         

- xml管理Bean                                                                                                                                        

- 注解完成属性注入

- 使用过程中， 想要让注解生效，就需要开启注解支持

  ```xml
  <context:component-scan base-package="com.run.pojo"/>
  <context:annotation-config/>
  ```



作用：

- 进行注解驱动注册，从而使注解生效

- 用于激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册

- 如果不扫描包，就需要手动配置bean

- 如果不加注解驱动，则注入的值为null！

  





### 7.6 JavaConfig实现配置

JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息，在 Spring4 的版本， JavaConfig 已正式成为 Spring4 的核心功能 。

有了它，完全不需要XML配置文件了！



测试：

1、编写一个实体类Dog，添加注解`@Component`

```java
@Component  //将这个类标注为Spring的一个组件，放到容器中！
public class Dog {
    public String name = "dog";
}
```

2、新建一个config配置包，编写一个MyConfig配置类

```java
@Configuration  	//代表这是一个配置类(相当于XML配置文件)，也会被放到spring容器中，因为它本来就是一个@Component
public class MyConfig {

    //通过方法注册一个bean，这里的返回值就Bean的类型，方法名就是bean的id！
    @Bean 
    public Dog dog(){
        return new Dog();
    }

}
```

3、测试

```java
@Test
public void test2(){
    ApplicationContext context =
            new AnnotationConfigApplicationContext(MyConfig.class);
    Dog dog = (Dog) context.getBean("dog");
    System.out.println(dog.name);
}
```

4、成功输出结果！



**导入其他配置如何做呢？**

1、我们在config包中再编写一个配置类！

```java
@Configuration  //代表这是一个配置类
public class MyConfig2 {
}
```

2、在之前的配置类中我们来选择导入这个配置类

```java
@Configuration
@Import(MyConfig2.class)  //导入合并其他配置类，类似于配置文件中的 inculde 标签
public class MyConfig {

    @Bean
    public Dog dog(){
        return new Dog();
    }

}
```

关于这种纯Java类的配置方式，我们在之后的SpringBoot 和 SpringCloud中还会大量看到，我们需要知道这些注解的作用即可！





## 8. 代理模式

为什么要学习代理模式，因为AOP的底层机制就是动态代理！

代理模式：

- 静态代理
- 动态代理

学习aop之前 , 我们要先了解一下代理模式！



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251741517.png)

### 8.1 静态代理

**静态代理角色分析**

- 抽象角色 : 一般使用接口或者抽象类来实现

- 真实角色 : 被代理的角色

- 代理角色 : 代理真实角色 ; 代理真实角色后 , 一般会做一些附属的操作 .

- 客户  :  使用代理角色来进行一些操作 .

  

**代码实现：**

`Rent.java` 即抽象角色

```java
//抽象角色：租房
public interface Rent {
    public void rent();
}
```

`Host.java` 即真实角色

```java
//真实角色: 房东，房东要出租房子
public class Host implements Rent{
    public void rent() {
        System.out.println("房屋出租");
    }
}
```

`Proxy.java` 即代理角色

```java
//代理角色：中介
public class Proxy implements Rent {

    private Host host;
    public Proxy() { }
    public Proxy(Host host) {
        this.host = host;
    }

    //租房
    public void rent(){
        seeHouse();
        host.rent();
        fare();
    }
    //看房
    public void seeHouse(){
        System.out.println("带房客看房");
    }
    //收中介费
    public void fare(){
        System.out.println("收中介费");
    }
}
```

`Client.java` 即客户

```java
//客户类，一般客户都会去找代理！
public class Client {
    public static void main(String[] args) {
        //房东要租房
        Host host = new Host();
        //中介帮助房东
        Proxy proxy = new Proxy(host);

        //你去找中介！
        proxy.rent();
    }
}
```

分析：在这个过程中，你直接接触的就是中介，就如同现实生活中的样子，你看不到房东，但是你依旧租到了房东的房子通过代理，这就是所谓的代理模式，程序源自于生活，所以学编程的人，一般能够更加抽象的看待生活中发生的事情。

**静态代理的好处：**

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .

缺点 :

- 类多了 , 多了代理类 , 工作量变大了 . 开发效率降低 .

我们想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !



#### 静态代理再理解

我们再来举一个例子，巩固大家的学习！

练习步骤：

1、创建一个抽象角色，比如咋们平时做的用户业务，抽象起来就是增删改查！

```java
//抽象角色：增删改查业务
public interface UserService {
    void add();
    void delete();
    void update();
    void query();
}
```

2、我们需要一个真实对象来完成这些增删改查操作

```java
//真实对象，完成增删改查操作的人
public class UserServiceImpl implements UserService {

    public void add() {
        System.out.println("增加了一个用户");
    }

    public void delete() {
        System.out.println("删除了一个用户");
    }

    public void update() {
        System.out.println("更新了一个用户");
    }

    public void query() {
        System.out.println("查询了一个用户");
    }
}
```

3、需求来了，现在我们需要增加一个日志功能，怎么实现！

- 思路1 ：在实现类上增加代码 【麻烦！】
- 思路2：使用代理来做，能够不改变原来的业务情况下，实现此功能就是最好的了！

4、设置一个代理类来处理日志！代理角色

```java
//代理角色，在这里面增加日志的实现
public class UserServiceProxy implements UserService {
    private UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    public void add() {
        log("add");
        userService.add();
    }

    public void delete() {
        log("delete");
        userService.delete();
    }

    public void update() {
        log("update");
        userService.update();
    }

    public void query() {
        log("query");
        userService.query();
    }

    public void log(String msg){
        System.out.println("执行了"+msg+"方法");
    }

}
```

5、测试访问类：

```java
public class Client {
    public static void main(String[] args) {
        //真实业务
        UserServiceImpl userService = new UserServiceImpl();
        //代理类
        UserServiceProxy proxy = new UserServiceProxy();
        //使用代理类实现日志功能！
        proxy.setUserService(userService);

        proxy.add();
    }
}
```

OK，到了现在代理模式大家应该都没有什么问题了，重点大家需要理解其中的思想；

我们在不改变原来的代码的情况下，实现了对原有功能的增强，这是AOP中最核心的思想

聊聊AOP：纵向开发，横向开发



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251741029.png)





### 8.2 动态代理

#### 8.2.1 准备案例

**案例：**模拟企业业务功能开发，并完成每个功能的性能统计

**需求：**模拟某企业用户管理业务，需包含用户登录，用户删除，用户查询功能，并要统计每个功能的耗时。

**分析：**

1. 定义一个UserService表示用户业务接口，规定必须完成用户登录，用户删除，用户查询功能。
2. 定义一个实现类UserServiceImpl实现UserService，并完成相关功能，且统计每个功能的耗时。
3. 定义测试类，创建实现类对象，调用方法。



**代码：**

UserService接口类：

```java
public interface UserService {
    String login(String loginName , String passWord) ;
    void selectUsers();
    boolean deleteUsers();
    void updateUsers();
}
```

UserServiceImpl实现类：

```java
public class UserServiceImpl implements UserService{
    @Override
    public String login(String loginName, String passWord)  {
        long startTimer = System.currentTimeMillis();
        try {
            Thread.sleep(1000);
            if("admin".equals(loginName) && "1234".equals(passWord)) {
                return "success";
            }
            return "登录名和密码可能有误";
        } catch (Exception e) {
            e.printStackTrace();
            return "error";
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("login方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }



    }

    @Override
    public void selectUsers() {
        long startTimer = System.currentTimeMillis();
        System.out.println("查询了100个用户数据！");
        try {
            Thread.sleep(2000);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("selectUsers方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }
    }

    @Override
    public boolean deleteUsers() {
        long startTimer = System.currentTimeMillis();
        try {
            System.out.println("删除100个用户数据！");
            Thread.sleep(500);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("deleteUsers方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }
    }

    @Override
    public void updateUsers() {
        long startTimer = System.currentTimeMillis();
        try {
            System.out.println("修改100个用户数据！");
            Thread.sleep(2500);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("updateUsers方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        UserService userService = new UserServiceImpl();
        System.out.println(userService.login("admin", "1234"));
        System.out.println(userService.deleteUsers());
        userService.selectUsers();
        userService.updateUsers();
    }
}
```

输出：

```
login方法耗时：1.0s
success
删除100个用户数据！
deleteUsers方法耗时：0.501s
true
查询了100个用户数据！
selectUsers方法耗时：2.001s
修改100个用户数据！
updateUsers方法耗时：2.5s

Process finished with exit code 0
```



**本案例存在的问题**

- 业务对象的的每个方法都要进行时间性能统计，存在大量重复的代码。

接下来使用动态代理解决问题。

#### 8.2.2 使用动态代理

**动态代理**

- 代理就是被代理者没有能力或者不愿意去完成某件事情，需要找个人代替自己去完成这件事，动态代理就是用来对业务功能（方法）进行代理的。

**关键步骤**

1. 必须有**接口**，实现类要实现接口（代理通常是**基于接口**实现的）。
2. 创建一个实现类的对象，该对象为业务对象，紧接着为业务对象做一个代理对象。

![image-20220323194501966](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251737265.png)









**案例优化：**

UserService接口类不变

```java
public interface UserService {
    String login(String loginName , String passWord) ;
    void selectUsers();
    boolean deleteUsers();
    void updateUsers();
}
```



UserServiceImpl实现类中有关性能分析的代码全都删掉，只保留真正的业务功能

```java
public class UserServiceImpl implements UserService{
    @Override
    public String login(String loginName, String passWord)  {
        try {
            Thread.sleep(1000);
        } catch (Exception e) {
            e.printStackTrace();
        }
        if("admin".equals(loginName) && "1234".equals(passWord)) {
            return "success";
        }
        return "登录名和密码可能有毛病";
    }

    @Override
    public void selectUsers() {
        System.out.println("查询了100个用户数据！");
        try {
            Thread.sleep(2000);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public boolean deleteUsers() {
        try {
            System.out.println("删除100个用户数据！");
            Thread.sleep(500);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    @Override
    public void updateUsers() {
        try {
            System.out.println("修改100个用户数据！");
            Thread.sleep(2500);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



新增ProxyUtil代理类

```java
/**
    public static Object newProxyInstance(ClassLoader loader,  Class<?>[] interfaces, InvocationHandler h)
    参数一：类加载器，负责加载代理类到内存中使用。
    参数二：获取被代理对象实现的全部接口。代理要为全部接口的全部方法进行代理
    参数三：代理的核心处理逻辑
 */
public class ProxyUtil {
    /**
      生成业务对象的代理对象。
     * @param obj
     * @return
     */
    
    //写成泛型，任何类型的对象都能通用
    public static <T> T  getProxy(T obj) {
        // 返回一个代理对象
        return (T)Proxy.newProxyInstance(obj.getClass().getClassLoader(),
                obj.getClass().getInterfaces(),
                new InvocationHandler() {		//InvocationHandler是接口，用匿名形式创建对象
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        // 参数一：代理对象本身。一般不管
                        // 参数二：正在被代理的方法
                        // 参数三：被代理方法，应该传入的参数
                        
                        //时间的计算代理来做
                       long startTimer = System .currentTimeMillis();
                        
                        // 触发方法的真正执行(触发真正的业务功能)
                        Object result = method.invoke(obj, args);

                        long endTimer = System.currentTimeMillis();
                        System.out.println(method.getName() + "方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");

                        // 把业务功能方法执行的结果返回给调用者
                        return result;
                    }
                });
    }
}
```

调用代理执行：

```java
public class Test {
    public static void main(String[] args) {
        // 1、把业务对象，直接做成一个代理对象返回，代理对象的类型也是 UserService类型
        UserService userService = ProxyUtil.getProxy(new UserServiceImpl());
        //一调用方法就先走代理
        System.out.println(userService.login("admin", "1234"));
        System.out.println(userService.deleteUsers());
        userService.selectUsers();
        userService.updateUsers();		//新增一个方法，也会走代理
    }
}
```

输出结果：

```
login方法耗时：1.001s
success
删除100个用户数据！
deleteUsers方法耗时：0.501s
true
查询了100个用户数据！
selectUsers方法耗时：2.0s
修改100个用户数据！
updateUsers方法耗时：2.5s

Process finished with exit code 0

```



**动态代理的优点：**

- 非常的灵活，支持任意接口类型的实现类对象做代理，也可以直接为接本身做代理。
- 可以为被代理对象的所有方法做代理。
- 可以在不改变方法源码的情况下，实现对方法功能的增强。
- 不仅简化了编程工作、提高了软件系统的可扩展性，同时也提高了开发效率。



**动态代理的思想：**

**AOP**是Aspect Oriented Programming，即面向切面编程。

那什么是AOP？

我们先回顾一下OOP：Object Oriented Programming，OOP作为面向对象编程的模式，获得了巨大的成功，OOP的主要功能是数据封装、继承和多态。

而AOP是一种新的编程方式，它和OOP不同，OOP把系统看作多个对象的交互，AOP把系统分解为不同的关注点，或者称之为切面（Aspect）。







## 9. AOP

### 9.1 什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251742604.png)

### 9.2 AOP在Spring中的作用

提供声明式事务；允许用户自定义切面

以下名词需要了解下：

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（Pointcut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251742650.png)



SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251742295.png)

即 AOP 在 不改变原有代码的情况下 , 去增加新的功能 .

### 9.3 使用Spring实现AOP

**【重点】**使用AOP织入，需要导入一个依赖包！

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```



#### 方式一：Spring API

首先编写我们的业务接口和实现类

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void search();
}
```

```java
public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加用户");
    }

    @Override
    public void delete() {
        System.out.println("删除用户");
    }

    @Override
    public void update() {
        System.out.println("更新用户");
    }

    @Override
    public void search() {
        System.out.println("查询用户");
    }
}
```



然后去写我们的增强类 , 我们编写两个 , 一个前置增强 一个后置增强

```java
public class BeforeLog implements MethodBeforeAdvice {

    //method：要执行的目标对象的方法
    //args：传入的参数
    //target：目标对象
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass() + "的" + method.getName() + "被执行了！");
    }
}
```

```java
public class AfterLog implements AfterReturningAdvice {
    //returnValue：返回值
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行的方法为：" + method.getName() + "，返回结果为：" + returnValue);
    }
}
```



最后去spring的文件中注册 , 并实现aop切入实现 , 注意导入约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.run.service.UserServiceImpl"/>
    <bean id="beforeLog" class="com.run.log.BeforeLog"/>
    <bean id="afterLog" class="com.run.log.AfterLog"/>

    <!--配置aop：需要导入aop的约束-->
    <aop:config>
        <!--切入点；expression：表达式，execution(要执行的位置)-->
        <aop:pointcut id="pointcut" expression="execution(* com.run.service.UserServiceImpl.*(..))"/>
        <!--执行环绕；advice-ref：执行方法，pointcut-ref：切入点-->
        <aop:advisor advice-ref="beforeLog" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

</beans>
```

`"execution(* com.run.service.UserServiceImpl.*(..))"`的理解：

- 执行包`com.run.service.UserServiceImpl`下的任意方法(*)，任意参数类型((..))

execution表达式说明：[execution() 表达式](https://blog.csdn.net/zhuanglicheng/article/details/109110176)



测试

```java
public class MyTest {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        //动态代理，代理的是接口，不是实现类
        UserService userService = (UserService) context.getBean("userService");

        userService.add();
        userService.delete();
    }
}
```



输出：

```
class com.run.service.UserServiceImpl的add被执行了！
增加用户
执行的方法为：add，返回结果为：null
class com.run.service.UserServiceImpl的delete被执行了！
删除用户
执行的方法为：delete，返回结果为：null

Process finished with exit code 0

```



Spring的Aop就是将公共的业务 (日志，安全等) 和领域业务结合起来，当执行领域业务时，将会把公共业务加进来。实现公共业务的重复利用。领域业务更纯粹，程序猿专注领域业务，其本质还是动态代理。





#### 方式二：自定义类

目标业务类不变依旧是userServiceImpl



新建diy包，自定义一个切入类

```java
public class DiyPointcut {
    public void before(){
        System.out.println("---------方法执行前---------");
    }
    public void after(){
        System.out.println("---------方法执行后---------");
    }
}
```



去spring中配置

```xml
    <!--方式二：自定义类-->
    <!--注册bean-->
    <bean id="diy" class="com.run.diy.DiyPointcut"/>

    <aop:config>
        <!--自定义切面，ref：要引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.run.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
```



测试

```java
public class MyTest {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        //动态代理，代理的是接口，不是实现类
        UserService userService = (UserService) context.getBean("userService");

        userService.add();
        userService.delete();
    }
}
```



输出：

```
---------方法执行前---------
增加用户
---------方法执行后---------
---------方法执行前---------
删除用户
---------方法执行后---------

Process finished with exit code 0

```





#### 方式三：注解

第一步：编写一个注解实现的增强类

```java
@Aspect
public class AnnotationPointcut {

    @Before("execution(* com.run.service.UserServiceImpl.*(..))")
    public void before() {
        System.out.println("---------方法执行前---------");
    }

    @After("execution(* com.run.service.UserServiceImpl.*(..))")
    public void after() {
        System.out.println("---------方法执行后---------");
    }

    //在环绕增强中，可以给定一个参数，代表要获取处理切入的点
    @Around("execution(* com.run.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("=====环绕前=====");

        Signature signature = jp.getSignature();    //获得签名(被执行的方法)
        System.out.println("签名：" + signature);

        //执行目标方法proceed (add就执行add，delete就执行delete)
        Object proceed = jp.proceed();
        System.out.println(proceed);

        System.out.println("=====环绕后=====");
    }
}
```



第二步：在Spring配置文件中，注册bean，并增加支持注解的配置

```xml
    <!--方式三：注解-->
    <bean id="annotationPointcut" class="com.run.diy.AnnotationPointcut"/>
    <!--开启注解支持-->
    <aop:aspectj-autoproxy/>
```



输出：注意方法执行的顺序

```
=====环绕前=====
签名：void com.run.service.UserService.add()
---------方法执行前---------
增加用户
---------方法执行后---------
null
=====环绕后=====

Process finished with exit code 0

```





`aop:aspectj-autoproxy`说明：

通过aop命名空间的`<aop:aspectj-autoproxy />`声明自动为spring容器中那些配置`@aspectJ`切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被`<aop:aspectj-autoproxy />`隐藏起来了



`<aop:aspectj-autoproxy />`有一个`proxy-target-class`属性，默认为false，表示使用jdk动态代理织入增强

当配为`<aop:aspectj-autoproxy  poxy-target-class="true"/>`时，表示使用`CGLib`动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。





## 10. 整合MyBatis

### 10.1 配置环境

1、导入相关jar包

junit

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

mybatis

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>
```

mysql-connector-java

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

spring相关

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.10.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.1.10.RELEASE</version>
</dependency>
```

aspectJ AOP 织入器

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

mybatis-spring整合包 【重点】

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.2</version>
</dependency>
```

配置Maven静态资源过滤问题！

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

2、编写配置文件

3、代码实现



### 10.2 回忆MyBatis

**编写pojo实体类**

```java
package com.kuang.pojo;

public class User {
    private int id;  //id
    private String name;   //姓名
    private String pwd;   //密码
}
```

**实现MyBatis的配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <package name="com.run.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <package name="com.run.dao"/>
    </mappers>
</configuration>
```

**UserDao接口编写**

```java
public interface UserMapper {
    List<User> selectUser();
}
```

**接口对应的Mapper映射文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.run.dao.UserMapper">

    <select id="selectUser" resultType="User">
        select * from mybatis.user
    </select>

</mapper>
```

**测试类**

```java
public class MyTest {
    @Test
    public void test() throws IOException {
        String resource = "mybatis-config.xml";
        InputStream is = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        List<User> userList = mapper.selectUser();
        for (User user : userList) {
            System.out.println(user);
        }

        sqlSession.close();
    }
}
```



### 10.3 MyBatis-Spring

引入Spring之前需要了解mybatis-spring包中的一些重要类；

http://www.mybatis.org/spring/zh/index.html



**什么是 MyBatis-Spring？**

MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。

**知识基础**

在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。这很重要

MyBatis-Spring 需要以下版本：

| MyBatis-Spring | MyBatis | Spring 框架 | Spring Batch | Java    |
| :------------- | :------ | :---------- | :----------- | :------ |
| 2.0            | 3.5+    | 5.0+        | 4.0+         | Java 8+ |
| 1.3            | 3.4+    | 3.2.2+      | 2.1+         | Java 6+ |

如果使用 Maven 作为构建工具，仅需要在 pom.xml 中加入以下代码即可：

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.2</version>
</dependency>
```



#### 快速上手

要和 Spring 一起使用 MyBatis，需要在 Spring 应用上下文中定义至少两样东西：一个 `SqlSessionFactory` 和至少一个`数据映射器类`。

在 MyBatis-Spring 中，可使用 `SqlSessionFactoryBean`来创建 `SqlSessionFactory`。 要配置这个工厂 bean，只需要把下面代码放在 Spring 的 XML 配置文件中：

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
</bean>
```

```java
@Configuration
public class MyBatisConfig {
  @Bean
  public SqlSessionFactory sqlSessionFactory() throws Exception {
    SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
    factoryBean.setDataSource(dataSource());
    return factoryBean.getObject();
  }
}
```

注意：`SqlSessionFactory` 需要一个 `DataSource`（数据源）。这可以是任意的 `DataSource`，只需要和配置其它 Spring 数据库连接一样配置它就可以了。

假设你定义了一个如下的 mapper 接口：

```java
public interface UserMapper {
  @Select("SELECT * FROM users WHERE id = #{userId}")
  User getUser(@Param("userId") String userId);
}
```

那么可以通过 `MapperFactoryBean` 将接口加入到 Spring 中:

```xml
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
  <property name="mapperInterface" value="org.mybatis.spring.sample.mapper.UserMapper" />
    
  <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```

需要注意的是：所指定的映射器类**必须**是一个接口，而不是具体的实现类。在这个示例中，通过注解来指定 SQL 语句，但是也可以使用 MyBatis 映射器的 XML 配置文件。

配置好之后，你就可以像 Spring 中普通的 bean 注入方法那样，将映射器注入到你的业务或服务对象中。`MapperFactoryBean` 将会负责 `SqlSession` 的创建和关闭。 如果使用了 Spring 的事务功能，那么当事务完成时，session 将会被提交或回滚。最终任何异常都会被转换成 Spring 的 `DataAccessException` 异常。

使用 Java 代码来配置的方式如下：

```java
@Configuration
public class MyBatisConfig {
  @Bean
  public UserMapper userMapper() throws Exception {
    SqlSessionTemplate sqlSessionTemplate = new SqlSessionTemplate(sqlSessionFactory());
    return sqlSessionTemplate.getMapper(UserMapper.class);
  }
}
```

要调用 MyBatis 的数据方法，只需一行代码：

```java
public class FooServiceImpl implements FooService {

  private final UserMapper userMapper;

  public FooServiceImpl(UserMapper userMapper) {
    this.userMapper = userMapper;
  }

  public User doSomeBusinessStuff(String userId) {
    return this.userMapper.getUser(userId);
  }
}
```



#### 使用SqlSession

在基础的 MyBatis 用法中，是通过 `SqlSessionFactoryBuilder` 来创建 `SqlSessionFactory` 的。而在 MyBatis-Spring 中，则使用 `SqlSessionFactoryBean` 来创建。

在 MyBatis 中，你可以使用 `SqlSessionFactory` 来创建 `SqlSession`。一旦你获得一个 session 之后，你可以使用它来执行映射了的语句，提交或回滚连接，最后，当不再需要它的时候，你可以关闭 session。

`SqlSessionFactory`有一个唯一的必要属性：用于 JDBC 的 DataSource。这可以是任意的 DataSource 对象，它的配置方法和其它 Spring 数据库连接是一样的。

一个常用的属性是 `configLocation`，它用来指定 MyBatis 的 XML 配置文件路径。它在需要修改 MyBatis 的基础配置非常有用。通常，基础配置指的是 `< settings>` 或 `< typeAliases>`元素。

需要注意的是，这个配置文件并不需要是一个完整的 MyBatis 配置。确切地说，任何环境配置（`<environments>`），数据源（`<DataSource>`）和 MyBatis 的事务管理器（`<transactionManager>`）都会被忽略。`SqlSessionFactoryBean` 会创建它自有的 MyBatis 环境配置（`Environment`），并按要求设置自定义环境的值。

`SqlSessionTemplate` 是 MyBatis-Spring 的核心。作为 SqlSession 的一个实现，这意味着可以使用它无缝代替你代码中已经在使用的 SqlSession。

模板可以参与到 Spring 的事务管理中，并且由于其是线程安全的，可以供多个映射器类使用，你应该总是用 `SqlSessionTemplate` 来替换 MyBatis 默认的 `DefaultSqlSession` 实现。在同一应用程序中的不同类之间混杂使用可能会引起数据一致性的问题。

可以使用 `SqlSessionFactory `作为构造方法的参数来创建 `SqlSessionTemplate` 对象。

```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
```

现在，这个 bean 就可以直接注入到你的 DAO bean 中了。你需要在你的 bean 中添加一个 SqlSession 属性，就像下面这样：

```java
public class UserDaoImpl implements UserDao {

  private SqlSession sqlSession;

  public void setSqlSession(SqlSession sqlSession) {
    this.sqlSession = sqlSession;
  }

  public User getUser(String userId) {
    return sqlSession.getMapper...;
  }
}
```

按下面这样，注入 `SqlSessionTemplate`：

```xml
<bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
  <property name="sqlSession" ref="sqlSession" />
</bean>
```

具体可查看`mybatis-spring`文档





### 10.4 整合实现一

1、在`resources`下新建`spring-dao.xml`文件，引入Spring配置，接下来的配置都在这里面写

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        
</beans
```



2、配置数据源替换mybaits的数据源

```xml
    <!--配置数据源：数据源有非常多，可以使用第三方的，例如c3p0，dbcp，druid，这里使用Spring提供的-->
    <!--这里配置了数据源，mybatis-config.xml里就不用配了-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>
```



3、配置SqlSessionFactory，关联MyBatis

```xml
    <!--配置SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--关联Mybatis-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!--这里配置了mapper，mybatis-config.xml 里就可以不用配了-->
        <property name="mapperLocations" value="classpath:com/run/dao/*.xml"/>
    </bean>
```



4、注册sqlSessionTemplate，关联sqlSessionFactory；

```xml
    <!--注册sqlSessionTemplate(就是sqlSession) 关联sqlSessionFactory-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能利用构造器注入，因为它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
```



5、增加Dao接口的实现类；私有化sqlSessionTemplate

```java
public class UserMapperImpl implements UserMapper{

    //以前使用sqlSession来执行所有操作，现在使用SqlSessionTemplate
    private SqlSessionTemplate sqlSession;
    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```



6、注册bean实现：UserMapperImpl

```xml
    <!--注册bean实现-->
    <bean id="userMapper" class="com.run.dao.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
```



7、测试

```java
    @Test
    public void test2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        UserMapper mapper = (UserMapper) context.getBean("userDao");
        List<User> user = mapper.selectUser();
        System.out.println(user);
    }
```

结果成功输出！



现在Mybatis配置文件的状态！发现都可以被Spring整合！

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>
</configuration>
```





### 10.5 整合实现二

mybatis-spring1.2.3版以上的才有这个

官方文档：

**`SqlSessionDaoSupport`**

`SqlSessionDaoSupport` 是一个抽象的支持类，用来为你提供 `SqlSession`。调用 `getSqlSession()` 方法你会得到一个 `SqlSessionTemplate`，之后可以用于执行 SQL 方法，就像下面这样:

```java
public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao {
  public User getUser(String userId) {
    return getSqlSession().selectOne("org.mybatis.spring.sample.mapper.UserMapper.getUser", userId);
  }
}
```

在这个类里面，通常更倾向于使用 `MapperFactoryBean`，因为它不需要额外的代码。但是，如果你需要在 DAO 中做其它非 MyBatis 的工作或需要一个非抽象的实现类，那么这个类就很有用了。

`SqlSessionDaoSupport` 需要通过属性设置一个 `sqlSessionFactory` 或 `SqlSessionTemplate`。如果两个属性都被设置了，那么 `SqlSessionFactory` 将被忽略。

假设类 `UserMapperImpl` 是 `SqlSessionDaoSupport` 的子类，可以编写如下的 Spring 配置来执行设置：

```xml
<bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
  <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```



dao继承Support类 , 直接利用 getSqlSession() 获得 , 然后直接注入SqlSessionFactory . 比起方式1 , 不需要管理SqlSessionTemplate , 而且对事务的支持更加友好 . 可跟踪源码查看



**具体实现：**

1、将我们上面写的UserDaoImpl修改一下

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    @Override
    public List<User> selectUser() {
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        return mapper.selectUser();
    }
}
```

2、bean的配置

```
<bean id="userMapper2" class="com.run.dao.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
```

3、测试

```java
    @Test
    public void test02() {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper mapper = context.getBean("userMapper2", UserMapper.class);
        List<User> userList = mapper.selectUser();
        for (User user : userList) {
            System.out.println(user);
        }
    }
```

也能成功输出！



**总结 : 整合到spring以后可以完全不要mybatis的配置文件，除了这些方式可以实现整合之外，我们还可以使用注解来实现，这个等我们后面学习SpringBoot的时候还会测试整合！**





## 11. 声明式事务

### 11.1 回顾事务

- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
- 事务管理是企业级应用程序开发中必备技术，用来确保数据的完整性和一致性。

事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。

**事务四个属性ACID**

原子性（atomicity）

- 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用

一致性（consistency）

- 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中

隔离性（isolation）

- 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏

持久性（durability）

- 事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中



#### 测试

将上面的代码拷贝到一个新项目中

在之前的案例中，我们给userDao接口新增两个方法，删除和增加用户；

```java
//添加一个用户
int addUser(User user);

//根据id删除用户
int deleteUser(int id);
```



mapper文件中增加sql语句，我们故意把 deletes 写错，测试！

```xml
    <insert id="addUser" parameterType="user">
        insert into mybatis.user(id, name, pwd)
        values (#{id}, #{name}, #{pwd})
    </insert>

    <delete id="deleteUser">
        deletes from mybatis.user
        where id = #{id}
    </delete>
```



编写接口的实现类，在实现类中，我们去操作一波

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    @Override
    public List<User> selectUser() {
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        User user = new User(5, "小王", "111111");
        mapper.addUser(user);
        mapper.deleteUser(5);
        return mapper.selectUser();
    }

    @Override
    public int addUser(User user) {
        return getSqlSession().getMapper(UserMapper.class).addUser(user);
    }

    @Override
    public int deleteUser(int id) {
        return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
    }
}
```

测试

```java
public class MyTest {
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper mapper = (UserMapper) context.getBean("userMapper2");
        List<User> userList = mapper.selectUser();
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```

报错：sql异常，delete写错了

结果 ：插入成功！

没有进行事务的管理；我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要**事务！**

以前我们都需要自己手动管理事务，十分麻烦！

但是Spring给我们提供了事务管理，我们只需要配置即可；



### 11.2 Spring中的事务管理

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

**编程式事务管理**

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**声明式事务管理**

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

**使用Spring管理事务，注意头文件的约束导入 : `tx`**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

**事务管理器**

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

**JDBC事务**

```xml
    <!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
```

**配置好事务管理器后我们需要去配置事务的通知**

```xml
    <!--结合AOP实现事务的织入-->
    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务，配置事务的传播特性：propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
```

**spring事务传播特性：**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- `propagation_required`：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- `propagation_supports`：支持当前事务，如果没有当前事务，就以非事务方法执行。
- `propagation_mandatory`：使用当前事务，如果没有当前事务，就抛出异常。
- `propagation_required_new`：新建事务，如果当前存在事务，把当前事务挂起。
- `propagation_not_supported`：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- `propagation_never`：以非事务方式执行操作，如果当前事务存在则抛出异常。
- `propagation_nested`：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与`propagation_required`类似的操作



Spring 默认的事务传播行为是 `PROPAGATION_REQUIRED`，它适合于绝大多数的情况。

假设 ServiveX#methodX() 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：Service1#method1()->Service2#method2()->Service3#method3()，那么这 3 个服务类的 3 个方法通过 Spring 的事务传播机制都工作在同一个事务中。

就好比，我们刚才的几个方法存在调用，所以会被放在一组事务当中！



**配置AOP**

导入aop的头文件！

```xml
<!--配置aop织入事务-->
<aop:config>
    <aop:pointcut id="txPointcut" expression="execution(* com.kuang.dao.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config>
```

**进行测试**

删掉刚才插入的数据，再次测试！

```java
public class MyTest {
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper mapper = (UserMapper) context.getBean("userMapper2");
        List<User> userList = mapper.selectUser();
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```

可以发现，sql异常，delete写错了；增加也没有生效，这就是事务。



> 思考问题

为什么需要配置事务？

- 如果不在Spring中配置，就需要我们手动提交控制事务；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！







































