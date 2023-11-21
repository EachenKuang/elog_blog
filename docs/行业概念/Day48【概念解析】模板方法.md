---
password: ""
icon: ""
date: "2023-11-08"
type: Post
category: 行业概念
slug: industry-day48
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day48【概念解析】模板方法
status: Published
cover: "https://images.unsplash.com/photo-1626544827763-d516dce335e2?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 387b2c0d-afc4-4c75-b4f1-7844b7ffeacc
updated: "2023-11-08 21:27:00"
---

# 整理定义

中文名称：模板方法模式

英文名称：template method

| 出处           | 定义                                                                                                                                            |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 定义一个操作的算法框架，算法的某些步骤会在子类中实现，它使得子类可以不改变整个算法的结构即可重定义该算法的某些特定步骤的一种设计模式。          |
| 设计模式       | **模板方法模式**是一种行为设计模式，  它在超类中定义了一个算法的框架，  允许子类在不修改结构的情况下重写算法的特定步骤。                        |
| 百度百科       | **模板方法模式**定义了一个算法的步骤，并允许子类别为一个或多个步骤提供其实践方式。 让子类别在不改变算法架构的情况下，重新定义算法中的某些步骤。 |
| 维基百科       | 模板方法是超类（通常是抽象超类）中的方法，并根据许多高级步骤定义操作的骨架。 这些步骤本身是通过与模板方法相同的类中的其他辅助方法来实现的。     |

# 复述展开

## What is template method？

> 💡 模板方法模式是一种行为设计模式。它在父类中定义一个算法操作的框架，允许子类在不修改结构的情况下重写算法的特定步骤，从而可以重新定义一系列新的方法。

## 🌰\*1

**制作食物**：做饭、烘焙等食物制作过程中，通常有一些固定的步骤，比如准备食材、烹饪、装盘等。这些步骤的顺序是固定的，但具体的实现可能会根据食物的种类和食谱的不同而不同。在这种情况下，我们可以使用模板方法模式，将制作食物的步骤定义在一个方法中，然后让子类来实现具体的烹饪步骤。

## 🌰\*2

**建筑设计**：在建筑设计中，通常有一些固定的步骤，比如设计、获取许可、施工、装修等。这些步骤的顺序是固定的，但具体的实现可能会根据建筑的类型和设计的不同而不同。在这种情况下，我们可以使用模板方法模式，将建筑设计的步骤定义在一个方法中，然后让子类来实现具体的设计和施工步骤。

## 🌰\*3

**软件开发生命周期**：在软件开发中，通常有一些固定的步骤，比如需求分析、设计、编码、测试、部署等。这些步骤的顺序是固定的，但具体的实现可能会根据项目的需求和开发方法的不同而不同。在这种情况下，我们可以使用模板方法模式，将软件开发的步骤定义在一个方法中，然后让子类来实现具体的设计、编码和测试步骤。

## 模板方法模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/40b1152e-8bd8-4992-8ddf-b77fe8f8f799/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120358Z&X-Amz-Expires=3600&X-Amz-Signature=21a9f574d70f8e11c52a0f6a72097db97a19e67f10ed5b7b38a1398d3a9b0727&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **抽象类** （Abstract­Class）  会声明作为算法步骤的方法，  以及依次调用它们的实际模板方法。  算法步骤可以被声明为  `抽象`类型，  也可以提供一些默认实现。
2. **具体类** （Concrete­Class）  可以重写所有步骤，  但不能重写模板方法自身。

## 优缺点

优点：

- 你可仅允许客户端重写一个大型算法中的特定部分，  使得算法其他部分修改对其所造成的影响减小。
- 你可将重复代码提取到一个超类中。

缺点：

- 部分客户端可能会受到算法框架的限制。
- 通过子类抑制默认步骤实现可能会导致违反*里氏替换原则*。
- 模板方法中的步骤越多，  其维护工作就可能会越困难。

# 理解体会

模板方法模式的主要优点是它可以实现代码复用，提高代码的可维护性。同时，它也提供了一种控制子类扩展的方法。然而，如果子类过多，可能会导致代码复杂性增加。

模板方法可以降低代码的重复度，如果有多个场景拥有相同的步骤，只是有些许细微差别，那么模板方法会很适合。

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
