---
password: ""
icon: ""
date: "2023-12-06"
type: Post
category: 行业概念
slug: industry-day76
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day76 【概念解析】 Intention Lock
status: Published
urlname: 655028a0-9cbb-4736-bf8f-ad946efae144
updated: "2023-12-07 12:01:00"
---

# 整理定义

在学习意向锁之前，我们先学习下 **多粒度锁**

> 💡 **多粒度锁**（英语：Multiple granularity locking，**MGL**）是一种用在[数据库](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93)以及[关系数据库](https://zh.wikipedia.org/wiki/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93)的锁定方式。MGL 常被用在两阶段锁定法（Two-phase locking）以确保可串行性（Serializability）。

**意向锁（Intention Lock）**：意向锁是放置在资源层次结构的一个级别上的锁，以保护较低级别资源上的共享锁或排它锁。

# 复述展开

意向锁（Intention Locks）是一种在数据库系统中使用的锁，它们是一种表明事务打算在表的某些行上加锁的信号。在支持多粒度锁定（Multiple Granularity Locking, MGL）的数据库系统中，意向锁是非常重要的，因为它们允许锁系统在不同级别上有效地工作，例如表级别和行级别。

### 意向锁的种类

在 InnoDB 存储引擎中，有两种主要的意向锁：

1. **意向共享锁（Intention Shared Lock, IS 锁）**：事务想要在某个数据行上加共享锁之前，必须先在包含该行的表上加上 IS 锁。
2. **意向排他锁（Intention Exclusive Lock, IX 锁）**：事务想要在某个数据行上加排他锁之前，必须先在包含该行的表上加上 IX 锁。

### 意向锁的作用

意向锁的主要作用是为了在数据库中实现锁的兼容性和冲突检测。它们不会阻止其他事务对表中的行进行读取或写入，但是它们会阻止其他事务对整个表加排他锁，直到所有的意向锁都被释放。这样做的目的是为了在不同级别的锁之间提供一种协调机制。

### 意向锁的工作原理

当一个事务想要对一行数据进行操作时，它会根据操作的类型（读或写）在表上设置相应的意向锁。如果事务想要读取一行数据，它会设置一个 IS 锁；如果事务想要修改一行数据，它会设置一个 IX 锁。

其他事务在尝试对表加锁时会检查这些意向锁。例如，如果一个事务想要对整个表加一个排他锁，它必须等待直到所有的 IS 锁和 IX 锁都被释放。这是因为排他锁要求没有其他事务可以读取或写入表中的任何行。

### 意向锁的优势

意向锁的主要优势在于它们允许数据库系统在保持高并发的同时，维护一致性和隔离性。通过在表级别上设置意向锁，事务可以在不必检查表中每一行的锁状态的情况下，快速确定是否可以对表进行锁定。这大大减少了锁定检查的开销，提高了系统的整体性能。

# 理解体会

意向锁是数据库并发控制的一个重要组成部分，它们使得数据库能够在维护事务隔离性的同时，提供高效的锁定策略。通过在表级别上声明事务的意图，意向锁简化了锁兼容性的检查过程，并使得锁管理更加高效。虽然意向锁本身不会阻止对行的访问，但它们是实现行级锁定和表级锁定协调的关键机制。

|          | **`X`** | **`IX`** | **`S`**  | **`IS`** |
| -------- | ------- | -------- | -------- | -------- |
| **`X`**  | 冲突    | 冲突     | 冲突     | 冲突     |
| **`IX`** | 冲突    | **兼容** | 冲突     | 冲突     |
| **`S`**  | 冲突    | 冲突     | **兼容** | **兼容** |
| **`IS`** | 冲突    | 冲突     | **兼容** | **兼容** |

如果想更深入 MGL 可以参考

[Multiple Granularity Locking in DBMS - GeeksforGeeks](https://www.geeksforgeeks.org/multiple-granularity-locking-in-dbms/)

# 参考

[Multiple Granularity Locking in DBMS - GeeksforGeeks](https://www.geeksforgeeks.org/multiple-granularity-locking-in-dbms/)

[Multiple granularity locking - Wikipedia](https://en.wikipedia.org/wiki/Multiple_granularity_locking)

[意向锁\_百度百科 (baidu.com)](https://baike.baidu.com/item/%E6%84%8F%E5%90%91%E9%94%81/244186)
