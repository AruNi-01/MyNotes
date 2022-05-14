## 1. Nosql概述

### 1.1 为什么使用Nosql

> 1、单机Mysql时代

![image-20220405190302277](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b70477a767f3a2c2caa82a7fe2979b87.png)

90年代，一个网站的访问量一般不会太大，单个数据库完全够用。随着用户增多，网站出现以下问题

1. 数据量增加到一定程度，单机数据库就放不下了
2. 数据的索引（B+ Tree），一个机器内存也存放不下
3. 访问量变大后（读写混合），一台服务器承受不住。

> 2、[Memcached](https://so.csdn.net/so/search?q=Memcached&spm=1001.2101.3001.7020)(缓存) + Mysql + 垂直拆分（读写分离）

网站80%的情况都是在读，每次都要去查询数据库的话就十分的麻烦！所以说我们希望减轻数据库的压力，我们可以使用**缓存**来保证效率！

![image-20220405192019200](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/35fcd5810e0eaabdaa808aeb6883a111.png)

优化过程经历了以下几个过程：

1. 优化数据库的数据结构和索引 (难度大)
2. 文件缓存，通过IO流获取比每次都访问数据库效率略高，但是流量爆炸式增长时候，IO流也承受不了
3. MemCache，当时最热门的技术，通过在数据库和数据库访问层之间加上一层缓存，第一次访问时查询数据库，将结果保存到缓存，后续的查询先检查缓存，若有直接拿去使用，效率显著提升。

> 3、分库分表 + 水平拆分 + Mysql集群

早些年，数据库引擎用的MyISAM，在读数据时会锁表，十分影响效率，高并发下就会出现严重的锁问题。

后来将引擎转为Innodb，在读数据时只会锁行，比锁表大大提高了效率。

再后来就开始使用分库分表来解决写数据的压力

有了Mysql集群后，很好解决了当时的所有需求

![在这里插入图片描述](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/86d2f5001149eb6b6f139c0b8c2e6a85.png)

> 4、如今最近的年代

如今信息量井喷式增长，各种各样的数据出现（用户定位数据，图片数据，热榜单数据等），大数据的背景下关系型数据库（RDBMS）无法满足大量数据要求。Nosql数据库就能轻松解决这些问题。

> 目前一个基本的互联网项目

![image-20220405193823327](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a7abf57a6cbaac2ec42f3532a4dc67c0.png)

> 为什么要用NoSQL ？

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长！
这时候我们就需要使用NoSQL数据库的，Nosql可以很好的处理以上的情况！

### 1.2 什么是Nosql

**NoSQL = Not Only SQL（不仅仅是SQL）**

Not Only Structured Query Language

- 关系型数据库：列 + 行，同一个表下数据的结构是一样的。

- 非关系型数据库：数据存储没有固定的格式，并且可以进行横向扩展。

**NoSQL泛指非关系型数据库**，随着web2.0互联网的诞生，传统的关系型数据库很难对付web2.0时代！尤其是超大规模的高并发的社区，暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，Redis是发展最快的。

### 1.3 Nosql特点

1. 方便扩展（数据之间没有关系，很好扩展！）

2. 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）

3. 数据类型是多样型的！（不需要事先设计数据库，随取随用）

4. 传统的 RDBMS 和 NoSQL

   - 传统的RDBMS(关系型数据库)：
     - 结构化组织
     - SQL
     - 数据和关系都存在单独的表中 row col
     - 操作，数据定义语言
     - 严格的一致性
     - 基础的事务
   - Nosql(非关系型数据库)：
     - 不仅仅是数据
     - 没有固定的查询语言
     - 键值对存储，列存储，文档存储，图形数据库（社交关系）
     - 最终一致性
     - CAP定理和BASE
     - 高性能，高可用，高扩展

   

> 了解：3V + 3高

大数据时代的3V ：主要是**描述问题**的

1. 海量Velume
2. 多样Variety
3. 实时Velocity

大数据时代的3高 ： 主要是**对程序的要求**

1. 高并发
2. 高可扩
3. 高性能

真正在公司中的实践：NoSQL + RDBMS 一起使用才是最强的。

### 1.4 阿里巴巴演进分析

推荐阅读：阿里云的这群疯子https://yq.aliyun.com/articles/653511

![image-20220405200217079](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0966ece97f9af0b663114807babe12c1.png)

![image-20220405201024758](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bc73178b2b07bbf682e9de8e63d43bd2.png)

```shell
# 商品信息
- 一般存放在关系型数据库：Mysql，阿里巴巴使用的Mysql都是经过内部改动的

# 商品描述、评论(文字居多)
- 文档型数据库：MongoDB

# 图片
- 分布式文件系统 FastDFS
- 淘宝：TFS
- Google: GFS
- Hadoop: HDFS
- 阿里云: OSS

# 商品关键字 用于搜索
- 搜索引擎：Solr, Elasticsearch
- 阿里：ISearch

# 商品热门的波段信息
- 内存数据库：Redis，Memcache

# 商品交易，外部支付接口
- 第三方应用
```

### 1.5 Nosql的四大分类

> KV键值对

- 新浪：**Redis**
- 美团：Redis + Tair
- 阿里、百度：Redis + Memcache

> 文档型数据库（Bson数据格式）

- **MongoDB**(掌握)
  - 基于分布式文件存储的数据库。C++编写，用于处理大量文档。
  - MongoDB是RDBMS和NoSQL的中间产品。MongoDB是非关系型数据库中功能最丰富的，NoSQL中最像关系型数据库的数据库。
- ConthDB

> 列存储数据库

- **HBase**(大数据必学)
- 分布式文件系统

> 图关系数据库

用于存广告推荐，社交网络，并不是存图形的

- **Neo4j**、InfoGrid



> 四者对比

| **分类**                | **Examples举例**                                   | **典型应用场景**                                             | **数据模型**                                    | **优点**                                                     | **缺点**                                                     |
| ----------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **键值对（key-value）** | Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等 | Key 指向 Value 的键值对，通常用hash table来实现 | 查找速度快                                                   | 数据无结构化，通常只被当作字符串或者二进制数据               |
| **列存储数据库**        | Cassandra, HBase, Riak                             | 分布式的文件系统                                             | 以列簇式存储，将同一列数据存在一起              | 查找速度快，可扩展性强，更容易进行分布式扩展                 | 功能相对局限                                                 |
| **文档型数据库**        | CouchDB, MongoDb                                   | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） | Key-Value对应的键值对，Value为结构化数据        | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                       |
| **图形(Graph)数据库**   | Neo4J, InfoGrid, Infinite Graph                    | 社交网络，推荐系统等。专注于构建关系图谱                     | 图结构                                          | 利用图结构相关算法。比如最短路径寻址，N度关系查找等          | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群 |

## 2. Redis入门

### 2.1 概述

> Redis是什么？

Redis（**Re**mote **Di**ctionary **S**erver )，即远程字典服务。

是一个开源的使用ANSI C语言编写、支持网络、可基于[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)亦可持久化的日志型、**Key-Value数据库**，并提供多种语言的API。

与Memcached一样，为了保证效率，**数据都是缓存在内存中**。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

> Redis能该干什么

1. 内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
2. 高效率、用于高速缓冲
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器(eg：浏览量)

> 特性

1. 多样的数据类型
2. 持久化
3. 集群
4. 事务

### 2.2 环境搭建

官网：https://redis.io/

推荐使用Linux服务器学习。

windows版本的Redis已经停更很久了…

### 2.3 Windows安装

https://github.com/dmajkic/redis

1. 解压安装包
   ![在这里插入图片描述](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/36160ddab6497a9d5d665ca83debdec4.png)
2. 开启`redis-server.exe`
3. 启动`redis-cli.exe`测试![在这里插入图片描述](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c475d8b0eb5b6b90b30cea48ef8388bb.png)

### 2.4 Linux安装

1、下载安装包！`redis-5.0.8.tar.gz`，程序一般放在`/opt`目录下

2、解压Redis的安装包

```shell
[root@VM-16-12-centos opt]# ls
mellanox  redis-6.0.6.tar.gz  rh
[root@VM-16-12-centos opt]# tar -zxvf redis-6.0.6.tar.gz
# 解压......
```

解压完成后查看目录：

![image-20220405224639191](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/43396ee7e60f887c2d64d13cf9e0a1f4.png)



3、进入redis目录，可以看到redis的配置文件：

![image-20220405224842259](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ba5ecf5bd52416d66457c5a6726473a4.png)4、基本环境安装

```bash
# 先安装gcc
yum install gcc-c++
# 然后执行make
make
# 最后执行
make install
```

![image-20220405231347844](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f3c39bf977b6b230ff0c84dfe7f3ab93.png)

注意：安装6.0以上版本需要升级gcc到5.3及以上,如下：升级到gcc 9.3：
`yum -y install centos-release-scl`
`yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils`
`scl enable devtoolset-9 bash`
需要注意的是scl命令启用只是临时的，退出shell或重启就会恢复原系统gcc版本。
如果要长期使用gcc 9.3的话：
`echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile`
这样退出shell重新打开就是新版的gcc了



5、redis默认安装路径 `/usr/local/bin`

![image-20220405231850548](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7cda151f3fd5218a797811d120616521.png)

6、将redis的配置文件复制到当前程序安装目录 `/usr/local/bin/自己的配置文件处`下，我们之后就用这个配置文件启动redis

![image-20220405232203634](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/68c2bef1f5cdf04b4d1652a17916a3b0.png)

7、redis默认不是后台启动的，需要修改配置文件：`vim redis.conf`后按`i`进入编辑，将`daemonize no`改为`yes`后按`esc`，再按`:`+`wq`保存退出

![image-20220405232405071](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bf6af700e8940ecaade24413bc30c651.png)

8、通过指定的配置文件启动redis服务`redis-server run-config/redis.conf`

![image-20220405232724996](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e995951883dc782b151472421b8a70ae.png)

9、使用`redis-cli`连接指定的端口号测试，Redis的默认端口6379：`redis-cli -p 6379`

![image-20220405233015262](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a203a551f589307803094866a34c5a74.png)

10、新建一个会话，链接服务器；查看redis进程是否开启`ps -ef|grep redis`

![image-20220405233659344](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1d3aeaf64c7a2140c264367d649b75a8.png)

11、关闭Redis服务 `shutdown`，退出`exit`

![image-20220405233956439](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b41a5fccb0927ecff13cf270f243621c.png)

12、再次查看进程是否存在，可以看到redis-server和redis-cli都没有了

![image-20220405233902856](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9a90206448ec6a8835190b908bfedb37.png)

13、后面我们会使用单机多Redis启动集群测试

### 2.5 测试性能

**redis-benchmark：**Redis官方提供的性能测试工具，参数选项如下：

![image-20220405234225650](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9d10abc41e6816807f12ae8e4767afd6.png)

**简单测试**：100个并发连接 100000请求`redis-benchmark -h localhost -p 6379 -c 100 -n 100000`

1. 先开启redis服务：

   ![image-20220405235035837](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/42c9c63c6b8880fa14a084eb2cbf609f.png)

2. 在另一个会话中测试：

   ![image-20220405235253678](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/aa0f9232446d290eae85ac2822204cc1.png)

3. 分析：

   ![image-20220405235704492](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6da574c400a343a21d675d6edfbc91c3.png)

### 2.6 基础知识

> redis默认有16个数据库

默认使用的第0个

16个数据库为：DB 0 ~ DB 15
默认使用DB 0 ，可以使用`select n`切换到DB n，`dbsize`可以查看当前数据库的大小，与key数量相关。

```bash
127.0.0.1:6379> config get databases # 命令行查看数据库数量databases
1) "databases"
2) "16"

127.0.0.1:6379> select 8 # 切换数据库 DB 8
OK
127.0.0.1:6379[8]> dbsize # 查看数据库大小
(integer) 0

# 不同数据库之间 数据是不能互通的，并且dbsize 是根据库中key的个数。
127.0.0.1:6379> set name sakura 
OK
127.0.0.1:6379> SELECT 8
OK
127.0.0.1:6379[8]> get name # db8中并不能获取db0中的键值对。
(nil)
127.0.0.1:6379[8]> DBSIZE
(integer) 0
127.0.0.1:6379[8]> SELECT 0
OK
127.0.0.1:6379> keys *
1) "counter:__rand_int__"
2) "mylist"
3) "name"
4) "key:__rand_int__"
5) "myset:__rand_int__"
127.0.0.1:6379> DBSIZE # size和key个数相关
(integer) 5
```

`keys *` ：查看当前数据库中所有的key。

`flushdb`：清空当前数据库中的键值对。

`flushall`：清空所有数据库的键值对。

> **Redis不仅仅是单线程，Redis是基于内存操作的**

一般来说 Redis 的瓶颈并不在 CPU，而在内存和网络。如果要使用 CPU 多核，可以搭建多个 Redis 实例来解决。

其实，Redis 4.0 开始就有多线程的概念了，比如 Redis 通过多线程方式在后台删除对象、以及通过 Redis 模块实现的阻塞命令等。

**Redis6.0 已经发布，在网络 IO 处理方面用上了多线程，如网络数据的读写和协议解析等，需要注意的是，执行命令的核心模块还是单线程的**

> 为什么网络处理要引入多线程

Redis 的瓶颈并不在 CPU，而在内存和网络。

内存不够的话，可以加内存或者做数据结构优化和其他优化等，但网络的性能优化才是大头，网络 IO 的读写在 Redis 整个执行期间占用了大部分的 CPU 时间，如果把网络处理这部分做成多线程处理方式，那对整个 Redis 的性能会有很大的提升。

> **Redis为什么单线程还这么快**

- 误区1：高性能的服务器一定是多线程的？
- 误区2：多线程（CPU上下文会切换）一定比单线程效率高！

核心：Redis是将所有的数据放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作），对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个CPU上的，在内存存储数据情况下，单线程就是最佳的方案。

## 3. 五大数据类型

Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作**数据库**，**高速缓存**和**消息队列代理**

支持的数据类型：

-   字符串（string）
-   哈希（hash）
-   列表（list）
-   集合（set）
-   有序集合（sorted set）
-   位图  ( Bitmaps )
-   基数统计 ( HyperLogLogs )

内置复制、Lua脚本、LRU收回、事务以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis Cluster提供自动分区

### 3.1 Redis-key

> 在redis中无论什么数据类型，在数据库中都是以key-value形式保存，通过进行对Redis-key的操作，来完成对数据库中数据的操作。

常用命令：

- `exists key`：判断键是否存在
- `del key`：删除键值对
- `move key db`：将键值对移动到指定数据库
- `expire key second`：设置键值对的过期时间
- `type key`：查看value的数据类型

```bash
127.0.0.1:6379> keys * 	# 查看当前数据库所有key
(empty list or set)
127.0.0.1:6379> set name qinjiang 	# set key
OK
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> move age 1 	# 将键值对移动到指定数据库
(integer) 1
127.0.0.1:6379> EXISTS age 	# 判断键是否存在
(integer) 0		 # 不存在
127.0.0.1:6379> EXISTS name
(integer) 1 	# 存在
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> keys *
1) "age"
127.0.0.1:6379[1]> del age 		# 删除键值对
(integer) 1 	# 删除个数


127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> expire age 15 	# 设置键值对的过期时间，单位秒
(integer) 1 	# 设置成功 开始计数
127.0.0.1:6379> ttl age		 # 查看key的过期剩余时间
(integer) 13
127.0.0.1:6379> ttl age
(integer) 11
127.0.0.1:6379> ttl age
(integer) 9
127.0.0.1:6379> ttl age
(integer) -2 	# -2 表示key过期，-1表示key未设置过期时间

127.0.0.1:6379> get age 	# 过期的key 会被自动delete
(nil)
127.0.0.1:6379> keys *
1) "name"

127.0.0.1:6379> type name 	# 查看value的数据类型
string
```

> 关于`TTL`命令

Redis的key，通过TTL命令返回key的过期时间，一般来说有3种：

1. 当前key没有设置过期时间，所以会返回-1
2. 当前key有设置过期时间，而且key已经过期，所以会返回-2
3. 当前key有设置过期时间，且key还没有过期，故会返回 key 的正常剩余时间

> 关于重命名`RENAME`和`RENAMENX`

- `RENAME key newkey`修改 key 的名称
- `RENAMENX key newkey`仅当 newkey 不存在时，将 key 改名为 newkey 。

更多命令学习：https://www.redis.net.cn/order/

### 3.2 String(字符串)

> String常用命令

- `APPEND key value`：向指定key的value后面追加字符串

  ```bash
  127.0.0.1:6379> set msg Hello, 
  OK
  127.0.0.1:6379> append msg Redis!	# 追加，若不存在该key就是set
  (integer) 12
  127.0.0.1:6379> get msg
  "Hello,Redis!"
  127.0.0.1:6379> strlen msg		# 获取长度
  (integer) 12
  ```

- `INCR/DECR key`：将指定key的value数值+1/-1(仅对于数字)

  ```bash
  127.0.0.1:6379> set views 0
  OK
  127.0.0.1:6379> incr views
  (integer) 1
  127.0.0.1:6379> get views
  "1"
  127.0.0.1:6379> decr views
  (integer) 0
  127.0.0.1:6379> get views
  "0"
  ```

- `INCRBY/DECRBY key n`：按指定的步长对数值进行加减

  ```bash
  127.0.0.1:6379> get views
  "0"
  127.0.0.1:6379> incrby views 10
  (integer) 10
  127.0.0.1:6379> get views
  "10"
  127.0.0.1:6379> decrby views 5
  (integer) 5
  127.0.0.1:6379> get views
  "5"
  ```

- `INCREBYFLOAT key n`：为数值加上浮点型数值

  ```bash
  127.0.0.1:6379> get views
  "5"
  127.0.0.1:6379> INCRBYFLOAT views 0.20
  "5.2"
  ```

- `STRLEN key`：获取key的值的字符串长度

  ```bash
  127.0.0.1:6379> get msg
  "Hello,Redis!"
  127.0.0.1:6379> strlen msg
  (integer) 12
  ```

- `GETRANGE key start end`：按起止位置(从0开始)获取字符串(左闭右闭) range: 范围

  ```bash
  127.0.0.1:6379> get msg
  "Hello,Redis!"
  127.0.0.1:6379> getrange msg 0 4
  "Hello"
  ```

- `SETRANGE key offset value`：用指定的value替换key中offset开始的值(value有多少个就替换多少个)

  ```bash
  127.0.0.1:6379> get msg
  "Hello,Redis!"
  127.0.0.1:6379> setrange msg 6 Linux
  (integer) 12
  127.0.0.1:6379> get msg
  "Hello,Linux!"
  ```

- `GETSET key value`：如果不存在值，则返回nil，如果存在值，反回原来的值，并设置新的值

  ```bash
  127.0.0.1:6379> get msg
  "Hello,Linux!"
  127.0.0.1:6379> getset msg Hello,World!
  "Hello,Linux!"
  127.0.0.1:6379> get msg
  "Hello,World!"
  ```

- `SETNX key value`：仅当key不存在时进行set      NX: not exist

  ```bash
  127.0.0.1:6379> get msg
  "Hello,World!"
  127.0.0.1:6379> setnx msg Hi	# 当key存在时，set失败
  (integer) 0
  127.0.0.1:6379> get msg
  "Hello,World!"
  127.0.0.1:6379> setnx msg2 Hi	# 当key不存在时，set成功
  (integer) 1
  127.0.0.1:6379> get msg2
  "Hi"
  ```

- `SETEX key seconds value`：set键值对并且设置过期时间，单位为秒   EX: expire失效

  ```bash
  127.0.0.1:6379> setex name 20 AruNi		# 20s过期
  OK
  127.0.0.1:6379> ttl name
  (integer) 16
  127.0.0.1:6379> ttl name
  (integer) 1
  127.0.0.1:6379> ttl name	#已经过期了
  (integer) -2
  127.0.0.1:6379> get name
  (nil)
  ```

- `PSETEX key milliseconds value`：和`SETEX`一样，set键值对并设置过期时间，只是单位为毫秒

- `MSET key 1 value 1 [key2 value2 ......]`：批量set键值对

  ```bash
  127.0.0.1:6379> mset name AruNi age 18 hobby coding
  OK
  127.0.0.1:6379> keys *
  1) "hobby"
  2) "age"
  3) "name"
  ```

- `MGET key1 [key2 ......]`：批量获取key的值

  ```bash
  127.0.0.1:6379> mget name age hobby
  1) "AruNi"
  2) "18"
  3) "coding"
  ```

- `MSETNX key1 value1 [key2 value2 .....]`：批量设置键值对，仅当参数中所有的key都不存在时才执行成功

  ```bash
  127.0.0.1:6379> msetnx name admin sex man	# name已经存在了，整体set失败
  (integer) 0
  127.0.0.1:6379> keys *
  1) "hobby"
  2) "age"
  3) "name"
  127.0.0.1:6379> msetnx sex man birth 2001/10/12	 	# 所有key都不存在，set成功
  (integer) 1
  127.0.0.1:6379> keys *
  1) "hobby"
  2) "name"
  3) "sex"
  4) "age"
  5) "birth"
  127.0.0.1:6379> get birth
  "2001/10/12"
  ```

  

对于对象的处理：

1. `set user:1 {name:zhangsan,age:3}`：将`user:1`作为key，值为json字符串来保存一个对象

2. `mset user:1:name zhangsan user:1:age 18`：将`user:1:xxx`作为key，值为xxx的属性值

   ```bash
   127.0.0.1:6379> mset user:1:name zhangsan user:1:age 18
   OK
   127.0.0.1:6379> mget user:1:name user:1:age
   1) "zhangsan"
   2) "18"
   ```

   这里的key是一个巧妙地设计：`user:{id}:{filed}`



String类似的使用场景：value除了是字符串还可以是数字，用途举例：

- 计数器
- 统计多单位的数量：`uid:123666:follow 0`
- 粉丝数
- 对象存储缓存

### 3.3 List(列表)

> Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）
>
> 一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

首先我们列表，可以经过规则定义将其变为队列、栈、双端队列等

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-VPvbIltc-1597890996518)(狂神说 Redis.assets/image-20200813114255459.png)]](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fa386ed6b605b06f11359df4a31ff0e7.png)

正如图Redis中List是可以进行**双端**操作的，所以命令也就分为了LXXX和RXXX两类，有时候L也表示List例如LLEN

> List常用命令

- `LPUSH key value`、`RPUSH key value`：向头部/尾部插入元素

- `LRANGE key start end`：按起始位置获取元素值

  ```bash
  127.0.0.1:6379> lpush myList one	# lpush将值放在头部
  (integer) 1
  127.0.0.1:6379> lpush myList two
  (integer) 2
  127.0.0.1:6379> lpush myList three
  (integer) 3
  127.0.0.1:6379> get myList	 # 普通的get是无法获取list值的
  (error) WRONGTYPE Operation against a key holding the wrong kind of value
  127.0.0.1:6379> lrange myList 0 -1 	# 获取全部元素
  1) "three"
  2) "two"
  3) "one"
  127.0.0.1:6379> lrange myList 0 1	# 获取0-1号元素（lpush和栈类似）
  1) "three"
  2) "two"
  
  127.0.0.1:6379> rpush myList 1 2 3		# rpush将值放在尾部
  (integer) 6
  127.0.0.1:6379> lrange myList 0 -1
  1) "three"
  2) "two"
  3) "one"
  4) "1"
  5) "2"
  6) "3"
  ```

- `LPUSHX key value`、`RPUSHX key value`：判断列表是否存在后再向头部/尾部添加元素

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "2"
  3) "3"
  4) "4"
  127.0.0.1:6379> lpushx my 1 2	# my列表不存在，添加失败
  (integer) 0
  127.0.0.1:6379> lpushx myList 5
  (integer) 5
  127.0.0.1:6379> rpushx myList 0
  (integer) 6
  127.0.0.1:6379> lrange myList 0 -1
  1) "5"
  2) "1"
  3) "2"
  4) "3"
  5) "4"
  6) "0"
  ```

- `LPOP key`、`RPOP key`：从头部/尾部弹出元素

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "three"
  2) "two"
  3) "one"
  4) "1"
  5) "2"
  6) "3"
  127.0.0.1:6379> lpop myList
  "three"
  127.0.0.1:6379> rpop myList
  "3"
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "one"
  3) "1"
  4) "2"
  ```

- `LINDEX key index`：通过index下标获取元素值

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "one"
  3) "1"
  4) "2"
  127.0.0.1:6379> lindex myList 1
  "one"
  127.0.0.1:6379> lindex myList 2
  "1"
  ```

- `LLEN key`：返回列表的长度

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "one"
  3) "1"
  4) "2"
  127.0.0.1:6379> llen myList
  (integer) 4
  ```

- `LSET key index value`：按index设置元素值，前提是列表的该index存在

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "2"
  3) "3"
  4) "4"
  127.0.0.1:6379> lset myList 1 two
  OK
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "two"
  3) "3"
  4) "4"
  127.0.0.1:6379> 
  
  ```

- `LREM key count element `：按值和个数移除列表中的元素

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "one"
  3) "1"
  4) "2"
  5) "one"
  6) "one"
  127.0.0.1:6379> lrem myList 1 one	# 只移除1个one，从上往下移
  (integer) 1
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "1"
  3) "2"
  4) "one"
  5) "one"
  127.0.0.1:6379> lrem myList 2 one	# 将两个one都移除
  (integer) 2
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "1"
  3) "2"
  ```

- `LTRIM key start stop `：按起始位置截取列表，其他的将被自动删除

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "2"
  3) "3"
  4) "4"
  5) "5"
  6) "6"
  127.0.0.1:6379> ltrim myList 2 4
  OK
  127.0.0.1:6379> lrange myList 0 -1
  1) "3"
  2) "4"
  3) "5"
  ```

- `RPOPLPUSH source destination `：将一个列表的最后一个元素移到另一个列表的头部

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "3"
  2) "4"
  3) "5"
  127.0.0.1:6379> lrange otherList 0 -1
  1) "two"
  2) "one"
  127.0.0.1:6379> rpoplpush myList otherList
  "5"
  127.0.0.1:6379> lrange myList 0 -1
  1) "3"
  2) "4"
  127.0.0.1:6379> lrange otherList 0 -1
  1) "5"
  2) "two"
  3) "one"
  ```

- `LINSERT key BEFORE | AFTER pivot element`：在指定元素之前/之后添加一个元素

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "2"
  3) "3"
  4) "4"
  127.0.0.1:6379> linsert myList before 2 two
  (integer) 5
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "two"
  3) "2"
  4) "3"
  5) "4"
  127.0.0.1:6379> linsert myList after 3 three
  (integer) 6
  127.0.0.1:6379> lrange myList 0 -1
  1) "1"
  2) "two"
  3) "2"
  4) "3"
  5) "three"
  6) "4"
  127.0.0.1:6379> 
  ```

- `BLPOP key [key ...] timeout`、`BRPOP`：移除并获取列表头部/尾部元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。可以设置备用key或超时时间

  ```bash
  127.0.0.1:6379> lrange myList 0 -1
  1) "two"
  2) "2"
  3) "3"
  4) "three"
  5) "4"
  127.0.0.1:6379> lrange otherList 0 -1
  1) "one"
  127.0.0.1:6379> blpop otherList myList 20	# 从otherList中的头部移除一个元素，myList是备选，超时时间为20s
  1) "otherList"
  2) "one"
  127.0.0.1:6379> blpop otherList myList 20
  1) "myList" 		# otherList中没有元素了，此时在myList中移除
  2) "1"
  127.0.0.1:6379> blpop otherList 20	# 没有备选key，otherList中也没有元素
  (nil)
  (20.01s)	# 超时了自动退出
  127.0.0.1:6379> 
  ```

  可以在该列表等待的时候，另外开一个会话，向该列表中插入一个元素，此时就发现可弹出元素了：

  ![image-20220406194041230](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f6d1e7206a6b346ca3dbd6b60552b567.png)

  ![image-20220406194134770](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b1ec9835fd2672f647eda6e652947830.png)

  ![image-20220406194404661](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7ff208c79eee7d0a9c49926e77b31644.png)

> 小结

- list实际上是一个链表，before Node after left right 都可以插入值
- **如果key不存在，则创建新的链表**
- 如果key存在，新增内容
- 如果移除了所有值，空链表，也代表不存在
- 在两边插入或者改动值，效率最高！修改中间元素，效率相对较低

**应用：**

**消息排队！消息队列(Lpush Rpop)，栈(Lpush Lpop)**

### 3.4 Set(集合)

- Redis的Set是**string类型**的**无序**集合。集合成员是唯一的，这就意味着集合中**不能**出现**重复**的数据

- Redis中集合是通过哈希表实现的，所以添加，删除，查找的时间复杂度都是O(1)

- 集合中最大的成员数为 2^32 - 1 (4294967295，每个集合可存储40多亿个成员)



| 命令                                      | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| `SADD key member1[member2..]`             | 向集合中无序增加一个/多个成员                                |
| `SCARD key`                               | 获取集合的成员数                                             |
| `SMEMBERS key`                            | 返回集合中所有的成员                                         |
| `SISMEMBER key member`                    | 查询member元素是否是集合的成员,结果是无序的                  |
| `SRANDMEMBER key [count]`                 | 随机返回集合中count个成员，count缺省值为1                    |
| `SPOP key [count]`                        | 随机移除并返回集合中count个成员，count缺省值为1              |
| `SMOVE source destination member`         | 将source集合的成员member移动到destination集合                |
| `SREM key member1[member2..]`             | 移除集合中一个/多个成员                                      |
| `SDIFF key1[key2..]`                      | 返回所有集合的差集 key1- key2 - …                            |
| `SDIFFSTORE destination key1[key2..]`     | 在SDIFF的基础上，将结果保存到集合中==(覆盖)==。不能保存到其他类型key噢！ |
| `SINTER key1 [key2..]`                    | 返回所有集合的交集                                           |
| `SINTERSTORE destination key1[key2..]`    | 在SINTER的基础上，存储结果到集合中。覆盖                     |
| `SUNION key1 [key2..]`                    | 返回所有集合的并集                                           |
| `SUNIONSTORE destination key1 [key2..]`   | 在SUNION的基础上，存储结果到及和张。覆盖                     |
| `SSCAN KEY [MATCH pattern] [COUNT count]` | 在大量数据环境下，使用此命令遍历集合中元素，每次遍历部分     |

> 单个集合操作

```bash
---------------SADD--SCARD--SMEMBERS--SISMEMBER--------------------

127.0.0.1:6379> sadd myset m1 m2 m3 m4 		# 向myset中增加成员 m1~m4
(integer) 4
127.0.0.1:6379> scard myset 	# 获取集合的成员数目
(integer) 4
127.0.0.1:6379> smembers myset 		# 获取集合中所有成员
1) "m4"
2) "m3"
3) "m2"
4) "m1"
127.0.0.1:6379> sismember myset m5 		# 查询m5是否是myset的成员
(integer) 0 		# 不是，返回0
127.0.0.1:6379> sismember myset m2
(integer) 1		 # 是，返回1
127.0.0.1:6379> sismember myset m3
(integer) 1

---------------------SRANDMEMBER--SPOP----------------------------------

127.0.0.1:6379> srandmenmber myset 3		 # 随机返回3个成员
1) "m2"
2) "m3"
3) "m4"
127.0.0.1:6379> srandmenmber myset 		# 随机返回1个成员
"m3"
127.0.0.1:6379> spop myset 2 		# 随机移除并返回2个成员
1) "m1"
2) "m4"
127.0.0.1:6379> smembers myset
1) "m3"
2) "m2"

---------------------SMOVE--SREM----------------------------------------
127.0.0.1:6379> smembers myset 
1) "m4"
2) "m3"
3) "m2"
4) "m1"
127.0.0.1:6379> smove myset newset m3 	# 将myset中m3成员移动到newset集合
(integer) 1
127.0.0.1:6379> smembers myset
1) "m4"
2) "m2"
3) "m1"
127.0.0.1:6379> smembers newset
1) "m3"
127.0.0.1:6379> srem newset m3		 # 从newset中移除m3元素
(integer) 1
127.0.0.1:6379> smembers newset
(empty list or set)
```

> 多集合操作，若只有一个参数则默认和自身进行运算

```bash
# setx, sety, setz中的元素如下：
# setx=>{m1,m2,m4,m6}, sety=>{m2,m5,m6}, setz=>{m1,m3,m6}

-----------------------------SDIFF：差集------------------------------------

127.0.0.1:6379> sdiff setx sety setz 		# 等价于setx - sety - setz
1) "m4"
127.0.0.1:6379> sdiff setx sety 		# setx - sety
1) "m4"
2) "m1"
127.0.0.1:6379> sdiff sety setx		 # sety - setx
1) "m5"

# 注意：sdiff中是以第一个参数为参考

-------------------------SINTER：交集---------------------------------------
127.0.0.1:6379> sinter setx sety setz 		# 求 setx、sety、setx的交集
1) "m6"
127.0.0.1:6379> sinter setx sety		 # 求setx sety的交集
1) "m2"
2) "m6"

-------------------------SUNION：并集---------------------------------------
127.0.0.1:6379> sunion setx sety setz 		# setx sety setz的并集
1) "m4"
2) "m6"
3) "m3"
4) "m2"
5) "m1"
6) "m5"
127.0.0.1:6379> sunion setx sety 		# setx sety 并集
1) "m4"
2) "m6"
3) "m2"
4) "m1"
5) "m5"
```

### 3.5 Hash（哈希）

- Redis Hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象

- Set就是一种简化的Hash，只变动key，而value使用默认值填充。可以将一个Hash表作为一个对象进行存储，表中存放对象的信息

| 命令                                             | 描述                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| `HSET key field value`                           | 将哈希表 key 中的字段 field 的值设为 value 。重复设置同一个field会覆盖,返回0 |
| `HMSET key field1 value1 [field2 value2..]`      | 同时将多个 field-value (域-值)对设置到哈希表 key 中。        |
| `HSETNX key field value`                         | 只有在字段 field 不存在时，设置哈希表字段的值。              |
| `HEXISTS key field`                              | 查看哈希表 key 中，指定的字段是否存在。                      |
| `HGET key field value`                           | 获取存储在哈希表中指定字段的值                               |
| `HMGET key field1 [field2..]`                    | 获取所有给定字段的值                                         |
| `HGETALL key`                                    | 获取在哈希表key 的所有字段和值                               |
| `HKEYS key`                                      | 获取哈希表key中所有的字段                                    |
| `HLEN key`                                       | 获取哈希表中字段的数量                                       |
| `HVALS key`                                      | 获取哈希表中所有值                                           |
| `HDEL key field1 [field2..]`                     | 删除哈希表key中一个/多个field字段                            |
| `HINCRBY key field n`                            | 为哈希表 key 中的指定字段的整数值加上增量n，并返回增量后结果 一样只适用于整数型字段 |
| `HINCRBYFLOAT key field n`                       | 为哈希表 key 中的指定字段的浮点数值加上增量 n。              |
| `HSCAN key cursor [MATCH pattern] [COUNT count]` | 迭代哈希表中的键值对。                                       |

```bash
------------------------HSET--HMSET--HSETNX----------------
127.0.0.1:6379> hset studentx name sakura 		# 将studentx哈希表作为一个对象，设置name为sakura
(integer) 1
127.0.0.1:6379> hset studentx name gyc 		# 重复设置field进行覆盖，并返回0
(integer) 0
127.0.0.1:6379> hset studentx age 20 		# 设置studentx的age为20
(integer) 1
127.0.0.1:6379> hmset studentx sex 1 tel 15623667886 		# 批量设置sex为1，tel为15623667886
OK
127.0.0.1:6379> hsetnx studentx name gyc 		# HSETNX 设置已存在的field
(integer) 0 	# 失败
127.0.0.1:6379> hsetnx studentx email 12345@qq.com
(integer) 1	 # 成功

-------------------HGET--HMGET--HGETALL-----------
127.0.0.1:6379> hget studentx name 		# 获取studentx中name字段的value
"gyc"
127.0.0.1:6379> hmget studentx name age tel 		# 批量获取studentx中name、age、tel字段的value
1) "gyc"
2) "20"
3) "15623667886"
127.0.0.1:6379> hgetall studentx 		# 获取studentx中所有的field及其value
 1) "name"
 2) "gyc"
 3) "age"
 4) "20"
 5) "sex"
 6) "1"
 7) "tel"
 8) "15623667886"
 9) "email"
10) "12345@qq.com"

--------------------HKEYS--HLEN--HVALS--------------
127.0.0.1:6379> hkeys studentx 		# 查看studentx中所有的field
1) "name"
2) "age"
3) "sex"
4) "tel"
5) "email"
127.0.0.1:6379> hlen studentx 		# 查看studentx中的字段数量
(integer) 5
127.0.0.1:6379> hvals studentx 		# 查看studentx中所有的value
1) "gyc"
2) "20"
3) "1"
4) "15623667886"
5) "12345@qq.com"

-------------------------HDEL--------------------------
127.0.0.1:6379> hdel studentx sex tel 	# 删除studentx 中的sex、tel字段
(integer) 2
127.0.0.1:6379> hkeys studentx
1) "name"
2) "age"
3) "email"

----------------------HEXISTS--------------------------------
127.0.0.1:6379> hexists studentx name		 # name字段在studentx中是否存在
(integer) 1 	# 存在
127.0.0.1:6379> hexists studentx addr
(integer) 0 	# 不存在

----------------------HSETNX--------------------------------
127.0.0.1:6379> hkeys studentx
1) "name"
2) "age"
3) "email"
127.0.0.1:6379> hsetnx studentx name zhangsan		# 如果不存在则可以设置
(integer) 0		# name在studentx中存在，设置失败

-------------HINCRBY--HINCRBYFLOAT------------------------
127.0.0.1:6379> hincrby studentx age 1 		# studentx的age字段数值+1
(integer) 21
127.0.0.1:6379> HINCRBY studentx name 1		 # 非整数字型字段不可用
(error) ERR hash value is not an integer
127.0.0.1:6379> hincrbyfloat studentx age 0.5 		# age字段增加0.6
"21.5"
```

 Hash变更的数据user name age，尤其是用户信息之类的，经常变动的信息！**Hash更适合于对象的存储，String更加适合字符串存储**

### 3.6 Zset（有序集合）

Zset其实就是SortedSet，这里的Z就行XYZ轴的Z，意味一个新的维度。

- 不同的是每个元素都会关联一个double类型的分数（score）。redis正是通过分数来为集合中的成员进行**从小到大**的排序。

- score相同：按字典顺序排序

- 有序集合的**成员是唯一**的,但**分数(score)**却**可以重复**。

| 命令                                              | 描述                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| `ZADD key score member1 [score2 member2]`         | 向有序集合添加一个或多个成员，或者更新已存在成员的分数       |
| `ZCARD key`                                       | 获取有序集合的成员数                                         |
| `ZCOUNT key min max`                              | 计算在有序集合中指定区间score的成员数                        |
| `ZINCRBY key n member`                            | 有序集合中对指定成员的分数加上增量 n                         |
| `ZSCORE key member`                               | 返回有序集中，成员的分数值                                   |
| `ZRANK key member`                                | 返回有序集合中指定成员的索引                                 |
| `ZRANGE key start end`                            | 通过索引区间返回有序集合成指定区间内的成员                   |
| `ZRANGEBYLEX key min max`                         | 通过字典区间返回有序集合的成员                               |
| `ZRANGEBYSCORE key min max`                       | 通过分数返回有序集合指定区间内的成员==-inf 和 +inf分别表示最小最大值，只支持开区间()== |
| `ZLEXCOUNT key min max`                           | 在有序集合中计算指定字典区间内成员数量                       |
| `ZREM key member1 [member2..]`                    | 移除有序集合中一个/多个成员                                  |
| `ZREMRANGEBYLEX key min max`                      | 移除有序集合中给定的字典区间的所有成员                       |
| `ZREMRANGEBYRANK key start stop`                  | 移除有序集合中给定的排名区间的所有成员                       |
| `ZREMRANGEBYSCORE key min max`                    | 移除有序集合中给定的分数区间的所有成员                       |
| `ZREVRANGE key start end`                         | 返回有序集中指定区间内的成员，通过索引，分数从高到底         |
| `ZREVRANGEBYSCORRE key max min`                   | 返回有序集中指定分数区间内的成员，分数从高到低排序           |
| `ZREVRANGEBYLEX key max min`                      | 返回有序集中指定字典区间内的成员，按字典顺序倒序             |
| `ZREVRANK key member`                             | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| `ZINTERSTORE destination numkeys key1 [key2 ..]`  | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中，numkeys：表示参与运算的集合数，将score相加作为结果的score |
| `ZUNIONSTORE destination numkeys key1 [key2..]`   | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| `ZSCAN key cursor [MATCH pattern\] [COUNT count]` | 迭代有序集合中的元素（包括元素成员和元素分值）               |

```bash
-------------------ZADD--ZCARD--ZCOUNT--------------
127.0.0.1:6379> zadd myzset 1 m1 2 m2 3 m3 	# 向有序集合myzset中添加成员m1 score=1 以及成员m2 score=2..
(integer) 3
127.0.0.1:6379> zcard myzset 	# 获取有序集合的成员数
(integer) 3
127.0.0.1:6379> zcount myzset 0 1 	# 获取score在 [0,1]区间的成员数量
(integer) 1
127.0.0.1:6379> zcount myzset 1 3
(integer) 3

----------------ZINCRBY--ZSCORE--------------------------
127.0.0.1:6379> zincrby myzset 5 m2 	# 将成员m2的score +5
"7"
127.0.0.1:6379> zscore myzset m1 	# 获取成员m1的score
"1"
127.0.0.1:6379> sscore myzset m2
"7"

--------------ZRANK--ZRANGE-----------------------------------
# 此时的zset集合：1 m1 7 m2 3 m3
127.0.0.1:6379> zrank myzset m1		 # 获取成员m1的索引，索引按照score升序排序，score相同索引值按字典顺序顺序增加
(integer) 0
127.0.0.1:6379> zrank myzset m2		# m2的score为7，排在最后
(integer) 2
127.0.0.1:6379> zrange myzset 0 1 	# 获取索引在 0~1的成员
1) "m1"
2) "m3"
127.0.0.1:6379> zrange myzset 0 -1 		# 获取全部成员
1) "m1"
2) "m3"
3) "m2"

-----------------------ZRANGEBYSCORE---------------------
127.0.0.1:6379> zrangebyscore myzset 1 10 	# 返回score在 [1,10]之间的的成员
1) "m1"
2) "m3"
3) "m2"
127.0.0.1:6379> zrangebyscore myzset 1 5
1) "m1"
2) "m3"

# 测试有序集合testset=>{abc,add,amaze,apple,back,java,redis} score均为0
------------------ZRANGEBYLEX--LIMIT---------------------------------
127.0.0.1:6379> zrangebylex testset - + 		# 返回所有成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
5) "back"
6) "java"
7) "redis"
127.0.0.1:6379> zrangebylex testset - + limit 0 3 		# 按index分页查询，获取从index=0开始的3个元素
1) "abc"
2) "add"
3) "amaze"
127.0.0.1:6379> zrangebylex testset - + limit 3 3		 # 显示 3,4,5条记录
1) "apple"
2) "back"
3) "java"
127.0.0.1:6379> zrangebylex testset (- [apple 		# 显示 (-,apple] 区间内的成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
127.0.0.1:6379> zrangebylex testset [apple [java 	# 显示 [apple,java]字典区间的成员
1) "apple"
2) "back"
3) "java"

--------------------ZLEXCOUNT-----------------------------
127.0.0.1:6379> zlexcount testset - +
(integer) 7
127.0.0.1:6379> zlexcount testset [apple [java
(integer) 3

------------------ZREM--ZREMRANGEBYLEX--ZREMRANGBYRANK--ZREMRANGEBYSCORE--------------------------------
127.0.0.1:6379> zrem testset abc		 # 移除成员abc
(integer) 1
127.0.0.1:6379> zremrangebylex testset [apple [java 	# 移除字典区间[apple,java]中的所有成员
(integer) 3
127.0.0.1:6379> zremrangebyrank testset 0 1 	# 移除排名0~1的所有成员
(integer) 2
127.0.0.1:6379> zremrangebyscore myzset 0 3 	# 移除score在 [0,3]的成员
(integer) 2


# 测试有序结合testset=> {abc,add,apple,amaze,back,java,redis} score均为0
# 			myzset=> {(m1,1),(m2,2),(m3,3),(m4,4),(m7,7),(m9,9)}
----------------ZREVRANGE--ZREVRANGEBYSCORE--ZREVRANGEBYLEX-----------
127.0.0.1:6379> zrevrange myzset 0 3 	# 按score递减排序，然后按索引返回结果的 0~3
1) "m9"
2) "m7"
3) "m4"
4) "m3"
127.0.0.1:6379> zrevrange myzset 2 4 	# 返回排序结果，index为2~4的元素
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> zrevrangebyscore myzset 6 2 	# 按score递减顺序 返回集合中分数在[2,6]之间的成员
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> zrevrangebylex testset [java (add 	# 按字典倒序 返回集合中(add,java]字典区间的成员
1) "java"
2) "back"
3) "apple"
4) "amaze"

-------------------------ZREVRANK------------------------------
127.0.0.1:6379> zrevrank myzset m7 	# 按score递减顺序，返回成员m7索引
(integer) 1
127.0.0.1:6379> zrevrank myzset m2
(integer) 4


# mathscore=>{(xm,90),(xh,95),(xg,87)} 小明、小红、小刚的数学成绩
# enscore=>{(xm,70),(xh,93),(xg,90)} 小明、小红、小刚的英语成绩
----------------ZINTERSTORE--ZUNIONSTORE--WITHSCORES-----------------------
127.0.0.1:6379> zinterstore sumscore 2 mathscore enscore 	# 将mathscore enscore进行合并 结果存放到sumscore
(integer) 3
127.0.0.1:6379> zrange sumscore 0 -1 withscores 	# 合并后的score是之前集合中所有score的和，withscores就是结果中带上score的值
1) "xm"
2) "160"
3) "xg"
4) "177"
5) "xh"
6) "188"

127.0.0.1:6379> zunionstore lowestscore 2 mathscore enscore AGGREGATE MIN 	# 取两个集合的成员，score最小值作为结果，赋给一个新集合
(integer) 3
127.0.0.1:6379> zrange lowestscore 0 -1 withscores
1) "xm"
2) "70"
3) "xg"
4) "87"
5) "xh"
6) "93"

127.0.0.1:6379> zunionstore maxscore 2 mathscore enscore AGGREGATE MAX
(integer) 3
127.0.0.1:6379> zrange maxscore 0 -1 withscores
1) "xg"
2) "90"
3) "xm"
4) "90"
5) "xh"
6) "95"
```

应用案例：

- set排序 存储班级成绩表 工资表排序！
- 普通消息，1.重要消息 2.带权重进行判断
- 排行榜应用实现，取Top N测试

## 4. 三种特殊数据类型

### 4.1 Geospatial(地理位置)

| 命令                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `geoadd key longitud(经度) latitude(纬度) member [..]`       | 将具体经纬度的坐标存入一个有序集合                           |
| `geopos key member [member..]`                               | 获取集合中的一个/多个成员坐标                                |
| `geodist key member1 member2 [unit]`                         | 返回两个给定位置之间的距离。默认以米作为单位。               |
| `GEORADIUS key longitude latitude radius m|km|mi|ft [WITHCOORD][WITHDIST] [WITHHASH] [COUNT count]` | 以给定的经纬度为中心， 返回集合包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。 |
| `GEORADIUSBYMEMBER key member radius...`                     | 功能与GEORADIUS相同，只是中心位置不是具体的经纬度，而是使用结合中已有的成员作为中心点。 |
| `geohash key member1 [member2..]`                            | 返回一个或多个位置元素的Geohash表示。使用Geohash位置52点整数编码。 |

**有效经纬度**

> - 有效的经度从-180度到180度。
> - 有效的纬度从-85.05112878度到85.05112878度。

指定单位的参数 **unit** 必须是以下单位的其中一个：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

**关于GEORADIUS的参数**

> 通过`georadius`就可以完成 **附近的人**功能
>
> withcoord: 带上坐标
>
> withdist: 带上距离，单位与半径单位相同
>
> COUNT n: 只显示前n个(按距离递增排序)

```bash
-------------GEOADD--GEOPOS-------------
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing		# 添加beijing的经纬度到集合中
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai 106.50 29.53 chongqin
(integer) 2
127.0.0.1:6379> geopos china:city beijing chongqin		# 获取集合中元素的经纬度
1) 1) "116.39999896287918091"
    2) "39.90000009167092543"
2) 1) "106.49999767541885376"
    2) "29.52999957900659211"

------------------GEODIST------------------
127.0.0.1:6379> geodist china:city beijing shanghai 	# 查看两地之间的直线距离，默认以m为单位，可以在后面指定单位
"1067378.7564"
127.0.0.1:6379> geodist china:city beijing shanghai km
"1067.3788"


-------------------GEORADIUS--GEORADIUSBYMEMBER-----------------------
# 集合中只有beijing shanghai chongqin三个元素
127.0.0.1:6379> georadius china:city 120 30 1300 km withcoord withdist		# 查询经纬度(120,30)坐标1300km半径内的成员，带上坐标和距离
1) 1) "shanghai"
    2) "196.2512"
    3) 1) "121.47000163793563843"
        2) "31.22999903975783553"
2) 1) "beijing"
    2) "1148.7178"
    3) 1) "116.39999896287918091"
        2) "39.90000009167092543"
127.0.0.1:6379> georadius china:city 120 30 1300 km withcoord withdist count 1	# 可以用COUNT参数来指定查询的个数
1) 1) "shanghai"
    2) "196.2512"
    3) 1) "121.47000163793563843"
        2) "31.22999903975783553"
        
127.0.0.1:6379> georadiusbymember china:city beijing 1100 km	# 查询集合中指定元素1100km半径内的周围的其他元素
1) "beijing"
2) "shanghai"


-----------------GEOHASH-----------------------
127.0.0.1:6379> geohash china:city beijing shanghai		# 获取成员经纬坐标用geohash表示，将二维的经纬度转为一维的字符串
1) "wx4fbxxfke0"
2) "wtw3sj5zbj0"
```

> GEO的底层就是用Zset实现的，所以可以使用Zset的所有命令

```bash
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqin"
2) "shanghai"
3) "beijing"
127.0.0.1:6379> zrem china:city chongqin
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "shanghai"
2) "beijing"
```

### 4.2 HyperLogLog(基数统计)

- Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的

- 花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数

- 因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素

- 其底层使用string数据类型

**什么是基数？**

- 数据集中不重复的元素的个数

**应用场景**：

- 网页的访问量UV(Unique Visitor)：一个用户多次访问，也只能算作一个人

传统实现：存储用户的id，然后每次进行比较。当用户变多之后这种方式及其浪费空间，而我们的目的只是**计数**，Hyperloglog就能帮助我们利用最小的空间完成。

| 命令                                      | 描述                                      |
| ----------------------------------------- | ----------------------------------------- |
| `PFADD key element1 [elememt2..]`         | 添加指定元素到 HyperLogLog 中             |
| `PFCOUNT key [key]`                       | 返回给定 HyperLogLog 的基数估算值。       |
| `PFMERGE destkey sourcekey [sourcekey..]` | 将多个 HyperLogLog 合并为一个 HyperLogLog |

```bash
----------PFADD--PFCOUNT---------------------
127.0.0.1:6379> pfadd myelemx a b c d e f g h i j k 	# 添加元素
(integer) 1
127.0.0.1:6379> type myelemx 	# hyperloglog底层使用String
string
127.0.0.1:6379> pfcount myelemx 	# 估算myelemx的基数，会去重
(integer) 11
127.0.0.1:6379> pfadd myelemy i j k z m c b v p q s
(integer) 1
127.0.0.1:6379> pfcount myelemy
(integer) 11

----------------PFMERGE-----------------------
127.0.0.1:6379> pfmerge myelemz myelemx myelemy 		# 合并myelemx和myelemy 成为myelemz
OK
127.0.0.1:6379> pfcount myelemz		 # 估算基数，会去重
(integer) 17
```

- 如果允许容错，那么一定可以使用Hyperloglog 

- 如果不允许容错，就使用set或者自己的数据类型即可 

### 4.3 BitMaps(位图)

- 使用位存储，信息状态只有 0 和 1

- Bitmap是一串连续的2进制数字（0或1），每一位所在的位置为偏移(offset)，在bitmap上可执行AND,OR,XOR,NOT以及其它位操作。

| 命令                                  | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| `setbit key offset value`             | 为指定key的offset位设置值                                    |
| `getbit key offset`                   | 获取offset位的值                                             |
| `bitcount key [start end]`            | 统计字符串被设置为1的bit数，也可以指定统计范围按字节         |
| `bitop operration destkey key[key..]` | 对一个或多个保存二进制位的字符串 key 进行位元操作，并将结果保存到 destkey 上。 |
| `BITPOS key bit [start] [end]`        | 返回字符串里面第一个被设置为1或者0的bit位。start和end只能按字节,不能按位 |

```bash
------------setbit--getbit--------------
127.0.0.1:6379> setbit sign 0 1 		# 设置sign的第0位为 1 
(integer) 0
127.0.0.1:6379> setbit sign 2 1 		# 设置sign的第2位为 1  不设置默认 是0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 5 1
(integer) 0
127.0.0.1:6379> type sign   		# bitmaps底层是string
string

127.0.0.1:6379> getbit sign 2 	# 获取第2位的数值
(integer) 1
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 4 		# 未设置默认是0
(integer) 0

-----------bitcount----------------------------
127.0.0.1:6379> BITCOUNT sign # 统计sign中为1的位数
(integer) 4
```

**应用场景**：

- 签到统计、状态统计、打卡记录





**bitmaps的底层**



这样设置以后你能get到的值是：**\xA2\x80**，所以bitmaps是一串从左到右的二进制串

## 5. 事务

Redis的**单条命令**是**保证原子性**的，但是Redis**事务不能保证原子性**

> Redis事务本质：一组命令的集合。
>
> ----------------- 队列 set set set 执行 -------------------
>
> 事务中每条命令都会被序列化，执行过程中按顺序执行，不允许其他命令进行干扰。
>
> - 一次性
> - 顺序性
> - 排他性
>
> ------
>
> 1. Redis事务没有隔离级别的概念
>    - 所有命令在事务中，并没有被直接执行；只有发起执行命令的时候才会按顺序执行
> 2. Redis单条命令是保证原子性的，但是事务不保证原子性



### 5.1 Redis事务操作过程

- 开启事务（`multi`）
- 命令入队（常用命令）
- 执行事务（`exec`）

所以事务中的命令在加入时都没有被执行，直到提交时才会开始执行(Exec)一次性完成

```bash
127.0.0.1:6379> multi		 # 开启事务
OK
127.0.0.1:6379> set k1 v1 		# 命令入队
QUEUED
127.0.0.1:6379> set k2 v2 	# ..
QUEUED
127.0.0.1:6379> get k1
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> keys *
QUEUED
127.0.0.1:6379> exec		 # 事务执行，将队列里的命令依次执行
1) OK
2) OK
3) "v1"
4) OK
5) 1) "k3"
    2) "k2"
    3) "k1"
```

**取消事务(`discurd`)**

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> DISCARD 	# 放弃事务
OK
127.0.0.1:6379> EXEC 
(error) ERR EXEC without MULTI 		# 当前未开启事务
127.0.0.1:6379> get k1 		# 被放弃事务中命令并未执行
(nil)
```

### 5.2 事务错误

> 代码语法错误（编译时异常）所有的命令都不执行

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> error k1 	# 这是一条语法错误命令
(error) ERR unknown command `error`, with args beginning with: `k1`, 	# 会报错但是不影响后续命令入队 
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors. 	# 执行报错
127.0.0.1:6379> get k1 
(nil) 		# 其他命令并没有被执行，k1不存在
```

> 代码逻辑错误 (运行时异常) **其他命令可以正常执行 ** >> 所以不保证事务原子性

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> incr k1 		# 这条命令逻辑错误（对字符串进行增量）
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) (error) ERR value is not an integer or out of range 		# 运行时报错
4) "v2" 		# 其他命令正常执行

# 虽然中间有一条命令报错了，但是后面的指令依旧正常执行成功了
# 所以说Redis单条指令保证原子性，但是Redis事务不能保证原子性
```

### 5.3 监控

**悲观锁：**

- 很悲观，认为什么时候都会出现问题，无论做什么都会加锁

**乐观锁：**

- 很乐观，认为什么时候都不会出现问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人修改过这个数据
- 获取version
- 更新的时候比较version

Redis使用`watch key`监控指定数据，相当于乐观锁加锁

`unwatch`解锁

> 正常执行

```bash
127.0.0.1:6379> set money 100 		# 设置余额: 100
OK
127.0.0.1:6379> set use 0 		# 支出使用: 0
OK
127.0.0.1:6379> watch money 	# 监视money (上锁)
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY use 20
QUEUED
127.0.0.1:6379> exec 		# 监视值没有被中途修改，事务正常执行
1) (integer) 80
2) (integer) 20
```

> 测试多线程修改值，使用watch可以当做redis的乐观锁操作（相当于getversion）

我们启动另外一个客户端模拟插队线程。

线程1：

```bash
127.0.0.1:6379> set money 100 		# 设置余额: 100
OK
127.0.0.1:6379> set use 0 		# 支出使用: 0
OK
127.0.0.1:6379> watch money 	# money上锁
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY use 20
QUEUED
127.0.0.1:6379> 	# 此时事务并没有执行
```

模拟线程插队，线程2：

```bash
127.0.0.1:6379> INCRBY money 500 	# 修改了线程一中监视的money
(integer) 600
```

回到线程1，执行事务：

```bash
127.0.0.1:6379> EXEC	 # 执行之前，另一个线程修改了我们的值，这个时候就会导致事务执行失败
(nil) 		# 没有结果，说明事务执行失败

127.0.0.1:6379> get money 		# 线程2 修改生效
"600"
127.0.0.1:6379> get use 		# 线程1事务执行失败，数值没有被修改
"0"
```

如果执行失败，可以先解锁`unwatch`获取最新的值，然后再次进行监视`watch`，重新开始事务

注意：每次提交执行exec后都会自动释放锁，不管是否成功

## 6. Jedis

- Jedis是Redis官方推荐使用的Java连接Redis的客户端

使用步骤：

1. 新建一个空项目，添加一个Maven项目，导入依赖

   ```xml
   <!--导入jredis的包-->
   <dependency>
       <groupId>redis.clients</groupId>
       <artifactId>jedis</artifactId>
       <version>3.2.0</version>
   </dependency>
   <!--fastjson-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>fastjson</artifactId>
       <version>1.2.73</version>
   </dependency>
   ```
   
2. 连接Redis

   1. 修改redis的配置文件

      ```bash
      vim /usr/local/bin/r/redis.conf
      ```
      
   1. 将只绑定本地`bind 127.0.0.1`注释掉
      
   2. 保护模式`protected-mode`设置为 no
      
      ![image-20220407170131122](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c372bc35a971f4a38b0250463bd2d863.png)
      
   3. redis后台启动`daemonize`设置为yes
   
3. 开放端口6379

   ```bash
   firewall-cmd --zone=public --add-port=6379/tcp --permanent
   ```
   

![image-20220407172356551](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/44a9ec79bf9c6fa4e36a58c8a08005b3.png)

若开放失败先查看防火墙是否开启：

   1. 开启防火墙：`systemctl start firewalld`
   
      关闭防火墙：`systemctl stop firewalld`

      查看防火墙状态：`systemctl status firewalld`

4. 重启防火墙服务

   ```bash
   systemctl restart firewalld.service
   ```

5. 可以查看一下开启的端口：`firewall-cmd --list-ports`

   ![image-20220407172715533](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2ff42b6bcf45736d1db910eec86290c3.png)

6. 云服务器在控制台配置Redis端口的安全组或防火墙

7. 重启redis-server

   ```bash
   [root@VM-16-12-centos bin]# redis-server run-config/redis.conf
   ```

8. 测试连接：`TestPing.java`

   ```java
   public class TestPing {
       public static void main(String[] args) {
           Jedis jedis = new Jedis("152.136.20.129",6379);		//服务器ip地址和redis端口号
           String response = jedis.ping();
           System.out.println(response);		//输出PONG
       }
   }
   ```

若报错，则可能需要先设置一下Redis的密码

- `vim run-config/redis.conf`将里面的`requirepass`设置一个值，取消注释

  ![image-20220407212342880](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c9e4b43ca92cdb6360f0302be51b4ff6.png)

- 此时在测试中添加Redis的密码`jedis.auth("123456")`

> 事务

```java
public class TestTransaction {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("152.136.20.129", 6379);
        jedis.auth("123456");
        
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("name", "AruNi");
        jsonObject.put("age", 18);
        // 开启事务
        Transaction multi = jedis.multi();
        String info = jsonObject.toJSONString();
        //jedis.watch(result);      // 开启乐观锁
        try {
            multi.set("user", info);
            // 执行事务
            multi.exec();
        } catch (Exception e) {
            // 放弃事务
            multi.discard();
        } finally {
            System.out.println(jedis.get("user"));
            // 关闭连接
            jedis.close();
        }
    }
}
```

输出：

```text
{"name":"AruNi","age":18}

Process finished with exit code 0
```



## 7. SpringBoot整合

新建SpringBoot项目，勾选相关工具

![image-20220407220613111](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/982a0ff474b10045e5f9967a7bb0738a.png)

SpringBoot 2.x后，原来使用的 Jedis 被 lettuce 替换

- Jedis：采用的**直连**，多个线程操作的话，是不安全的。如果要避免不安全，使用jedis pool连接池！更像BIO(阻塞)模式
- lettuce：采用netty，实例可以在**多个线程中共享**，不存在线程不安全的情况！可以减少线程数据了，更像NIO(New IO)模式



> 源码浅析

我们在学习SpringBoot自动配置的原理时，整合一个组件并进行配置一定会有一个自动配置类`xxxAutoConfiguration`, 并且在`spring.factories`中也一定能找到这个类的完全限定名。Redis也不例外。

![image-20220407221205347](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/407dcaa4dbdf331f00c80964bc6b4468.png)

那么就一定还存在一个`RedisProperties`类

![image-20220407221402362](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a89529391865369e6e57f66c5789580e.png)

之前我们说SpringBoot2.x后默认使用Lettuce来替换Jedis，现在我们就能来验证了。

先看Jedis：

![image-20220407222304786](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/58276864d54e24a8db773f76bbe3058f.png)

`@ConditionalOnClass`注解中有两个类是默认不存在的，所以Jedis是无法生效的



然后再看Lettuce：

![image-20220407222714051](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8ae76673f609afb940595cd11aafee52.png)



现在我们回到`RedisAutoConfiguratio`：

只有两个简单的Bean

- **RedisTemplate**
- **StringRedisTemplate**

当看到xxTemplate时可以对比RestTemplat、SqlSessionTemplate,通过使用这些Template来间接操作组件。那么这俩也不会例外。分别用于操作Redis和Redis中的String数据类型。

在RedisTemplate上也有一个条件注解`@ConditionalOnMissingBean`，说明我们是可以对其进行定制化的

```java
public class RedisAutoConfiguration {

	@Bean
	@ConditionalOnMissingBean(name = "redisTemplate")	// 在缺失bean的时候使用官方这个默认的，所以可以自定义Template添加上Bean注解来替换这个
	@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
	public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        	// 默认的RedisTemplate没有过多的设置，redis对象都是需要序列化的
        	// 两个泛型都是Object类型，后面使用需要强制转换<String, Object>
		RedisTemplate<Object, Object> template = new RedisTemplate<>();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}

	@Bean
	@ConditionalOnMissingBean
	@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
        // 由于String是redis中最常使用的类型，所以单独提出来了一个Bean
	public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
		return new StringRedisTemplate(redisConnectionFactory);
	}

}
```

说完这些，我们需要知道如何编写配置文件然后连接Redis，就需要阅读`RedisProperties`：

![image-20220407223914763](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0d273e44aadbda9a120945fdef04eaf0.png)

这是一些基本的配置属性：

![image-20220407224008458](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/975e02a27dda1daa261f54c8722c007c.png)

还有一些连接池相关的配置。注意使用时一定使用Lettuce的连接池。

![image-20220407224103052](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ff6e89816b5f3d50fe75401783413825.png)

分析完源码，来配置一下SpringDataRedis！

> 配置SpringDataRedis

1. 编写配置文件

   ```properties
   # 配置redis
   spring.redis.host=152.136.20.129
   spring.redis.port=6379
   spring.redis.password=123456
   ```
   
2. 在测试类中使用RedisTemplate

   ```java
   @SpringBootTest
   class Redis02SpringbootApplicationTests {
   
       @Autowired
       private RedisTemplate redisTemplate;
   
       @Test
       void contextLoads() {
           // redisTemplate 操作不同的数据类型，api和我们的指令是一样的
           // opsForValue 操作字符串 类似String
           // opsForList 操作List 类似List
           // opsForHah
   
           // 除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务和基本的CRUD
   
           // 获取连接对象
           // RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
           // connection.flushDb();
           // connection.flushAll();
   
           redisTemplate.opsForValue().set("name", "AruNi");
           System.out.println(redisTemplate.opsForValue().get("name"));    //AruNi
       }
   
   }
   ```
   
3. 测试结果

   **此时我们回到Redis查看数据时候，惊奇的发现key全是乱码，可是程序中可以正常输出**！user是上面Jedis测试时候插入的元素，第一个元素是刚刚插入的name，发现已经乱码了。

   ![image-20220407225112055](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fa8537cae5dd1b88f0a69123bb315edc.png)

   

   这时候就关系到存储对象的**序列化**问题，在网络中传输的对象也是一样需要序列化，否者就全是乱码。

   我们转到看那个默认的RedisTemplate内部什么样子：

   ![image-20220408102323297](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7ab71b3004e421cd131f3f641f25fadd.png)

   在最开始就能看到几个关于序列化的参数。

   默认的序列化器是采用JDK序列化器

   ![image-20220408102423429](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9fbc08337458ae237864c1fed9a21888.png)

   而默认的RedisTemplate中的所有序列化器都是使用这个序列化器：

   ![image-20220408102525263](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3e439ec18fedc460071bd2bbbc85a6fb.png)

   后续我们定制RedisTemplate就可以对其进行修改。

   `RedisSerializer`提供了多种序列化方案：

   - 直接调用RedisSerializer的静态方法来返回序列化器，然后set

     例如：`template.setKeySerializer(RedisSerializer.string());`  key采用String序列化方式

     ![image-20220408102616265](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2a8146490f2b87491f059adbc59ea06c.png)

   - 自己new相应的序列化对象，然后set想要的序列化类型，可以设置的参数如下：

     我们下面的自定义RedisTemplate就采用这种方式

     ![image-20220408111859225](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7f11788e99997ef7e2333214231189e6.png)

> 自定义RedisTemplate

我们创建一个Bean加入容器，就会触发RedisTemplate上的条件注解使默认的RedisTemplate失效。

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        // 将template 泛型设置为 <String, Object>，为了开发方便
        RedisTemplate<String, Object> template = new RedisTemplate();
        // 连接工厂，不必修改
        template.setConnectionFactory(redisConnectionFactory);

        // Json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL);
        //String序列化配置
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // 序列化设置
        // key和 hash的key采用String序列化方式

        // 方式一：直接调用RedisSerializer的静态方法来返回序列化器，然后set
        // template.setKeySerializer(RedisSerializer.string());
        // template.setHashKeySerializer(RedisSerializer.string());

        //方式二：自己new相应的序列化对象(如上序列化配置)，然后set想要的序列化类型
        template.setKeySerializer(stringRedisSerializer);
        template.setHashKeySerializer(stringRedisSerializer);

        // value和hash的value采用Jackson序列化方式
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.setHashValueSerializer(jackson2JsonRedisSerializer);

        // 将自己的配置set进去
        template.afterPropertiesSet();
        return template;
    }

}
```

这样一来，只要**实体类进行了序列化**，我们存什么都不会有乱码了。

在使用自己定义的RedisTemplate时，可以采用`@Qualifier`注解指定，防止使用了默认的Template：

```java
@Autowired
@Qualifier("redisTemplate")     //可以指定使用自定义的template，防止使用了默认的
private RedisTemplate redisTemplate;
```

示例：

新建一个实体类User并将其序列化

```java
@Component
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {
    private String name;
    private int age;
}
```

使用自定义的RedisTemplate进行测试：

```java
@SpringBootTest
class Redis02SpringbootApplicationTests {

    @Autowired
    @Qualifier("redisTemplate")     // 可以指定使用自定义的template，防止使用了默认的
    private RedisTemplate redisTemplate;

    @Test
    public void test() throws JsonProcessingException {
        // 真实开发一般都是用Json来传递对象
        User user = new User("AruNi", 17);
        redisTemplate.opsForValue().set("user", user);
    }
}
```

运行后，在Redis中查看，可以发现user没有乱码：

![image-20220408134608957](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/26bbdb013861bf6ff6714bab594af36a.png)





## 8. 自定义Redis工具类

直接用RedisTemplate操作Redis，需要很多行代码，使用`RedisTemplate`需要频繁调用`.opsForxxx`然后才能进行对应的操作；因此直接封装好一个RedisUtils，这样写代码更方便点。这个RedisUtils交给Spring容器实例化，使用时直接注解注入。

工具类参考博客：

https://www.cnblogs.com/zeng1994/p/03303c805731afc9aa9c60dbbd32a323.html

工具类的使用和Redis的命令完全一致，下面给出一个List的示例：

```java
@SpringBootTest
class Redis02SpringbootApplicationTests {

    @Autowired
    private RedisUtil redisUtil;

    @Test
    public void test2() {
        User user1 = new User("AruNi", 17);
        User user2 = new User("kuangshen", 3);
        List<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        redisUtil.lSet("user", list);
        System.out.println(redisUtil.lGet("user", 0, -1));
    }
}
```

运行后在控制台查看：

```text
[[{name=AruNi, age=17}, {name=kuangshen, age=3}]]

Process finished with exit code 0
```

在Redis中查看：

![image-20220408141001378](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d3c3c4bd989c4c4a894e19d1ade23508.png)

> 总结

在开发中，我们一般只需要自己编写一个`RedisConfig`配置类，自定义`RedisTemplate`，然后再导入一个`RedisUitl`的工具类，就可以直接通过工具类在Java中使用Redis。

## 9. Redis.conf

Redis启动的时候，就是通过配置文件来启动的！

> 容量单位unit不区分大小写，X和XB有区别

![image-20220408142611122](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/47f23cf27810c6920b5b36a8b61dd8d0.png)

> 可以使用include组合多个配置文件

![image-20220408142734451](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8facde92ad4dfdd2d3f550fc74e169ed.png)

> 网络配置

![image-20220408142919456](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f0624fad27ce1c494634f369d2a06e35.png)

> 通用配置GENERAL

![image-20220408143251897](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/af03562f9a9ad7db4d98a7144964d8b0.png)

**pid文件扩展**：

- pid文件的作用：防止启动多个进程副本

- pid文件的原理：进程运行后会给`.pid`文件加一个文件锁，只有获得该锁的进程才有写入权限（F_WRLCK），把自身的pid写入该文件中。其他试图获得该锁的进程会自动退出
- 注意事项：
  1. 进程退出，该进程加的锁自动失效
  2. 进程关闭了该文件描述符，加的锁失效
  3. 锁的状态不会被子进程继承，进程关闭则锁失效，不管子进程是否在运行

> 日志输出级别、输出文件

![image-20220408143440880](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5de8b99948a5cdaa9a465f35df05302b.png)

> 快照

由于Redis是基于内存的数据库，需要将数据由内存持久化到文件`.rdb`、`.aof`中

持久化方式：

- RDB
- AOF

![image-20220408144446449](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cda976af90574c16bd1d816204a0c8a9.png)

> RDB文件相关

![image-20220408144745182](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2c284ae9626f8ba02867bb9a37da2555.png)

![image-20220408144822526](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2cb977c1be9a643b310b2ebe2aaac387.png)

> 主从复制

![image-20220408145121598](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8028bbc72a124b88d52978c803e7470b.png)

> Security模块中进行密码设置

![image-20220408145319647](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b3999310e7dc5387f1a0fbc6cd3d626c.png)

> 客户端连接相关

```bash
maxclients 10000  		# 最大客户端数量
maxmemory <bytes> 		# 最大内存限制
maxmemory-policy noeviction 		# 内存达到限制值的处理策略
```

redis 中的**默认**的过期策略是 **volatile-lru**

**设置方式**

```bash
config set maxmemory-policy volatile-lru 
```

#### maxmemory-policy 六种方式

**1、volatile-lru：**只对设置了过期时间的key进行LRU（默认值）

**2、allkeys-lru ：** 删除lru算法的key

**3、volatile-random：**随机删除即将过期key

**4、allkeys-random：**随机删除

**5、volatile-ttl ：** 删除即将过期的

**6、noeviction ：** 永不过期，返回错误

> AOF相关部分

![image-20220408145657261](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6edf7be34738a0b28ed3ebf499b0494e.png)

![image-20220408145905934](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c184e9d0e3e84069ae610475a3f959a4.png)

- always：每次修改都会 sync，消耗性能
- no：不执行 sync，这时操作系统自己同步数据，速度最快

## 10. 持久化RDB

Redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。所以Redis提供了持久化功能！

Redis持久化有不同的级别：

- RDB持久化方式：可以在指定的时间间隔能对数据进行快照存储
- AOF持久化方式：记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以redis协议追加保存每次写的操作到文件末尾。Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大

如果服务器开启了AOF持久化功能。服务器会优先使用AOF文件还原数据。只有关闭了AOF持久化功能，服务器才会使用RDB文件还原数据
![image-20220408152928008](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/733a03b9a688a3e2311f66d98b0f1bc9.png)

### 10.1 什么是RDB

RDB：Redis Databases

在 Redis持久化时，RDB 程序将当前内存中的数据库状态保存到磁盘文件中

在 Redis 重启动时，RDB 程序可以通过载入 RDB 文件来还原数据库的状态

![image-20220408153039398](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/68c94bce556f001590cacf651c9eeeaa.png)

默认情况下， Redis 将数据库快照保存在名字为`dump.rdb`的二进制文件中。文件名可以在配置文件中进行自定义。

### 10.2 工作原理

------

在进行 **`RDB`** 的时候，**`redis`** 的主线程是不会做 **`io`** 操作的，主线程会 **`fork`** 一个子线程来完成该操作：

1. Redis 调用forks。同时拥有父进程和子进程。
2. 子进程将数据集写入到一个临时 RDB 文件中。
3. 当子进程完成对新 RDB 文件的写入时，Redis 用新 RDB 文件替换原来的 RDB 文件，并删除旧的 RDB 文件。

这种工作方式使得 Redis 可以从 写时复制（copy-on-write）机制中获益 (因为是使用子进程进行写操作，而父进程依然可以接收来自客户端的请求)

### 10.3 触发机制

------

1. save的规则满足的情况下，会自动触发rdb原则
2. 执行`flushall`命令，也会触发我们的rdb原则
3. 退出redis，也会自动产生rdb文件

#### 10.3.1 save

使用 `save` 命令，会立刻对当前内存中的数据进行持久化，但是会**阻塞**，也就是不接受其他操作了，直到RDB文件创建完毕为止

> 由于 `save` 命令是同步命令，会占用Redis的主进程。若Redis数据非常多时，`save`命令执行速度会非常慢，阻塞所有客户端的请求。

![image-20220408153142839](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/de24f08cdbd53d74cf65f14c472e73fc.png)

#### 10.3.2 flushall命令

`flushall` 命令也会触发持久化 

#### 10.3.3 bgsave

`bgsave` 是异步进行，进行持久化的时候，`redis` 还可以将继续响应客户端请求 ；

![image-20220408153224544](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d3eb11a05f8b4b9a31c338e6f3108f33.png)

#### 10.3.4 自动触发策略

自动触发策略，是指 Redis 在指定的时间内，数据发生了多少次变化时，会自动执行`BGSAVE`命令。自动触发的条件包含在了 Redis 的配置文件中

![image-20220408152208965](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cda976af90574c16bd1d816204a0c8a9.png)

> bgsave和save对比

| 命令   | save               | bgsave                             |
| ------ | ------------------ | ---------------------------------- |
| IO类型 | 同步               | 异步                               |
| 阻塞   | 是                 | 是（阻塞发生在fock()，通常非常快） |
| 复杂度 | O(n)               | O(n)                               |
| 优点   | 不会消耗额外的内存 | 不阻塞客户端命令                   |
| 缺点   | 阻塞客户端命令     | 需要fock子进程，消耗内存           |

### 10.4 优缺点

**优点**：

1. 适合大规模的数据恢复
2. 对数据的完整性要求不高

**缺点**：

1. 需要一定的时间间隔进行操作，如果redis意外宕机了，这个最后一次修改的数据就没有了
2. fork进程的时候，会占用一定的内容空间

## 11. 持久化AOF

**Append Only File**

将我们所有的命令都记录下来 history，恢复的时候就把这个文件全部再执行一遍

> 以日志的形式来记录每个写的操作，将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

![image-20220408154525442](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a6a22e12683a5f553cadc7c5db767a49.png)

### 11.1 什么是AOF

快照功能（RDB）并不是非常耐久（durable）： 如果 Redis 因为某些原因而造成故障停机， 那么服务器将丢失最近写入、以及未保存到快照中的那些数据。 从 1.1 版本开始， Redis 增加了一种完全耐久的持久化方式： AOF 持久化。

如果要使用AOF，需要修改配置文件：

```bash
appendonly yes  	# 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分的情况下，rdb完全够用
appendfilename "appendonly.aof"
```

默认是不开启的，我们需要手动配置，然后重启redis，就可以生效了！

如果这个aof文件有错误，这时候redis是启动不了的，我需要修改这个aof文件

redis给我们提供了一个工具：`redis-check-aof --fix`，执行之后重启redis即可



### 11.2 持久化策略

AOF持久化策略(即缓冲区内容写入和同步sync到AOF中)，可以通过配置appendfsync属性来选择AOF持久化策略：

- always：将aof_buf缓冲区中的所有内容写入并同步到AOF文件，每次有新命令追加到 AOF 文件时就执行一次 fsync。
- everysec（默认）：如果上次同步AOF的时间距离现在超过一秒，先将aof_buf缓冲区中的所有内容写入到AOF文件，再次对AOF文件进行同步，且同步操作由一个专门线程负责执行。
- no：将aof_buf缓冲区中的所有内容写入到AOF文件，但并不对AOF文件进行同步，何时同步由操作系统(OS)决定。

![image-20220408154821972](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c184e9d0e3e84069ae610475a3f959a4.png)

![image-20220408154842789](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1bef5db779c91a35fa4dd82697de34b3.png)

AOF持久化策略的效率与安全性：

- `always`：效率最慢的，但安全性是最安全的，即使出现故障宕机，持久化也只会丢失一个事件 循环的命令数据
- `everysec`：兼顾速度和安全性，出现宕机也只是丢失一秒钟的命令数据
- `no`：写入最快，但综合起来单次同步是时间是最长的，且出现宕机时会丢失上传同步AOF文件之后的所有命令数据。



### 11.3 AOF重写

由于AOF持久化会把执行的写命令追加到AOF文件中，所以随着时间写入命令会不断增加， AOF文件的体积也会变得越来越大。AOF文件体积大对Reids服务器，甚至宿主服务器造成影响。

为了解决AOF文件体积膨胀的问题，Redis提供了AOF文件重写（rewrite）功能：

- 生成一个不保存任何浪费空间的冗余命令新的AOF文件，且新旧AOF文件保存数据库状态一样的

- 新的AOF文件是通过读取数据库中的键值对来实现的，程序无须对现有的AOF文件进行读入，分析，或者写入操作。

- 为防止缓冲区溢出，重写处理list，hash，set以及Zset时，超过设置常量数量时会多条相同命令记录一个集合。

- 可以通过配置自动触发 AOF 重写，触发参数 `auto-aof-rewrite-percentage`(触发AOF文件执行重写的增长率) 以及 `auto-aof-rewrite-min-size`(触发AOF文件执行重写的最小尺寸)

  ![image-20220408155519837](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cec2444e2f036500d1a4f7616b560127.png)

**AOF重写的作用**：

- 减少磁盘占用量
- 加速数据恢复

Redis服务器使用**单个线程**来处理命令请求，服务器大量调用`aof_rewrite`函数，在AOF重写期间，则无法处理client发来的命令请求，所以**AOF重写程序**放在**子进程**执行，好处：

1. 子进程进行AOF重写期间，服务器进程可以继续处理命令请求
2. 子进程带有服务器进程的数据副本，保证了数据的安全性

AOF重写使用子进程会造成数据库与重写后的AOF保存的数据不一致，为了解决这种数据不一致，redis使用了AOF重写缓冲区实现：

![image-20220408155652215](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ac888ab0fbb95298895697939b1ecba8.png)

BGREWRITEAOF命令实现原理（只有信号处理函数执行时才对服务器进程造成阻塞）：

- 执行命令，同时将命令追加到AOF缓冲区和AOF重写缓冲区
- 当AOF子进程重写完成后，发送一个信号给父进程，父进程将执行AOF重写缓冲区中的所有内容写入到新AOF文件中，新AOF文件保存的数据库状态将和服务器当前的数据库状态一致
- 对新的AOF文件进行改名，原子性地覆盖现有AOF文件，完成新旧两个AOF文件替换处理完成

### 11.4 优缺点

**优点**：

1. 每一次修改都会同步，文件的完整性会更加好
2. 没秒同步一次，可能会丢失一秒的数据
3. 从不同步，效率最高

**缺点**：

1. 相对于数据文件来说，aof远远大于rdb，修复速度比rdb慢
2. aof运行效率也要比rdb慢，所以我们redis默认的配置就是rdb持久化

## 12. 持久化方式抉择

### 12.1 RDB 和 AOF 对比

|            | RDB    | AOF          |
| ---------- | ------ | ------------ |
| 启动优先级 | 低     | 高           |
| 体积       | 小     | 大           |
| 恢复速度   | 快     | 慢           |
| 数据安全性 | 丢数据 | 根据策略决定 |

### 12.2 如何选择持久化方式

一般来说，如果想达到足以媲美 PostgreSQL 的数据安全性，你应该同时使用两种持久化功能。

如果你非常关心你的数据，但仍然可以承受数分钟以内的数据丢失，那么你可以只使用 RDB 持久化。

有很多用户都只使用 AOF 持久化，但并不推荐这种方式：因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份，并且 RDB 恢复数据集的速度也要比 AOF 恢复的速度要快。

## 13. Redis发布与订阅

### 13.1 介绍

Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。

![image-20220408161023036](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b86d26383ccd73036740cdbb88b466f7.png)

下图展示了频道 channel1，以及订阅这个频道的三个客户端 —— client2 、client5 和 client1 之间的关系：

![在这里插入图片描述](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dda8b22f22e9522fb754dfdedf453c65.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![image-20220408161208522](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bdcdf1d55a45fa2d36979c0853331e57.png)

### 13.2 命令

| 命令                                     | 描述                               |
| ---------------------------------------- | ---------------------------------- |
| `PSUBSCRIBE pattern [pattern..]`         | 订阅一个或多个符合给定模式的频道。 |
| `PUNSUBSCRIBE pattern [pattern..]`       | 退订一个或多个符合给定模式的频道。 |
| `PUBSUB subcommand [argument[argument]]` | 查看订阅与发布系统状态。           |
| `PUBLISH channel message`                | 向指定频道发布消息                 |
| `SUBSCRIBE channel [channel..]`          | 订阅给定的一个或多个频道。         |
| `SUBSCRIBE channel [channel..]`          | 退订一个或多个频道                 |

### 13.3 示例

模拟一个订阅端，此时还没有信息发送到此频道：

![image-20220408161738275](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7e70d12d21079ffd9067558474fd5d01.png)

开启另一个客户端，模拟信息发布端，向频道发出信息：

![image-20220408162108242](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d89f2d04c79e490aa539ec19ec33d5d3.png)

此时查看订阅端：

![image-20220408162200478](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/efa720ce03376d1ef50c24c235f01f3e.png)

可以在发布端查看活跃的频道：

![image-20220408162535475](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/39f44ecd8fbadfceaeae0b759f836e2a.png)

### 13.4 原理

每个 Redis 服务器进程都维持着一个表示服务器状态的 redis.h/redisServer 结构， 结构的 pubsub_channels 属性是一个字典， 这个字典就用于保存订阅频道的信息，其中，字典的键为正在被订阅的频道， 而字典的值则是一个链表， 链表中保存了所有订阅这个频道的客户端。

![image-20220408162855722](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e76dc55782efc0f77fa13afe9910d996.png)

客户端订阅，就被链接到对应频道的链表的尾部，退订则就是将客户端节点从链表中移除。

### 13.5 缺点

1. 如果一个客户端订阅了频道，但自己读取消息的速度却不够快的话，那么不断积压的消息会使redis输出缓冲区的体积变得越来越大，这可能使得redis本身的速度变慢，甚至直接崩溃
2. 这和数据传输可靠性有关，如果在订阅方断线，那么他将会丢失所有在短线期间发布者发布的消息

### 13.6 应用

1. 消息订阅：公众号订阅，微博关注等等（其实更多是使用**消息队列**来进行实现）
2. 多人在线聊天室。

稍微复杂的场景，我们就会使用消息中间件MQ(Message Queue)处理

## 14. Redis主从复制

### 14.1 概念

 主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点（Master/Leader），后者称为从节点（Slave/Follower），数据的复制是**单向**的！只能由主节点复制到从节点（**主节点以写为主、从节点以读为主**）。

默认情况下，**每台Redis服务器都是主节点**，一个主节点可以有0个或者多个从节点，但每个从节点只能有一个主节点。

### 14.2 作用

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式
2. 故障恢复：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式
3. 负载均衡：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写的场景下，通过多个从节点分担负载，提高并发量
4. 高可用基石：主从复制还是哨兵和集群能够实施的基础

### 14.3 为什么使用集群

1. 单台服务器难以负载大量的请求
2. 单台服务器故障率高，系统崩坏概率大
3. 单台服务器内存容量有限

### 14.4 环境配置

Redis配置文件中，有一个`replication`模块 (见Redis.conf中第8条)

查看当前库的信息：`info replication`

```bash
127.0.0.1:6379> info replication
# Replication
role:master 		# 角色
connected_slaves:0 		# 从机数量
master_replid:3b54deef5b7b7b7f7dd8acefa23be48879b4fcff
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

既然需要启动多个服务，就需要多个配置文件。每个配置文件对应修改以下信息，因为使用默认的会重复：

- 端口号
- pid文件名
- 日志文件名
- rdb文件名





示例：搭建一个单机多服务

1. 在自己的配置文件中新建多个`redis.conf`，这里创建了3个新的配置文件：

   ![image-20220408164852903](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bff26277ed62e13b9dff33d01f84d7b0.png)

2. 分别对新建的配置文件修改其对应的端口号，pid文件名，日志文件名，rdb文件名

3. 修改完后在不同的客户端启动所有端口的redis服务：

   ![image-20220408171436545](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b54c84f5f389176cdfd5300af469b910.png)

4. 现在到另一个客户端查看redis进程：

   ![image-20220408171509717](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3b4898dba95cc97b459aeca0bbc56ff5.png)

### 14.5 一主二从配置

**默认情况下，每台Redis服务器都是主节点**；我们一般情况下只用配置从机就好了！

1. 首先确认主从：一主（79）二从（80，81）

2. 连接`redis-cli`

3. 使用`SLAVEOF host port`就可以为从机配置主机，主机有密码则需要配置主机的密码，使用`config set masterauth 密码`，下面给出6380从机的示例：

   ![image-20220408172959307](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a79ae414d0f24bd01b1b427bffd1481b.png)

4. 然后主机上也能看到从机的状态：

   ![image-20220408173120793](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8268dd0fc979cc193db7e8d1013339d8.png)

我们这里是使用命令搭建，是暂时的，**真实开发中应该在从机的配置文件中进行配置**，这样的话是永久的。

![image-20220408173533376](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3b0690c27502a66ea83343688dd0d131.png)

### 14.6 使用规则

1. 从机只能读，不能写，主机可读可写但是多用于写

   ```bash
   127.0.0.1:6381> set name sakura 	# 从机6381写入失败
   (error) READONLY You can't write against a read only replica.
   
   127.0.0.1:6380> set name sakura 	# 从机6380写入失败
   (error) READONLY You can't write against a read only replica.
   
   127.0.0.1:6379> set name sakura
   OK
   127.0.0.1:6379> get name
   "sakura"
   ```
   
2. 当**主机断电宕机**后，默认情况下**从机的角色不会发生变化** ，集群中只是失去了写操作，当主机恢复以后，又会连接上从机恢复原状

3. 当**从机断电宕机**后，若**不是使用配置文件配置的从机(临时的)**，再次启动后作为主机是无法获取之前主机的数据的，若此时重新配置成为从机，又可以获取到主机的所有数据。这里就涉及同步原理

   - 若主机在宕机的时候，另一个从机通过手动命令`slaveof on one`让自己变成主机，那其他的结点也可以通过手动命令连接到此新的主机。
     - 若此时原来的主机修复了，它也没有从机，需要从机重新配置，认他做主机

4. 若这台从机是另一台的主机，也可以完成主从复制；不过对于这台从机而言，他还是从节点。

第二条中提到，默认情况下，主机故障后，不会出现新的主机，有两种方式可以产生新的主机：

- 从机手动执行命令`slaveof no one`，这样执行以后从机会独立出来成为一个主机
- 使用哨兵模式（自动选举）



### 14.7 主从复制原理

从总体上来说，Redis主从复制的策略就是：当主从服务器刚建立连接的时候，进行全量同步；全量复制结束后，进行增量复制。当然，如果有需要，slave 在任何时候都可以发起全量同步。

#### 14.7.1 全量复制

Redis全量复制一般发生在Slave初始化阶段，这时Slave需要将Master上的所有数据都复制一份，具体步骤如下：

1. slave服务器连接到master服务器，便开始进行数据同步，发送psync命令（Redis2.8之前是sync命令）

2. master服务器收到psync命令之后，开始执行bgsave命令生成RDB快照文件并使用缓存区记录此后执行的所有写命令

   > - 如果master收到了多个slave并发连接请求，它只会进行一次持久化，而不是每个连接都执行一次，然后再把这一份持久化的数据发送给多个并发连接的slave
   > - 如果RDB复制时间超过60秒（repl-timeout），那么slave服务器就会认为复制失败，可以适当调节大这个参数

3. master服务器bgsave执行完之后，就会向所有Slava服务器发送快照文件，并在发送期间继续在缓冲区内记录被执行的写命令

   > `client-output-buffer-limit slave 256MB 64MB 60`，如果在复制期间，内存缓冲区持续消耗超过64MB，或者一次性超过256MB，那么停止复制，复制失败

4. slave服务器收到RDB快照文件后，会将接收到的数据写入磁盘，然后清空所有旧数据，在从本地磁盘载入收到的快照到内存中，同时基于旧的数据版本对外提供服务

5. master服务器发送完RDB快照文件之后，便开始向slave服务器发送缓冲区中的写命令

6. slave服务器完成对快照的载入，开始接收命令请求，并执行来自主服务器缓冲区的写命令

7. 如果slave node开启了AOF，那么会立即执行BGREWRITEAOF，重写AOF

![image-20220408175344464](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8e96bcb6b98b6196b2fcb0b7a68c78df.png)



#### 14.7.2 增量复制

Redis的增量复制是指在初始化的全量复制并开始正常工作之后，master服务器将发生的写操作同步到slave服务器的过程，增量复制的过程主要是master服务器每执行一个写命令就会向slave服务器发送相同的写命令，slave服务器接收并执行收到的写命令。

## 15. 哨兵模式

更多信息参考博客：https://www.jianshu.com/p/06ab9daf921d

**主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可**。这不是一种推荐的方式，更多时候，我们优先考虑**哨兵模式**。

### 15.1 概述

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例**。

单哨兵模式：

![image-20220409102321051](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7070c061b6ffdaf82144523cbd489db3.png)

这里的哨兵有两个作用

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过**发布订阅模式**通知其他的从服务器，修改配置文件，让它们切换主机。

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式。

多哨兵模式：

![image-20220409102444378](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/19c5a99f06a7d1440648dfe4831dafe0.png)

**故障切换(failover)**的过程：假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为**客观下线**。这样对于客户端而言，一切都是透明的。

哨兵的核心配置：`sentinel monitor 被监控的名称 host port 1`

数字1表示 ：只有1个或1个以上的哨兵认为主服务器不可用的时候，才会进行failover操作



### 15.2 测试

目前的状态是一主二从

1. 添加哨兵配置文件`sentinel.conf`，在配置文件中配置哨兵和对应的密码

   ![image-20220409110740356](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f2963705a83e844aeedf0b52d60b3b9d.png)

2. 启动哨兵模式`redis-sentinel run-config/sentinel.conf `：

   ![image-20220409110941813](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b9c42596f4bb281c5040aa7926f86a00.png)

3. 此时哨兵监视着我们的主机6379，当我们断开主机后，就会选举出新的主机(通过一个算法)：

   ![image-20220409111855308](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4c895dd3c9f4a83db2d54add8d7dbee4.png)

4. 此时查看6381，已经成为主机了，处于连接中的6380属于它的从机：

   ![image-20220409111954453](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5d253c24c00e6a237475f446b6dd5331.png)

5. 此时重新连接6379，发现已经成为了6381的从机：

   ![image-20220409112538760](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cb340e182b2363d5ec95b046866e280d.png)

   

### 15.3 优缺点

**优点：**

1. 哨兵集群，基于主从复制模式，所有主从复制的优点，它都有
2. 主从可以切换，故障可以转移，系统的可用性更好
3. 哨兵模式是主从模式的升级，手动到自动，更加健壮

**缺点：**

1. Redis不好在线扩容，集群容量一旦达到上限，在线扩容就十分麻烦
2. 实现哨兵模式的配置其实是很麻烦的，里面有很多配置项



### 15.4 全部配置

完整的哨兵模式配置文件 `sentinel.conf`，可以在redis的文件目录查看：

![image-20220409113922747](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/447134f607e72595bed780b4d231c61b.png)

```bash
# Example sentinel.conf
 
# 哨兵sentinel实例运行的端口 默认26379
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster 123456
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
# 配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
# 对于脚本的运行结果有以下规则：
# 若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
# 若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
# 如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
# 一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
# 通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
# 这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
# 一个是事件的类型，
# 一个是事件的描述。
# 如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
# 通知脚本
# sentinel notification-script <master-name> <script-path>
sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh
```

## 16. 缓存穿透/击穿和雪崩

Redis缓存的使用,极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。 其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。

### 16.1 缓存穿透（查不到）

#### 16.1.1 概念

意味着有特殊请求在查询一个不存在的数据，**即不数据存在 Redis 也不存在于数据库。**

导致每次请求都会**穿透**到数据库，若黑客利用此漏洞进行攻击可能压垮数据库。

![image-20220409115221006](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/77ec001294c43e9b1be7fcb46cfbdb01.png)

#### 16.1.2 解决方案

**布隆过滤器**：

对所有可能查询的参数以Hash的形式存储，以便快速确定是否存在这个值，在控制层先进行拦截校验，校验不通过直接打回，减轻了存储系统的压力。

BloomFilter 要缓存全量的 key，这就要求全量的 key 数量不大，10 亿 条数据以内最佳，因为 10 亿 条数据大概要占用 1.2GB 的内存。

![image-20220409120621814](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a906901a09fbcdff2f9dc042c404e83b.png)

> 布隆过滤器原理

BloomFilter 的算法是，首先分配一块内存空间做 bit 数组，数组的 bit 位初始值全部设为 0。

加入元素时，采用 k 个相互独立的 Hash 函数计算，然后将元素 Hash 映射的 K 个位置全部设置为 1。

检测 key 是否存在，仍然用这 k 个 Hash 函数计算出 k 个位置，如果位置全部为 1，则表明 key 存在，否则不存在。

如下图所示：

![image-20220409120208659](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/980e9b393693bcc5b34452ce9273e2ec.png)

**哈希函数会出现碰撞，所以布隆过滤器会存在误判**。

这里的误判率是指，BloomFilter 判断某个 key 存在，但它实际不存在的概率，因为它存的是 key 的 Hash 值，而非 key 的值。

所以有概率存在这样的 key，它们内容不同，但多次 Hash 后的 Hash 值都相同。

**对于 BloomFilter 判断不存在的 key ，则是 100% 不存在的，反证法，如果这个 key 存在，那它每次 Hash 后对应的 Hash 值位置肯定是 1，而不会是 0。布隆过滤器判断存在不一定真的存在**。





**缓存空对象**：

一次请求若在缓存和数据库中都没找到，就在缓存中放置一个空对象，设置一个缺省值（比如：None）。当后续再次进行查询则直接返回空值或者缺省值。

![image-20220409115906957](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/47ad32eb7edc43f4b60ca1fc1291c380.png)

缺陷：

- 存储空对象也需要空间，大量的空对象会耗费一定的空间，存储效率并不高。解决这个缺陷的方式就是设置较短过期时间

- 即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响

### 16.2 缓存击穿（量太大，缓存过期）

#### 16.2.1 概念

**高并发流量，访问的这个数据是热点数据，请求的数据在 DB 中存在，但是 Redis 存的那一份已经过期，后端需要从 DB 从加载数据并写到 Redis**。

缓存被击穿只是针对其中**某个key的缓存不可用而导致击穿**，但是其他的key依然可以使用缓存响应。

关键字：**单一热点数据、高并发、数据失效**

但是由于高并发，可能会把 DB 压垮，导致服务不可用。如下图所示：

![image-20220409121236298](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d31886fa8bac61cd0ce48e8b5c719350.png)



#### 16.2.2 解决方案

> 过期时间 + 随机值

对于热点数据，我们不设置过期时间，这样就可以把请求都放在缓存中处理，充分把 Redis 高吞吐量性能利用起来。

或者过期时间再加一个随机值。

设计缓存的过期时间时，使用公式：过期时间=baes 时间+随机时间。

即相同业务数据写缓存时，在基础过期时间之上，再加一个随机的过期时间，让数据在未来一段时间内慢慢过期，避免瞬时全部过期，对 DB 造成过大压力

> 预热

预先把热门数据提前存入 Redis 中，并设热门数据的过期时间超大值。

> 使用锁

当发现缓存失效的时候，不是立即从数据库加载数据。

而是先获取分布式锁，获取锁成功才执行数据库查询和写数据到缓存的操作，获取锁失败，则说明当前有线程在执行数据库查询操作，当前线程睡眠一段时间在重试。

这样只让一个请求去数据库读取数据。

伪代码如下：

```java
public Object getData(String id) {
    String desc = redis.get(id);
        // 缓存为空，过期了
        if (desc == null) {
            // 互斥锁，只有一个请求可以成功
            if (redis(lockName)) {
                try 
                    // 从数据库取出数据
                    desc = getFromDB(id);
                    // 写到 Redis
                    redis.set(id, desc, 60 * 60 * 24);
                } catch (Exception ex) {
                    LogHelper.error(ex);
                } finally {
                    // 确保最后删除，释放锁
                    redis.del(lockName);
                    return desc;
                }
            } else {
                // 否则睡眠200ms，接着获取锁
                Thread.sleep(200);
                return getData(id);
            }
        }
}
```

### 16.3 缓存雪崩

#### 16.3.1 概念

缓存雪崩指的是**大量的请求无法在 Redis 缓存系统中处理，请求全部打到数据库，导致数据库压力激增**，甚至宕机。

出现该原因主要有两种：

- 大量热点数据同时过期，导致大量请求需要查询数据库并写到缓存
- Redis 故障宕机，缓存系统异常

**缓存雪崩是发生在大量数据同时失效的场景，而缓存击穿（失效）是在某个热点数据失效的场景，这是他们最大的区别**。

> 缓存大量数据同时过期

数据保存在缓存系统并设置了过期时间，但是由于在同时一刻，大量数据同时过期。

系统就把请求全部打到数据库获取数据，并发量大的话就会导致数据库压力激增。

![image-20220409122051714](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f633de63dc10f9c1cd0f0ff50b297b40.png)

#### 16.3.2 解决方案

> 缓存过期

- **过期时间添加随机值**：

  要避免给大量的数据设置一样的过期时间，过期时间 = baes 时间+ 随机时间（较小的随机数，比如随机增加 1~5 分钟）。

  这样一来，就不会导致同一时刻热点数据全部失效，同时过期时间差别也不会太大，既保证了相近时间失效，又能满足业务需求。

- **接口限流**：

  当访问的不是核心数据的时候，在查询的方法上加上**接口限流保护**。比如设置 10000 req/s。

  如果访问的是核心数据接口，缓存不存在允许从数据库中查询并设置到缓存中。

  这样的话，只有部分请求会发送到数据库，减少了压力。

  限流，就是指在**业务系统的请求入口前端控制每秒进入系统的请求数，避免过多的请求被发送到数据库**，如下图：

  ![image-20220409122552305](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9891e181c1dc5ae7d8c13d33f2c8af8f.png)

> Redis 故障宕机

一个 Redis 实例能支撑 10 万的 QPS，而一个数据库实例只有 1000 QPS。

一旦 Redis 宕机，会导致大量请求打到数据库，从而发生缓存雪崩。



对于缓存系统故障导致的缓存雪崩的解决方案有两种：

- **服务熔断和接口限流**：

  在业务系统中，**针对高并发的使用服务熔断来有损提供服务从而保证系统的可用性。**

  **服务熔断就是当从缓存获取数据发现异常，则直接返回错误数据给前端，防止所有流量打到数据库导致宕机**。

  **服务熔断和限流属于在发生了缓存雪崩，如何降低雪崩对数据库造成的影响的方案**。

- **构建高可用缓存集群系统**：

  缓存系统一定要构建一套 Redis 高可用集群，比如 Redis 哨兵集群或者 Redis Cluster 集群，如果 Redis 的主节点故障宕机了，从节点还可以切换成为主节点，继续提供缓存服务，避免了由于缓存实例宕机而导致的缓存雪崩问题。

### 16.4 总结

- **缓存穿透**：数据库本就没有这个数据，请求直奔数据库，缓存系统形同虚设
- **缓存击穿（失效）**：数据库有数据，缓存本应该也有数据，但是缓存过期了，Redis 这层流量防护屏障被击穿了，请求直奔数据库
- **缓存雪崩**：**大量**的热点数据无法在 Redis 缓存中处理（大面积热点数据缓存失效、Redis 宕机），流量全部打到数据库，导致数据库极大压力

