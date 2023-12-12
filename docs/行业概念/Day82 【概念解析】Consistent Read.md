---
password: ""
icon: ""
date: "2023-12-12"
type: Post
category: 行业概念
slug: industry-day82
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day82 【概念解析】Consistent Read
status: Published
urlname: 6bfc9cbd-11f7-4230-b508-305a460af8ca
updated: "2023-12-12 02:25:00"
---

# 整理定义

C**onsistent Read 一致性读**

> **consistent read**

    A read operation that uses _**snapshot**_ information to present query results based on a point in time, regardless of changes performed by other transactions running at the same time. If queried data has been changed by another transaction, the original data is reconstructed based on the contents of the _**undo log**_. This technique avoids some of the _**locking**_ issues that can reduce _**concurrency**_ by forcing transactions to wait for other transactions to finish.
    With _**REPEATABLE READ**_ _**isolation level**_, the snapshot is based on the time when the first read operation is performed. With _**READ COMMITTED**_ isolation level, the snapshot is reset to the time of each consistent read operation.
    Consistent read is the default mode in which `InnoDB` processes `SELECT` statements in _**READ COMMITTED**_ and _**REPEATABLE READ**_ isolation levels. Because a consistent read does not set any locks on the tables it accesses, other sessions are free to modify those tables while a consistent read is being performed on the table.
    ——《[MySQL :: MySQL 8.0 Reference Manual :: MySQL Glossary](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_consistent_read)》

**一致性读**，是一种读操作，它使用快照信息基于某个时间点呈现查询结果，而不管同时运行的其他事务执行的更改如何。 如果查询的数据被另一个事务更改，则根据 undo log 的内容重建原始数据。 此技术通过强制事务等待其他事务完成来避免一些可能降低并发性的锁定问题。

使用 `REPEATABLE READ` 隔离级别时，快照基于执行第一次读取操作的时间。 使用 `READ COMMITTED` 隔离级别，快照将重置为每个一致读操作的时间。

一致性读是 InnoDB 在 `READ COMMITTED` 和 `REPEATABLE READ` 隔离级别下处理 SELECT 语句的默认模式。 由于一致性读不会对其访问的表设置任何锁定，因此在对表执行一致性读时，其他会话可以自由修改这些表。

# 复述展开

**一致性读**意味着 InnoDB 使用多版本控制来为查询提供数据库在某一时间点的快照。查询只能看到在那个时间点<u>之前</u>已提交的事务所做的更改，而看不到之后或未提交事务所做的更改。这个规则的例外是，<u>查询可以看到同一事务中较早语句所做的更改</u>。

> 🚫 这个例外会导致以下异常情况：如果你更新了表中的一些行，一个 SELECT 语句会看到这些行的最新版本，但它也可能看到任何行的旧版本。如果其他会话同时更新同一个表，这种异常意味着你可能看到的表状态是数据库中实际从未存在过的。

不同隔离级别下的一致性读

- 在 REPEATABLE READ（默认级别）隔离级别下，同一事务内的所有一致性读取都会读取该事务中**第一次读取**时建立的快照。通过提交当前事务然后发出新的查询，你可以为你的查询获取一个更新的快照。
- 在 READ COMMITTED 隔离级别下，事务中的每个一致性读取都会设置并读取它自己的**最新快照**。

> 💡 一致性读取是 InnoDB 在 READ COMMITTED 和 REPEATABLE READ 隔离级别下处理 SELECT 语句的默认模式。一致性读取不会对它访问的表设置任何锁，因此其他会话可以自由地同时修改这些表。  
> 假设你正在默认的 REPEATABLE READ 隔离级别下运行。当你发出一致性读取（即，一个普通的 SELECT 语句）时，InnoDB 会给你的事务一个时间点，根据这个时间点你的查询就可以看到数据库的状态。如果另一个事务在你的时间点被分配后删除了一行并提交，你将看不到该行被删除。插入和更新也是类似处理的。

在下面的示例中，仅当 Session B 提交了插入并且 Session A 也提交了时，SessionA 才能看到 Session B 插入的行，因此时间点提前超过了 Session B 的提交。

```sql
        	 Session A              Session B

           SET autocommit=0;      SET autocommit=0;
time
|          SELECT * FROM t;
|          empty set
|                                 INSERT INTO t VALUES (1, 2);
|
v          SELECT * FROM t;
           empty set
                                  COMMIT;

           SELECT * FROM t;
           empty set

           COMMIT;

           SELECT * FROM t;
           ---------------------
           |    1    |    2    |
           ---------------------
```

如果想看到最新的结果，可以使用

```sql
SELECT * FROM t FOR SHARE;
```

在`READ COMMITTED`隔离级别下，事务中的每个一致性读取都会设置并读取它自己的新鲜快照。使用 FOR SHARE 时，会发生锁定读取：SELECT 语句会阻塞，直到包含最新行的事务结束。

一致性读取在某些 DDL 语句上不适用：

- 一致性读取在 DROP TABLE 上不适用，因为 MySQL 无法使用已被删除的表，而 InnoDB 会销毁该表。
- 一致性读取在 ALTER TABLE 操作上不适用，这些操作会创建原始表的临时副本，并在临时副本建立后删除原始表。当你在事务中重新发起一致性读取时，新表中的行不可见，因为这些行在事务快照拍摄时不存在。在这种情况下，事务会返回一个错误：ER_TABLE_DEF_CHANGED，“表定义已更改，请重试事务”。

对于像 INSERT INTO ... SELECT、UPDATE ... (SELECT)和 CREATE TABLE ... SELECT 这样的子句中的选择，如果没有指定 FOR UPDATE 或 FOR SHARE，读取类型会有所不同：

- 默认情况下，InnoDB 对这些语句使用更强的锁定，并且 SELECT 部分的行为类似`READ COMMITTED`，其中每个一致性读取，即使在同一事务内，也会设置并读取它自己的新鲜快照。
- 要在这种情况下执行非锁定读取，将事务的隔离级别设置为`READ UNCOMMITTED`或`READ COMMITTED`，以避免对从所选表中读取的行设置锁。

# 理解体会

一致性读（Consistent Read）是数据库事务中的一个重要概念，特别是在支持多版本并发控制（MVCC）的数据库系统中，如 InnoDB 存储引擎。我的理解如下：

1. **时间点快照**：一致性读提供了在特定时间点的数据快照。这意味着，当你执行一个查询操作时，你看到的数据是在事务开始时或者第一次一致性读取发生时的状态，不受之后其他事务所做更改的影响。
2. **非阻塞读取**：在一致性读取模式下，SELECT 语句不会对读取的数据加锁，这允许其他事务可以并发地对这些数据进行修改，从而提高了数据库的并发性能。
3. **隔离级别的影响**：一致性读取的行为会受到事务隔离级别的影响。在 REPEATABLE READ（可重复读）隔离级别下，事务中的所有一致性读取都会看到第一次读取时的数据快照。而在 READ COMMITTED（读已提交）隔离级别下，每次一致性读取都会获得一个新的数据快照。
4. **处理数据修改**：在一致性读取中，你可以看到事务开始前已经提交的修改，但看不到在你的事务开始后其他事务所做的修改。如果你的事务中有修改操作，那么同一事务中后续的一致性读取会看到这些修改，因为它们是由当前事务所做的。
5. **与锁定读取的对比**：一致性读取与锁定读取（如 SELECT ... FOR UPDATE 或 SELECT ... FOR SHARE）不同。锁定读取会对选中的数据行加锁，防止其他事务对这些行进行修改，直到当前事务完成。而一致性读取则允许其他事务的并发修改，因为它不依赖于锁来保持数据的一致性。
6. **DDL 语句的限制**：一致性读取不适用于某些数据定义语言（DDL）操作，如 DROP TABLE 或 ALTER TABLE。这是因为这些操作会改变表的结构，甚至删除表，从而使得在操作前的数据快照不再有效。
7. **错误处理**：如果在执行 DDL 操作后尝试进行一致性读取，数据库可能会返回错误，因为所依赖的数据结构已经发生了变化。
8. **事务中的一致性读取**：在一个事务中，如果你执行了一个更新操作，随后的一致性读取会看到这个更新，即使其他事务尚未看到这个变化。这是因为事务提供了一个隔离的环境，确保了操作的原子性和一致性。

总的来说，一致性读取是数据库事务隔离的一个关键特性，它允许用户在不加锁的情况下读取一致的数据快照，同时也支持高并发的数据访问。这种机制在保证数据一致性的同时，避免了长时间的锁等待和潜在的死锁问题，是数据库设计中平衡一致性、可用性和分区容错性（CAP 理论）的一个实际应用。
