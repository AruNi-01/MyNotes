zSet 的 ZSCORE 命令支持通过 key 获取指定 member 的 score（复杂度为 `O(1)`），也能通过 ZRANGEBYSCORE 按照 score 获取指定范围的 member（复杂度为 `O(logN)`）。

其实这与 zSet 的底层数据结构有关，翻开 zSet 的源码，可以看到它是由 **dict 哈希表和 zsl 跳表** 组合实现的：

```c
typedef struct zset {
    dict *dict;
    zskiplist *zsl;
} zset;
```

所以，我们在往 zSet 中插入/更新数据时，Redis 会分别在跳表和哈希表中都进行插入/更新。

在哈希表中，**key 就是 member，value 是一个指针，指向该 member 在跳表中的 score**，所以可以快速定位到一个 member 的 score。

在 Redis 中，跳表节点的定义如下：

```c
typedef struct zskiplistNode {
    // Sorted Set中的元素
    sds ele;
    // 元素权重值
    double score;
    // 后向指针
    struct zskiplistNode *backward;
    // 节点的level数组，保存每层上的前向指针和跨度
    struct zskiplistLevel {
        struct zskiplistNode *forward;
        unsigned long span;
    } level[];
} zskiplistNode;
```

可以看到，除了保存数据、score 等信息之外，还有一个 level 数组，里面记录了该节点的前向指针（用于倒序查询），以及跨度 span，span 的值是两节点之间的节点数 + 1，所以可以通过这个 span 来获取某元素在集合中的排名（即 `ZRANK` 命令）。

下面再来看看跳表结构体的定义：

```c
typedef struct zskiplist {
    struct zskiplistNode *header, *tail;
    unsigned long length;
    int level;
} zskiplist;
```

跳表结构体中定义了头尾指针、跳表的长度和层数，这样在查询时直接从头/尾指针开始访问即可。

完整的跳表结构如下：

![image-20230711224822896](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307120018340.png)

可以发现，在查询时，从最顶层根据跳表的特性往下层寻找，**当上下相邻层的结点数之比为 1:2 时，就把链表的 O(N) 复杂度降到了 O(logN)**，类似二分查找。

那么 Redis 是如何维持上下相邻层的结点数之比为 1:2 的呢？

如果要强行维持，那么在势必会影响插入/删除性能，因为涉及到其他节点层数的调整。

来看看 Redis 是怎么设计的，其中 `zslRandomLevel()` 是用来生成节点的层数的：

```c
#define ZSKIPLIST_MAXLEVEL 64  // 最大层数为64
#define ZSKIPLIST_P 0.25       // 随机数的值为0.25
int zslRandomLevel(void) {
    // 初始化层为1
    int level = 1;
    while ((random()&0xFFFF) < (ZSKIPLIST_P * 0xFFFF))
        level += 1;
    return (level<ZSKIPLIST_MAXLEVEL) ? level : ZSKIPLIST_MAXLEVEL;
}
```

可以发现 Redis 是 **随机生成每一个节点的层数**，具体来说：

- 层数先初始化为 1；
- 然后生成一个随机数，如果小于 ZSKIPLIST_P（节点增加一层的概率，值为 0.25），那么层数就 +1；
- 所以节点的层数每增加一层的概率不超过 25%，从概率学的角度来说，这能保证上下相邻层的结点数之比约为 1:2。

这样一来，当向跳表中插入一个节点时，就只需要修改前后结点的指针指向即可。





