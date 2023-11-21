---
password: ""
icon: ""
date: "2023-11-14"
type: Post
category: 行业概念
slug: industry-day54
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day54 【概念解析】MyISAM
status: Published
cover: "https://image.kuangyichen.com/image/MyISAM.jpg"
urlname: e1225d6a-d281-440a-918d-b165e0dfac69
updated: "2023-11-16 22:20:00"
---

# 前言

> 💡 `MyISAM = My + ISAM`，如同 `MySQL = My + SQL` 的命名方式一般，MyISAM 也是由 My 与 ISAM 合成而来。

# 整理定义

## What is ISAM?

**索引顺序存取方法**（**ISAM, Indexed Sequential Access Method**）最初是[IBM](https://zh.wikipedia.org/wiki/IBM)公司发展起来的一个[文件系统](https://zh.wikipedia.org/wiki/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)，可以连续地（按照他们进入的顺序）或者任意地（根据索引）记录任何访问。每个索引定义了一次不同排列的记录。现在这个概念用在许多场合：

- 特指 IBM 公司的 ISAM 产品
- [数据库](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93)系统中提供用户接口从数据文件中检索数据。
- 通常指，数据库的[索引](https://zh.wikipedia.org/wiki/%E7%B4%A2%E5%BC%95)，这种索引被大多数[数据库](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93)所采用，包括[关系数据库](https://zh.wikipedia.org/wiki/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93)或其它。

在 ISAM 系统，数据组织成有固定长度的记录，按顺序存储的。

## What is MyISAM?

**MyISAM**是[MySQL](https://zh.wikipedia.org/wiki/MySQL)的默认[数据库引擎](https://zh.wikipedia.org/wiki/%E8%B3%87%E6%96%99%E5%BA%AB%E5%BC%95%E6%93%8E)（5.5 版之前），由早期的[ISAM](https://zh.wikipedia.org/wiki/ISAM)所改良。虽然性能极佳，但却有一个缺点：不支持[事务处理](<https://zh.wikipedia.org/w/index.php?title=%E4%BA%8B%E5%8A%A1%E8%99%95%E7%90%86_(%E6%95%B8%E6%93%9A%E5%BA%AB)&action=edit&redlink=1>)（transaction）。不过，在这几年的发展下，[MySQL](https://zh.wikipedia.org/wiki/MySQL)也导入了[InnoDB](https://zh.wikipedia.org/wiki/InnoDB)（另一种数据库引擎），以强化[参照完整性](https://zh.wikipedia.org/w/index.php?title=%E5%8F%83%E8%80%83%E5%AE%8C%E6%95%B4%E6%80%A7&action=edit&redlink=1)与[并发违规处理](https://zh.wikipedia.org/w/index.php?title=%E4%B8%A6%E8%A1%8C%E9%81%95%E8%A6%8F%E8%99%95%E7%90%86&action=edit&redlink=1)机制，后来就逐渐取代 MyISAM。

每个 MyISAM 资料表，皆由存储在硬盘上的 3 个文件所组成，每个文件都以资料表名称为文件主名，并搭配不同扩展名区分文件类型：

1. `.frm`－－存储资料表定义，此文件非 MyISAM 引擎的一部分。
2. `.MYD`－－存放真正的资料。
3. `.MYI`－－存储索引信息。

# 复述展开

MyISAM 是 MySQL 的一种数据库引擎，（MyISAM 在 MySQL 5.5 版本之前还是默认引擎，之后是 innoDB），它由 ISAM 改良而来，支持全文索引，不支持事务 ACID，表级锁定。

# MySQL 中 innoDB VS MyISAM

| 特性/属性            | MyISAM                                          | InnoDB                                         |
| -------------------- | ----------------------------------------------- | ---------------------------------------------- |
| **事务支持**         | 不支持                                          | 支持，遵循 ACID 原则                           |
| **锁定粒度**         | 表级锁定                                        | 行级锁定                                       |
| **全文索引**         | 支持                                            | MySQL 5.6 及以上版本支持                       |
| **外键约束**         | 不支持                                          | 支持                                           |
| **哈希索引**         | 不支持                                          | 不支持                                         |
| **数据存储方式**     | 磁盘                                            | 磁盘                                           |
| **MVCC**             | 不支持                                          | 支持                                           |
| **空间使用**         | 相对较小                                        | 相对较大，因为 InnoDB 会为每个表创建一个表空间 |
| **数据安全性**       | 较低，崩溃后恢复困难                            | 较高，支持事务和崩溃后的自动恢复               |
| **读写性能**         | 读取速度较快，但写入时需要锁定整个表            | 读写性能都很高，特别是在并发环境下             |
| **数据缓存**         | 只缓存索引                                      | 缓存索引和行数据                               |
| **BLOB/TEXT 字段**   | 可以被索引和搜索                                | 只有前 768 字节可以被索引                      |
| **表的物理组织形式** | MyISAM 表是表级碎片，可以进行表级别的优化和修复 | InnoDB 表是行级碎片，无法进行优化和修复        |
| **适用场景**         | 读操作多于写操作，不需要事务支持的应用          | 大量读写操作，需要事务支持的应用               |

# 参考：

[MySQL :: MySQL 8.0 Reference Manual :: 16.2 The MyISAM Storage Engine](https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html)

MySQL 中各个引擎的对比：【[Comparison of MySQL database engines - Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_MySQL_database_engines)】

| Name                                                                                                   | Vendor                                           | License                                                               | [Transactional](https://en.wikipedia.org/wiki/Database_transaction#Transactional_databases) | Under active development | MySQL versions | MariaDB versions |
| ------------------------------------------------------------------------------------------------------ | ------------------------------------------------ | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------ | -------------- | ---------------- |
| [ColumnStore (formerly InfiniDB)](https://en.wikipedia.org/wiki/InfiniDB)                              | Calpont                                          | GPL                                                                   | Yes                                                                                         | Yes                      | None           | 10.5.4 - present |
| [MyRocks](https://en.wikipedia.org/wiki/MyRocks)                                                       | Facebook                                         | GPLv2                                                                 | Yes                                                                                         | Yes                      | None           | 10.2 - present   |
| Mroonga                                                                                                | Groonga Project                                  | GPL                                                                   | No                                                                                          | Yes                      | None           | 10.0 - present   |
| [SPIDER](<https://en.wikipedia.org/w/index.php?title=SPIDER_(storage_engine)&action=edit&redlink=1>)   | Kentoku Shiba                                    | GPL                                                                   | Yes                                                                                         | Yes                      | None           | 10.0 - present   |
| [Aria](<https://en.wikipedia.org/wiki/Aria_(storage_engine)>)                                          | [MariaDB](https://en.wikipedia.org/wiki/MariaDB) | GPL                                                                   | No                                                                                          | Yes                      | None           | 5.1 - present    |
| [CONNECT](<https://en.wikipedia.org/w/index.php?title=CONNECT_(storage_engine)&action=edit&redlink=1>) | [MariaDB](https://en.wikipedia.org/wiki/MariaDB) | GPL                                                                   | No                                                                                          | Yes                      | None           | 10.0 - present   |
| [FederatedX](https://en.wikipedia.org/w/index.php?title=FederatedX&action=edit&redlink=1)              | [MariaDB](https://en.wikipedia.org/wiki/MariaDB) | GPL                                                                   | Yes                                                                                         | No                       | None           | ? - present      |
| [S3](<https://en.wikipedia.org/w/index.php?title=S3_(storage_engine)&action=edit&redlink=1>)           | [MariaDB](https://en.wikipedia.org/wiki/MariaDB) | GPL                                                                   | No                                                                                          | Yes                      | None           | 10.5 - present   |
| SEQUENCE                                                                                               | MariaDB                                          | GPL                                                                   | No                                                                                          | Yes                      | None           | 10.0 - present   |
| [Archive](https://en.wikipedia.org/wiki/MySQL_Archive)                                                 | Oracle                                           | GPL                                                                   | No                                                                                          | Yes                      | 5.0 - present  | 5.1 - present    |
| [Berkeley DB](https://en.wikipedia.org/wiki/Berkeley_DB)                                               | Oracle                                           | [AGPLv3](https://en.wikipedia.org/wiki/Affero_General_Public_License) | Yes                                                                                         | No                       | ? - 5.0        | None             |
| [BLACKHOLE](https://en.wikipedia.org/w/index.php?title=BLACKHOLE&action=edit&redlink=1)                | Oracle                                           | GPL                                                                   | No                                                                                          | Yes                      | 5.0 - present  | 5.1 - present    |
| [CSV](<https://en.wikipedia.org/w/index.php?title=CSV_(storage_engine)&action=edit&redlink=1>)         | Oracle                                           | GPL                                                                   | No                                                                                          | Yes                      | 5.0 - present  | 5.1 - present    |
| [Falcon](<https://en.wikipedia.org/wiki/Falcon_(storage_engine)>)                                      | Oracle                                           | GPL                                                                   | Yes                                                                                         | No                       | ?              | None             |
| [Federated](https://en.wikipedia.org/wiki/MySQL_Federated)                                             | Oracle                                           | GPL                                                                   | ?                                                                                           | No                       | 5.0 - present  | ?                |
| [InnoDB](https://en.wikipedia.org/wiki/InnoDB)                                                         | Oracle                                           | GPL                                                                   | Yes                                                                                         | Yes                      | 3.23 - present | 5.1 - present    |
| [MEMORY](<https://en.wikipedia.org/wiki/MEMORY_(storage_engine)>)                                      | Oracle                                           | GPL                                                                   | No                                                                                          | Yes                      | 3.23 - present | 5.1 - present    |
| [MyISAM](https://en.wikipedia.org/wiki/MyISAM)                                                         | Oracle                                           | GPL                                                                   | No                                                                                          | No                       | 3.23 - present | 5.1 - present    |
| [NDB](https://en.wikipedia.org/wiki/MySQL_Cluster)                                                     | Oracle                                           | GPLv2                                                                 | Yes                                                                                         | Yes                      | ?              | None             |
| [OQGRAPH](https://en.wikipedia.org/w/index.php?title=OQGRAPH&action=edit&redlink=1)                    | Oracle                                           | GPLv2                                                                 | No                                                                                          | No                       | None           | 5.2 - present    |
| [TempTable](https://en.wikipedia.org/w/index.php?title=TempTable&action=edit&redlink=1)                | Oracle                                           | GPL                                                                   | No                                                                                          | Yes                      | 8.0 - present  | None             |
| [TokuDB](https://en.wikipedia.org/wiki/TokuDB)                                                         | Percona                                          | Modified GPL                                                          | Yes                                                                                         | No                       | None           | 5.5 - present    |
| [XtraDB](https://en.wikipedia.org/wiki/XtraDB)                                                         | Percona                                          | GPL                                                                   | Yes                                                                                         | Yes                      | None           | 5.1 - 10.1       |
| [Sphinx](<https://en.wikipedia.org/wiki/Sphinx_(search_engine)#SphinxSE>)                              | Sphinx Technologies Inc.                         | GPL                                                                   | No                                                                                          | No                       | None           | 5.2 - present    |

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
