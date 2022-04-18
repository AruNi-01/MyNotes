# 1. 环境搭建

项目说明：

1. 提供前端工程，只需要实现后端接口即可
2. 项目以单体架构入手，先快速开发，不考虑项目优化，降低开发负担
3. 开发完成后，开始优化项目，提升编程思维能力
4. 比如页面静态化，缓存，云存储，日志等
5. docker部署上线
6. 云服务器购买，域名购买，域名备案等

项目环境：

- JDK 1.8
- IDEA 2021.3
- SpringBoot 2.5
- MySQL 5.7

技术栈 ：

SpringBoot + MybatisPlus+Redis+MySQL



## 1.1 创建数据库

> 文章表

```sql
CREATE TABLE `blog`.`blog_article`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `comment_counts` int(0) NULL DEFAULT NULL COMMENT '评论数量',
  `create_date` bigint(0) NULL DEFAULT NULL COMMENT '创建时间',
  `summary` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '简介',
  `title` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '标题',
  `view_counts` int(0) NULL DEFAULT NULL COMMENT '浏览数量',
  `weight` int(0) NOT NULL COMMENT '是否置顶',
  `author_id` bigint(0) NULL DEFAULT NULL COMMENT '作者id',
  `body_id` bigint(0) NULL DEFAULT NULL COMMENT '内容id',
  `category_id` int(0) NULL DEFAULT NULL COMMENT '类别id',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 25 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```

> 文章标签表和标签表

```sql
CREATE TABLE `blog`.`blog_article_tag`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `article_id` bigint(0) NOT NULL,
  `tag_id` bigint(0) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `article_id`(`article_id`) USING BTREE,
  INDEX `tag_id`(`tag_id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

CREATE TABLE `blog`.`blog_tag`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `avatar` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NULL DEFAULT NULL,
  `tag_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 11 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```

> 用户表

```sql
CREATE TABLE `blog`.`blog_sys_user`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `account` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '账号',
  `admin` bit(1) NULL DEFAULT NULL COMMENT '是否管理员',
  `avatar` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '头像',
  `create_date` bigint(0) NULL DEFAULT NULL COMMENT '注册时间',
  `deleted` bit(1) NULL DEFAULT NULL COMMENT '是否删除',
  `email` varchar(128) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '邮箱',
  `last_login` bigint(0) NULL DEFAULT NULL COMMENT '最后登录时间',
  `mobile_phone_number` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '手机号',
  `nickname` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '昵称',
  `password` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '密码',
  `salt` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '加密盐',
  `status` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '状态',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 16 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```



## 1.2 新建项目

1. 新建SpringBoot项目，添加依赖：

   ![image-20220410134421824](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220410134421824.png)

2. 在`pom.xml`中添加项目相关依赖：

   ```java
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.5.12</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
       <groupId>com.run</groupId>
       <artifactId>blog</artifactId>
       <version>0.0.1-SNAPSHOT</version>
       <name>blog</name>
       <description>blog</description>
       <properties>
           <java.version>1.8</java.version>
       </properties>
       <dependencies>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter</artifactId>
               <!-- 排除 默认使用的logback(slf4j和log4j不兼容) -->
               <exclusions>
                   <exclusion>
                       <groupId>org.springframework.boot</groupId>
                       <artifactId>spring-boot-starter-logging</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
   
           <!-- log4j2 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-log4j2</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-aop</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-mail</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-data-redis</artifactId>
           </dependency>
   
   
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>fastjson</artifactId>
               <version>1.2.76</version>
           </dependency>
   
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.apache.commons</groupId>
               <artifactId>commons-lang3</artifactId>
           </dependency>
   
           <dependency>
               <groupId>commons-collections</groupId>
               <artifactId>commons-collections</artifactId>
               <version>3.2.2</version>
           </dependency>
   
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-boot-starter</artifactId>
               <version>3.4.3</version>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-configuration-processor</artifactId>
               <optional>true</optional>
           </dependency>
   
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
                   <configuration>
                       <excludes>
                           <exclude>
                               <groupId>org.projectlombok</groupId>
                               <artifactId>lombok</artifactId>
                           </exclude>
                       </excludes>
                   </configuration>
               </plugin>
           </plugins>
       </build>
   
   </project>
   ```



## 1.2 配置

1. 在`resource`包下新建`application.yaml`，编写配置信息：

   ```yaml
   server:
     port: 8888
     servlet:
       application-display-name: run_blog
   
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=UTF-8&serverTimeZone=UTC
       username: root
       password: 123456
       driver-class-name: com.mysql.cj.jdbc.Driver
       
   # MybatisPlus输出SQL到控制台
   mybatis-plus:
     configuration:
       log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
     global-config:
       db-config:
         # 输出前缀为blog_表的SQL
         table-prefix: blog_
   ```

2. 新建`config`配置包，创建`MybatisPlusConfig`类，添加分页插件：

   ```java
   @Configuration
   // 扫包，将此包下的接口生成代理实现类，并且注册到spring容器中
   @MapperScan("com.run.blog.dao")
   public class MybatisPlusConfig {
   
       @Bean
       public MybatisPlusInterceptor mybatisPlusInterceptor() {
           MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
           // 添加分页插件
           interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
           return interceptor;
       }
   }
   ```

3. 在`config`包下新建`WebMvcConfig`类，编写跨域配置：

   ```java
   public class WebMvcConfig implements WebMvcConfigurer {
   
       @Override
       public void addCorsMappings(CorsRegistry registry) {
           // 跨域配置，让前端ip能访问本项目(8888)的所有接口
           registry.addMapping("/**").allowedOrigins("http://localhost:8080");
       }
   }
   ```



# 2. 首页

## 2.1 文章列表

### 2.1.1 接口说明

接口url：/articles

请求方式：POST

请求参数：

| 参数名称 | 参数类型 | 说明           |
| -------- | -------- | -------------- |
| page     | int      | 当前页数       |
| pageSize | int      | 每页显示的数量 |

返回数据：

```json
{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": [
        {
            "id": 1,
            "title": "springboot介绍以及入门案例",
            "summary": "通过Spring Boot实现的服务，只需要依靠一个Java类，把它打包成jar，并通过`java -jar`命令就可以运行起来。\r\n\r\n这一切相较于传统Spring应用来说，已经变得非常的轻便、简单。",
            "commentCounts": 2,
            "viewCounts": 54,
            "weight": 1,
            "createDate": "2609-06-26 15:58",
            "author": "12",
            "body": null,
            "tags": [
                {
                    "id": 5,
                    "avatar": null,
                    "tagName": "444"
                },
                {
                    "id": 7,
                    "avatar": null,
                    "tagName": "22"
                },
                {
                    "id": 8,
                    "avatar": null,
                    "tagName": "11"
                }
            ],
            "categorys": null
        },
        {
            "id": 9,
            "title": "Vue.js 是什么",
            "summary": "Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。",
            "commentCounts": 0,
            "viewCounts": 3,
            "weight": 0,
            "createDate": "2609-06-27 11:25",
            "author": "12",
            "body": null,
            "tags": [
                {
                    "id": 7,
                    "avatar": null,
                    "tagName": "22"
                }
            ],
            "categorys": null
        },
        {
            "id": 10,
            "title": "Element相关",
            "summary": "本节将介绍如何在项目中使用 Element。",
            "commentCounts": 0,
            "viewCounts": 3,
            "weight": 0,
            "createDate": "2609-06-27 11:25",
            "author": "12",
            "body": null,
            "tags": [
                {
                    "id": 5,
                    "avatar": null,
                    "tagName": "444"
                },
                {
                    "id": 6,
                    "avatar": null,
                    "tagName": "33"
                },
                {
                    "id": 7,
                    "avatar": null,
                    "tagName": "22"
                },
                {
                    "id": 8,
                    "avatar": null,
                    "tagName": "11"
                }
            ],
            "categorys": null
        }
    ]
}
```

### 2.1.2 编码

> Dao层

1. 在`dao`包下新建`pojo`包，创建实体类：

   文章实体类`Article`：

   ```java
   @Data
   public class Article {
       public static final int Article_TOP = 1;
   
       public static final int Article_Common = 0;
   
       private Long id;
   
       private String title;
   
       private String summary;
   
       private int commentCounts;
   
       private int viewCounts;
   
       /**
        * 作者id
        */
       private Long authorId;
       /**
        * 内容id
        */
       private Long bodyId;
       /**
        *类别id
        */
       private Long categoryId;
   
       /**
        * 置顶
        */
       private int weight = Article_Common;
   
   
       /**
        * 创建时间
        */
       private Long createDate;
   }
   ```

   用户实体类`SysUser`：

   ```java
   @Data
   public class SysUser {
       private Long id;
   
       private String account;
   
       private Integer admin;
   
       private String avatar;
   
       private Long createDate;
   
       private Integer deleted;
   
       private String email;
   
       private Long lastLogin;
   
       private String mobilePhoneNumber;
   
       private String nickname;
   
       private String password;
   
       private String salt;
   
       private String status;
   }
   ```

   标签实体类`Tag`：

   ```java
   @Data
   public class Tag {
       private Long id;
   
       private String avatar;
   
       private String tagName;
   }
   ```

2. 在`dao`包下新建`mapper`类，创建相关mapper接口，继承`BaseMapper<>`，直接使用MybatisPlus的SQL：

   ```java
   public interface ArticleMapper extends BaseMapper<ArticleMapper> {
   }
   
   public interface SysUserMapper extends BaseMapper<SysUserMapper> {
   }
   
   public interface TagMapper extends BaseMapper<Tag> {
   }
   ```





> Controller层

先创建一个vo(value object)包，用来做值的存储与传递。

1. 新建controller包，创建`ArticleController`类：

   ```java
   ```

   

   















