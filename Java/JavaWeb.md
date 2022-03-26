# JDBC

## 1. JDBC简介

JDBC（Java DataBase Connectivity）是Java和数据库之间的一个桥梁，是一个规范而不是一个实现，能够执行SQL语句。它由一组用Java语言编写的类和接口组成。各种不同类型的数据库都有相应的实现。

![image-20220312154210128](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743888.png)



## 2. JDBC连接数据库

**注意：** 6.0版本以上，不用手动加载驱动(`Class.forName("com.mysql.jdbc.Driver")`)，直接使用即可！

```java
public class JDBCTest {
    public static void main(String[] args) {
        //1. 通过DriverManager来获得数据库连接
        try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/study","root","123456");
            //2. 创建一个用于执行SQL的Statement对象
             Statement statement = connection.createStatement()) {   //注意前两步都放在try()中，因为在最后需要释放资源！
            //3. 执行SQL语句，并得到结果集
            ResultSet set = statement.executeQuery("select * from student");
            //4. 查看结果
            while (set.next()) {
                System.out.println("id=" + set.getObject("sid"));
                System.out.println("name=" + set.getObject("name"));
                System.out.println("sex=" + set.getObject("sex"));
            }
        }catch (SQLException e) {
            e.printStackTrace();
        }
        //5. 释放资源，try-with-resource语法会自动帮助我们close
    }
}
```



### 2.1 DriverManager

- `DriverManager`是管理数据库驱动的



### 2.2 Connection

- `Connection`是数据库的连接对象，可以通过它来创建一个`Statement`用于执行SQL语句
- 除了普通的`Statement`，还存在`PreparedStatement`：`PreparedStatement`能够有效地预防SQL注入式攻击。它还支持事务处理



### 2.3 Statement

- 使用`executeQuery()`方法来执行`select`语句，返回一个`ResultSet对象`

- `Statement`除了执行查询语句外，还可以使用`executeUpdate()`方法来执行增删改语句，它会返回一个int类型，表示执行后`受影响的行数`，可以通过它来判断语句是否执行成功



### 2.4 SQL注入攻击

当用到的SQL语句是`拼接`出来的，其中有一部分内容是由用户从客户端传入，所以当用户传入的数据中`包含sql关键字`时,就有可能通过这些关键字改变sql语句的语义，从而执行一些特殊的操作，从而跳过程序处理的业务逻辑，这样的攻击方式就叫做sql注入攻击



**通过一个登录demo来模仿一个SQL注入攻击：**

user数据库表中的数据：![image-20220312173310046](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743889.png)

```java
public class LoginTest {
    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/study", "root", "123456");
             Statement statement = connection.createStatement()) {

            Scanner scanner = new Scanner(System.in);
            System.out.print("请输入用户名：");
            String username = scanner.nextLine();
            System.out.print("请输入密码：");
            String pwd = scanner.nextLine();

            String sql = " select * from user where username = '"+username+"'and pwd = '"+pwd+"' ";
            ResultSet res = statement.executeQuery(sql);
            //将执行的sql打印出来查看
            System.out.println(sql);
            while (res.next()){
                String name = res.getString(1);
                System.out.println(name+" 登陆成功！");
            }
        }catch (SQLException e){
            e.printStackTrace();
        }
    }
}
```

测试：

```
请输入用户名：root
请输入密码：safis'or'1=1
 select * from user where username = 'root'and pwd = 'safis'or'1=1' 
root 登陆成功！

Process finished with exit code 0
```

很明显输入的密码是错误的，但是却登录成功了，通过打印出来的sql语句可以看到sql语句时一直成立的，这就是所谓的sql注入攻击。

使用`PreparedStatement`先将sql语句进行预编译，能有效的防止sql注入攻击问题。



### 2.5 预编译机制

**PreparedStatement预编译的机制：**

PreparedStatement先将sql语句的主干传给数据库服务器进行预编译，其中需要用到的参数可以先用 "?" 进行占位，之后在使用预编译对象的方法将参数传给服务器，这个时候无论参数中是否有sql语句的关键字，数据库服务器都只是把他当作字符串参数使用，关键字不会起作用。从而达到防注入问题。



**将上面登录demo使用`PreparedStatement`进行改进：**

```java
public class LoginTest {
    public static void main(String[] args) {
        try{
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/study", "root", "123456");

            Scanner scanner = new Scanner(System.in);
            System.out.print("请输入用户名：");
            String username = scanner.nextLine();
            System.out.print("请输入密码：");
            String pwd = scanner.nextLine();

            String sql = " select * from user where username = ? and pwd = ? ";

            //使用PreparedStatement预编译
            PreparedStatement statement = connection.prepareStatement(sql);
            //将值传入对应的'?'位置
            statement.setString(1, username);
            statement.setString(2, pwd);

            //执行sql
            ResultSet res = statement.executeQuery();

            //输出statement，查看sql语句
            System.out.println(statement);

            while (res.next()){
                String name = res.getString(1);
                System.out.println(name+" 登陆成功！");
            }

            //关闭连接，释放资源（先开后关）
            statement.close();
            connection.close();

        }catch (SQLException e){
            e.printStackTrace();
        }
    }
}
```

测试：

```
请输入用户名：root
请输入密码：sadlfj'or'1=1
com.mysql.cj.jdbc.ClientPreparedStatement:  select * from user where username = 'root' and pwd = 'sadlfj''or''1=1' 

Process finished with exit code 0
-------------------------------------------------

请输入用户名：root
请输入密码：123456
com.mysql.cj.jdbc.ClientPreparedStatement:  select * from user where username = 'root' and pwd = '123456' 
root 登陆成功！

Process finished with exit code 0
```

当用户输入的参数一旦出现`'`时，会被变为转义形式`\'`，而最外层有一个真正的`'`来将我们输入的内容进行包裹，因此它能够有效地防止SQL注入攻击！



### 2.6 执行批量处理操作

当我们要执行很多条语句时，可以不用一次一次地提交，而是一口气全部交给数据库处理，这样会节省很多的时间。

```java
public class DML {
    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/study", "root", "lrr0110123+");
             Statement statement = connection.createStatement()){

            //添加每条批处理语句
            statement.addBatch("insert into student values(2000, '大虎', '男')");
            statement.addBatch("insert into student values(2001, '大亮', '男')");
            statement.addBatch("insert into student values(2002, '大张', '女')");
            //一起执行
            statement.executeBatch();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```



## 3. JDBC管理事务

**JDBC默认的事务处理行为是自动提交，所以前面我们执行一个SQL语句就会被直接提交（相当于没有启动事务）**，所以JDBC需要进行事务管理时，首先要通过**Connection对象调用setAutoCommit(false) 方法**, 将SQL语句的提交（commit）由驱动程序转交给应用程序负责。

```java
connection.setAutoCommit(false);   //关闭自动提交后相当于开启事务。
// SQL语句
// SQL语句
// SQL语句
connection.commit();    或 connection.rollback();
```



一旦关闭自动提交，那么现在执行所有的操作如果在最后不进行`commit()`来提交事务的话，那么所有的操作都会丢失，只有提交之后，所有的操作才会被保存！

也可以使用`rollback()`来手动回滚之前的全部操作！



**提交操作：**

```java
public static void main(String[] args) throws ClassNotFoundException {
    try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
         Statement statement = connection.createStatement()){

        connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
        statement.executeUpdate("insert into user values ('a', 1234)");
        statement.executeUpdate("insert into user values ('b', 1234)");
        statement.executeUpdate("insert into user values ('c', 1234)");

        connection.commit();   //如果前面任何操作出现异常，将不会执行commit()，之前的操作也就不会生效
    }catch (SQLException e){
        e.printStackTrace();
    }
}
```



**回滚操作：**

```java
public static void main(String[] args) throws ClassNotFoundException {
    try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
         Statement statement = connection.createStatement()){

        connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
        statement.executeUpdate("insert into user values ('a', 1234)");
        statement.executeUpdate("insert into user values ('b', 1234)");

        connection.rollback();   //回滚，撤销前面全部操作

        statement.executeUpdate("insert into user values ('c', 1234)");

        connection.commit();   //提交事务（注意，回滚之前的内容都没了）

    }catch (SQLException e){
        e.printStackTrace();
    }
}
```



**创建一个回滚点来实现定点回滚：**

```java
public static void main(String[] args) throws ClassNotFoundException {
    try (Connection connection = DriverManager.getConnection("URL","用户名","密码");
         Statement statement = connection.createStatement()){

        connection.setAutoCommit(false);  //关闭自动提交，现在将变为我们手动提交
        statement.executeUpdate("insert into user values ('a', 1234)");
        
        Savepoint savepoint = connection.setSavepoint();   //创建回滚点
        statement.executeUpdate("insert into user values ('b', 1234)");

        connection.rollback(savepoint);   //回滚到回滚点，撤销前面全部操作

        statement.executeUpdate("insert into user values ('c', 1234)");

        connection.commit();   //提交事务（注意，回滚之前的内容都没了）

    }catch (SQLException e){
        e.printStackTrace();
    }
}
```





## 4. 数据库连接池

### 4.1 简介

一个数据库连接对象均对应一个物理数据库连接，每次操作都打开一个物理连接，使用完都关闭连接，这样造成系统的性能低下。使用数据库连接池可以节约资源，程序更加高效。

数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。这项技术能明显提高对数据库操作的性能。数据库连接池其实就是一个容器（集合），存放着数据库连接。

连接池基本的思想是在系统初始化的时候，将数据库连接作为对象存储在内存中，当用户需要访问数据库时，并非建立一个新的连接，而是从连接池中取出一个已建立的空闲连接对象。使用完毕后，用户也并非将连接关闭，而是将连接放回连接池中，以供下一个请求访问使用。而连接的建立、断开都由连接池自身来管理。同时，还可以通过设置连接池的参数来控制连接池中的初始连接数、连接的上下限数以及每个连接的最大使用次数、最大空闲时间等等。也可以通过其自身的管理机制来监视数据库连接的数量、使用情况等。

 

### 4.2 Java中常见的数据库连接池

1. C3P0：是一个开放源代码的JDBC连接池，它在lib目录中与Hibernate一起发布，包括了实现jdbc3和jdbc2扩展规范说明的Connection 和Statement 池的DataSources 对象。
2. Druid：Druid不仅是一个数据库连接池，还包含一个ProxyDriver、一系列内置的JDBC组件库、一个SQL Parser。支持所有JDBC兼容的数据库，包括Oracle、MySql、Derby、Postgresql、SQL Server、H2等。
3. Proxool：是一个Java SQL Driver驱动程序，提供了对选择的其它类型的驱动程序的连接池封装。可以非常简单的移植到现存的代码中，完全可配置，快速、成熟、健壮。可以透明地为现存的JDBC驱动程序增加连接池功能。
4. DBCP：DBCP是一个依赖Jakarta commons-pool对象池机制的数据库连接池，DBCP可以直接的在应用程序中使用，Tomcat的数据源使用的就是DBCP。

我们在使用连接池时，连接池给我们提供了获取数据库连接的方法，但使用完成后还是要调用 close 方法来关闭连接。但是实际上使用连接池时调用 close() 方法实际上并没有关闭连接，只是将该连接归还给连接池。



### 4.3 C3P0连接池

C3P0是一个开源的JDBC连接池，它实现了数据源与JNDI绑定，支持JDBC3规范和实现了JDBC2的标准扩展说明的Connection和Statement池的DataSources对象。即将用于连接数据库的连接整合在一起形成一个随取随用的数据库连接池（Connection pool）。



#### 4.1.1 基于maven项目使用C3P0

1、添加依赖：

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.12</version>
</dependency>
<dependency>
  <groupId>com.mchange</groupId>
  <artifactId>c3p0</artifactId>
  <version>0.9.5.2</version>
</dependency>
```

因为使用的是 mysql，所以我们要添加 mysql 的驱动包依赖。



2、新建 c3p0 的配置文件 `c3p0-config.xml`，并将该配置文件放在 `resource` 包下。只有放在 resource 包下，编译过后你才会发现 cp30 的配置文件生成在 target 的classes 文件夹下。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!--使用默认的配置读取连接池对象-->
    <default-config>
        <!--连接参数-->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/db_test</property>
        <property name="user">root</property>
        <property name="password">123456</property>
 
        <!--连接池参数-->
        <!--初始化申请的连接数量。在初始化时就申请的连接数-->
        <property name="initialPoolSize">5</property>
        <!--超时时间-->
        <property name="checkoutTimeout">3000</property>
        <property name="maxIdleTime">30</property>
        <!--最大的连接数量。超过该数目不会再申请多的连接-->
        <property name="maxPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxStatements">200</property>
    </default-config>
 
    <!--配置连接池mysql-->
    <named-config name="mysql">
        <property name="acquireIncrement">50</property>
        <property name="initialPoolSize">100</property>
        <property name="minPoolSize">50</property>
        <property name="maxPoolSize">1000</property><!-- intergalactoApp adopts a different approach to configuring statement caching -->
        <property name="maxStatements">0</property>
        <property name="maxStatementsPerConnection">5</property>
    </named-config>
 
    <!--配置连接池2-->
    <!--......-->
</c3p0-config>
```

在配置文件中可以配置多个连接池，default-config 是默认的连接池配置，可以通过 name-config 标签来配置其他的连接池配置，通过 name 属性为该配置指定名称。在使用额外的连接池配置时，最终的配置信息是默认配置和该额外的配置的并集，并且以额外的配置优先。

在获取连接池对象时，如果不传参则使用的是默认配置，也可以通过使用 name 参数来使用指定配置。代码如下，我们测试一下连接池是否配置成功：

```java
public class CP30_test {
    public void test01() throws SQLException {
        //1.创建数据库连接池对象。不指定名称使用的是默认配置，即default-config
        DataSource ds = new ComboPooledDataSource();
        //可以通过参数来使用指定的配置
        //DataSource ds = new ComboPooledDataSource("mysql-config");
        //2.获取连接对象
        Connection conn = ds.getConnection();
        //3.打印
        System.out.println(conn);
    }
}
```

打印结果：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251744484.png)

说明配置已经成功。我们可以通过该连接池来获取连接对象，JDBC 的其他步骤跟不使用连接池时一样。



3、新建`c3p0Util`工具类，方便使用连接池：

```java
public class C3P0Util {
    //使用ComboPooledDataSource来生成DataSource的实例
    private static DataSource dataSource =new ComboPooledDataSource();
 
    //从连接池中获取连接
    public static Connection getConnection() {
        try {
            return dataSource.getConnection();
        }catch (SQLException e) {
            // TODO Auto-generated catch block
            throw new RuntimeException();
        }
    }
 
    //释放连接回连接池
    public static void release(Connection conn, Statement stmt, ResultSet rs) {
        if (rs !=null) {
            try {
                rs.close();
            }catch (Exception e) {
                e.printStackTrace();
            }
            rs =null;
        }
        if (stmt !=null) {
            try {
                stmt.close();
            }catch (Exception e) {
                e.printStackTrace();
            }
            stmt =null;
        }
        if (conn !=null) {
            try {
                conn.close();
            }catch (Exception e) {
                e.printStackTrace();
            }
            conn =null;
        }
    }
}
```



准备工作已完成，连接池使用示例：

```java
public class TestCRUD {
    public void testInsert() {
        Connection conn =null;
        PreparedStatement ps =null;
        conn = C3P0Util.getConnection();
        try {
            ps = conn.prepareStatement("INSERT INTO users (username,PASSWORD,email,birthday) VALUES('SUN99','123','123456@qq.com','2020-01-01')");
            ps.executeUpdate();
            System.out.println("添加操作执行成功！");
        }catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
            System.out.println("添加操作执行失败！");
        }finally {
            C3P0Util.release(conn, ps,null);
        }
    }
 }
```





### 4.4 Druid连接池

#### 4.4.1 简介

Druid是阿里开源的数据库连接池，作为后起之秀，性能比dbcp、c3p0更高，使用也越来越广泛。Druid不仅是一个数据库连接池，还包含一个ProxyDriver、一系列内置的JDBC组件库、一个SQL Parser。支持所有JDBC兼容的数据库，包括Oracle、MySql、Derby、Postgresql、SQL Server、H2等。



druid的优点：

1. 高性能。性能比dbcp、c3p0高很多。
2. 只要是jdbc支持的数据库，druid都支持，对数据库的支持性好。并且Druid针对oracle、mysql做了特别优化。
3. 提供监控功能。可以监控sql语句的执行时间、ResultSet持有时间、返回行数、更新行数、错误次数、错误堆栈等信息，来了解连接池、sql语句的工作情况，方便统计、分析SQL的执行性能



#### 4.4.2 基于maven项目使用Druid连接池

1、添加依赖：

```xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.8</version>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.12</version>
</dependency>
```



2、新建 druid 的配置文件 `druid.properties`（可自定义文件名称），并将该配置文件放在 `resource` 包下（只有放在 resource 包下，编译过后你才会发现 cp30 的配置文件生成在 target 的classes 文件夹下）。

```properties
url=jdbc:mysql://localhost:3306/test
#这个可以缺省的，会根据url自动识别
driverClassName=com.mysql.jdbc.Driver
username=root
password=123456
 
##初始连接数，默认0
initialSize=10
#最大连接数，默认8
maxActive=30
#最小闲置数
minIdle=10
#获取连接的最大等待时间，单位毫秒
maxWait=2000
#缓存PreparedStatement，默认false
poolPreparedStatements=true
#缓存PreparedStatement的最大数量，默认-1（不缓存）。大于0时会自动开启缓存PreparedStatement，所以可以省略上一句设置
maxOpenPreparedStatements=20
```

此时我们就可以通过连接池来获取数据库连接了：

```java
public class DruidTest {
    
    @Test
    public void test01() {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            //数据源配置
            Properties properties=new Properties();
            //通过当前类的class对象获取资源文件
            InputStream is = DruidTest.class.getResourceAsStream("/druid.properties");
            properties.load(is);
            //返回的是DataSource，不是DruidDataSource
            DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
            //获取连接
            connection = dataSource.getConnection();
            //PreparedStatement接口
            String sql = "update user set name = 'newName' where id = 2";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.execute();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if(preparedStatement != null) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
           //关闭连接。实际上是将连接归还给连接池
            try {
                if(connection != null) {
                    connection.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



3、建立druid工具类，方便使用连接池。在该类中实现多个方法封装以我们方便使用，包括：获取数据库连接池、获取数据库的连接、关闭数据库连接资源。

```java
public class DruidUtil {
    private static DataSource ds;
    static {
        //加载配置文件和建立连接池
        try {
            Properties pro = new Properties();
            InputStream resourceAsStream = DruidUtil.class.getClassLoader().getResourceAsStream("Druid.properties");
            pro.load(resourceAsStream);
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    /**
     * 获取数据库连接池
     * @return
     */
    public static DataSource getDataSource(){
        return ds;
    }
    /**
     * 获取连接池中的一个连接
     * @return
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
    /**
     * 关闭数据库的资源  三个对象都存在时
     * @param conn
     * @param res
     * @param pstmt
     */
    public static void close(Connection conn, ResultSet res, PreparedStatement pstmt){
        if (conn!=null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (res!=null){
            try {
                res.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (pstmt!=null){
            try {
                pstmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    /**
     * 关闭数据库的连接（只存在Connection和PreparedStatement对象时）
     * @param conn
     * @param pstmt
     */
    public void close(Connection conn,PreparedStatement pstmt){
        if (conn!=null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (pstmt!=null){
            try {
                pstmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



准备工作已完成，连接池使用示例：

```java
public class DruidTest {
    public void utilTest() {
        Connection connection = null;
        ResultSet resultSet = null;
        PreparedStatement preparedStatement = null;
        try {
            connection = DruidUtil.getConnection();   //获取数据库连接
            String sql = "select * from user";
            preparedStatement = connection.prepareStatement(sql);
            resultSet = preparedStatement.executeQuery();
            while (resultSet.next()){
                Integer id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                String password = resultSet.getString("password");
                System.out.println(id+" "+name+" "+password);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DruidUtil.close(connection,resultSet,preparedStatement);   //释放资源
        }
    }
}
```







# Servlet

## 1. Servlet简介

Servlet（Server Applet）是Java Servlet的简称，称为小服务程序或服务连接器，用Java编写的服务器端程序，具有独立于平台和协议的特性

> 主要功能：交互式地浏览和生成数据，生成动态Web内容。

狭义的Servlet指Java语言实现的一个接口

广义的Servlet是指任何实现了这个Servlet接口的类

一般情况下，人们将Servlet理解为后者。Servlet运行于支持Java的应用服务器中。从原理上讲，Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器。



**Servlet各类关系图：**

![image-20220311203000847](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743890.png)



**Servlet实现过程：**

1. 客户端发送请求至服务器端
2. 服务器将请求信息发送至 Servlet
3. Servlet 生成响应内容并将其传给服务器。响应内容动态生成，通常取决于客户端的请求
4. 服务器将响应返回给客户端

## 2. HelloServlet

**步骤一：**

创建`HelloServlet`类，继承`HttpServlet`，重写`doGet`和`doPost`方法

```java
public class HelloServlet extends HttpServlet {
    //由于get或post只是请求实现的不同方式，可以相互调用，业务逻辑都一样

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入doGet方法");
        PrintWriter writer = resp.getWriter();
        writer.print("Hello, Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```



**步骤二：**

在`web.xml`中添加`Servlet`配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--注册Servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.run.HelloServlet</servlet-class>
    </servlet>
    <!--Servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

</web-app>
```



**步骤三：**

配置Tomcat，启动程序，访问设置的url：

![image-20220310235035889](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743891.png)



## 3. Servlet原理

Servlet是由Web服务器调用，Web服务器在接收到浏览器的请求后，会执行以下操作：

![image-20220311010902698](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743892.png)



## 4. 请求路径Mapping

1. 一个Servlet可以指定一个映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

   

2. 一个Servlet可以指定多个映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello1</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
   ```

   

3. 一个Servlet可以指定通用映射路径

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
       </servlet-mapping>
   ```

   

4. 默认请求路径（首页）

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>
   ```

   

5. 指定一些后缀或前缀

   ```xml
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.run</url-pattern>
           <!--注意：*号前面不能加映射路径-->
       </servlet-mapping>
   ```

   在网页中只要带有`.run`后缀的url，都能进到相应的Servlet

6. 优先级问题：

   - 指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求



自定义一个404页面：

ErrorServlet类：

```java
public class ErrorServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        PrintWriter writer = resp.getWriter();
        writer.print("<h1>404</h1>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

映射路径：

```xml
    <servlet>
        <servlet-name>error</servlet-name>
        <servlet-class>com.run.ErrorServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>error</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
```

只有访问固有的路径才能进入到正确的界面，如果访问其他，则默认进自定义的404界面



## 5. ServletContext对象

ServletContext官方叫servlet上下文。服务器会为每一个工程创建一个ServletContext对象。这个对象全局唯一，而且工程内部的所有servlet都共享这个对象。所以叫全局应用程序共享对象。

### 5.1 共享数据

在一个Servlet中保存的数据，可以在另一个Servlet中获取

1. 创建一个往Context中存放数据的类`SetServlet`

   ```java
   public class SetServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           ServletContext context = this.getServletContext();
   
           String username = "张三";     //数据
           context.setAttribute("name", username);     //将数据保存在ServletContext中，名为name，值为username
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           super.doPost(req, resp);
       }
   }
   ```

   

2. 创建一个往Context中获取数据的类`GetServlet`

   ```java
   public class GetServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           ServletContext context = this.getServletContext();
           String username = (String) (context.getAttribute("name"));
   
           resp.setContentType("text/html");
           resp.setCharacterEncoding("utf-8");
           resp.getWriter().print("名字：" + username);
   
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
   
       }
   }
   ```

3. 在`web.xml`中添加映射

   ```xml
       <servlet>
         <servlet-name>setc</servlet-name>
         <servlet-class>com.run.SetServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>setc</servlet-name>
           <url-pattern>/setc</url-pattern>
       </servlet-mapping>
       
       <servlet>
           <servlet-name>getc</servlet-name>
           <servlet-class>com.run.GetServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>getc</servlet-name>
           <url-pattern>/getc</url-pattern>
       </servlet-mapping>
   ```

4. 需要先访问`/setc`将数据存入，再访问`/getc`将数据取出，否则获取到的数据为`null`



### 5.2 获取初始化参数

1. 在`web.xml`中添加参数配置

   ```xml
       <!--配置一些Web应用初始化参数-->
       <context-param>
           <param-name>url</param-name>
           <param-value>jbdc:mysql://localhost:3306</param-value>
       </context-param>
   
       <servlet>
           <servlet-name>param</servlet-name>
           <servlet-class>com.run.ServletDemo</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>param</servlet-name>
           <url-pattern>/param</url-pattern>
       </servlet-mapping>
   ```

2. 在Java代码中通过`getInitParameter()`取得参数

   ```java
   @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           ServletContext context = this.getServletContext();
           
           String url = context.getInitParameter("url");
           resp.getWriter().print(url);
   
       }
   ```

3. 访问对应的路径，能获取url对应的参数值



### 5.3 请求转发

1. 在`web.xml`中添加mapping映射

   ```xml
       <servlet>
           <servlet-name>transmit</servlet-name>
           <servlet-class>com.run.ServletTransmit</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>transmit</servlet-name>
           <url-pattern>/transmit</url-pattern>
       </servlet-mapping>
   ```

2. 创建`ServletTransmit`类

   ```java
   public class ServletTransmit extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           ServletContext context = this.getServletContext();
   //        RequestDispatcher requestDispatcher = context.getRequestDispatcher("/param");   //转发的请求路径
   //        requestDispatcher.forward(req, resp);       //调用forward方法实现请求转发
   
           //合并：
           context.getRequestDispatcher("/param").forward(req, resp);
   
       }
   }
   ```

3. 当访问`/transmit`路径时，返回的是`/param`对应的页面，这就是转发



### 5.4 读取资源文件

`properties`：

- 只有resources下的properties文件会被默认打包，如果想Java目录下也被打包，则需要在`pom.xml`中添加配置

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
              
              <!--resources下配不配置都可以-->
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

  

- 在Java目录下新建properties文件和在resources下新建properties文件，发现都被打包到了classes路径下，这个路径称为classpath



1. 在`resources`目录下新建`db.properties`文件

   ```properties
   username=root
   password=123456
   ```

2. 新建`ServletProperties`类，读取该资源文件

   ```java
   public class ServletProperties extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");
   
           Properties properties = new Properties();
           properties.load(is);
           String user = properties.getProperty("username");
           String pwd = properties.getProperty("password");
   
           resp.getWriter().print("username: " + user + ", password: " + pwd);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

3. 添加mapping配置

   ```xml
       <servlet>
           <servlet-name>properties</servlet-name>
           <servlet-class>com.run.ServletProperties</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>properties</servlet-name>
           <url-pattern>/properties</url-pattern>
       </servlet-mapping>
   ```

4. 运行程序，访问该路径，发现读取出了资源文件中的数据

   ![image-20220311134437608](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743893.png)



## 6. Response

Web服务器接收到客户端的Http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，和一个代表相应的HttpServletResponse对象

- 获取客户端请求过来的参数：HttpServletRequest
- 给客户端相应一些信息：HttpServletResponse



### 6.1 方法分类

- 负责向浏览器发送数据的方法

  ```java
  ServletOutputStream getOutputStream() throws IOException;
  
  PrintWriter getWriter() throws IOException;
  ```

- 负责向浏览器发送响应头的方法

  ```java
      void setCharacterEncoding(String var1);
  
      void setContentLength(int var1);
  
      void setContentType(String var1);
  
      void setBufferSize(int var1);
  
      void setDateHeader(String var1, long var2);
  
      void addDateHeader(String var1, long var2);
  
      void setHeader(String var1, String var2);
  
      void addHeader(String var1, String var2);
  
      void setIntHeader(String var1, int var2);
  
      void addIntHeader(String var1, int var2);
  ```

- 响应的状态码

  ```java
      int SC_CONTINUE = 100;
      int SC_SWITCHING_PROTOCOLS = 101;
      int SC_OK = 200;
      int SC_CREATED = 201;
      int SC_ACCEPTED = 202;
      int SC_NON_AUTHORITATIVE_INFORMATION = 203;
      int SC_NO_CONTENT = 204;
      int SC_RESET_CONTENT = 205;
      int SC_PARTIAL_CONTENT = 206;
      int SC_MULTIPLE_CHOICES = 300;
      int SC_MOVED_PERMANENTLY = 301;
      int SC_MOVED_TEMPORARILY = 302;
      int SC_FOUND = 302;
      int SC_SEE_OTHER = 303;
      int SC_NOT_MODIFIED = 304;
      int SC_USE_PROXY = 305;
      int SC_TEMPORARY_REDIRECT = 307;
      int SC_BAD_REQUEST = 400;
      int SC_UNAUTHORIZED = 401;
      int SC_PAYMENT_REQUIRED = 402;
      int SC_FORBIDDEN = 403;
      int SC_NOT_FOUND = 404;
      int SC_METHOD_NOT_ALLOWED = 405;
      int SC_NOT_ACCEPTABLE = 406;
      int SC_PROXY_AUTHENTICATION_REQUIRED = 407;
      int SC_REQUEST_TIMEOUT = 408;
      int SC_CONFLICT = 409;
      int SC_GONE = 410;
      int SC_LENGTH_REQUIRED = 411;
      int SC_PRECONDITION_FAILED = 412;
      int SC_REQUEST_ENTITY_TOO_LARGE = 413;
      int SC_REQUEST_URI_TOO_LONG = 414;
      int SC_UNSUPPORTED_MEDIA_TYPE = 415;
      int SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
      int SC_EXPECTATION_FAILED = 417;
      int SC_INTERNAL_SERVER_ERROR = 500;
      int SC_NOT_IMPLEMENTED = 501;
      int SC_BAD_GATEWAY = 502;
      int SC_SERVICE_UNAVAILABLE = 503;
      int SC_GATEWAY_TIMEOUT = 504;
      int SC_HTTP_VERSION_NOT_SUPPORTED = 505;
  ```



### 6.2 常见应用

#### 6.2.1 向浏览器输出信息（前面有示例）



#### 6.2.2下载文件

1. 获取下载文件的路径
2. 下载的文件名是什么
3. 设置让浏览器支持下载的东西
4. 获取下载文件的输入流
5. 创建缓冲区
6. 获取OutputStream对象
7. 将FileOutPutStream流写入到buffer缓冲区
8. 使用OutPutStream将缓冲区中的数据输出到客户端

```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1. 获取下载文件的路径
        String realPath = "D:\\Java\\Project\\Servlet-Study\\response\\src\\main\\resources\\shejie.png";
        System.out.println("loadUrl: " + realPath);

        //2. 下载的文件名是什么
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);   //截取'/'的下一个索引，即文件名的首位

        //3. 设置让浏览器支持下载的东西
        resp.setHeader("Content-Disposition", "attachment;filename=" + fileName);

        //4. 获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);

        //5. 创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];

        //6. 获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();

        //7. 将FileOutPutStream流写入到buffer缓冲区, 使用OutPutStream将缓冲区中的数据输出到客户端
        while ((len = in.read(buffer)) > 0) {
            out.write(buffer, 0, len);
        }
        
        in.close();
        out.close();

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

运行程序，访问该Servlet，发现可以直接下载图片

![image-20220311150027847](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743894.png)

#### 6.2.3 实现重定向（重点）

一个Web资源收到客户端的请求后，会通知客户去访问另一个Web资源，这个过程叫重定向



**重定向和转发的区别：**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743895.png)

实现：只需要在响应的时候重定向即可

```java
public class RedirectServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.sendRedirect("/resp/filedown");    //重定向到resp下的filedown
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}
```

mapping映射路径：

```xml
    <servlet>
        <servlet-name>filedown</servlet-name>
        <servlet-class>com.run.FileServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>filedown</servlet-name>
        <url-pattern>/filedown</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>redirect</servlet-name>
        <servlet-class>com.run.RedirectServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>redirect</servlet-name>
        <url-pattern>/redirect</url-pattern>
    </servlet-mapping>
```

运行程序时，访问`/redirect`路径，会重定向到`filedown`请求，返回`/filedown`的路径，下载文件



**转发和重定向区别：**

- 转发使用的是`getRequestDispatcher()`方法；重定向使用的是`sendRedirect()`
- 转发：浏览器URL的地址栏不变；重定向：浏览器URL的地址栏改变
- 转发是服务器行为；重定向是客户端行为
- 转发是浏览器只做了一次访问请求；重定向是浏览器做了至少两次的访问请求
- 转发2次跳转之间传输的信息不会丢失；重定向2次跳转之间传输的信息会丢失（request范围）

getRequestDispatcher是服务器内部跳转，地址栏信息不变，只能跳转到web应用内的网页。 
sendRedirect是页面重定向，地址栏信息改变，可以跳转到任意网页。 



**重定向的使用：**

1. 在`index.jsp`中写一个登录界面

   ```jsp
   <html>
   <body>
       <h2>Hello World!</h2>
   
       <form action="${pageContext.request.contextPath}/login" method="get">
   
           <%@ page contentType="text/html; charset=UTF-8"%>
           用户名：<input type="text" name="username"> <br/>
           密码：<input type="password" name="password"> <br/>
           <input type="submit">
       </form>
   </body>
   </html>
   ```

2. 创建`RequestTest`类，继承`HttpServlet`，然后在xml文件中配置相关映射路径

   ```java
   public class RequestTest extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //接收参数
           String username = req.getParameter("username");
           String password = req.getParameter("password");
           System.out.println(username + ": " + password);
           //重定向新的页面
           resp.sendRedirect("/resp/success.jsp");
   
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

   

   ```xml
       <servlet>
           <servlet-name>request</servlet-name>
           <servlet-class>com.run.RequestTest</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>request</servlet-name>
           <url-pattern>/login</url-pattern>
       </servlet-mapping>
   ```

3. 创建`success.jsp`文件，编写重定向后的新页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>success</title>
   </head>
   <body>
   <h1>登录成功</h1>
   </body>
   </html>
   ```

4. 运行程序，输入账号密码后点击登录，可以看到`HttpServletRequest`已经接收到了来自jsp页面的参数(控制台有相应的输出)，接着页面跳转到了一个新的页面

   ![image-20220311160604353](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743896.png)

   ![image-20220311160702252](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743897.png)

   ![image-20220311160725319](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743898.png)





## 7. Request

HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息。



**HttpServletRequest常见应用：**

- 接收前端传来的参数

  ```
  req.getParameter("");
  ```

  

-  请求转发

  ```java
  req.getRequestDispatcher("/test.jsp").forward(req, resp);
  ```







# Cookie Session

**会话（Session）**跟踪是Web程序中常用的技术，用来**跟踪用户的整个会话**。常用的会话跟踪技术是Cookie与Session。**Cookie通过在客户端记录信息确定用户身份**，**Session通过在服务器端记录信息确定用户身份**。



## 1. Cookie

### 1.1 什么是Cookie

Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端会把Cookie保存起来。

当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。



### 1.2 创建一个Cookie

1. 服务端从客户端请求中获取Cookie

2. 服务端给客户端响应一个Cookie

   ```java
   public class CookieDemo01 extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //乱码问题
           req.setCharacterEncoding("GBK");
           resp.setCharacterEncoding("GBk");
   
           PrintWriter out = resp.getWriter();
   
           //1.服务端从客户端请求中获取Cookie
           Cookie[] cookies = req.getCookies();        //Cookie存在多个
   
           if (cookies != null) {
               out.write("您上次访问的时间是：");
               for (int i = 0; i < cookies.length; i++) {
                   Cookie cookie = cookies[i];
                   //获取cookie的名字
                   if (cookie.getName().equals("lastLoginTime")) {
                       //获取cookie中的值
                       long lastLoginTime = Long.parseLong(cookie.getValue());     //将字符串变成long类型（valueOf也可以，其本质就是调用parseLong）
                       Date date = new Date(lastLoginTime);
                       out.write(date.toString());
                   }
               }
           }else {
               out.write("这是您第一次访问本站点");
           }
   
           //2.服务端给客户端响应一个Cookie
           Cookie cookie = new Cookie("lastLoginTime", String.valueOf(System.currentTimeMillis()));
           cookie.setMaxAge(24*60*60);     //cookie有效期(单位s)
           resp.addCookie(cookie);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

3. 编写mapping映射路径，配置Tomcat，运行项目



首页， Cookie中没有`lostLoginTime`：![image-20220311212925136](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743899.png)

第一次访问：由于是第一次访问，所以没有时间，不显示 "这是您第一次访问本站点" 是因为网站中有一些默认的cookie，所以cookie并不为null：![image-20220311213209699](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743900.png)

第二次访问：![image-20220311214013435](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743901.png)





## 2. Session（重点）

### 2.1 什么是Session

Session是另一种记录客户状态的机制，不同的是**Cookie*保存在客户端**浏览器**中，而**Session**保存在**服务器**上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。

每个用户访问服务器都会建立一个session，那服务器是怎么标识用户的唯一身份呢？事实上，用户与服务器建立连接的同时，服务器会自动为其分配一个SessionId。


### 2.2 创建一个Session

```java
public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();

        //给Session中存东西
        session.setAttribute("name", "会话");

        //获取Session的ID
        String sessionId = session.getId();

        //判断Session是不是新创建的
        if (session.isNew()) {
            resp.getWriter().write("session创建成功，ID：" + sessionId);
        } else {
            resp.getWriter().write("session已经在服务器中存在了，ID：" + sessionId);
        }

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

编写mapping映射路径，配置Tomcat，运行项目：

![image-20220311221540261](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743903.png)



再创建一个类，得到Session后，就可以取得里面的值

```java
public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();

        //取值
        String name = (String) session.getAttribute("name");

        resp.getWriter().write("名字：" + name);

    }
    @Override
    protected void doPost (HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

先访问`/session`给Session里面存东西，再访问`/s2`获取

![image-20220311223013996](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743904.png)



**Session的过期时间：**

1. 在`web.xml`中设置

   ```xml
   <session-config>
       <!--单位：min-->
       <session-timeout>1</session-timeout>
   </session-config>
   ```

2. 手动注销Session

   ```java
   HttpSession session = req.getSession();
   session.removeAttribute("name");
   session.invalidate();
   ```



### 2.3 Session和Cookie的区别

- Cookie数据存放在客户的浏览器上，Session数据放在服务器上
- Cookie不是很安全，别人可以分析存放在本地的Cookie并进行Cookie欺骗，考虑到安全应当使用Session
- 设置cookie时间可以使cookie过期。但是使用session-destory()，我们将会销毁会话
- session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用cookie
- 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。(Session对象没有对存储的数据量的限制，其中可以保存更为复杂的数据类型)









# JSP

## 1. 什么是JSP

JSP全名为Java Server Pages，java服务器页面。JSP是一种基于文本的程序，其特点就是HTML和Java代码共同存在！JSP是为了简化Servlet的工作出现的替代品，Servlet输出HTML非常困难，JSP就是替代Servlet输出HTML的。

Tomcat访问任何的资源都是在访问Servlet！JSP也不例外！JSP本身就是一种Servlet。其实JSP在第一次被访问的时候会被编译为HttpJspPage类（该类是HttpServlet的一个子类）



**JSP本质就是一个Servlet**

```java
//初始化
public void _jspInit() {
}

//销毁
public void _jspDestroy() {
}

//JSPService
public void _jspService(final HttpServletRequest request, final HttpServletResponse response)
```



**编译过程：**

浏览器第一次请求jsp时，Tomcat会将jsp转化成`_jsp.java`这么一个类，并将该文件编译成class文件。编译完毕后再运行class文件来响应浏览器的请求。

以后访问jsp就不再重新编译jsp文件了，直接调用class文件来响应浏览器。当然了，如果Tomcat检测到JSP页面改动了的话，会重新编译的。



**那JSP页面中的HTML排版标签是怎么样被发送到浏览器的**？通过看编译后的`_jsp.java`的源码可知，原来就是用`write()`输出。**说到底，JSP就是封装了Servlet的java程序**





## 2. EL表达式

> 为了简化咱们的jsp代码，具体一点就是为了简化在jsp里面写的那些Java代码

- 语法结构：${expression }
- 从指定的作用域取参数：${requestScope.表达式}



### 2.1 EL表达式的使用

1. 取出4个作用域中存放的值

   ```jsp
   <%
   pageContext.setAttribute("name", "page");
   request.setAttribute("name", "request");
   session.setAttribute("name", "session");
   application.setAttribute("name", "application");
   %>
   
   按普通手段取值<br>
   
   <%= pageContext.getAttribute("name")%>
   <%= request.getAttribute("name")%>
   <%= session.getAttribute("name")%>
   <%= application.getAttribute("name")%>
   
   
   <br>使用EL表达式取出作用域中的值<br>
   ${ pageScope.name }
   ${ requestScope.name }
   ${ sessionScope.name }
   ${ applicationScope.name }
   ```

2. 如果域中所存的是数组

   ```jsp
   <%
   String [] a = {"aa","bb","cc","dd"};
   pageContext.setAttribute("array", a);
   %>
         		
   使用EL表达式取出作用域中数组的值<br>
   ${array[0] } , ${array[1] },${array[2] },${array[3] }
   ```

3. 如果域中所存的是集合

   ```java
   <%
   String[] a = {"a","b","c","d","f"};
   pageContext.setAttribute("array",a);
   List<String> b = new ArrayList<>();
   b.add("aa");
   b.add("bb");
   b.add("cc");
   b.add("dd");
   b.add("ff");
   pageContext.setAttribute("li",b);
   %>
       
       
   使用EL表达式取出作用域中集合的值<br>
   ${li[0] } , ${li[1] },${li[2] },${li[3] },${li[4] }
   ```

4. 取出Map集合的值

   ```jsp
   <%
   Map map = new HashMap();
   map.put("name", "zhangsna");
   map.put("age",18);
   map.put("address","北京..");
   
   map.put("address.aa","深圳..");
   
   
   pageContext.setAttribute("map", map);
   %>
   
   使用EL表达式取出作用域中Map的值<br>
   ${map.name } , ${map.age } , ${map.address }  , ${map["address.aa"] }
   ```



## 3. JSTL

> 全称 ： JSP Standard Tag Library       Jsp标准标签库

作用：简化Jsp的代码编写。 替换` <% %> `写法。 一般与EL表达式配合



### 3.1 JSTL的使用

1. 导入依赖

2. jsp页面上方，使用taglib指令，引入标签库

   ```jsp
   <!--JSTL核心标签库-->
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

3. 注意：如果想支持EL表达式，那么引入的标签库必须选择1.1以上的版本，1.0的版本不支持EL表达式



**常用的JSTL：**

向Session中设置属性，属性的类型是List集合：

```jsp
<%
        List list = new ArrayList<>();
        list.add("zhongfucheng");
        list.add("ouzicheng");
        list.add("xiaoming");

        session.setAttribute("list", list);
%>
```

遍历session属性中的List集合,items：即将要迭代的集合。var：当前迭代到的元素

```jsp
<c:forEach  var="list" items="${list}" >
        ${list}		<br/>
</c:forEach>
```





# Filter

过滤器实际上就是对web资源进行拦截，做一些处理后再交给下一个过滤器或Servlet处理
通常都是用来拦截request进行处理的，也可以对返回的response进行拦截处理

大概流程图如下：

![image-20220312105101008](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743905.png)

**应用场景：**

- 自动登录
- 统一设置编码格式
- 访问权限控制
- 敏感字符过滤等



## 1. 编写一个Filter

1. 实现Filter接口，重写方法

   ```java
   public class CharacterEncodingFilter implements Filter {
       @Override
       //初始化：web服务器启动时初始化，随时等待过滤对象的出现
       public void init(FilterConfig filterConfig) throws ServletException {
       }
   
       @Override
       //过滤方法：对req和resp进行一些处理，然后交给下一个过滤器或Servlet处理
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           servletRequest.setCharacterEncoding("utf-8");
           servletResponse.setCharacterEncoding("utf-8");
           servletResponse.setContentType("text/html; charset=UTF-8");
   
           //交给下一个过滤器或servlet处理，如果不写程序到这就被拦截停止
           filterChain.doFilter(servletRequest, servletResponse);
       }
   
       @Override
       //销毁：web服务器关闭时，过滤器被销毁
       public void destroy() {
       }
   }
   ```

2. 在`web.xml`中配置Filter

   ```xml
       <filter>
           <filter-name>characterEncodingFilter</filter-name>
           <filter-class>com.run.filter.CharacterEncodingFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>characterEncodingFilter</filter-name>
           <!--只要是/servlet下的任何请求，都会经过这个过滤器-->
           <url-pattern>/servlet/*</url-pattern>
       </filter-mapping>
   ```



`<url-pattern>`为配置要拦截的资源：

- 以指定资源匹配。例如`"/index.jsp"`
- 以目录匹配。例如`"/servlet/*"`
- 以后缀名匹配，例如`"*.jsp"`
- 通配符，拦截所有web资源。`"/*"`



## 2. 过滤器实现登录拦截

用户登录之后可以进入主页，注销后不能进入

1. 用户登录后，向Session中存入用户的数据
2. 进入主页的时候用过滤器判断用户是否已经登录



登录界面`login.jsp`：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<h1>登录</h1>
<form action="${pageContext.request.contextPath}/svl/login" method="post">
    <input type="text" name="username">
    <input type="submit">
</form>

</body>
</html>
```



点击登录后，`LoginServlet.java`处理登录请求：

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取前端请求的参数
        String username = req.getParameter("username");
        if (username.equals("admin")) {
            req.getSession().setAttribute("USER_SESSION", req.getSession().getId());    //登录成功，将用户存入session中
            resp.sendRedirect("/cs/sys/success.jsp");
        }else {
            resp.sendRedirect("/cs/error.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

![image-20220312144747909](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743906.png)



如果登录的username不匹配，则返回`error.jsp`界面：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登录失败</h1>
<a href="${pageContext.request.contextPath}/login.jsp">返回登录</a>
</body>
</html>
```

![image-20220312144803847](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743907.png)



如果登录成功，进入sys目录下的`success.jsp`页面：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登陆成功</h1>

<a href="${pageContext.request.contextPath}/svl/logout">注销</a>
</body>
</html>
```

![image-20220312144836761](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743908.png)



现在不点击注销，返回主页后再进入登录成功的页面，发现可以进来；

接下来点击注销，`LogoutServlet.java`处理注销请求，移除该用户的session：

```java
public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object user_session = req.getSession().getAttribute("USER_SESSION");
        if (user_session != null) {
            req.getSession().removeAttribute("USER_SESSION");       //用户存在，则移除session完成注销
            resp.sendRedirect("/cs/login.jsp");
        }else {
            resp.sendRedirect("/cs/login.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



现在再进入登录成功的页面，发现显示登录失败：

![image-20220312145216738](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251743909.png)

在进入登录成功界面的时候，会先进入`SysFilter.java`过滤器：

```java
public class SysFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //将ServletRequest转为HttpServletRequest，才能获取session
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //用户注销后session为空，拦截登录页面的进入
        if (request.getSession().getAttribute("USER_SESSION") == null) {
            response.sendRedirect("/cs/error.jsp");
        }

        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

`web.xml`文件中的配置，因为登录成功界面在`/sys`目录下，所以进入的时候会先经过过滤器

```xml
    <servlet>
        <servlet-name>loginServlet</servlet-name>
        <servlet-class>com.run.servlet.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>loginServlet</servlet-name>
        <url-pattern>/svl/login</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>logoutServlet</servlet-name>
        <servlet-class>com.run.servlet.LogoutServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>logoutServlet</servlet-name>
        <url-pattern>/svl/logout</url-pattern>
    </servlet-mapping>

    <filter>
        <filter-name>sysFilter</filter-name>
        <filter-class>com.run.servlet.SysFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>sysFilter</filter-name>
        <!--过滤/sys路径下的所有-->
        <url-pattern>/sys/*</url-pattern>
    </filter-mapping>
```



**优化：**

- 代码中大量用到了`USER_SESSION`常量，可以定义一个常量类，专门用来存放。需要的时候直接`类名.常量名`的方式获取即可，如果后续需要修改常量的名字，直接在常量类中修改该常量名所对应的值。





































