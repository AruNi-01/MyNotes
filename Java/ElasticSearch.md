# 1. ElasticSearch

**Elaticsearch**，简称为**ES**，**ES**是一个开源的**高扩展的分布式全文检索引擎**，它可以近乎**实时的存储、检索数据**；本身扩展性很好，可以扩展到上百台服务器，处理 PB 级别（[大数据](https://so.csdn.net/so/search?q=大数据&spm=1001.2101.3001.7020)时代）的数据。

**ES**由 Java 语言开发并使用 **Lucene** 作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的 RESTFULL [API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020) 来隐藏 **Lucene** 的复杂性，从而让全文搜索变得简单。据国际权威的数据库产品评测机构 DB Engines 的统计，在2016 年1月，**ElasticSearch** 已超过 **Solr** 等，成为排名第一的搜索引擎类应用。

## 1.1 介绍

**聊聊Doug Cutting**

1998年9月4日,Google公司在美国硅谷成立,正如大家所知,它是一家做搜索引擎起家的公司

无独有偶，一位名叫Doug Cutting的美国工程师，也迷上了搜索引擎。他做了一个用于文本搜索的函数库（姑且理解为软件的功能组件），命名为[Lucene](https://so.csdn.net/so/search?q=Lucene&spm=1001.2101.3001.7020)。

Lucene是用JAVA写成的，目标是为各种中小型应用软件加入[全文检索](https://so.csdn.net/so/search?q=全文检索&spm=1001.2101.3001.7020)功能。因为好用而且开源(代码公开），非常受程序员们的欢迎。

早期的时候，这个项目被发布在Doug Cutting的个人网站和SourceForge (一个开源软件网站)。后来，2001年底，Lucene成为Apache软件基金会jakarta项目的一个子项目。

2004年，Doug Cutting再接再励，在Lucene的基础上，和Apache开源伙伴Mike Cafarella合作，开发了一款可以代替当时的主流搜索的开源搜索引擎，命名为Nutch。

**Lucene和ElasticSearch关系**：

- ElasticSearch是基于Lucene做了一些封装和增强(上手简单)

## 1.2 为什么要用ElasticSearch

为什么要使用Elasticsearch呢？我们在日常开发中，**数据库**也能做到（实时、存储、搜索、分析）。

相对于数据库，Elasticsearch的强大之处就是可以**模糊查询**

数据库怎么就不能模糊查询了？如下SQL：

```sql
select * from user where name like '%公众号Java3y%'
```

这不就可以把**公众号Java3y**相关的内容搜索出来了吗？

的确，这样做的确可以。但是要明白的是：`name like %Java3y%`这类的查询是不走**索引**的，不走索引意味着：只要数据库的量很大（1亿条），查询肯定会是**秒**级别的。

而且，即便给从数据库根据**模糊匹配**查出相应的记录了，那往往会返回**大量的数据**给你，往往你需要的数据量并没有这么多，可能50条记录就足够了。

还有一个就是：用户输入的内容往往并没有这么的**精确**，比如从Google输入`ElastcSeach`（打错字），但是Google还是能估算我想输入的是`Elasticsearch`。

而Elasticsearch是专门做**搜索**的，就是为了解决上面所讲的问题而生的：

- Elasticsearch对模糊搜索非常擅长（搜索速度很快）
- 从Elasticsearch搜索到的数据可以根据**评分**过滤掉大部分的，只要返回评分高的给用户就好了（原生就支持排序）
- 没有那么准确的关键字也能搜出相关的结果（能匹配有相关性的记录）

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

- 当单纯的对已有数据进行搜索时，Solr更快

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a0dc8ca9961b84bde81e8d3df5634ff1.webp)

- 当实时建立索引时, Solr会产生io阻塞，查询性能较差，Elasticsearch具有明显的优势

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b208dd95ad6aa23520f001a4749aee67.webp)

- 随着数据量的增加，Solr的搜索效率会变得更低，而Elasticsearch却没有明显的变化

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/082c44343a48346636a5823da29c2764.webp)

综上所述，Solr的架构不适合实时搜索的应用。

>  实际生产环境测试

- 将搜索引擎从Solr转到Elasticsearch以后的平均查询速度有了50倍的提升

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/de472d3421f60d05eab2078b73599f9c.webp)

## 2.5 **总结**

1、es基本是开箱即用，非常简单。Solr安装略微复杂一丢丢

2、Solr利用Zookeeper进行分布式管理，而Elasticsearch自身带有分布式协调管理功能。

3、polr支持更多格式的数据，比如ISON、XML、CSv，而Elasticsearch仅支持json文件格式。

4、Solr官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑

5、Solr查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用；

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

   ![image-20220420151103111](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c7e1c308977cfbf9d44a0326fde599db.png)

   

3. 访问测试

   ![image-20220420145851446](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e364e43aa306ffe3437394080cd42b7a.png)

## 3.2 可视化界面es head

下载地址:https://github.com/mobz/elasticsearch-head

前提需要npm，在该目录下安装依赖`npm install` 也可以用cnpm

启动`npm run start`：![image-20220420150534073](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/11ee6c2b43e106155fee6a1b0a5e11d3.png)

由于**跨域**问题，目前是访问不到我们的ES的，需要配置 `config/elsaticsearch.yml`：

![image-20220420150802020](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3b4925659647d00960eb24d5613a42b1.png)

重启ES测试：

![image-20220420151128890](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e5cd20e291c3d1b9ca5ab80800db163f.png)

可以新建立一个索引进行学习

![image-20220420151340965](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4c90c8bbb292c1ff5219c54801d42e6b.png)

初学时可以把es当做一个数据库(可以建立索引(库)，文档(库中的数据))。

**这个head 我们就把它当做数据展示工具，我们后面所有的查询都用Kibana**

## 3.3 了解ELK

ELK是`Elasticsearch`、`Logstash`、`Kibana`三大开源框架首字母大写简称。市面上也被成为Elastit Stack。其中Elasticsearch是一个基于Lucene、分布式、通过Restful方式进行交互的近实时搜索平台框架。像类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用Elasticsearch作为底层支持框架，可见Elasticsearch提供的搜索能力确实强大,市面上很多时候我们简称Elasticsearch为es。`Logstash`是ELK的**中央数据流引擎**，用于从不同目标（文件/数据存储/MQ）收集的不同格式数据，经过过滤后支持输出到不同目的地(文件/MQ/redis/elasticsearch/kafka等 )。`Kibana`可以将elasticsearch的**数据通过友好的页面展示出来**，提供实时分析的功能。

市面上很多开发只要提到ELK能够一致说出它是一个日志分析架构技术栈总称，但实际上ELK不仅仅适用于日志分析，它还可以支持其它任何数据分析和收集的场景，日志分析和收集只是更具有代表性。并非唯一性。

![image-20220420151737841](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/76f3b140bb46b65087749a83fcf3bfb0.png)

## 3.4 安装Kibana

Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据。使用Kibana ,可以通过各种图表进行高级数据分析及展示。Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板( dashboard ) 实时显示Elasticsearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构，几分钟内就可以完成Kibana安装并启动Elasticsearch索引监测。

官网https://www.elastic.co/cn/downloads/kibana

Kibana 版本要和 ES 一致

**好处**：ELK都是基本上拆箱即用

> 启动测试

1. 解压后的目录

   ![image-20220420152750697](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/13dde045928bd156605c579e1c5b5560.png)

2. 启动`bin/kibana.bat`，默认端口: 5601，访问测试

   ![image-20220420152948044](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8097b1ec22b4caf914aa079ad8be5b0c.png)

3. 开发工具 (Post、curl、head、谷歌浏览器插件测试)

左边菜单 Dev Tools

![image-20220420153019284](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e1908048801381fd7ec5c9043b1a3b13.png)

我们之后的所有操作都在这里进行编写

汉化：修改kibana配置即可 zh-CN

![image-20220420153115729](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/47aa86b88f833c64cb8c008909d99e7e.png)

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

![image-20220420211343349](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1a9ac3d61b69841b288637b86bbf0f99.png)

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

![image-20220420212145744](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fe37dffb4947404453dd1d429d5ec4f6.png)

一个集群至少有一个节点，而一个节点就是一个elasricsearch进程，节点可以有多个索引。

默认的，如果你创建索引，那么索引将会有个5个分片( primary shard , 又称主分片)构成，每一个主分片会有一个副本( replica shard , 又称复制分片)

下图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内，这样有利于某个节点挂掉了，数据也不至于丢失。实际上，一个分片是一个Lucene索引，一个包含**倒排索引**的文件目录，倒排索引的结构使得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的关键字。

![image-20220420213031656](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9bc02764089f1adbb214e74debe33c8e.png)

> 倒排索引

elasticsearch使用的是一种称为倒排索引的结构，采用Lucene倒排索引作为底层。这种结构适用于快速的全文搜索，一个索引文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。

例如，现在有两个文档，每个文档包含如下内容∶

```bash
Study every day,good good up to forever 	# 文档1包含的内容
TO forever, study every day, good good up 	  # 文档2包含的内容
```

为了创建倒排索引，我们首先要将每个文档拆分成独立的词(或称为词条或者tokens)，然后创建一个包含所有不重复的词条的排序列表，然后列出每个词条出现在哪个文档：

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| Study   | √     | x     |
| To      | x     | x     |
| every   | √     | √     |
| forever | √     | √     |
| day     | √     | √     |
| study   | x     | √     |
| good    | √     | √     |
| every   | √     | √     |
| to      | √     | x     |
| up      | √     | √     |

现在，我们试图搜索to forever，只需要查看包含每个词条的文档

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| to      | √     | x     |
| forever | √     | √     |
| total   | 2     | 1     |

两个文档都匹配，但是第一个文档比第二个匹配程度更高。如果没有别的条件，现在，这两个包含关键字的文档都将返回。

再来看一个示例，比如我们通过博客标签来搜索博客文章。那么倒排索引列表就是这样的一个结构：

![image-20220420213650950](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f2e8d5b7febf218bbabd4d73b0e96c1a.png)

如果要搜索含有python标签的文章，那相对于查找所有原始数据而言，查找倒排索引后的数据将会快的多。只需要查看标签这一栏，然后获取相关的文章ID即可。完全过滤掉无关的所有数据，提高效率!

elasticsearch的索引和Lucene的索引对比：

在elasticsearch中，索引（库) 这个词被频繁使用，这就是术语的使用。在elasticsearch中，索引被分为多个分片，每份分片是一个Lucene的索引。所以一个elasticsearch索引是由多个Lucene索引组成的。别问为什么，谁让elasticsearch使用Lucene作为底层呢！如无特指，说起索引都是指elasticsearch的索引。

接下来的一切基础操作都在Kibana中Dev Tools下的Console里完成

# 5. IK分词器插件

> 什么是IK分词器?

分词︰即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个词，比如 "我爱狂神" 会被分为 "我” “爱” “狂” "神”，这显然是不符合要求的，所以我们需要安装中文分词器ik来解决这个问题。

如果要使用中文，建议使用ik分词器

IK提供了两个分词算法：

- ik_smart：最少切分
- ik_max_word：最细粒度划分

> 安装

- 下载https://github.com/medcl/elasticsearch-analysis-ik
- 下载完毕后,放入我们的ElasticSearch插件中即可

![image-20220421133609354](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fda51e122a91ae035f7f567aa0801911.png)

重启观察ES

![image-20220421133956796](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/18ebcbfacafd6c9c13d77f4b5d82fd68.png)

可以看到ik分词器被加载

`elasticsearch-plugin`可以通过这个命令来查看加载进来的插件

![image-20220421133917091](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f6c177edbd057f94563e38ca0e352477.png)

接下来使用Kibana测试

> 查看不同的分词效果

**ik_smart为最少切分**：

![image-20220421135031502](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6d02cd5089fbf95342790d0e2bcfc73a.png)

**ik_max_word为最细粒度划分**，**穷尽词库的可能**

![image-20220421135051190](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4e9df6d091e59a0d81abd0d390fd27d4.png)

问题：狂神说 被拆开了

这种需要的词，需要自己加到我们的分词器的字典中

> ik分词器中增加自己的配置

`IKAnalyzer.cfg.xml`为分词的配置文件；

![image-20220421135546218](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e288ef939a5b3e7e52ab0815f5553be5.png)

在自己的配置文件中添加分词：

![image-20220421135717514](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b3f064e797bb267845da094f7f4d03c6.png)

重启es

![image-20220421135857086](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/34eb8c006ae82f57ec6c0c55e92a7df3.png)

重启后再次使用：

![image-20220421135926378](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a79f4ab728bfaeb2847c758f4d73b9e9.png)

以后的话，我们需要自己配置分词就在自己定义的dic文件中进行配置即可！

# 6. Rest风格说明

一种软件架构风格，而不是标准，只是提供了一组设计原则和约束条件。

它主要是用于客户端和服务器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

基本Rest命令说明：

| method | url地址                                         | 描述                   |
| ------ | ----------------------------------------------- | ---------------------- |
| PUT    | localhost:9200/索引名称/类型名称/文档id         | 创建文档(指定文档id)   |
| POST   | localhost:9200/索引名称/类型名称                | 创建文档（随机文档id） |
| POST   | localhost:9200/索引名称/类型名称/文档id/_update | 修改文档               |
| DELETE | localhost:9200/索引名称/类型名称/文档id         | 删除文档               |
| GET    | localhost:9200/索引名称/类型名称/文档id         | 通过文档id查询文档     |
| POST   | localhost:9200/索引名称/类型名称/_search        | 查询所有的数据         |

# 7. 关于索引的基本操作

> 添加PUT，查看GET

1. 创建第一个索引

   ```bash
   PUT /索引名/~索引名(未来可能取消不写)~/文档id
   {请求体}
   
   PUT /test1/type1/1
   {
     "name": "AruNi",
     "age": "18"
   }
   ```
   
   ![image-20220421161308521](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f693037af7b0209e1fcf69f27c24bea0.png)
   
   完成了自动增加了索引，数据也成功的添加了：
   
   ![image-20220421161406591](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ffd0a6c48dce30ee5ccf64e6493ca38f.png)
   
2. 那么name这个字段用不用指定类型呢。毕竟我们关系型数据库时需要指定类型的

   - 字符串类型：text, keyword

   - 数值类型：long, integer, short, byte, double, float, half float, scaled fload

   - 日期类型：date

   - 布尔值类型：boolean

   - 二进制类型：binart

   - 等等…

3. 定义规则，指定字段的类型：

   ![image-20220421161822063](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c8056bbafb60cedd26bad34eeabeec34.png)

4. 获得这个规则信息, 可以通过GET请求获取具体的信息

   ![image-20220421162202958](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/30d54140e15972abec18426df3b7a2cf.png)

5. 查看默认的信息：

   先新建一个索引，插入一个数据：

   ![image-20220421162508309](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/65455038f6c99c79845c6cd320ed01e4.png)

   查看默认的信息：

   ![image-20220421162824161](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6ce7832c773ef57f5a268a50993054d2.png)

   如果自己的文档字段没有指定，那么es就会给我们默认配置字段类型!

   **扩展:**

   通过`get _cat/xxx` 可以获得当前的很多信息，例如：`GET _cat/indices?v`查看版本信息

   ![image-20220421163044142](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/549bf9be6c75d4e17eb806973812124c.png)

   还有很多命令：

   ![image-20220421163236095](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/983e677627407396eca3ca67343e1578.png)

> 修改

曾经的方法：用PUT覆盖

![image-20220421163612519](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1c1f88338df1f5a1e20ec333c0ca5d33.png)

目前的方法：用POST修改

![image-20220421164004579](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d1168643282c4ad5babc802df89c46ae.png)

> 删除索引

![image-20220421164214648](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6981af6111dcb0b2b2125d708d8bcc1a.png)

通过DELETE命令实现删除, 根据你的请求来判断是删除索引还是删除文档记录

使用RESTFUL风格是ES推荐大家使用的！

# 8. 关于文档的基本操作

> 基本操作

和索引的基本操作一致：

1、添加数据

```java
PUT run/user/1
{
  "name": "AruNi",
  "age": 20,
  "desc": "正在开心的学习ES",
  "tags": ["宅", "码农", "篮球"]
}
```

![image-20220421164918479](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b1ea5bdc2126afc3a5d65ca0ed6b15ce.png)

添加成功，再随便添加几条，id不能相同；

2、获取数据

![image-20220421165422474](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b2cfda740d3e1a7ef12440e2b7305991.png)

3、更新数据 PUT(相当于是覆盖了上一条)

![image-20220421165616222](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8454d77e8e2f4dc3ae5a1a52947144d9.png)

4、POST _update，推荐使用这种修改方式

```bash
POST run/user/1/_update
{
  "doc": {
    "name": "AruNi333"
  }
}
```

![image-20220421165942667](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e59ab52e29f51bbe594bcf46df179db8.png)

> 简单的查询搜索

```bash
GET run/user/1
```

简单的条件查询`GET _search?q=`，可以根据默认的映射规则，产生基本的查询

![image-20220421170741631](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a6c0b1d36504fdeb4f852800f6d681b9.png)

> 复杂的查询搜索select (排序, 分页, 高亮, 模糊查询, 精准查询等)

![image-20220421171221349](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3016e16515d26b301b03844bce10f59b.png)

添加一个数据后，再次查询：

![image-20220421171657327](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dd365777fc5c713ebbf9c2694751c455.png)

可以使用`_source`选择查询的结果：

![image-20220421171823034](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c355f6ea981d0e85c9caf10185b35062.png)

之后使用Java操作es，所有的方法和对象就是这里面的key

> 排序

![image-20220421172620042](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d19c9cdaab2559f9b63f5acd943de3d8.png)

> 分页查询

![image-20220421172745654](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5eaa18e3cb5412e7a958c963c669f50e.png)

数据下标还是从0开始

> 布尔值查询

- `must` 所有的条件都要符合，相当于SQL中的 `where id = 1 and name = xxx`

![image-20220421173127567](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/686b7735c8acaebf7e8106e4fa5ca729.png)

- `should` 所有的条件符合其一，相当于SQL中的 `where id = 1 or name = xxx`

![image-20220421173312097](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e91f305a05ea4935e6a6f587b1154c4d.png)

- `must_not` 反向查询

![image-20220421173504848](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7c18718352d1bda960e84963039b8bf9.png)

- `filter` 过滤器：
  - gt 大于
  - gte 大于等于
  - lt 小于
  - lte 小于等于

![image-20220421173836562](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/91c395613897b8d3819de001ca704505.png)

> 匹配多个条件

多个条件用空格隔开，只要满足其中一个就可以被查出，可以通过分值来反映出匹配的程度：

![image-20220421174402901](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fbc240e2a47ae8b702f22c8c66b640ae.png)

> 精确查询

`term` 查询是直接通过**倒排索引**指定的词条进程精确的查找的

**关于分词:**

- `term`：直接查询精确的
- `match`：会使用分词器解析 (先分析文档，然后通过分析的文档进行查询)

**两个类型:**

- text：会被分词器解析
- keyword：不会被分词器解析

下面通过`GET _analyze`来分析这两个类型：

![image-20220421175520637](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/02552dde67dbdb07b06d476fa24ae698.png)

![image-20220421175614904](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a8755b8a7c7d6e18e267fb7289b516fb.png)

下面通过查询来分析：

![image-20220421180203838](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/748a6eba26dfbe2dd72292d43033794c.png)

![image-20220421180335651](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/19ebcbc03c43eb85ad39fc47b8bfc96a.png)

> 多个值匹配的精确查询

![image-20220421180916042](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/efd725e17d9390761bd5a3cde408cce3.png)

> 高亮查询

![image-20220421181221489](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bf58a76091350cc28bac7fdb3dc8f2a9.png)

自定义搜索高亮条件：在查询的关键字添加HTML标签前后缀即可

![image-20220421181457675](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5e5a9e80ff66b4bcd0e4a16bf741b9da.png)

这些其实MySQL也可以做，只是MySQL效率比较低：

- 匹配
- 按条件匹配
- 精确匹配
- 匹配字段过滤
- 多条件查询
- 高亮查询
- 倒排查询

# 9. 集成SpringBoot

> 找文档

进入ElasticSearch官网，找到跟客户端相关的文档：

![image-20220421181909005](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/73666adc33764609b0e36c1bcf66df57.png)

点击进去，发现有很多客户端：

![image-20220421182104040](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/573d6bfcd4a15603388bd42ea1bf3a43.png)



![image-20220421182619170](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9885ff7362a6c79daca991f3821c0f9c.png)

1. 找到原生的依赖

   ```xml
   <repositories>
       <repository>
           <id>es-snapshots</id>
           <name>elasticsearch snapshot repo</name>
           <url>https://snapshots.elastic.co/maven/</url>
       </repository>
   </repositories>
   ```
   
2. 找对象

![image-20220422221230606](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/82dff9b2f7cafcae2512aa8bef2ea5c7.png)

分析这个类中的方法即可

> 配置基本的项目

新建SpringBoot项目，添加ES的支持。

**问题:** 一定要保证我们导入的依赖和我们的es版本一致

![image-20220421210915602](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7343d949da18022ba9cef94d4b089c64.png)

将本地对应的版本与此保持一致

![image-20220421211851056](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b1a06a93c15f221662d73b19e139064c.png)

> 编写ES客户端配置，注入到Spring中

```java
@Configuration
public class ElasticSearchClientConfig {

    @Bean
    public RestHighLevelClient restHighLevelClient() {
        RestHighLevelClient client = new RestHighLevelClient(
            RestClient.builder(
                new HttpHost("127.0.0.1", 9200, "http")));
        return client;
    }
}
```

> 具体的API测试

1. 创建索引
2. 判断索引是否存在
3. 删除索引
4. 创建文档
5. CRUD文档

**示例**：

```java
@SpringBootTest
class EsApiApplicationTests {

    @Autowired
    private RestHighLevelClient client;

    // 创建索引 Request == PUT run_index
    @Test
    void testCreateIndex() throws IOException {
        // 1.创建索引请求
        CreateIndexRequest request = new CreateIndexRequest("run_index");
        // 2.客户端执行请求
        CreateIndexResponse createIndexResponse = client.indices()
                .create(request, RequestOptions.DEFAULT);
        System.out.println(createIndexResponse);
    }

    // 获取索引，判断其是否存在
    @Test
    void testExistIndex() throws IOException {
        GetIndexRequest request = new GetIndexRequest("run_index");
        boolean exists = client.indices().exists(request, RequestOptions.DEFAULT);
        System.out.println(exists);     // true
    }

    // 删除索引
    @Test
    void testDeleteIndex() throws IOException {
        DeleteIndexRequest request = new DeleteIndexRequest("run_index");
        AcknowledgedResponse delete = client.indices().delete(request, RequestOptions.DEFAULT);
        System.out.println(delete.isAcknowledged());    // true
    }

    // 添加文档; User实体类中只有name和age两个属性
    @Test
    void testAddDocument() throws IOException {
        // 1.创建对象
        User user = new User("AruNi", 18);
        // 2.创建请求
        IndexRequest request = new IndexRequest("run_index");

        // 3.规则   PUT run_index/_doc/1
        request.id("1");
        request.timeout(TimeValue.timeValueSeconds(1));     // 过期规则
        request.timeout("1s");  // 同上

        // 4.将数据放入请求，先转为json类型(使用fastjson)
        request.source(JSON.toJSONString(user), XContentType.JSON);

        // 5.客户端发送请求，获取响应的结果
        IndexResponse response = client.index(request, RequestOptions.DEFAULT);

        // 输出具体的json信息
        System.out.println(response.toString());
        // IndexResponse[index=run_index,type=_doc,id=1,version=1,result=created,seqNo=0,primaryTerm=1,shards={"total":2,"successful":1,"failed":0}]

        // 输出对应命令返回的状态 CREATED
        System.out.println(response.status());      // CREATED
    }

    // 获取文档，判断其是否存在，不获取具体信息
    @Test
    void testIsExists() throws IOException {
        GetRequest getRequest = new GetRequest("run_index", "1");
        // 不获取返回的_source的上下文了
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        // 排序规则，不选择
        getRequest.storedFields("_none_");

        boolean exists = client.exists(getRequest, RequestOptions.DEFAULT);
        System.out.println(exists);     // true
    }

    // 获取文档的信息
    @Test
    void testGetDocument() throws IOException {
        GetRequest getRequest = new GetRequest("run_index", "1");
        GetResponse getResponse = client.get(getRequest, RequestOptions.DEFAULT);
        // 打印文档的内容    {"age":18,"name":"AruNi"}
        System.out.println(getResponse.getSourceAsString());
        // 返回的全部内容和命令时一样的
        System.out.println(getResponse);
        // {"_index":"run_index","_type":"_doc","_id":"1","_version":1,"_seq_no":0,"_primary_term":1,"found":true,"_source":{"age":18,"name":"AruNi"}}
    }

    // 更新文档信息
    @Test
    void testUpdateRequest() throws IOException {
        UpdateRequest updateRequest = new UpdateRequest("run_index", "1");
        updateRequest.timeout("1s");

        User user = new User("张三", 23);
        updateRequest.doc(JSON.toJSONString(user), XContentType.JSON);

        UpdateResponse updateResponse = client.update(updateRequest, RequestOptions.DEFAULT);
        // 输入更新的结果：OK
        System.out.println(updateResponse.status());
    }

    // 删除文档
    @Test
    void testDeleteRequest() throws IOException {
        DeleteRequest deleteRequest = new DeleteRequest("run_index", "1");
        deleteRequest.timeout("1s");

        DeleteResponse deleteResponse = client.delete(deleteRequest, RequestOptions.DEFAULT);
        System.out.println(deleteResponse.status());  // OK
    }

    // 特殊情况：实际项目中一般会批量插入/更新/删除数据
    @Test
    void testBulkRequest() throws IOException {
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout("10s");

        ArrayList<User> userList = new ArrayList<>();
        userList.add(new User("小一", 18));
        userList.add(new User("小三", 19));
        userList.add(new User("小六", 20));
        userList.add(new User("小九", 21));

        // 批量处理请求
        for (int i = 0; i < userList.size(); i++) {
            // 批量更新/删除类似，修改对应的Request即可，这里只演示插入-IndexRequest
            bulkRequest.add(new IndexRequest("run_index")
                    .id("" + (i + 1))   // 不加id会默认生成随机id
                    .source(JSON.toJSONString(userList.get(i)), XContentType.JSON));
        }
        BulkResponse bulkResponse = client.bulk(bulkRequest, RequestOptions.DEFAULT);
        // 是否有失败，返回false表示都没有失败，即全部插入成功了
        System.out.println(bulkResponse.hasFailures());     // false
    }

    /**
     * 搜索
     * SearchRequest 搜索请求
     * SearchSourceBuilder 条件构造
     *
     * xxxQueryBuilder  搜索条件
     * TermQueryBuilder 精确查询
     * MatchAllQueryBuilder 匹配所有
     *
     * HighLightBuilder 构建高亮
     * @throws IOException
     */
    @Test
    void testSearch() throws IOException {
        // 1.创建搜索请求
        SearchRequest searchRequest = new SearchRequest("run_index");
        // 2.创建搜索构建器，SearchSourceBuilder中可以选择很多搜索功能(高亮/排序/分页etc)
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        // 3.选择搜索条件，可以使用QueryBuilders 工具类来实现
        // QueryBuilders.termQuery   精确匹配
        // QueryBuilders.matchAllQuery   匹配所有
        // termQuery使用中文会失效，可以使用name.keyword，将中文名字当成一个词，keyword不会被解析
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("name.keyword", "小一");
//      MatchAllQueryBuilder matchAllQueryBuilder = QueryBuilders.matchAllQuery();

        // 4.将搜索条件放入搜索构建器中，进行搜索
        sourceBuilder.query(termQueryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));

        // 5.将搜索构建器放入请求中
        searchRequest.source(sourceBuilder);

        // 6. 执行请求，获取响应结果
        SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);

        // 所有的结果都封装在Hits里面
        System.out.println(JSON.toJSONString(searchResponse.getHits()));

        System.out.println("==================搜索的所有结果=========================");
        for (SearchHit documentFields : searchResponse.getHits()) {
            System.out.println(documentFields.getSourceAsMap());
            // 只有一条结果，输出：{name=小一, age=18}
        }
    }
}
```

# 10. JD搜索实战

防京东搜索，先将京东的所有数据爬取下来，然后用ES实现搜索功能！

## 10.1 项目搭建

1. 新建SpringBoot项目，添加Thymeleaf、ES等相关依赖。

2. 修改ES默认版本为7.6.1，使之与本地版本一致

   ```xml
   <properties>
       <java.version>1.8</java.version>
       <elasticsearch.version>7.6.1</elasticsearch.version>
   </properties>
   ```

3. 导入相关静态资源

   ![image-20220422182834474](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ce668c4b091f7985628333b8a632d712.png)

4. 新建`IndexController.java`，编写跳转到首页的请求：

   ```java
   @Controller
   public class IndexController {
       @GetMapping({"/", "/index"})
       public String index() {
           return "index";
       }
   }
   ```

5. 启动项目，访问`9090`端口：

   ![image-20220422183251521](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6dcbc2e3c508053a0bd02e767c4a1827.png)



## 10.2 爬取数据

爬取数据：获取请求返回的页面信息，筛选出我们想要的数据就可以了。

首先需要知道京东的商品列表的存放在哪个div里面：

![image-20220422184655036](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f9ba51bb80b61d7a4fdd7745a91e7e9a.png)

其次查看商品的属性值：

![image-20220422190225855](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f1d1930a6e5adbde530fe6064813efea.png)

1. 导入jsoup依赖，用于解析网页

   ```xml
   <!--解析网页jsoup-->
   <dependency>
       <groupId>org.jsoup</groupId>
       <artifactId>jsoup</artifactId>
       <version>1.14.3</version>
   </dependency>
   ```

2. 创建utils包并建立`HtmlParseUtil.java` 爬取测试：

   ```java
   public class HtmlParseUtil {
       public static void main(String[] args) throws IOException {
           // 获取请求：https://search.jd.com/Search?keyword=java
           String url = "https://search.jd.com/Search?keyword=java";
           // 解析网页（jsoup返回的Document就是浏览器的Document对象）
           Document document = Jsoup.parse(new URL(url), 30000);
           // 获取JD商品列表的id，所有在js里面使用的方法在这里都可以使用
           Element element = document.getElementById("J_goodsList");
           // 获取所有的li标签
           Elements elements = element.getElementsByTag("li");
   
           // 遍历所有的li标签，获取标签中的数据
           int count = 0;  // 统计数量
           for (Element el : elements) {
               count++;
               // 注意: 直接attr("src")爬不出来，因为京东使用了img懒加载
               String img = el.getElementsByTag("img").eq(0).attr("data-lazy-img");
               String price = el.getElementsByClass("p-price").eq(0).text();
               String title = el.getElementsByClass("p-name").eq(0).text();
               String shopName = el.getElementsByClass("p-shopnum").eq(0).text();
   
               System.out.println("==============================");
               System.out.println(img);
               System.out.println(price);
               System.out.println(title);
               System.out.println(shopName);
           }
           System.out.println("共爬取数据条数：" + count);
       }
   }
   ```

3. 测试爬取结果：

   ![image-20220422192345354](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/959678f2c94f64fedbf32380d47f8186.png)





**获取结果没有问题后，将此方法封装为一个工具类使用**：

1. 创建pojo实体类`Content.java`：

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Content {
       private String title;
       private String img;
       private String price;
       // 可以自行添加属性
   }
   ```

2. 在`HtmlParseUtils.java`类中封装一个`parseJD`方法：

   ```java
   public class HtmlParseUtil {
   
       // 测试
       public static void main(String[] args) throws IOException {
           new HtmlParseUtil().parseJD("数据库").forEach(System.out::println);
       }
   
       /**
        * 根据关键字查询京东的商品信息
        * @param keywords
        * @return
        * @throws IOException
        */
       public List<Content> parseJD(String keywords) throws IOException {
           String url = "https://search.jd.com/Search?keyword=" + keywords;
           Document document = Jsoup.parse(new URL(url), 30000);
           Element element = document.getElementById("J_goodsList");
           Elements elements = element.getElementsByTag("li");
   
           List<Content> goodList = new ArrayList<>();
   
           for (Element el : elements) {
               String img = el.getElementsByTag("img").eq(0).attr("data-lazy-img");
               String price = el.getElementsByClass("p-price").eq(0).text();
               String title = el.getElementsByClass("p-name").eq(0).text();
   
               Content content = new Content();
               content.setImg(img);
               content.setPrice(price);
               content.setTitle(title);
               goodList.add(content);
           }
           return goodList;
       }
       
   }
   ```

3. 编写业务层代码 (这里就不写接口了)`ContentService.java`，首先完成一个方法让爬取的数据存入ES中：

   ```java
   @Service
   public class ContentService {
   
       @Autowired
       private RestHighLevelClient client;
   
       /**
        * 根据关键词搜索京东中的数据，再将数据存入ES中
        * @param keyword
        * @return
        * @throws IOException
        */
       public boolean parseContent(String keyword) throws IOException {
           List<Content> contents = new HtmlParseUtil().parseJD(keyword);
           // 把查询到的数据放入ES中
           BulkRequest bulkRequest = new BulkRequest();
           bulkRequest.timeout("2s");
   
           for (int i = 0; i < contents.size(); i++) {
               bulkRequest.add(new IndexRequest("jd_goods")
                       .source(JSON.toJSONString(contents.get(i)), XContentType.JSON));
           }
           BulkResponse bulkResponse = client.bulk(bulkRequest, RequestOptions.DEFAULT);
           return !bulkResponse.hasFailures();
       }
   }
   ```

4. 在Controller包下建立`ContentController.java`：

   ```java
   @RestController
   public class ContentController {
       
       @Autowired
       private ContentService contentService;
       
       @GetMapping("/parse/{keyword}")
       public Boolean parse(@PathVariable("keyword") String keyword) throws IOException {
           return contentService.parseContent(keyword);
       }
   }
   ```

5. 启动Springboot项目，测试访问是否能将数据爬取至ES

   ![image-20220422194645044](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0bd37f392005c18a8dee697311380837.png)

6. 在ES中查看：

   ![image-20220422194807491](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/38e98d648674be22f3ff01e82f5311a2.png)

## 10.3 实现搜索功能

1. 在`ContentService.java`中添加搜索的方法：

   ```java
       /**
        * 从ES中获取数据
        * @param keyword 关键词
        * @param pageNo 起始位置
        * @param pageSize 页面大小
        * @return
        * @throws IOException
        */
       public List<Map<String, Object>> searchPage(String keyword, int pageNo, int pageSize) throws IOException {
           if (pageNo <= 1) pageNo = 1;
           if (pageSize <=1 ) pageSize = 1;
   
           // 搜索请求
           SearchRequest searchRequest = new SearchRequest("jd_goods");
   
           // 搜索构造器
           SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
   
           // 用搜索构造器设置分页
           sourceBuilder.from(pageNo).size(pageSize);
   
           // 搜索条件，精准匹配，
           TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", keyword);
   
           // 将搜索条件放入搜索构建器中，进行搜索
           sourceBuilder.query(termQueryBuilder);
           sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
   
           // 将搜索构建器放入搜索请求中
           searchRequest.source(sourceBuilder);
   
           // 执行请求，获取响应的结果
           SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
   
           // 解析结果
           List<Map<String, Object>> list = new ArrayList<>();
           // searchResponse.getHits()获取搜索的总结果，再.getHits()获取单个结果
           for (SearchHit documentFields : searchResponse.getHits().getHits()) {
               list.add(documentFields.getSourceAsMap());
           }
   
           return list;
       }
   ```

2. 在`ContentController.java`中添加搜索请求，使用Restful风格：

   ```java
       @GetMapping("/search/{keyword}/{pageNo}/{pageSize}")
       public List<Map<String, Object>> search(@PathVariable("keyword") String keyword, @PathVariable("pageNo") int pageNo, @PathVariable("pageSize") int pageSize) throws IOException {
           return contentService.searchPage(keyword, pageNo, pageSize);
       }
   ```
   
3. 访问http://127.0.0.1:9090/search/java/1/10 (查询Java 并从第一条显示到第十条)

   ![image-20220422205602097](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5d8b421fd5156746254d86d5c33aaea5.png)

到此数据的爬取和搜索都没有问题了，下面就要开始前后端的分离工作了

## 10.4 前后端分离

1. 前端需要接受数据，用vue接受数据：

   ```vue
   <!--前端使用vue完成前后端分离-->
   <script th:src="@{/js/axios.min.js}"></script>
   <script th:src="@{/js/vue.min.js}"></script>
   <script>
       new Vue({
           el: '#app',
           data: {
               keyword: '',  		// 搜索的关键字
               result: []  		// 搜索的结果
           },
           methods: {
               searchKey() {
                   var keyword = this.keyword
                   // 对接后端的接口
                   axios.get('search/' + keyword + '/1/10').then(response => {
                       //console.log(response);
                       this.result = response.data;	// 绑定数据
                   })
               }
           }
       });
   </script>
   ```
   
2. 然后为按钮绑定点击事件

   ![image-20220422211316938](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3d59b45552f50af2bc75e31a31c63100.png)

3. 用vue给前端传递数据

   ![image-20220422212144944](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/29455977b413c31de2c468afefe2d8bf.png)

4. 访问[127.0.0.1:9090]() 搜索java进行测试：

   ![image-20220422212308681](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e67a1b77caa9fc247cb0c71d759c5ee4.png)


## 10.5 搜索高亮

1. 在`ContentService.java`中添加一个高亮搜索的方法：

   ```java
       /**
        * 从ES中获取数据，并且高亮显示title
        * @param keyword 关键词
        * @param pageNo 起始位置
        * @param pageSize 页面大小
        * @return
        * @throws IOException
        */
       public List<Map<String, Object>> searchPageHighlight(String keyword, int pageNo, int pageSize) throws IOException {
           if (pageNo <= 1) pageNo = 1;
           if (pageSize <=1 ) pageSize = 1;
   
           // 搜索请求
           SearchRequest searchRequest = new SearchRequest("jd_goods");
   
           // 搜索构造器
           SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
   
           // 用搜索构造器设置分页
           sourceBuilder.from(pageNo).size(pageSize);
   
           // 搜索条件，精准匹配，
           TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("title", keyword);
   
           // ===============高亮设置==============
           // 获取高亮构造器
           HighlightBuilder highlightBuilder = new HighlightBuilder();
   
           highlightBuilder.field("title");        // 设置需要高亮的字段
           highlightBuilder.requireFieldMatch(false);      // 不需要多个字段高亮
           highlightBuilder.preTags("<span style='color: red'>");      // 前缀
           highlightBuilder.postTags("</span>");       // 后缀
   
           // 将搜索条件放入搜索构建器中，进行搜索
           sourceBuilder.query(termQueryBuilder);
           sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
   
           // 将搜索构建器放入搜索请求中
           searchRequest.source(sourceBuilder);
   
           // 执行请求，获取响应的结果
           SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
   
           // 解析结果
           List<Map<String, Object>> list = new ArrayList<>();
           // searchResponse.getHits()获取搜索的总结果，再.getHits()获取单个结果
           for (SearchHit documentFields : searchResponse.getHits().getHits()) {
   
               // 获取高亮的字段
               Map<String, HighlightField> highlightFields = documentFields.getHighlightFields();
               // 得到需要高亮的字段，我们只设置了title
               HighlightField title = highlightFields.get("title");
               // 得到原来的结果
               Map<String, Object> sourceAsMap = documentFields.getSourceAsMap();
   
               // 解析高亮的字段，将原来的title替换成高亮的title
               if (title != null) {
                   Text[] fragments = title.fragments();
                   String new_title = "";
                   for (Text text : fragments) {
                       new_title += text;
                   }
                   // 用高亮字段替换掉原来的
                   sourceAsMap.put("title", new_title);
               }
               // 再将有高亮title的结果存入集合中
               list.add(sourceAsMap);
           }
   
           return list;
       }

2. 在`ContentController.java`中添加响应的请求：

   ```java
       @GetMapping("/searchPageHighlight/{keyword}/{pageNo}/{pageSize}")
       public List<Map<String, Object>> searchPageHighlight(@PathVariable("keyword") String keyword, @PathVariable("pageNo") int pageNo, @PathVariable("pageSize") int pageSize) throws IOException {
           return contentService.searchPageHighlight(keyword, pageNo, pageSize);
       }
   ```
   
3. js中的axios请求也要改成`searchPageHighlight`：

   ```js
   axios.get('searchPageHighlight/' + keyword + '/0/20').then(response => {
       console.log(response);
       this.results = response.data;
   })
   ```

4. 访问测试：

   ![image-20220422220553205](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dbbd67d63e7908772532c27e5c8a5b86.png)

   发现不是我们需要的红色高亮，而是标签样式显现出来了，这是因为前端没有解析HTML；

5. 解决问题，Vue给了很好的解决办法，只需使用`v-html`即可解析HTML：

   ![image-20220422220732597](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e69dc05d45443a8e66842897185535b2.png)

6. 再次访问测试：

   ![image-20220422220804326](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/44c77a44fc9eb8980c6026a9415e34dd.png)
