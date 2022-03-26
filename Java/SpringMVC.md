# SpringMVC

## 1. 回顾MVC

### 1.1 什么是MVC

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。
- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是**降低了视图与业务逻辑间的双向偶合**。
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异。



**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**



![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203242356434.png)



### 1.2 Model 1时代

- 在web早期的开发中，通常采用的都是Model 1
- Model 1中，主要分为两层，视图层和模型层

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250003674.png)

Model1优点：架构简单，比较适合小型项目开发；

Model1缺点：JSP职责不单一，职责过重，不便于维护。



### 1.3 Model 2时代

Model2把一个项目分成三部分，包括**视图、控制、模型。**

<img src="https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251732266.png" alt="图片"  />

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给servlet
4. servlet转向到JSP，由JSP来渲染页面
5. 响应给前端更新后的页面

**职责分析：**

**Controller：控制器**

1. 取得表单数据
2. 调用业务逻辑
3. 转向指定的页面

**Model：模型**

1. 业务逻辑
2. 保存数据的状态

**View：视图**

1. 显示页面

Model2这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model 1模式的实现比较简单，适用于快速开发小规模项目，Model1中JSP页面身兼View和Controller两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。Model2消除了Model1的缺点。



### 1.4 回顾Servlet

1. 新建一个Maven工程当做父工程！pom依赖！

   ```xml
   <dependencies>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>servlet-api</artifactId>
           <version>2.5</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>jsp-api</artifactId>
           <version>2.2</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
   </dependencies>
   ```

2. 建立一个Moudle：springmvc-01-servlet ， 添加Web app的支持！

3. 导入servlet 和 jsp 的 jar 依赖

   ```xml
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.2</version>
   </dependency>
   ```

4. 编写一个Servlet类，用来处理用户的请求

   ```java
   public class HelloServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //1.获取前端参数
           String method = req.getParameter("method");
           if (method.equals("add")) {
               req.getSession().setAttribute("msg", "执行了add方法");
           } else if (method.equals("delete")) {
               req.getSession().setAttribute("msg", "执行了delete方法");
           }
   
           //2.调用业务层
   
   
           //3.视图转发或重定向
           req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req, resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

5. 在WEB-INF目录下新建一个jsp的文件夹，新建`test.jsp`

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <%--接收servlet层传过来的信息--%>
   ${msg}
   </body>
   </html>
   ```

6. 在web.xml中注册Servlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <servlet>
           <servlet-name>hello</servlet-name>
           <servlet-class>com.run.servlet.HelloServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   
       <!--session过期时间-->
   <!--    <session-config>-->
   <!--        <session-timeout>30</session-timeout>-->
   <!--    </session-config>-->
   
       <!--起始页(默认index.jsp)-->
   <!--    <welcome-file-list>-->
   <!--        <welcome-file>index.jsp</welcome-file>-->
   <!--    </welcome-file-list>-->
   </web-app>
   ```

7. 在web目录下新建`from.jsp`，用于将方法名传给servlet层

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   
   <form action="${pageContext.request.contextPath}/hello" method="post">
       <input type="text" name="method">
       <input type="submit" value="提交方法">
   </form>
   
   </body>
   </html>
   ```

8. 配置Tomcat，并启动测试

9. - 进入`/from.jsp`页面，提交add方法：

     ![image-20220324103032118](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002212.png)

     ![image-20220324104300147](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002213.png)

   - 提交delete方法，也执行成功

   

**MVC框架要做哪些事情**

1. 将url映射到java类或java类的方法
2. 封装用户提交的数据 .
3. 处理请求--调用相关的业务处理--封装响应数据 .
4. 将响应的数据进行渲染 . jsp / html 等表示层数据 .

**说明：**

​	常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等等....



## 2. 什么是SpringMVC

### 2.1 概述

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。

查看官方文档：https://docs.spring.io/spring/docs/5.2.0.RELEASE/spring-framework-reference/web.html#spring-web



**为什么要学习SpringMVC呢?**

 Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活



Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计。

DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；

正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使用SpringIoC和Aop) , 使用约定优于配置 . 能够进行简单的junit测试 . 支持Restful风格，异常处理 , 本地化 , 国际化 , 数据验证 , 类型转换 , 拦截器 等等......所以我们要学习 



### 2.2 中心控制器

​	Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

​	Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能**，**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250004214.png)

SpringMVC的原理如下图所示：

​	当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250026102.png)

### 2.3 执行原理

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250026867.png)

图为SpringMVC的一个较完整的流程图，实线表示SpringMVC框架提供的技术，不需要开发者实现，虚线表示需要开发者实现。

**简要分析执行流程**

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   

   **如上url拆分成三部分：**

   1. http://localhost:8080服务器域名
   2. SpringMVC部署在服务器上的web站点
   3. hello表示控制器

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter，如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。

在这里先听一遍原理，不理解没有关系，我们马上来写一个对应的代码实现大家就明白了







## 3. Hello SpringMVC

### 3.1 XML配置版

1、新建一个Moudle，springmvc-02-hello，添加web的支持！

2、确定导入了SpringMVC 的依赖！

3、配置`web.xml`，注册DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别：1 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <!-- /：只匹配所有请求，不包括.jsp -->
    <!-- /*：匹配所有请求，包括.jsp，导致进入spring的DispatcherServlet类，然后去寻找controller，接着找不到对应的controller所以报错-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

注意：`<url-pattern>/</url-pattern>`和`<url-pattern>/*</url-pattern>`的区别：

- `<url-pattern>/</url-pattern>`：不会匹配到*.jsp，不会进入spring的DispatcherServlet类
- `<url-pattern>/*</url-pattern>`：会匹配*.jsp，导致进入spring的DispatcherServlet类，然后去寻找controller，接着找不到对应的controller所以报错。

官方文档解释：

`<url-pattern>/</url-pattern>` ：如果我们的项目中配置了"/"，会覆盖掉tomcat中的默认servlet，当其他的url-pattern匹配不上时都会走这个servlet，它会匹配到后缀型url，它除了能够处理静态资源如“.js”，“.css”，".png"等，还能够处理HTTP缓存请求，媒体（音频/视频）数据流和文件下载简历。说到为什么JSP页面的请求（`*.jsp`）并不会命中这个servlet，那是因为当有这种url请求时，servlet容器内建的JSP servlet将会被调用，而这个容器内建的JSP servlet已经默认地映射在了`*.jsp`上。 

`<url-pattern>/*</url-pattern>`：会覆盖所有其它的servlet。不管你发出了什么样的请求，最终都会在这个servlet中结束。因此，对于servlet来说，这是一个很糟糕的URL模式。通常来讲，你只会想要在一个Filter中使用这种模式。它可以通过调用doFilter()方法来使请求继续。 



4、新建SpringMVC 的 配置文件：`springmvc-servlet.xml` ，在里面添加SpringMVC的三大核心配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"     
       xsi:schemaLocation="http://www.springframework.org/schema/beans       
       http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

5、添加 处理器映射器

```xml
    <!--处理映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    
```

6、添加 处理器适配器

```xml
    <!--处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

```

7、添加 视图解析器

```xml
    <!--视图解析器：DispatcherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```

8、编写要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个ModelAndView，装数据，封视图；

```java
public class HelloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中
        mv.addObject("msg", "HelloSpringMVC!");

        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello");        //只用写页面名，因为配了前后缀，地址：WEB-INF/jsp/hello.jsp

        return mv;
    }
}
```

9、将自己的类交给SpringIoC容器，注册bean

```xml
    <!--将类交给SpringIoC容器，注册bean-->
    <!--id：请求的位置；当在地址栏输入xxx/hello时，进入HelloController控制器-->
    <bean id="/hello" class="com.run.controller.HelloController"/>
```

10、在`WEB-INF`下新建jsp目录，创建要跳转的jsp页面`hello.jsp`，显示ModelandView存放的数据，以及我们的正常页面；

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
   <title>Title</title>
</head>
<body>
${msg}
</body>
</html>
```

11、配置Tomcat 启动测试，输入`/hello`进入到controller获取msg

![image-20220324130845995](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002214.png)





**可能遇到的问题：访问出现404，排查步骤：**

1. 查看控制台输出，看一下是不是缺少了什么jar包。

2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！

   ![image-20220324130939614](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002215.png)

3. 重启Tomcat 即可解决！



看这个都能理解其中的原理了，但是我们实际开发才不会这么写！我们来看个注解版实现，这才是SpringMVC的精髓，十分简单！





### 3.2 注解版

>  步骤

**1、新建一个Moudle，springmvc-03-annotation 。添加web支持！**

**2、由于Maven可能存在资源过滤的问题，我们将`pom.xml`配置完善**

```
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

**3、在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们在父依赖中已经引入了！**

**4、配置web.xml**

注意点：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">

   <!--1.注册servlet-->
   <servlet>
       <servlet-name>SpringMVC</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:springmvc-servlet.xml</param-value>
       </init-param>
       <!-- 启动顺序，数字越小，启动越早 -->
       <load-on-startup>1</load-on-startup>
   </servlet>

   <!--所有请求都会被springmvc拦截 -->
   <servlet-mapping>
       <servlet-name>SpringMVC</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>

</web-app>
```

**/ 和 /\* 的区别：**

`< url-pattern > / </ url-pattern >` 不会匹配到.jsp， 只针对我们编写的请求；即：.jsp 不会进入spring的 DispatcherServlet类 。

`< url-pattern > /* </ url-pattern >` 会匹配 *.jsp，会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。

- 注意web.xml版本问题，要最新版！
- 注册DispatcherServlet
- 关联SpringMVC的配置文件
- 启动级别为1
- 映射路径为 / 【不要用/*，会404】



**5、添加Spring MVC配置文件**

在resource目录下添加`springmvc-servlet.xml`配置文件，配置的形式与Spring容器配置基本类似，为了支持基于注解的IoC，设置了自动扫描包的功能，具体配置信息如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 自动扫描包，让指定包下的注解生效,由IoC容器统一管理 -->
    <context:component-scan base-package="com.run.controller"/>
    <!-- 让Spring MVC不处理静态资源(.css .js .html .png .mp3...)-->
    <mvc:default-servlet-handler />


    <!--
    支持mvc注解驱动
        在spring中一般采用@RequestMapping注解来完成映射关系
        要想使@RequestMapping注解生效
        必须向上下文中注册DefaultAnnotationHandlerMapping
        和一个AnnotationMethodHandlerAdapter实例
        这两个实例分别在类级别和方法级别处理。
        而annotation-driven配置帮助我们自动完成上述两个实例的注入。
     -->
    <mvc:annotation-driven />

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

- 让IoC的注解生效

- 静态资源过滤 ：HTML . JS . CSS . 图片 ， 视频 .....

- MVC的注解驱动

- 配置视图解析器

  

**6、创建Controller**

编写一个Java控制类：`com.run.controller.HelloController` , 注意编码规范

```
@Controller
@RequestMapping("/HelloController")
public class HelloController {

    @RequestMapping("/hello")
    public String hello(Model model) {
        //封装数据：通过Model，可以在jsp页面取出该msg
        model.addAttribute("msg", "Hello SpringMVC By Annotation!");

        //返回hello字符串，该字符串会被返回到视图解析器，拼接前后缀，形成地址WEB-INF/jsp/hello.jsp
        return "hello";
    }
}
```

- `@Controller`是为了让Spring IOC容器初始化时自动扫描到；
- `@RequestMapping`是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是`/HelloController/hello`；
- 方法中声明Model类型的参数是为了把Action中的数据带到视图中；
- 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成`WEB-INF/jsp/hello.jsp。`



**7、创建视图层**

在`WEB-INF/jsp`目录中创建`hello.jsp` ， 视图可以直接取出并展示从Controller带回的信息；

可以通过EL表示取出Model中存放的值，或者对象；

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>
```



**8、配置Tomcat运行**

配置Tomcat，开启服务器 ， 访问对应的请求路径！

![image-20220324140246590](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002216.png)

**运行成功！**





>  小结

实现步骤其实非常的简单：

1. 新建一个web项目
2. 导入相关jar包
3. 编写`web.xml` , 注册DispatcherServlet
4. 编写`springmvc-servlet.xml`配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试



使用springMVC必须配置的三大件：

**处理器映射器、处理器适配器、视图解析器**



通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大段的xml配置







## 4. 控制器和RestFul

### 4.1 控制器Controller

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现
- 控制器负责解析用户的请求并将其转换为一个模型
- 在Spring MVC中一个控制器类可以包含多个方法
- 在Spring MVC中，对于Controller的配置方式有很多种



#### 4.1.1 实现Controller接口

Controller是一个接口，在`org.springframework.web.servlet.mvc`包下，接口中只有一个方法：

```java
//实现该接口的类获得控制器功能
public interface Controller {
    //处理请求且返回一个模型与视图对象
    ModelAndView handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws Exception;
}
```



**测试**

1. 新建一个Moudle，springmvc-04-controller。将刚才的03 拷贝一份, 我们进行操作！

2. 删掉HelloController，mvc的配置文件只留下 视图解析器！
   
3. 编写一个Controller类，ControllerTest1，实现Controller接口

   ```java
   //定义控制器
   //注意点：不要导错包，实现Controller接口，重写方法；
   public class ControllerTest1 implements Controller {
   
       public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
           //返回一个模型视图对象
           ModelAndView mv = new ModelAndView();
           mv.addObject("msg","Test1Controller");
           mv.setViewName("test");
           return mv;
       }
   }
   ```

4. 编写完毕后，去Spring配置文件中注册请求的bean；name对应请求路径，class对应处理请求的类

   ```xml
   <bean name="/t1" class="com.kuang.controller.ControllerTest1"/>
   ```

5. 编写前端`test.jsp`，注意在`WEB-INF/jsp`目录下编写，对应我们的视图解析器

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Kuangshen</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

6. 配置Tomcat运行测试，我这里没有项目发布名配置的就是一个 / ，所以请求不用加项目名，OK！

   

   ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027104.png)

**说明：**

- 实现接口Controller定义控制器是较老的办法
- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller；定义的方式比较麻烦；



#### 4.1.2 使用注解@Controller

- `@Controller`注解类型用于声明Spring类的实例是一个控制器（在讲IoC时还提到了另外3个注解：`@Component`、`@Service`、`@Repository`）；

- Spring可以使用**扫描机制**来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需要在配置文件中声明组件扫描。

  ```xml
  <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
  <context:component-scan base-package="com.kuang.controller"/>
  ```

- 增加一个ControllerTest2类，使用注解实现；

  ```java
  //@Controller注解的类会自动添加到Spring上下文中
  @Controller
  public class ControllerTest2{
  
      //映射访问路径
      @RequestMapping("/t2")
      public String index(Model model){
          //Spring MVC会自动实例化一个Model对象用于向视图中传值
          model.addAttribute("msg", "ControllerTest2");
          //返回视图位置
          return "test";
      }
  }
  ```

- 运行tomcat测试

  ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027634.png)

  

**可以发现，我们的两个请求都可以指向一个视图`test`，但是页面结果的结果是不一样的，从这里可以看出视图是被复用的，而控制器与视图之间是弱偶合关系。**

**注解方式是平时使用的最多的方式！**



### 4.2 RequestMapping

**`@RequestMapping`：**

- `@RequestMapping`注解用于映射url到控制器类或一个特定的处理程序方法。可用于**类**或**方法**上。

  - 用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

- 为了测试结论更加准确，我们可以加上一个项目名测试 myweb

- 只注解在方法上面

  ```java
  @Controller
  public class TestController {
      @RequestMapping("/h1")
      public String test(){
          return "test";
      }
  }
  ```

  访问路径：`http://localhost:8080/项目名/h1`

- 同时注解类与方法

  ```java
  @Controller
  @RequestMapping("/admin")
  public class TestController {
      @RequestMapping("/h1")
      public String test(){
          return "test";
      }
  }
  ```

  访问路径：`http://localhost:8080/项目名/admin/h1`  , 需要先指定类的路径再指定方法的路径；





### 4.3 RestFul风格

**概念**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。



**功能**

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。



**传统方式操作资源**  ：通过不同的参数来实现不同的效果！方法单一，post 和 get

​	http://127.0.0.1/item/queryItem.action?id=1 查询，GET

​	http://127.0.0.1/item/saveItem.action 新增，POST

​	http://127.0.0.1/item/updateItem.action 更新，POST

​	http://127.0.0.1/item/deleteItem.action?id=1 删除，GET或POST



**使用Restful操作资源** ：可以通过不同的**请求方式**来实现不同的效果！如下：请求地址一样，但是功能可以不同！

​	http://127.0.0.1/item/1 查询,GET

​	http://127.0.0.1/item 新增,POST

​	http://127.0.0.1/item 更新,PUT

​	http://127.0.0.1/item/1 删除,DELETE



**学习测试**

1. 在新建一个类 RestFulController

   ```java
   @Controller
   public class RestFulController {
   }
   ```

2. 在Spring MVC中可以使用  `@PathVariable` 注解，让**方法参数的值**对应绑定到一个**URI模板变量**上。

   ```java
   @Controller
   public class RestFulController {
   
       //映射访问路径
       @RequestMapping("/commit/{p1}/{p2}")
       public String index(@PathVariable int p1, @PathVariable int p2, Model model){
           
           int result = p1+p2;
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg", "结果："+result);
           //返回视图位置
           return "test";
           
       }
       
   }
   ```

3. 我们来测试请求查看下

   ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027607.png)

4. 思考：使用路径变量的好处？

5. - 使路径变得更加简洁；

   - 获得参数更加方便，框架会自动进行类型转换。

   - 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

     

     ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027184.png)

6. 我们来修改下对应的p2参数类型，再次测试

   ```java
   //映射访问路径
   @RequestMapping("/commit/{p1}/{p2}")
   public String index(@PathVariable int p1, @PathVariable String p2, Model model){
   
       String result = p1+p2;
       //Spring MVC会自动实例化一个Model对象用于向视图中传值
       model.addAttribute("msg", "结果："+result);
       //返回视图位置
       return "test";
   
   }
   ```

   

   ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027533.png)



**使用method属性指定请求类型**

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE等

我们来测试一下：

- 增加一个方法

  ```java
  //映射访问路径,必须是POST请求
  @RequestMapping(value = "/hello",method = {RequestMethod.POST})
  public String index2(Model model){
      model.addAttribute("msg", "hello!");
      return "test";
  }
  ```

- 我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

  ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027956.png)

- 如果将POST修改为GET则正常了；

  ```java
  //映射访问路径,必须是Get请求
  @RequestMapping(value = "/hello",method = {RequestMethod.GET})
  public String index2(Model model){
      model.addAttribute("msg", "hello!");
      return "test";
  }
  ```

  

  ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027608.png)



**小结：**

Spring MVC 的 `@RequestMapping` 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。

**参数值相同的情况下，可以通过不同的**请求方式**来实现不同的效果**

**所有的地址栏请求默认都会是 GET 类型的。**



可以用下面的注解来代替`@RequestMapping(method =RequestMethod.XXX)`

方法级别的注解变体有如下几个：组合注解

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

`@GetMapping` 是一个组合注解，平时使用的会比较多！

它所代表的是 `@RequestMapping(method =RequestMethod.GET)` 的一个快捷方式。





## 5. 跳转和数据处理

### 5.1 结果跳转方式

#### 5.1.1 ModelAndView

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面 .

页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
      id="internalResourceViewResolver">
    <!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/" />
    <!-- 后缀 -->
    <property name="suffix" value=".jsp" />
</bean>
```

对应的controller类

```java
public class ControllerTest1 implements Controller {

    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //返回一个模型视图对象
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","ControllerTest1");
        mv.setViewName("test");
        return mv;
    }
}
```



#### 5.1.2 ServletAPI

通过设置ServletAPI , 不需要视图解析器 .

1、通过HttpServletResponse进行输出

2、通过HttpServletResponse实现重定向

3、通过HttpServletResponse实现转发

```java
@Controller
public class ResultGo {

    @RequestMapping("/result/t1")
    public void test1(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
        rsp.getWriter().println("Hello,Spring BY servlet API");
    }

    @RequestMapping("/result/t2")
    public void test2(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
        //重定向
        rsp.sendRedirect("/index.jsp");
    }

    @RequestMapping("/result/t3")
    public void test3(HttpServletRequest req, HttpServletResponse rsp) throws Exception {
        //转发
        req.setAttribute("msg","/result/t3");
        req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
    }

}
```



#### 5.1.3 SpringMVC

**通过SpringMVC来实现转发和重定向 - 无需视图解析器：**

测试前，需要将视图解析器注释掉

```java
@Controller
public class ResultSpringMVC {
    @RequestMapping("/rsm/t1")
    public String test1(){
        //转发
        return "/index.jsp";
    }

    @RequestMapping("/rsm/t2")
    public String test2(){
        //转发二
        return "forward:/index.jsp";
    }

    @RequestMapping("/rsm/t3")
    public String test3(){
        //重定向到首页
        return "redirect:/index.jsp";
    }
}
```



**通过SpringMVC来实现转发和重定向 - 有视图解析器：**

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
      id="internalResourceViewResolver">
    <!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/" />
    <!-- 后缀 -->
    <property name="suffix" value=".jsp" />
</bean>
```

重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题.

可以重定向到另外一个请求实现 .

```java
@Controller
public class ResultSpringMVC2 {
    @RequestMapping("/rsm2/t1")
    public String test1(){
        //转发
        return "test";
    }

    @RequestMapping("/rsm2/t2")
    public String test2(){
        //重定向
        return "redirect:/index.jsp";
        //return "redirect:hello.do"; 	//hello.do为另一个请求/
    }
}
```





### 5.2 数据处理

#### 5.2.1 前端提交的数据处理

**1、提交的域名称和处理方法的参数名一致**

提交数据 : `http://localhost:8080/hello?name=kuangshen`

处理方法 :

```java
@RequestMapping("/hello")
public String hello(String name){
    System.out.println(name);
    return "hello";
}
```

后台输出 : kuangshen



**2、提交的域名称和处理方法的参数名不一致**

提交数据 :` http://localhost:8080/hello?username=kuangshen`

处理方法 :

```java
//@RequestParam("username") : username提交的域的名称 .
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
    System.out.println(name);
    return "hello";
}
```

后台输出 : kuangshen



**3、提交的是一个对象**

要求提交的表单域和对象的属性名一致  , 参数使用对象即可

1、实体类

```java
public class User {
    private int id;
    private String name;
    private int age;
    //构造
    //get/set
    //tostring()
}
```

2、提交数据 : `http://localhost:8080/mvc04/user?name=kuangshen&id=1&age=15`

3、处理方法 :

```java
@RequestMapping("/user")
public String user(User user){
    System.out.println(user);
    return "hello";
}
```

后台输出 : User { id=1, name='kuangshen', age=15 }

说明：如果使用对象的话，前端传递的**参数名**和 实体类的**对象名必须一致**，否则就是null。



#### 5.2.2 数据显示到前端

**第一种 : 通过ModelAndView**

我们前面一直都是如此，就不过多解释

```java
public class ControllerTest1 implements Controller {

    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //返回一个模型视图对象
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","ControllerTest1");
        mv.setViewName("test");
        return mv;
    }
}
```



**第二种 : 通过ModelMap**

ModelMap

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name);
    model.addAttribute("name",name);
    System.out.println(name);
    return "hello";
}
```



**第三种 : 通过Model**

Model

```java
@RequestMapping("/ct2/hello")
public String hello(@RequestParam("username") String name, Model model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name);
    model.addAttribute("msg",name);
    System.out.println(name);
    return "test";
}
```



#### 5.2.3 对比

就对于新手而言简单来说使用区别就是：

- Model：只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
- ModelMap：继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
- ModelAndView：可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。



当然更多的以后开发考虑的更多的是性能和优化，就不能单单仅限于此的了解。

**请使用80%的时间打好扎实的基础，剩下18%的时间研究框架，2%的时间去学点英文，框架的官方文档永远是最好的教程。**



#### 5.2.4 乱码问题

测试步骤：

1、我们可以在首页编写一个提交的表单

```jsp
<form action="/e/t" method="post">
  <input type="text" name="name">
  <input type="submit">
</form>
```

2、后台编写对应的处理类

```java
@Controller
public class Encoding {
    @RequestMapping("/e/t")
    public String test(Model model,String name){
        model.addAttribute("msg", name); 	//表单提交的值返回给前端
        return "test"; 		//跳转到test页面显示输入的值
    }
}
```

3、输入中文测试，发现乱码

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027857.png)

不得不说，乱码问题是在我们开发中十分常见的问题，也是让我们程序猿比较头大的问题！

以前乱码问题通过过滤器解决 , 而SpringMVC给我们提供了一个过滤器 , 可以在`web.xml`中配置

修改了xml文件需要重启服务器！

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

但是我们发现 , 有些**极端情况**下这个过滤器对get的支持不好

处理方法：

1、修改tomcat配置文件 ：设置编码！

```xml
<Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

2、自定义过滤器

```java
package com.kuang.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;

/**
 * 解决get和post请求 全部乱码的过滤器
 */
public class GenericEncodingFilter implements Filter {

    @Override
    public void destroy() {
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        //处理response的字符编码
        HttpServletResponse myResponse=(HttpServletResponse) response;
        myResponse.setContentType("text/html;charset=UTF-8");

        // 转型为与协议相关对象
        HttpServletRequest httpServletRequest = (HttpServletRequest) request;
        // 对request包装增强
        HttpServletRequest myrequest = new MyRequest(httpServletRequest);
        chain.doFilter(myrequest, response);
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

}

//自定义request对象，HttpServletRequest的包装类
class MyRequest extends HttpServletRequestWrapper {

    private HttpServletRequest request;
    //是否编码的标记
    private boolean hasEncode;
    //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
    public MyRequest(HttpServletRequest request) {
        super(request);// super必须写
        this.request = request;
    }

    // 对需要增强方法 进行覆盖
    @Override
    public Map getParameterMap() {
        // 先获得请求方式
        String method = request.getMethod();
        if (method.equalsIgnoreCase("post")) {
            // post请求
            try {
                // 处理post乱码
                request.setCharacterEncoding("utf-8");
                return request.getParameterMap();
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        } else if (method.equalsIgnoreCase("get")) {
            // get请求
            Map<String, String[]> parameterMap = request.getParameterMap();
            if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                for (String parameterName : parameterMap.keySet()) {
                    String[] values = parameterMap.get(parameterName);
                    if (values != null) {
                        for (int i = 0; i < values.length; i++) {
                            try {
                                // 处理get乱码
                                values[i] = new String(values[i]
                                        .getBytes("ISO-8859-1"), "utf-8");
                            } catch (UnsupportedEncodingException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                hasEncode = true;
            }
            return parameterMap;
        }
        return super.getParameterMap();
    }

    //取一个值
    @Override
    public String getParameter(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        if (values == null) {
            return null;
        }
        return values[0]; // 取回参数的第一个值
    }

    //取所有值
    @Override
    public String[] getParameterValues(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        return values;
    }
}
```

这个是网上找的一些大神写的，一般情况下，SpringMVC默认的乱码处理就已经能够很好的解决了，如果实在遇到都解决不了的情况，可以用这个自定义的方法！

**然后在web.xml中配置这个过滤器`GenericEncodingFilter`即可！**

乱码问题，需要平时多注意，在尽可能能设置编码的地方，都设置为统一编码 UTF-8！





## 6. JSON交互处理

前后端分离时代：

- 后端：部署后端，提供接口，提供数据
- 前端：独立部署，负责渲染后端的数据

前后端通过JSON来进行数据的交互



### 6.1 什么是JSON

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的**数据交换格式**，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。



在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组



**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号` "" `包裹，使用冒号` : `分隔，然后紧接着值：

```json
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```



很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

```javascript
var obj = {a: 'Hello', b: 'World'};		 //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}';	 //这是一个 JSON 字符串，本质是一个字符串
```



**JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 `JSON.parse()` 方法：

```javascript
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现从JavaScript 对象转换为JSON字符串，使用 `JSON.stringify()` 方法：

```javascript
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```



**代码测试**

1、新建一个module ，springmvc-05-json ， 添加web的支持

2、在web目录下新建一个 `json-1.html` ， 编写测试内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JSON_秦疆</title>
</head>
<body>

<script type="text/javascript">
    //编写一个js的对象
    var user = {
        name:"秦疆",
        age:3,
        sex:"男"
    };
    
    //将js对象转换成json字符串
    var str = JSON.stringify(user);
    console.log(str);
    
    //将json字符串转换为js对象
    var user2 = JSON.parse(str);
    console.log(user2.age,user2.name,user2.sex);

</script>

</body>
</html>
```

3、在IDEA中使用浏览器打开，查看控制台输出！

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250027539.png)



### 6.2 Controller返回JSON数据

**Jackson**应该是目前比较好的**json解析工具**

当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

我们这里使用Jackson，使用它需要导入它的jar包：

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
</dependency>
```

配置SpringMVC需要的配置

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--1.注册servlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动顺序，数字越小，启动越早 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>


    <!--乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.run.controller"/>
    <!-- 让Spring MVC不处理静态资源(.css .js .html .png .mp3...)-->
    <mvc:default-servlet-handler />


    <!--支持mvc注解驱动-->
    <mvc:annotation-driven />

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

我们随便编写一个User的实体类，然后我们去编写我们的测试Controller；

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private int age;
    private String sex;
}
```

这里我们需要两个新东西，一个是`@ResponseBody`，一个是ObjectMapper对象，我们看下具体的用法

编写一个Controller；

```java
@Controller
public class UserController {
    @RequestMapping(value = "/json1",produces = "application/json;charset=utf-8")
    @ResponseBody       //它就不会走视图解析器，直接返回一个json字符串
    public String json1() throws JsonProcessingException {
        //创建一个jackson的对象映射器，用来解析数据
        ObjectMapper mapper = new ObjectMapper();

        User user = new User("小明", 18, "男");

        //将对象解析成json格式
        String str = mapper.writeValueAsString(user);

        //由于@REsponseBody注解，这里会将str转成json格式返回，十分方便
        return str;
    }
}
```



乱码问题：需要设置一下他的编码格式为utf-8，以及它返回的类型；

通过@RequestMaping的produces属性来实现，修改下代码

```java
//produces:指定响应体返回类型和编码
@RequestMapping(value = "/json1",produces = "application/json;charset=utf-8")
```



启动Tomcat，进行测试：

![image-20220324204529665](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002217.png)



**【注意：使用json记得处理乱码问题】**





> 代码优化

**乱码统一解决**

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样就不用每次都去处理了！

我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

```xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```



**返回json字符串统一解决**

在**类**上直接使用 **`@RestController`** ，这样子，里面所有的方法都只会**返回 json 字符串**了，不用再每一个都添加`@ResponseBody` ！我们在前后端分离开发中，一般都使用 `@RestController` ，十分便捷！

```java
@RestController
public class UserController {
    @RequestMapping("/json1")
    public String json1() throws JsonProcessingException {
        //创建一个jackson的对象映射器，用来解析数据
        ObjectMapper mapper = new ObjectMapper();

        User user = new User("小明", 18, "男");

        //将对象解析成json格式
        String str = mapper.writeValueAsString(user);

        //由于@REsponseBody注解，这里会将str转成json格式返回，十分方便
        return str;
    }
}
```

启动tomcat测试，结果都正常输出！



#### 6.2.1 集合输出

增加一个新的方法

```java
    //输出集合
    @RequestMapping("/json2")
    public String json2() throws JsonProcessingException {
        //创建一个jackson的对象映射器，用来解析数据
        ObjectMapper mapper = new ObjectMapper();
        //创建一个对象
        User user1 = new User("秦疆1号", 3, "男");
        User user2 = new User("秦疆2号", 6, "男");
        User user3 = new User("秦疆3号", 9, "男");
        User user4 = new User("秦疆4号", 12, "男");
        List<User> list = new ArrayList<User>();
        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);

        //将我们的对象解析成为json格式
        String str = mapper.writeValueAsString(list);
        return str;
    }
```

运行结果 : 十分完美，没有任何问题！

![image-20220324205428107](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002218.png)



#### 6.2.2 输出时间对象

增加一个新的方法

```java
    //输出时间对象(时间戳方式)
    @RequestMapping("/json3")
    public String json3() throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper();

        //创建时间一个对象，java.util.Date
        Date date = new Date();
        //将我们的对象解析成为json格式
        String str = mapper.writeValueAsString(date);
        return str;
    }
```

运行结果 :

![image-20220324205728480](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002219.png)

- 默认日期格式会变成一个数字，是1970年1月1日到当前日期的毫秒数！
- Jackson 默认是会把时间转成timestamps时间戳的形式



**解决方案：取消timestamps形式 ， 自定义时间格式**

```java
    //输出时间对象(自定义日期格式)
    @RequestMapping("/json4")
    public String json4() throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper();

        //不适用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);

        //自定义日期格式对象
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

        //指定日期格式
        mapper.setDateFormat(sdf);

        Date date = new Date();
        String str = mapper.writeValueAsString(date);
        return str;
    }
```

运行结果 : 成功的输出了时间！

![image-20220324210208546](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002220.png)





#### 6.2.3 抽取为工具类

**如果要经常使用的话，这样是比较麻烦的，我们可以将这些代码封装到一个工具类中**

新建utils工具包，创建JsonUtil的json工具类：

```java
public class JsonUtil {
    
    //直接套用下面的方法，使得getJson方法可以有多种参数的情况，传几个参数就走相应的方法
    public static String getJson(Object object) {
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object,String dateFormat) {
        ObjectMapper mapper = new ObjectMapper();
        //不使用时间差的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        //自定义日期格式对象
        SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
        //指定日期格式
        mapper.setDateFormat(sdf);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

我们使用工具类，代码就更加简洁了！

```java
    //自定义工具类
    @RequestMapping("json5")
     public String json5() {
        Date date = new Date();

        //调用两个参数的方法，自己设定日期样式
        String str1 = JsonUtil.getJson(date, "yy-MM-dd");

        //调用1个参数的方法，使用工具类种的默认日期样式
        String str2 = JsonUtil.getJson(date);

        return str1;
	//return str2;
    }
```

大功告成！完美！

![image-20220324213528469](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002221.png)





### 6.3 FastJson

`fastjson.jar`是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转换，实现JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结果都是一样的。

fastjson 的 pom依赖！

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.60</version>
</dependency>
```



fastjson 三个主要的类：

**JSONObject  代表 json 对象** 

- JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。
- JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的。

**JSONArray代表 json 对象数组**

- 内部是有List接口中的方法来完成操作的。

**JSON代表JSONObject和JSONArray的转化**

- JSON类源码分析与使用
- 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化。



**代码测试：**

```java
    //使用alibaba的FastJson
    @RequestMapping("/fastjson")
    public String fastjson() {
        User user1 = new User("秦疆1号", 3, "男");
        User user2 = new User("秦疆2号", 6, "男");
        User user3 = new User("秦疆3号", 9, "男");
        User user4 = new User("秦疆4号", 12, "男");
        List<User> list = new ArrayList<User>();
        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);

        //Java对象转为Json字符串
        String str1 = JSON.toJSONString(user1);
        //集合转为Json字符串
        String str2 = JSON.toJSONString(list);

        //Java对象转为Json对象
        JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);


        //Json字符串转为Java对象
        User jsonUser1 = JSON.parseObject(str1, User.class);

        //Json对象转为Java对象
        User jsonUser2 = JSON.toJavaObject(jsonObject1, User.class);

        return ("Java对象转为Json字符串：" + str1 +
                "集合转为Json字符串：" + str2 +
                "Java对象转为Json对象：" + jsonObject1 +
                "Json字符串转为Java对象：" + jsonUser1 +
                "Json对象转为Java对象：" + jsonUser2);
    }
```



将页面输出的内容排好格式：

```
Java对象转为Json字符串：{"age":3,"name":"秦疆1号","sex":"男"}
集合转为Json字符串：[{"age":3,"name":"秦疆1号","sex":"男"},{"age":6,"name":"秦疆2号","sex":"男"},{"age":9,"name":"秦疆3号","sex":"男"},{"age":12,"name":"秦疆4号","sex":"男"}]
Java对象转为Json对象：{"sex":"男","name":"秦疆2号","age":6}
Json字符串转为Java对象：User(name=秦疆1号, age=3, sex=男)
Json对象转为Java对象：User(name=秦疆2号, age=6, sex=男)
```



这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。





## 7. 整合SSM

### 1. 对SSM框架的理解

**Spring**

Spring就像是整个项目中装配bean的大工厂，在配置文件中可以指定使用特定的参数去调用实体类的构造方法来实例化对象。也可以称之为项目中的粘合剂。

Spring的核心思想是IoC（控制反转），即不再需要程序员去显式地`new`一个对象，而是让Spring框架帮你来完成这一切。

**SpringMVC**

SpringMVC在项目中拦截用户请求，它的核心Servlet即DispatcherServlet承担中介或是前台这样的职责，将用户请求通过HandlerMapping去匹配Controller，Controller就是具体对应请求所执行的操作。SpringMVC相当于SSH框架中struts。

**mybatis**

mybatis是对jdbc的封装，它让数据库底层操作变的透明。mybatis的操作都是围绕一个sqlSessionFactory实例展开的。mybatis通过配置文件关联到各实体类的Mapper文件，Mapper文件中配置了每个类对数据库所需进行的sql语句映射。在每次与数据库交互时，通过sqlSessionFactory拿到一个sqlSession，再执行sql命令。

页面发送请求给控制器，控制器调用业务层处理逻辑，逻辑层向持久层发送请求，持久层与数据库交互，后将结果返回给业务层，业务层将处理逻辑发送给控制器，控制器再调用视图展现数据。 

![image-20220225180725425](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250028795.png)



> SSM框架中个层的理解

SSM框架中的Dao层，Mapper层，controller层，service层，model层，entity层都用什么作用。

SSM：sping+springMVC+mybatis集成的框架。

MVC：model view controller。

model层：entity层或者pojo层。存放我们的实体类，与数据库中的属性值基本保持一致。

service层：存放业务逻辑处理，也是一些关于数据库处理的操作，但不是直接和数据库打交道，他有接口还有接口的实现方法，在接口的实现方法中需要导入mapper层，

mapper层：直接跟数据库打交道的，他也是个接口，只有方法名字，具体实现在mapper.xml文件里，service是供我们使用的方法。

mapper层=dao层，现在用mybatis逆向工程生成的mapper层，其实就是dao层。对数据库进行数据持久化操作，他的方法语句是直接针对数据库操作的，而service层是针对我们controller，也就是针对我们使用者。service的impl是把mapper和service进行整合的文件。

（数据持久化操作就是指，把数据放到持久化的介质中，同时提供增删改查操作，比如数据通过hibernate插入到数据库中。）

controller层：控制器，导入service层，因为service中的方法是我们使用到的，controller通过接收前端传过来的参数进行业务操作，在返回一个指定的路径或者数据表。

在实际开发中的Service层可能被处理为实体Service层，而不是接口，业务逻辑直接写在Service（Class，不是Interface）层中，Controller直接调用Service，Service调用Mapper。

当然了，Service之间也是可以互相调用！

![image-20220225151043812](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250028861.png)



### 2. 环境和基本结构的构建

> 环境要求

环境：

- IDEA
- JDK 1.8
- MySQL 5.7.19
- Tomcat 9
- Maven 3.6

 要求：

- 需要熟练掌握MySQL数据库，Spring，JavaWeb及MyBatis知识，简单的前端知识；



> 数据库环境

创建一个存放书籍数据的数据库表

```sql
CREATE DATABASE `ssmbuild`;

USE `ssmbuild`;

DROP TABLE IF EXISTS `books`;

CREATE TABLE `books` (
`bookID` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT(11) NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID` (`bookID`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT  INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```



> 基本环境搭建

1、新建一Maven项目！SSM-2 ， 添加web的支持

2、导入相关的pom依赖！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>SSM-2</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
        </dependency>
        <!--Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <!--数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!-- 数据库连接池 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

        <!--Servlet - JSP -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

        <!--Mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>

        <!--Spring-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
    </dependencies>

    <!-- Maven资源过滤设置 -->
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
</project>
```



3、建立基本结构和配置框架！

- com.ssm.pojo

- com.ssm.dao

- com.ssm.service

- com.ssm.controller

- mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
         PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
         "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  
  </configuration>
  ```

- applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  </beans>
  ```



### 3. Mybatis层

1、数据库配置文件 **database.properties**

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=123456
```

2、IDEA关联数据库

3、编写MyBatis的核心配置文件`mybatis-config.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
        <package name="com.ssm.pojo"/>
    </typeAliases>
    <mappers>
        <mapper resource="mapper/BookMapper.xml"/>
    </mappers>
</configuration>
```

4、编写数据库对应的实体类 com.ssm.pojo.Books

使用lombok插件！

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Books {
    private int bookID;
    private String bookName;
    private int bookCounts;
    private String detail;
}
```

5、编写Dao层的 Mapper接口！

```java
public interface BookMapper {
    //增加一个Book
    int addBook(Books book);
    
    //根据id删除一个Book
    int deleteBookById(int id);
    
    //更新Book
    int updateBook(Books books);
    
    //根据id查询,返回一个Book
    Books queryBookById(int id);

    //查询全部Book,返回list集合
    List<Books> queryAllBooks();
}
```

6、在`resources`目录下新建mapper文件夹，编写接口对应的 `BookMapper.xml` 文件。需要导入MyBatis的包；

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ssm.dao.BookMapper">

    <insert id="addBook" parameterType="int">
        insert into ssm_demo.books(bookName, bookCounts, detail)
            values(#{bookName}, #{bookCounts}, #{detail})
    </insert>

    <delete id="deleteBookById" parameterType="int">
        delete from ssm_demo.books where bookID = #{bookId}
    </delete>

    <update id="updateBook" parameterType="Books">
        update ssm_demo.books set bookName = #{bookName},
            bookCounts = #{bookCounts}, detail = #{detail} where bookID = #{bookID}
    </update>

    <select id="queryBookById" resultType="Books">
        select * from ssm_demo.books where bookID = #{bookId}
    </select>

    <select id="queryAllBooks" resultType="Books">
        select * from ssm_demo.books
    </select>

</mapper>
```

7、编写Service层的接口和实现类

创建接口：

```java
//BookService: 底下需要去实现,调用dao层
public interface BookService {
    int addBook(Books book);
    int deleteBookById(int id);
    int updateBook(Books books);
    Books queryBookById(int id);
    List<Books> queryAllBook();
}
```

新建impl包，创建对应的实现类：

```java
package com.ssm.service;

import com.ssm.dao.BookMapper;
import com.ssm.pojo.Books;

import java.util.List;

/**
 * @author AruNi_Lu
 * @data 2022/2/25
 */
public class BookServiceImpl implements BookService{

    //service层调用dao层 操作，设置一个set接口，方便Spring管理
    private BookMapper bookMapper;
    public  void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    @Override
    public int addBook(Books book) {
        return bookMapper.addBook(book);
    }

    @Override
    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    @Override
    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    @Override
    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    @Override
    public List<Books> queryAllBook() {
        return bookMapper.queryAllBooks();
    }
}
```

**到此，底层需求操作编写完毕！**



### 4. Spring层

1、配置**Spring整合MyBatis**，数据源使用c3p0连接池；

2、编写Spring整合Mybatis的相关的配置文件；spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

    <!--1.关联数据库配置文件-->
    <context:property-placeholder location="classpath:database.properties"/>

    <!--2.连接池
        dbcp: 半自动化操作，不能自动连接
        c3p0: 自动化操作（自动化的加载配置文件，并且可以自动设置到对象中）
        druid...
    -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!-- c3p0连接池的私有属性 -->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!-- 关闭连接后不自动commit -->
        <property name="autoCommitOnClose" value="false"/>
        <!-- 获取连接超时时间 -->
        <property name="checkoutTimeout" value="10000"/>
        <!-- 当获取连接失败重试次数 -->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!-- 3.配置SqlSessionFactory对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 配置MyBatis全局配置文件:mybatis-config.xml -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!-- 4.配置扫描Dao接口包，动态实现Dao接口注入到spring容器中 -->
    <!--解释 ：https://www.cnblogs.com/jpfss/p/7799806.html-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!-- 给出需要扫描Dao接口包 -->
        <property name="basePackage" value="com.ssm.dao"/>
    </bean>

</beans>
```

3、**Spring整合service层**

编写Spring整合service的相关的配置文件；spring-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd">

    <!--1.扫描service相关的bean -->
    <context:component-scan base-package="com.ssm.service" />

    <!--2.将所有业务类(BookServiceImpl)注入到Spring IoC容器中，此处通过配置（注解也行）-->
    <bean id="bookServiceImpl" class="com.ssm.service.impl.BookServiceImpl">
        <!--Service调用dao的mapper，serviceImplz的set方法对应property中的name，ref为BookMapper接口-->
        <property name="bookMapper" ref="bookMapper"/>
    </bean>

    <!--3.配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
    </bean>
</beans>
```

Spring层搞定！Spring就是一个大杂烩，一个容器！



### 5. SpringMVC层

1、**web.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--DispatcherServlet-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <!--一定要注意:这里加载的是总的配置文件！-->
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--encodingFilter(乱码过滤)-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--Session过期时间-->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>

</web-app>
```

2、**在resource中创建spring-mvc.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd
   http://www.springframework.org/schema/mvc
   https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 配置SpringMVC -->
    <!-- 1.开启SpringMVC注解驱动 -->
    <mvc:annotation-driven />
    <!-- 2.静态资源默认servlet配置-->
    <mvc:default-servlet-handler/>

    <!-- 3.配置jsp 显示ViewResolver视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 4.扫描包：controller    扫描web相关的bean -->
    <context:component-scan base-package="com.ssm.controller" />
</beans>
```

3、**Spring配置整合文件，applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--确保SSM整合在一起了(或者在Module模块的Spring中IDEA帮助整合-->
    <import resource="spring-dao.xml"/>
    <import resource="spring-service.xml"/>
    <import resource="spring-mvc.xml"/>

</beans>
```



### 6. 测试

**此时，先编写一个测试用例，来看看整个项目是否能正常运行**

1、编写controller层的BookController类，方法为查询所有书籍

```java
@Controller
@RequestMapping("/book")
public class BookController {
    //controller 调 service 层
    @Autowired
    @Qualifier("bookServiceImpl")
    private BookService bookService;

    @RequestMapping("/allBook")
    public String list(Model model) {
        List<Books> list = bookService.queryAllBook();
        model.addAttribute("list", list);
        return "allBook";
    }
}
```

2、编写首页`index.jsp`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>首页</title>
  </head>
  <body>
  <a href="${pageContext.request.contextPath}/book/allBook">点击浏览所有书籍</a>
  </body>
</html>
```

3、再WEB-INF下新建jsp文件夹，编写所有书籍页面`allBook.jsp`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>所有书籍</title>
</head>
<body>
<h3>
    所有书籍！！！
</h3>
</body>
</html>
```

此时配置好Tomcat，运行项目：

![image-20220225175337634](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002224.png)

点击后，跳转成功。

![image-20220225175353427](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002225.png)

> 注意事项/遇到的BUG

1、在`database.properties`文件中，`jdbc.url`中的`useSSL=false`，改成true后会遇到各种错误。

2、JDK，Tomcat，MySQL等版本一定要相匹配，不然可能会有一些其他的错误。







>  **配置文件，暂时结束！正式编写Controller 和 View层**

1、BookController 类编写 ， 方法一：查询全部书籍

```java
@Controller
@RequestMapping("/book")
public class BookController {
    //controller 调 service 层
    @Autowired
    @Qualifier("bookServiceImpl")
    private BookService bookService;

    //查询所有书籍
    @RequestMapping("/allBook")
    public String allBook(Model model) {
        List<Books> list = bookService.queryAllBook();
        //将查询到的所有书籍(list)传到前端（参数1可以自定义，前端接收时对应即可）
        model.addAttribute("list", list);
        return "allBook";
    }
}
```

2、编写首页 **index.jsp**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE HTML>
<html>
<head>
  <title>首页</title>
  <style type="text/css">
    a {
      text-decoration: none;
      color: black;
      font-size: 18px;
    }
    h3 {
      width: 180px;
      height: 38px;
      margin: 100px auto;
      text-align: center;
      line-height: 38px;
      background: deepskyblue;
      border-radius: 4px;
    }
  </style>
</head>
<body>

<h3>
  <a href="${pageContext.request.contextPath}/book/allBook">点击进入列表页</a>
</h3>
</body>
</html>
```

3、书籍列表页面 **allbook.jsp**

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>书籍列表</title>
    <!-- 引入 Bootstrap：CSS美化界面-->
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

<div class="container">

    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>书籍列表 —— 显示所有书籍</small>
                </h1>
            </div>
        </div>
    </div>

<%--新增书籍按钮--%>
    <div class="row">
        <div class="col-md-4 column">
            <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
        </div>
    </div>

    <div class="row clearfix">
        <div class="col-md-12 column">
            <table class="table table-hover table-striped">
                <thead>
                <tr>
                    <th>书籍编号</th>
                    <th>书籍名字</th>
                    <th>书籍数量</th>
                    <th>书籍详情</th>
                    <th>操作</th>
                </tr>
                </thead>

<%--            书籍从数据库中查询出来，从这个list中遍历出来：foreach--%>
                <tbody>
<%--                用items接收从后端传过来的所有书籍list--%>
                <c:forEach var="book" items="${list}">
                    <tr>
                        <td>${book.getBookID()}</td>
                        <td>${book.getBookName()}</td>
                        <td>${book.getBookCounts()}</td>
                        <td>${book.getDetail()}</td>
                        <td>
<%--                            点击更改操作时，需要传该书籍的ID参数过去，才知道修改的是哪本书 （也可以使用deleteBook的@PathVariable方法）--%>
                            <a href="${pageContext.request.contextPath}/book/toUpdateBook?id=${book.getBookID()}">更改</a>
                            丨
                            <a href="${pageContext.request.contextPath}/book/deleteBook/${book.getBookID()}">删除</a>
                        </td>
                    </tr>
                </c:forEach>
                </tbody>
            </table>
        </div>
    </div>
</div>
```

4、BookController 类编写 ， 方法二：添加书籍

```java
//跳转到添加书籍页面
    @RequestMapping("/toAddBook")
    public String toAddBook() {
        //返回到添加书籍页面
        return "addBook";
    }
    //添加书籍的请求
    @RequestMapping("/addBook")
    public String addBook(Books books) {
        bookService.addBook(books);
        //重定向到@RequestMapping("/allBook") 请求，走一遍查询所有书籍的方法
        return "redirect:/book/allBook";
    }
```

5、添加书籍页面：**addBook.jsp**

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
    <title>新增书籍</title>
    <!-- 引入 Bootstrap -->
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">

    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>新增书籍</small>
                </h1>
            </div>
        </div>
    </div>

    <form action="${pageContext.request.contextPath}/book/addBook" method="post">
        <div class="form-group">
            <label for="bkname">书籍名称</label>
            <input type="text" name="bookName" class="form-control" id="bkname" required>
        </div>
        <div class="form-group">
            <label for="bknum">书籍数量</label>
            <input type="text" name="bookCounts" class="form-control" id="bknum" required>
        </div>
        <div class="form-group">
            <label for="bkdesc">书籍描述</label>
            <input type="text" name="detail" class="form-control" id="bkdesc" required>
        </div>
        <div class="form-group">
            <input type="submit" class="form-control" value="添加">
        </div>

    </form>

</div>
```

6、BookController 类编写 ， 方法三：修改书籍

```java
//跳转到修改书籍页面
    @RequestMapping("/toUpdateBook")
    public String toUpdateBook(Model model, int id) {
        Books books = bookService.queryBookById(id);
        //将查询到的书籍(books)传到前端
        model.addAttribute("queryBooks", books);
        //返回到修改书籍页面
        return "updateBook";
    }
    //修改书籍
    @RequestMapping("/updateBook")
    public String updateBook(Books books) {
        bookService.updateBook(books);
        return "redirect:/book/allBook";
    }
```

7、修改书籍页面  **updateBook.jsp**

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
  <title>修改信息</title>
  <!-- 引入 Bootstrap -->
  <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">

  <div class="row clearfix">
    <div class="col-md-12 column">
      <div class="page-header">
        <h1>
          <small>修改信息</small>
        </h1>
      </div>
    </div>
  </div>

  <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
<%--后端也要把ID传过来，但是前端页面看不到，所以要用传递隐藏域--%>
    <input type="hidden" name="bookID" value="${queryBooks.getBookID()}"/>

    <div class="form-group">
        <label for="bkname">书籍名称</label>
<%--        用value接收后端传过来的值--%>
        <input type="text" name="bookName"  value="${queryBooks.bookName}" class="form-control" id="bkname" required>
    </div>
    <div class="form-group">
        <label for="bknum">书籍数量</label>
        <input type="text" name="bookCounts" value="${queryBooks.bookCounts}" class="form-control" id="bknum" required>
    </div>
    <div class="form-group">
        <label for="bkdesc">书籍描述</label>
        <input type="text" name="detail" value="${queryBooks.detail}" class="form-control" id="bkdesc" required>
    </div>
    <div class="form-group">
        <input type="submit" class="form-control" value="修改">
    </div>
  </form>

</div>
```

8、BookController 类编写 ， 方法四：删除书籍

```java
//删除书籍
    @RequestMapping("/deleteBook/{bookId}")
    //用@PathVariable取一个前端传过来的bookId
    public String deleteBook(@PathVariable("bookId") int id) {
        bookService.deleteBookById(id);
        return "redirect:/book/allBook";
    }
```



**配置Tomcat，进行运行！**

对于基本的增删改查基本上已经完成！！！

用了SSM框架后，功能的编写变得十分方便，只需要编写前端的jsp和相应的controller层，就可以完成。



### 7. 搜索功能添加

如果想要添加功能，也是非方便，接下来添加一个搜索功能。

采用自顶向下的方法编写功能：

客户浏览的顺序：前端 =》controller层 =》service层 =》dao层 =》数据库

开发顺序反过来即可

1、dao层mapper接口中添加方法，在xml文件中编写对应的SQL语句

```java
//通过书名搜索书籍（模糊查询）
List<Books> queryBookByName(String name);
```

```sql
<select id="queryBookByName" resultType="Books">
        select * from ssm_demo.books where bookName like '%${bookName}%'
</select>
```



2、在service层添加相应的方法

3、在首页中添加一个搜索框

```jsp
<div class="col-md-4 column">
            <%--搜索书籍框--%>
            <form action="${pageContext.request.contextPath}/book/queryBook" method="post" style="float: right" class="form-inline">
                <%--若未查到，后端会传一个error过来--%>
                <span style="color:red; font-weight: bold">${error}</span>
                <%--将查询书籍的名字(queryBookName)传给后端--%>
                <input type="text" name="queryBookName" class="form-control" placeholder="请输入书籍名称">
                <input type="submit" value="查询" class="btn btn-success">
            </form>
</div>
```



3、controller层

```java
//**搜索书籍
    @RequestMapping("/queryBook")
    //queryBookName为前端传过来的书籍名称
    public String queryBook(String queryBookName, Model model) {
        List<Books> list = bookService.queryBookByName(queryBookName);

        //若数据库中无相关书籍，给用户一个提示信息
        if (list.isEmpty()) {
            list = bookService.queryAllBook();
            //将error提示传给前端
            model.addAttribute("error", "未查到");
        }
        //将查询到的list集合返回给前端
        model.addAttribute("list", list);
        return "allBook";
    }
```



### 8. 最终项目结构图

![image-20220309205343992](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203250002226.png)







## 8. Ajax

### 8.1 简介

- **AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）**
- AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。
- **Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。**
- 在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。Google Suggest能够自动帮你完成搜索单词。
- Google Suggest 使用 AJAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。
- 就和国内百度的搜索框一样!

- 传统的网页(即不用ajax技术的网页)，想要更新内容或者提交一个表单，都需要重新加载整个网页。
- 使用ajax技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新。
- 使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面。



### 8.2 伪造Ajax

我们可以使用前端的一个标签来伪造一个ajax的样子。iframe标签

1、新建一个module ：springmvc-06-ajax，导入web支持！

2、编写一个`test.html`使用 iframe 测试，感受下效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>iframe体验页面无刷新</title>

    <script>
        function go() {
            let url = document.getElementById("url").value;
            document.getElementById("iframe1").src=url;
        }
    </script>
</head>
<body>

<div>
    <p>请输入地址：</p>
    <p>
        <input type="text" id="url">
        <input type="button" value="提交" onclick="go()">
    </p>
</div>

<div>
    <iframe id="iframe1" style="width: 100%; height: 500px"></iframe>
</div>

</body>
</html>
```

3、使用IDEA开浏览器测试一下！

![image-20220325104239766](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251042967.png)



**利用AJAX可以做：**

- 注册时，输入用户名自动检测用户是否已经存在。
- 登陆时，提示用户名密码错误
- 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除。
- ....等等



### 8.3 jQuery.ajax

纯JS原生实现Ajax我们不去讲解这里，直接使用jquery提供的，方便学习和使用，避免重复造轮子，有兴趣的同学可以去了解下JS原生XMLHttpRequest ！

Ajax的核心是**XMLHttpRequest**对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

jQuery 提供多个与 AJAX 有关的方法。

通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。

jQuery 不是生产者，而是大自然搬运工。

jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

```javascript
jQuery.ajax(...)
       部分参数：
              url：请求地址
             type：请求方式，GET、POST（1.9.0之后用method）
          headers：请求头
             data：要发送的数据
      contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
            async：是否异步
          timeout：设置请求超时时间（毫秒）
       beforeSend：发送请求前执行的函数(全局)
         complete：完成之后执行的回调函数(全局)
          success：成功之后执行的回调函数(全局)
            error：失败之后执行的回调函数(全局)
          accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
         dataType：将服务器端返回的数据转换成指定类型
            "xml": 将服务器端返回的内容转换成xml格式
           "text": 将服务器端返回的内容转换成普通文本格式
           "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
         "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
           "json": 将服务器端返回的内容转换成相应的JavaScript对象
          "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```





**我们来个简单的测试，使用最原始的HttpServletResponse处理 , 最简单 , 最通用**

1、配置web.xml 和 springmvc的配置文件，复制上面案例的即可 【记得静态资源过滤和注解驱动配置上】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.run.controller"/>
    <!-- 让Spring MVC不处理静态资源(.css .js .html .png .mp3...)-->
    <mvc:default-servlet-handler />

    <!--支持mvc注解驱动-->
    <mvc:annotation-driven />

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

2、编写一个AjaxController

```java
@Controller
public class AjaxController {
    @RequestMapping("/a1")
    public void a1(String name, HttpServletResponse response) throws IOException {
        response.setCharacterEncoding("utf-8");     //乱码处理
        if (name.equals("小明")) {
            response.getWriter().print("用户名正确");
        } else {
            response.getWriter().print("用户名错误");
        }
    }
}
```

3、导入jquery ， 可以使用在线的CDN ， 也可以下载导入

```html
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>

<script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
```

4、编写index.jsp测试

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>

    <script>
      function a() {
        $.post({
          url:"${pageContext.request.contextPath}/a1",     //向后端发起请求/a1
          data:{'name':$("#username").val()},          //获取文本框中输入的值，传给后端
          success:function (data) {         //如果执行成功，回调一个function方法，data为后端传回来的信息
            alert(data);
          }
        });
      }
    </script>
  </head>
  <body>

  <%--onblur：失去焦点触发事件--%>
  用户名：<input type="text" id="username" onblur="a()">


  </body>
</html>
```

5、启动tomcat测试！打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个ajax的请求！是后台返回给我们的结果！测试成功！

<img src="D:\Snipaste截图\Snipaste_2022-03-25_11-33-13.png"  />

![image-20220325113354164](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220325113354164.png)

 



#### 8.3.1 后端数据回显

**Springmvc实现**

实体类user

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private int age;
    private String sex;
}
```

我们来获取一个集合对象，展示到前端页面

```java
    @RequestMapping("/a2")
    @ResponseBody
    public List<User> a2() {
        List<User> userList = new ArrayList<>();
        userList.add(new User("秦疆1号",3,"男"));
        userList.add(new User("秦疆2号",6,"男"));
        userList.add(new User("秦疆3号",9,"男"));
        return userList;        //由于@ResponseBody注解(在类上加@RestController也行)，会将list转为json格式返回前端
    }
```

前端页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>

    <script>
        $(function () {
           $("#btn").click(function () {
               //$.post中参数的简写(url, param可以省略, success)
               $.post("${pageContext.request.contextPath}/a2", function (data) {
                   console.log(data);
                   var html="";     //用来拼接数据
                   for (let i = 0; i < data.length; i++) {
                       html += "<tr>" +
                           "<td>" + data[i].name + "</td>" +
                           "<td>" + data[i].age + "</td>" +
                           "<td>" + data[i].sex + "</td>" +
                           "</tr>"
                   }

                   $("#content").html(html);        //将拼接好的数据显示在<tbody>里
               })
           })
        })
    </script>
</head>
<body>

<input type="button" value="加载数据" id="btn">
<table>
    <tr>
        <td>姓名</td>
        <td>年龄</td>
        <td>性别</td>
    </tr>

    <tbody id="content">
    </tbody>
</table>


</body>
</html>
```

测试：

![image-20220325124131409](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220325124131409.png)

**成功实现了数据回显！可以体会一下Ajax的好处！**





#### 8.3.2 注册提示效果

我们再测试一个小Demo，思考一下我们平时注册时候，输入框后面的实时提示怎么做到的；如何优化

写一个Controller

```java
    @RequestMapping("/a3")
    @ResponseBody
    public String a3(String name, String pwd) {
        String msg = "";
        if (name != null) {
            if ("admin".equals(name)) {
                msg = "用户名正确";
            } else {
                msg = "用户名错误";
            }
        }

        if (pwd != null) {
            if ("123456".equals(pwd)) {
                msg = "密码正确";
            } else {
                msg = "密码错误";
            }
        }
        return msg;     //返回json对象
    }
```

前端页面 **login.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>

    <script>
        function a1() {
            $.post({
                url:"${pageContext.request.contextPath}/a3",
                data:{"name":$("#name").val()},
                success:function (data) {
                    if (data.toString() === "用户名正确") {
                        $("#userInfo").css("color", "green");
                    } else {
                        $("#userInfo").css("color", "red");
                    }
                    $("#userInfo").html(data);      //将信息传到<span>标签显示
                }
            })
        }

        function a2() {
            $.post({
                url:"${pageContext.request.contextPath}/a3",
                data:{"pwd":$("#pwd").val()},
                success:function (data) {
                    if (data.toString() === "密码正确") {
                        $("#pwdInfo").css("color", "green");
                    } else {
                        $("#pwdInfo").css("color", "red");
                    }
                    $("#pwdInfo").html(data);      //将信息传到<span>标签显示               }
                }
            })
        }
    </script>
</head>
<body>

<p>
    用户名:<input type="text" id="name" onblur="a1()"/>
    <span id="userInfo"></span>
</p>
<p>
    密码:<input type="text" id="pwd" onblur="a2()"/>
    <span id="pwdInfo"></span>
</p>

</body>
</html>
```

处理json乱码问题，在**applicationContext.xml**中添加json乱码的配置

```xml
    <!--json乱码问题-->
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
```



测试一下效果，动态请求响应，局部刷新，就是如此！

![image-20220325143742047](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220325143742047.png)





> 获取baidu接口Demo

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>JSONP百度搜索</title>
    <style>
        #q{
            width: 500px;
            height: 30px;
            border:1px solid #ddd;
            line-height: 30px;
            display: block;
            margin: 0 auto;
            padding: 0 10px;
            font-size: 14px;
        }
        #ul{
            width: 520px;
            list-style: none;
            margin: 0 auto;
            padding: 0;
            border:1px solid #ddd;
            margin-top: -1px;
            display: none;
        }
        #ul li{
            line-height: 30px;
            padding: 0 10px;
        }
        #ul li:hover{
            background-color: #f60;
            color: #fff;
        }
    </style>
    <script>

        // 2.步骤二
        // 定义demo函数 (分析接口、数据)
        function demo(data){
            var Ul = document.getElementById('ul');
            var html = '';
            // 如果搜索数据存在 把内容添加进去
            if (data.s.length) {
                // 隐藏掉的ul显示出来
                Ul.style.display = 'block';
                // 搜索到的数据循环追加到li里
                for(var i = 0;i<data.s.length;i++){
                    html += '<li>'+data.s[i]+'</li>';
                }
                // 循环的li写入ul
                Ul.innerHTML = html;
            }
        }

        // 1.步骤一
        window.onload = function(){
            // 获取输入框和ul
            var Q = document.getElementById('q');
            var Ul = document.getElementById('ul');

            // 事件鼠标抬起时候
            Q.onkeyup = function(){
                // 如果输入框不等于空
                if (this.value != '') {
                    // ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆JSONPz重点☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
                    // 创建标签
                    var script = document.createElement('script');
                    //给定要跨域的地址 赋值给src
                    //这里是要请求的跨域的地址 我写的是百度搜索的跨域地址
                    script.src = 'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+this.value+'&cb=demo';
                    // 将组合好的带src的script标签追加到body里
                    document.body.appendChild(script);
                }
            }
        }
    </script>
</head>

<body>
<input type="text" id="q" />
<ul id="ul">

</ul>
</body>
</html>
```



Ajax在我们开发中十分重要，一定要学会使用！







## 9. 拦截器

### 9.1 概述

SpringMVC的处理器拦截器类似于Servlet开发中的过滤器Filter,用于对处理器进行预处理和后处理。开发者可以自己定义一些拦截器来实现特定的功能。

**过滤器与拦截器的区别：**拦截器是AOP思想的具体应用。

**过滤器**

- servlet规范中的一部分，任何java web工程都可以使用
- 在`url-pattern`中配置了`/*`之后，可以对所有要访问的资源进行拦截

**拦截器** 

- 拦截器是SpringMVC框架自己的，只有使用了SpringMVC框架的工程才能使用
- 拦截器只会拦截**访问的控制器方法**， 如果访问的是jsp/html/css/image/js是不会进行拦截的





### 9.2 自定义拦截器

想要自定义拦截器，必须实现 `HandlerInterceptor` 接口。

`HandlerInterceptor`接口中的三个主要方法：

- `preHandle`：在请求处理的方法之前执行；如果返回true执行下一个拦截器，返回false就不执行下一个拦截器
- `postHandle`：在请求处理方法执行后执行
- `afterCompletion`：在dispatcherServlet处理后执行，做清理工作



步骤：

1、新建一个Moudule ， springmvc-07-Interceptor  ， 添加web支持

2、配置`web.xml` 和 `applicationContext.xml` 文件

3、新建一个config包，编写一个拦截器`MyInterceptor`

```java
public class MyInterceptor implements HandlerInterceptor {
    @Override
    //preHandle 在请求处理的方法之前执行
    //如果返回true执行下一个拦截器
    //如果返回false就不执行下一个拦截器
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("=====处理前=====");
        return true;
    }

    @Override
    //postHandle 在请求处理方法执行后执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("=====处理后=====");
    }

    @Override
    //在dispatcherServlet处理后执行，做清理工作
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("=====清理=====");
    }
}
```

4、在springmvc的配置文件中配置拦截器

```xml
    <!--关于拦截器的配置-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--/** 包括路径及其子路径-->
            <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-->
            <!--/admin/** 拦截的是/admin/下的所有-->
            <mvc:mapping path="/**"/>
            <!--bean配置的就是拦截器-->
            <bean class="com.run.config.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

5、编写一个Controller，接收请求

```java
@Controller
public class InterceptorController {
    @RequestMapping("/t1")
    @ResponseBody
    public String t1() {
        System.out.println("控制器执行了");
        return "hello";
    }
}
```

7、启动tomcat，输入对应的地址，控制台输出：

```
=====处理前=====
控制器执行了
=====处理后=====
=====清理=====
```





### 9.3 验证用户是否登录 (认证用户)

**实现思路**

1、有一个登陆页面，需要写一个controller访问页面。

2、登陆页面有一提交表单的动作。需要在controller中处理。判断用户名密码是否正确。如果正确，向session中写入用户信息。*返回登陆成功。*

3、拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面

**步骤：**

1、在WEB-INF下新建jsp目录，编写一个登陆页面`login.jsp`  (WEB-INF下的所以文件只能通过controller或servlet访问)

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录</title>
</head>

<body>

<h1>登录页面</h1>
<hr>
<form action="${pageContext.request.contextPath}/user/login" method="post">
    用户名：<input type="text" name="username"> <br>
    密码：<input type="password" name="pwd"> <br>
    <input type="submit" value="提交">
</form>

</body>
</html>
```

2、编写一个Controller处理请求

```java
@Controller
public class LoginController {

    //跳转到用户主页面
    @RequestMapping("/user/goMain")
    public String goMain() {
        return "main";
    }

    //跳转到登录页面
    @RequestMapping("/user/goLogin")
    public String goLogin() {
        return "login";
    }

    //登录提交
    @RequestMapping("/user/login")
    public String login(String username, String password, HttpSession session) {
        session.setAttribute("userInfo", username);
        return "main";
    }

    //注销
    @RequestMapping("/user/goOut")
    public String goOut(HttpSession session) {
        session.removeAttribute("userInfo");
        return "redirect:/user/goLogin";    //注销后重定向到登录页面
    }
}
```

3、编写一个登陆成功的用户主页面 `main.jsp`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>用户主页</h1>
<%--从session中获取用户信息--%>
<span>欢迎您！${sessionScope.userInfo}</span>
<hr>

<p>
    <a href="${pageContext.request.contextPath}/user/goOut">注销</a>
</p>

</body>
</html>
```

4、在 index 页面上测试跳转！启动Tomcat 测试，未登录也可以进入主页！接下来编写拦截器，完善登录功能。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>首页</title>
  </head>
  <body>
  <h1>首页</h1>
  <hr>
  <a href="${pageContext.request.contextPath}/user/goLogin">登录</a> <br/>
  <a href="${pageContext.request.contextPath}/user/goMain">用户主页面</a>
  </body>
</html>
```

5、编写用户登录拦截器

```java
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //获取session
        HttpSession session = request.getSession();
        if (session.getAttribute("userInfo") != null) {
            return true;     //当session中有该用户的信息，放行
        }
        if (request.getRequestURI().contains("login")) {
            return true;     //当点击登录，也放行
        }

        //其他情况，不放行，禁止进入用户主页；跳转到登录页面
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
        return false;

    }
}
```

6、在Springmvc的配置文件中注册拦截器

```xml
<!--关于拦截器的配置-->
<mvc:interceptors>
    <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean id="loginInterceptor" class="com.run.config.LoginInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

7、再次重启Tomcat测试！

**测试登录拦截功能无误**







## 10. 文件上传和下载

> 预备知识

文件上传是项目开发中最常见的功能之一 ,springMVC 可以很好的支持文件上传，但是SpringMVC上下文中默认没有装配MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用Spring的文件上传功能，则需要在上下文中配置MultipartResolver。



前端表单要求：为了能上传文件，必须将表单的**method**设置为`POST`，并将**enctype**设置为`multipart/form-data`。只有在这样的情况下，浏览器才会把用户选择的文件以**二进制数据**发送给服务器；

**对表单中的 enctype 属性做个详细的说明：**

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式。
- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码。
- text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

```
<form action="" enctype="multipart/form-data" method="post">
    <input type="file" name="file"/>
    <input type="submit">
</form>
```

一旦设置了enctype为multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处理则涉及在服务器端解析原始的HTTP响应。在2003年，Apache Software Foundation发布了开源的Commons FileUpload组件，其很快成为Servlet/JSP程序员上传文件的最佳选择。

- Servlet3.0规范已经提供方法来处理文件上传，但这种上传需要在Servlet中完成。
- 而Spring MVC则提供了更简单的封装。
- Spring MVC为文件上传提供了直接的支持，这种支持是用即插即用的MultipartResolver实现的。
- Spring MVC使用Apache Commons FileUpload技术实现了一个MultipartResolver实现类：
- CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件。



### 10.1 文件上传

1、导入文件上传的jar包`commons-fileupload`， Maven会自动帮我们导入他的依赖包 `commons-io`包；

```xml
<!--文件上传-->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>
<!--servlet-api导入高版本的-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
</dependency>
```

2、在`applicationContext.xml`中配置bean：multipartResolver

【**注意！！！这个bena的id必须为：multipartResolver ， 否则上传文件会报400的错误！**】

```xml
<!--文件上传配置-->
<bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，默认为ISO-8859-1 -->
    <property name="defaultEncoding" value="utf-8"/>
    <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
    <property name="maxUploadSize" value="10485760"/>
    <property name="maxInMemorySize" value="40960"/>
</bean>
```



CommonsMultipartFile 的 常用方法：

- **String getOriginalFilename()：获取上传文件的原名**
- **InputStream getInputStream()：获取文件流**
- **void transferTo(File dest)：将上传文件保存到一个目录文件中**

 我们去实际测试一下

3、编写前端页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="/upload" enctype="multipart/form-data" method="post">
    <input type="file" name="file"/>
    <input type="submit" value="upload">
  </form>
  </body>
</html>
```

4、**Controller**

```java
@Controller
public class FileController {
    //@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
    //批量上传CommonsMultipartFile则为数组即可
    @RequestMapping("/upload")
    public String fileUpload(@RequestParam("file") CommonsMultipartFile file , HttpServletRequest request) throws IOException {

        //获取文件名 : file.getOriginalFilename();
        String uploadFileName = file.getOriginalFilename();

        //如果文件名为空，直接回到首页！
        if ("".equals(uploadFileName)){
            return "redirect:/index.jsp";
        }
        System.out.println("上传文件名 : "+uploadFileName);

        //上传路径保存设置
        String path = request.getServletContext().getRealPath("/upload");
        //如果路径不存在，创建一个
        File realPath = new File(path);
        if (!realPath.exists()){
            realPath.mkdir();
        }
        System.out.println("上传文件保存地址："+realPath);

        InputStream is = file.getInputStream(); //文件输入流
        OutputStream os = new FileOutputStream(new File(realPath,uploadFileName)); //文件输出流

        //读取写出
        int len=0;
        byte[] buffer = new byte[1024];
        while ((len=is.read(buffer))!=-1){
            os.write(buffer,0,len);
            os.flush();
        }
        os.close();
        is.close();
        return "redirect:/index.jsp";
    }
}
```

5、测试上传文件，OK！



**采用file.Transto 来保存上传的文件**

1、编写Controller

```java
/*
 * 采用file.Transto 来保存上传的文件
 */
@RequestMapping("/upload2")
public String  fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {

    //上传路径保存设置
    String path = request.getServletContext().getRealPath("/upload");
    File realPath = new File(path);
    if (!realPath.exists()){
        realPath.mkdir();
    }
    //上传文件地址
    System.out.println("上传文件保存地址："+realPath);

    //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
    file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));

    return "redirect:/index.jsp";
}
```

2、前端表单提交地址修改

3、访问提交测试，OK！



> 文件下载

**文件下载步骤：**

1、设置 response 响应头

2、读取文件 -- InputStream

3、写出文件 -- OutputStream

4、执行操作

5、关闭流 （先开后关）

**代码实现：**

```java
@RequestMapping(value="/download")
public String downloads(HttpServletResponse response ,HttpServletRequest request) throws Exception{
    //要下载的图片地址
    String  path = request.getServletContext().getRealPath("/upload");
    String  fileName = "基础语法.jpg";

    //1、设置response 响应头
    response.reset(); //设置页面不缓存,清空buffer
    response.setCharacterEncoding("UTF-8"); //字符编码
    response.setContentType("multipart/form-data"); //二进制传输数据
    //设置响应头
    response.setHeader("Content-Disposition",
            "attachment;fileName="+URLEncoder.encode(fileName, "UTF-8"));

    File file = new File(path,fileName);
    //2、 读取文件--输入流
    InputStream input=new FileInputStream(file);
    //3、 写出文件--输出流
    OutputStream out = response.getOutputStream();

    byte[] buff =new byte[1024];
    int index=0;
    //4、执行 写出操作
    while((index= input.read(buff))!= -1){
        out.write(buff, 0, index);
        out.flush();
    }
    out.close();
    input.close();
    return null;
}
```

前端

```jsp
<a href="/download">点击下载</a>
```

测试，文件下载OK，大家可以和我们之前学习的JavaWeb原生的方式对比一下，就可以知道这个便捷多了!

























