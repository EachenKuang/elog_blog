---
password: ""
icon: ""
date: "2023-10-05"
type: Post
category: 行业概念
slug: industry-day14
tags:
  - 行业概念
  - 文字
  - 思考
summary: ""
title: Day14【概念解析】数据库
status: Published
cover: "https://source.unsplash.com/random/720x480/?encryption"
urlname: a085ecb4-0199-4862-a75b-752996e2efd1
updated: "2023-10-27 19:22:00"
---

# 整理定义

中文名：数据库

英文名：Database

| 来源                                                                                                                   | 定义                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [TechTarget](https://searchdatamanagement.techtarget.com/definition/database)                                          | 数据库是一个组织的数据集合，它被设计为支持和满足特定组织的各种应用需求。数据库的主要目标是提供一种方式来存储和检索数据库信息，这种方式既可以方便又可以快速。                                                                                                                                                                                       |
| [Microsoft](https://docs.microsoft.com/en-us/sql/relational-databases/databases/databases?view=sql-server-ver15)       | 数据库是一个用于存储和管理数据的集合。它可以用来存储各种类型的信息，如产品目录、客户信息、业务交易等。                                                                                                                                                                                                                                             |
| [Wikipedia](https://en.wikipedia.org/wiki/Database)                                                                    | 数据库是一个组织的数据集合，它被设计为支持和满足特定组织的各种应用需求。数据库的主要目标是提供一种方式来存储和检索数据库信息，这种方式既可以方便又可以快速。                                                                                                                                                                                       |
| [Britannica](https://www.britannica.com/technology/database-computing)                                                 | 数据库是一个用于存储和管理数据的集合。它可以用来存储各种类型的信息，如产品目录、客户信息、业务交易等。                                                                                                                                                                                                                                             |
| [百度百科](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93/103728)                                            | 数据库是按照数据结构来组织、存储和管理数据的仓库。每个数据库都有一个或多个不同的 API 用于创建、访问、管理、搜索和复制所保存的数据。                                                                                                                                                                                                                |
| [数据库系统原理及应用教程（第 5 版)](https://weread.qq.com/web/reader/3f832df072015c8e3f8accbke4d32d5015e4da3b7fbb1fa) | 数据库（Database，DB）是一个按数据结构来存储和管理数据的计算机软件系统。数据库的概念实际上包括两层意思：数据库是一个实体，它是能够合理保管数据的“仓库”，用户在该“仓库”中存放要管理的事物数据，“数据”和“库”两个概念结合成为“数据库”；数据库是数据管理的新方法和技术，它能够更合理地组织数据、更方便地维护数据、更严密地控制数据和更有效地利用数据。 |
| 软件设计师教程（第 5 版）                                                                                              | 数据库 (DataBase，DB)。数据库是统一管理的、长期储存在计算机内的、有组织的 相关数据的集合。其特点是数据问联系密切、元余度小、独立性较高、易扩展，并且可为各类用户共享。                                                                                                                                                                             |

以上定义可能会有所不同，但主要的概念是一致的：数据库是一个用于存储和管理数据的集合，它被设计为支持和满足特定组织的各种应用需求。

# 复述展开

### 数据与信息

> “信息”（Information）和“数据”（Data）是两种非常重要的东西。“信息”可以告诉人们有用的事实和知识，“数据”可以更有效地表示、存储和抽取信息。  
> ——《数据库系统原理及应用教程》（第 5 版）

> **数据**是描述事物的符号记录，它具有多种表现形式，可以是文字、图形、图像、声音和语言等。信息是现实世界事物的存在方式或状态的反映。

    **信息**具有可感知、可存储、可加工、可传递和可再生等自然属性，信息己是社会各行各业不可缺少的资源，这也是信息的社会属性。 **数据是信息的符号表示，而信息是具有特定释义和意义的数据 。**


    ——《软件设计师教程》（第5版）

【这两个概念在后续的进行展开说明】

### 与数据库相关的概念

**数据库技术**是研究数据库的结构、存储、设计、管理和应用的一门软件学科。

**数据库系统，**一个数据库系统应由<u>计算机硬件</u>、<u>数据库</u>、<u>数据库管理系统</u>、<u>数据库应用系统</u>和<u>数据库管理员</u>5 部分构成。【《数据库系统原理及应用教程》（第 5 版）】

**数据库系统 (DataBase System，DBS )**是 一个采用了数据库技术，有组织地、动态地存储 大量相关数据，方便多用户访问的计算机系统。广义上讲，DBS 是由数据库、硬件、软件和人员组成的。【《软件设计师教程》（第 5 版）】

**数据库管理系统（Database Management System，DBMS）**是专门用于管理数据库的计算机系统软件。数据库管理系统能够为数据库提供数据的定义、建立、维护、查询和统计等操作功能，并完成对数据完整性、安全性进行控制的功能。

凡使用数据库技术管理数据（信息）的系统都称为**数据库应用系统（Database Application System）**。一个数据库应用系统应携带有较大的数据量，否则它就不需要数据库管理。数据库应用系统按其实现的功能可以被划分为 3 类系统，即数据传递系统、数据处理系统和管理信息系统。

# 理解体会

1、数据库技术是软件工程中的一门重要的学科，它研究数据库的结构、存储、设计、管理以及应用，需要理解数据库的底层原理，数据库系统、数据库管理系统等重要概念，然后构建一个关于数据库原理的重要架构图。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/01703ad7-6831-4aaf-be01-332a9ac431bb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120603Z&X-Amz-Expires=3600&X-Amz-Signature=ef1a84949ebe3788ae07269362808453515c72739a5e642c8be2a429c9e2f25c&X-Amz-SignedHeaders=host&x-id=GetObject)

2、实际上，我们常见的几种数据库软件如 MySQL、Sql Server、Oracle，实质上都属于数据库管理系统（DBMS），他们都提供了服务端、客户端、数据库本身的能力。而一般的，我们在开发过程中，都使用了**数据库**这个概念来进行代替，实际上指代的是数据库管理系统。

3、数据库作为一个大类，它的下级概念有很多，都可能单独开个专题来研究。后续针对工作本身以及兴趣，可以对一些常见的 DBMS 进行概念分析，诸如 MySQL、SQL Server、Oracle，以及非关系型数据库如 Redis、ES 等等。

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
