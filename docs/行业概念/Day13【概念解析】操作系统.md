---
password: ""
icon: ""
date: "2023-10-04"
type: Post
category: 行业概念
slug: industry-day13
tags:
  - 行业概念
  - 文字
  - 思考
summary: ""
title: Day13【概念解析】操作系统
status: Published
cover: "https://www.notion.so/images/page-cover/met_john_singer_sargent_morocco.jpg"
urlname: 5ff18cd3-f26b-47a8-b675-2c057c3bf1f9
updated: "2023-10-27 19:22:00"
---

# 整理定义

中文名称：操作系统

外文名：Operating System

简称：OS

| 来源                                                                                                          | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [百度百科](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/192?fromModule=lemma_search-box) | **操作系统***（英语：Operating System，缩写：OS）*是一组主管并控制计算机操作、运用和运行硬件、软件资源和提供公共服务来组织用户交互的相互关联的系统软件程序。根据运行的环境，操作系统可以分为桌面操作系统，手机操作系统，服务器操作系统，嵌入式操作系统等。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| [维基百科（En）](https://en.wikipedia.org/wiki/Operating_system)                                              | An **operating system** (**OS**) is [system software](https://en.wikipedia.org/wiki/System_software) that manages [computer hardware](https://en.wikipedia.org/wiki/Computer_hardware) and [software](https://en.wikipedia.org/wiki/Software) resources, and provides common [services](<https://en.wikipedia.org/wiki/Daemon_(computing)>) for [computer programs](https://en.wikipedia.org/wiki/Computer_program).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 【**操作系统**（OS）是管理计算机硬件和软件资源并为计算机程序提供通用服务的系统软件。】                        |
| [维基百科（Zn）](https://zh.wikipedia.org/wiki/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)                          | **操作系统**（英语：**O**perating **S**ystem，缩写：**OS**）是一组主管并控制[计算机](https://zh.wikipedia.org/wiki/%E7%94%B5%E5%AD%90%E8%AE%A1%E7%AE%97%E6%9C%BA)操作、运用和运行[硬件](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A1%AC%E4%BB%B6)、[软件](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6)[资源](<https://zh.wikipedia.org/wiki/%E8%B3%87%E6%BA%90_(%E8%A8%88%E7%AE%97%E6%A9%9F%E7%A7%91%E5%AD%B8)>)和提供公共[服务](https://zh.wikipedia.org/wiki/%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B)来组织用户交互的相互关联的[系统软件](https://zh.wikipedia.org/wiki/%E7%B3%BB%E7%BB%9F%E8%BD%AF%E4%BB%B6)[程序](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BA%8F)，同时也是计算机系统的内核与基石。操作系统需要处理如管理与配置[内存](https://zh.wikipedia.org/wiki/%E5%86%85%E5%AD%98)、决定系统资源供需的优先次序、控制输入与输出设备、操作[网络](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C)与管理[文件系统](https://zh.wikipedia.org/wiki/%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)等基本事务。操作系统也提供一个让用户与系统交互的操作界面。 |
| 《软件设计师》（第五版）                                                                                      | **操作系统**：能有效地组织和管理系统中的各种软/硬件资源，合理地组织计算机系统工作流程，控制程序的执行，并且向用户提供一个良好的工作环境和友好的接口。操作系统是计算机系统的资源管理者，它含有对系统软/硬件资源实施管理的一组程序。其 首要作用就是通过 CPU 管理、存储管理、设备管理和文件管理对各种资源进行合理的分配，改善资源的共享和利用程度，最大限度地发挥计算机系统的 工作效率，提高计算机系统在单位时 间内处理工作的能力。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

# 复述展开

**操作系统的作用**：

- 第一，通过资源管理提高计算机系统的效率;
- 第二，改善人机界面向用户提供友好的工作环境。

**操作系统的特征与功能**：

> 📌 4 个特征：并发性、共享性、虚拟性和不确定性  
> 5 个功能：进程管理、文件管理、存储管理、设备管理和作业管理
>
> ——《软件设计师》（第五版）

    5个功能：进程管理、文件管理、存储管理、设备管理和作业管理


    ——《软件设计师》（第五版）

**操作系统的分类：**

> 📌 操作系统可以分为批处理操作系统、分时操作系统、实时操作系统、网络操作系统、 分布式操作系统、微型计算机操作系统和嵌入式操作系统等类型  
> ——《软件设计师》（第五版）

> 📌 计算机的操作系统根据不同的用途分为不同的种类，从功能角度分析，分别有实时系统、批处理系统、分时系统、网络操作系统等。  
> —— 百度百科

比较常见的操作系统有：

1. **Microsoft Windows**：最广泛使用的桌面操作系统，包括多个版本，如 Windows 10, Windows 8, Windows 7 等。
2. **MacOS**：苹果公司为其 Mac 电脑设计的操作系统。
3. **Linux**：一个开源的 Unix-like 操作系统。有许多不同的版本，包括 Ubuntu, Fedora, Debian 等。
4. **Android**：谷歌开发的，主要用于移动设备如智能手机和平板电脑的操作系统。
5. **iOS**：苹果公司为 iPhone 和 iPad 设计的移动操作系统。
6. **Unix**：一个强大的多用户、多任务操作系统，广泛用于服务器、工作站和大型计算机。
7. **FreeBSD**：一个开源的 Unix-like 操作系统，广泛用于服务器。
8. **Chrome OS**：谷歌开发的，主要用于 Chromebook 笔记本电脑的操作系统。
9. **Windows Server**：微软为服务器环境设计的操作系统，包括 Windows Server 2019, Windows Server 2016 等。
10. **Red Hat Enterprise Linux (RHEL)**：红帽公司开发的商业 Linux 发行版，主要用于企业服务器。

以上是一些常见的操作系统，每个操作系统都有其特定的用途和优势。

下面的链接是不同的操作性之间的比较：

[bookmark](https://en.wikipedia.org/wiki/Comparison_of_operating_systems)

# 理解体会

**【学好操作系统非常重要！！！】**

操作系统这个概念在计算机软件领域是一个非常重要的概念，同时也是软件工程、计算机科学与技术专业的必修课程，所以掌握操作系统对于计算机行业来说非常关键。

操作系统是计算机系统的核心，它管理和控制计算机础础硬件和软件资源，同时也为用户和其他软件提供各种服务。因此，对于计算机行业的专业人员来说，理解和掌握操作系统的知识是非常重要的。

首先，操作系统是计算机科学的基础。它涵盖了许多计算机科学的基本概念，如进程管理、内存管理、文件系统、I/O 设备管理、并发控制、安全性和网络等。通过学习操作系统，我们可以深入理解这些基本概念，从而更好地理解计算机系统的工作原理。

其次，操作系统是软件开发的基础。无论是开发桌面应用、移动应用，还是云应用，都需要在某种操作系统上运行。对操作系统的理解可以帮助开发者更有效地编写程序，利用操作系统提供的服务和资源，提高程序的性能和可靠性。

此外，操作系统也是计算机安全的基础。许多安全问题，如病毒、恶意软件、权限越界等，都与操作系统有关。通过学习操作系统，我们可以理解这些安全问题的原理，从而更好地防范和应对这些问题。

对于计算机系统管理员和网络工程师来说，操作系统的知识也是必不可少的。他们需要管理和维护运行着各种操作系统的计算机和服务器，处理各种系统问题，如系统配置、性能优化、故障恢复等。

总的来说，操作系统是连接硬件和软件，连接计算机科学和软件工程，连接理论和实践的桥梁。无论是学习计算机科学，还是从事软件开发、系统管理、网络工程等工作，都需要掌握操作系统的知识。因此，学习操作系统对于计算机行业的重要性不言而喻。

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
