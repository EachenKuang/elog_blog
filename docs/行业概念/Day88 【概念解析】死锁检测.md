---
password: ""
icon: ""
date: "2023-12-18"
type: Post
category: 行业概念
slug: industry-day88
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day88 【概念解析】死锁检测
status: Published
urlname: 1bc8c4f5-5de3-4b41-a82a-c96d8ba57c72
updated: "2023-12-18 08:35:00"
---

# 整理定义

死锁检测（**Deadlock Detection**）

当启用死锁检测（默认）时，InnoDB 会自动检测事务死锁并回滚一个或多个事务以打破死锁。 InnoDB 尝试选择小事务进行回滚，其中事务的大小由插入、更新或删除的行数决定。

如果 `innodb_table_locks` = 1（默认值）且 `autocommit` = 0，则 InnoDB 会意识到表锁，而它上方的 MySQL 层知道行级锁。 否则，InnoDB 无法检测涉及 `MySQL LOCK TABLES` 语句设置的表锁或 `InnoDB` 以外的存储引擎设置的锁的死锁。 通过设置 `innodb_lock_wait_timeout` 系统变量的值来解决这些情况。

如果 `InnoDB Monitor` 输出的 `LATEST DETECTED DEADLOCK` 部分包含一条消息，指出 `TOO DEEP OR LONG SEARCH IN THE LOCK TABLE WAITS-FOR GRAPH, WE WILL ROLL BACK FOLLOWING TRANSACTION`，这表明等待列表上的事务数量已达到 `200` 的上限。**超过 200 个事务的等待列表将被视为死锁，并且尝试检查等待列表的事务将被回滚。** 如果锁定线程必须查看等待列表上的事务拥有的超过 1,000,000 个锁，也可能会发生相同的错误。

## 禁用死锁检测

在高并发系统上，当大量线程等待同一锁时，死锁检测可能会导致速度减慢。 有时，禁用死锁检测并在发生死锁时依靠 innodb_lock_wait_timeout 设置进行事务回滚可能会更有效。 可以使用 innodb_deadlock_detect 变量禁用死锁检测。

# 复述展开

> **Deadlock Detection**

    When [deadlock detection](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_deadlock_detection) is enabled (the default), `InnoDB` automatically detects transaction [deadlocks](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_deadlock) and rolls back a transaction or transactions to break the deadlock. `InnoDB` tries to pick small transactions to roll back, where the size of a transaction is determined by the number of rows inserted, updated, or deleted.


    `InnoDB` is aware of table locks if `innodb_table_locks = 1` (the default) and [`autocommit = 0`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_autocommit), and the MySQL layer above it knows about row-level locks. Otherwise, `InnoDB` cannot detect deadlocks where a table lock set by a MySQL [`LOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html) statement or a lock set by a storage engine other than `InnoDB` is involved. Resolve these situations by setting the value of the [`innodb_lock_wait_timeout`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_lock_wait_timeout) system variable.


    If the `LATEST DETECTED DEADLOCK` section of `InnoDB` Monitor output includes a message stating TOO DEEP OR LONG SEARCH IN THE LOCK TABLE WAITS-FOR GRAPH, WE WILL ROLL BACK FOLLOWING TRANSACTION, this indicates that the number of transactions on the wait-for list has reached a limit of 200. A wait-for list that exceeds 200 transactions is treated as a deadlock and the transaction attempting to check the wait-for list is rolled back. The same error may also occur if the locking thread must look at more than 1,000,000 locks owned by transactions on the wait-for list.


    For techniques to organize database operations to avoid deadlocks, see [Section 15.7.5, “Deadlocks in InnoDB”](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks.html).


    **Disabling Deadlock Detection**


    On high concurrency systems, deadlock detection can cause a slowdown when numerous threads wait for the same lock. At times, it may be more efficient to disable deadlock detection and rely on the [`innodb_lock_wait_timeout`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_lock_wait_timeout) setting for transaction rollback when a deadlock occurs. Deadlock detection can be disabled using the [`innodb_deadlock_detect`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_deadlock_detect) variable.


    ——《[MySQL :: MySQL 8.0 Reference Manual :: 15.7.5.2 Deadlock Detection](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlock-detection.html)》

# 理解体会

在 InnoDB 中，死锁检测是一个重要的功能，它可以自动检测并解决事务死锁的问题。事务死锁是指两个或更多的事务在等待对方释放资源，从而导致所有事务都无法继续进行的情况。

当启用死锁检测（默认情况下是启用的）时，InnoDB 会自动检测这种死锁情况，并选择一个或多个事务进行回滚，以打破死锁。InnoDB 通常会选择较小的事务进行回滚，这里的"小"是指事务涉及的行数较少。

InnoDB 可以感知到表级别的锁，如果设置了`innodb_table_locks = 1`（默认值）和`autocommit = 0`。然而，如果死锁涉及的是由 MySQL 的`LOCK TABLES`语句或者由其他存储引擎设置的锁，InnoDB 则无法检测到。在这种情况下，可以通过设置`innodb_lock_wait_timeout`系统变量的值来解决。

在某些情况下，例如高并发系统中，死锁检测可能会导致性能下降，因为可能有大量的线程在等待同一把锁。在这种情况下，可能更有效的策略是禁用死锁检测，而依赖于`innodb_lock_wait_timeout`设置来在发生死锁时进行事务回滚。可以通过设置`innodb_deadlock_detect`变量来禁用死锁检测。

总的来说，InnoDB 的死锁检测是一个强大的工具，可以帮助数据库自动解决可能的死锁问题，但在某些情况下，可能需要进行一些调整以优化性能。
