## 1. 设计

实现一个最简单的 Bean 容器，由于我们需要通过 Bean 的名字来获取该 Bean 对象，所以使用 Map 进行映射最好不过了。

我们把 Bean 对象单独放在一个 BeanDefinition 类中，再定义一个 Bean 工厂 BeanFacotry 来存取 Bean。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202303091506104.png)

## 2. 实现

各类的定义如下类图所示：

![image-20230309111726175](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202303091506111.png)

- BeanDefinition 中定义 Bean，提供一个获取 Bean 的方法；
- BeanFactory 工厂提供一个 Bean 容器 Map，以及提供往容器中注入 Bean，从容器中获取 Bean 的方法。

## 3. 测试

测试方法如下：

```java
@Test
public void test_BeanFactory() {
    // 1. 初始化 BeanFactory
    BeanFactory beanFactory = new BeanFactory();
    // 创建 BeanDefinition
    BeanDefinition beanDefinition = new BeanDefinition(new UserService());
    
    // 2. 注册 bean
    beanFactory.registerBeanDefinition("userService", beanDefinition);

    // 3. 获取 bean
    UserService userService = (UserService) beanFactory.getBean("userService");
    userService.queryUserInfo();

    // 输出：查询用户信息
}
```

## 4. 流程

流程如下：

![image-20230309112028253](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202303091506754.png)

