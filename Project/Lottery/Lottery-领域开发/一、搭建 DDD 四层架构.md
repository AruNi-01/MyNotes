## 1. 什么是 DDD

文章：

- https://zhuanlan.zhihu.com/p/351162895

先来看看传统的 MVC 架构：

![img](https://pic3.zhimg.com/80/v2-e39768d62b5e1ba449c3d0dcc416dfaa_1440w.webp)

- 其中用户展现层主要是 Controller 层 + JSP/Thymeleaf，如果前后端分离则 Controller 层负责请求的接受以及返回 JSON 数据即可；
- 会提供一些 Java Beans（Entity），或者自己自定义一些 VO、BO 等。

再来看看 DDD 架构：

![img](https://pic2.zhimg.com/80/v2-3737c7c5cd31d66e433dfa34b32b73f9_1440w.webp)

相比于 MVC 主要有如下区别：

Controller 层更加纯粹：

- MVC 中，Controller 可能会聚合多个 Service 层的调用；
- DDD 中，Controllers 纯粹作为与前端或外部系统交互的数据转换层，不再包含业务逻辑，也就是单纯的用户接口。

Service 层改为 Application Services，主要对 Service 进行了分级、打薄处理：

- MVC 中，Service 包含各种业务逻辑，所有的逻辑都放到该层进行处理；
- DDD 中，Application Services 作为更高级别的抽象，新增了下面的领域层，抽离原 Service 层的部分逻辑到领域层。

领域层分为聚合和领域服务，其中聚合包括 Entity + VO，**Entity 和 VO 不像 MVC 那样提倡的贫血模型（仅包含 getter/setter 方法的简单对象），而是包含了业务逻辑的丰富对象，即 DDD 所提倡的充血模型对象**。

简单来看上面架构的区别，思考我们以往的开发，传统三层架构更像是面向过程编程，业务全部集中在service层，或许大家都在 service 层写过大量的数据校验代码，整个业务方法冗长，即使做了方法的提取封装，但整体仍然会有凌乱的感觉，但 **DDD 则更符合面向对象的设计**，按照 DDD 的架构分层，各层级之间的分级更加明确，service 层的含义表述则会更加清晰简洁，而聚合和领域服务的粒度更小，更加的内聚。

## 2. DDD 服务架构调用关系

![image-20230405110649170](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051107623.png)

- **应用接口层**（Interfaces）：负责以 Restful 风格 **接受 Web 请求，然后将请求路由给 Application 层执行**，并返回视图模型（View Model），其载体通常是 DTO（Data Transfer Object）；

- **应用层**（Application）：主要负责 **获取输入，组装上下文，做输入校验，调用领域层做业务处理**，如果需要的话，发送消息通知。当然，层次是开放的，若有需要，应用层也可以直接访问基础实施层；

- **领域层**（Domain）：主要是 **封装了核心业务逻辑**，并通过领域服务（Domain Service）和领域对象（Entities）的函数对外部提供业务逻辑的计算和处理；

- **基础层**（Infrastructure）：主要包含 **Tunnel（数据通道）、防腐层，Config 和 Common**，Config 负责应用的配置；Common 是通用的工具类。

    > 这里我们使用 **Tunnel** 这个概念来**对所有的数据来源进行抽象**，这些数据来源可以是数据库（MySQL，NoSQL）、搜索引擎、文件系统、也可以是 SOA 服务等。

## 3. DDD + RPC 搭建

本项目的整体模块图如下：

![image-20230405122824440](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051228851.png)

由于引入了 RPC 接口，所以按照模块拆分的话，需要做一些处理：

- 应用层，不再给领域层定义接口，而是自行处理对领域层接口的包装。否则领域层既引入了应用层的 Jar，应用层也引入了领域层的 Jar，就会出现循环依赖的问题；
- 基础层中的数据仓储的定义也需要从领域层剥离（图中虚线），否则也会出现循环依赖的问题；
- RPC 层定义接口描述，包括：入参 Req、出参 Res、DTO 对象，接口信息。这些内容定义出来的 Jar 给接口层使用，也给外部调用方使用。

那么，这样拆分以后就可以按照模块化的结构进行创建系统结构了，每一层按照各自的职责完成各自的功能，同时又不会破坏 DDD中领域充血模型的实现。

整体目录结构如下所示：

![image-20230405125428051](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304051254968.png)