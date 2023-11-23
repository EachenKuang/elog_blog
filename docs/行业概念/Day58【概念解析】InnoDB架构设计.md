---
password: ""
icon: ""
date: "2023-11-18"
type: Post
category: 行业概念
slug: industry-day58
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
  - 数据库
summary: ""
title: Day58【概念解析】InnoDB架构设计
status: Published
urlname: 87f02b71-13f2-4c9c-a648-34dcf7b5a0bd
updated: "2023-11-23 05:34:00"
---

# 前言

> 😄 作为 MySQL5.6 之后的默认存储引擎，InnoDB 因其支持事务、外键等特性可是风靡一时，现在也是数据库理论中的面试必备要素，知其然还须知其所以然，那么掌握其架构当然也是至关重要。

# 整理定义

![MySQL 5.7 InnoDB 存储引擎架构图](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0 InnoDB 存储引擎架构图](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

**左边是 InnoDB 的**[**内存数据架构**](https://dev.mysql.com/doc/refman/8.0/en/innodb-in-memory-structures.html)**（In-Memory Structure），右边是**[**磁盘数据架构**](https://dev.mysql.com/doc/refman/8.0/en/innodb-on-disk-structures.html)**（On-disk Structure）**

# 复述展开

## InnoDB 内存结构设计

InnoDB 作为 MySQL 的一个存储引擎，它的内存结构设计用于优化数据处理性能。InnoDB 的内存结构主要包括以下几个部分：

1. **Buffer Pool**：<u>这是 InnoDB 最重要的内存结构之一，用于缓存表数据和索引</u>。当 MySQL 需要读取或写入数据时，它首先会查看这些数据是否已经在 Buffer Pool 中。如果在，那么就可以直接从内存中读取或写入，避免了磁盘 I/O 操作，从而大大提高了性能。
2. **Change Buffer**：Change Buffer 是 Buffer Pool 的一部分，用于缓存对二级索引的修改操作。当 MySQL 需要修改二级索引时，它可以先将修改操作写入 Change Buffer，然后在后台慢慢地将这些修改应用到磁盘上的索引，这样可以减少磁盘 I/O 操作，提高性能。
3. **Adaptive Hash Index**：这是 InnoDB 的一个优化特性，用于加速对 Buffer Pool 中数据的访问。当 MySQL 反复访问同一数据时，InnoDB 会自动创建一个哈希索引，使得 MySQL 可以更快地找到这些数据。
4. **Log Buffer**：这是 InnoDB 的另一个重要内存结构，用于缓存事务日志。当 MySQL 执行一个事务时，它会先将事务日志写入 Log Buffer，然后在适当的时机将这些日志刷新到磁盘上，这样可以减少磁盘 I/O 操作，提高性能。
5. **InnoDB 内部数据字典**：InnoDB 有一个内部的数据字典，用于存储表的元数据，如表的结构信息、索引信息等。

## InnoDB 磁盘结构

InnoDB 的磁盘结构是其如何在磁盘上存储数据和索引的方式。以下是 InnoDB 磁盘结构的主要组成部分：

1. **表（Tables）**：InnoDB 存储引擎中的表是行式存储的，每个表都有一个或多个聚簇索引或非聚簇索引。表中的数据按主键顺序存储在聚簇索引中。
2. **索引（Indexes）**：InnoDB 使用 B+树数据结构来实现索引，以加快数据访问速度。每个表至少有一个聚簇索引（主键索引），也可以有一个或多个二级索引。
3. **表空间（Tablespaces）**：表空间是 InnoDB 存储数据和索引的地方。默认情况下，所有的 InnoDB 表都存储在一个名为 ibdata1 的系统表空间中，但用户也可以配置 InnoDB 使用多个或单独的表空间。
4. **双写缓冲区（Doublewrite Buffer）**：双写缓冲区是 InnoDB 用来保护数据完整性的一种机制。在将数据页写入磁盘之前，InnoDB 会先将其写入双写缓冲区。如果在写入过程中发生系统崩溃，InnoDB 可以使用双写缓冲区中的数据恢复损坏的页。
5. **重做日志（Redo Log）**：重做日志是 InnoDB 用来保证事务的持久性的一种机制。在事务提交时，InnoDB 会先将事务的修改记录到重做日志，并确保重做日志被写入磁盘。如果在事务提交后发生系统崩溃，InnoDB 可以使用重做日志恢复未完成的事务。
6. **回滚日志（Undo Logs）**：回滚日志是 InnoDB 用来实现多版本并发控制（MVCC）和事务回滚的一种机制。当事务修改数据时，InnoDB 会在回滚日志中保存修改前的数据。如果事务需要回滚，或者其他事务需要读取修改前的数据，InnoDB 可以使用回滚日志来获取这些数据。

# 理解体会

理解 InnoDB 的结构是非常重要的，主要有以下几个原因：

1. 性能优化：理解 InnoDB 的内存和磁盘结构可以帮助你更好地理解 MySQL 的性能瓶颈，并进行有效的优化。例如，理解 Buffer Pool 和 Redo Log 的工作原理可以帮助你更好地配置这些参数，以提高 MySQL 的性能。
2. 数据恢复：当数据库发生故障时，理解 InnoDB 的结构可以帮助你更好地进行数据恢复。例如，理解 Doublewrite Buffer 和 Redo Log 的工作原理可以帮助你恢复因系统崩溃而损坏的数据。
3. 事务管理：理解 InnoDB 的事务管理机制，如 Undo Logs 和 MVCC，可以帮助你更好地理解事务的行为，如何处理并发控制和事务隔离。

> 💡 **如何学习呢？**
>
> - 阅读官方文档，这是第一手资料，也是最原始的资料：[MySQL :: MySQL 8.0 Reference Manual :: 15.4 InnoDB Architecture](https://dev.mysql.com/doc/refman/8.0/en/innodb-architecture.html)
> - 阅读相关书籍，最好是英文版，中文版的话一则需要翻译过程，可能过时，二则翻译过程中没有办法原汁原味，可能会失去一些干货，如果有能力的话最好还是阅读英文。如果英文基础确实不好，那还是读中文版本的。
> - 实践操作，除了理论学习之外，还是需要实践操作的。”实践是检验真理的唯一标准“。在实际过程中，可能能够发现更多纸上找不到的东西。
> - 源码学习，这是最难的一点，也是最直接的一点，`show me code` 。结合源码，更能够体会设计的实际含义在内的。

# 参考：

[MySQL :: MySQL 8.0 Reference Manual :: 15.4 InnoDB Architecture](https://dev.mysql.com/doc/refman/8.0/en/innodb-architecture.html)
