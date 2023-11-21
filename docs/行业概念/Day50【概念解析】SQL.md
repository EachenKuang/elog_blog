---
password: ""
icon: ""
date: "2023-11-10"
type: Post
category: 行业概念
slug: industry-day50
tags:
  - 行业概念
  - 文字
  - 思考
  - SQL
summary: ""
title: Day50【概念解析】SQL
status: Published
cover: "https://images.unsplash.com/photo-1662027044921-6febc57a0c53?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: bacdfe9c-3249-4ccd-ae5a-73feadf296d5
updated: "2023-11-12 10:54:00"
---

# 整理定义

## 定义 1

> **SQL**(发音为字母 S-Q-L 或 sequel)是结构化查询语言(Structured Query Language)的缩写。SQL 是一种专门用来与数据库通信的语言。

    ——《SQL必知必会》

## 定义 2

> Codd 基于对关系模型的定义，他提出了一种叫作 DSL/Alpha 的语言，用于操作关系数据表中的数据。在 Codd 的论文发表后不久，IBM 委托一个小组根据 Codd 的想法建立一个原型。这个小组创建了 DSL/Alpha 的简化版本 SQUARE。经过对 SQUARE 的改进，产生了 SEQUEL 语言，最终该语言被命名为 SQL。尽管 SQL 最初是用于操作关系型数据库中的数据，但如今已经演变为一种可以处理各种数据库技术的语言。

    SQL现在已有40多年的历史，其间经历了很大的变化。20世纪80年代中期，美国国家标准协会（American National Standards Institute，ANSI）开始制定SQL语言的第一个标准，该标准最终于1986年发布。随后经过改进，陆续在1989年、1992年、1999年、2003年、2006年、2008年、2011年、2016年发布了新版本。在改良核心语言的同时，SQL语言中还添加了一些新特性，引入了面向对象功能等。之后的标准侧重于相关技术的集成，比如可扩展标记语言（XML）和JavaScript对象表示法（JSON）。


    ——《SQL学习指南》

# 复述展开

## 什么是 SQL？

> 💡 SQL 是 Structured Query Language 的缩写，读音 S-Q-L 或者 sequel。它是一种用于编程的特定领域的结构化语言，旨在管理关系型数据库系统。

## SQL 的优点

❑ SQL 不是某个特定数据库厂商专有的语言。绝大多数重要的 DBMS 支持 SQL，所以学习此语言使你几乎能与所有数据库打交道。

❑ SQL 简单易学。它的语句全都是由有很强描述性的英语单词组成，而且这些单词的数目不多。

❑ SQL 虽然看上去很简单，但实际上是一种强有力的语言，灵活使用其语言元素，可以进行非常复杂和高级的数据库操作。

# 理解体会

SQL 是一种用于数据库的结构化语言，它也是一种标准，1986 年成为 ANSI 标准，1987 年成为 ISO 标准，1992 年，ISO 和 IEC 发布了 SQL 的国际标准，成为 SQL-92，现在最新的是[**ISO/IEC 9075-1:2023**](https://www.iso.org/standard/76583.html)。

许多 DBMS 厂商通过增加语句或指令，对 SQL 进行了扩展。这种扩展的目的是提供执行特定操作的额外功能或简化方法。虽然这种扩展很有用，但一般都是针对个别 DBMS 的，很少有两个厂商同时支持这种扩展。标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。所有主要的 DBMS，即使有自己的扩展，也都支持 ANSI SQL。各个实现有自己的名称，如 Oracle 的 PL/SQL、微软 SQL Server 用的 Transact-SQL 等。

学好了标准的 SQL，不管后续使用其他的 RDBMS，都能轻松自如。

## 标准

| Year | Name                                               | Alias                                                                               | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---- | -------------------------------------------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1986 | SQL-86                                             | SQL-87                                                                              | First formalized by ANSI                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 1989 | SQL-89                                             | [FIPS](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard) 127-1 | Minor revision that added integrity constraints adopted as FIPS 127-1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 1992 | [SQL-92](https://en.wikipedia.org/wiki/SQL-92)     | SQL2, FIPS 127-2                                                                    | Major revision (ISO 9075), *Entry Level* SQL-92 adopted as FIPS 127-2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 1999 | [SQL:1999](https://en.wikipedia.org/wiki/SQL:1999) | SQL3                                                                                | Added regular expression matching, [recursive queries](https://en.wikipedia.org/wiki/Hierarchical_and_recursive_queries_in_SQL) (e.g., [transitive closure](https://en.wikipedia.org/wiki/Transitive_closure)), [triggers](https://en.wikipedia.org/wiki/Database_trigger), support for procedural and control-of-flow statements, nonscalar types (arrays), and some object-oriented features (e.g., [structured types](https://en.wikipedia.org/wiki/Structured_type)), support for embedding SQL in Java ([SQL/OLB](https://en.wikipedia.org/wiki/SQL/OLB)) and vice versa ([SQL/JRT](https://en.wikipedia.org/wiki/SQL/JRT)) |
| 2003 | [SQL:2003](https://en.wikipedia.org/wiki/SQL:2003) |                                                                                     | Introduced [XML](https://en.wikipedia.org/wiki/XML)-related features ([SQL/XML](https://en.wikipedia.org/wiki/SQL/XML)), [window functions](https://en.wikipedia.org/wiki/SQL_window_function), standardized sequences, and columns with autogenerated values (including identity columns)                                                                                                                                                                                                                                                                                                                                       |
| 2006 | [SQL:2006](https://en.wikipedia.org/wiki/SQL:2006) |                                                                                     | ISO/IEC 9075-14:2006 defines ways that SQL can be used with XML. It defines ways of importing and storing XML data in an SQL database, manipulating it within the database, and publishing both XML and conventional SQL data in XML form. In addition, it lets applications integrate queries into their SQL code with [XQuery](https://en.wikipedia.org/wiki/XQuery), the XML Query Language published by the World Wide Web Consortium ([W3C](https://en.wikipedia.org/wiki/W3C)), to concurrently access ordinary SQL-data and XML documents.[[31]](https://en.wikipedia.org/wiki/SQL#cite_note-SQLXML2006-34)               |
| 2008 | [SQL:2008](https://en.wikipedia.org/wiki/SQL:2008) |                                                                                     | Legalizes ORDER BY outside cursor definitions. Adds INSTEAD OF triggers, TRUNCATE statement,[[32]](https://en.wikipedia.org/wiki/SQL#cite_note-iablog.sybase.com-paulley-35) FETCH clause                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 2011 | [SQL:2011](https://en.wikipedia.org/wiki/SQL:2011) |                                                                                     | Adds temporal data (PERIOD FOR)[[33]](https://en.wikipedia.org/wiki/SQL#cite_note-feature_temporal-36) (more information at [Temporal database#History](https://en.wikipedia.org/wiki/Temporal_database#History)). Enhancements for [window functions](https://en.wikipedia.org/wiki/SQL_window_function) and FETCH clause.[[34]](https://en.wikipedia.org/wiki/SQL#cite_note-features_2011-37)                                                                                                                                                                                                                                  |
| 2016 | [SQL:2016](https://en.wikipedia.org/wiki/SQL:2016) |                                                                                     | Adds row pattern matching, polymorphic table functions, operations on [JSON](https://en.wikipedia.org/wiki/JSON) data stored in character string fields                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 2019 | SQL:2019                                           |                                                                                     | Adds Part 15, multidimensional arrays (MDarray type and operators)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 2023 | [SQL:2023](https://en.wikipedia.org/wiki/SQL:2023) |                                                                                     | Adds data type JSON (SQL/Foundation); Adds Part 16, Property Graph Queries (SQL/PGQ)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

# 参考

[ISO/IEC 9075-1:2023 - Information technology — Database languages SQL — Part 1: Framework (SQL/Framework)](https://www.iso.org/standard/76583.html)

《SQL 必知必会》

《SQL 经典实例》

《SQL 学习指南》

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
