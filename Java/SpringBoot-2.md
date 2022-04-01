# 11. 员工管理系统

## 11.1 准备工作

1. 导入相关静态资源

   ![image-20220331215243683](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220331215243683.png)

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

   ![image-20220401123042971](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401123042971.png)

3. 启动项目，查看首页：

   ![image-20220331225852489](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220331225852489.png)

## 11.3 页面国际化

有的时候，我们的网站会去涉及中英文甚至多语言的切换，这时候我们就需要学习国际化了！

### 11.3.1 准备工作

先在IDEA中统一设置properties的编码问题！

![image-20220331213516288](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220331213516288.png)

编写国际化配置文件，抽取页面需要显示的国际化页面消息。

### 11.3.2 配置文件编写

1. 我们在resources资源文件下新建一个`i18n`（internationalization缩写）目录，存放国际化配置文件

2. 建立一个`login.properties`文件，还有一个`login_zh_CN.properties`；发现IDEA自动识别了我们要做国际化操作；文件夹变了！

   ![image-20220401102524302](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401102524302.png)

3. 我们可以在这上面去新建一个文件；

   ![image-20220401102553866](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401102553866.png)

   弹出如下页面：我们再添加一个英文的；

   ![image-20220401102620870](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401102620870.png)

   这样就快捷多了！

   ![image-20220401102643052](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401102643052.png)

4. **接下来，我们就来编写配置，我们可以看到idea下面有另外一个视图**

   ![image-20220401105424073](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401105424073.png)

   这个视图我们点击 + 号就可以直接添加属性了；我们新建一个`login.tip`，可以看到边上有三个文件框可以输入

   ![image-20220401105512451](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401105512451.png)

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

![image-20220401111528571](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401111528571.png)

我们可以去启动项目，访问一下，发现已经自动识别为中文的了！

![image-20220401111547450](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401111547450.png)

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

![image-20220401114001820](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401114001820.png)

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

   ![image-20220401124241604](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401124241604.png)



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

   ![image-20220401135154331](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401135154331.png)

5. 登录成功后，可以通过session拿到用户的名字，在主页中显示，在`dashboard.html`中对应位置获取即可：

   ```html
   <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>
   ```

6. 测试：

   ![image-20220401135736113](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401135736113.png)



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

   ![image-20220401145922383](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401145922383.png)

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

   ![image-20220401152205641](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401152205641.png)

   ![image-20220401152224972](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401152224972.png)

   

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

![image-20220401154227488](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401154227488.png)



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

   ![image-20220401164646710](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401164646710.png)

   ![image-20220401164714897](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401164714897.png)



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

   ![image-20220401181044013](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220401181044013.png)





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

   ![1595741635349](https://gitee.com/lzh_gitee/SpringBoot/raw/master/SpringBoot%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/SpringBoot07%EF%BC%9A%E6%95%B4%E5%90%88JDBC.assets/1595741635349.png)

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

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为要使用的连接池实现的完全限定名**。

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate

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

## 13.3 配置Druid数据源监控

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











# 14. 整合MyBatis

官方文档：[http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/](https://gitee.com/link?target=http%3A%2F%2Fmybatis.org%2Fspring-boot-starter%2Fmybatis-spring-boot-autoconfigure%2F)

Maven仓库地址：[https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.3](https://gitee.com/link?target=https%3A%2F%2Fmvnrepository.com%2Fartifact%2Forg.mybatis.spring.boot%2Fmybatis-spring-boot-starter%2F2.1.3)

![image-20200728083023851](https://gitee.com/lzh_gitee/springboot_image/raw/master/img/image-20200728083023851.png)

## 14.1 整合步骤

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





