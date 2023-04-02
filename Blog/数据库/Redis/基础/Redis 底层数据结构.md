



::: info 前言

Redis 之所以快，除了它是基于内存的数据库外，还有一个重要的因素，就是它底层的 **数据结构**。

所以，想知道 Redis 为什么快的秘诀之一，就是去探究探究它底层的数据结构。

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

不同的编码对应着不同的底层数据结构，这就意味着可以 **根据不同的场景来选择不同的数据结构**，从而达到更加高效的数据存取，因为每种数据结构都有各自的优势与劣势。当你学习完这些数据结构后，就知道它们各自的使用场景了。

## 1. Redis 数据结构有哪些

Redis 中的 **数据结构** 不是指 String、List、Hash、Set、zSet 等这些基本数据类型，而是指这些 **数据类型的底层实现**。

下面通过一张数据类型和数据结构的对应关系图，了解一下五种基本数据类型的底层实现：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202302232032985.png)

可以看见，之前的双向链表和压缩列表分别被 quicklist 和 listpack 代替了，至于为什么要做这个改变，看下去你就知道了。

## 2. 简单动态字符串 SDS

Redis 是用 C 语言实现的，但是它并没有直接使用 C 语言的 `char*` 字符数组来作为 Redis 字符串的底层实现，而是自己构建了一种 **简单动态字符串**（simple dynamic string，SDS）。

Redis 这么追求高效，不使用 C 语句的字符串肯定是它有一些缺陷，所以得先了解它有什么缺陷，才能知道 Redis 的 SDS 为什么要这么设计。

### 2.1 C 字符串有什么缺陷















