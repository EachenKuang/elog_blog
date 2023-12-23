---
password: ""
icon: ""
date: "2023-12-23"
type: Post
category: 行业概念
slug: industry-day93
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day93【概念解析】File Space Management
status: Published
urlname: f0baef65-459b-42b1-a40d-47199f028309
updated: "2023-12-23 04:05:00"
---

> 😄 本章对于 InnoDB 的文件空间管理进行整理，会从不同类型的表空间开始展开，然后从文件存储的各个单元（页、区、段、表空间）进行阐述，然后谈谈关于这些文件管理的一些配置。

# 整理定义

首先我们看看 Tablespace，Segment， Extend， Page 的关系图

![](https://image.kuangyichen.com/image/innodb_tablespaces.png)

# 复述展开

## Tablespace 表空间

关于表空间，可以在【InnoDB Tablespace】中回顾。下面引用之前文章中的部分定义：

[bookmark](https://kuangyichen.com/article/industry-day68)

**定义**

> **表空间**是 InnoDB 存储系统的逻辑单位，它包含了用于存储表数据、索引、元数据等的数据文件。在物理层面，表空间可以是一个或多个文件，这些文件可以分布在文件系统的不同位置。

**分类**

> InnoDB 的表空间可以分为以下几类：

    1. **系统表空间**：这是InnoDB初始化时创建的表空间，通常包含在名为`ibdata1`的文件中。系统表空间不仅存储了系统表的数据和索引，还包括了撤销日志（undo logs）、数据字典、双写缓冲区（doublewrite buffer）和插入缓冲区（insert buffer）等。
    2. **文件每表表空间**：从MySQL 5.6.6版本开始，InnoDB默认为每个表创建独立的表空间文件，这些文件的扩展名通常是`.ibd`。这种方式使得每个表的数据和索引被隔离在单独的文件中，便于管理和优化。
    3. **通用表空间**：这是MySQL 5.7版本引入的特性，允许用户创建共享的表空间，多个表可以共享同一个表空间文件。这种方式提供了更灵活的数据管理选项。
    4. **临时表空间**：用于存储临时表的数据和索引。这些临时表主要用于查询处理过程中的排序和哈希操作。
    5. **撤销表空间**：用于存储撤销日志的表空间。在MySQL 5.7及更高版本中，可以配置多个撤销表空间，以提高并发事务的性能。

您在配置文件中使用 `innodb_data_file_path` 配置选项定义的数据文件组成了 InnoDB 系统表空间。这些文件在逻辑上串联起来形成系统表空间。这里没有使用条带化技术。您无法定义表在系统表空间内的具体分配位置。在新创建的系统表空间中，InnoDB 从第一个数据文件开始分配空间。

[bookmark](https://kuangyichen.com/article/industry-day69)

为了避免将所有表和索引存储在系统表空间内带来的问题，您可以启用 innodb_file_per_table 配置选项（默认情况下启用），该选项将每个新创建的表存储在一个单独的表空间文件中（扩展名为 .ibd）。对于以这种方式存储的表，在磁盘文件中的碎片更少，当表被截断时，空间会被返回给操作系统，而不是仍然被 InnoDB 在系统表空间内保留。【关于文件每表表空间，可以参考之前的[文章](https://kuangyichen.com/article/industry-day70)】

[bookmark](https://kuangyichen.com/article/industry-day70)

您还可以将表存储在通用表空间中。通用表空间是使用 CREATE TABLESPACE 语法创建的共享表空间。它们可以在 MySQL 数据目录之外创建，能够容纳多个表，并支持所有行格式的表。【关于通用表空间，可以参考】

[bookmark](https://kuangyichen.com/article/industry-day71)

## Page 页

**一种表示 InnoDB 在任意时刻在磁盘（数据文件）和内存（缓冲池）之间传输多少数据的单位**。 一页可以包含一行或多行，具体取决于每行中的数据量。 如果一行不能完全放入一页，InnoDB 会设置额外的指针式数据结构，以便有关该行的信息可以存储在一页中。

在每个页面中容纳更多数据的一种方法是使用压缩行格式。 对于使用 BLOB 或大型文本字段的表，紧凑行格式允许将这些大型列与行的其余部分分开存储，从而减少不引用这些列的查询的 I/O 开销和内存使用量。

当 InnoDB 批量读取或写入一组页面以增加 I/O 吞吐量时，它一次读取或写入一个范围。

**MySQL 实例中的所有 InnoDB 磁盘数据结构共享相同的页面大小。**

> What is Page ([MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_page))

    A unit representing how much data `InnoDB` transfers at any one time between disk (the _**data files**_) and memory (the _**buffer pool**_). A page can contain one or more _**rows**_, depending on how much data is in each row. If a row does not fit entirely into a single page, `InnoDB` sets up additional pointer-style data structures so that the information about the row can be stored in one page.


    One way to fit more data in each page is to use _**compressed row format**_. For tables that use BLOBs or large text fields, _**compact row format**_ allows those large columns to be stored separately from the rest of the row, reducing I/O overhead and memory usage for queries that do not reference those columns.


    When `InnoDB` reads or writes sets of pages as a batch to increase I/O throughput, it reads or writes an _**extent**_ at a time.


    All the `InnoDB` disk data structures within a MySQL instance share the same _**page size**_.

## **Extent 区**

**Tablespace 中的一组 Page**。 **对于 16KB 的\*\***默认页面\***\*大小，一个 Extent 包含 64 个 Page。**

| InnoDB 页面大小          | Extent 大小 | Page 数量/Extend |
| ------------------------ | ----------- | ---------------- |
| 4KB                      | 1MB         | 256              |
| 8KB                      | 1MB         | 128              |
| 16KB（默认大小）         | 1MB         | 64               |
| 32KB (自 MySQL 5.7.6 起) | 2MB         | 64               |
| 64KB (自 MySQL 5.7.6 起) | 4MB         | 64               |

> 💡 在 MySQL 5.6 中，InnoDB 实例的页面大小可以是 4KB、8KB 或 16KB，由 `innodb_page_size` 配置选项控制。 对于 4KB、8KB 和 16KB 页面大小，**Extent**大小始终为 1MB（或 1048576 字节）。  
> MySQL 5.7.6 中添加了对 32KB 和 64KB InnoDB 页大小的支持。 对于 32KB 页面大小，**Extent**大小为 2MB。 对于 64KB 页面大小，扩展区大小为 4MB。
>
> InnoDB 功能（例如段、预读请求和双写缓冲区）使用一次读取、写入、分配或释放数据的 I/O 操作。

> What is Extend ([MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_extent))

    A group of _**pages**_ within a _**tablespace**_. For the default _**page size**_ of 16KB, an extent contains 64 pages. In MySQL 5.6, the page size for an `InnoDB` instance can be 4KB, 8KB, or 16KB, controlled by the [`innodb_page_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_page_size) configuration option. For 4KB, 8KB, and 16KB pages sizes, the extent size is always 1MB (or 1048576 bytes).


    Support for 32KB and 64KB `InnoDB` page sizes was added in MySQL 5.7.6. For a 32KB page size, the extent size is 2MB. For a 64KB page size, the extent size is 4MB.


    `InnoDB` features such as _**segments**_, _**read-ahead**_ requests and the _**doublewrite buffer**_ use I/O operations that read, write, allocate, or free data one extent at a time.

## **Segment 段**

InnoDB 表空间内的分区。 如果表空间类似于目录，那么 segment 就类似于该目录中的文件。 segment 可以增长，也可以创建新 segment。

例如，在 ***file-per-table*** tablespace 中，表数据位于一个 segment 中，每个关联索引位于其自己的 segment 中。 系统表空间包含许多不同的段，因为它可以容纳许多表及其关联的索引。 在 MySQL 8.0 之前，系统表空间还包含一个或多个用于撤消日志的回滚段。

> ⛔ 注意，这里的回滚段（rollback segment 与 这里说的 segment 不同）
>
> > The storage area containing the ***undo logs***. Rollback segments have traditionally resided in the ***system tablespace***. As of MySQL 5.6, rollback segments can reside in ***undo tablespaces***. As of MySQL 5.7, rollback segments are also allocated to the *global temporary tablespace*.

随着数据的插入和删除，segment 会增长和缩小。 当一个 segment 需要更多空间时，它一次扩展一个 extend（1MB）。 类似地，当不再需要某个范围内的所有数据时，段会释放该 extend 的空间。

> What is Segment （[MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_segment)）

    A division within an `InnoDB` _**tablespace**_. If a tablespace is analogous to a directory, the segments are analogous to files within that directory. A segment can grow. New segments can be created.


    For example, within a _**file-per-table**_ tablespace, table data is in one segment and each associated index is in its own segment. The _**system tablespace**_ contains many different segments, because it can hold many tables and their associated indexes. Prior to MySQL 8.0, the system tablespace also includes one or more _**rollback segments**_ used for _**undo logs**_.


    Segments grow and shrink as data is inserted and deleted. When a segment needs more room, it is extended by one _**extent**_ (1 megabyte) at a time. Similarly, a segment releases one extent's worth of space when all the data in that extent is no longer needed.

## Row 行与页的关系

对于 4KB、8KB、16KB 和 32KB innodb_page_size 设置，最大行长度略小于数据库页大小的一半。 例如，对于默认的 16KB InnoDB 页面大小，最大行长度略小于 8KB。 对于 innodb_page_size = 64KB 的设置，最大行长度略小于 16KB。

如果某行不超过最大行长度，则所有行都存储在本地页面内。 如果某行超过最大行长度，则会选择可变长度列进行**外部页外存储**，直到该行符合<u>最大行长度限制</u>。 可变长度列的外部页外存储因行格式而异：

- 紧凑和冗余行格式【COMPACT and REDUNDANT Row Formats】

  当选择可变长度列用于外部页外存储时，InnoDB 将前 768 个字节存储在本地行中，其余部分存储在外部溢出页中。 每个这样的列都有自己的溢出页面列表。 768 字节前缀附带一个 20 字节值，该值存储列的真实长度并指向存储其余值的溢出列表。 请参见第 15.10 节“InnoDB 行格式”。

- 动态和压缩行格式【DYNAMIC and COMPRESSED Row Formats】

  当选择可变长度列用于外部页外存储时，InnoDB 将 20 字节指针本地存储在行中，其余部分存储在外部溢出页中。

`LONGBLOB` 和 `LONGTEXT` 列必须小于 4GB，并且总行长度（包括 BLOB 和 TEXT 列）必须小于 4GB。

这里可以联动下 InnoDB 中的限制：

[bookmark](https://kuangyichen.com/article/industry-day91)

# 理解体会

本章对于 InnoDB 的文件空间管理进行整理回顾。对于表空间、段、区、页、行进行了展开介绍，也回顾了之前学习的一些内容，并且整体对上述文件单元进行了概括。

从这篇文章中，可以了解各个单元的组成以及相互之间的关系，还有各部分大小的换算等等。
