---
password: ""
icon: ""
date: "2023-11-23"
type: Post
category: 行业概念
slug: industry-day63
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day63【概念解析】InnoDB Redo Log
status: Published
urlname: 736ee81e-b82a-4b23-acd2-79554d2e1a40
updated: "2023-11-23 05:34:00"
---

# 整理定义

中文名称：重做日志

英文名称：Redo Log

![MySQL 5.7版本](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

> 🎈 **What is Redo Log？**  
> **重做日志（Redo Log）**是一种基于<u>磁盘</u>的<u>数据结构</u>，用于在<u>崩溃恢复期间修正由不完整事务写入的数据</u>。在正常操作期间，重做日志对由 SQL 语句或低级 API 调用导致的更改表数据的请求进行编码。在意外关闭之前未完成更新数据文件的修改将在初始化和接受连接之前自动重放。
>
> 重做日志在磁盘上以<u>重做日志文件</u>的形式物理表示。写入重做日志文件的数据以受影响的记录为单位进行编码，这些数据统称为**重做（Redo）**。数据通过重做日志文件的传递由递增的**LSN（日志序列号）**值表示。重做日志数据在数据修改发生时追加，随着检查点的进行，最旧的数据将被截断。
>
> 默认情况下**【MySQL 8.0.30 之前】**，重做日志在磁盘上以两个文件命名为 ib_logfile0 和 ib_logfile1 的形式进行物理表示。MySQL 以循环方式向重做日志文件写入数据。重做日志中的数据以受影响的记录为单位进行编码，这些数据统称为重做。数据通过重做日志的传递由递增的 LSN（日志序列号）值表示。

# 复述展开

**redo log（重做日志）** 实现持久化和原子性 在 innoDB 的存储引擎中，事务日志通过重做(redo)日志和 innoDB 存储引擎的日志缓冲(InnoDB Log Buffer)实现。事务开启时，事务中的操作，都会先写入存储引擎的日志缓冲中，在事务提交之前，这些缓冲的日志都需要提前刷新到磁盘上持久化，这就是 DBA 们口中常说的“日志先行”(Write-Ahead Logging)。 当事务提交之后，在 Buffer Pool 中映射的数据文件才会慢慢刷新到磁盘。此时如果数据库崩溃或者宕机，那么当系统重启进行恢复时，就可以根据 redo log 中记录的日志，把数据库恢复到崩溃前的一个状态。未完成的事务，可以继续提交，也可以选择回滚，这基于恢复的策略而定。 在系统启动的时候，就已经为[redo log](https://www.zhihu.com/search?q=redo%20log&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1935044903%7D)分配了一块连续的存储空间，以顺序追加的方式记录 Redo Log，通过顺序 IO 来改善性能。所有的事务共享 redo log 的存储空间，它们的 Redo Log 按语句的执行顺序，依次交替的记录在一起。

## Redo Log File 的组成

名称：`ib_logfileN`

默认情况下：`ib_logfile0` and `ib_logfile1`

如果有 N 个，则为 `ib_logfile0`，`ib_logfile1`，`ib_logfile2`，… `ib_logfileN`，

最多 100 个文件，所有文件总容量最大为 512GB

## 如何更改 Redo Log 文件的数量或大小？

更改 InnoDB 重做日志文件的数量或大小的步骤如下：

1. 停止 MySQL 服务器，并确保它能够正常关闭，没有出现错误。
2. 编辑`my.cnf`文件以更改日志文件的配置。要更改日志文件的大小，配置`innodb_log_file_size`。要增加日志文件的数量，配置`innodb_log_files_in_group`。
3. 再次启动 MySQL 服务器。

如果 InnoDB 检测到`innodb_log_file_size`与重做日志文件的大小不同，它会写入一个日志检查点，关闭并删除旧的日志文件，创建新的请求大小的日志文件，并打开新的日志文件。

### `innodb_log_file_size`

| **Command-Line Format**      | `--innodb-log-file-size=#`                                                                                           |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **System Variable**          | [`innodb_log_file_size`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_file_size) |
| **Scope**                    | Global                                                                                                               |
| **Dynamic**                  | No                                                                                                                   |
| **Type**                     | Integer                                                                                                              |
| **Default Value**            | `50331648` 【48MB】                                                                                                  |
| **Minimum Value (≥ 5.7.11)** | `4194304`                                                                                                            |
| **Minimum Value (≤ 5.7.10)** | `1048576`                                                                                                            |
| **Maximum Value**            | `512GB / innodb_log_files_in_group`                                                                                  |
| **Unit**                     | bytes                                                                                                                |

[`innodb_log_file_size`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_file_size)  最小值 从 **1MB** 改为 **4MB** 在 **MySQL 5.7.11**.

### `innodb_log_files_in_group`

| **Command-Line Format** | `--innodb-log-files-in-group=#`                                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **System Variable**     | [`innodb_log_files_in_group`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_files_in_group) |
| **Scope**               | Global                                                                                                                         |
| **Dynamic**             | No                                                                                                                             |
| **Type**                | Integer                                                                                                                        |
| **Default Value**       | `2`                                                                                                                            |
| **Minimum Value**       | `2`                                                                                                                            |
| **Maximum Value**       | `100`                                                                                                                          |

> 🔥 **注意：**  
> ([`innodb_log_file_size`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_file_size) \* [`innodb_log_files_in_group`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_files_in_group)) 的值不能超过 512GB.

## 优化指南

考虑以下优化重做日志记录的指南：

1. **将重做日志文件设置得很大，甚至可以与缓冲池一样大**。当 InnoDB 将重做日志文件写满时，它必须在检查点中将缓冲池的修改内容写入磁盘。较小的重做日志文件会导致许多不必要的磁盘写入。尽管过去大的重做日志文件会导致较长的恢复时间，但现在恢复速度更快，您可以放心使用大的重做日志文件。
2. 通过配置`innodb_log_file_size`和`innodb_log_files_in_group`选项来设置重做日志文件的大小和数量。
3. **考虑增加日志缓冲区的大小**。较大的日志缓冲区可以使大型事务在提交之前无需将日志写入磁盘。因此，如果您有更新、插入或删除大量行的事务，增大日志缓冲区可以节省磁盘 I/O。可以使用`innodb_log_buffer_size`配置选项来配置日志缓冲区的大小。
4. 配置`innodb_log_write_ahead_size`选项以避免“读写冲突”。该选项定义了重做日志的预写块大小。**将\*\***`innodb_log_write_ahead_size`\***\*设置为与操作系统或文件系统缓存块大小相匹配。**当由于重做日志的预写块大小与操作系统或文件系统缓存块大小不匹配而无法完全缓存重做日志块时，会发生读写冲突。
5. **`innodb_log_write_ahead_size`\*\***的有效值是 InnoDB 日志文件块大小（2 的 n 次方）的倍数\*\*。最小值为 InnoDB 日志文件块大小（512）。当指定最小值时，不会发生预写。最大值等于`innodb_page_size`值。如果为`innodb_log_write_ahead_size`指定的值大于`innodb_page_size`值，则`innodb_log_write_ahead_size`设置将被截断为`innodb_page_size`值。
6. 如果将`innodb_log_write_ahead_size`值设置得太低，与操作系统或文件系统缓存块大小相比，会导致读写冲突。如果将值设置得太高，可能会对日志文件写入的 fsync 性能产生轻微影响，因为多个块会同时被写入。

# 理解体会

InnoDB 的重做日志（Redo Log）是 MySQL 数据库中的一个重要组件，它是一种持久化存储机制，用于记录数据库事务执行过程中对数据的所有修改操作。

定义：重做日志是一种日志文件，它记录了所有修改数据库数据的操作，以便在数据库崩溃后可以用来恢复数据。重做日志是循环写入的，当达到一定大小后，会从头开始覆盖旧的日志记录。

作用：

1. 数据恢复：在数据库崩溃后，可以通过重做日志来恢复数据，保证数据的持久性。
2. 提高性能：在事务提交时，只需要将重做日志写入到磁盘，而不需要立即将修改的数据写入到磁盘，这可以减少磁盘 I/O 操作的次数，提高性能。

> 🔥 **注意**  
> 在 MySQL 8.0.30 版本之后，Redo Log 的配置有些许变化，可以参考链接。本文主要讲解之前的情况。

# 参考

[MySQL :: MySQL 8.0 Reference Manual :: 15.6.5 Redo Log](https://dev.mysql.com/doc/refman/8.0/en/innodb-redo-log.html)

[MySQL :: MySQL 8.0 Reference Manual :: 8.5.4 Optimizing InnoDB Redo Logging](https://dev.mysql.com/doc/refman/8.0/en/optimizing-innodb-logging.html)
