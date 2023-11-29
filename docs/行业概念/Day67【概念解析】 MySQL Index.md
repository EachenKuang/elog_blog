---
password: ""
icon: ""
date: "2023-11-27"
type: Post
category: 行业概念
slug: industry-day67
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day67【概念解析】 MySQL Index
status: Published
urlname: 94e33f21-1481-481a-a107-6b254db2817c
updated: "2023-11-29 05:12:00"
---

# 整理定义

## 什么是索引

> 一句话简单来说，索引的出现其实就是为了提高数据查询的效率，就像书的目录一样。一本 500 页的书，如果你想快速找到其中的某一个知识点，在不借助目录的情况下，那我估计你可得找一会儿。同样，对于数据库的表而言，索引其实就是它的“目录”。

    ——《MySQL 45讲》林晓斌

## MySQL 的索引

**在 MySQL 中，索引是一种数据结构，它通过形成代表特定列或一组列的所有值的树结构（B 树），为表的行提供快速查找能力。**

InnoDB 表总是有一个代表主键的聚簇索引。它们也可以有一个或多个定义在一个或多个列上的辅助索引。根据它们的结构，辅助索引可以被分类为部分索引、列索引或复合索引。

索引是<u>查询性能</u>的关键方面。数据库架构师设计表、查询和索引，以便快速查找应用程序所需的数据。理想的数据库设计在实际中使用覆盖索引；查询结果完全由索引计算得出，无需读取实际的表数据。每个外键约束也需要一个索引，以高效地检查父表和子表中是否存在值。

# 复述展开

在 MySQL 中，有以下几种索引：

| 索引类型 | 优点                                           | 缺点                                             | 使用场景                           | 存储引擎                  |
| -------- | ---------------------------------------------- | ------------------------------------------------ | ---------------------------------- | ------------------------- |
| B-Tree   | 广泛适用，支持全键值、键值范围和键值排序的搜索 | 不适合全文搜索                                   | 大多数情况下的查找、排序和范围查询 | InnoDB, MyISAM, Memory 等 |
| Hash     | 快速的等值查询                                 | 不支持范围查询，不支持排序操作，冲突可能影响性能 | 等值比较，如哈希表                 | Memory, NDB               |
| Fulltext | 支持复杂的文本搜索，包括模糊匹配               | 仅限于文本数据，占用空间较大                     | 文本搜索                           | InnoDB, MyISAM            |
| R-Tree   | 支持空间数据的多维范围搜索和多维索引           | 仅适用于空间数据类型                             | 地理空间数据查询                   | MyISAM                    |
| Spatial  | 优化了空间数据的存储和查询                     | 仅支持空间数据类型，不是所有存储引擎都支持       | 地理信息系统（GIS）数据            | InnoDB, MyISAM            |

## B-Tree 与 B+Tree

B-Tree（平衡树）和 B+Tree 是两种常用的索引和数据结构，它们在数据库系统中广泛应用于数据的组织、管理和索引。

### B-Tree

![](https://image.kuangyichen.com/image/2880px-B-tree.svg.png)

B-Tree 是一种自平衡的树数据结构，它维持数据的排序，允许搜索、顺序访问、插入和删除在对数时间内完成。B-Tree 的特点包括：

- 每个节点有多个孩子，节点中的键值按顺序排列。
- 树的所有叶子节点都在同一层。
- 节点中的键值数目有一个上限和下限（除了根节点和叶子节点）。
- B-Tree 通过分裂和合并节点来维持平衡。

### B+Tree

![](https://image.kuangyichen.com/image/Bplustree.png)

B+Tree 是 B-Tree 的一个变种，它具有 B-Tree 的所有特性，但在其结构上有所不同：

- 所有的数据记录都存储在叶子节点上，叶子节点形成了一个链表，便于进行全范围扫描。
- 非叶子节点（内部节点）仅存储键值信息，不存储数据记录，这意味着相比于 B-Tree，B+Tree 可以有更高的分支因子，使得树更加矮胖，减少了磁盘 I/O 次数。
- 叶子节点之间通过指针连接，这提供了顺序访问数据的能力。

### B-Tree 与 B+Tree 的区别

- **数据存储**：B-Tree 的数据存储在**每个节点**上，而 B+Tree 的数据仅存储在**叶子节点**上。
- **树的高度**：B+Tree 通常更矮胖，因为内部节点不存储数据，可以有更多的子节点。
- **范围查询**：B+Tree 由于叶子节点的**链表结构**，使得范围查询更加高效。
- **磁盘读写**：B+Tree 的查询性能更加稳定，因为所有查询都要查找到叶子节点，而 B-Tree 的查询可能在非叶子节点就结束了。

### 为什么 MySQL 选择使用 B+Tree

MySQL 选择使用 B+Tree 作为索引结构，主要是因为 B+Tree 在数据库索引中的几个优势：

- **高效的范围查询**：由于叶子节点的链表结构，B+Tree 特别适合处理范围查询，这在数据库操作中非常常见。
- **更少的磁盘 I/O**：B+Tree 的内部节点不存储数据，可以拥有更多的分支，这减少了树的高度，从而减少了磁盘 I/O 次数，提高了查询效率。
- **查询性能稳定**：B+Tree 的所有查询都要走到叶子节点，这使得每次查询的磁盘读取次数相对固定，性能更加稳定。

# 理解体会

理解 MySQL，必须要学习好 MySQL 中的索引，知道索引的数据结构，才能更好的对其进行优化与调整。

# 参考

- MySQL 索引概述: [**https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html**](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html)
- B-Tree 索引: [**https://dev.mysql.com/doc/refman/8.0/en/btree-indexes.html**](https://dev.mysql.com/doc/refman/8.0/en/btree-indexes.html)
- Hash 索引: [**https://dev.mysql.com/doc/refman/8.0/en/hash-indexes.html**](https://dev.mysql.com/doc/refman/8.0/en/hash-indexes.html)
- Fulltext 索引: [**https://dev.mysql.com/doc/refman/8.0/en/fulltext-search.html**](https://dev.mysql.com/doc/refman/8.0/en/fulltext-search.html)
- R-Tree 索引: [**https://dev.mysql.com/doc/refman/8.0/en/rtree-indexes.html**](https://dev.mysql.com/doc/refman/8.0/en/rtree-indexes.html)
- Spatial 索引: [**https://dev.mysql.com/doc/refman/8.0/en/spatial-types.html**](https://dev.mysql.com/doc/refman/8.0/en/spatial-types.html)
