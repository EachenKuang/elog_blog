---
password: ""
icon: ""
date: "2023-12-02"
type: Post
category: 行业概念
slug: industry-day72
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day72【概念解析】InnoDB Undo Tablespace
status: Published
urlname: 36f26696-ded6-49ae-89a5-c4882c94987e
updated: "2023-12-05 11:53:00"
---

# 整理定义

中文名称：InnoDB 撤销表空间

英文名称：InnoDB Undo Tablespace

![MySQL 5.7版本](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

# 复述展开

### 什么是 Undo Tablespace？

Undo Tablespace 是 InnoDB 存储引擎用来存储 undo 日志的地方。Undo 日志记录了事务发生之前的数据状态，如果一个事务需要回滚，InnoDB 就会使用 undo 日志来恢复数据到事务开始前的状态。此外，undo 日志也用于 MVCC 中，以提供事务的隔离级别保证，允许读取事务开始前的数据版本。

### Undo 日志的作用

Undo 日志主要有两个作用：

1. **事务回滚：** 当事务执行失败或者被显式地回滚时，undo 日志可以用来撤销已经执行的操作，确保数据的一致性。
2. **多版本并发控制（MVCC）：** 在读取旧数据版本时，即使数据已经被其他事务修改，undo 日志也能提供一个数据的快照，这样就可以实现非锁定读取。

### Undo Tablespace 的结构

Undo Tablespace 由多个 undo 日志段组成，每个段可以包含多个 undo 日志。当 InnoDB 初始化时，会创建两个 undo 表空间，但是用户也可以根据需要配置更多的 undo 表空间。

### 管理 Undo Tablespace

在 MySQL 5.7 及以后的版本中，Undo Tablespace 可以被配置为独立的表空间，这样做的好处是可以更好地管理磁盘空间，提高性能。管理员可以根据数据库的工作负载来增加或减少 undo 表空间的数量。

### 如何学习 Undo Tablespace？

1. **官方文档：** 阅读 MySQL 官方文档中关于 InnoDB Undo Tablespace 的章节，这是获取最权威、最详细信息的途径。
2. **在线教程和课程：** 有许多在线资源提供了关于 InnoDB 和 Undo Tablespace 的教程和课程，这些可以帮助你从基础到高级的知识。
3. **实践操作：** 在自己的 MySQL 环境中实际操作，比如创建新的

# 理解体会

- MySQL 在初始化时会创建两个默认的 undo 表空间，用于存储必要的回滚段，以便可以处理 SQL 语句。
- 这些表空间通常位于由`innodb_undo_directory`变量指定的目录中，如果未指定，则在数据目录中创建。
- 默认的 undo 表空间文件被命名为`undo_001`和`undo_002`。
- 从 MySQL 8.0.14 版本开始，可以在运行时动态地添加更多的 undo 表空间。
- undo 表空间的初始大小在 MySQL 8.0.23 之前依赖于页面大小，而从 8.0.23 版本开始，通常固定为 16MiB，但在某些情况下可能会有所不同

# 参考文档：

[MySQL :: MySQL 8.0 Reference Manual :: 15.6.3.4 Undo Tablespaces](https://dev.mysql.com/doc/refman/8.0/en/innodb-undo-tablespaces.html#innodb-undo-tablespace-rollback-segments)
