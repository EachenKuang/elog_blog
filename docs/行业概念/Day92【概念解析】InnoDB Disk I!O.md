---
password: ""
icon: ""
date: "2023-12-22"
type: Post
category: 行业概念
slug: industry-day92
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day92【概念解析】InnoDB Disk I/O
status: Published
urlname: 5519ed7b-ce9d-4e4d-acd3-f5039d58c34d
updated: "2023-12-22 12:05:00"
---

> 😄 本章主要讲讲 InnoDB 的磁盘 IO 行为，重点从 **Read-Ahead** 与 **Doublewrite Buffer** 两方面展开。之前在 Buffer Pool 与 Doublewrite Buffer 这两章有着重介绍过，本章不再赘述。

# 整理定义

InnoDB 在可能的情况下使用异步磁盘 I/O，通过创建多个线程来处理 I/O 操作，同时允许其他数据库操作在 I/O 仍在进行中时继续进行。在 Linux 和 Windows 平台上，InnoDB 使用可用的操作系统和库函数来执行“原生”的异步 I/O。在其他平台上，InnoDB 仍然使用 I/O 线程，但这些线程可能实际上会等待 I/O 请求完成；这种技术被称为“模拟”的异步 I/O。

# 复述展开

## 预读（Read-Ahead）

如果 InnoDB 能够确定有高概率需要某些数据，它会执行预读操作，将这些数据带入缓冲池，以便数据在内存中可用。对连续数据进行少数几次大的读取请求比进行多次小的、分散的请求更有效率。InnoDB 中有两种预读启发式方法：

在顺序预读中，如果 InnoDB 注意到对表空间中的段的访问模式是顺序的，它会提前批量发布数据库页面的读取请求到 I/O 系统。

在随机预读中，如果 InnoDB 注意到表空间中的某个区域似乎正在被完全读入缓冲池，它会将剩余的读取请求发布到 I/O 系统。

[bookmark](https://kuangyichen.com/article/industry-day59)

## 双写缓冲区（Doublewrite Buffer）

InnoDB 使用一种称为双写缓冲区的新颖文件刷新技术，默认情况下在大多数情况下是启用的（innodb_doublewrite=ON）。它为意外退出或断电后的恢复增加了安全性，并且通过减少对 fsync()操作的需求，在大多数 Unix 变体上提高了性能。

在将页面写入数据文件之前，InnoDB 首先将它们写入称为双写缓冲区的存储区域。只有在写入和刷新到双写缓冲区完成后，InnoDB 才将页面写入数据文件的适当位置。如果在页面写入过程中出现操作系统、存储子系统故障或意外的 mysqld 进程退出（导致“撕裂页面”情况），InnoDB 可以在恢复期间从双写缓冲区中找到页面的良好副本。

[bookmark](https://kuangyichen.com/article/industry-day65)

# 理解体会

**总结：**

- InnoDB 在可能的情况下使用异步磁盘 I/O，以提高效率。
- 在 Linux 和 Windows 上，InnoDB 执行原生异步 I/O；在其他平台上，执行模拟异步 I/O。
- InnoDB 有两种预读机制来提高数据访问效率：顺序预读和随机预读。
  - **顺序预读**：当检测到对表空间段的顺序访问时，InnoDB 会提前批量请求读取数据库页面。
  - **随机预读**：当检测到表空间的某个区域正在被读入缓冲池时，InnoDB 会请求读取剩余的页面。
- InnoDB 使用双写缓冲区来提高数据的安全性和提升性能。
  - 在数据写入数据文件之前，先写入双写缓冲区。
  - 这样做可以在系统崩溃或断电后，通过双写缓冲区恢复完好的页面数据。
- 双写缓冲区默认启用，并且在大多数 Unix 系统上减少了对 fsync()操作的需求，从而提高了性能。

了解和配置这些特性可以帮助数据库管理员优化 InnoDB 的性能，并确保数据的完整性和可靠性。
