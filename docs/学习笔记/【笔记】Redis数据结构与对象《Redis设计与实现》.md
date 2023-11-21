---
password: ""
icon: ""
date: "2021-10-25"
type: Post
category: 学习笔记
slug: redis-data-structure
tags:
  - Redis
  - 数据库
  - 工具
summary: Redis的数据结构分为顶层数据结构和底层数据结构，我们来根据《Redis设计与实现》尝试剖析下。
title: 【笔记】Redis数据结构与对象《Redis设计与实现》
status: Published
cover: "https://images.unsplash.com/photo-1569396116180-210c182bedb8?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 76048781-d532-4ea6-a0ac-00f84cfd56ca
updated: "2023-08-18 12:50:00"
---

> 😀 Redis（Remote Dictionary Server）是一种开源的内存数据结构存储系统，它提供了多种灵活且高效的数据结构，使得开发人员能够在应用程序中快速存储和检索数据。Redis 的数据结构不仅仅是简单的键值对，它还支持更复杂的数据结构，如字符串、哈希表、列表、集合和有序集合等。这些数据结构不仅可以满足常见的数据存储需求，还可以支持更高级的操作，如排序、计数、范围查询等。

# 一、顶层数据结构

## 1.1 常用的基本结构

### String

Redis 字符串存储字节序列，包括文本、序列化对象和二进制数组。 因此，字符串是最基本的 Redis 数据类型。 它们通常用于缓存，但它们支持额外的功能，让您也可以实现计数器并执行按位操作。

> 💡 By default, a single Redis string can be a maximum of 512 MB.

**常用命令：**

获取或者设置字符串

- SET
- SETNX
- GET
- MGET

计数器

- INCRBY
- INCR
- DECR
- DECRBY

### List

用于实现队列或者栈

> 💡 The max length of a Redis list is 2^32 - 1 (4,294,967,295) elements.

**常用命令**

- LPUSH
- LPOP
- LLEN
- LMOVE
- LTRIM

### Set

无序无重复集合。

场景：去重、集合操作，抽奖

> 💡 The max size of a Redis set is 2^32 - 1 (4,294,967,295) members.

**常用命令**

- SADD
- SREM
- SISMEMBER 测试一个 String 是否是 set 成员
- SINTER 返回两个 set 的交集
- SCARD 返回 set 数量

SMEMBER 在 set 数量比较大的情况下慎用，考虑使用 SSCAN 用于替换

### Hash

用于记录键值对。也可以用于基础对象以及它的计数器，或者其他。

> 💡 Every hash can store up to 4,294,967,295 (2^32 - 1) field-value pairs. In practice, your hashes are limited only by the overall memory on the VMs hosting your Redis deployment.

**常用命令**

- HSET
- HGET
- HMGET
- HINCREBY

HKEYS、HVALS、HGETALL O(N) 复杂度

### Sorted set

有序 Set，带有一个权重的值。

场景：排行榜、限速器

**常用命令**

- ZADD
- ZRANGE
- ZRANK 获取 zset 中某个键的排行，正序（低到高）
- ZREVRANK 获取 zset 中某个键的排行，倒序 （高到低）

## 1.2 额外的基本结构

**HyperLogLog**

**Bitmap**

**Geospatial indexe**

# 二、底层数据结构

SDS、双端链表、字典、压缩列表、整数集合

### SDS 与 C 字符串的区别

1. 常数复杂度获取字符串长度
2. 杜绝缓冲区溢出
3. 减少修改字符串时带来的内存重新分配次数
4. 二进制安全
5. 兼容部分 C 字符串函数

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/11728898-aeec-40e0-9a39-60cfeac714bf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=253dacb14bdcfa42fc287f0f5f24dbbe109f89e2188981535427999476acfe27&X-Amz-SignedHeaders=host&x-id=GetObject)

Redis 只会使用 C 字符串作为字面量，在大多数情况下，Redis 使用 SDS（Simple Dynamic String，简单动态字符串）作为字符串表示。

## 链表

```c
typedef struct listNode {
    struct listNode *prev;
    struct listNode *next;
    void *value;
} listNode;

typedef struct listIter {
    listNode *next;
    int direction;
} listIter;

typedef struct list {
    listNode *head;
    listNode *tail;
    void *(*dup)(void *ptr);
    void (*free)(void *ptr);
    int (*match)(void *ptr, void *key);
    unsigned long len;
} list;
```

Redis 的链表实现的特性

- 双端：
- 无环：
- 带表头指针和表尾指针：获取链表头尾的时间复杂度 O(1)
- 带链表长度计数器：使用 len 属性获取，O(1) 时间复杂度
- 多态：链表节点使用 void\* 指针保存节点值，可以保存各种不同类型的值

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/bbf72996-4fb8-40d4-a79d-40d5adfe148f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120742Z&X-Amz-Expires=3600&X-Amz-Signature=b5ff7468ab26f0f2961902b79e7818f2d2cef4d42da05bb56c7a267a2021a9f5&X-Amz-SignedHeaders=host&x-id=GetObject)

## 字典

hashtable

```c
/* This is our hash table structure. Every dictionary has two of this as we
 * implement incremental rehashing, for the old to the new table. */
typedef struct dictht {
    dictEntry **table;
    unsigned long size;
    unsigned long sizemask;
    unsigned long used;
} dictht;
```

```c
typedef struct dictEntry {
    void *key;
    union {
        void *val;
        uint64_t u64;
        int64_t s64;
        double d;
    } v;
		// 指向下一个哈希表节点，形成链表（用于相同hash值的，解决冲突）
    struct dictEntry *next;
} dictEntry;
```

字典

```c
typedef struct dict {
    dictType *type;
    void *privdata;
    dictht ht[2];
    long rehashidx; /* rehashing not in progress if rehashidx == -1 */
    unsigned long iterators; /* number of iterators currently running */
} dict;
```

ht 属性数一个包含两个项的数组，数组中的每个项都是一个 `dictht` 哈希表，一般情况下，字典只使用 `ht[0]` 哈希表， `ht[1]` 哈希表只会在对 `ht[0]` 哈希表进行 rehash 时使用。

如果没有在进行 `rehash` 那么它的值为 -1

**哈希算法**

Redis 使用 MurmurHash2 算法来计算哈希值。随机性分布，且计算速度快。

**解决键冲突**

链地址法，同一个哈希值的，每个新节点都放置在头指针位置。（因为没有尾指针，考虑到性能

）

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/47a36557-fd20-415b-8a7a-919e702368ec/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120742Z&X-Amz-Expires=3600&X-Amz-Signature=33d405fee1235c0efcf6eef332b39e5eb855b10f026df98baa0411b9a1971695&X-Amz-SignedHeaders=host&x-id=GetObject)

服务一般是分多次进行 rehash，也就是渐进性 hash。这里用到了 rehashidx 索引，也就是时候一个一个索引进行 迁移。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/ccaca300-4519-46ca-929e-ee568f95cb33/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120742Z&X-Amz-Expires=3600&X-Amz-Signature=6c987a48cba12dc0227a7657c6bdf2080cb9b276d9a22ce5df5c1a0c601cd450&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/6609f60b-c540-4baf-9d73-bdb09644d0cc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=bb237924f4df6bc083e90b41f7e75adf480849e90e893b38fc82f9ebceb52099&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/159eebb5-a0fc-49b3-9221-6927d9e7148b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=70edee2d6667aeaaf9505a18fe0724b8e084e3ca5ae53b3b5607d741bf756a15&X-Amz-SignedHeaders=host&x-id=GetObject)

## 跳跃表

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/8abee6fd-4006-4549-9ae1-b6976b0057c8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=e6039cceba9070c671bbcada55eb0d88a64e39e6eecb9d885771b6f2b03aff5e&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/28a90d0b-c57d-4b90-80f7-0e6dc05381a2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=1257f399f8c50feadb43c64e97b0daea2446989a708b71576e7dd72dd16a9e80&X-Amz-SignedHeaders=host&x-id=GetObject)

zskiplist 和 zskiplistNode 两个结构组成

前者保存跳跃表信息（表头节点、表尾节点、长度）

后者是跳跃表节点

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/08cf6bbf-0b16-4367-8116-769cd6c9d458/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=14bf57cd8cff08f6d4bac12f340b28b7b39e84278c596f6e7271ce6563aa3c09&X-Amz-SignedHeaders=host&x-id=GetObject)

## 整数集合

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/3fc7db08-4343-451b-8133-61f6f6384172/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=592b62d9a7e8ce1a00eef7ddb9abd30685766707a1b73ec9dbe4f07a28719a2d&X-Amz-SignedHeaders=host&x-id=GetObject)

**升级**（超过 encoding 的数值插入后，会进行升级，整个数组会被重新分配）

好处：提升整数集合的灵活性，尽可能节约内存

**降级**（不支持降级）

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/c5fba175-861d-48d7-8443-d251be42669c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=43cc0592e061cfd7c35f5b89f1510ef7900c7e119ab27f500b8f15a4a119fd81&X-Amz-SignedHeaders=host&x-id=GetObject)

## 压缩列表（ZipList）

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/9484906e-7c62-45f6-86c1-9ff1d5b29ba3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=13b2e6fc0bc67dc9fcc3100d9b2e33aa84722b972ac8211fac0db407c21539a4&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/3a70e9c0-2523-4ab5-96a6-8873d8e31deb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=5dd29d9d900de52acbc93704d7ca9dba5be4cb06e972854410ced578af5be56c&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/7be31f11-ee25-42b0-a71e-68aada7ebafd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=08fef0e5b1dcd99edafbd880e2d797499fd78ae476e29c34555a9955469c0805&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/7db03147-2f24-4967-8c7e-bf3ef8f671de/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=11802262de90291d91b4785d6046b09985242daa0266c50da5123ef34789e204&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/c4a4ad7a-261e-4563-a8e1-954a686dd9eb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=483eb4ac762622c467638329ee5b7fc0dfc718248077ab639210c2439bd07c07&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/e5b01042-58eb-46c3-a253-922b22cb02b9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=5e8fd2d1866b222a0c7d4bf51a4a4c2d7082df1a4e04381fec921dce0abf438b&X-Amz-SignedHeaders=host&x-id=GetObject)

**连锁更新**

连锁更新在最坏情况下需要对压缩列表执行 N 次空间重分配操作，而每次空间重分配的最坏复杂度为 O(N)，所以连锁更新的最坏复杂度为 O(N^2)。

尽管这个复杂度很高，但它真正出现的几率是很低的：

- 首先，压缩列表里要恰好有多个连续的、长度介于 250-253 字节之间的节点，连锁更新才可能引发，在实际中，这种情况并不多见。
- 其次，即使出现连锁更新，单只要被更新的节点数量不多，就不会对性能造成任何影响。

# 三、对象系统

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/272d210d-7e73-4c25-81ef-7066d62e4311/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=7607f0c08e37f0d79e153eb331197126b2c42d6a778c85fd19d1db9ff885911c&X-Amz-SignedHeaders=host&x-id=GetObject)

Redis 中每个对象都由一个 redisObject 结构表示

```c++
typedef struct redisObject {
    unsigned type:4;
    unsigned encoding:4;
    unsigned lru:LRU_BITS; /* LRU time (relative to global lru_clock) or
                            * LFU data (least significant 8 bits frequency
                            * and most significant 16 bits access time). */
    int refcount;
    // 指向底层实现数据结构的指针
		void *ptr;
} robj;
```

### 类型

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/130b3e43-cf77-469c-ad60-dc0a2feae0cd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=4168d7d649493a7985d4195c461fa31352cfeaae4afdf7b9c4973a607a5d2f98&X-Amz-SignedHeaders=host&x-id=GetObject)

### 编码和底层实现

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/8c99f045-9d05-46c4-9063-045384e3f536/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=8263e9fbe896c5861bcdfb84a0155ce24e3e00c7ca289df5c47bdab02b2637c7&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/11cac09c-fa1d-43cd-910c-ab648a88b199/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=09275024a9243f44a8ba7818dd50453a9dcba8034376e71412e2bb5d3b3e9a0a&X-Amz-SignedHeaders=host&x-id=GetObject)

## 字符串对象

编码有三种：

- int (可以用 long 类型保存的整数)
- raw （可以用 long double 类型保存的浮点数）
- embstr （字符串值，或者因为长度太大而没办法用 Long 类型表示的整数，又或者因为长度太大而没有办法用 long double 类型表示的浮点数）

如果字符串对象保存的是一个字符串值，并且这个字符串的长度小于等于 32 字节，那么字符串对象将使用 emstr 编码的方式来保存这个字符串值。

embstr 与 raw 区别：

- 大小的区别
- 修改时的区别
- 前者是只需分配一次连续空间给 redisObject 和 sdshdr，相应的，释放内存也是一次
- 后者需要分配两次空间，一个给 redisObject，一个给 sdshdr，释放内存两次

### 编码转换

embstr 是只读，如果修改字符串，那么 embstr 会变成 raw 编码在再进行修改

### 字符串命令的实现

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/1a6cac21-cd86-43fa-a629-840a54b1c15f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120744Z&X-Amz-Expires=3600&X-Amz-Signature=93ef0853cc0cae701c7529b42ab69dc2e2541a61fb00597fc7c0f80c6e61f980&X-Amz-SignedHeaders=host&x-id=GetObject)

## 列表对象

编码有：

- ziplist
- linkedlist

### 编码转换

当列表对象可以同时满足以下条件时，列表对象使用 ziplist 编码：

- 列表对象保存的所有字符串元素的长度都小于 64 字节；
- 列表对象保存的元素数量小于 521 个；

不能满足这两个条件的列表对象需要使用 linkedlist 编码。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/bc67647d-f473-47ee-82d5-12c35862498f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=583157facfc7252e0f8b3044741ae43b37f6e5913c046cbb2c6712a5990be2a8&X-Amz-SignedHeaders=host&x-id=GetObject)

## 哈希对象

编码有：

- ziplist
- hashtable

ziplist 底层

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/45bf02ca-ba86-47ef-90c2-a1cc37a824c6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=8b3266c257639315e1130b0aa23ef67a553d86a3cfcd8255724c26a209bd97fe&X-Amz-SignedHeaders=host&x-id=GetObject)

hashtable 底层

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/684a5834-5816-4dc5-90db-9f33a336ea23/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=25879b44047692284b0f2ebfec3743188b625dca081dd98a203a3ab96164d7d1&X-Amz-SignedHeaders=host&x-id=GetObject)

### 编码转换

当哈希对象可以同时满足以下两个条件时，哈希对象使用 ziplist 编码：

- 哈希对象保存的所有键值对的键和值的字符串长度都小于 64 字节
- 哈希对象保存的键值对数量小于 512 个

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/b22723a8-5639-4e87-81e5-2b41e160c388/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=4a46bcb8110dd1c3ba0c4847ee0768169a79d41179a152b87f3f53f7fd0df253&X-Amz-SignedHeaders=host&x-id=GetObject)

## 集合对象

编码：

- intset
- hashtable

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/76141797-693a-4819-92b3-038d019d8dd4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=527a01d655c466fb825363056e7b4cdfd8be3b9f593ac4da4b767f97294cc535&X-Amz-SignedHeaders=host&x-id=GetObject)

### 编码的转换

当集合对象可以同时满足以下两个条件时，对象使用 intset 编码：

- 集合对象保存的所有元素都是整数值
- 集合对象保存的元素数量不超过 512 个

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/3171fb1f-5f20-49b5-acb0-5b4f3a8d08e1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=d90a1185733bdd37053946d707d11317f862bf9c8edf51d61d9e721908df73ba&X-Amz-SignedHeaders=host&x-id=GetObject)

## 有序集合对象

编码：

- ziplist
- skiplist

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/174ad8a8-7af4-4537-9a96-4e9f6e97f069/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=71932acf3db17338354f0b625d5227389a681d98e98dcd99c691304a76a52a4d&X-Amz-SignedHeaders=host&x-id=GetObject)

```c++
typedef struct zset {
		zskiplist *zsl;
		dict *dict;
}
```

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/627c9efe-7a5d-44ff-8b81-4f683920e0ae/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=7365ebdeef1c47b66c6ef48f15106ea04b1afc7a3031d9f8bdac05df97c5284b&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/6e480ad8-b440-4899-9501-971f953bc9d4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=c7d081e4fec7e287fc6f2fca6e15e55e70600c3c5b740b5bf30ae03b7b1cec39&X-Amz-SignedHeaders=host&x-id=GetObject)

### 编码的转换

当有序集合对象可以同时满足以下两个条件时，使用 ziplist 编码：

- 有序集合保存的元素数量小于 128 个
- 有序集合保存的所有元素的成员的长度都小于 64 字节

否则，使用 skiplist 编码

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/1fd39d82-95f3-4af3-9b0e-6b777a94996a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=adfce51b21b912e5e43111a8bf1c854e40420c53c55acccf08a0c7035d4adcdd&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/7db1f476-5df2-4316-bfd2-02cc9376b027/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120745Z&X-Amz-Expires=3600&X-Amz-Signature=3c998c43dc62e2ef60ab8995bf1ad5d66eb81fc9b097147e742034b84b104495&X-Amz-SignedHeaders=host&x-id=GetObject)

# 参考书籍

《Redis 设计与实现》
