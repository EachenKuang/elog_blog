---
password: ""
icon: ""
date: "2023-10-26"
type: Post
category: 行业概念
slug: industry-day35
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day35【概念解析】组合模式
status: Published
cover: "https://images.unsplash.com/photo-1635789145651-d5770bc33039?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 7815278a-17df-4bc7-97f0-e214b0c31fae
updated: "2023-10-27 19:26:00"
---

# 整理定义

中文名称：组合模式

英文名称：composite pattern

| 出处                                                                                                                                                                                                                               | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                                                                                                     | 将对象<u>组成树结构</u>来表示局部和整体的层次关系，客户可以统一处理单个对象和对象的组合的一种设计模式                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 设计模式                                                                                                                                                                                                                           | 组合模式是一种结构型设计模式，你可以使用它将对象组合成<u>树状结构</u>，并且能像使用<u>独立对象</u>一样使用它们。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 百度百科                                                                                                                                                                                                                           | **组合模式** (Composite Pattern) 组合模式使得用户对<u>单个对象</u>和<u>组合对象</u>的使用具有一致性。 有时候又叫做<u>部分-整体模式</u>，它使我们<u>树型结构</u>的问题中，模糊了简单元素和复杂元素的概念，客户程序可以像处理简单元素一样来处理复杂元素,从而使得客户程序与复杂元素的内部结构 解耦 。                                                                                                                                                                                                                                                                                                                                              |
| 维基百科                                                                                                                                                                                                                           | In [software engineering](https://en.wikipedia.org/wiki/Software_engineering), the **composite pattern** is a partitioning [design pattern](<https://en.wikipedia.org/wiki/Design_pattern_(computer_science)>). The composite pattern describes a group of objects that are treated the same way as a single instance of the same type of object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.[[1]](https://en.wikipedia.org/wiki/Composite_pattern#cite_note-GangOfFour-1) |
| 【在软件工程中，组合模式是一种分区设计模式。 模式组合描述了一组对象，这些对象的处理方式与同一类型对象的单个实例相同。 组合的目的是将对象“组合”成树结构以表示部分-整体层次结构。 实现复合模式可以让客户端统一处理单个对象和组合。】 |

# 复述展开

## What is composite pattern？

> 📌 组合模式是一种结构型对象设计模式。它利用分治的思想将对象组合成树状结构，使得可以像使用独立对象一样使用它们。

该方式的最大优点在于你无需了解构成树状结构的对象的具体类。 你也无需了解对象是简单的还是复杂的。 你只需调用通用接口以相同的方式对其进行处理即可。 当你调用该方法后， 对象会将请求沿着树结构传递下去。

这里用到了分而治之，递归的思想去解决问题。

## 🌰\*1

在真实世界中，军队就是这样设置的，可以看成是组合模式。部队包含几个师，师又包含旅，旅包含团，再则营，连，排。一个命令从上层发出，通过每个层级进行传递，直到每个士兵都服从这个命令。

## 🌰\*2

Notion 本身，是有一个一个的 block 构成，它可以是一个 text，是 head，引用等等。同时，block 也可以包含 block，可以嵌套下去，这种模式也是一种组合模式。使用 notion api 时，返回的也是嵌套的 block 列表。

## 组合模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/dff45544-9916-4e50-89e1-6644fa78f953/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120447Z&X-Amz-Expires=3600&X-Amz-Signature=ced69b8b3d2480d7ba0168d8cc48b5e0cb036991824bbd41a0ae40120f954fd2&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **组件** （Component）  接口描述了树中简单项目和复杂项目所共有的操作。
2. **叶节点** （Leaf）  是树的基本结构，  它不包含子项目。

   一般情况下，  叶节点最终会完成大部分的实际工作，  因为它们无法将工作指派给其他部分。

3. **容器** （Container）——又名  “组合  （Composite）”——是包含叶节点或其他容器等子项目的单位。  容器不知道其子项目所属的具体类，  它只通过通用的组件接口与其子项目交互。

   容器接收到请求后会将工作分配给自己的子项目，  处理中间结果，  然后将最终结果返回给客户端。

4. **客户端** （Client）  通过组件接口与所有项目交互。  因此，  客户端能以相同方式与树状结构中的简单或复杂项目交互。

## 优缺点

优点：

- 你可以利用多态和递归机制更方便地使用复杂树结构。
- _开闭原则_。  无需更改现有代码，  你就可以在应用中添加新元素，  使其成为对象树的一部分。

缺点：

- 对于功能差异较大的类， 提供公共接口或许会有困难。 在特定情况下， 你需要过度一般化组件接口， 使其变得令人难以理解。

# 理解体会

组合模式再生活中其实非常常见，也是一种非常常见的是设计模式，文件系统就是有一种典型的树状结构。

对于绝大多数需要生成树状结构的问题来说， 组合都是非常受欢迎的解决方案。 组合最主要的功能是在整个树状结构上递归调用方法并对结果进行汇总。

合理利用好组合模式，就能掌握好树状结构相关的需求。

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
