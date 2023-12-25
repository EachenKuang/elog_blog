---
password: ""
icon: ""
date: "2023-12-25"
type: Post
category: 行业概念
slug: industry-day95
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day95【概念解析】InnoDB Monitors
status: Published
urlname: cc986261-bc28-4f37-8493-883dd3bcf140
updated: "2023-12-25 12:25:00"
---

# 整理定义

**InnoDB Monitors（InnoDB 监视器）**

`InnoDB` monitors provide information about the `InnoDB` internal state. This information is useful for performance tuning.

InnoDB Monitors 是 MySQL 中的一组特殊工具，用于监控 InnoDB 存储引擎的内部工作状态。这些监控工具可以提供关于 InnoDB 性能和操作的详细信息，帮助数据库管理员诊断问题、优化性能以及更好地理解 InnoDB 的行为。

# 复述展开

## 分类

有两种类型的“InnoDB”监视器：

- The standard `InnoDB` Monitor （标准监视器）
- The `InnoDB` Lock Monitor（锁监视器）

其中

标准监视器显示以下类型的信息：

    - 主后台线程完成的工作
    - 信号量等待
    - 有关最新外键和死锁错误的数据
    - 锁定等待交易
    - 活动事务持有的表和记录锁
    - 待处理的 I/O 操作和相关统计数据
    - 插入缓冲区和自适应哈希索引统计
    - 重做日志数据
    - 缓冲池统计
    - 行操作数据

锁定监视器打印附加锁定信息作为标准“InnoDB”监视器输出的一部分。

> [MySQL :: MySQL 8.0 Reference Manual :: 15.17.1 InnoDB Monitor Types](https://dev.mysql.com/doc/refman/8.0/en/innodb-monitor-types.html)

    There are two types of `InnoDB` monitor:

    - The standard `InnoDB` Monitor displays the following types of information:
    	- Work done by the main background thread
    	- Semaphore waits
    	- Data about the most recent foreign key and deadlock errors
    	- Lock waits for transactions
    	- Table and record locks held by active transactions
    	- Pending I/O operations and related statistics
    	- Insert buffer and adaptive hash index statistics
    	- Redo log data
    	- Buffer pool statistics
    	- Row operation data
    - The `InnoDB` Lock Monitor prints additional lock information as part of the standard `InnoDB` Monitor output.

# 理解体会

启用这些监控器通常涉及到在 MySQL 服务器上执行特定的 SQL 命令或设置系统变量。一些监控器，如标准监控器，可以临时启用以获取即时的状态报告；而其他监控器可能需要将输出重定向到一个文件中，以便进行持续监控和后续分析。

需要注意的是，虽然 InnoDB 监控器是强大的诊断工具，但它们也可能对系统性能产生影响，特别是在高负载的生产环境中。因此，建议仅在需要时启用它们，并在分析完毕后关闭。此外，监控器输出的信息可能非常庞大和复杂，需要一定的专业知识来正确解读和利用这些数据。

总的来说，InnoDB 监控器是数据库管理员和开发人员的宝贵资源，可以帮助他们深入了解数据库的内部运作，并在必要时进行优化和故障排除。
