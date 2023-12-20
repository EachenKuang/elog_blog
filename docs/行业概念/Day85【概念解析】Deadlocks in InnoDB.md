---
password: ""
icon: ""
date: "2023-12-15"
type: Post
category: 行业概念
slug: industry-day85
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day85【概念解析】Deadlocks in InnoDB
status: Published
urlname: 751b1aff-d2cb-49d4-889f-deed17bbefd4
updated: "2023-12-20 04:56:00"
---

# 整理定义

死锁是一种情况，不同的事务无法进行，因为每个事务都持有另一个事务需要的锁。由于所有事务都在等待资源变得可用，所以它们都不会释放它们持有的锁。

> A **deadlock** is a situation where different transactions are unable to proceed because each holds a lock that the other needs. Because both transactions are waiting for a resource to become available, neither ever release the locks it holds.  
> ——《[MySQL :: MySQL 8.0 Reference Manual :: 15.7.5 Deadlocks in InnoDB](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks.html)》

# 复述展开

## 死锁的发生

当事务在多个表中锁定行（通过诸如`UPDATE`或`SELECT ... FOR UPDATE`之类的语句），但顺序相反时，可能会发生死锁。当这样的语句锁定索引记录和间隙的范围时，也可能发生死锁，每个事务由于时间问题获取一些锁，但没有获取其他锁。

## 如何避免死锁

为了减少死锁的可能性，使用事务而不是`LOCK TABLES`语句；保持插入或更新数据的事务足够小，以至于它们不会长时间保持打开状态；当不同的事务更新多个表或大范围的行时，使用相同的操作顺序（如`SELECT ... FOR UPDATE`）在每个事务中；在`SELECT ... FOR UPDATE`和`UPDATE ... WHERE`语句中使用的列上创建索引。死锁的可能性不受隔离级别的影响，因为隔离级别改变了读操作的行为，而死锁是由于写操作而发生的。

## 死锁检测

当启用死锁检测（默认）并且确实发生死锁时，InnoDB 检测到这种情况并回滚其中一个事务（受害者）。如果使用`innodb_deadlock_detect`变量禁用了死锁检测，InnoDB 依赖于`innodb_lock_wait_timeout`设置在死锁情况下回滚事务。因此，即使你的应用程序逻辑是正确的，你仍然必须处理事务必须重试的情况。要查看 InnoDB 用户事务中的最后一个死锁，使用`SHOW ENGINE INNODB STATUS`。如果频繁的死锁突显出事务结构或应用程序错误处理的问题，启用`innodb_print_all_deadlocks`将所有死锁的信息打印到 mysqld 错误日志。

# 理解体会

死锁是指多个事务因为互相持有对方需要的锁而无法进行的情况。为了避免死锁，我们可以使用事务，保持事务小而短，以及在更新多个表或大范围行的不同事务中使用相同的操作顺序。当死锁发生时，InnoDB 会检测并回滚其中一个事务。即使应用程序逻辑正确，也需要处理事务重试的情况。我们可以通过 SHOW ENGINE INNODB STATUS 查看最后一个死锁，如果频繁的死锁，可以启用`innodb_print_all_deadlocks`将所有死锁的信息打印到错误日志。

> [[MySQL :: MySQL 8.0 Reference Manual :: 15.7.5 Deadlocks in InnoDB](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks.html)]

    A deadlock is a situation where different transactions are unable to proceed because each holds a lock that the other needs. Because both transactions are waiting for a resource to become available, neither ever release the locks it holds.


    A deadlock can occur when transactions lock rows in multiple tables (through statements such as [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html) or [`SELECT ... FOR UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/select.html)), but in the opposite order. A deadlock can also occur when such statements lock ranges of index records and gaps, with each transaction acquiring some locks but not others due to a timing issue. For a deadlock example, see [Section 15.7.5.1, “An InnoDB Deadlock Example”](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlock-example.html).


    To reduce the possibility of deadlocks, use transactions rather than [`LOCK TABLES`](https://dev.mysql.com/doc/refman/8.0/en/lock-tables.html) statements; keep transactions that insert or update data small enough that they do not stay open for long periods of time; when different transactions update multiple tables or large ranges of rows, use the same order of operations (such as [`SELECT ... FOR UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/select.html)) in each transaction; create indexes on the columns used in [`SELECT ... FOR UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/select.html) and [`UPDATE ... WHERE`](https://dev.mysql.com/doc/refman/8.0/en/update.html) statements. The possibility of deadlocks is not affected by the isolation level, because the isolation level changes the behavior of read operations, while deadlocks occur because of write operations. For more information about avoiding and recovering from deadlock conditions, see [Section 15.7.5.3, “How to Minimize and Handle Deadlocks”](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks-handling.html).


    When deadlock detection is enabled (the default) and a deadlock does occur, `InnoDB` detects the condition and rolls back one of the transactions (the victim). If deadlock detection is disabled using the [`innodb_deadlock_detect`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_deadlock_detect) variable, `InnoDB` relies on the [`innodb_lock_wait_timeout`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_lock_wait_timeout) setting to roll back transactions in case of a deadlock. Thus, even if your application logic is correct, you must still handle the case where a transaction must be retried. To view the last deadlock in an `InnoDB` user transaction, use [`SHOW ENGINE INNODB STATUS`](https://dev.mysql.com/doc/refman/8.0/en/show-engine.html). If frequent deadlocks highlight a problem with transaction structure or application error handling, enable [`innodb_print_all_deadlocks`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_print_all_deadlocks) to print information about all deadlocks to the [**mysqld**](https://dev.mysql.com/doc/refman/8.0/en/mysqld.html) error log. For more information about how deadlocks are automatically detected and handled, see [Section 15.7.5.2, “Deadlock Detection”](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlock-detection.html).
