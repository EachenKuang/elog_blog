---
password: ""
icon: ""
date: "2023-12-20"
type: Post
category: 行业概念
slug: industry-day90
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day90【概念解析】Transaction Scheduling
status: Published
urlname: 7f0e9c68-f40d-44c7-a58b-10cf142c14f0
updated: "2023-12-20 04:57:00"
---

# 整理定义

## **Transaction Scheduling**

> In the fields of [databases](https://en.wikipedia.org/wiki/Database) and [transaction processing](https://en.wikipedia.org/wiki/Transaction_processing) (transaction management), a **schedule** (or **history**) of a system is an abstract model to describe [execution](<https://en.wikipedia.org/wiki/Execution_(computing)>) of transactions running in the system. Often it is a *list* of operations (actions) ordered by time, performed by a set of [transactions](https://en.wikipedia.org/wiki/Database_transaction) that are executed together in the system. If the order in time between certain operations is not determined by the system, then a [_partial order_](https://en.wikipedia.org/wiki/Partial_order) is used. Examples of such operations are requesting a read operation, reading, writing, aborting, [committing](<https://en.wikipedia.org/wiki/Commit_(data_management)>), requesting a [lock](<https://en.wikipedia.org/wiki/Lock_(computer_science)>), locking, etc. Not all transaction operation types should be included in a schedule, and typically only selected operation types (e.g., data access operations) are included, as needed to reason about and describe certain phenomena. Schedules and schedule properties are fundamental concepts in database [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control) theory.

**事务调度：**<u>在数据库和事务处理（事务管理）领域中，系统的调度（或历史）是描述系统中运行的事务的执行的抽象模型</u>。 通常，它是按时间排序的操作（动作）列表，由系统中一起执行的一组事务执行。 如果某些操作之间的时间顺序不是由系统确定的，则使用部分顺序。 此类操作的示例包括请求读操作、读取、写入、中止、提交、请求锁、加锁等。并非所有事务操作类型都应包含在调度中，并且通常仅包含选定的操作类型（例如，数据访问操作） ）根据需要被包括在内，以推理和描述某些现象。 调度和调度属性是数据库并发控制理论中的基本概念。

# 复述展开

## CATS

CATS：Contention-Aware Transaction Scheduling algorithm

InnoDB 使用`CATS算法`来优先处理等待锁的事务。当多个事务在等待同一个对象上的锁时，CATS 算法通过分配一个调度权重来确定哪个事务首先获得锁，这个权重是基于一个事务阻塞的其他事务数量来计算的。
例如，如果有两个事务在等待同一个对象上的锁，那么<u>阻塞更多事务的那个事务</u>会被分配<u>更高的调度权重</u>。如果权重相同，优先权将给予<u>等待时间最长</u>的事务。

> ⛔ **注意**  
> 在`MySQL 8.0.20`之前，InnoDB 还使用先进先出（FIFO）算法来调度事务，只有在锁争用非常激烈的情况下才使用 CATS 算法。MySQL 8.0.20 中 CATS 算法的增强使得 FIFO 算法变得多余，允许其被移除。自 MySQL 8.0.20 起，之前由 FIFO 算法执行的事务调度工作由 CATS 算法来执行。在某些情况下，这一变化可能会影响事务获得锁的顺序。

你可以通过查询信息模式 INNODB_TRX 表中的 TRX_SCHEDULE_WEIGHT 列来查看事务调度权重。权重只为等待中的事务计算。等待中的事务是指那些处于 LOCK WAIT 事务执行状态的事务，如 TRX_STATE 列所报告的。一个不在等待锁的事务会报告一个 NULL 的 TRX_SCHEDULE_WEIGHT 值。

INNODB_METRICS 计数器提供了用于监控代码级别事务调度事件的功能。

- `lock_rec_release_attempts`：尝试释放记录锁的次数。单次尝试可能导致零个或多个记录锁被释放，因为在单个结构中可能有零个或多个记录锁。
- `lock_rec_grant_attempts`：尝试授予记录锁的次数。单次尝试可能导致零个或多个记录锁被授予。
- `lock_schedule_refreshes`：分析等待图以更新调度事务权重的次数。

# 理解体会

InnoDB 中的 Contention-Aware Transaction Scheduling（CATS）算法是一种用于管理数据库锁争用的高级调度机制。在数据库系统中，当多个事务试图同时访问同一资源（如行、页或表）时，就会发生锁争用。为了维护数据的一致性和完整性，数据库管理系统（DBMS）必须确保在任何给定时间，只有一个事务可以修改特定的数据项。

在传统的锁调度机制中，如先进先出（FIFO）策略，事务是按照它们到达的顺序获得锁的。然而，这种方法并不总是最优的，因为它没有考虑到事务之间的依赖关系。一个事务可能阻塞了多个其他事务，而另一个事务可能只阻塞了一个。在这种情况下，让阻塞更多事务的那个事务先行，可能会更快地解决锁争用，从而提高整体的并发性和性能。

CATS 算法正是基于这样的思想。它通过分配一个调度权重来优先处理等待锁的事务。这个权重是基于一个事务阻塞的其他事务的数量来计算的。如果一个事务阻塞了许多其他事务，它将获得更高的权重，因此有更高的优先级来获得锁。这样做的目的是尽量减少整体的等待时间和提高事务吞吐量。

如果两个事务的调度权重相同，CATS 算法会考虑它们的等待时间，优先给予等待时间最长的事务。这种方法既考虑了事务的影响范围，也考虑了公平性。

从 MySQL 8.0.20 版本开始，由于 CATS 算法的增强，原有的 FIFO 算法被移除，所有的事务调度都由 CATS 算法来执行。这意味着在高并发和锁争用的环境下，CATS 算法可以更智能地调度事务，减少锁等待时间，提高数据库的性能。

总的来说，CATS 算法是一种更加智能和动态的事务调度方法，它通过考虑事务之间的相互影响和等待时间，来优化锁的分配，从而提高数据库的并发处理能力和整体性能。
