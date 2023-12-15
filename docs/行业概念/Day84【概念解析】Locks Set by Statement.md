---
password: ""
icon: ""
date: "2023-12-14"
type: Post
category: 行业概念
slug: industry-day84
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day84【概念解析】Locks Set by Statement
status: Published
urlname: facd9060-c655-4456-a316-b930174e50db
updated: "2023-12-15 01:30:00"
---

# 整理定义

**不同语句的的加锁机制（Locks Set by Statement）**

本章主要总结下之前学习的 行锁，间隙锁，临键锁在不同语句下的表现。

【[MySQL :: MySQL 8.0 Reference Manual :: 15.7.3 Locks Set by Different SQL Statements in InnoDB](https://dev.mysql.com/doc/refman/8.0/en/innodb-locks-set.html)】

# 复述展开

锁定读取、更新或删除通常会在 SQL 语句处理过程中扫描的每个索引记录上设置记录锁。 语句中是否存在排除该行的 WHERE 条件并不重要。 InnoDB 不记得确切的 WHERE 条件，而只知道扫描了哪些索引范围。 这些锁通常是下一个键锁，它们也会阻止插入到紧邻记录之前的“间隙”中。 但是，可以显式禁用`Gap Lock`，这会导致不使用`Next-key Lock`。

如果搜索时使用二级索引，并且要设置的索引记录锁是排它的，InnoDB 也会检索相应的聚集索引记录并对其设置锁。

如果没有适合您的语句的索引，并且 MySQL 必须扫描整个表来处理该语句，则表的每一行都会被锁定，从而阻止其他用户对该表的所有插入。 创建良好的索引非常重要，这样您的查询就不会扫描不必要的行。

在 InnoDB 中，不同的 SQL 语句会设置不同的锁定机制：

1. `SELECT ... FROM`：这是一种一致性读取，它读取数据库的快照并且不设置锁，除非事务隔离级别设置为 SERIALIZABLE。对于 SERIALIZABLE 级别，搜索会在遇到的索引记录上设置共享的 next-key 锁。
2. `SELECT ... FOR UPDATE` 和 `SELECT ... FOR SHARE`：这些语句会为扫描到的行加锁，并释放不符合结果集包含条件的行的锁。然而，在某些情况下，行可能不会立即解锁，因为在查询执行过程中，结果行与其原始来源之间的关系可能会丢失。
3. 对于锁定读取（带有 FOR UPDATE 或 FOR SHARE 的 SELECT）、UPDATE 和 DELETE 语句，所采取的锁取决于语句是否使用具有唯一搜索条件的唯一索引或范围类型搜索条件。
4. 对于具有唯一搜索条件的唯一索引，InnoDB 仅锁定找到的索引记录，而不是它之前的间隙。
5. 对于其他搜索条件和非唯一索引，InnoDB 使用间隙锁或 next-key 锁锁定扫描的索引范围，以阻止其他会话插入范围覆盖的间隙。
6. `UPDATE ... WHERE ...`：在搜索遇到的每个记录上设置独占的 next-key 锁。然而，对于使用唯一索引搜索唯一行的语句，只需要索引记录锁。
7. 当 UPDATE 修改聚簇索引记录时，会在受影响的二级索引记录上隐式地加锁。在插入新的二级索引记录之前执行重复检查扫描时，以及在插入新的二级索引记录时，UPDATE 操作还会在受影响的二级索引记录上加共享锁。
8. "DELETE FROM ... WHERE ..."在搜索遇到的每个记录上设置独占的 next-key 锁。然而，对于使用唯一索引搜索唯一行的语句，只需要索引记录锁。
9. INSERT 在插入的行上设置独占锁。这个锁是索引记录锁，而不是 next-key 锁（也就是说，没有间隙锁），并且不阻止其他会话在插入的行之前插入到间隙中。

   在插入行之前，会设置一种称为插入意图间隙锁的间隙锁。这种锁表示插入的意图，以便多个事务在同一索引间隙中插入时，如果它们不在间隙内的同一位置插入，就不需要等待彼此。假设有索引记录，其值为 4 和 7。分别尝试插入值 5 和 6 的单独事务在获取插入行的独占锁之前，都会用插入意图锁锁定 4 和 7 之间的间隙，但是不会阻塞彼此，因为行是非冲突的。

10. "INSERT ... ON DUPLICATE KEY UPDATE"与简单的"INSERT"不同，因为当出现重复键错误时，会在要更新的行上放置独占锁，而不是共享锁。对于重复的主键值，会取得独占的索引记录锁。对于重复的唯一键值，会取得独占的 next-key 锁。
11. 如果在唯一键上没有冲突，那么"REPLACE"就像"INSERT"一样进行。否则，会在要替换的行上放置独占的 next-key 锁。
12. "INSERT INTO T SELECT ... FROM S WHERE ..."在每个插入到 T 的行上设置独占的索引记录锁（没有间隙锁）。如果事务隔离级别是 READ COMMITTED，InnoDB 将 S 上的搜索作为一致性读取（无锁）。否则，InnoDB 在 S 的行上设置共享的 next-key 锁。在后一种情况下，InnoDB 必须设置锁：在使用基于语句的二进制日志进行向前滚动恢复时，每个 SQL 语句必须以与原来完全相同的方式执行。

    "CREATE TABLE ... SELECT ..."以共享的 next-key 锁或作为一致性读取执行 SELECT，就像"INSERT ... SELECT"一样。

    当 SELECT 用于"REPLACE INTO t SELECT ... FROM s WHERE ..."或"UPDATE t ... WHERE col IN (SELECT ... FROM s ...)"的构造中时，InnoDB 在 s 表的行上设置共享的 next-key 锁。

13. 在初始化表上之前指定的 AUTO_INCREMENT 列时，InnoDB 在与 AUTO_INCREMENT 列关联的索引的末尾设置独占锁。

# 理解体会

总的来说，InnoDB 的锁定机制取决于 SQL 语句的类型和使用的索引类型，以确保数据的一致性和事务的隔离性。
