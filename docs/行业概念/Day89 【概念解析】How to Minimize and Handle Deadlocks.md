---
password: ""
icon: ""
date: "2023-12-19"
type: Post
category: 行业概念
slug: industry-day89
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day89 【概念解析】How to Minimize and Handle Deadlocks
status: Published
urlname: 8a43e44a-4180-4259-aa33-555a396c4107
updated: "2023-12-19 13:00:00"
---

# 整理定义

本章为死锁实践，主要是对于 Day88 的死锁检测中进行一些拓展。

解释了如何组织数据库操作以最大限度地减少死锁以及应用程序中所需的后续错误处理。

死锁是事务数据库中的一个典型问题，但它们并不危险，除非它们太频繁以至于您根本无法运行某些事务。 通常，您必须编写应用程序，以便它们始终准备好在事务因死锁而回滚时重新发出事务。

InnoDB 使用自动行级锁定。 即使事务只插入或删除一行，也可能会出现死锁。 这是因为这些操作并不是真正的“原子”； 它们自动在插入或删除的行的（可能是多个）索引记录上设置锁。

- 随时使用 `SHOW ENGINE INNODB STATUS` 来确定最近一次死锁的原因。这可以帮助你调整应用程序以避免死锁。
- 如果频繁的死锁警告引起关注，通过启用 `innodb_print_all_deadlocks` 变量来收集更多的调试信息。MySQL 错误日志中会记录每一次死锁的信息，而不仅仅是最新的一次。在完成调试后禁用此选项。
- 始终准备重新发起因死锁而失败的事务。死锁并不危险。只需再试一次。
- 保持事务小而持续时间短，以使它们不太容易发生冲突。
- 在进行一系列相关更改后立即提交事务，以使它们不太容易发生冲突。特别是，不要在交互式 mysql 会话中长时间保持未提交的事务。
- 如果你使用锁定读（`SELECT ... FOR UPDATE` 或 `SELECT ... FOR SHARE`），尝试使用较低的隔离级别，如 `READ COMMITTED`。
- 在事务中修改多个表，或在同一表中修改不同的行集时，每次都按照一致的顺序进行这些操作。这样，事务形成良好定义的队列，不会发生死锁。例如，在应用程序中将数据库操作组织成函数，或者调用存储过程，而不是在不同的地方编写多个类似的 INSERT、UPDATE 和 DELETE 语句序列。
- 为你的表添加合适的索引，这样你的查询就会扫描更少的索引记录并设置更少的锁。使用 `EXPLAIN SELECT` 来确定 MySQL 服务器认为哪些索引最适合你的查询。
- 使用更少的锁定。如果你可以允许 `SELECT` 返回旧快照中的数据，那么不要在它上面添加 `FOR UPDATE` 或 `FOR SHARE` 子句。在这里使用 `READ COMMITTED` 隔离级别是好的，因为同一事务中的每个一致读取都来自它自己的新鲜快照。
- 如果其他方法都无效，可以通过表级锁来序列化你的事务。与事务性表（如 InnoDB 表）一起使用`LOCK TABLES`的正确方法是先用`SET autocommit = 0`（不是`START TRANSACTION`）开始一个事务，然后是`LOCK TABLES`，并且在你明确提交事务之前不要调用`UNLOCK TABLES`。例如，如果你需要对表 t1 进行写操作并从表 t2 读取，你可以这样做：

  ```sql
  SET autocommit=0;
  LOCK TABLES t1 WRITE, t2 READ, ...;
  ... 在这里对表t1和t2做一些操作 ...
  COMMIT;
  UNLOCK TABLES;
  ```

  表级锁阻止了对表的并发更新，以牺牲系统响应性为代价避免了死锁。

- 另一种序列化事务的方法是创建一个只包含单行的辅助“信号量”表。在访问其他表之前，让每个事务更新那一行。这样，所有事务都会以串行方式发生。请注意，InnoDB 即时死锁检测算法在这种情况下也有效，因为序列化锁是一个行级锁。对于 MySQL 表级锁，必须使用超时方法来解决死锁。

# 复述展开

> **How to Minimize and Handle Deadlocks**

    This section builds on the conceptual information about deadlocks in [Section 15.7.5.2, “Deadlock Detection”](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlock-detection.html). It explains how to organize database operations to minimize deadlocks and the subsequent error handling required in applications.


    [Deadlocks](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_deadlock) are a classic problem in transactional databases, but they are not dangerous unless they are so frequent that you cannot run certain transactions at all. Normally, you must write your applications so that they are always prepared to re-issue a transaction if it gets rolled back because of a deadlock.


    `InnoDB` uses automatic row-level locking. You can get deadlocks even in the case of transactions that just insert or delete a single row. That is because these operations are not really “atomic”; they automatically set locks on the (possibly several) index records of the row inserted or deleted.


    You can cope with deadlocks and reduce the likelihood of their occurrence with the following techniques:

    - At any time, issue [`SHOW ENGINE INNODB STATUS`](https://dev.mysql.com/doc/refman/8.0/en/show-engine.html) to determine the cause of the most recent deadlock. That can help you to tune your application to avoid deadlocks.
    - If frequent deadlock warnings cause concern, collect more extensive debugging information by enabling the [`innodb_print_all_deadlocks`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_print_all_deadlocks) variable. Information about each deadlock, not just the latest one, is recorded in the MySQL [error log](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_error_log). Disable this option when you are finished debugging.
    - Always be prepared to re-issue a transaction if it fails due to deadlock. Deadlocks are not dangerous. Just try again.
    - Keep transactions small and short in duration to make them less prone to collision.
    - Commit transactions immediately after making a set of related changes to make them less prone to collision. In particular, do not leave an interactive [**mysql**](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) session open for a long time with an uncommitted transaction.
    - If you use [locking reads](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_locking_read) ([`SELECT ... FOR UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/select.html) or [`SELECT ... FOR SHARE`](https://dev.mysql.com/doc/refman/8.0/en/select.html)), try using a lower isolation level such as [`READ COMMITTED`](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html#isolevel_read-committed).
    - When modifying multiple tables within a transaction, or different sets of rows in the same table, do those operations in a consistent order each time. Then transactions form well-defined queues and do not deadlock. For example, organize database operations into functions within your application, or call stored routines, rather than coding multiple similar sequences of `INSERT`, `UPDATE`, and `DELETE` statements in different places.
    - Add well-chosen indexes to your tables so that your queries scan fewer index records and set fewer locks. Use [`EXPLAIN SELECT`](https://dev.mysql.com/doc/refman/8.0/en/explain.html) to determine which indexes the MySQL server regards as the most appropriate for your queries.
    - Use less locking. If you can afford to permit a [`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html) to return data from an old snapshot, do not add a `FOR UPDATE` or `FOR SHARE` clause to it. Using the [`READ COMMITTED`](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html#isolevel_read-committed) isolation level is good here, because each consistent read within the same transaction reads from its own fresh snapshot.
    - If nothing else helps, serialize your transactions with table-level locks. The correct way to use [`LOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html) with transactional tables, such as `InnoDB` tables, is to begin a transaction with `SET autocommit = 0` (not [`START TRANSACTION`](https://dev.mysql.com/doc/refman/8.0/en/commit.html)) followed by [`LOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html), and to not call [`UNLOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html) until you commit the transaction explicitly. For example, if you need to write to table `t1` and read from table `t2`, you can do this:

    	`SET autocommit=0;
    	LOCK TABLES t1 WRITE, t2 READ, ...;`
    	_`... do something with tables t1 and t2 here ...`_`COMMIT;
    	UNLOCK TABLES;`


    	Table-level locks prevent concurrent updates to the table, avoiding deadlocks at the expense of less responsiveness for a busy system.

    - Another way to serialize transactions is to create an auxiliary “semaphore” table that contains just a single row. Have each transaction update that row before accessing other tables. In that way, all transactions happen in a serial fashion. Note that the `InnoDB` instant deadlock detection algorithm also works in this case, because the serializing lock is a row-level lock. With MySQL table-level locks, the timeout method must be used to resolve deadlocks.

    ——《[MySQL :: MySQL 8.0 Reference Manual :: 15.7.5.3 How to Minimize and Handle Deadlocks](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks-handling.html)》

# 理解体会

上面的内容偏向实战一些，可以多多考虑下。
