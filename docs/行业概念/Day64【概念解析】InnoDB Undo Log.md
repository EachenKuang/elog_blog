---
password: ""
icon: ""
date: "2023-11-24"
type: Post
category: 行业概念
slug: industry-day64
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day64【概念解析】InnoDB Undo Log
status: Published
urlname: 9c82cf19-8fa4-48fa-ad46-77e0652d29c7
updated: "2023-11-24 03:06:00"
---

# 整理定义

中文名称：撤销日志

英文名称：Undo Log

![MySQL 5.7版本](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

> An undo log is a collection of undo log records associated with a single read-write transaction. An undo log record contains information about how to undo the latest change by a transaction to a [clustered index](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_clustered_index) record. If another transaction needs to see the original data as part of a consistent read operation, the unmodified data is retrieved from undo log records. Undo logs exist within [undo log segments](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_undo_log_segment), which are contained within [rollback segments](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_rollback_segment). Rollback segments reside in the [system tablespace](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_system_tablespace), in [undo tablespaces](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_undo_tablespace), and in the [temporary tablespace](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_temporary_tablespace).  
> ——[MySQL :: MySQL 5.7 Reference Manual :: 14.6.7 Undo Logs](https://dev.mysql.com/doc/refman/5.7/en/innodb-undo-logs.html)

**撤销日志（Undo Log）**是与单个读写事务相关联的一组撤销日志记录。撤销日志记录包含有关如何撤消事务对<u>聚簇索引</u>记录的最新更改的信息。<u>如果另一个事务需要在一致性读操作中查看原始数据，则从撤销日志记录中检索未修改的数据</u>。撤销日志存在于**撤销日志段（undo log segments）**中，这些段包含在**回滚段（rollback segements）**中。**回滚段**位于系统表空间（system tablespace）、撤销表空间（undo tablespace）和临时表空间（temporary tablespace）中。

位于临时表空间（temporary tablespace）中的撤销日志（Undo Log）用于修改用户定义的临时表中的数据的事务。<u>这些撤销日志不会被重做记录，因为它们不需要用于崩溃恢复</u>。它们仅在服务器运行时用于回滚。这种类型的撤销日志通过避免重做日志 I/O 来提高性能。

## 相关概念解析

**聚簇索引（clustered index）**

> 聚簇索引是 InnoDB 中用于主键索引的术语。InnoDB 表存储是根据主键列的值进行组织的，以加快涉及主键列的查询和排序。为了获得最佳性能，请根据性能关键的查询仔细选择主键列。由于修改聚簇索引的列是一项昂贵的操作，请选择很少或从不更新的主键列。

    **clustered index**


    The `InnoDB` term for a _**primary key**_ index. `InnoDB` table storage is organized based on the values of the primary key columns, to speed up queries and sorts involving the primary key columns. For best performance, choose the primary key columns carefully based on the most performance-critical queries. Because modifying the columns of the clustered index is an expensive operation, choose primary columns that are rarely or never updated.

**撤销日志段（undo log segment）**

> 撤销日志段是一组**撤销日志**的集合。撤销日志段存在于**回滚段**中。一个撤销日志段可能包含来自多个事务的撤销日志。<u>一个撤销日志段一次只能被一个事务使用</u>，<u>但在事务</u><u>**提交**</u><u>或</u><u>**回滚**</u><u>后可以被重新使用</u>。也可以称为“撤销段”。

    **undo log segment**


    A collection of _**undo logs**_. Undo log segments exists within _**rollback segments**_. An undo log segment might contain undo logs from multiple transactions. An undo log segment can only be used by one transaction at a time but can be reused after it is released at transaction _**commit**_ or _**rollback**_. May also be referred to as an “undo segment”.

**回滚段**

> 回滚段是包含撤销日志的存储区域。传统上，回滚段位于系统表空间中。从 MySQL 5.6 开始，回滚段可以位于撤销表空间中。从 MySQL 5.7 开始，回滚段也分配给全局临时表空间。

    **rollback segment**
    The storage area containing the _**undo logs**_. Rollback segments have traditionally resided in the _**system tablespace**_. As of MySQL 5.6, rollback segments can reside in _**undo tablespaces**_. As of MySQL 5.7, rollback segments are also allocated to the _global temporary tablespace_.

**撤销表空间**

> 一个 Undo 表空间包含 Undo 日志。Undo 日志存在于 Undo 日志段中，这些日志段被包含在回滚段中。回滚段传统上位于系统表空间中。从 MySQL 5.6 开始，回滚段可以位于 Undo 表空间中。在 MySQL 5.6 和 MySQL 5.7 中，Undo 表空间的数量由 innodb_undo_tablespaces 配置选项控制。在 MySQL 8.0 中，当 MySQL 实例初始化时，会创建两个默认的 Undo 表空间，可以使用 CREATE UNDO TABLESPACE 语法创建额外的 Undo 表空间。

    **undo tablespace**


    An undo tablespace contains _**undo logs**_. Undo logs exist within _**undo log segments**_, which are contained within _**rollback segments**_. Rollback segments have traditionally resided in the system tablespace. As of MySQL 5.6, rollback segments can reside in undo tablespaces. In MySQL 5.6 and MySQL 5.7, the number of undo tablespaces is controlled by the [`innodb_undo_tablespaces`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_undo_tablespaces) configuration option. In MySQL 8.0, two default undo tablespaces are created when the MySQL instance is initialized, and additional undo tablespaces can be created using [`CREATE UNDO TABLESPACE`](https://dev.mysql.com/doc/refman/5.7/en/create-tablespace.html) syntax.

# 复述展开

**InnoDB 支持最多 128 个回滚段，其中 32 个分配给临时表空间。这样就留下了 96 个回滚段可以分配给修改常规表中的事务**。`innodb_rollback_segments`变量定义了 InnoDB 使用的回滚段数量。

回滚段支持的事务数量取决于回滚段中的撤销槽数量以及每个事务所需的撤销日志数量。回滚段中的撤销槽数量根据 InnoDB 页面大小而异。如下图所示：

| **InnoDB Page Size** | **Number of Undo Slots in a Rollback Segment (InnoDB Page Size / 16)** |
| -------------------- | ---------------------------------------------------------------------- |
| `4096 (4KB)`         | `256`                                                                  |
| `8192 (8KB)`         | `512`                                                                  |
| `16384 (16KB)`       | `1024`                                                                 |
| `32768 (32KB)`       | `2048`                                                                 |
| `65536 (64KB)`       | `4096`                                                                 |

一个事务最多分配四个撤销日志，分别用于以下操作类型：

- 用户定义表上的`INSERT`操作
- 用户定义表上的`UPDATE`和`DELETE`操作
- 用户定义临时表上的`INSERT`操作
- 用户定义临时表上的`UPDATE`和`DELETE`操作

撤销日志**根据需要**进行分配。
例如，对<u>常规表</u>和<u>临时表</u>执行`INSERT`、`UPDATE`和`DELETE`操作的事务需要分配四个完整的撤销日志。而只对常规表执行 INSERT 操作的事务只需要一个撤销日志。

对常规表执行操作的事务从分配的**系统表空间**或**撤销表空间**回滚段中分配撤销日志。对临时表执行操作的事务从分配的**临时表空间**回滚段中分配撤销日志。

分配给事务的撤销日志将在事务的整个持续时间内与之关联。
例如，分配给事务的撤销日志用于该事务执行的所有常规表的 INSERT 操作。

## 如何估计 InnoDB 能够支持的并发读写事务数量

根据上述因素，可以使用以下公式来估计 InnoDB 能够支持的并发读写事务数量。

- 如果每个事务**只**执行 INSERT，或者**只**执行 UPDATE 或 DELETE 操作，那么 InnoDB 能够支持的并发读写事务数量为：

  $$
  [
  \left(\frac{{\text{{innodb\_page\_size}}}}{{16}}\right) \times \left(\text{{innodb\_rollback\_segments}} - 32\right)
  ]
  $$

- 如果每个事务执行 INSERT 和 UPDATE 或 DELETE 操作，那么 InnoDB 能够支持的并发读写事务数量为：

$$
[
\left(\frac{{\text{{innodb\_page\_size}}}}{{16 \times 2}}\right) \times \left(\text{{innodb\_rollback\_segments}} - 32\right)
]
$$

- 如果每个事务在临时表上执行 INSERT 操作，那么 InnoDB 能够支持的并发读写事务数量为：

$$
[
\left(\frac{{\text{{innodb\_page\_size}}}}{{16}}\right) \times 32
]
$$

- 如果每个事务在临时表上执行 INSERT 和 UPDATE 或 DELETE 操作，那么 InnoDB 能够支持的并发读写事务数量为：

  $$
  [
  \left(\frac{{\text{{innodb\_page\_size}}}}{{16 \times 2}}\right) \times 32
  ]
  $$

# 理解体会

InnoDB 的 Undo Log 是 MySQL 数据库中的一个重要组件，它主要用于在事务执行过程中记录数据的旧版本信息，以便在需要时恢复数据。

定义：Undo Log 是一种日志文件，它记录了事务执行过程中对数据的修改前的状态，以便在事务失败或者进行 ROLLBACK 操作时可以恢复数据。

**作用：**

1. 数据恢复：在事务失败或者进行 ROLLBACK 操作时，可以通过 Undo Log 来恢复数据。
2. 实现 MVCC：Undo Log 是实现多版本并发控制（MVCC）的关键，它可以提供事务开始时的数据快照，使得每个事务都能看到一致的数据视图。

磁盘分布情况：Undo Log 存储在 InnoDB 的系统表空间或者独立的 Undo 表空间中。在 MySQL 8.0 及更高版本中，推荐使用独立的 Undo 表空间。**【从文章开头的对比图可以看出】**

**注意事项：**

1. Undo Log 的数量：Undo Log 的数量由`innodb_undo_logs`变量控制。如果这个值设置得太小，那么可能会限制并发事务的数量。如果这个值设置得太大，那么可能会浪费磁盘空间。
2. Undo Log 的大小：Undo Log 的大小由`innodb_undo_log_truncate`和`innodb_max_undo_log_size`两个变量控制。如果 Undo Log 的大小超过了`innodb_max_undo_log_size`，并且`innodb_undo_log_truncate`设置为 ON，那么 InnoDB 会尝试截断 Undo Log 以释放磁盘空间。
