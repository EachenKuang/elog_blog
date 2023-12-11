---
password: ""
icon: ""
date: "2023-12-08"
type: Post
category: 行业概念
slug: industry-day78
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day78【概念解析】Record Lock
status: Published
urlname: e576558c-e12f-49fa-99ab-5b71add06e02
updated: "2023-12-11 05:03:00"
---

# 整理定义

记录锁（**Record Locks）**

> **Record Locks**

    A record lock is a lock on an index record. For example, `SELECT c1 FROM t WHERE c1 = 10 FOR UPDATE;` prevents any other transaction from inserting, updating, or deleting rows where the value of `t.c1` is `10`.


    Record locks always lock index records, even if a table is defined with no indexes. For such cases, `InnoDB` creates a hidden clustered index and uses this index for record locking.


    ————《[MySQL :: MySQL 8.0 Reference Manual :: 15.7.1 InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-record-locks)》

记录锁是索引记录上的锁。 例如，SELECT c1 FROM t WHERE c1 = 10 FOR UPDATE； 防止任何其他事务插入、更新或删除 t.c1 值为 10 的行。

记录锁始终锁定索引记录，即使表定义为没有索引。 对于这种情况，InnoDB 创建一个隐藏的聚集索引并使用该索引进行记录锁定。

# 复述展开

### 记录锁的工作原理

当事务 A 尝试修改表中的一行数据时，InnoDB 会检查该行是否已经被其他事务锁定：

- 如果没有被锁定，事务 A 会在该行上放置一个记录锁，并执行修改操作。
- 如果已经被其他事务锁定，事务 A 将会等待，直到锁被释放，或者超时（根据 InnoDB 的超时设置）。

记录锁是排他的（exclusive），这意味着如果事务 A 在一行数据上持有记录锁，其他事务不能对这行数据进行修改或删除，直到事务 A 提交或回滚释放锁。

### 记录锁与事务隔离级别

InnoDB 的记录锁行为受到事务隔离级别的影响。MySQL 支持四种事务隔离级别：

1. READ UNCOMMITTED（读未提交）
2. READ COMMITTED（读已提交）
3. REPEATABLE READ（可重复读，默认级别）
4. SERIALIZABLE（可串行化）

在 REPEATABLE READ 级别下，InnoDB 会使用 next-key 锁，这是记录锁和间隙锁的组合，它锁定索引记录并防止在索引记录之间插入新行。而在 READ COMMITTED 级别下，InnoDB 使用的是纯记录锁，不会锁定间隙。

### 记录锁的影响

记录锁是保证事务安全的重要机制，但它们也可能导致一些问题，如死锁和锁竞争。死锁发生在两个或多个事务相互等待对方释放锁的情况下，这会导致事务无法继续执行。InnoDB 有自己的死锁检测机制来处理这种情况，通常是通过回滚其中一个事务来解锁。

锁竞争发生在高并发环境中，许多事务试图同时锁定同一数据行。这可能会导致性能下降，因为事务必须等待其他事务释放锁。

### 最佳实践

为了最大限度地减少记录锁带来的性能问题和死锁的可能性，开发者应该遵循一些最佳实践：

- 尽量减少事务的大小和持续时间，以减少锁定资源的时间。
- 在可能的情况下，使用乐观锁定而不是悲观锁定。
- 设计索引以优化查询，减少需要锁定的行数。
- 避免不必要的全表扫描，这可能会导致大量的行被锁定。
- 使用合适的事务隔离级别，以平衡一致性需求和性能。

### 记录锁的性能优化

虽然记录锁对于事务的一致性至关重要，但不恰当的使用可能会导致性能问题。为了优化性能，可以采取以下措施：

- **索引优化**：确保查询都能利用到索引，这样可以减少锁定的数据量，因为 InnoDB 只会锁定查询触及到的索引记录。
- **查询优化**：优化查询逻辑，避免复杂的关联查询，这样可以减少锁的数量和持续时间。
- **分批处理**：对于大量数据的更新和删除操作，可以分批进行，每次处理一小部分数据，这样可以减少锁定资源的时间。
- **锁定粒度控制**：在某些情况下，可以通过锁定更大的数据粒度（如表锁）来减少锁的数量，但这通常会降低并发性。

# 理解体会

InnoDB 的记录锁是一个强大的工具，用于确保事务的一致性和隔离性。然而，它们需要谨慎使用，以避免性能降低和死锁问题。理解记录锁的工作原理和它们如何与 InnoDB 的其他锁类型（如间隙锁和临键锁）相互作用，对于数据库管理员和开发者来说至关重要。

**记录锁一般会和间隙锁一起使用，他们组合起来也就是临键锁。**

在 InnoDB 中，记录锁通常与间隙锁和临键锁一起使用，以实现不同的隔离级别。临键锁是记录锁和间隙锁的组合，它不仅锁定了一条记录，还锁定了该记录之前的间隙。这种锁的机制在**可重复读（REPEATABLE READ）**隔离级别下是默认行为，它可以防止幻读——即在一个事务中两次执行同样的查询却得到不同结果的现象。
