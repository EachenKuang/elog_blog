---
password: ""
icon: ""
date: "2023-10-24"
type: Post
category: 行业概念
slug: industry-day33
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day33【概念解析】桥接模式
status: Published
cover: "https://images.unsplash.com/photo-1476725994324-6f6833ea0631?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 806bfa79-55d9-4aa1-8be1-68f874e20a6b
updated: "2023-10-27 19:25:00"
---

# 整理定义

中文名称：桥接模式

英文名称：bridge pattern

| 出处                                                                                                                                                                   | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                                         | 将类的<u>抽象</u>部分和它的<u>实现</u>部分分离，使它们可以相互独立地变化的一种设计模式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 设计模式                                                                                                                                                               | **桥接模式**是一种结构型设计模式，  可将一个大类或一系列紧密相关的类拆分为<u>抽象</u>和<u>实现</u>两个独立的层次结构，  从而能在开发时分别使用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 百度百科                                                                                                                                                               | **桥接模式**是将抽象部分与它的实现部分分离，使它们都可以独立地变化。 它是一种对象结构型模式，又称为柄体 (Handle and Body)模式或接口 (interface)模式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 维基百科                                                                                                                                                               | The **bridge pattern** is a [design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) used in [software engineering](https://en.wikipedia.org/wiki/Software_engineering) that is meant to *"decouple an* [_abstraction_](<https://en.wikipedia.org/wiki/Abstraction_(computer_science)>) *from its* [_implementation_](https://en.wikipedia.org/wiki/Implementation) *so that the two can vary independently"*, introduced by the [Gang of Four](https://en.wikipedia.org/wiki/Design_Patterns).[[1]](https://en.wikipedia.org/wiki/Bridge_pattern#cite_note-GoFp151-1) The *bridge* uses [encapsulation](<https://en.wikipedia.org/wiki/Encapsulation_(computer_science)>), [aggregation](https://en.wikipedia.org/wiki/Object_composition#Aggregation), and can use [inheritance](<https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)>) to separate responsibilities into different [classes](<https://en.wikipedia.org/wiki/Class_(computer_science)>). |
| 【桥接模式是软件工程中使用的一种设计模式，旨在“将抽象与其实现分离，以便两者可以独立变化”，由四人帮引入。该桥使用封装、聚合，并且可以使用继承将职责分离到不同的类中。】 |

# 复述展开

## What is bridge pattern?

桥接模式是一种结构型对象设计模式，它可以将类的抽象部分与实现部分分离，使两者可以独立变化。

桥接模式通过<u>组合</u>的方式将两个类进行结合，使两者进行<u>解耦</u>。

桥接模式的重点：

- **抽象部分**：经过抽象化，忽略某些信息，将不同的实体当作同一个对待；面向对象中将对象的共同性质抽取出来，形成类的过程给你，就是抽象化过程。
- **实现部分**：对于具体实现的部分，也要进行实现化，针对抽象化，给出具体实现；这个过程就是实现过程，过程的产出就是具体实现部分，具体实现部分产生的对象，比抽象产生的更具体，使对抽象化事物的具体化产物。

## 桥接模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/6704dc2f-ad2e-4144-b1c4-468a23753ec8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120452Z&X-Amz-Expires=3600&X-Amz-Signature=72ae705d2ddc1c5687588c3daacf51a4e56b03688d8681c539a98ec4b516525c&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **抽象部分** （Abstraction）  提供高层控制逻辑，  依赖于完成底层实际工作的实现对象。
2. **实现部分** （Implementation）  为所有具体实现声明通用接口。  抽象部分仅能通过在这里声明的方法与实现对象交互。
   抽象部分可以列出和实现部分一样的方法，  但是抽象部分通常声明一些复杂行为，  这些行为依赖于多种由实现部分声明的原语操作。
3. **具体实现** （Concrete Implementations）  中包括特定于平台的代码。
4. **精确抽象** （Refined Abstraction）  提供控制逻辑的变体。  与其父类一样，  它们通过通用实现接口与不同的实现进行交互。
5. 通常情况下， **客户端** （Client）  仅关心如何与抽象部分合作。  但是，  客户端需要将抽象对象与一个实现对象连接起来。

## 桥接模式优缺点

**优点：**

- 你可以创建与平台无关的类和程序。
- 客户端代码仅与高层抽象部分进行互动，  不会接触到平台的详细信息。
- _开闭原则_。  你可以新增抽象部分和实现部分，  且它们之间不会相互影响。
- _单一职责原则_。  抽象部分专注于处理高层逻辑，  实现部分处理平台细节。

**缺点：**

- 对高内聚的类使用该模式可能会让代码更加复杂。

# 理解体会

理解桥接模式，就是需要将抽象与实现进行解耦，通过组合的方式在抽象与实现之间构建一个桥梁，使之进行连接。

桥接模式一个典型的例子就是，需要在不同的操作系统，例如 Mac，Win，Android，iOS；开发支持播放格式有 MP4，AVA，RMVN，FLV 等的播放器，这种情况就可以使用桥接模式。

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
