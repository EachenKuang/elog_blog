---
password: ""
icon: ""
date: "2023-11-15"
type: Post
category: 行业概念
slug: industry-day55
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day55 【概念解析】InnoDB
status: Published
cover: "https://image.kuangyichen.com/image/innodb.jpeg"
urlname: f563fe3c-8ecd-43ec-9b3a-3ebe17239f8e
updated: "2023-11-15 11:29:00"
---

# 前言

> 💡 InnoDB 在 MySQL 5.5.5 之后，作为默认引擎的存在，是需要着重学习的，也是数据库相关领域考察的重点。

# 整理定义

## InnoDB 引擎概述

> **InnoDB 是 MySQL 默认的通用存储引擎**。默认情况下，InnoDB 将数据存储在一系列的数据文件中，这些文件统被称为表空间(tablespace)。表空间本质上是一个由 InnoDB 自己管理的黑盒。

    InnoDB使用**`MVCC`**来实现高并发性，并实现了所有4个SQL标准隔离级别。InnoDB默认为REPEATABLE READ隔离级别，并且通过间隙锁(next-key locking)策略来防止在这个隔离级别上的幻读：InnoDB不只锁定在查询中涉及的行，还会对索引结构中的间隙进行锁定，以防止幻行被插入。


    InnoDB表是基于**聚簇索引构建**的。InnoDB的索引结构与MySQL其他大部分存储引擎有很大的不同。聚簇索引提供了非常快速的主键查找。但是，因为二级索引（secondary index，非主键索引）需要包含主键列，如果主键较大，则其他索引也会很大。如果表中的索引较多，主键应当尽量小。


    InnoDB内部做了很多优化。其中包括从磁盘预取数据的可预测性预读、能够自动在内存中构建哈希索引以进行快速查找的自适应哈希索引(adaptive hash index)，以及用于加速插入操作的插入缓冲区(insert buffer)。


    ——《高性能MySQL》

# 复述展开

InnoDB 是 MySQL 的默认存储引擎，它是一个提供高性能、高可靠性和高并发的存储引擎。以下是我对 InnoDB 的一些理解：

1. **事务支持**：InnoDB 支持 ACID 事务，这是它最重要的特性之一。事务可以帮助保证数据的一致性和完整性。
2. **行级锁定**：InnoDB 支持行级锁定，这意味着在进行数据修改（如 UPDATE 或 DELETE）时，只有被修改的数据行被锁定，其他行仍然可以被其他事务访问。这大大提高了并发性能。
3. **支持外键**：InnoDB 支持外键和引用完整性，这是其他许多 MySQL 存储引擎不支持的。
4. **MVCC**：InnoDB 通过使用多版本并发控制（MVCC）来解决读-写冲突，从而使得读操作不会被写操作阻塞，提高了数据库的并发读写能力。
5. **数据恢复能力**：InnoDB 有很好的崩溃恢复能力，通过日志和事务的回滚操作，可以恢复到崩溃前的状态。
6. **支持 B+树索引**：InnoDB 使用 B+树作为索引结构，数据文件是索引的一部分，主键索引的叶节点就是数据节点。这种方式称为索引组织表。
7. **支持全文索引**：从 MySQL 5.6 版本开始，InnoDB 也开始支持全文索引，这使得它可以进行全文搜索。
8. **支持数据压缩**：从 MySQL 5.1 版本开始，InnoDB 支持表和索引的压缩，可以节省存储空间。

总的来说，InnoDB 是一个功能强大、性能高效的存储引擎，特别适合处理大量的读写操作和需要事务支持的应用。

## What is InnoDB?

InnoDB 是数据库管理系统 MySQL 和 MariaDB 的存储引擎。自 2010 年 MySQL 5.5.5 发布以来，它取代了 MyISAM 成为 MySQL 的默认表类型。

如果直接使用 `create table` 语句不带 `engine` 关键字，那么会创建一个 InnoDB 的表。

## InnoDB 的主要优势

- 其 DML 操作遵循 ACID 模型，事务具有提交、回滚和崩溃恢复功能，以保护用户数据。
- 行级锁定和 Oracle 风格的一致性读取提高了多用户并发性和性能。
- InnoDB 表在磁盘上排列数据以根据主键优化查询。 每个 InnoDB 表都有一个称为聚集索引的主键索引，用于组织数据以最大程度地减少主键查找的 I/O。
- 为了保持数据完整性，InnoDB 支持 FOREIGN KEY 约束。 使用外键，会检查插入、更新和删除，以确保它们不会导致相关表之间出现不一致。

## InnoDB 最佳实践

- 为每个表指定一个主键，使用最频繁查询的列或列，或者如果没有明显的主键，则使用自动增量值。
- 无论何时根据这些表的相同 ID 值从多个表中提取数据，都使用连接。为了快速的连接性能，在连接列上定义外键，并在每个表中用相同的数据类型声明这些列。添加外键确保引用的列被索引，这可以提高性能。外键还将删除和更新传播到所有受影响的表，并阻止在子表中插入数据，如果父表中不存在相应的 ID。
- 关闭自动提交。每秒提交数百次会限制性能（受存储设备的写速度限制）。
- 将相关的 DML 操作组合成事务，用 START TRANSACTION 和 COMMIT 语句将它们括起来。
- 不要使用 LOCK TABLES 语句。InnoDB 可以处理多个会话同时读写同一张表，而不牺牲可靠性或高性能。要获得一组行的独占写访问权限，使用 SELECT ... FOR UPDATE 语法仅锁定你打算更新的行。
- 启用 innodb_file_per_table 变量或使用通用表空间将表的数据和索引放入单独的文件中，而不是系统表空间。innodb_file_per_table 变量默认启用。
- 评估您的数据和访问模式是否受益于 InnoDB 表或页面压缩功能。您可以在不牺牲读/写能力的情况下压缩 InnoDB 表。
- 使用--sql_mode=NO_ENGINE_SUBSTITUTION 选项运行服务器，以防止使用您不想使用的存储引擎创建表。

> 💡 总之，使用 InnoDB 表时的最佳实践包括为每个表指定主键、使用连接、关闭自动提交、将 DML 操作分组到事务中、避免使用 LOCK TABLES 语句、启用 innodb_file_per_table 变量、评估数据压缩功能的适用性以及使用 NO_ENGINE_SUBSTITUTION SQL 模式。遵循这些最佳实践可以帮助您充分利用 InnoDB 存储引擎的性能和功能。

# 理解体会

[**InnoDB 存储引擎特性表**](https://dev.mysql.com/doc/refman/8.0/en/innodb-introduction.html)

| 特性                                                  | 是否支持                                                                  |
| ----------------------------------------------------- | ------------------------------------------------------------------------- |
| B-tree 索引                                           | 是                                                                        |
| 备份/点恢复（在服务器上实现，而不是在存储引擎上实现） | 是                                                                        |
| 集群数据库支持                                        | 否                                                                        |
| 聚簇索引                                              | 是                                                                        |
| 数据压缩                                              | 是                                                                        |
| 数据缓存                                              | 是                                                                        |
| 数据加密                                              | 是（通过服务器的加密函数实现；在 MySQL 5.7 及以后版本，支持数据静态加密） |
| 外键支持                                              | 是                                                                        |
| 全文搜索索引                                          | 是（在 MySQL 5.6 及以后版本，支持 FULLTEXT 索引）                         |
| 地理空间数据类型支持                                  | 是                                                                        |
| 地理空间索引支持                                      | 是（在 MySQL 5.7 及以后版本，支持地理空间索引）                           |
| 哈希索引                                              | 否（InnoDB 内部使用哈希索引作为其自适应哈希索引特性）                     |
| 索引缓存                                              | 是                                                                        |
| 锁定粒度                                              | 行级                                                                      |
| MVCC（多版本并发控制）                                | 是                                                                        |
| 复制支持（在服务器上实现，而不是在存储引擎上实现）    | 是                                                                        |
| 存储限制                                              | 64TB                                                                      |
| T-tree 索引                                           | 否                                                                        |
| 事务                                                  | 是                                                                        |
| 更新数据字典的统计信息                                | 是                                                                        |

与其他引擎对比：

| 特性                   | MyISAM     | Memory       | InnoDB     | Archive    | NDB        |
| ---------------------- | ---------- | ------------ | ---------- | ---------- | ---------- |
| B-tree 索引            | 是         | 是           | 是         | 否         | 否         |
| 备份/点恢复            | 是         | 是           | 是         | 是         | 是         |
| 集群数据库支持         | 否         | 否           | 否         | 否         | 是         |
| 聚簇索引               | 否         | 否           | 是         | 否         | 否         |
| 数据压缩               | 是（注 2） | 否           | 是         | 是         | 否         |
| 数据缓存               | 否         | N/A          | 是         | 否         | 是         |
| 数据加密               | 是（注 3） | 是（注 3）   | 是（注 4） | 是（注 3） | 是（注 5） |
| 外键支持               | 否         | 否           | 是         | 否         | 是         |
| 全文搜索索引           | 是         | 否           | 是（注 6） | 否         | 否         |
| 地理空间数据类型支持   | 是         | 否           | 是         | 是         | 是         |
| 地理空间索引支持       | 是         | 否           | 是（注 7） | 否         | 否         |
| 哈希索引               | 否         | 是           | 否（注 8） | 否         | 是         |
| 索引缓存               | 是         | N/A          | 是         | 否         | 是         |
| 锁定粒度               | 表级       | 表级         | 行级       | 行级       | 行级       |
| MVCC（多版本并发控制） | 否         | 否           | 是         | 否         | 否         |
| 复制支持               | 是         | 有限（注 9） | 是         | 是         | 是         |
| 存储限制               | 256TB      | RAM          | 64TB       | 无         | 384EB      |
| T-tree 索引            | 否         | 否           | 否         | 否         | 是         |
| 事务                   | 否         | 否           | 是         | 否         | 是         |
| 更新数据字典的统计信息 | 是         | 是           | 是         | 是         | 是         |

注：

1. 在服务器上实现，而不是在存储引擎上实现。
2. 只有在使用压缩行格式时，才支持压缩的 MyISAM 表。使用 MyISAM 的压缩行格式的表是只读的。
3. 通过服务器的加密函数实现。
4. 通过服务器的加密函数实现；在 MySQL 5.7 及以后版本，支持数据静态加密。
5. 通过服务器的加密函数实现；从 NDB 8.0.22 开始，支持加密的 NDB 备份；从 NDB 8.0.29 开始，支持透明的 NDB 文件系统加密。
6. 在 MySQL 5.6 及以后版本，支持 FULLTEXT 索引。
7. 在 MySQL 5.7 及以后版本，支持地理空间索引。
8. InnoDB 内部使用哈希索引作为其自适应哈希索引特性。

# 参考：

[MySQL :: MySQL 8.0 Reference Manual :: 15.1 Introduction to InnoDB](https://dev.mysql.com/doc/refman/8.0/en/innodb-introduction.html)

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
