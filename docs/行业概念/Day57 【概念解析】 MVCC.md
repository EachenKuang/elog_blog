---
password: ""
icon: ""
date: "2023-11-17"
type: Post
category: 行业概念
slug: industry-day57
tags:
  - 行业概念
  - 文字
  - 思考
  - 数据库
summary: ""
title: Day57 【概念解析】 MVCC
status: Published
cover: "https://image.kuangyichen.com/image/mvcc.webp"
urlname: e1fbca6a-fd99-46db-9c24-58ef0864224a
updated: "2023-11-19 11:31:00"
---

# 前言

> 😄 通过介绍 MVCC 的定义、特性以及在 InnoDB 中的实现来获取高并发。

# 整理定义

MVCC（multiversion concurrency control），多版本并发控制。

> **Multiversion concurrency control** (**MCC** or **MVCC**), is a [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control) method commonly used by [database management systems](https://en.wikipedia.org/wiki/Database_management_system) to provide concurrent access to the database and in programming languages to implement [transactional memory](https://en.wikipedia.org/wiki/Transactional_memory).

    ——《Wikipedia》

多版本并发控制（MCC 或 MVCC）是数据库管理系统常用的一种并发控制方法，用于提供对数据库的并发访问，并在编程语言中实现事务性内存。

# 复述展开

## Why MVCC？

InnoDB 通过使用 MVCC 来获取高并发性，并且实现了 SQL 标准的**4 种**隔离级别，其中，在 InnoDB 中，默认级别为 **REPEATABLE-READ(可重复读)\*\***。**值得一提的是，InnoDB 使用了 **next-key lockling（临键锁）\*\* 的策略来避免幻读（phantom）现象的产生。

之前在[数据库事务](https://kuangyichen.com/article/industry-day56)这一概念中，介绍了 SQL 标准的 4 种隔离级别

> **SQL 标准的 4 种隔离级别**

    - **READ-UNCOMMITTED(读未提交)**：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。
    - **READ-COMMITTED(读已提交)**：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生。
    - **REPEATABLE-READ(可重复读)**：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
    - **SERIALIZABLE(可串行化)**：最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及[幻读](https://www.zhihu.com/search?q=%E5%B9%BB%E8%AF%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1935044903%7D)。

[MySQL :: MySQL 8.0 Reference Manual :: 15.7.2.1 Transaction Isolation Levels](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html)

## What is MVCC

### **MVCC 多版本并发控制**

> 😄 MySQL 的大多数事务型存储引擎实现都不是简单的行级锁。基于提升并发性考虑，一般都同时实现了多版本并发控制（MVCC），包括 Oracle、PostgreSQL。只是实现机制各不相同。  
> 可以认为 MVCC 是行级锁的一个变种，但它在很多情况下避免了加锁操作，因此开销更低。虽然实现机制有所不同，但大都实现了非阻塞的读操作，写操作也只是锁定必要的行。

    可以认为 MVCC 是行级锁的一个变种，但它在很多情况下避免了加锁操作，因此开销更低。虽然实现机制有所不同，但大都实现了非阻塞的读操作，写操作也只是锁定必要的行。

MVCC 的实现是**通过保存数据在某个时间点的快照**来实现的。也就是说不管需要执行多长时间，每个事物看到的数据都是一致的。

典型的 MVCC 实现方式，分为**乐观（optimistic）并发控制和悲观（pressimistic）并发控制**。下边通过 InnoDB 的简化版行为来说明 MVCC 是如何工作的。

InnoDB 的 MVCC，是通过在每行记录后面保存两个隐藏的列来实现。这两个列，一个保存了行的创建时间，一个保存行的过期时间（删除时间）。

当然存储的并不是真实的时间，而是系统版本号（system version number）。每开始一个新的事务，系统版本号都会自动递增。事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。

> 💡 **隐藏字段说明**
>
> > Internally, `InnoDB` adds three fields to each row stored in the database:
>
> InnoDB 存储引擎在每行数据的后面添加了三个隐藏字段：
>
> 1. **DB_TRX_ID**(6 字节)：表示最近一次对本记录行作修改（insert | update）的事务 ID。至于 delete 操作，InnoDB 认为是一个 update 操作，不过会更新一个另外的删除位，将行表示为 deleted。并非真正删除。
>
> 2. **DB_ROLL_PTR**(7 字节)：回滚指针，指向当前记录行的 undo log 信息
>
> 3. **DB_ROW_ID**(6 字节)：随着新行插入而单调递增的行 ID。理解：当表没有主键或唯一非空索引时，innodb 就会使用这个行 ID 自动产生聚簇索引。如果表有主键或唯一非空索引，聚簇索引就不会包含这个行 ID 了。**这个 DB_ROW_ID 跟 MVCC 关系不大。**
>
> [MySQL :: MySQL 8.0 Reference Manual :: 15.3 InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)

    > Internally, `InnoDB` adds three fields to each row stored in the database:

    	- A 6-byte `DB_TRX_ID` field indicates the transaction identifier for the last transaction that inserted or updated the row. Also, a deletion is treated internally as an update where a special bit in the row is set to mark it as deleted.
    	- A 7-byte `DB_ROLL_PTR` field called the roll pointer. The roll pointer points to an undo log record written to the rollback segment. If the row was updated, the undo log record contains the information necessary to rebuild the content of the row before it was updated.
    	- A 6-byte `DB_ROW_ID` field contains a row ID that increases monotonically as new rows are inserted. If `InnoDB` generates a clustered index automatically, the index contains row ID values. Otherwise, the `DB_ROW_ID` column does not appear in any index.

    InnoDB存储引擎在每行数据的后面添加了三个隐藏字段：


    1. **DB_TRX_ID**(6字节)：表示最近一次对本记录行作修改（insert | update）的事务ID。至于delete操作，InnoDB认为是一个update操作，不过会更新一个另外的删除位，将行表示为deleted。并非真正删除。


    2. **DB_ROLL_PTR**(7字节)：回滚指针，指向当前记录行的undo log信息


    3. **DB_ROW_ID**(6字节)：随着新行插入而单调递增的行ID。理解：当表没有主键或唯一非空索引时，innodb就会使用这个行ID自动产生聚簇索引。如果表有主键或唯一非空索引，聚簇索引就不会包含这个行ID了。**这个DB_ROW_ID跟MVCC关系不大。**


    [MySQL :: MySQL 8.0 Reference Manual :: 15.3 InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)

**REPEATABLE READ（可重读）隔离级别下 MVCC 如何工作：**

- SELECT：InnoDB 会根据以下两个条件检查每行记录：
- InnoDB 只查找版本早于当前事务版本的数据行，这样可以确保事务读取的行，要么是在开始事务之前已经存在要么是事务自身插入或者修改过的
- 行的删除版本号要么未定义，要么大于当前事务版本号，这样可以确保事务读取到的行在事务开始之前未被删除
- 只有符合上述两个条件的才会被查询出来
- INSERT：InnoDB 为新插入的每一行保存当前系统版本号作为行版本号
- DELETE：InnoDB 为删除的每一行保存当前系统版本号作为行删除标识
- UPDATE：InnoDB 为插入的一行新纪录保存当前系统版本号作为行版本号，同时保存当前系统版本号到原来的行作为删除标识

保存这两个额外系统版本号，使大多数操作都不用加锁。使数据操作简单，性能很好，并且也能保证只会读取到符合要求的行。不足之处是每行记录都需要额外的存储空间，需要做更多的行检查工作和一些额外的维护工作。

**MVCC 只在 COMMITTED READ（读提交）和 REPEATABLE READ（可重复读）两种隔离级别下工作**。

> 💡 **多版本与二级索引**  
> InnoDB 多版本并发控制 (MVCC) 对待**二级索引**的方式与**聚集索引**不同。 聚集索引中的记录就地更新(update in-place)，它们的隐藏系统列指向撤消日志条目，可以从中重建早期版本的记录。 与聚集索引记录不同，二级索引记录不包含隐藏的系统列，也不会就地更新。
>
> [MySQL :: MySQL 8.0 Reference Manual :: 15.3 InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)
>
> > InnoDB multiversion concurrency control (MVCC) treats secondary indexes differently than clustered indexes. Records in a clustered index are updated in-place, and their hidden system columns point undo log entries from which earlier versions of records can be reconstructed. Unlike clustered index records, secondary index records do not contain hidden system columns nor are they updated in-place.

    InnoDB 多版本并发控制 (MVCC) 对待**二级索引**的方式与**聚集索引**不同。 聚集索引中的记录就地更新(update in-place)，它们的隐藏系统列指向撤消日志条目，可以从中重建早期版本的记录。 与聚集索引记录不同，二级索引记录不包含隐藏的系统列，也不会就地更新。


    [MySQL :: MySQL 8.0 Reference Manual :: 15.3 InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)


    > InnoDB multiversion concurrency control (MVCC) treats secondary indexes differently than clustered indexes. Records in a clustered index are updated in-place, and their hidden system columns point undo log entries from which earlier versions of records can be reconstructed. Unlike clustered index records, secondary index records do not contain hidden system columns nor are they updated in-place.


    	When a secondary index column is updated, old secondary index records are delete-marked, new records are inserted, and delete-marked records are eventually purged. When a secondary index record is delete-marked or the secondary index page is updated by a newer transaction, InnoDB looks up the database record in the clustered index. In the clustered index, the record's DB_TRX_ID is checked, and the correct version of the record is retrieved from the undo log if the record was modified after the reading transaction was initiated.


    	If a secondary index record is marked for deletion or the secondary index page is updated by a newer transaction, the covering index technique is not used. Instead of returning values from the index structure, InnoDB looks up the record in the clustered index.


    	However, if the index condition pushdown (ICP) optimization is enabled, and parts of the WHERE condition can be evaluated using only fields from the index, the MySQL server still pushes this part of the WHERE condition down to the storage engine where it is evaluated using the index. If no matching records are found, the clustered index lookup is avoided. If matching records are found, even among delete-marked records, InnoDB looks up the record in the clustered index.

# 理解体会

多版本控制（Multiversion Concurrency Control）: 指的是一种提高并发的技术。最早的数据库系统，只有读读之间可以并发，读写，写读，写写都要阻塞。引入多版本之后，只有写写之间相互阻塞，其他三种操作都可以并行，这样大幅度提高了 InnoDB 的并发度。在内部实现中，InnoDB 通过 undo log 保存每条数据的多个版本，并且能够找回数据历史版本提供给用户读，每个事务读到的数据版本可能是不一样的。在同一个事务中，用户只能看到该事务创建快照之前已经提交的修改和该事务本身做的修改。

理解 InnoDB 中的 MVCC 非常关键，了解引入的背景已经如何实现的原理。

# 参考

[MySQL :: MySQL 8.0 Reference Manual :: 15.3 InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)

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
