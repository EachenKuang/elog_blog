---
password: ""
icon: ""
date: "2023-10-01"
type: Post
category: 行业概念
slug: industry-day10
tags:
  - 行业概念
  - 文字
  - 思考
summary: ""
title: Day10【概念解析】数据结构
status: Published
cover: "https://www.notion.so/images/page-cover/nasa_new_york_city_grid.jpg"
urlname: bbad1c5d-2fe9-431e-92c3-5e93decd25e0
updated: "2023-10-27 19:22:00"
---

# 整理定义

| 出处                                                                                                                                                                                                                                                                                                                                                                                          | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 菜鸟教程                                                                                                                                                                                                                                                                                                                                                                                      | 数据结构（英语：data structure）是**计算机中存储、组织数据的方式**。 数据结构是一种具有一定逻辑关系，在计算机中应用某种存储结构，并且封装了相应操作的数据元素集合。 它包含三方面的内容，逻辑关系、存储关系及操作。 不同种类的数据结构适合于不同种类的应用，而部分甚至专门用于特定的作业任务。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 百度百科                                                                                                                                                                                                                                                                                                                                                                                      | 数据结构是计算机存储、组织数据的方式。数据结构是指相互之间存在一种或多种特定关系的[数据元素](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%85%83%E7%B4%A0/715313?fromModule=lemma_inlink)的集合。通常情况下，精心选择的数据结构可以带来更高的运行或者存储效率。数据结构往往同高效的检索算法和索引技术有关。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 维基百科[En]                                                                                                                                                                                                                                                                                                                                                                                  | In [computer science](https://en.wikipedia.org/wiki/Computer_science), a **data structure** is a [data](https://en.wikipedia.org/wiki/Data) organization, management, and storage format that is usually chosen for [efficient](https://en.wikipedia.org/wiki/Efficiency) [access](https://en.wikipedia.org/wiki/Data_access) to data.[[1]](https://en.wikipedia.org/wiki/Data_structure#cite_note-1)[[2]](https://en.wikipedia.org/wiki/Data_structure#cite_note-2)[[3]](https://en.wikipedia.org/wiki/Data_structure#cite_note-3) More precisely, a data structure is a collection of data values, the relationships among them, and the functions or operations that can be applied to the data,[[4]](https://en.wikipedia.org/wiki/Data_structure#cite_note-4) i.e., it is an [algebraic structure](https://en.wikipedia.org/wiki/Algebraic_structure) about [data](https://en.wikipedia.org/wiki/Data). |
| 【在计算机科学中，数据结构是一种数据组织、管理和存储格式，通常被选择用于有效访问数据。更准确地说，数据结构是数据值、它们之间的关系以及可以应用于数据的函数或操作的集合，即，它是关于数据的代数结构。】                                                                                                                                                                                        |
| 维基百科[Zn]                                                                                                                                                                                                                                                                                                                                                                                  | 在[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)中，**数据结构**（英语：data structure）是计算机中存储、组织[数据](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE)的方式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 数据结构意味着[介面](<https://zh.wikipedia.org/wiki/%E4%BB%8B%E9%9D%A2_(%E9%9B%BB%E8%85%A6%E7%A7%91%E5%AD%B8)>)或[封装](<https://zh.wikipedia.org/wiki/%E5%B0%81%E8%A3%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)>)：一个数据结构可被视为两个函数之间的介面，或者是由[数据类型](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)联合组成的存储内容的访问方法封装。 |

# 复述展开

## 拆文解字

**数据结构=数据+结构**

前面有提到，**软件=程序+数据+文档**，数据结构作为软件中必不可少的内容，是一种在计算机中存储、组织数据的方式。通常，数据机构与算法息息相关，一些算法需要依据合适的数据结构来完成优化，数据结构也是算法的前导需求。

## 数据结构类型

数据结构主要可以分为四种类型：线性结构、树结构、图结构和集合。

1. **线性结构**：元素存在一对一的关系。主要包括以下几种：
   - **数组**：数组是一种线性数据结构，用一组连续的内存空间，来存储一组具有相同类型的数据。数组的特点是支持随机访问，但插入和删除操作效率低。
   - **链表**：链表是一种线性数据结构，由一系列节点组成，每个节点包含数据和指向下一个节点的指针。链表的特点是插入和删除操作效率高，但不支持随机访问。
   - **栈**：栈是一种特殊的线性数据结构，只允许在一端（称为栈顶）进行插入和删除操作。栈遵循后进先出（LIFO）的原则。
   - **队列**：队列是一种特殊的线性数据结构，只允许在一端进行插入操作，另一端进行删除操作。队列遵循先进先出（FIFO）的原则。
2. **树结构**：元素存在一对多的关系。主要包括以下几种：
   - **二叉树**：每个节点最多有两个子节点的树结构，通常子节点被称作左子节点和右子节点。
   - **二叉搜索树**：二叉搜索树是一种特殊的二叉树，对于任何一个节点，其左子树中的每个节点的值，都要小于这个节点的值，而右子树节点的值都大于这个节点的值。
   - **堆**：堆是一种特殊的完全二叉树，分为最大堆和最小堆。在最大堆中，父节点的值总是大于或等于其子节点的值；在最小堆中，父节点的值总是小于或等于其子节点的值。
   - **B 树和 B+树**：B 树和 B+树是为磁盘或其他直接存取辅助设备设计的一种平衡查找树。
3. **图结构**：元素存在多对多的关系。图由一组节点和一组连接节点的边组成。根据边是否有方向，图可以分为无向图和有向图。根据边是否有权，图可以分为无权图和有权图。
4. **集合**：集合是一种包含不同元素的数据结构。集合的一个重要特性是唯一性，集合中的元素没有重复。集合可以进行的操作有并集、交集、差集等。

# 理解体会

数据结构是软件开发中的基本知识，算法也是其中的常客，这两者相辅相成、缺一不可。

在学习软件的过程中，数据结构是非常重要的的，不论在哪种编程语言中，数据结构都是重中之重，如线性结构中的列表和链表；以及非线性结构中的树和图。掌握好数据机构是学习好编程语言的毕竟之路，而在掌握基本的数据结构之后，还需要了解各方面的算法，像排序算法就多达十余种。

【个人经验之谈】

在 IT 行业中，往往面试都与其他的行业有些不一样。在面试过程中一般都是先来几道算法题（类似 LeetCode 那种模式），使用任意一种编程语言解决一个实际问题，需要满足一定的时间复杂度和空间复杂度，在有限的时间内完成做题。一般情况下，需要至少完成 3 道中的 2 道以上，才可能顺利通过面试。可以看出，数据结构和算法在 IT 行业都是非常重要的面试手段。所以，学习好数据结构，并且在各种编程语言中掌握使用是顺利通过面试的必经之路。（面试的经验啥的可以写很多，这里就不赘述了）。

[bookmark](https://leetcode.com/)

[bookmark](https://leetcode.cn/)

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
