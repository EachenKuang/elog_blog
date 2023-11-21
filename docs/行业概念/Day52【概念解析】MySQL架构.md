---
password: ""
icon: ""
date: "2023-11-12"
type: Post
category: 行业概念
slug: industry-day52
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
  - SQL
summary: ""
title: Day52【概念解析】MySQL架构
status: Published
cover: "https://image.kuangyichen.com/image/mysql-arch.webp"
urlname: 7fdb0733-a3f1-42dc-bc74-0a3421a3c5a3
updated: "2023-11-15 14:19:00"
---

# 前言

> 💡 这篇文章基于前述的【[Day51【概念解析】 MySQL | 易浅小站 (kuangyichen.com)](https://kuangyichen.com/article/industry-day51)】，着重介绍下 MySQL 的架构。  
> 本文概述了 MySQL 服务端的架构、各种存储引擎之间的主要区别，以及这些区别的重要性

    本文概述了MySQL服务端的架构、各种存储引擎之间的主要区别，以及这些区别的重要性

# MySQL 的逻辑架构

数据库产品的架构一般可以分为应用层、逻辑层、物理层，对于 MySQL，同样可以理解为如下的 3 个层次。

- **应用层**。负责和客户端、用户进行交互，需要和不同的客户端和中间服务器进行交互，建立连接，记住连接的状态，响应它们的请求，返回数据和控制信息（错误信息、状态码等）。
- **逻辑层**。负责具体的查询处理、事务管理、存储管理、恢复管理，以及其他的附加功能。查询处理器负责查询的解析、执行。当接收到客户端的查询时，数据库就会分配一个线程来处理它。先由查询处理器（优化器）生成执行计划，然后交由计划执行器来执行，执行器有时需要访问更底层的事务管理器、存储管理器来操作数据，事务管理器、存储管理器主要负责事务管理、并发控制、存储管理。这其中，将由事务管理器来确保“ACID”特性，通过锁管理器来控制并发，由日志管理器来确保数据持久化，存储管理器一般还包括一个缓冲管理器，由它来确定磁盘和内存缓冲之间的数据传输。
- **物理层**。实际物理磁盘（存储）上的数据库文件，比如，数据文件、日志文件等。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/5f4e54d5-301d-45c0-b23e-7ae932a7ac85/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120338Z&X-Amz-Expires=3600&X-Amz-Signature=972eeff9e8e7543834e8545a5c36aa4a28461ca040cc5a945527b604e0949c14&X-Amz-SignedHeaders=host&x-id=GetObject)

上图时官方文档的一个基础架构图。

1. 其中 Connectors 可以理解为各种客户端、应用服务；
2. Connection Pool 可以理解为应用层，负责连接、验证等功能；
3. Management Services & Utilities、SQL Interface、Parser、Optimizer、Caches & Buffers、Pluggable Storage Engines 可以理解为数据库的大脑——逻辑层
4. 最下方的 Files&Logs 可以理解为物理层。

# MySQL 版本

> 💡 MySQL 目前可分为 4 个版本：**MySQL 社区版**、**MySQL 标准版**、**MySQL 企业版**、**MySQL 集群版**。

## MySQL 社区版

可免费下载使用的开源版本，遵循 GPL 协议，包括如下的这些特性。

- 可插拔的存储引擎架构
- 多存储引擎支持 InnoDB、MyISAM、NDB（MySQL Cluster 即采用 NDB 存储引擎）、Memory、Merge、Archive、CSV 等
- 复制
- 分区
- 存储过程、触发器、视图
- 信息数据库（Information-Schema）
- MySQL 连接器
- MySQL 工作台（MySQL Workbench）

目前已经发布了 MySQL 5.0、MySQL 5.1、MySQL 5.5、MySQL 5.6、MySQL 5.7 一共 5 个 GA 版本。一般来说，后面的版本比前面的版本功能更强、扩展性更好。

## MySQL 标准版

和社区版差别不大，提供社区版所支持的各种特性。

## MySQL 企业版

MySQL 企业版提供 7×24 小时的技术支持服务，用户可直接联系 MySQL 专业支持工程师，获取关于 MySQL 应用程序开发、部署和管理的全方位支持。

MySQL 企业版提供了更全面的高级功能、管理工具和技术支持，例如：MySQL 企业级备份可为数据库提供在线“热”备份，从而降低数据丢失的风险。它支持完全、增量和部分备份，以及时间点恢复和备份压缩。

MySQL 线程池提供了一个高效的线程处理模型，旨在降低客户端连接和语句执行线程的管理开销。

MySQL 企业级安全性提供了一些立即可用的外部身份验证模块，可将 MySQL 轻松集成到现有的安全基础架构中。

其他特性还有 MySQL 企业级审计、MySQL 企业级监视器（MySQL Enterprise Monitor）和 MySQL 查询分析器（MySQL Query Analyzer）等。

MySQL 的一些新特性出现在了企业版中，但并没有出现在社区版，这导致很多人对于 MySQL 产生了疑虑，但 MySQL 的生态已经建立成熟，官方版本和其他分支也都在稳定地发展改进中，一般的中小公司选择社区版本即可。一些行业、领域要求更好的服务，更高的稳定性，或者有其他复杂的业务需求，对于它们企业版是一个很好的选择。

## MySQL 集群（MySQL Cluster）版

Oracle 收购 MySQL 之后，对 MySQL Cluster 做了大量改进，这也是 Oracle 力推的产品。集群版是一种分布式、无共享（share-nothing）的架构，也就是说把数据分布在各个节点的内存里。据官方宣称，集群版可比单机数据库提供更高的可用性，高达 99.999%。它还有一些好处，比如自动分片、动态添加节点、支持跨 IDC 复制、减少维护成本等。但这个产品比较复杂，国内也缺少精通 MySQL Cluster 的专家，如果一定要使用，建议做好充分的测试验证工作。

据说现在的 MySQL Cluster 版本已经允许存储部分数据到硬盘上，但由于主要数据需要存放在内存中，因此其部署成本会比较高。另外，随着 MySQL Cluster 节点的增多，节点之间通信、同步的代价也越来越大，所以其扩展性也是有限的。对于海量数据，MySQL Cluster 可能不是很好的方案，从理论上来讲，仅仅把热点数据加载到内存是更经济的做法。

# 理解体会

本文主要介绍了 MySQL 的架构以及 MySQL 的版本。

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

[MySQL DBA 修炼之道-陈晓勇-微信读书 (qq.com)](https://weread.qq.com/web/reader/85a329405d039885a68ca85kaab325601eaab3238922e53)
