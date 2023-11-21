---
password: ""
icon: ""
date: "2023-11-20"
type: Post
category: 行业概念
slug: industry-day60
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day60【概念解析】InnoDB Change Buffer
status: Published
cover: "https://image.kuangyichen.com/image/innodb%20change%20buffer.webp"
urlname: 9d0ce237-43af-4dce-8f99-4f86a900b26e
updated: "2023-11-20 20:10:00"
---

# 整理定义

> The general term for the features involving the ***change buffer***, consisting of ***insert buffering***, ***delete buffering***, and ***purge buffering***. Index changes resulting from SQL statements, which could normally involve random I/O operations, are held back and performed periodically by a background ***thread***. This sequence of operations can write the disk blocks for a series of index values more efficiently than if each value were written to disk immediately. Controlled by the [`innodb_change_buffering`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_change_buffering) and [`innodb_change_buffer_max_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_change_buffer_max_size) configuration options.

**Change Buffer**是一种特殊的数据结构，当二级索引页不在**Buffer Pool**中时，它会缓存对这些页的更改。 **Change Buffer**可能由 INSERT、UPDATE 或 DELETE 操作 (DML) 产生，稍后当其他读取操作将页面加载到缓冲池中时，这些更改将被 **Merge** 合并。

![innodb-change-buffer.png](https://image.kuangyichen.com/image/innodb-change-buffer.png)

# 复述展开

> 🎈 **为什么需要 Change Buffer？**

与聚簇索引不同，二级索引通常是非唯一的，并且对二级索引的插入操作发生在相对随机的顺序中。同样，删除和更新操作可能会影响不相邻的二级索引页。在其他操作将受影响的页读入缓冲池时，合并缓存中的更改可以避免需要从磁盘读取二级索引页的大量随机访问 I/O。

> 🎈 **Change Buffer 的清理与更新机制？**

定期地，在系统大部分空闲或慢速关闭时运行的清理操作将更新的索引页写入磁盘。清理操作可以更高效地写入一系列索引值的磁盘块，而不是立即将每个值写入磁盘。

当有许多受影响的行和众多二级索引需要更新时，更改缓冲合并可能需要几个小时。在此期间，磁盘 I/O 增加，可能会导致磁盘限制的查询显著减慢。更改缓冲合并可能会在事务提交后继续发生，甚至在服务器关闭和重启后也会继续发生。

> 🎈 **Change Buffer 在内存中与磁盘上所在的位置**

在内存中，更改缓冲占用缓冲池的一部分。在磁盘上，更改缓冲是系统表空间的一部分，在数据库服务器关闭时缓冲索引更改。

> 🎈 **两个关键配置**

在配置 Change Buffer 时，有几个配置项需要注意：

1. `innodb_change_buffering`：这个配置项用于控制哪些类型的操作可以被缓存在 Change Buffer 中。默认值是"all"，表示所有类型的操作都可以被缓存。你可以根据你的应用的特性来调整这个配置项。关于配置的具体内容，可以查看附录。
2. `innodb_change_buffer_max_size`：这个配置项用于控制 Change Buffer 可以使用的内存的最大比例。默认值是 25，表示 Change Buffer 可以使用 InnoDB 缓冲池的 25%的内存。**如果你的应用有大量的插入、删除和更新操作，你可能需要增加这个值**。

> 🎈 Change Buffer 启用的场景

因为它可以减少磁盘读取和写入，所以更改缓冲对于 I/O 密集型工作负载最有价值； 例如，**具有大量 DML 操作**（例如批量插入）的应用程序可以从更改缓冲中受益。

但是，更改缓冲区占用了缓冲池的一部分，从而减少了可用于缓存数据页的内存。 如果工作集几乎适合缓冲池，或者表的二级索引相对较少，则禁用更改缓冲可能会很有用。 如果工作数据集完全适合缓冲池，则更改缓冲不会带来额外的开销，因为它仅适用于不在缓冲池中的页面。

# 理解体会

Change Buffer 是 MySQL 数据库 InnoDB 存储引擎的一个特性，主要用于优化磁盘 I/O 操作。当 InnoDB 需要修改一个数据页，但该数据页当前不在内存中时，InnoDB 可以选择将这些修改操作（如插入、删除和更新操作）缓存在 Change Buffer 中，而不是立即将数据页加载到内存中。然后，在后台或者在数据页被加载到内存中时，InnoDB 会将这些修改操作应用到数据页上。

**Change Buffer 的主要作用是减少磁盘 I/O 操作，提高数据库的性能**。特别是在大量插入、删除和更新操作的场景下，Change Buffer 可以显著提高数据库的性能。

使用 Change Buffer 的方式很简单，你只需要在你的 MySQL 配置文件中设置上述的配置项，然后重启 MySQL 服务即可。但是，你需要根据你的应用的特性和你的硬件资源来合理配置 Change Buffer，以达到最佳的性能。

总的来说，Change Buffer 是 InnoDB 的一个强大的特性，可以显著提高数据库的性能。但是，使用 Change Buffer 需要一定的专业知识，如果配置不当，可能会影响数据库的性能和稳定性。因此，我们在使用 Change Buffer 时，需要谨慎小心。

# 附录

| **Command-Line Format**                                                                                                | `--innodb-change-buffering=value`                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **System Variable**                                                                                                    | [`innodb_change_buffering`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_change_buffering) |
| **Scope**                                                                                                              | Global                                                                                                                     |
| **Dynamic**                                                                                                            | Yes                                                                                                                        |
| [**`SET_VAR`**](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-set-var) **Hint Applies** | No                                                                                                                         |
| **Type**                                                                                                               | Enumeration                                                                                                                |
| **Default Value**                                                                                                      | `all`                                                                                                                      |
| **Valid Values**                                                                                                       | `none`                                                                                                                     |

`inserts`
`deletes`
`changes`
`purges`
`all` |

| **Value**     | **Numeric Value** | **Description**                                                                                                                      |
| ------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **`none`**    | `0`               | Do not buffer any operations.                                                                                                        |
| **`inserts`** | `1`               | Buffer insert operations.                                                                                                            |
| **`deletes`** | `2`               | Buffer delete marking operations; strictly speaking, the writes that mark index records for later deletion during a purge operation. |
| **`changes`** | `3`               | Buffer inserts and delete-marking operations.                                                                                        |
| **`purges`**  | `4`               | Buffer the physical deletion operations that happen in the background.                                                               |
| **`all`**     | `5`               | The default. Buffer inserts, delete-marking operations, and purges.                                                                  |

# 参考：

[InnoDB 原理篇：Change Buffer 是如何提升索引性能的？\_51CTO 博客\_innodb change buffer](https://blog.51cto.com/u_13626762/5163122)

[MySQL :: MySQL 8.0 Reference Manual :: 15.5.2 Change Buffer](https://dev.mysql.com/doc/refman/8.0/en/innodb-change-buffer.html)

[MySQL :: MySQL 8.0 Reference Manual :: A.16 MySQL 8.0 FAQ: InnoDB Change Buffer](https://dev.mysql.com/doc/refman/8.0/en/faqs-innodb-change-buffer.html)

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
