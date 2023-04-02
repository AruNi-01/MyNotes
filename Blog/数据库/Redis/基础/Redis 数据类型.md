

::: info 前言

Redis 并没有直接使用数据结构来实现键值对数据库，而是 **基于数据结构创建了一些数据类型**，简单的五种数据类型分别是：**字符串 String、列表 List、哈希 Hash、集合 Set、有序集合 zSet**。此外还有一些高级的数据类型，例如 BitMap、HyperLogLog、GEO、Stream。

> Redis 中的 **数据结构** 包括：简单动态字符串 SDS、链表 List、压缩列表 ziplist、哈希表 hash、整数集合 intset、跳表 zskiplist、quicklist、listpack。本章主要是讲解数据类型，而不是底层实现的数据结构。

:::

## > 前置知识：redisObject

Redis 使用 **redisObject 结构体表示各种数据结构**（Redis 种把各种数据类型看成不同的对象），redisObject 结构体中的属性如下：

```c
typedef struct redisObject {
    // 类型
    unsigned type:4;
    
    // 编码
    unsigned encoding:4;
    
    // 指向 type 类型底层实现（数据结构）的指针
    void *ptr;
} robj;
```

接下来分别简单的介绍一下这三个属性。

::: info 类型（type）

**type 属性记录了对象（数据结构）的类型**，即 String、List、Hash、Set、zSet。

在 Redis 种，key 总是 String 类型，而 value 可以是任意一种对象。

我们也可以使用 `TYPE key` 命令来查看该 key 对应 **value 的类型**。

:::

::: info 编码（encoding）和 底层实现的指针（ptr）

**encoding 属性记录了对象的编码**，也就是 **这个对象使用了什么数据结构作为底层实现**，而 **ptr 指针则指向了这个底层的数据结构**。

Redis 中每种对象都至少使用了 2 种不同的编码，如下图列出了每种类型的对象可以使用的编码：

![image-20230223185531411](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202302231858184.png)

Redis 为对象提供至少 2 种不同的编码，Redis 为什么要这么做呢？

不同的编码对应着不同的底层数据结构，这就意味着可以 **根据不同的场景来选择不同的数据结构**，从而达到更加高效的数据存取，因为每种数据结构都有各自的优势与劣势。

> Redis 底层的数据结构本篇文章就不细讲了。

:::

知道了 redisObject 后，下面开始本篇文章的正题。

## 1. 字符串 String

String 是使用频率最高的数据类型，除了 key 总是使用 String 外，value 使用 String 的场景也非常多。

需要注意的是，String 不仅仅是存储字符串，也可以存储数字（整数或浮点数），整数对应到编码里的 `REDIS_ENCODING_INT`，此时的 ptr 的类型就从 `void*` 转换成了 `long`，浮点数和字符串都使用。

例如，使用一个 SET 命令将 value 设置为一个数字 `SET number 10086`，那么这个 value 的字符串对象如下图所示：

![image-20230223195417752](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202302231954030.png)

























