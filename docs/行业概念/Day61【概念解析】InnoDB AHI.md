---
password: ""
icon: ""
date: "2023-11-21"
type: Post
category: 行业概念
slug: industry-day61
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day61【概念解析】InnoDB AHI
status: Published
urlname: ca64831c-9fae-4a56-af56-14cf15a04af4
updated: "2023-11-22 02:30:00"
---

# 整理定义

中文名称：自适应哈希索引

英文名称：**Adaptive Hash Index（AHI）**

![](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

**自适应哈希索引（Adaptive Hash Index）**使 InnoDB 能够在具有适当的工作负载组合和足够的缓冲池内存的系统上执行更像内存数据库的操作，而无需牺牲事务功能或可靠性。

# 复述展开

根据观察到的搜索模式，使用索引键的前缀构建哈希索引。 前缀可以是任意长度，并且可能只有 B 树中的某些值出现在哈希索引中。 哈希索引是根据需要为经常访问的索引页构建的。

如果表几乎完全适合主内存，则哈希索引可以通过直接查找任何元素、将索引值转换为某种指针来加速查询。 InnoDB 有一个监控索引搜索的机制。 如果 InnoDB 发现查询可以从构建哈希索引中受益，它会自动执行此操作。

## 何时应该启用或者禁用？

对于某些工作负载，哈希索引查找所带来的加速远远超过了监视索引查找和维护哈希索引结构的额外工作。 在繁重的工作负载（例如多个并发联接）下，对自适应哈希索引的访问有时会成为争用的根源。 使用 LIKE 运算符和 % 通配符的查询往往也不会受益。 对于无法从自适应哈希索引中受益的工作负载，将其关闭可以减少不必要的性能开销。 由于很难提前预测自适应哈希索引是否适合特定系统和工作负载，可以在启用和禁用它的情况下**运行基准测试**。

## 相关配置

- **innodb_adaptive_hash_index**

  自适应哈希索引由 `innodb_adaptive_hash_index` 变量启用，或在服务器启动时通过 `--skip-innodb-adaptive-hash-index` 关闭。**默认为开启**。

- **innodb_adaptive_hash_index_parts**

  自适应哈希索引特征是<u>分区</u>的。 每个索引都绑定到一个特定的分区，并且每个分区都由单独的锁存器保护。 分区由 `innodb_adaptive_hash_index_parts` 变量控制。 `innodb_adaptive_hash_index_parts` 变量默认值为 8。 最大设置为 512。

## 如何监控

监控**自适应哈希索引（Adaptive Hash Index）**的使用情况：你可以通过`SHOW ENGINE INNODB STATUS`命令来查看 AHI 的使用情况，包括哈希索引的数量，哈希索引的大小等。这可以帮助你更好地理解和配置 AHI。

# 理解体会

InnoDB 的自适应哈希索引（Adaptive Hash Index，AHI）是一种特殊的索引，它是 InnoDB 存储引擎为了提高查询性能而设计的。当某些索引值被频繁地访问时，InnoDB 会自动创建哈希索引，以加速对这些索引值的查询。这种索引是自适应的，因为它会根据数据库的实际访问模式动态地创建和删除。

AHI 的主要优点是可以显著提高查询性能，特别是在大量的点查询（即查询单个索引值）和等值查询（即查询索引值等于某个常量的记录）的场景下。然而，AHI 也有一些缺点。首先，AHI 会占用一部分内存，如果内存资源有限，可能会影响其他功能的性能。其次，AHI 的创建和删除需要一定的 CPU 资源，如果 CPU 资源有限，可能会影响数据库的性能。

总的来说，AHI 是 InnoDB 的一个强大的功能，可以显著提高查询性能。但是，使用 AHI 需要一定的专业知识，如果配置不当，可能会影响数据库的性能和稳定性。因此，我们在使用 AHI 时，需要谨慎小心。

# 参考：

[MySQL :: MySQL 8.0 Reference Manual :: 15.5.3 Adaptive Hash Index](https://dev.mysql.com/doc/refman/8.0/en/innodb-adaptive-hash.html)

[MySQL :: MySQL 8.0 Reference Manual :: 8.3.9 Comparison of B-Tree and Hash Indexes](https://dev.mysql.com/doc/refman/8.0/en/index-btree-hash.html)
