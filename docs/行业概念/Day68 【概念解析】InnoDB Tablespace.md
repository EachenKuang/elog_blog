---
password: ""
icon: ""
date: "2023-11-28"
type: Post
category: 行业概念
slug: industry-day68
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day68 【概念解析】InnoDB Tablespace
status: Published
urlname: 182b509a-8264-41d6-a76f-885fc47f2642
updated: "2023-11-29 04:36:00"
---

# 整理定义

中文名称：InnoDB 表空间

英文名称：InnoDB Tablespace

![MySQL 5.7版本](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

## 概念

InnoDB 是 MySQL 数据库的默认存储引擎，它使用了一种称为“表空间”（tablespace）的结构来存储数据。表空间是 InnoDB 存储系统的逻辑单位，它包含了用于存储表数据、索引、元数据等的数据文件。在物理层面，表空间可以是一个或多个文件，这些文件可以分布在文件系统的不同位置。

## 分类

InnoDB 的表空间可以分为以下几类：

1. **系统表空间**：这是 InnoDB 初始化时创建的表空间，通常包含在名为`ibdata1`的文件中。系统表空间不仅存储了系统表的数据和索引，还包括了撤销日志（undo logs）、数据字典、双写缓冲区（doublewrite buffer）和插入缓冲区（insert buffer）等。
2. **文件每表表空间**：从 MySQL 5.6.6 版本开始，InnoDB 默认为每个表创建独立的表空间文件，这些文件的扩展名通常是`.ibd`。这种方式使得每个表的数据和索引被隔离在单独的文件中，便于管理和优化。
3. **通用表空间**：这是 MySQL 5.7 版本引入的特性，允许用户创建共享的表空间，多个表可以共享同一个表空间文件。这种方式提供了更灵活的数据管理选项。
4. **临时表空间**：用于存储临时表的数据和索引。这些临时表主要用于查询处理过程中的排序和哈希操作。
5. **撤销表空间**：用于存储撤销日志的表空间。在 MySQL 5.7 及更高版本中，可以配置多个撤销表空间，以提高并发事务的性能。

# 复述展开

> **Tablespace**

    A data file that can hold data for one or more `InnoDB` _**tables**_ and associated _**indexes**_.


    The _**system tablespace**_ contains the `InnoDB` _**data dictionary**_, and prior to MySQL 5.6 holds all other `InnoDB` tables by default.


    The [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_file_per_table) option, enabled by default in MySQL 5.6 and higher, allows tables to be created in their own tablespaces. File-per-table tablespaces support features such as efficient storage of _**off-page columns**_, table compression, and transportable tablespaces. See [Section 14.6.3.2, “File-Per-Table Tablespaces”](https://dev.mysql.com/doc/refman/5.7/en/innodb-file-per-table-tablespaces.html) for details.


    `InnoDB` introduced general tablespaces in MySQL 5.7.6. General tablespaces are shared tablespaces created using [`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/5.7/en/create-tablespace.html) syntax. They can be created outside of the MySQL data directory, are capable of holding multiple tables, and support tables of all row formats.


    ——[MySQL :: MySQL 5.7 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_tablespace)

**表空间是一个数据文件，它可以存储一个或多个 InnoDB 表和相关的索引**。

**系统表空间（system tablespace）**包含 InnoDB 数据字典，并且在 MySQL 5.6 之前，默认情况下，所有其他 InnoDB 表都存储在其中。

**文件每表表空间（File-per-table tablespaces）**在 MySQL 5.6 及更高版本中，默认启用的`innodb_file_per_table`选项允许在各自的表空间中创建表。每表一个文件的表空间支持诸如高效存储离页列、表压缩和可传输表空间等功能。

在 MySQL 5.7.6 中，InnoDB 引入了**通用表空间（**general tablespace**）**。通用表空间是使用 CREATE TABLESPACE 语法创建的共享表空间。它们可以在 MySQL 数据目录之外创建，能够容纳多个表，并支持所有行格式的表。

以下是对比表：

| 类型               | 版本                   | 存储位置                   | 支持的功能                                                |
| ------------------ | ---------------------- | -------------------------- | --------------------------------------------------------- |
| 系统表空间         | 所有版本               | MySQL 数据目录             | 存储 InnoDB 数据字典，所有 InnoDB 表（在 MySQL 5.6 之前） |
| **文件每表表空间** | MySQL 5.6 及更高版本   | MySQL 数据目录或自定义目录 | 高效存储离页列、表压缩、可传输表空间                      |
| 通用表空间         | MySQL 5.7.6 及更高版本 | MySQL 数据目录或自定义目录 | 容纳多个表，支持所有行格式的表                            |

# 理解体会

我对表空间的理解经历了从模糊到清晰的过程。一开始，表空间这个概念对我来说相当抽象，我只知道它是与存储数据相关的某种东西。但随着不断学习和实践，我逐渐理解了表空间在 InnoDB 存储引擎中的核心作用。

在我看来，InnoDB 的表空间就像是一个大型的仓库，里面有许多不同的房间，每个房间都用来存放不同的物品。系统表空间就像是仓库的主要区域，存放着最重要的物品，比如撤销日志、数据字典等。而文件每表表空间则像是给每个物品分配了一个独立的小储物柜，这样就可以非常方便地管理和访问这些物品。

在学习过程中，我对**文件每表表空间**的好处有了深刻的体会。当我在实验中删除一个表时，我发现相关的.ibd 文件也随之消失了，这让我意识到了这种设计对于数据库维护和管理的便利性。我不需要担心删除一个表会影响到其他表的数据，因为它们都是独立存储的。

**通用表空间**的概念一开始让我感到困惑，但当我了解到它可以让多个表共享同一个表空间时，我意识到这对于节省磁盘空间有很大的好处。这就像是在仓库中有一个共享区域，可以由多个小组共同使用，而不是每个小组都需要单独的存储空间。

**临时表空间**的学习让我想到了计算机操作系统中的临时文件概念。这些临时表是为了处理查询而创建的，它们不需要长期存储，就像是我们在做数学题时草稿纸上的计算一样，用完即丢。这种设计让我对数据库的效率有了新的认识，因为它确保了即使是复杂的查询也不会对数据库的长期性能造成影响。

**撤销表空间**的概念在我最初的学习中是最难以把握的。但当我在实验中故意制造事务失败，然后观察 InnoDB 如何回滚到稳定状态时，我对这一机制产生了敬畏。这就像是一个安全网，无论发生什么错误，都能确保数据不会丢失，这对于任何关心数据一致性的系统来说都是至关重要的。

总的来说，InnoDB 表空间的学习让我对数据库的内部工作有了更深入的理解。我从中学到了数据库不仅仅是存储数据的地方，它还涉及到数据的组织、管理、安全和恢复等多个方面。虽然学习过程中有时会感到困难和挑战，但每当我理解了一个新概念或成功解决了一个问题时，那种成就感和满足感是无与伦比的。
