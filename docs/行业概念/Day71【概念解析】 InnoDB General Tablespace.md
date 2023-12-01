---
password: ""
icon: ""
date: "2023-12-01"
type: Post
category: 行业概念
slug: industry-day71
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day71【概念解析】 InnoDB General Tablespace
status: Published
urlname: 29299214-e0fb-487f-9cd4-64e9725b2c69
updated: "2023-12-01 12:12:00"
---

# 整理定义

中文名称：InnoDB 通用表空间

英文名称：InnoDB General Tablespace

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

### 概念定义

> A general tablespace is a shared `InnoDB` tablespace that is created using [`CREATE TABLESPACE`](https://dev.mysql.com/doc/refman/8.0/en/create-tablespace.html) syntax.  
> ——《[MySQL 官网](https://dev.mysql.com/doc/refman/8.0/en/general-tablespaces.html)》

在 InnoDB 存储引擎中，表空间（Tablespace）是存储表数据和索引的物理文件。传统上，InnoDB 使用单独的表空间文件（每个表一个.ibd 文件）或者将所有表数据存储在系统表空间中。**通用表空间**提供了一种中间的解决方案，允许用户创建一个独立的表空间文件，这个文件可以包含多个表，但不是所有的表。

InnoDB 通用表空间（General Tablespace）是 MySQL 数据库中的一个特性，它允许用户创建一个共享的表空间，其中可以包含多个 InnoDB 表。这个特性从 MySQL 5.7.6 版本开始引入，提供了一种灵活的方式来管理数据库中的表。

# 复述展开

MySQL 官网提到**通用表空间**具有下面一些能力：

- 与**系统表空间（system tablespace）**类似，**通用表空间**是**共享**的表空间，能够存储多个表的数据。
- 与**文件每表表空间（file-per-table tablespaces）**相比，通用表空间在内存方面可能有优势。服务器会在表空间的生命周期内，将表空间元数据保留在内存中。相比于同样数量的表分别存储在各自的每个表一个文件的表空间中，多个表存储在较少的通用表空间中会消耗更少的内存用于表空间元数据。
- 通用表空间的数据文件可以放置在相对于 MySQL 数据目录的目录中，或者独立于 MySQL 数据目录之外，这为您提供了许多与**文件每表表空间（file-per-table tablespaces）**相同的数据文件和存储管理能力。与**文件每表表空间（file-per-table tablespaces）**一样，将数据文件放置在 MySQL 数据目录之外的能力，允许您分别管理关键表的性能，为特定表设置 RAID 或 DRBD，或者将表绑定到特定的磁盘。
- 通用表空间支持所有表行格式及其相关功能。
- 可以在`CREATE TABLE`时使用`TABLESPACE`选项，在**通用表空间**、**文件每表表空间**或**系统表空间**中创建表。
- 可以在`ALTER TABLE`时使用`TABLESPACE`选项，将表在**通用表空间**、**文件每表表空间**和**系统表空间**之间移动。

### 用途

通用表空间的引入主要是为了提供更好的灵活性和管理便利性。它的用途包括：

- **空间管理：**  通过将表组织到单独的文件中，可以更容易地管理磁盘空间。
- **备份和恢复：**  通用表空间可以简化备份和恢复过程，因为相关的表都存储在同一个文件中。
- **传输数据：**  通用表空间可以用于更方便地在服务器之间迁移数据。
- **文件大小限制：**  对于使用文件系统限制单个文件大小的环境，通用表空间可以帮助规避这些限制。

### 使用方式

创建通用表空间非常简单，可以使用以下 SQL 命令：

```sql
CREATE TABLESPACE tablespace_name
ADD DATAFILE 'file_name.ibd'
ENGINE=InnoDB;
```

在创建了通用表空间之后，可以在创建新表或移动现有表时指定这个表空间：

```sql
CREATE TABLE table_name (
    ...
) TABLESPACE tablespace_name ENGINE=InnoDB;

ALTER TABLE table_name TABLESPACE tablespace_name;
```

### 注意事项

在使用通用表空间时，有几个重要的注意事项：

- **兼容性：**  一旦表被添加到通用表空间，它就不能被移回到系统表空间或转换为文件表空间。
- **备份：**  当使用通用表空间时，需要确保备份策略包括了这些表空间文件。
- **性能：**  由于多个表共享同一个表空间，可能会影响到 I/O 性能，特别是在高负载环境下。
- **文件管理：**  通用表空间的文件需要手动管理，数据库不会自动删除这些文件，即使相关的表已经被删除。
- **迁移：**  在迁移通用表空间到另一个服务器时，需要确保文件路径和名称与原服务器保持一致。

# 理解体会

InnoDB 通用表空间是 MySQL 数据库管理中的一个强大工具，它提供了额外的灵活性和便利性。通过使用通用表空间，数据库管理员可以更好地控制数据的物理存储，简化备份和恢复过程，并在必要时轻松迁移数据。然而，这也需要管理员对表空间的管理采取更加谨慎的态度，以确保数据的完整性和性能的最优化。在实施之前，应该仔细考虑通用表空间的优势与潜在的挑战，并制定相应的策略来充分利用这一特性。
