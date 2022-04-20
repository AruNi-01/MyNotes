# 1. ElasticSearch

**Elaticsearch**，简称为**ES**，**ES**是一个开源的**高扩展的分布式全文检索引擎**，它可以近乎**实时的存储、检索数据**；本身扩展性很好，可以扩展到上百台服务器，处理 PB 级别（[大数据](https://so.csdn.net/so/search?q=大数据&spm=1001.2101.3001.7020)时代）的数据。**ES**由 Java 语言开发并使用 **Lucene** 作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的 RESTFULL [API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020) 来隐藏 **Lucene** 的复杂性，从而让全文搜索变得简单。据国际权威的数据库产品评测机构 DB Engines 的统计，在2016 年1月，**ElasticSearch** 已超过 **Solr** 等，成为排名第一的搜索引擎类应用。

## 1.1 介绍

**聊聊Doug Cutting**

1998年9月4日,Google公司在美国硅谷成立,正如大家所知,它是一家做搜索引擎起家的公司

无独有偶，一位名叫Doug Cutting的美国工程师，也迷上了搜索引擎。他做了一个用于文本搜索的函数库（姑且理解为软件的功能组件），命名为[Lucene](https://so.csdn.net/so/search?q=Lucene&spm=1001.2101.3001.7020)。

![image-20210201212452334](https://img-blog.csdnimg.cn/img_convert/b2c0877691440e35dd4f89f08b2074f0.png)

Lucene是用JAVA写成的，目标是为各种中小型应用软件加入[全文检索](https://so.csdn.net/so/search?q=全文检索&spm=1001.2101.3001.7020)功能。因为好用而且开源(代码公开），非常受程序员们的欢迎。

早期的时候，这个项目被发布在Doug Cutting的个人网站和SourceForge (一个开源软件网站)。后来，2001年底，Lucene成为Apache软件基金会jakarta项目的一个子项目。

2004年，Doug Cutting再接再励，在Lucene的基础上，和Apache开源伙伴Mike Cafarella合作，开发了一款可以代替当时的主流搜索的开源搜索引擎，命名为Nutch。

…

> 回到主题

Lucene 是一套信息检索工具包 jar包 不包含搜索引擎系统

包含的: 索引结构 读写索引的工具 排序,搜索规则…工具类

**Lucene和ElasticSearch关系**

ElasticSearch是基于Lucene做了一些封装和增强(上手简单)

# 2. ES和Solr的差别

## 2.1 ElasticSearch简介

Elasticsearch是一个实时[分布式](https://so.csdn.net/so/search?q=分布式&spm=1001.2101.3001.7020)搜索和分析引擎。它让你以前所未有的速度处理大数据成为可能。它用于全文搜索、结构化搜索、分析以及将这三者混合使用︰

维基百科使用Elasticsearch提供全文搜索并高亮关键字，以及输入实时搜索(search-asyou-type)和搜索纠错(did-you-mean)等搜索建议功能。

英国卫报使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以实时的反馈，以便及时了解公众对新发表的文章的回应。

StackOverflow结合全文搜索与地理位置查询，以及more-like-this功能来找到相关的问题和答案。Github使用Elasticsearch检索1300亿行的代码。

但是Elasticsearch不仅用于大型企业，它还让像DataDog以及Klout这样的创业公司将最初的想法变成可扩展的解决方案。Elasticsearch可以在你的笔记本上运行，也可以在数以百计的服务器上处理PB级别的数据。

Elasticsearch是一个基于Apache Lucene™的开源搜索引擎。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。

但是，Lucene只是一个库。想要使用它，你必须使用ava来作为开发语言并将其直接集成到你的应用中，更糟糕的是, Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的[RESTful](https://so.csdn.net/so/search?q=RESTful&spm=1001.2101.3001.7020) API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

## 2.2 Solr简介

Solr是Apache下的一个顶级开源项目，采用ava开发，它是基于Lucene的全文搜索服务器。Solr提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展，并对索引、搜索性能进行了优化

Solr可以独立运行，运行在)ltty、Tomcat等这些Servlet容器中，Solr索引的实现方法很简单，用POST方法向Solr服务器发送一个描述Field及其内容的XML文档，Solr根据xml文档添加、删除、更新索引。Solr搜索只需要发送HTTP GET请求然后对 Solr返回Xml.json等格式的查询结果进行解析，组织页面布局。Solr不提供构建UI的功能，Solr提供了一个管理界面，通过管理界面可以查询Solr的配置和运行情况。

solr是基于lucene开发企业级搜索服务器，实际上就是封装了lucene。

Solr是一个独立的企业级搜索应用服务器，它对外提供类似于Web-service的APlI接口。用户可以通过http请求，向搜索引擎服务器提衣一定格式的文件，牛成索引:也可以通讨提出杳找请求，并得到返回结果

## 2.3 Lucene简介

Lucene是apache软件基金会4 jakarta项目组的一个子项目，是一个开放源代码的全文检索引擎工具包，但它不是一个完整的全文检索引擎，而是一个全文检索引擎的架构，提供了完整的查询引擎和索引引擎，部分文本分析引擎（英文与德文两种西方语言)。Lucene的目的是为软件开发人员提供一个简单易用的工具包，以方便的在目标系统中实现全文检索的功能，或者是以此为基础建立起完整的全文检索引擎。Lucene是一套用于全文检索和搜寻的开源程式库，由Apache软件基金会支持和提供。Lucene提供了一个简单却强大的应用程式接口，能够做全文索引和搜寻。在Java开发环境里Lucene是一个成熟的免费开源工具。就其本身而言，Lucene是当前以及最近几年最受欢迎的免费Java信息检索程序库。人们经常提到信息检索程序库，虽然与搜索引擎有关，但不应该将信息检索程序库与搜索引擎相混淆。

Lucene是一个全文检索引擎的架构。那什么是全文搜索引擎?

全文搜索引擎是名副其实的搜索引擎，国外具代表性的有Google、Fast/AliTheWeb、AltaVista、Inktomi、Teoma、WiseNut等，国内著名的有百度(Baidu )。它们都是通过从互联网上提取的各个网站的信息（以网页文字为主）而建立的数据库中，检索与用户查询条件匹配的相关记录，然后按一定的排列顺序将结果返回给用户，因此他们是真正的搜索引擎。

从搜索结果来源的角度，全文搜索引擎又可细分为两种，一种是拥有自己的检索程序(Indexer )，俗称"蜘蛛" ( Spider )程序或"机器人" ( Robot)程序，并自建网页数据库，搜索结果直接从自身的数据库中调用，如上面提到的7家引擎;另一种则是租用其他引擎的数据库，并按自定的格式排列搜索结果，如Lycos引擎。

## 2.4 **ElasticSearch和Solr比较**

![image-20210201215917363](https://img-blog.csdnimg.cn/img_convert/2dcdd4002baffe0b4cab89fa228f9bb1.png)

![image-20210201215936203](https://img-blog.csdnimg.cn/img_convert/d963eb99df00cf9f33a51162f83b0f6e.png)

![image-20210201215951444](https://img-blog.csdnimg.cn/img_convert/3117fda9ece535b8987e0672bd81a47d.png)

![image-20210201220026178](https://img-blog.csdnimg.cn/img_convert/db848cb2094d552d1cdcc15ebf386e02.png)

## 2.5 **总结**

1、es基本是开箱即用，非常简单。Solr安装略微复杂一丢丢!

2、Solr利用Zookeeper进行分布式管理，而Elasticsearch自身带有分布式协调管理功能。

3、polr支持更多格式的数据，比如ISON、XML、CSv，而Elasticsearch仅支持json文件格式。

4、Solr官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑

5、Solr查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用﹔

- ES建立索引快（即查询慢），即实时性查询快，用于facebook新浪等搜索。
- Solr是传统搜索应用的有力解决方案，但Elasticsearch更适用于新兴的实时搜索应用。

6、Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区，而Elasticsearch相对开发维护者较少，更新太快，学习使用成本较高。

# 3. ElasticSearch安装

声明：最低JDK8

下载地址:https://www.elastic.co/cn/downloads/elasticsearch

## 3.1 Windows 下安装

1. 熟悉目录

   ```bash
   bin 启动文件
   config 		配置文件
   	log4j2.properties 	   日志配置文件
   	jvm.options		 java虚拟机相关的配置
   	ElasticSearch.yml 	ElasticSearch的配置文件 默认9200端口
   lib 		相关jar包
   logs 		日志
   modules		 功能模块
   plugins 		插件
   ```

2. `bin/elasticsearch.bat`启动，访问9200

   ![image-20220420151103111](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420151103111.png)

   

3. 访问测试

   ![image-20220420145851446](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420145851446.png)

## 3.2 可视化界面es head

下载地址:https://github.com/mobz/elasticsearch-head

前提需要npm，在该目录下安装依赖`npm install` 也可以用cnpm

启动`npm run start`：![image-20220420150534073](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420150534073.png)

由于**跨域**问题，目前是访问不到我们的ES的，需要配置 `config/elsaticsearch.yml`：

![image-20220420150802020](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420150802020.png)

重启ES测试：

![image-20220420151128890](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420151128890.png)

可以新建立一个索引进行学习

![image-20220420151340965](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420151340965.png)

初学时可以把es当做一个数据库(可以建立索引(库)，文档(库中的数据))。

**这个head 我们就把它当做数据展示工具，我们后面所有的查询都用Kibana**

## 3.3 了解ELK

ELK是`Elasticsearch`、`Logstash`、`Kibana`三大开源框架首字母大写简称。市面上也被成为Elastit Stack。其中Elasticsearch是一个基于Lucene、分布式、通过Restful方式进行交互的近实时搜索平台框架。像类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用Elasticsearch作为底层支持框架，可见Elasticsearch提供的搜索能力确实强大,市面上很多时候我们简称Elasticsearch为es。`Logstash`是ELK的**中央数据流引擎**，用于从不同目标（文件/数据存储/MQ）收集的不同格式数据，经过过滤后支持输出到不同目的地(文件/MQ/redis/elasticsearch/kafka等 )。`Kibana`可以将elasticsearch的**数据通过友好的页面展示出来**，提供实时分析的功能。

市面上很多开发只要提到ELK能够一致说出它是一个日志分析架构技术栈总称，但实际上ELK不仅仅适用于日志分析，它还可以支持其它任何数据分析和收集的场景，日志分析和收集只是更具有代表性。并非唯一性。

![image-20220420151737841](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420151737841.png)

## 3.4 安装Kibana

Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据。使用Kibana ,可以通过各种图表进行高级数据分析及展示。Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板( dashboard ) 实时显示Elasticsearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构，几分钟内就可以完成Kibana安装并启动Elasticsearch索引监测。

官网https://www.elastic.co/cn/downloads/kibana

Kibana 版本要和 ES 一致

**好处**：ELK都是基本上拆箱即用

> 启动测试

1. 解压后的目录

   ![image-20220420152750697](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420152750697.png)

2. 启动`bin/kibana.bat`，默认端口: 5601，访问测试

   ![image-20220420152948044](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420152948044.png)

3. 开发工具 (Post、curl、head、谷歌浏览器插件测试)

左边菜单 Dev Tools

![image-20220420153019284](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420153019284.png)

我们之后的所有操作都在这里进行编写

汉化：修改kibana配置即可 zh-CN

![image-20220420153115729](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420153115729.png)

# 4. ES核心概念

- 索引

- 字段类型(mapping)

- 文档(documents)

> 概述

在前面的学习中，我们已经掌握了es是什么，同时也把es的服务已经安装启动，那么es是如何去存储数据，数据结构是什么，又是如何实现搜索的呢? 我们先来聊聊ElasticSearch的相关概念。

**集群，节点，索引，类型，文档，分片，映射是什么?**

elasticsearch是面向文档， **一切都是JSON**

关系行数据库和elasticsearch客观的对比：

| Relational DB    | ElasticSearch |
| ---------------- | ------------- |
| 数据库(database) | 索引(indices) |
| 表(tables)       | types         |
| 行(rows)         | documents     |
| 字段(columns)    | fields        |

elasticsearch(集群)中可以包含多个索引(数据库)，每个索引中可以包含多个类型(表)，每个类型下又包含多个文档行)，每个文档中又包含多个字段(列)。

**物理设计:**

elasticsearch在后台把每个索引划分成多个分片，每分分片可以在集群中的不同服务器间迁移。

一个人就是一个集群，默认的集群名称就是elasticsearch

![image-20220420211343349](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420211343349.png)

**逻辑设计∶**

一个索引类型中，包含多个文档，比如说文档1，文档2。当我们索引一篇文档时，可以通过这样的一各顺序找到它**:**索引》类型文档ID，通过这个组合我们就能索引到某个具体的文档。注意:ID不必是整数，实际上它是个字符串

> 文档 documents

之前说elasticsearch是面向文档的，那么就意味着索引和搜索数据的最小单位是文档，elasticsearch中，文档有几个重要属性︰

- 自我包含，一篇文档同时包含字段和对应的值，也就是同时包含key:value 
- 可以是层次型的，一个文档中包含自文档，复杂的逻辑实体就是这么来的
- 灵活的结构，文档不依赖预先定义的模式，我们知道关系型数据库中，要提前定义字段才能使用，在elasticsearch中，对于字段是非常灵活的，有时候，我们可以忽略该字段，或者动态的添加一个新的字段

尽管我们可以随意的新增或者忽略某个字段，但是，每个字段的类型非常重要，比如一个年龄字段类型，可以是字符串也可以是整形。因为elasticsearch会保存字段和类型之间的映射及其他的设置。这种映射具体到每个映射的每种类型，这也是为什么在elasticsearch中，类型有时候也称为映射类型。

> 类型

类型是文档的逻辑容器，就像关系型数据库一样，表格是行的容器。类型中对于字段的定义称为映射，比如name映射为字符串类型。我们说文档是无模式的，它们不需要拥有映射中所定义的所有字段，比如新增一个字段，那么elasticsearch是怎么做的呢?elasticsearch会自动的将新字段加入映射，但是这个字段的不确定它是什么类型，elasticsearch就开始猜，如果这个值是18，那么elasticsearch会认为它是整形。但是elasticsearch也可能猜不对，所以最安全的方式就是提前定义好所需要的映射，这点跟关系型数据库殊途同归了，先定义好字段，然后再使用，别整什么幺蛾子。

> 索引

就是数据库

索引是映射类型的容器，elasticsearch中的索引是一个非常大的文档集合。索引存储了映射类型的字段和其他设置。然后它们被存储到了各个分片上了。我们来研究下分片是如何工作的。

**物理设计︰节点和分片如何工作**

![image-20220420212145744](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420212145744.png)

一个集群至少有一个节点，而一个节点就是一个elasricsearch进程，节点可以有多个索引。

默认的，如果你创建索引，那么索引将会有个5个分片( primary shard , 又称主分片)构成，每一个主分片会有一个副本( replica shard , 又称复制分片)

下图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内，这样有利于某个节点挂掉了，数据也不至于丢失。实际上，一个分片是一个Lucene索引，一个包含**倒排索引**的文件目录，倒排索引的结构使得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的关键字。

![image-20220420213031656](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420213031656.png)

> 倒排索引

elasticsearch使用的是一种称为倒排索引的结构，采用Lucene倒排索引作为底层。这种结构适用于快速的全文搜索，一个索引文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。

例如，现在有两个文档，每个文档包含如下内容∶

```bash
Study every day,good good up to forever 	# 文档1包含的内容
TO forever, study every day, good good up 	  # 文档2包含的内容
```

为了创建倒排索引，我们首先要将每个文档拆分成独立的词(或称为词条或者tokens)，然后创建一个包含所有不重复的词条的排序列表，然后列出每个词条出现在哪个文档：

![image-20220420212452836](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420212452836.png)

现在，我们试图搜索to forever，只需要查看包含每个词条的文档

![image-20220420212516079](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420212516079.png)

两个文档都匹配，但是第一个文档比第二个匹配程度更高。如果没有别的条件，现在，这两个包含关键字的文档都将返回。

再来看一个示例，比如我们通过博客标签来搜索博客文章。那么倒排索引列表就是这样的一个结构：

![image-20220420213650950](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220420213650950.png)

如果要搜索含有python标签的文章，那相对于查找所有原始数据而言，查找倒排索引后的数据将会快的多。只需要查看标签这一栏，然后获取相关的文章ID即可。完全过滤掉无关的所有数据，提高效率!

elasticsearch的索引和Lucene的索引对比：

在elasticsearch中，索引（库) 这个词被频繁使用，这就是术语的使用。在elasticsearch中，索引被分为多个分片，每份分片是一个Lucene的索引。所以一个elasticsearch索引是由多个Lucene索引组成的。别问为什么，谁让elasticsearch使用Lucene作为底层呢！如无特指，说起索引都是指elasticsearch的索引。

接下来的一切基础操作都在Kibana中Dev Tools下的Console里完成

# 5. IK分词器插件

> 什么是IK分词器?

分词︰即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个词，比如““我爱狂神"会被分为"我”“爱”“狂”"神”，这显然是不符合要求的，所以我们需要安装中文分词器ik来解决这个问题。

如果要使用中文,建议使用ik分词器

IK提供了两个分词算法: ik smart和ik_max_word，其中ik_smart为最少切分， ik_max_word为最细粒度划分!一会我们测试!

> 1. 安装

- 下载https://github.com/medcl/elasticsearch-analysis-ik
- 下载完毕后,放入我们的ElasticSearch插件中即可

![image-20210202135545804](https://img-blog.csdnimg.cn/img_convert/f116a7fca0e025bd67489a33969a722f.png)

- 重启观察ES

![image-20210202135723462](https://img-blog.csdnimg.cn/img_convert/cb58c640b6988537a7143cdda6a9315e.png)

可以看到ik分词器被加载

- elasticsearch-plugin可以通过这个命令来查看加载进来的插件

![image-20210202135921650](https://img-blog.csdnimg.cn/img_convert/438dd782a4c50b800b481a60ce261d9e.png)

- 使用Kibana测试

> 查看不同的分词效果

**ik_smart为最少切分**

![image-20210202140607602](https://img-blog.csdnimg.cn/img_convert/beb14ca976c7566b4c0d7ac98c9cc31f.png)

**ik_max_word为最细粒度划分**,**穷尽词库的可能**

![image-20210202140734273](https://img-blog.csdnimg.cn/img_convert/7f50eda4e2e45a1d55a57508a5b76a49.png)

> **问题**

![image-20210202141417855](https://img-blog.csdnimg.cn/img_convert/187a36447e31bc9a2ebecacd9c09db6b.png)

发现问题: 阿灰被拆开了

这种需要的词,需要自己加到我们的分词器的字典中

> ik分词器增加自己的配置

![image-20210202141345602](https://img-blog.csdnimg.cn/img_convert/44fd664485a4679fa7480576ffa485ad.png)

重启es

![image-20210202142003622](https://img-blog.csdnimg.cn/img_convert/d67b78f0571564d59114bb554f141937.png)

重启后再次使用

![image-20210202142102988](https://img-blog.csdnimg.cn/img_convert/aaa2aba49c1788de2d086d6b0898459e.png)

以后的话,我们需要自己配置分词就在自己定义的dic文件中进行配置即可

# 6. Rest风格说明

一种软件架构风格,而不是标准,只是提供了一组设计原则和约束条件.它主要是用于客户端和服务器交互类的软件.基于这个风格设计的软件可以更简洁,更有层次,更易于实现缓存等机制

基本Rest命令说明:

![image-20210202160722418](https://img-blog.csdnimg.cn/img_convert/e7c318298e4f0390472bd40a6d5ad078.png)

# 7. 关于索引的基本操作

1. 创建第一个索引

   ```bash
   PUT /索引名/~索引名(未来可能取消不写)~/文档id
   {请求体}
   12
   ```

   ![image-20210202161418109](https://img-blog.csdnimg.cn/img_convert/c0c120bb7a95fd9b737ffafa2604f05f.png)

   完成了自动增加了索引!数据也成功的添加了，这就是我说大家在初期可以把它当做数据库学习的原因

   ![image-20210202161807824](https://img-blog.csdnimg.cn/img_convert/50f063178c7a8be7b4ed04c0de8ed2e3.png)

2. 那么name这个字段用不用指定类型呢.毕竟我们关系型数据库时需要指定类型的啊

   - 字符串类型

     text, keyword

   - 数值类型

     long, integer, short, byte, double, float, half float, scaled fload

   - 日期类型

     date

   - 布尔值类型

     boolean

   - 二进制类型

     binart

   - 等等…

3. 指定字段的类型 ![image-20210202163130128](https://img-blog.csdnimg.cn/img_convert/dc8d7c4623383587a0a32517af2fd2f9.png)

4. 获得这个规则信息, 可以通过GET请求获取具体的信息

   ![image-20210202163237781](https://img-blog.csdnimg.cn/img_convert/f8c910850732350ee87e1c81ccce7add.png)

5. 查看默认的信息

   ![image-20210202163536686](https://img-blog.csdnimg.cn/img_convert/0b65ba6814d6e638ebd97391ccdaad00.png)

   ![image-20210202163627279](https://img-blog.csdnimg.cn/img_convert/527e3c2f44d527f67365a19da6e40a0e.png)

   如果自己的文档字段没有指定，那么es就会给我们默认配置字段类型!

   **扩展:**

   通过get _cat/ 可以获得当前的很多信息

   ![image-20210202164041757](https://img-blog.csdnimg.cn/img_convert/c873899b818ba2fbfcb7affd0b5c9c1c.png)

> **修改** 提交还是使用PUT即可,然后覆盖值,或用新办法

曾经的方法 用PUT覆盖

![image-20210202164323607](https://img-blog.csdnimg.cn/img_convert/4a05a689efb2ba47a3e5f0e95477af0e.png)

目前的方法

![image-20210202164549624](https://img-blog.csdnimg.cn/img_convert/d0a9cf9ec02364a86f5cb412b2dde557.png)

> 删除索引

![image-20210202164715701](https://img-blog.csdnimg.cn/img_convert/85d1d2a39d06214cf1652ed85848a241.png)

通过DELETE命令实现删除, 根据你的请求来判断是删除索引还是删除文档记录,

使用RESTFUL风格是我们ES推荐大家使用的

# 8. 关于文档的基本操作 🐼

> 基本操作

1. 添加数据

```java
PUT ahui/test/1
{
  "name": "ahui",
  "age": 21,
  "desc": "愿你拥有大风与烈酒,也能享受孤独与自由",
  "tags": ["二次元","宅男","码农"]
}
1234567
```

![image-20210203101204724](https://img-blog.csdnimg.cn/img_convert/c086630888a86c6f1ff44293d38e8fe3.png)

1. 获取数据

![image-20210203150531109](https://img-blog.csdnimg.cn/img_convert/5b164769746dc288572d2d8ed2eb1fc6.png)

1. 更新数据 PUT(相当于是覆盖了上一条)

![image-20210203150728077](https://img-blog.csdnimg.cn/img_convert/fc68596b1d6e9cc4b6d61d119babf6a3.png)

1. POST _update,推荐使用这种修改方式

![image-20210202164549624](https://img-blog.csdnimg.cn/img_convert/d0a9cf9ec02364a86f5cb412b2dde557.png)

简单的搜索

```bash
GET ahui/user/1
1
```

简单的条件查询,可以根据默认的映射规则,产生基本的查询

![image-20210203151433813](https://img-blog.csdnimg.cn/img_convert/346583767cea8bb9f50dc8e4079e71ba.png)

![image-20210203151713936](https://img-blog.csdnimg.cn/img_convert/e614570c14181b82ee0b0a0da378e7d7.png)

> 复杂查询搜索 select (排序,分页,高亮,模糊查询,精准查询等)

![image-20210203152113369](https://img-blog.csdnimg.cn/img_convert/79b0ab542629731dffd51a1766f8ce82.png)

![image-20210203152609744](https://img-blog.csdnimg.cn/img_convert/b1fffd234e10fe3473ed026e3317bf2b.png)

![image-20210203153319693](https://img-blog.csdnimg.cn/img_convert/02ff14237c3ef7dce224a23fcde0affc.png)

我们之后使用Java操作es, 所有的方法和对象就是这里面的key

> 排序

![image-20210203153603209](https://img-blog.csdnimg.cn/img_convert/1ff84ae4b41ec4fd23c3a0c49668c210.png)

> 分页查询

![image-20210203153809932](https://img-blog.csdnimg.cn/img_convert/45b55da3f2c06694be279d4d8be25ba6.png)

数据下标还是从0开始

> 布尔值查询

must (and) 所有的条件都要符合 相当于 where id = 1 and name = xxx

![image-20210203154023860](https://img-blog.csdnimg.cn/img_convert/51769c2a8a8f6a198b2208bb06e3f560.png)

should (or) 所有的条件符合其一 相当于 where id = 1 or name = xxx

![image-20210203154138680](https://img-blog.csdnimg.cn/img_convert/306c5f5776d20caf511f944b3c6f011a.png)

must_not (not) 反向查询

![image-20210203154237315](https://img-blog.csdnimg.cn/img_convert/3b7f52b00b44791af8ef2a99b2fd82ef.png)

过滤器 filter

![image-20210203154402972](https://img-blog.csdnimg.cn/img_convert/763f5f0c61c0aeb56206b01641a25d74.png)

- gt 大于
- gte 大于等于
- lt 小于
- lte 小于等于

> 匹配多个条件

![image-20210203154645084](https://img-blog.csdnimg.cn/img_convert/b9146b503760bc2a5d58236b7d9752fd.png)

> 精确查询

term 查询是直接通过倒排索引指定的词条进程精确的查找的

**关于分词:**

- term, 直接查询精确的
- match, 会使用分词器解析 (先分析文档,然后通过分析的文档进行查询)

**两个类型 text keyword**

![image-20210203155207040](https://img-blog.csdnimg.cn/img_convert/8d2e7faede3e502c75f853123fc7b91c.png)

![image-20210203155224582](https://img-blog.csdnimg.cn/img_convert/c60b6a8c36b7cae3d5949fa426e1a092.png)

![image-20210203155356125](https://img-blog.csdnimg.cn/img_convert/30459543e477d0cf0671a12ae2aeea26.png)

> 多个值匹配的精确查询

![image-20210203155527658](https://img-blog.csdnimg.cn/img_convert/4e9a067fdc52936b8e121d7beaef249a.png)

> 高亮查询

![image-20210203155714355](https://img-blog.csdnimg.cn/img_convert/41a801ed839dc73486b609ab4045f28c.png)

![image-20210203155812809](https://img-blog.csdnimg.cn/img_convert/fdb1b09bb360ec04fbd4ae7e04e7cb8a.png)

这些其实MySQL也可以做,试试Mysql效率比较低

- 匹配
- 按条件匹配
- 精确匹配
- 匹配字段过滤
- 多条件查询
- 高亮查询
- 倒排查询

# 9. 集成Springboot 🐼

> 找文档

![image-20210203170152242](https://img-blog.csdnimg.cn/img_convert/94ce2c2047763d76d358dae0eb7d837d.png)

![image-20210203165627533](https://img-blog.csdnimg.cn/img_convert/5e0a7136ed1b9c15f6c4c31b208212b3.png)

![image-20210203170135062](https://img-blog.csdnimg.cn/img_convert/5e922dd90a4254db266d4e5e221d74b2.png)

1. 找到原生的依赖

   ```xml
   <repositories>
       <repository>
           <id>es-snapshots</id>
           <name>elasticsearch snapshot repo</name>
           <url>https://snapshots.elastic.co/maven/</url>
       </repository>
   </repositories>
   1234567
   ```

2. 找对象

![image-20210203170606532](https://img-blog.csdnimg.cn/img_convert/fa9d7240e99c764c9cde8557ab1e6447.png)

1. 分析这个类中的方法即可

> 配置基本的项目

**问题:** 一定要保证我们导入的依赖和我们的es版本一致

![image-20210203192443948](https://img-blog.csdnimg.cn/img_convert/358e38b8d9c451517cf9e04c90ab6f32.png)

将本地对应的版本与此保持一致

![image-20210203192800522](https://img-blog.csdnimg.cn/img_convert/f93b90f45ce3832f4e56f4ac939f94b4.png)

源码中提供的对象

![image-20210203194341740](https://img-blog.csdnimg.cn/img_convert/ac188832eb26fd2c34d7a45725f83508.png)

> 具体的API测试

1. 创建索引
2. 判断索引是否存在
3. 删除索引
4. 创建文档
5. CRUD文档

**测试方法**

```java
package com.onlylmf;

import com.alibaba.fastjson.JSON;
import com.onlylmf.pojo.User;
import org.elasticsearch.action.admin.indices.delete.DeleteIndexRequest;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.support.master.AcknowledgedResponse;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.client.indices.CreateIndexRequest;
import org.elasticsearch.client.indices.CreateIndexResponse;
import org.elasticsearch.client.indices.GetIndexRequest;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.MatchAllQueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.index.query.TermQueryBuilder;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.FetchSourceContext;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.test.context.SpringBootTest;

import java.io.IOException;
import java.util.ArrayList;
import java.util.concurrent.TimeUnit;

@SpringBootTest
class AhuiEsApiApplicationTests {

    @Autowired
    @Qualifier("restHighLevelClient") //不加也行 但定义的名字要是这个名字
    private RestHighLevelClient client;


    //测试索引的创建 Request PUT ahui_index
    @Test
    void testCreateIndex() throws IOException {
        //1. 创建索引请求
        CreateIndexRequest request = new CreateIndexRequest("ahui_index");
        //2. 客户端执行请求
        CreateIndexResponse createIndexResponse = client.indices()
                .create(request, RequestOptions.DEFAULT);

        System.out.println(createIndexResponse);
    }

    //测试获取索引,判断其是否存在
    @Test
    void testExistIndex() throws IOException {
        GetIndexRequest request = new GetIndexRequest("ahui_index");
        boolean exists = client.indices().exists(request, RequestOptions.DEFAULT);
        System.out.println(exists);
    }

    //测试删除索引
    @Test
    void testDeleteIndex() throws IOException {
        DeleteIndexRequest request = new DeleteIndexRequest("ahui_index");
        //删除
        AcknowledgedResponse delete = client.indices().delete(request, RequestOptions.DEFAULT);
        System.out.println(delete.isAcknowledged());
    }

    @Test
    //测试添加文档
    void testAddDocument() throws IOException {
        //创建对象
        User user = new User("阿灰", 21);
        //创建请求
        IndexRequest request = new IndexRequest("ahui_index");

        //规则 put ahui_index/_doc/1
        request.id("1");
        request.timeout(TimeValue.timeValueSeconds(1));
        request.timeout("1s");

        //将我们的数据放入请求 json (使用fastjson进行转换)
        IndexRequest source = request.source(JSON.toJSONString(user), XContentType.JSON);

        //客户端发送请求, 获取响应的结果
        IndexResponse indexResponse = client.index(request, RequestOptions.DEFAULT);

        //返回具体的json信息
        System.out.println(indexResponse.toString());
        //对应我们命令返回的状态 CREATED
        System.out.println(indexResponse.status());
    }

    @Test
    //获取文档
    void testIsExistes() throws IOException {
        GetRequest getRequest = new GetRequest("ahui_index", "1");
        //不获取返回的_source 的上下文了
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        getRequest.storedFields("_none_");

        boolean exists = client.exists(getRequest, RequestOptions.DEFAULT);
        System.out.println(exists);
    }

    @Test
    //获取文档的信息
    void testGetDocument() throws IOException {
        GetRequest getRequest = new GetRequest("ahui_index", "1");
        GetResponse getResponse = client.get(getRequest, RequestOptions.DEFAULT);
        //打印文档的内容
        System.out.println(getResponse.getSourceAsString());
        //返回的全部内容和命令是一样的
        System.out.println(getResponse);
    }

    @Test
    //获取文档的信息
    void testUpdateRequest() throws IOException {
        UpdateRequest updateRequest = new UpdateRequest("ahui_index", "1");
        updateRequest.timeout("1s");

        User user = new User("阿灰学Java", 22);
        updateRequest.doc(JSON.toJSONString(user),XContentType.JSON);

        UpdateResponse updateResponse = client.update(updateRequest, RequestOptions.DEFAULT);
        System.out.println(updateResponse.status());

    }

    @Test
    //删除文档记录
    void testDeleteRequest() throws IOException {
        DeleteRequest deleteRequest = new DeleteRequest("ahui_index", "1");
        deleteRequest.timeout("1s");

        DeleteResponse delete = client.delete(deleteRequest, RequestOptions.DEFAULT);
        System.out.println(delete.status());

    }

    @Test
    //特殊的,实际项目中一般都会批量插入数据
    void testBulkRequest() throws IOException{
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("10s");

        ArrayList<User> userList = new ArrayList<>();
        userList.add(new User("ahui1",18));
        userList.add(new User("ahui2",18));
        userList.add(new User("ahui3",18));
        userList.add(new User("onlylmf1",18));
        userList.add(new User("onlylmf2",18));
        userList.add(new User("onlylmf3",18));

        //批量处理请求
        for (int i = 0; i < userList.size(); i++) {
            //批量更新和批量修改等, 就在这里修改对应的请求就可以了
            bulkRequest.add(new IndexRequest("ahui_index")
            .id("" + (i + 1)) //不加id的话会默认生成随机id
            .source(JSON.toJSONString(userList.get(i)),XContentType.JSON));
        }
        BulkResponse bulkResponse = client.bulk(bulkRequest, RequestOptions.DEFAULT);
        //是够失败, 返回false代表成功
        System.out.println(bulkResponse.hasFailures());
    }

    @Test
    //查询
    //SearchRequest 搜索请求
    //SearchSourceBuilder 条件构造
    //HighLightBuilder 构建高亮
    //xxx QueryBuilder 对应我们刚才看到的所有命令
    void testSearch() throws IOException {
        SearchRequest searchRequest = new SearchRequest("ahui_index");
        //构建搜索条件
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        //查询条件,我们可以使用QueryBuilders 工具类来实现
        //QueryBuilders.termQuery  精确匹配
        //QueryBuilders.matchAllQuery 匹配所有
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("name", "ahui1");
//        MatchAllQueryBuilder matchAllQueryBuilder = QueryBuilders.matchAllQuery();
        sourceBuilder.query(termQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
        
        searchRequest.source(sourceBuilder);

        SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(searchResponse.getHits()));
        System.out.println("==========================================");
        for (SearchHit documentFileds : searchResponse.getHits().getHits()) {
            System.out.println(documentFileds.getSourceAsMap());
        }
    }
}

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206
```

# 10. JD实战

**Jsoup爬取JD数据**

![image-20210204103051653](https://img-blog.csdnimg.cn/img_convert/ffa7aa6eb2d95249049f4be8734a67aa.png)

## 爬虫

> 数据问题? 数据库获取,消息队列中获取,都可以成为数据源,爬虫

1. 上JD进行搜索java 复制链接https://search.jd.com/Search?keyword=java&enc=utf-8

   ![image-20210205102102403](https://img-blog.csdnimg.cn/img_convert/3ee886ecff5700dd4816026b33a6f591.png)

2. F12进行查看书籍在那个div下

   ![image-20210205102205642](https://img-blog.csdnimg.cn/img_convert/d10dce1004ab2a46c91bed16bdffaca6.png)

   已经每本书下个信息

   ![image-20210205102246791](https://img-blog.csdnimg.cn/img_convert/54fda8eb00ed38b690e0528e66657465.png)

3. 导入jsoup依赖

   ```xml
   <!--解析网页jsoup-->
           <dependency>
               <groupId>org.jsoup</groupId>
               <artifactId>jsoup</artifactId>
               <version>1.13.1</version>
           </dependency>
   
   <!--引入阿里巴巴的fastjson-->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>fastjson</artifactId>
               <version>1.2.75</version>
           </dependency>
   12345678910111213
   ```

4. 创建utils包并建立HtmlParseUtil.java爬取测试

   ```java
   //测试数据
   public static void main(String[] args) throws IOException, InterruptedException {
   	//获取请求
       String url = "https://search.jd.com/Search?keyword=java";
   	// 解析网页 （Jsou返回的Document就是浏览器的Docuement对象）
       Document document = Jsoup.parse(new URL(url), 30000);
       //获取id，所有在js里面使用的方法在这里都可以使用
       Element element = document.getElementById("J_goodsList");
       //获取所有的li元素
       Elements elements = element.getElementsByTag("li");
       //用来计数
       int c = 0;
       //获取元素中的内容  ，这里的el就是每一个li标签
       for (Element el : elements) {
           c++;
           //这里有一点要注意，直接attr使用src是爬不出来的，因为京东使用了img懒加载
           String img = el.getElementsByTag("img").eq(0).attr("data-lazy-img");
           //获取商品的价格，并且只获取第一个text文本内容
           String price = el.getElementsByClass("p-price").eq(0).text();
           String title = el.getElementsByClass("p-name").eq(0).text();
           String shopName = el.getElementsByClass("p-shop").eq(0).text();
   
           System.out.println("========================================");
           System.out.println(img);
           System.out.println(price);
           System.out.println(title);
           System.out.println(shopName);
       }
       System.out.println(c);
   }
   123456789101112131415161718192021222324252627282930
   ```

   测试结果

   ![image-20210205102437867](https://img-blog.csdnimg.cn/img_convert/80980bf73f6a4fa5a84d1996c1338b27.png)

   获取结果没有问题后,将此方法封装为一个工具类使用

5. 创建pojo实体类

   Content.java

   ```java
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Content {
       private String title;
       private String img;
       private String price;
       //可以自行添加属性
   }
   12345678910111213
   ```

6. 封装工具类

   HtmlParseUtils.java

   ```java
   @Component
   public class HtmlParseUtil {
   //    public static void main(String[] args) throws IOException {
   //        new HtmlParseUtil().parseJD("Java").forEach(System.out::println);
   //    }
   
       public List<Content> parseJD(String keywords) throws IOException {
           //获取请求 https://search.jd.com/Search?keyword=java&enc=utf-8
           //前提需要连网
           String url = "https://search.jd.com/Search?keyword=" + keywords + "&enc=utf-8";
           //解析网页 (Jsoup返回Document就是浏览器Document对象)
           Document document = Jsoup.parse(new URL(url), 30000);
           //所有在js中能使用的方法,这里都能用
           Element element = document.getElementById("J_goodsList");
           //获取所有li元素
           Elements elements = element.getElementsByTag("li");
   
           ArrayList<Content> goodList = new ArrayList<>();
   
           //通过元素中的内容,这里el就是每一个li标签了
           for (Element el : elements) {
               //加if判断是为了 过滤空标签
               if (el.attr("class").equalsIgnoreCase("gl-item")) {
                   //关于这种图片特别多的网页,所有的图片都是延迟加载的
                   //在jd搜索后f12可以看到存放在data-lazy-img中
                   String img = el.getElementsByTag("img").eq(0).attr("data-lazy-img");
                   String price = el.getElementsByClass("p-price").eq(0).text();
                   String title = el.getElementsByClass("p-name").eq(0).text();
   
                   Content content = new Content();
                   content.setImg(img);
                   content.setPrice(price);
                   content.setTitle(title);
                   goodList.add(content);
               }
   
           }
           return goodList;
    }
   }
   12345678910111213141516171819202122232425262728293031323334353637383940
   ```

7. 编写业务层代码 (这里就不写接口了)

   ContentService.java

   首先完成一个方法让爬取的数据存入ES中

   ```java
   //业务编写
   @Service
   public class ContentService {
   	
   	//将客户端注入
       @Autowired
       @Qualifier("restHighLevelClient")
       private RestHighLevelClient client;
   
       //1、解析数据放到 es 中
       public boolean parseContent(String keyword) throws IOException {
           List<Content> contents = new HtmlParseUtil().parseJD(keyword);
           //把查询的数据放入 es 中
           BulkRequest request = new BulkRequest();
           request.timeout("2m");
   
           for (int i = 0; i < contents.size(); i++) {
               request.add(
                       new IndexRequest("jd_goods")
                               .source(JSON.toJSONString(contents.get(i)), XContentType.JSON));
   
           }
           BulkResponse bulk = client.bulk(request, RequestOptions.DEFAULT);
           return !bulk.hasFailures();
       }
   }
   1234567891011121314151617181920212223242526
   ```

8. 在Controller包下建立

   ContentController.java

   ```java
   //请求编写
   @RestController
   public class ContentController {
   
       @Autowired
       private ContentService contentService;
   
       @GetMapping("/parse/{keyword}")
       public Boolean parse(@PathVariable("keyword") String keyword) throws IOException {
           return contentService.parseContent(keyword);
       }
   }
   123456789101112
   ```

9. 启动Springboot项目,访问是否能将数据爬取至ES

   ![image-20210206093547570](https://img-blog.csdnimg.cn/img_convert/16e774225e9d6f52369b443bce78eb60.png)

![image-20210206093754269](https://img-blog.csdnimg.cn/img_convert/735d746b3d5e0debff12d531a7c74892.png)

### 实现搜索功能

1. 在ContentService.java中添加

   ```java
   //2、获取这些数据实现基本的搜索功能
   public List<Map<String, Object>> searchPage(String keyword, int pageNo, int pageSize) throws IOException {
       if (pageNo <= 1) {
           pageNo = 1;
       }
       if (pageSize <= 1) {
           pageSize = 1;
       }
   
       //条件搜索
       SearchRequest searchRequest = new SearchRequest("jd_goods");
       SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
   
       //分页
       sourceBuilder.from(pageNo).size(pageSize);
   
       //精准匹配
       TermQueryBuilder termQuery = QueryBuilders.termQuery("title", keyword);
   
       sourceBuilder.query(termQuery);
       sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
       //执行搜索
       SearchRequest source = searchRequest.source(sourceBuilder);
       SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
       //解析结果
   
       List<Map<String, Object>> list = new ArrayList<>();
       for (SearchHit documentFields : searchResponse.getHits().getHits()) {
           list.add(documentFields.getSourceAsMap());
       }
       return list;
   }
   1234567891011121314151617181920212223242526272829303132
   ```

2. 在ContentController.java中添加搜索请求,使用RestFul

   ```java
   @GetMapping("/search/{keyword}/{pageNo}/{pageSize}")
   public List<Map<String, Object>> search(@PathVariable("keyword") String keyword,
                                           @PathVariable("pageNo") int pageNo,
                                           @PathVariable("pageSize") int pageSize) throws IOException {
       List<Map<String, Object>> list = contentService.searchPage(keyword, pageNo, pageSize);
       return list;
   }
   1234567
   ```

3. 访问访问http://127.0.0.1:9090/search/java/1/10 (查询Java 并从第一条显示到第十条)

   ![image-20210206094051219](https://img-blog.csdnimg.cn/img_convert/b43e8fcb959653baebf28967215cf848.png)

到此数据的爬取和搜索都没有问题了,下面就要开始前后端的分离工作了

## 前后端分离

首先导入准备好的资源 并把axios,jquery,vue的js包导入

![image-20210206094327869](https://img-blog.csdnimg.cn/img_convert/0c4d88be05fc2ef84b6d0888383651ea.png)

1. 前端需要接受数据

   用vue接受数据

   ![image-20210206094418199](https://img-blog.csdnimg.cn/img_convert/b549c688b0449ab4a18f2ea729916ebb.png)

   ```vue
   <!--前端使用vue完成前后端分离-->
   <script th:src="@{/js/axios.min.js}"></script>
   <script th:src="@{/js/vue.min.js}"></script>
   
   <script>
       new Vue({
           el: '#app',
           data: {
               keyword: '',  //搜索的关键字
               result: []  //搜索的结果
           },
           methods: {
               searchKey() {
                   var keyword = this.keyword
                   axios.get('search/' + keyword + '/1/210').then(response => {
                       //console.log(response);
                       this.result = response.data;//绑定数据！
                   })
               }
           }
       })
   </script>
   12345678910111213141516171819202122
   ```

   然后为按钮绑定点击事件

   ![image-20210206094550490](https://img-blog.csdnimg.cn/img_convert/41a6297f8f647dde2311da45ceff07bc.png)

2. 用vue给前端传递数据

   ![image-20210206094658559](https://img-blog.csdnimg.cn/img_convert/65f335b61aa466f9c6f0a5f348f04926.png)

3. 访问127.0.0.1:8080 搜索java进行尝试

   ![image-20210206094841592](https://img-blog.csdnimg.cn/img_convert/b425899087cc79eecb2d2c7f3a229a62.png)

   由此 基本功能实现完成

## 搜索高亮

1. 修改`ContentService.java`中的搜索功能

   ```java
   //3、获取这些数据实现基本的搜索高亮功能
   public List<Map<String, Object>> searchPagehighlighter(String keyword, int pageNo, int pageSize) throws IOException {
       if (pageNo <= 1) {
           pageNo = 1;
       }
       if (pageSize <= 1) {
           pageSize = 1;
       }
   
       //条件搜索
       SearchRequest searchRequest = new SearchRequest("jd_goods");
       SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
   
       //分页
       sourceBuilder.from(pageNo).size(pageSize);
   
       //精准匹配
       TermQueryBuilder termQuery = QueryBuilders.termQuery("title", keyword);
   
       //====================================   高   亮   ==========================================
       HighlightBuilder highlightBuilder = new HighlightBuilder(); //获取高亮构造器
       highlightBuilder.field("title"); //需要高亮的字段
       highlightBuilder.requireFieldMatch(false);//不需要多个字段高亮
       highlightBuilder.preTags("<span style='color:red'>"); //前缀
       highlightBuilder.postTags("</span>"); //后缀
       sourceBuilder.highlighter(highlightBuilder); //把高亮构造器放入sourceBuilder中
       sourceBuilder.query(termQuery);
       sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
       //执行搜索
       SearchRequest source = searchRequest.source(sourceBuilder);
       SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
       
       //解析结果
       List<Map<String, Object>> list = new ArrayList<>();
       for (SearchHit hit : searchResponse.getHits().getHits()) {
   
           Map<String, HighlightField> highlightFields = hit.getHighlightFields();//获取高亮字段
           HighlightField title = highlightFields.get("title"); //得到我们需要高亮的字段
           Map<String, Object> sourceAsMap = hit.getSourceAsMap();//原来的返回的结果
   
           //解析高亮的字段
           if (title != null) {
               Text[] fragments = title.fragments();
               String new_title = "";
               for (Text text : fragments) {
                   new_title += text;
               }
               sourceAsMap.put("title", new_title);  //高亮字段替换掉原来的内容即可
           }
           list.add(sourceAsMap);
       }
       return list;
   }
   1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253
   ```

2. 改变Controller中的搜索请求后进行尝试

   ```java
   @GetMapping("/search/{keyword}/{pageNo}/{pageSize}")
   public List<Map<String, Object>> search(@PathVariable("keyword") String keyword,
                                           @PathVariable("pageNo") int pageNo,
                                           @PathVariable("pageSize") int pageSize) throws IOException {
       List<Map<String, Object>> list = contentService.searchPagehighlighter(keyword, pageNo, pageSize);
       return list;
   }
   1234567
   ```

3. 访问尝试

   ![image-20210206095537325](https://img-blog.csdnimg.cn/img_convert/df7ef93e8951d0ad09beda9bff245aec.png)

   发现不是我们需要的红字 而是标签样式显现出来了

4. 解决问题

   Vue给了很好的解决办法

   ![image-20210206095656929](https://img-blog.csdnimg.cn/img_convert/66d014b80db58e1871184015f2de6d56.png)

5. 再次访问尝试

   ![image-20210206095732768](https://img-blog.csdnimg.cn/img_convert/b77eeddb1504e2b63768f71eea968168.png)

文章知识点与官方知识档案匹配，可进一步学习相关知识

[云原生入门技能树](https://bbs.csdn.net/skill/cloud_native/cloud_native-f22bbbe40efb4940914c5ba59021998d)[容器编排(学习环境 k8s)](https://bbs.csdn.net/skill/cloud_native/cloud_native-f22bbbe40efb4940914c5ba59021998d)[安装kubectl](https://bbs.csdn.net/skill/cloud_native/cloud_native-f22bbbe40efb4940914c5ba59021998d)240 人正在系统学习中