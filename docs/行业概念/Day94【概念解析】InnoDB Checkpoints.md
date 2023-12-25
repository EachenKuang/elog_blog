---
password: ""
icon: ""
date: "2023-12-24"
type: Post
category: 行业概念
slug: industry-day94
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day94【概念解析】InnoDB Checkpoints
status: Published
urlname: 2729d946-80f9-4140-8e72-966a2b1e8283
updated: "2023-12-25 12:01:00"
---

# 整理定义

**InnoDB Checkpoints** （InnoDB 检查点）

> As changes are made to data pages that are cached in the ***buffer pool***, those changes are written to the ***data files*** sometime later, a process known as ***flushing***. The checkpoint is a record of the latest changes (represented by an ***LSN*** value) that have been successfully written to the data files.

当对缓存在缓冲池中的数据页进行更改时，这些更改会在稍后写入数据文件，这个过程称为刷新（Flushing）。 检查点是已成功写入数据文件的最新更改（由 LSN 值表示）的记录。

[MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_checkpoint)

**这里重新复习下之前学过的一些词汇：**

1.  Buffer pool：是 InnoDB 存储引擎的一个重要组成部分，它是 InnoDB 用于缓存表和索引数据的内存区域。

    [bookmark](https://kuangyichen.com/article/industry-day59)

2.  _**flushing:**_

    将更改写入已缓冲在内存区域或临时磁盘存储区域中的数据库文件。 定期刷新的 InnoDB 存储结构包括**重做日志**、**撤消日志**和**缓冲池**。

    刷新可能会发生，因为内存区域已满并且系统需要释放一些空间，因为提交操作意味着可以完成事务的更改，或者因为缓慢的关闭操作意味着所有未完成的工作都应该完成。 当一次刷新所有缓冲数据并不重要时，InnoDB 可以使用一种称为模糊检查点的技术来刷新小批量的页面，以分散 I/O 开销。

    > [MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_flush)

        To write changes to the database files, that had been buffered in a memory area or a temporary disk storage area. The `InnoDB` storage structures that are periodically flushed include the _**redo log**_, the _**undo log**_, and the _**buffer pool**_.


        Flushing can happen because a memory area becomes full and the system needs to free some space, because a _**commit**_ operation means the changes from a transaction can be finalized, or because a _**slow shutdown**_ operation means that all outstanding work should be finalized. When it is not critical to flush all the buffered data at once, `InnoDB` can use a technique called _**fuzzy checkpointing**_ to flush small batches of pages to spread out the I/O overhead.

3.  LSN(log sequence number):

    LSN 是“日志序列号”的缩写。 **这个任意的、不断增加的值表示与重做日志中记录的操作相对应的时间点**。 （这一时间点与事务边界无关；它可以落在一个或多个事务的中间。）它由 InnoDB 在崩溃恢复期间内部使用并用于管理缓冲池。

    > 💡 在 MySQL 5.6.3 之前，LSN 是一个 **4 字节无符号整数**。 当重做日志文件大小限制从 4GB 增加到 512GB 时，LSN 在 MySQL 5.6.3 中变成了 **8 字节无符号整数**，因为需要额外的字节来存储额外的大小信息。 在 MySQL 5.6.3 或更高版本上构建并使用 LSN 值的应用程序应使用 64 位而不是 32 位变量来存储和比较 LSN 值。

    > [MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_lsn)

        Acronym for “log sequence number”. This arbitrary, ever-increasing value represents a point in time corresponding to operations recorded in the _**redo log**_. (This point in time is regardless of _**transaction**_ boundaries; it can fall in the middle of one or more transactions.) It is used internally by `InnoDB` during _**crash recovery**_ and for managing the _**buffer pool**_.


        Prior to MySQL 5.6.3, the LSN was a 4-byte unsigned integer. The LSN became an 8-byte unsigned integer in MySQL 5.6.3 when the redo log file size limit increased from 4GB to 512GB, as additional bytes were required to store extra size information. Applications built on MySQL 5.6.3 or later that use LSN values should use 64-bit rather than 32-bit variables to store and compare LSN values.


        In the _**MySQL Enterprise Backup**_ product, you can specify an LSN to represent the point in time from which to take an _**incremental backup**_. The relevant LSN is displayed by the output of the **mysqlbackup** command. Once you have the LSN corresponding to the time of a full backup, you can specify that value to take a subsequent incremental backup, whose output contains another LSN for the next incremental backup.

# 复述展开

将日志文件设置得非常大可能会减少检查点期间的磁盘 I/O。 将日志文件的总大小设置为与缓冲池一样大甚至更大通常是有意义的。

## **检查点处理的工作原理**

InnoDB 实现了一种称为模糊检查点的检查点机制（fuzzy checkpointing）。 InnoDB 小批量地从缓冲池中刷新修改的数据库页面。 无需在单个批次中刷新缓冲池，否则会在检查点过程中中断用户 SQL 语句的处理。

在崩溃恢复（crash recovery）期间，InnoDB 会查找写入日志文件的检查点标签。 它知道标签之前对数据库的所有修改都存在于数据库的磁盘映像中。 然后 InnoDB 从检查点向前扫描日志文件，将记录的修改应用到数据库。

crash recovery

> [MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_crash_recovery)  
> The cleanup activities that occur when MySQL is started again after a ***crash***. For ***InnoDB*** tables, changes from incomplete transactions are replayed using data from the ***redo log***. Changes that were ***committed*** before the crash, but not yet written into the ***data files***, are reconstructed from the ***doublewrite buffer***. When the database is shut down normally, this type of activity is performed during shutdown by the ***purge*** operation.  
> During normal operation, committed data can be stored in the ***change buffer*** for a period of time before being written to the data files. There is always a tradeoff between keeping the data files up-to-date, which introduces performance overhead during normal operation, and buffering the data, which can make shutdown and crash recovery take longer.

fuzzy checkpointing

> [MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_fuzzy_checkpointing)  
> A technique that ***flushes*** small batches of ***dirty pages*** from the ***buffer pool***, rather than flushing all dirty pages at once which would disrupt database processing.

# 理解体会

InnoDB 的检查点是一种优化磁盘 I/O 和确保数据一致性的机制。通过模糊检查点，InnoDB 能够在不中断数据库操作的情况下，逐步将缓冲池中更改过的页面刷新到磁盘。这种方法避免了在检查点时必须执行的大量磁盘写入操作，从而提高了系统的整体性能。

在系统崩溃后，InnoDB 利用检查点机制来恢复数据。它通过检查点标签确定哪些数据已经安全写入磁盘，然后从该点开始应用日志文件中记录的更改，以确保数据库的完整性和一致性。通过这种方式，InnoDB 能够在系统崩溃后快速恢复到最后一致的状态。

另外这章中，我们又学习几个概念：Buffer pool，flush，LSN，crash revovery, fuzzy checkpointing
