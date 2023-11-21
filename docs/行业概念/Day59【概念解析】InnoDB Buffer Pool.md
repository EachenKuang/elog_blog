---
password: ""
icon: ""
date: "2023-11-19"
type: Post
category: 行业概念
slug: industry-day59
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day59【概念解析】InnoDB Buffer Pool
status: Published
cover: "https://image.kuangyichen.com/image/innodb%20buffer%20pool.webp"
urlname: eee550ce-866f-41ee-a83c-387ac8b72c92
updated: "2023-11-20 15:01:00"
---

# 整理定义

Buffer Pool（缓冲池）是 InnoDB 存储引擎的一个重要组成部分，它是 InnoDB 用于缓存表和索引数据的内存区域。

定义和用途：Buffer Pool 是 InnoDB 存储引擎的主要内存区域，用于缓存磁盘上的数据和索引，以减少对磁盘的 I/O 操作。当 MySQL 需要读取或写入数据时，它首先会检查数据是否已经在 Buffer Pool 中。如果在，那么 MySQL 可以直接从内存中读取或写入数据，避免了磁盘 I/O 操作。如果不在，MySQL 需要从磁盘读取数据，并将其放入 Buffer Pool 中。

使用方式：Buffer Pool 的大小可以通过 innodb_buffer_pool_size 参数进行配置。这个参数的设置对 MySQL 的性能有很大影响。一般来说，Buffer Pool 的大小应该设置为系统内存的 50%-80%。但具体的设置需要根据系统的实际工作负载进行调整。

优点：Buffer Pool 可以显著提高 MySQL 的性能，因为内存访问速度远高于磁盘。通过缓存磁盘上的数据和索引，Buffer Pool 可以减少磁盘 I/O 操作，从而提高查询速度。

缺点：虽然 Buffer Pool 可以提高性能，但它也会消耗大量的内存。如果 Buffer Pool 设置得过大，可能会导致系统的其他部分内存不足，从而影响系统的整体性能。

作用：Buffer Pool 的主要作用是提高 MySQL 的性能。通过缓存磁盘上的数据和索引，Buffer Pool 可以减少磁盘 I/O 操作，从而提高查询速度。

# 复述展开

缓冲池是主内存中的一个区域，InnoDB 在访问时缓存表和索引数据。 缓冲池允许直接从内存访问经常使用的数据，从而加快处理速度。 在专用服务器上，高达 80% 的物理内存通常分配给缓冲池。

为了提高大容量读取操作的效率，缓冲池被划分为可以容纳多行的页面。 为了提高缓存管理的效率，缓冲池被实现为页面的链表； 使用最近最少使用 (LRU) 算法的变体，很少使用的数据会从缓存中老化。

## LRU 算法

缓冲池使用 LRU 算法的变体作为列表进行管理。 当需要空间向缓冲池添加新页面时，最近最少使用的页面将被逐出，并将新页面添加到列表的中间。 这种中点插入策略将列表视为两个子列表：

- 头部是最近访问的新（“年轻”）页面的子列表
- 在尾部，是最近访问较少的旧页面的子列表。

![innodb-buffer-pool-list.png](https://image.kuangyichen.com/image/innodb-buffer-pool-list.png)

该算法将经常使用的页面保留在新的子列表中。 旧的子列表包含不常用的页面； 这些页面是被驱逐的候选页面。

默认情况下，该算法的运行方式如下：

- 缓冲池的 3/8 专用于旧子列表。
- 列表的中点是新子列表的尾部与旧子列表的头部相交的边界。
- 当 InnoDB 将一页读入缓冲池时，它最初将其插入到<u>中点</u>（旧子列表的头部）。 可以读取页面是因为用户启动的操作（例如 SQL 查询）需要它，或者作为 InnoDB 自动执行的预读操作的一部分。
- <u>访问旧子列表中的页面会使其“年轻”，将其移动到新子列表的头部</u>。 如果该页是因为用户启动的操作需要而被读取的，则第一次访问会立即发生，并且该页会变为年轻页。 如果由于预读操作而读取该页，则第一次访问不会立即发生，并且在该页被逐出之前可能根本不会发生。
- <u>当数据库运行时，缓冲池中未访问的页面会通过向列表尾部移动而“老化”</u>。 新旧子列表中的页面随着其他页面的更新而老化。 当页面在中点插入时，旧子列表中的页面也会老化。 最终，未使用的页面到达旧子列表的尾部并被驱逐。

默认情况下，<u>查询读取的页面会立即移动到新的子列表中</u>，这意味着它们在缓冲池中停留的时间更长。 例如，针对 mysqldump 操作或不带 WHERE 子句的 SELECT 语句执行的表扫描可以将大量数据放入缓冲池并逐出等量的旧数据，即使新数据不再使用 。 类似地，由预读后台线程加载且仅访问一次的页面将被移动到新列表的头部。 这些情况可能会将经常使用的页面推送到旧的子列表，在那里它们会被驱逐。

## **Buffer Pool Configuration**

| 配置属性                            | 控制内容                              | 描述                                                                                                                                     |
| ----------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| innodb_buffer_pool_size             | 控制 Buffer Pool 的大小               | 这是 Buffer Pool 的大小，通常设置为系统内存的 50%-80%。这是**最重要**的 InnoDB 参数，因为它决定了 MySQL 可以缓存多少数据和索引。         |
| innodb_buffer_pool_instances        | 控制 Buffer Pool 的实例数量           | 这个参数用于将 Buffer Pool 划分为多个独立的实例，以减少线程之间的竞争。通常，每 1GB 的 Buffer Pool 大小，就应该有一个 Buffer Pool 实例。 |
| innodb_buffer_pool_dump_at_shutdown | 控制是否在关闭时转储 Buffer Pool      | 这个参数决定了 MySQL 在关闭时是否将 Buffer Pool 的内容转储到磁盘。如果启用，MySQL 在下次启动时可以更快地预热 Buffer Pool。               |
| innodb_buffer_pool_load_at_startup  | 控制是否在启动时加载 Buffer Pool      | 这个参数决定了 MySQL 在启动时是否从磁盘加载 Buffer Pool 的内容。如果启用，MySQL 在启动时可以更快地预热 Buffer Pool。                     |
| innodb_buffer_pool_dump_pct         | 控制在关闭时转储 Buffer Pool 的百分比 | 这个参数决定了 MySQL 在关闭时转储 Buffer Pool 的百分比。默认值是 25，表示转储 Buffer Pool 中的 25%的最热的页。                           |

> 💡 **小知识**
>
> > When increasing or decreasing [`innodb_buffer_pool_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size), the operation is performed in chunks. Chunk size is defined by the [`innodb_buffer_pool_chunk_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_chunk_size) configuration option, which has a default of `128M`.
>
> [`innodb_buffer_pool_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size) 的值需要为 [`innodb_buffer_pool_chunk_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_chunk_size) \* [`innodb_buffer_pool_instances`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_instances) 的整数倍，否则会动态调整。

    > When increasing or decreasing [`innodb_buffer_pool_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size), the operation is performed in chunks. Chunk size is defined by the [`innodb_buffer_pool_chunk_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_chunk_size) configuration option, which has a default of `128M`.


    [`innodb_buffer_pool_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size) 的值需要为 [`innodb_buffer_pool_chunk_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_chunk_size) * [`innodb_buffer_pool_instances`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_instances) 的整数倍，否则会动态调整。

## 缓冲池预读取（Buffer Pool Prefetching（Read-Ahead））

[MySQL :: MySQL 8.0 Reference Manual :: 15.8.3.4 Configuring InnoDB Buffer Pool Prefetching (Read-Ahead)](https://dev.mysql.com/doc/refman/8.0/en/innodb-performance-read_ahead.html)

<u>**预读请求(Read-Ahead)**</u>是异步预取缓冲池中的多个页面的 I/O 请求，以预测即将需要这些页面。 这些请求将所有页面引入<u>**Extend**</u>(某一范围内)。

> 💡 **what is** [**extend**](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_extent)**?**  
> A group of ***pages*** within a ***tablespace***. For the default ***page size*** of 16KB, an extent contains 64 pages. In MySQL 5.6, the page size for an `InnoDB` instance can be 4KB, 8KB, or 16KB, controlled by the [`innodb_page_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_page_size) configuration option. For 4KB, 8KB, and 16KB pages sizes, the extent size is always 1MB (or 1048576 bytes).  
> Support for 32KB and 64KB `InnoDB` page sizes was added in MySQL 5.7.6. For a 32KB page size, the extent size is 2MB. For a 64KB page size, the extent size is 4MB.  
> `InnoDB` features such as ***segments***, ***read-ahead*** requests and the ***doublewrite buffer*** use I/O operations that read, write, allocate, or free data one extent at a time.

    A group of _**pages**_ within a _**tablespace**_. For the default _**page size**_ of 16KB, an extent contains 64 pages. In MySQL 5.6, the page size for an `InnoDB` instance can be 4KB, 8KB, or 16KB, controlled by the [`innodb_page_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_page_size) configuration option. For 4KB, 8KB, and 16KB pages sizes, the extent size is always 1MB (or 1048576 bytes).
    Support for 32KB and 64KB `InnoDB` page sizes was added in MySQL 5.7.6. For a 32KB page size, the extent size is 2MB. For a 64KB page size, the extent size is 4MB.
    `InnoDB` features such as _**segments**_, _**read-ahead**_ requests and the _**doublewrite buffer**_ use I/O operations that read, write, allocate, or free data one extent at a time.

InnoDB 使用两种预读算法来提高 I/O 性能：

- **线性预读[Linear** read-ahead**]**是一种根据缓冲池中按顺序访问的页面来预测可能很快需要哪些页面的技术。
  可以通过使用配置参数 `innodb_read_ahead_threshold` （默认值为 56，范围是 0-64）调整触发异步读取请求所需的顺序页面访问数量来控制 InnoDB 何时执行预读操作。 在添加该参数之前，InnoDB 只会在读取当前 extent 的最后一页时才计算是否对整个下一个 extent 发出异步预取请求。
- **随机预读[Random** read-ahead**]**是一种根据缓冲池中已有的页面来预测何时可能很快需要页面的技术，而不管这些页面的读取顺序如何。 如果在缓冲池中找到来自同一盘区的 13 个连续页面，InnoDB 会异步发出请求来预取该盘区的剩余页面。 要启用此功能，可以将配置变量 `innodb_random_read_ahead` 设置为 ON。

# 理解体会

InnoDB 的缓冲池（buffer pool）是一个重要的组件，用于缓存数据库中的数据和索引。以下是一些关于 InnoDB 缓冲池的注意事项：

1. 适当配置缓冲池大小：缓冲池的大小对数据库性能有重要影响。过小的缓冲池可能导致频繁的磁盘读取，而过大的缓冲池可能占用过多的内存资源。根据系统的需求和可用的内存，合理配置缓冲池的大小是重要的。
2. 监控缓冲池的使用情况：定期监控缓冲池的使用情况，包括缓冲池的命中率、脏页的比例等。这些指标可以帮助您了解缓冲池的效率和性能，并根据需要进行调整。
3. 预热缓冲池：在数据库启动时，缓冲池是空的。为了避免冷启动时的性能下降，可以考虑使用预热技术，通过加载常用的数据和索引到缓冲池中，提前填充缓冲池。
4. 避免频繁的刷新：InnoDB 使用脏页刷新机制将修改过的数据写回磁盘。频繁的刷新操作可能导致磁盘 I/O 压力增加，影响性能。可以通过调整合适的刷新策略和参数来避免频繁的刷新。
5. 考虑使用 SSD 存储：SSD 存储设备的读写速度较快，可以显著提升 InnoDB 缓冲池的性能。如果条件允许，可以考虑将数据库存储在 SSD 上，以获得更好的性能。
6. 定期备份缓冲池：缓冲池中的数据是易失的，因此定期备份缓冲池是重要的。可以使用 MySQL 的备份工具或者自定义脚本来定期

> 📌 **快速跳转链接**  
> 【概念解析】启动
>
> 【概念解析】Day 1 - 10
>
> 【概念解析】Day 11 - 20
>
> 【概念解析】Day 21 - 30
>
> 【概念解析】Day 31 - 40
>
> 【概念解析】Day 41 - 50
>
> 【概念解析】Day 51 - 60
>
> 【概念解析】Day 61 - 70

<details>
<summary>【概念解析】启动</summary>

[bookmark](https://kuangyichen.com/article/industry)

[bookmark](https://kuangyichen.com/article/start-industry-100-words)

</details>

<details>
<summary>【概念解析】Day 1 - 10</summary>

[bookmark](https://kuangyichen.com/article/industry-day1)

[bookmark](https://kuangyichen.com/article/industry-day2)

[bookmark](https://kuangyichen.com/article/industry-day3)

[bookmark](https://kuangyichen.com/article/industry-day4)

[bookmark](https://kuangyichen.com/article/industry-day5)

[bookmark](https://kuangyichen.com/article/industry-day6)

[bookmark](https://kuangyichen.com/article/industry-day7)

[bookmark](https://kuangyichen.com/article/industry-day8)

[bookmark](https://kuangyichen.com/article/industry-day9)

[bookmark](https://kuangyichen.com/article/industry-day10)

</details>

<details>
<summary>【概念解析】Day 11 - 20</summary>

[bookmark](https://kuangyichen.com/article/industry-day11)

[bookmark](https://kuangyichen.com/article/industry-day12)

[bookmark](https://kuangyichen.com/article/industry-day13)

[bookmark](https://kuangyichen.com/article/industry-day14)

[bookmark](https://kuangyichen.com/article/industry-day15)

[bookmark](https://kuangyichen.com/article/industry-day16)

[bookmark](https://kuangyichen.com/article/industry-day17)

[bookmark](https://kuangyichen.com/article/industry-day18)

[bookmark](https://kuangyichen.com/article/industry-day19)

[bookmark](https://kuangyichen.com/article/industry-day20)

</details>

<details>
<summary>【概念解析】Day 21 - 30</summary>

[bookmark](https://kuangyichen.com/article/industry-day21)

[bookmark](https://kuangyichen.com/article/industry-day22)

[bookmark](https://kuangyichen.com/article/industry-day23)

[bookmark](https://kuangyichen.com/article/industry-day24)

[bookmark](https://kuangyichen.com/article/industry-day25)

[bookmark](https://kuangyichen.com/article/industry-day26)

[bookmark](https://kuangyichen.com/article/industry-day27)

[bookmark](https://kuangyichen.com/article/industry-day28)

[bookmark](https://kuangyichen.com/article/industry-day29)

[bookmark](https://kuangyichen.com/article/industry-day30)

</details>

<details>
<summary>【概念解析】Day 31 - 40</summary>

[bookmark](https://kuangyichen.com/article/industry-day31)

[bookmark](https://kuangyichen.com/article/industry-day32)

[bookmark](https://kuangyichen.com/article/industry-day33)

[bookmark](https://kuangyichen.com/article/industry-day34)

[bookmark](https://kuangyichen.com/article/industry-day35)

[bookmark](https://kuangyichen.com/article/industry-day36)

[bookmark](https://kuangyichen.com/article/industry-day37)

[bookmark](https://kuangyichen.com/article/industry-day38)

[bookmark](https://kuangyichen.com/article/industry-day39)

[bookmark](https://kuangyichen.com/article/industry-day40)

</details>

<details>
<summary>【概念解析】Day 41 - 50</summary>

[bookmark](https://kuangyichen.com/article/industry-day41)

[bookmark](https://kuangyichen.com/article/industry-day42)

[bookmark](https://kuangyichen.com/article/industry-day43)

[bookmark](https://kuangyichen.com/article/industry-day44)

[bookmark](https://kuangyichen.com/article/industry-day45)

[bookmark](https://kuangyichen.com/article/industry-day46)

[bookmark](https://kuangyichen.com/article/industry-day47)

[bookmark](https://kuangyichen.com/article/industry-day48)

[bookmark](https://kuangyichen.com/article/industry-day49)

[bookmark](https://kuangyichen.com/article/industry-day50)

</details>

<details>
<summary>【概念解析】Day 51 - 60</summary>

[bookmark](https://kuangyichen.com/article/industry-day51)

[bookmark](https://kuangyichen.com/article/industry-day52)

[bookmark](https://kuangyichen.com/article/industry-day53)

[bookmark](https://kuangyichen.com/article/industry-day54)

[bookmark](https://kuangyichen.com/article/industry-day55)

[bookmark](https://kuangyichen.com/article/industry-day56)

[bookmark](https://kuangyichen.com/article/industry-day57)

[bookmark](https://kuangyichen.com/article/industry-day58)

[bookmark](https://kuangyichen.com/article/industry-day59)

</details>
