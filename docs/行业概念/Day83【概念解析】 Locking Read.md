---
password: ""
icon: ""
date: "2023-12-13"
type: Post
category: 行业概念
slug: industry-day83
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day83【概念解析】 Locking Read
status: Published
urlname: 34716a96-cf51-4341-8b0a-5adc77309838
updated: "2023-12-13 08:15:00"
---

# 整理定义

> 💡 锁定读（Locking Read）是 InnoDB 中用于控制并发访问的机制之一，确保数据的一致性和完整性。

锁定读（Locking Read），锁定读是指在读取数据时，InnoDB 会对数据行施加锁定，以防止其他事务对这些数据进行修改。这是事务隔离级别和锁定策略的一部分，用于实现不同的一致性要求。

> 锁定读：一个在 InnoDB 表上执行锁定操作的 SELECT 语句。可以使用`SELECT ... FOR UPDATE`或`SELECT ... LOCK IN SHARE MODE`。根据事务的隔离级别，它有可能导致死锁。这是非锁定读（Non Lock Read）的相反操作。在只读事务中，全局表不允许使用此操作。

    在`MySQL 8.0.1`中，`SELECT ... FOR SHARE`取代了`SELECT ... LOCK IN SHARE MODE`，但为了向后兼容性，`LOCK IN SHARE MODE`仍然可用。


    ——《[MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_locking_read)》

# 复述展开

## 分类

InnoDB 中的**锁定读**主要有两种类型：

1. **共享锁（Shared Locks）**:
   - 允许一个事务读取一行数据。
   - 其他事务可以读取相同的行，但不能修改它，直到锁被释放。
   - 通常通过`SELECT ... LOCK IN SHARE MODE`语句获得。
2. **排他锁（Exclusive Locks）**:
   - 允许一个事务读取并修改一行数据。
   - 其他事务既不能读取也不能修改这行数据，直到锁被释放。
   - 通常通过`SELECT ... FOR UPDATE`语句获得。

## 例子

假设有一个账户表`accounts`，包含账户余额信息。如果你想要选中一个账户并更新其余额，你可能会这样做：

```sql
START TRANSACTION;
SELECT balance FROM accounts WHERE account_id = 1 FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
COMMIT;
```

这里，`SELECT ... FOR UPDATE`语句对账户 ID 为 1 的行施加了排他锁，直到事务提交之前，其他任何事务都不能读取或修改这一行。

# 理解体会

**锁定读（Locking Read）**需要与**一致性非锁定读（Consistent Read）**对比起来学习。另外，这里也提到了共享锁与排他锁，也算是对之前的内容进行回顾了。

InnoDB 的锁定读是事务处理中的一个重要特性，它允许数据库维护数据在并发环境下的一致性和完整性。通过共享锁和排他锁，InnoDB 能够在保证数据安全的同时，提供高效的并发访问。正确使用锁定读取可以避免不必要的数据冲突和潜在的死锁，但也需要谨慎使用，因为不当的锁定策略可能会导致性能问题。

# 参考：

[MySQL :: MySQL 8.0 Reference Manual :: 15.7.2.4 Locking Reads](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-reads.html)
