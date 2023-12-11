---
password: ""
icon: ""
date: "2023-12-11"
type: Post
category: 行业概念
slug: industry-day81
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: "Day81【概念解析】Autocommit, Commit, and Rollback"
status: Published
urlname: 02fb2da3-a36a-4f29-bb0e-02203e544821
updated: "2023-12-11 12:09:00"
---

# 整理定义

**autocommit（自动提交）**

`autocommit`模式是数据库系统的一个设置，当这个设置被启用时，每个 SQL 语句都会被视为一个独立的事务，并且在执行后立即被提交。这意味着，如果你执行了一个`INSERT`、`UPDATE`或`DELETE`语句，该操作会立即生效，数据被永久写入数据库，无需显式调用`COMMIT`命令。

在`autocommit`模式关闭的情况下，你需要显式地开始一个事务，并用`COMMIT`或`ROLLBACK`语句结束它。这允许你执行多个操作作为一个单一的事务单元，要么全部成功，要么在遇到错误时全部回滚。

**Commit（提交）**

`COMMIT`命令用于结束一个事务，并将自事务开始以来进行的所有修改永久保存到数据库中。当你执行`COMMIT`时，所有自上一个`COMMIT`或`ROLLBACK`以来的更改都会成为数据库的一部分。如果数据库系统崩溃或发生故障，已经提交的事务保证不会丢失，这是由数据库的持久性特性所保证的。

**Rollback（回滚）**

`ROLLBACK`命令与`COMMIT`相反，它用于撤销一个事务中的所有操作。如果在执行一系列操作时遇到错误，或者你决定出于某种原因不希望更改生效，可以使用`ROLLBACK`命令。执行`ROLLBACK`后，自上一个`COMMIT`或`ROLLBACK`以来的所有更改都将被撤销，数据库状态回到事务开始时的状态。

# 复述展开

1. **InnoDB 事务模型**：在 InnoDB 存储引擎中，所有用户活动都是在事务中进行的。如果启用了`autocommit`模式，每个 SQL 语句都会自动成为一个**单独**的事务。
2. **autocommit 模式**：默认情况下，MySQL 在每个新连接的会话中启用`autocommit`。这意味着，**如果 SQL 语句执行成功，MySQL 会在每个语句之后自动进行提交（COMMIT）**。如果语句执行出错，是否提交或回滚取决于具体的错误类型。
3. **显式事务**：即使在`autocommit`模式启用的情况下，也可以通过显式地使用`START TRANSACTION`或`BEGIN`开始一个事务，并使用`COMMIT`或`ROLLBACK`来结束它，从而执行多条语句的事务。
4. **禁用 autocommit 模式**：在会话中通过`SET autocommit = 0`禁用`autocommit`模式后，该会话将始终保持一个开放的事务。使用`COMMIT`或`ROLLBACK`可以结束当前事务，并自动开始一个新的事务。
5. **会话结束时的事务处理**：如果`autocommit`模式被禁用，并且会话在没有显式提交最后一个事务的情况下结束，MySQL 将回滚该事务。
6. **隐式提交**：某些语句执行时会隐式地结束当前事务，就好像在执行该语句之前进行了`COMMIT`一样。
7. **COMMIT 与 ROLLBACK**：`COMMIT`操作会使当前事务中的所有更改变为永久性，并且这些更改对其他会话可见。而`ROLLBACK`操作则取消当前事务中的所有修改。无论是`COMMIT`还是`ROLLBACK`，都会释放当前事务中设置的所有 InnoDB 锁。

## 语句模板

```sql
START TRANSACTION
    [transaction_characteristic [, transaction_characteristic] ...]

transaction_characteristic: {
    WITH CONSISTENT SNAPSHOT
  | READ WRITE
  | READ ONLY
}

BEGIN [WORK]
COMMIT [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
ROLLBACK [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
SET autocommit = {0 | 1}
```

这些语句提供对事务使用的控制：

- START TRANSACTION 或 BEGIN 启动新事务。
- COMMIT 提交当前事务，使其更改永久化。
- ROLLBACK 回滚当前事务，取消其更改。
- SET autocommit 禁用或启用当前会话的默认自动提交模式。

默认情况下，MySQL 运行时启用自动提交模式。 这意味着，当不在事务内部时，每个语句都是原子的，就好像它被 START TRANSACTION 和 COMMIT 包围一样。 您不能使用 ROLLBACK 来撤消效果； 但是，如果在语句执行期间发生错误，则该语句将回滚。

# 理解体会

通过对上述的学习，可以体会到了事务对于数据库操作的重要性，它们是保证数据完整性和一致性的关键。同时，了解`autocommit`模式的工作原理对于编写高效且安全的数据库交互代码至关重要。正确地管理事务可以避免数据丢失和脏读，也可以在出现错误时恢复到一致的状态。

# 参考

[MySQL :: MySQL 8.0 Reference Manual :: 15.7.2.2 autocommit, Commit, and Rollback](https://dev.mysql.com/doc/refman/8.0/en/innodb-autocommit-commit-rollback.html)

[MySQL :: MySQL 8.0 Reference Manual :: 13.3.1 START TRANSACTION, COMMIT, and ROLLBACK Statements](https://dev.mysql.com/doc/refman/8.0/en/commit.html)
