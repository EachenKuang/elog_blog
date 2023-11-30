---
password: ""
icon: ""
date: "2023-11-30"
type: Post
category: 行业概念
slug: industry-day70
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day70【概念解析】InnoDB File-Per-Table Tablespace
status: Published
urlname: 8c2141a8-6880-47be-9287-25410702723b
updated: "2023-11-30 04:49:00"
---

# 整理定义

InnoDB File-Per-Table Tablespaces

InnoDB **文件每表 表空间**

一个 File-Per-Table Tablespace 包含单个  `InnoDB`  表的数据和索引，并存储在文件系统中的单个数据文件中。

![MySQL 5.7版本](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

# 复述展开

### File-Per-Table Tablespace Configuration

要启用 File-Per-Table 模式，你需要在 MySQL 的配置文件（通常是 my.cnf 或 my.ini）中设置`innodb_file_per_table`选项为。这通常是默认设置。

```sql
[mysqld]
innodb_file_per_table=ON
```

当这个选项被启用时，每当你创建一个新的 InnoDB 表时，MySQL 会为该表创建一个独立的.ibd 文件，在**文件每表表空间（**File-Per-Table Tablespaces**）**中，如果禁用这个操作，表会在系统表空间创建。。

> When [`innodb_file_per_table`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_file_per_table) is enabled, tables are created in file-per-table tablespaces by default. When disabled, tables are created in the system tablespace by default.

### File-Per-Table Tablespace Data Files

在 File-Per-Table 模式下，每个 InnoDB 表的数据和索引都存储在一个单独的.ibd 文件中。这些文件位于 MySQL 的数据目录下，通常与表的名称相对应。例如，如果你有一个名为`users`的表，在数据库`mydatabase`中，相应的表空间文件将是`mydatabase/users.ibd`。

```sql
mysql> USE mydatabase;

mysql> CREATE TABLE users (
   id INT PRIMARY KEY AUTO_INCREMENT,
   name VARCHAR(100)
 ) ENGINE = InnoDB;

$> cd /path/to/mysql/data/mydatabase
$> ls
users.ibd
```

### File-Per-Table Tablespace Advantages

1. **磁盘空间管理**：在文件每表表空间中截断或删除表后，磁盘空间会直接返还给操作系统，而共享表空间中的表操作不会减少数据文件的大小，只会在表空间内部留下可重用的空间。
2. **ALTER TABLE 操作**：在共享表空间中进行复制表的 ALTER TABLE 操作可能会增加表空间所占用的磁盘空间，而文件每表表空间则会在操作完成后释放额外空间。
3. **性能**：在文件每表表空间中执行 TRUNCATE TABLE 操作通常比在共享表空间中执行时更快。
4. **存储优化**：文件每表表空间的数据文件可以在不同的存储设备上创建，以优化 I/O 性能、进行更好的空间管理或出于备份目的。
5. **数据迁移**：可以从另一个 MySQL 实例导入文件每表表空间中的表。
6. **行格式支持**：文件每表表空间中创建的表支持 DYNAMIC 和 COMPRESSED 行格式，这些格式不被系统表空间支持。
7. **数据恢复**：当数据损坏发生时，存储在独立表空间数据文件中的表可以更快地恢复，提高成功恢复的机会。
8. **备份与恢复**：使用 MySQL 企业备份，可以快速备份或恢复文件每表表空间中的表，而不会中断对其他 InnoDB 表的使用。

### File-Per-Table Tablespace Disadvantages

1. **空间利用率**：每个文件每表表空间可能会有未使用的空间，这些空间只能被同一个表的行使用，如果不适当管理，可能会导致空间浪费。
2. **fsync 操作**：需要对多个文件每表数据文件执行 fsync 操作，而不是单个共享表空间数据文件。由于 fsync 操作是按文件执行的，多个表的写操作不能合并，可能导致更高的总 fsync 操作次数。
3. **文件句柄**：mysqld 必须为每个文件每表表空间保持一个打开的文件句柄，如果有大量的表使用文件每表表空间，可能会影响性能。
4. **文件描述符**：当每个表都有自己的数据文件时，需要更多的文件描述符。
5. **碎片**：可能会有更多的碎片产生，这可能会妨碍 DROP TABLE 操作和表扫描性能。然而，如果碎片得到管理，文件每表表空间可以提高这些操作的性能。
6. **缓冲池扫描**：删除文件每表表空间中的表时，需要扫描缓冲池，对于大型缓冲池，这可能需要几秒钟。扫描是在一个广泛的内部锁的情况下执行的，可能会延迟其他操作。
7. **自动扩展**：`innodb_autoextend_increment`变量定义了共享表空间文件满时自动扩展的增量大小，但这不适用于文件每表表空间文件的自动扩展，不考虑`innodb_autoextend_increment`设置。文件每表表空间的初始文件扩展是以较小的数量进行的，之后扩展会以 4MB 的增量进行。

这些缺点表明，虽然文件每表表空间提供了许多管理和性能优势，但它们也引入了一些潜在的挑战，特别是在管理大量小表的环境中。数据库管理员需要权衡这些因素，以确定哪种表空间配置最适合他们的特定用例和性能需求。

# 理解体会

文件每表表空间为数据库管理员提供了更细粒度的控制，允许他们根据特定的性能和管理需求来优化表的存储。然而，这也需要更细心的空间管理和可能的性能权衡。在设计和维护 InnoDB 数据库时，选择使用文件每表表空间还是共享表空间，应该基于对这些权衡的理解和具体的应用场景需求。
