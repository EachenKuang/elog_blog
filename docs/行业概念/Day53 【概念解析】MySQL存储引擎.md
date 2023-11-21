---
password: ""
icon: ""
date: "2023-11-13"
type: Post
category: 行业概念
slug: industry-day53
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day53 【概念解析】MySQL存储引擎
status: Published
cover: "https://image.kuangyichen.com/image/database-engines.webp"
urlname: 39aff7c0-c412-4bd0-a309-8739a1542c17
updated: "2023-11-15 11:27:00"
---

# MySQL 存储引擎概述

> 存储引擎是 MySQL 的组件，用于处理不同表类型的 SQL 操作。不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能，使用不同的存储引擎，还可以获得特定的功能。

    使用哪一种引擎可以灵活选择，**一个数据库中多个表可以使用不同引擎以满足各种性能和实际需求**，使用合适的存储引擎，将会提高整个数据库的性能 。


    MySQL服务器使用可插拔的存储引擎体系结构，可以从运行中的 MySQL 服务器加载或卸载存储引擎 。


    —— [如何系统学习 MySQL](https://www.zhihu.com/question/21760988/answer/1935044903?utm_psn=1707896912420855808)

MySQL 中的存储引擎决定了数据库如何存储数据、如何为数据建立索引以及如何进行数据的插入、查询、更新和删除操作。以下是一些常见的 MySQL 存储引擎：

1. **InnoDB**：这是 MySQL 的默认存储引擎，它提供了诸如事务支持、行级锁定、外键约束、MVCC（多版本并发控制）等高级功能。InnoDB 引擎特别适合处理大量的读写操作的应用。
2. **MyISAM**：这是 MySQL 的另一种存储引擎，它提供了全文搜索和压缩等功能，但不支持事务和行级锁定。MyISAM 引擎特别适合用于读取操作远多于写入操作，或者不需要事务支持的应用。
3. **Memory**：这种存储引擎将所有数据存储在内存中，因此数据的读写速度非常快。但是，如果 MySQL 服务器重启，所有的数据都会丢失。Memory 引擎适合用于存储临时数据的临时表。
4. **CSV**：这种存储引擎将数据存储在 CSV 文件中。CSV 引擎允许用户直接使用 CSV 文件，而无需进行导入/导出操作。
5. **Archive**：这种存储引擎用于存储和检索大量的归档数据。Archive 引擎支持高速的插入操作，但不支持索引。
6. **Federated**：这种存储引擎允许 MySQL 服务器将一张表的数据存储在远程的另一台 MySQL 服务器上，就像它就在本地一样。
7. **Blackhole**：这种存储引擎不会存储任何数据，但会记录所有的数据更改操作。Blackhole 引擎主要用于在复制过程中记录二进制日志。
8. **NDB 或 NDBCLUSTER**：这是一个分布式存储引擎，用于创建冗余数据存储，以实现高可用性。

每种存储引擎都有其特定的用途，选择哪种存储引擎取决于你的特定需求。

# 存储引擎对比

| 存储引擎  | 事务支持 | 锁定粒度 | 是否支持全文索引 | 是否支持外键 | 是否支持哈希索引 | 数据存储方式 | 适用场景                                     |
| --------- | -------- | -------- | ---------------- | ------------ | ---------------- | ------------ | -------------------------------------------- |
| InnoDB    | 是       | 行级     | 是               | 是           | 否               | 磁盘         | 大量读写操作，需要事务支持                   |
| MyISAM    | 否       | 表级     | 是               | 否           | 否               | 磁盘         | 读操作多于写操作，不需要事务支持             |
| Memory    | 否       | 表级     | 否               | 否           | 是               | 内存         | 存储临时数据，需要快速读写                   |
| CSV       | 否       | 行级     | 否               | 否           | 否               | 磁盘         | 数据交换，可以直接用文本编辑器查看和编辑数据 |
| Archive   | 否       | 行级     | 否               | 否           | 否               | 磁盘         | 存储和检索大量的归档数据                     |
| Federated | 否       | 表级     | 否               | 否           | 否               | 磁盘         | 数据分布在不同的 MySQL 服务器上              |
| Blackhole | 否       | 表级     | 否               | 否           | 否               | 无           | 记录所有的数据更改操作，主要用于复制         |
| NDB       | 是       | 行级     | 否               | 是           | 是               | 磁盘         | 创建冗余数据存储，实现高可用性               |

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
