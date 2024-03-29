## 1. Redis 是单线程吗？

我们经常说 Redis 是单线程的，但实际上只是 **网络 IO** 和 **对键值对的读写** 是 **单线程完成**。而其他功能，例如 **持久化、异步删除、集群数据同步** 等，是由 **额外的线程** 来完成的。

而且 **在 Redis 6.0 时，网络 IO 也使用了多线程来处理**，后面会详细介绍。

## 2. 为什么用单线程能这么快？

既然 Redis 在处理客户端的请求时只有一个线程，那为什么它还能在 1 秒内处理数 10 万的请求呢？

首先来看看为什么要使用单线程，而不是多线程？

我们知道，一个系统中如果 **引入了多线程**，为了防止多线程的 **并发访问安全问题**，那么肯定要涉及到 **同步机制**，这种并发访问控制机制实现起来复杂不说，**还会带来额外的开销**。

如果只是简单的通过加一个粗粒度的锁来保证，即使是多线程，大部分也都是在等待，白白浪费了资源，吞吐量也不会随着线程数的增加而提高。

所以，Redis 直接采用单线程来处理请求，而无需再设计一个复杂的并发控制系统。

单线程的处理能力是要比多线程差很多的，Redis 之所以使用单线程还快，是它多方面设计的一个综合结果。

其中一个很重要的是 Redis 大部分操作都是 **在内存上完成**，另外，我们在 [Redis 数据类型](https://code.0x3f4.run/backend/database/redis/data_structure/redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.html) 中也说到过，它采用了很多 **高效的数据结构**。

还有一个重要因素，就是 Redis 采用了 **多路复用** 机制，**在网络 IO 操作中能并发处理大量客户端请求**，从而实现高吞吐率。

## 3. 基本 IO 模型与阻塞点

Redis 是采用 **单线程进行网络 IO**（6.0 之前），如果 **线程被阻塞了**，那就没有线程工作了，也就 **无法进行多路复用了**，所以 Redis 需要尤其关注网络 IO 中的 **阻塞点**。

下面来看看最基本的 socket 网络 IO 模型：

1. 服务端先通过 `bind` 绑定一个端口，然后调用 `listen` 监听客户端发来的请求；
2. 客户端发起请求后，服务端通过 `accept` 与其建立连接；
3. 服务端调用 `recv` 从 socket 中读取请求；
4. 最后服务端处理完成后，调用 `send` 往 socket 中写回数据。

其中，**bind、listen、accept、recv、send 是属于网络 IO 处理的**，这里面有两个 **阻塞点**，分别是 **accept 和 recv**：

- 当 Redis **监听到一个客户端有连接请求，但一直没成功建立起连接时**，会阻塞在 accept 函数；
- 当 Redis 通过 recv 读取数据时，如果 **数据一直没有到达**，会阻塞在 recv 函数。

一旦阻塞，Redis 就会阻塞，无法处理其他客户端的请求，导致吞吐量大大降低。不过，socket 网络模型本身就支持非阻塞模式。

**Redis 使用了基于多路复用的高性能 IO 模型**，它就是 Linux 中的一个非阻塞的 IO 模型。

## 4. 多路复用 IO 模型

**多路复用是指一个线程处理多个 IO 流**，也就是 Linux 中常见的 **select/epoll 机制**。该机制 **允许内核中同时存在多个监听套接字 listen 和已连接套接字 accept，内核会监听这些套接字上的连接请求或数据请求，一旦有请求到达，就会交给 Redis 线程来处理**。

Redis 的多路复用 IO 模型如下图所示：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/https%2Fstatic001.geekbang.org%2Fresource%2Fimage%2F00%2Fea-2023_12_29-1703839052.jpeg)

其中 FD 就是套接字，epoll 机制会让内核监听这些套接字，**Redis 线程不会阻塞在某个套接字上，因为它不直接跟套接字打交道**。

**为了在请求到达时能通知到 Redis 线程，select/epoll 提供了基于事件的回调机制**，针对不同的事件，调用相应的处理函数。

这个回调机制其实就是 **select/epoll 一旦检测到 FD 上有请求到达时，就会触发相应的事件，这些事件会被放入一个事件队列，Redis 线程来处理该事件队列（调用事件的处理函数）即可**。

这样 Redis 就无需一直轮询是否有请求到达，避免 CPU 资源的消耗。

比如，一个 **连接请求和读取数据请求**，分别对应 Accept 事件和 Read 事件，Redis 会对这两个事件注册 accept 和 get 回调函数。这样当 Linux 内核监听到有连接请求或读取数据请求到达时，就会触发 Accept 事件和 Read 事件，接着内核就会调用 Redis 对应的 accept 和 get 函数进行处理。

## 5. 单线程处理网络 IO 的性能瓶颈

Redis 单线程处理网络 IO 也有性能瓶颈，比如：

- **请求本身很耗时**，例如操作 bigkey、获取全量数据、内存上限后淘汰 key、AOF 开启 always 模式（每次都要刷盘）等；
- **并发量非常大时**，虽然采用多路复用，但 **读写客户端数据依旧是同步 IO (涉及内核缓冲区的数据拷贝)**，依然只能有一个线程操作。

对于第一个瓶颈，需要业务人员去规避，而对于第二个瓶颈，Redis 在 6.0 中推出了多线程处理网络 IO，可以针对客户端的读写进行多线程处理。

## 5. Redis 6.0 中的多线程网络 IO

在 Redis 6.0 前，**从网络 IO 处理到实际的数据读写** 都是由主线程来完成的。随着网络硬件的性能提升，Redis 的 **性能瓶颈** 有时会出现在 **网络 IO 的处理上**，**单个主线程处理网络请求的速度跟不上网络硬件的速度**。

从 Redis 6.0 开始，**采用了多个 IO 线程来处理网络请求，提高网络请求处理的并行度**。不过，多 IO 线程只是用来处理网络请求，对于 **读写命令**，依旧是使用 **单线程** 来处理。

下面来看看主线程和 IO 线程具体是怎么协作完成请求的。

**一、服务端和客户端建立 Socket 连接，并分配处理线程**

主线程负责接收建立连接请求，**连接建立后会把 Socket 放入 全局等待队列** 中，接着 **主线程就会轮询地把 Socket 连接分配给 IO 线程**。

**二、IO 线程读取并解析请求**

主线程把 Socket 分配给 IO 线程后，就会进入 **阻塞** 状态，**等待 IO 线程完成客户端请求的读取和解析**。这个过程有 **多个 IO 线程并行处理**，很快就能完成。

**三、主线程处理请求**

IO 线程解析完请求后，主线程就可以真正执行命令操作了，这个过程是单线程的。过程如下：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/https%2Fstatic001.geekbang.org%2Fresource%2Fimage%2F58%2Fcd-2023_12_29-1703844426.jpeg)

**四、IO 线程回写 Socket，主线程清空全局队列**

主线程处理完操作后，会把结果写入缓冲区，然后 **主线程会阻塞等待 IO 线程把缓冲区的结果回写到 Socket 中**，并返回给客户端。这个过程也是多 IO 线程并行处理，会很快。

**IO 线程回写 Socket 后，主线程会清空全局队列**，等待后续请求。

过程如下：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/https%2Fstatic001.geekbang.org%2Fresource%2Fimage%2F2e%2F1b-2023_12_29-1703844623.jpeg)

可以发现，**IO 线程主要完成请求的去读和解析**，解析完后主线程操作，接着 **IO 线程将结果写回 Socket，返回给客户端**。



::: info 如何开启 IO 多线程？

IO 多线程机制 **默认是关闭的**，需要在 redis.conf 中配置：

```sh
io-threads-do-reads yes

io-threads 6
```

一般来说，线程数要小于 CPU 核数，对于 8 核的机器，建议配置 6 个 IO 线程。

如果发现 Redis 实例的 **CPU 开销不大，吞吐量却较低**，可能就是在网络上遇到了瓶颈，可以考虑使用 IO 多线程机制，加速网络处理。

:::

## 6. 参考文章

- 《Redis 核心技术与实战》



