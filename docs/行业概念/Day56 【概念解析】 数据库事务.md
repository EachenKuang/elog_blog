---
password: ""
icon: ""
date: "2023-11-16"
type: Post
category: 行业概念
slug: industry-day56
tags:
  - 行业概念
  - 文字
  - 思考
  - 数据库
  - MySQL
summary: ""
title: Day56 【概念解析】 数据库事务
status: Published
cover: "https://image.kuangyichen.com/image/db_tx.webp"
urlname: b8166475-7087-42de-ae6d-0322521b323a
updated: "2023-11-16 15:18:00"
---

# 整理定义

## 事务（transaction）

> Transactions are atomic units of work that can be ***committed*** or ***rolled back***. When a transaction makes multiple changes to the database, either all the changes succeed when the transaction is committed, or all the changes are undone when the transaction is rolled back.

    Database transactions, as implemented by `InnoDB`, have properties that are collectively known by the acronym _**ACID**_, for atomicity, consistency, isolation, and durability.

`事务`是可以提交或回滚的<u>原子工作单元</u>。 当一个事务对数据库进行多次更改时，要么在提交事务时所有更改都成功，要么在事务回滚时所有更改都被撤消。由 InnoDB 实现的数据库事务具有统称为缩写词 ACID 的属性，即原子性、一致性、隔离性和持久性。

MySQL 事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你即需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务！

## 事务的基本特性——ACID

`ACID` 是数据库事务的四个基本特性，它们分别代表原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。

## 事务的隔离级别

数据库事务的隔离级别有 4 种，由低到高分别为

- **READ-UNCOMMITTED(读未提交)**：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。
- **READ-COMMITTED(读已提交)**：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生。
- **REPEATABLE-READ(可重复读)**：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- **SERIALIZABLE(可串行化)**：最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及[幻读](https://www.zhihu.com/search?q=%E5%B9%BB%E8%AF%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1935044903%7D)。

| 事务隔离级别                 | 读数据一致性                             | 脏读 | 不可重复读 | 幻读 |
| ---------------------------- | ---------------------------------------- | ---- | ---------- | ---- |
| 读未提交（Read Uncommitted） | 最低级别，只能保证不读取物理上损坏的数据 | ✅   | ✅         | ✅   |
| 读已提交（Read Committed）   | 语句级别                                 | ❌   | ✅         | ✅   |
| 可重复读（Repeatable Read）  | 事务级别                                 | ❌   | ❌         | ✅   |
| 串行化（Serializable）       | 最高级别                                 | ❌   | ❌         | ❌   |

# 复述展开

## ACID 展开学习

1. **原子性（Atomicity）**：原子性确保事务中的所有操作要么全部成功，要么全部失败回滚。如果事务中的任何操作失败，那么整个事务将被回滚到初始状态，不会对数据库产生任何影响。

   示例 SQL：

   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE id = 1;
   INSERT INTO transaction_history (account_id, amount) VALUES (1, 100);
   COMMIT;
   ```

   在上述示例中，如果更新账户余额和插入交易历史记录的任何一条语句失败，整个事务将被回滚，不会对数据库做出任何更改。

2. **一致性（Consistency）**：一致性确保事务在开始和结束时数据库的状态保持一致。这意味着事务必须遵守预定义的规则和约束，以确保数据的完整性。

   示例 SQL：

   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE id = 1;
   INSERT INTO transaction_history (account_id, amount) VALUES (1, 100);
   COMMIT;

   ```

   在上述示例中，如果更新账户余额导致余额为负数，事务将被回滚，以保持数据库的一致性。

3. **隔离性（Isolation）**：隔离性确保并发执行的事务相互隔离，使它们看起来像是按顺序执行的。每个事务都应该在完全独立的环境中运行，以避免数据的不一致性。

   示例 SQL：

   ```sql
   START TRANSACTION;
   SELECT balance FROM accounts WHERE id = 1;
   -- 并发事务可能会修改账户余额UPDATE accounts SET balance = balance + 50 WHERE id = 1;
   COMMIT;
   ```

   在上述示例中，如果有其他并发事务同时修改账户余额，根据隔离性的要求，这些事务应该在独立的环境中运行，以避免数据的不一致性。

4. **持久性（Durability）**：持久性确保一旦事务提交，其结果将永久保存在数据库中，即使在系统故障或崩溃的情况下也是如此。已提交的事务对数据库的更改应该是永久的。

   示例 SQL：

   ```sql
   START TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE id = 1;
   INSERT INTO transaction_history (account_id, amount) VALUES (1, 100);
   COMMIT;

   ```

   在上述示例中，一旦事务提交，更新账户余额和插入交易历史记录的结果将永久保存在数据库中，即使在系统故障或崩溃的情况下也是如此。

这些是 ACID 特性在 MySQL 中的解释和示例。ACID 特性确保了数据库事务的可靠性、一致性和持久性，使得数据库操作更加可靠和可控。

## 事务的隔离级别与并发问题

以下是四种事务隔离级别下可能出现的情况，以及与脏读、不可重复读和幻读的关系：

1. 读未提交（Read Uncommitted）：在这个级别，事务 A 可以读取事务 B 未提交的数据。例如，事务 B 正在尝试更新一条记录，但还未提交，此时事务 A 读取了这条记录，这就可能导致脏读，因为事务 B 可能在之后回滚，那么事务 A 读取的数据就是无效的。
2. 读已提交（Read Committed）：在这个级别，事务 A 只能读取事务 B 已经提交的数据。例如，事务 A 读取了一条记录，然后事务 B 更新了这条记录并提交，当事务 A 再次读取这条记录时，就会发现数据已经改变，这就可能导致不可重复读。
3. 可重复读（Repeatable Read）：在这个级别，事务 A 在同一事务内多次读取同一数据，会得到相同的结果，除非数据被事务 A 自己修改。例如，事务 A 读取了一个范围内的所有记录，然后事务 B 插入了一个新的记录并提交，当事务 A 再次读取这个范围的记录时，就会发现有一个新的记录，这就可能导致幻读。
4. 串行化（Serializable）：这是最高的隔离级别，事务 A 和事务 B 会被强制串行执行，这样就可以避免脏读、不可重复读和幻读。但是，这种级别的隔离性能最差，因为事务没有并发执行的可能。

| 隔离级别                     | 特点                                                                         | 并发问题                                                                                                                                                                                              |
| ---------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 读未提交（Read Uncommitted） | 一个事务可以读取另一个未提交事务的数据。这是最低的隔离级别。                 | 可能会导致脏读（Dirty Read，读取到其他未提交事务的数据）、不可重复读（Non-Repeatable Read，同一事务中多次读取同一数据返回的结果有所不同）和幻读（Phantom Read，同一事务两次查询的结果不一致）等问题。 |
| 读已提交（Read Committed）   | 一个事务只能读取另一个已提交事务的数据。这是大多数数据库系统的默认隔离级别。 | 可以避免脏读，但可能会导致不可重复读和幻读。                                                                                                                                                          |
| 可重复读（Repeatable Read）  | 在同一事务中，多次读取同一数据会返回相同的结果，除非数据被本事务自己修改。   | 可以避免脏读和不可重复读，但可能会导致幻读。**在 MySQL InnoDB 存储引擎中，可重复读是默认的隔离级别，并且通过多版本并发控制（MVCC）解决了幻读问题。**                                                  |
| 串行化（Serializable）       | 最严格的隔离级别。事务串行化顺序执行，避免了脏读、不可重复读和幻读。         | 可以避免所有并发问题，但性能最差，因为事务完全串行执行。                                                                                                                                              |

需要说明的是，事务隔离级别和数据访问的并发性是对立的，事务隔离级别越高并发性就越差。所以要根据具体的应用来确定合适的事务隔离级别，这个地方没有万能的原则。

## **并发事务处理带来的问题**

- **更新丢失（Lost Update)**：事务 A 和事务 B 选择同一行，然后基于最初选定的值更新该行时，由于两个事务都不知道彼此的存在，就会发生丢失更新问题
- **脏读(Dirty Reads)**：事务 A 读取了事务 B 更新的数据，然后 B 回滚操作，那么 A 读取到的数据是脏数据
- **不可重复读（Non-Repeatable Reads)**：事务 A 多次读取同一数据，事务 B 在事务 A 多次读取的过程中，对数据作了更新并提交，导致事务 A 多次读取同一数据时，结果不一致。
- **幻读（Phantom Reads)**：幻读与不可重复读类似。它发生在一个事务 A 读取了几行数据，接着另一个并发事务 B 插入了一些数据时。在随后的查询中，事务 A 就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

> 💡 **幻读\*\***和不可重复读的区别：\*\*
>
> - **不可重复读的重点是修改**：在同一事务中，同样的条件，第一次读的数据和第二次读的数据不一样。（因为中间有其他事务提交了修改）
> - **幻读的重点在于新增或者删除**：在同一事务中，同样的条件,，第一次和第二次读出来的记录数不一样。（因为中间有其他事务提交了插入/删除）

    - **不可重复读的重点是修改**：在同一事务中，同样的条件，第一次读的数据和第二次读的数据不一样。（因为中间有其他事务提交了修改）
    - **幻读的重点在于新增或者删除**：在同一事务中，同样的条件,，第一次和第二次读出来的记录数不一样。（因为中间有其他事务提交了插入/删除）

## **并发事务处理带来的问题的解决办法：**

- “更新丢失”通常是应该完全避免的。但防止更新丢失，并不能单靠数据库事务控制器来解决，需要应用程序对要更新的数据加必要的锁来解决，因此，防止更新丢失应该是应用的责任。
- “脏读” 、 “不可重复读”和“幻读” ，其实都是数据库读一致性问题，必须由数据库提供一定的事务隔离机制来解决：
- 一种是加锁：在读取数据前，对其加锁，阻止其他事务对数据进行修改。
- 另一种是数据多版本并发控制（MultiVersion Concurrency Control，简称 MVCC 或 MCC），也称为多版本数据库：不用加任何锁， 通过一定机制生成一个数据请求时间点的一致性数据快照 （Snapshot)，并用这个快照来提供一定级别 （语句级或事务级） 的一致性读取。从用户的角度来看，好象是数据库可以提供同一数据的多个版本。

# 理解体会

学习数据库事务是理解和掌握数据库管理系统的重要部分。事务不仅是数据库操作的基本单位，而且是保证数据一致性和完整性的关键机制。因此，对事务的深入理解对于任何使用或设计数据库系统的人来说都是必不可少的。

首先，理解事务的基本概念和特性是学习事务的第一步。事务是一系列数据库操作的集合，这些操作要么全部成功，要么全部失败。这种“全有或全无”的特性，也就是事务的原子性，是理解事务的关键。此外，事务还有其他重要的特性，如一致性、隔离性和持久性，这些都是事务的 ACID 属性。理解这些特性以及它们如何保证数据的一致性和完整性是非常重要的。

其次，理解事务的工作原理和实现机制也是学习事务的重要部分。例如，数据库系统如何使用锁和其他并发控制机制来实现事务的隔离性，如何使用日志和恢复机制来实现事务的持久性，等等。这些知识可以帮助我们理解数据库系统的内部工作原理，以及如何设计和优化数据库应用程序。

再次，理解不同的事务隔离级别以及它们的优点和缺点也是非常重要的。不同的隔离级别提供了不同的并发控制和性能权衡，理解这些权衡可以帮助我们根据应用程序的需求选择合适的隔离级别。

最后，通过实践来学习和理解事务也是非常重要的。通过编写和执行事务，我们可以更好地理解事务的工作原理，以及如何在实际应用中使用事务。实践也可以帮助我们理解事务的性能影响，以及如何优化事务以提高应用程序的性能。

总的来说，学习事务是一个深入理解数据库系统的过程，它需要理论知识和实践经验的结合。通过学习事务，我们可以更好地理解和掌握数据库系统，从而设计和实现更有效、更可靠的数据库应用程序。

> 📌 **快速跳转链接**  
> 【概念解析】启动
>
> 【概念解析】Day 1 - 10
>
> 【概念解析】Day 11 - 20
>
> 【概念解析】Day 21 - 30
>
> 【概念解析】Day 31 - 40
>
> 【概念解析】Day 41 - 50
>
> 【概念解析】Day 51 - 60
>
> 【概念解析】Day 61 - 70

<details>
<summary>【概念解析】启动</summary>

[bookmark](https://kuangyichen.com/article/industry)

[bookmark](https://kuangyichen.com/article/start-industry-100-words)

</details>

<details>
<summary>【概念解析】Day 1 - 10</summary>

[bookmark](https://kuangyichen.com/article/industry-day1)

[bookmark](https://kuangyichen.com/article/industry-day2)

[bookmark](https://kuangyichen.com/article/industry-day3)

[bookmark](https://kuangyichen.com/article/industry-day4)

[bookmark](https://kuangyichen.com/article/industry-day5)

[bookmark](https://kuangyichen.com/article/industry-day6)

[bookmark](https://kuangyichen.com/article/industry-day7)

[bookmark](https://kuangyichen.com/article/industry-day8)

[bookmark](https://kuangyichen.com/article/industry-day9)

[bookmark](https://kuangyichen.com/article/industry-day10)

</details>

<details>
<summary>【概念解析】Day 11 - 20</summary>

[bookmark](https://kuangyichen.com/article/industry-day11)

[bookmark](https://kuangyichen.com/article/industry-day12)

[bookmark](https://kuangyichen.com/article/industry-day13)

[bookmark](https://kuangyichen.com/article/industry-day14)

[bookmark](https://kuangyichen.com/article/industry-day15)

[bookmark](https://kuangyichen.com/article/industry-day16)

[bookmark](https://kuangyichen.com/article/industry-day17)

[bookmark](https://kuangyichen.com/article/industry-day18)

[bookmark](https://kuangyichen.com/article/industry-day19)

[bookmark](https://kuangyichen.com/article/industry-day20)

</details>

<details>
<summary>【概念解析】Day 21 - 30</summary>

[bookmark](https://kuangyichen.com/article/industry-day21)

[bookmark](https://kuangyichen.com/article/industry-day22)

[bookmark](https://kuangyichen.com/article/industry-day23)

[bookmark](https://kuangyichen.com/article/industry-day24)

[bookmark](https://kuangyichen.com/article/industry-day25)

[bookmark](https://kuangyichen.com/article/industry-day26)

[bookmark](https://kuangyichen.com/article/industry-day27)

[bookmark](https://kuangyichen.com/article/industry-day28)

[bookmark](https://kuangyichen.com/article/industry-day29)

[bookmark](https://kuangyichen.com/article/industry-day30)

</details>

<details>
<summary>【概念解析】Day 31 - 40</summary>

[bookmark](https://kuangyichen.com/article/industry-day31)

[bookmark](https://kuangyichen.com/article/industry-day32)

[bookmark](https://kuangyichen.com/article/industry-day33)

[bookmark](https://kuangyichen.com/article/industry-day34)

[bookmark](https://kuangyichen.com/article/industry-day35)

[bookmark](https://kuangyichen.com/article/industry-day36)

[bookmark](https://kuangyichen.com/article/industry-day37)

[bookmark](https://kuangyichen.com/article/industry-day38)

[bookmark](https://kuangyichen.com/article/industry-day39)

[bookmark](https://kuangyichen.com/article/industry-day40)

</details>

<details>
<summary>【概念解析】Day 41 - 50</summary>

[bookmark](https://kuangyichen.com/article/industry-day41)

[bookmark](https://kuangyichen.com/article/industry-day42)

[bookmark](https://kuangyichen.com/article/industry-day43)

[bookmark](https://kuangyichen.com/article/industry-day44)

[bookmark](https://kuangyichen.com/article/industry-day45)

[bookmark](https://kuangyichen.com/article/industry-day46)

[bookmark](https://kuangyichen.com/article/industry-day47)

[bookmark](https://kuangyichen.com/article/industry-day48)

[bookmark](https://kuangyichen.com/article/industry-day49)

[bookmark](https://kuangyichen.com/article/industry-day50)

</details>

<details>
<summary>【概念解析】Day 51 - 60</summary>

[bookmark](https://kuangyichen.com/article/industry-day51)

[bookmark](https://kuangyichen.com/article/industry-day52)

[bookmark](https://kuangyichen.com/article/industry-day53)

[bookmark](https://kuangyichen.com/article/industry-day54)

[bookmark](https://kuangyichen.com/article/industry-day55)

[bookmark](https://kuangyichen.com/article/industry-day56)

[bookmark](https://kuangyichen.com/article/industry-day57)

[bookmark](https://kuangyichen.com/article/industry-day58)

[bookmark](https://kuangyichen.com/article/industry-day59)

</details>
