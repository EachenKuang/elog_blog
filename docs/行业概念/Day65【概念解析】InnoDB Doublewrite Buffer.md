---
password: ""
icon: ""
date: "2023-11-25"
type: Post
category: 行业概念
slug: industry-day65
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day65【概念解析】InnoDB Doublewrite Buffer
status: Published
urlname: 5f02c699-29b2-4c1a-8464-a802364e6024
updated: "2023-11-27 02:41:00"
---

# 整理定义

中文名称：双写缓冲区

英文名称：Doublewrite Buffer

![MySQL 5.7版本](https://image.kuangyichen.com/image/innodb-architecture-5-7.png)

![MySQL 8.0版本](https://image.kuangyichen.com/image/innodb-architecture-8-0.png)

**双写缓冲区（Doublewrite Buffer）是**一个存储区域，InnoDB 在将页面写入 InnoDB 数据文件中的正确位置之前先将页面写入从缓冲池刷新的位置。 如果在页面写入过程中存在操作系统、存储子系统或意外的 mysqld 进程退出，InnoDB 可以在崩溃恢复期间从双写缓冲区中找到页面的良好副本。

在 `MySQL 8.0.20`之前，双写缓冲存储区域位于 InnoDB 系统表空间中。 从`MySQL 8.0.20`开始，双写缓冲区存储区域位于**双写缓冲文件（Doublewrite Buffer Files）**中。

## doublewrite buffer 配置提供以下参数：

### `innodb_doublewrite`

- `innodb_doublewrite`参数控制是否启用 doublewrite buffer。多数场景下默认是启用的。为了禁用 doublewrite buffer，设置`innodb_doublewrite=0`或者启动 MySQL 服务时加`-skip-innodb-doublewrite`选项。例如在性能压测的场景下，如果您更关注性能而不是数据可靠性，您可以禁用 doublewrite buffer。
  如果 doublewrite buffer 位于支持原子写的 Fusion-io 设备上，则自动禁用 doublewrite buffer，并使用 Fusion-io 原子写来执行数据文件写。但是，要注意`innodb_doublewrite`设置是全局的。当 doublewrite buffer 被禁用时，它对不在 Fusion-io 硬件上的数据文件也禁用。该功能仅在 Fusion-io 硬件上支持，在 Linux 中仅支持 Fusion-io NVMFS。为了充分利用这个功能，建议将`innodb_flush_method=O_DIRECT`

### `innodb_doublewrite_dir`

- `innodb_doublewrite_dir`（8.0.20 引入）定义了 InnoDB 创建双写文件的目录。如果目录没有指定，双写文件创建在`innodb_data_home_dir`目录下，没有指定默认在数据目录下。
  哈希符'#'会自动创建在指定目录名前缀，避免与 shema 名冲突。然而，如果使用了'.', '#'. 或者'/'指定了目录前缀，则不在目录名前缀没有哈希符'#'。
  理想情况下，双写目录应该放在最快的存储上。

### `innodb_doublewrite_files`

- `innodb_doublewrite_files`参数定义了双写文件的数量。默认情况下，每个缓冲池实例都会创建 2 个双写文件：一个刷新列表双写文件和一个 LRU 列表双写文件。
  刷新列表双写文件用于从缓冲池刷新列表中刷新页。刷新列表双写文件默认大小是`InnoDB page size * doublewrite page bytes`.
  LRU 列表双写文件是用于刷新从缓冲池 LRU 列表的页。它也包括单个页刷新的槽。LRU 列表双写文件默认大小为`InnoDB page size * (doublewrite pages + (512 / the number of buffer pool instances))`，512 是为单个页刷新保留的槽的总数。
  至少有 2 个双写文件。双写文件的最大数量是缓冲池实例的两倍。（缓冲池实例的数量由参数`innodb_buffer_pool_instances`控制）
  双写文件有以下格式：`#ib_page_size_file_number.dblwr`。例如，下面的双写文件是在一个 InnoDB 页大小为 16KB，单个缓冲池的 MySQL 实例上创建：
  #ib_16384_0.dblwr #ib_16384_1.dblwr
  `innodb_doublewrite_files`参数用于高级性能调优。默认设定已经适用于大多数用户。

### `innodb_doublewrite_pages`

- `innodb_doublewrite_pages`参数（MySQL8.0.20 引入）控制每个线程双写页的最大数量。如果这个值没有指定，`innodb_doublewrite_pages`设置为`innodb_write_io_threads`值。这个参数用于高级性能调优。默认值已经适用于大多数用户。

### `innodb_doublewrite_batch_size`

- `innodb_doublewrite_batch_size`参数（MySQL8.0.20 引入）控制一批写入双写页的数量。这个参数用于高级性能调优。默认值已经适用于大多数用户。

# 复述展开

## 为什么需要双写缓冲区？

我们常见的服务器一般都是 Linux 操作系统，Linux 文件系统页（OS Page）的大小默认是 4KB。而 MySQL 的页（Page）大小默认是 16KB。

可以使用如下命令查看 MySQL 的 Page 大小：

```sql
SHOW VARIABLES LIKE 'innodb_page_size';
```

一般情况下，其余程序因为需要跟操作系统交互，它们的页（Page）都会大于等于操作系统的页大小，为整数倍。比如，Oracle 的 Page 大小为 8KB。

MySQL 程序是跑在 Linux 操作系统上的，需要跟操作系统交互，所以 MySQL 中一页数据刷到磁盘，要写 4 个文件系统里的页。

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/aae5c118-2852-4035-a0ba-5a519b909b7c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231127T024350Z&X-Amz-Expires=3600&X-Amz-Signature=93578c6737e0be217e7ce14afb94fb25dc8d6bd24dfc74a9931324f7c47ec6af&X-Amz-SignedHeaders=host&x-id=GetObject)

**需要注意的是，这个操作并非原子操作，比如操作系统写到第二个页的时候，Linux 机器断电了，这时候就会出现问题了。造成”页数据损坏“。并且这种”页数据损坏“靠 redo 日志是无法修复的**。

**重做日志中记录的是对页的物理操作，而不是页面的全量记录，而如果发生 partial page write（部分页写入）问题时，出现问题的是未修改过的数据，此时重做日志(Redo Log)无能为力。写 doublewrite buffer 成功了，这个问题就不用担心了**。

Doublewrite Buffer 的出现就是为了解决上面的这种情况，虽然名字带了 Buffer，但实际上 Doublewrite Buffer 是**内存+磁盘**的结构。

Doublewrite Buffer 是一种特殊文件 flush 技术，带给 InnoDB 存储引擎的是<u>数据页的可靠性</u>。它的作用是，在把页写到数据文件之前，InnoDB 先把它们写到一个叫 doublewrite buffer（双写缓冲区）的共享表空间内，在写 doublewrite buffer 完成后，InnoDB 才会把页写到数据文件的适当的位置。如果在写页的过程中发生意外崩溃，InnoDB 在稍后的恢复过程中在 doublewrite buffer 中找到完好的 page 副本用于恢复。

【[MySQL 双写缓冲区(Doublewrite Buffer)-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/1307701)】

# 理解体会

最后我们总结下双写缓冲器的一些关键点：

## **Doublewrite 解决了什么问题？**

为了解决**部分页面写入**问题(Partial Page Write)。

MySQL 写入修改时刷新整个页面(默认 16kb)，而不仅仅是刷新页面中已更改的记录。而系统的单次 io，一般是 512byte 为单位的，在断电，OS crash(操作系统崩溃)情况下可能会丢失数据。

## **Doublewrite 是指哪两次写入？**

1. 写 Doublewrite buffer，注意: Doublewrite buffer**是磁盘不是内存**。
2. 写入数据文件。

> 写入顺序：先写 doublewrite buffer,写成功后再写到数据文件。

## **Doublewrite buffer 存储区位于什么地方？**

- 在 MySQL 8.0.20 之前:位于 InnoDB 系统表空间中(ibdata 文件)
- 从 MySQL 8.0.20 开始:位于 doublewrite 文件中,文件由`innodb_doublewrite_dir`和`innodb_doublewrite_files`配置确定

> doublewrite 由两部分组成，一部分是内存中的 doublewritebuffer，大小为 2MB，另一部分是物理磁盘上共享表空间中连续的 128 个页，即 2 个区（extent），大小同样为 2MB。在对缓冲池的脏页进行刷新时，并不直接写磁盘，而是会通过 memcpy 函数将脏页先复制到内存中的 doublewritebuffer，之后通过 doublewritebuffer 再分两次，每次 1MB 顺序地写入共享表空间的物理磁盘上，然后马上调用 fsync 函数，同步磁盘，避免缓冲写带来的问题。在这个过程中，因为 doublewrite 页是连续的，因此这个过程是顺序写的，开销并不是很大。在完成 doublewrite 页的写入后，再将 doublewritebuffer 中的页写入各个表空间文件中，此时的写入则是离散的。

    引用自：姜承尧. MySQL技术内幕：InnoDB存储引擎(第2版) (数据库技术丛书) (Chinese Edition) (p. 106). 机械工业出版社. Kindle 版本.

## **innodb 什么时候将脏页写入 Doublewrite buffer 中?**

由以下几个参数决定：

- innodb_max_dirty_pages_pct_lwm:低位水平标记，达到该值将启动缓冲刷新,默认为 10（百分比，脏页/缓冲池）
- innodb_max_dirty_pages_pct: 脏页数量与缓冲池比例阈值，默认为 90
- 如果开启自适应刷新（Adaptive Flushing）InnoDB 根据重做日志生成的速度和当前的刷新率，使用自适应刷新算法来动态调整刷新率。

# 参考

[MySQL 双写缓冲区(Doublewrite Buffer)-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/1307701)

[MySQL :: MySQL 5.7 Reference Manual :: 14.6.5 Doublewrite Buffer](https://dev.mysql.com/doc/refman/5.7/en/innodb-doublewrite-buffer.html)

[MySQL :: MySQL 8.0 Reference Manual :: 15.6.4 Doublewrite Buffer](https://dev.mysql.com/doc/refman/8.0/en/innodb-doublewrite-buffer.html)
