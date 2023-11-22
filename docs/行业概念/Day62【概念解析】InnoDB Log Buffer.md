---
password: ""
icon: ""
date: "2023-11-22"
type: Post
category: 行业概念
slug: industry-day62
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day62【概念解析】InnoDB Log Buffer
status: Published
urlname: e9f2dc4c-88cf-4d15-9dc5-e82a4432a018
updated: "2023-11-22 02:30:00"
---

# 整理定义

中文定义：日志缓冲区

英文定义：Log Buffer

![](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

> 💡 **What is log buffer?**

    The log buffer is the memory area that holds data to be written to the log files on disk. Log buffer size is defined by the [`innodb_log_buffer_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_buffer_size) variable. The default size is 16MB. The contents of the log buffer are periodically flushed to disk. A large log buffer enables large transactions to run without the need to write redo log data to disk before the transactions commit. Thus, if you have transactions that update, insert, or delete many rows, increasing the size of the log buffer saves disk I/O.


    The [`innodb_flush_log_at_trx_commit`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit) variable controls how the contents of the log buffer are written and flushed to disk. The [`innodb_flush_log_at_timeout`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_timeout) variable controls log flushing frequency.

**日志缓冲区（Log Buffer）**是保存要写入磁盘上日志文件的数据的内存区域。

日志缓冲区的内容会定期刷新到磁盘。 大型日志缓冲区允许大型事务运行，而无需在事务提交之前将重做日志数据写入磁盘。 因此，如果您有更新、插入或删除许多行的事务，则增加日志缓冲区的大小可以节省磁盘 I/O。

# 复述展开

相关配置：

## innodb_log_buffer_size

日志缓冲区大小由 `innodb_log_buffer_size` 变量定义。 默认大小为 `16MB`。

    | **Command-Line Format**                                                                                                | `--innodb-log-buffer-size=#`                                                                                             |
    | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
    | **System Variable**                                                                                                    | [`innodb_log_buffer_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_buffer_size) |
    | **Scope**                                                                                                              | Global                                                                                                                   |
    | **Dynamic**                                                                                                            | Yes                                                                                                                      |
    | [**`SET_VAR`**](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-set-var) **Hint Applies** | No                                                                                                                       |
    | **Type**                                                                                                               | Integer                                                                                                                  |
    | **Default Value**                                                                                                      | `16777216` 【16MB】                                                                                                        |
    | **Minimum Value**                                                                                                      | `1048576`  【1MB】                                                                                                         |
    | **Maximum Value**                                                                                                      | `4294967295`  【4G-1】                                                                                                     |

## innodb_flush_log_at_trx_commit

`innodb_flush_log_at_trx_commit` 是一个 MySQL 的系统变量，主要用于控制 InnoDB 存储引擎在事务提交时如何刷新（写入并同步）事务日志到磁盘。这个变量的设置会影响数据库的 ACID 属性和性能。

这个变量有三个可能的值：0，1，和 2。

1. 当`innodb_flush_log_at_trx_commit`设置为`1`时，每次事务提交时，InnoDB 都会**立即**将事务日志刷新到磁盘。这种设置可以提供最高的数据持久性，因为即使在数据库崩溃的情况下，也不会丢失已经提交的事务。**这是默认的设置，也是实现完全 ACID 合规性所必需的**。
2. 当`innodb_flush_log_at_trx_commit`设置为`0`时，InnoDB 只会**每秒钟**将事务日志刷新到磁盘一次。这种设置可以提供最高的性能，但是在数据库崩溃的情况下，**可能会丢失最近一秒钟内提交的事务**。
3. 当`innodb_flush_log_at_trx_commit`设置为`2`时，每次事务提交时，InnoDB 都会将事务日志写入**操作系统的缓冲区**，然后**每秒钟**将**操作系统缓冲区**的内容刷新到磁盘一次。这种设置在性能和数据持久性之间提供了一个折衷的选择。

| **Command-Line Format**                                                                                                | `--innodb-flush-log-at-trx-commit=#`                                                                                                     |
| ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **System Variable**                                                                                                    | [`innodb_flush_log_at_trx_commit`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit) |
| **Scope**                                                                                                              | Global                                                                                                                                   |
| **Dynamic**                                                                                                            | Yes                                                                                                                                      |
| [**`SET_VAR`**](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-set-var) **Hint Applies** | No                                                                                                                                       |
| **Type**                                                                                                               | Enumeration                                                                                                                              |
| **Default Value**                                                                                                      | `1`                                                                                                                                      |
| **Valid Values**                                                                                                       | `0`                                                                                                                                      |

`1`
`2` |

## innodb_flush_log_at_timeout

变量控制日志刷新频率。每 N 秒写并刷新一次。

| **Command-Line Format**                                                                                                | `--innodb-flush-log-at-timeout=#`                                                                                                  |
| ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **System Variable**                                                                                                    | [`innodb_flush_log_at_timeout`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_timeout) |
| **Scope**                                                                                                              | Global                                                                                                                             |
| **Dynamic**                                                                                                            | Yes                                                                                                                                |
| [**`SET_VAR`**](https://dev.mysql.com/doc/refman/8.0/en/optimizer-hints.html#optimizer-hints-set-var) **Hint Applies** | No                                                                                                                                 |
| **Type**                                                                                                               | Integer                                                                                                                            |
| **Default Value**                                                                                                      | `1`                                                                                                                                |
| **Minimum Value**                                                                                                      | `1`                                                                                                                                |
| **Maximum Value**                                                                                                      | `2700`                                                                                                                             |
| **Unit**                                                                                                               | seconds                                                                                                                            |

# 理解体会

**日志缓冲区（Log Buffer）**是数据库系统中的一个重要组件，它主要用于暂存即将写入到磁盘的日志记录。在 MySQL 的 InnoDB 存储引擎中，日志缓冲区是用于存储重做日志（Redo Log）的内存区域。

作用：

1. **提高性能**：日志缓冲区可以将多个小的日志写操作合并为一个大的写操作，从而减少磁盘 I/O 操作的次数，提高性能。
2. **提供持久性**：在事务提交时，InnoDB 会将日志缓冲区中的重做日志写入到磁盘，从而确保即使在数据库崩溃的情况下，也能恢复已经提交的事务。

注意事项：

1. 日志缓冲区的大小：日志缓冲区的大小由`innodb_log_buffer_size`变量控制。如果这个值设置得太小，那么日志缓冲区可能会频繁地溢出，导致日志过早地写入到磁盘，从而影响性能。如果这个值设置得太大，那么可能会浪费内存资源。
2. 刷新策略：日志缓冲区的刷新策略由`innodb_flush_log_at_trx_commit`和`innodb_flush_log_at_timeout`两个变量控制。你需要根据你的应用的需求和你的硬件资源来合理设置这两个变量。

[MySQL :: MySQL 8.0 Reference Manual :: 15.5.4 Log Buffer](https://dev.mysql.com/doc/refman/8.0/en/innodb-redo-log-buffer.html)
