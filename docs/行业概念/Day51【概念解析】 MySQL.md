---
password: ""
icon: ""
date: "2023-11-11"
type: Post
category: 行业概念
slug: industry-day51
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
  - SQL
summary: ""
title: Day51【概念解析】 MySQL
status: Published
cover: "https://images.unsplash.com/photo-1662026911591-335639b11db6?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 62986b24-a3f1-4b6e-a44a-f18d332c8644
updated: "2023-11-12 11:55:00"
---

# 整理定义

## What is MySQL？

MySQL 是一种 DBMS（数据库管理系统），即它是一种数据库软件。

## MySQL 的由来

它的名称由两部分组成：My + SQL。`My`是 MySQL 的创建者 [Michael Widenius](https://en.wikipedia.org/wiki/Michael_Widenius)'s 的女儿 My 的名字，`SQL`就是 [Structured Query Language](https://en.wikipedia.org/wiki/Structured_Query_Language)。

## MySQL 的详细信息

MySQL 是由 C，C++编写而成，可以跨平台使用（Linux，Solaris，MacOS，Windows，FreeBSD）。最早的版本可以追溯到 1995 年 5 月 23 日。

MySQL 是一款免费并且开源的软件，基于 GNU 公共协议。MySQL 由瑞典公司 MySQL AB 拥有和赞助，该公司被 Sun Microsystems（现为 Oracle Corporation）收购。 2010 年，当 Oracle 收购 Sun 时，Widenius Fork 了开源 MySQL 项目来创建 MariaDB。

# 复述展开

> **MySQL 简介**

    MySQL、Oracle以及Microsoft SQL Server等数据库是基于<u>**客户机-服务器**</u>的数据库。客户机-服务器应用分为两个不同的部分。

    - `服务器`部分（Server）是负责所有数据访问和处理的一个软件。这个软件运行在称为数据库服务器的计算机上。
    - `客户机`部分（Client）是与用户打交道的软件。

    服务器软件为MySQL DBMS。你可以在本地安装的副本上运行， 也可以连接到运行在你具有访问权的远程服务器上的一个副本。


    客户机可以是MySQL提供的工具、脚本语言(如Perl)、Web应用 开发语言(如ASP、ColdFusion、JSP和PHP)、程序设计语言(如 C、C++、Java)等。

如果想直到如何操作和使用 MySQL 可以参考这篇文章《MySQL 必知必会笔记》

[bookmark](https://kuangyichen.com/article/mysql-crash-course)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/f2381ed0-d3d2-4df4-b1d1-bda08b394e9b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120344Z&X-Amz-Expires=3600&X-Amz-Signature=3ee18ffa1179ee94700eefc7993a9d002d0fe29a0997aecc523eb534f342dc66&X-Amz-SignedHeaders=host&x-id=GetObject)

## 关于 MyISAM 与 InnoDB

**MySQL 5.5** 之前，`MyISAM` 引擎是 MySQL 的默认存储引擎，可谓是风光一时。

虽然，`MyISAM` 的性能还行，各种特性也还不错（比如全文索引、压缩、[空间函数](https://www.zhihu.com/search?q=%E7%A9%BA%E9%97%B4%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2419920557%7D)等）。但是，`MyISAM` 不支持事务和行级锁，而且最大的缺陷就是崩溃后无法安全恢复。

**5.5 版本之后**，MySQL 引入了 `InnoDB`（事务性数据库引擎），MySQL 5.5 版本后默认的存储引擎为 `InnoDB`。

| 类型     | InnoDB     | MyISAM       |
| -------- | ---------- | ------------ |
| MVCC     | 支持       | 不支持       |
| 事务支持 | 支持       | 不支持       |
| 外键     | 支持       | 不支持       |
| 表锁差异 | 支持行级锁 | ☞ 支持表级锁 |
| 全文索引 | 不支持     | 支持         |

# 理解体会

目前在关系型数据库这一块，MySQL 是数据库学习的必经之路，要学会如何使用不难，难的时还得了解其中的原理与实际应用的性能调优。后续针对 MySQL 进行展开，一方面重温下 MySQL 的原理，另一方面也巩固一些调优的经验。

# 参考

《MySQL 必知必会》

[【笔记】MySQL 必知必会 | 易浅小站 (kuangyichen.com)](https://kuangyichen.com/article/mysql-crash-course)

[MySQL Crash Course – Ben Forta](https://forta.com/books/0672327120/)

[MySQL](https://www.mysql.com/cn/)

[MySQL - Wikipedia](https://en.wikipedia.org/wiki/MySQL)

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
