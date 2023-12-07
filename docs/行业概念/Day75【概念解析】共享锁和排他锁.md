---
password: ""
icon: ""
date: "2023-12-05"
type: Post
category: 行业概念
slug: industry-day75
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day75【概念解析】共享锁和排他锁
status: Published
urlname: 7ab75082-9d83-4342-9e76-eaf9676b0d76
updated: "2023-12-07 12:01:00"
---

# 整理定义

**名词定义：Shared and Exclusive Locks（共享锁**和**排它锁**，或者也叫 S 锁，X 锁）

- 共享锁（Shared Lock，也叫 S 锁）
- 排他锁（Exclusive Lock，也叫排他锁）

# 复述展开

## 共享锁

> **Shared Locks**

    A kind of _**lock**_ that allows other _**transactions**_ to read the locked object, and to also acquire other shared locks on it, but not to write to it. The opposite of _**exclusive lock**_.

> 💡 一种锁，允许其他事务读取锁定的对象，并获取该对象上的其他共享锁，但不能写入该对象。 与排他锁相反。

## 排他锁

> **Exclusive Lock**  
> A kind of ***lock*** that prevents any other ***transaction*** from locking the same row. Depending on the transaction ***isolation level***, this kind of lock might block other transactions from writing to the same row, or might also block other transactions from reading the same row. The default `InnoDB` isolation level, ***REPEATABLE READ***, enables higher ***concurrency*** by allowing transactions to read rows that have exclusive locks, a technique known as ***consistent read***.

> 💡 一种锁，可防止任何其他事务锁定同一行。 根据事务隔离级别，这种锁可能会阻止其他事务写入同一行，也可能会阻止其他事务读取同一行。 默认的 InnoDB 隔离级别“可重复读取”通过允许事务读取具有排他锁的行来实现更高的并发性，这种技术称为“一致性读取”。

# 理解体会

下面用表格展示下共享锁和排它锁的兼容

|      | S 锁   | X 锁   |
| ---- | ------ | ------ |
| S 锁 | 兼容   | 不兼容 |
| X 锁 | 不兼容 | 不兼容 |

> 💡 如果一个事务 T1 给 r 行加了 S 锁，那么事务 T2 可以读取 r ，但无法写入 r，只有等 T1 释放了 S 锁才能写入。  
> 如果一个事务 T1 给 r 行加了 X 锁，那么事务 T2 既不能读取也不能写入 r，只有等 T1 释放了 X 锁才能操作。

共享锁的设计允许多个事务并发地读取同一数据，这在只读操作占主导的应用场景中非常有用，因为它提高了并发性能。然而，共享锁也可能导致所谓的“写入阻塞”，即如果有事务持有数据行的 S 锁，那么其他想要写入的事务就必须等待，直到所有的 S 锁被释放。

排他锁的主要特点是它保证了事务的独立性。在并发环境中，如果多个事务试图修改同一数据，排他锁确保每个事务都可以在没有其他事务干扰的情况下完成操作。这就避免了数据的不一致性和更新丢失的问题。例如，当两个银行客户同时试图从同一个账户中取款时，排他锁确保了只有一个事务能够在同一时间修改账户余额，从而防止了超额提款。
