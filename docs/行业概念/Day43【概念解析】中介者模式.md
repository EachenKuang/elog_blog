---
password: ""
icon: ""
date: "2023-11-03"
type: Post
category: 行业概念
slug: industry-day43
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day43【概念解析】中介者模式
status: Published
cover: "https://images.unsplash.com/photo-1572879023364-ab4f53e9d5fa?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: c971b9ef-29ed-4efc-a5cb-3ec44df3ecf1
updated: "2023-11-03 10:49:00"
---

# 整理定义

中文名称：中介者模式

英文名称：mediator pattern

| 出处                                                                                                                                                                                                                                                                            | 定义                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                                                                                                                                                  | 用一个中介对象来封装一系列对象的交互，从而把一批原来可能是交互关系复杂的对象转换成一组松散耦合的中间对象                                           |
| 设计模式                                                                                                                                                                                                                                                                        | **中介者模式**是一种行为设计模式，  能让你减少对象之间混乱无序的依赖关系。  该模式会限制对象之间的直接交互，  迫使它们通过一个中介者对象进行合作。 |
| 百度百科                                                                                                                                                                                                                                                                        | 一种设计模式。用一个中介对象来封装一系列对象的交互，从而把一批原来可能是交互关系复杂的对象转换成一组松散耦合的中间对象，以有利于维护和修改。       |
| 维基百科                                                                                                                                                                                                                                                                        | 在软件工程中，中介者模式定义了一个对象，该对象封装了一组对象如何交互。 该模式被认为是一种行为模式，因为它可以改变程序的运行行为。                  |
| 在面向对象编程中，程序通常由许多类组成。 业务逻辑和计算分布在这些类中。 然而，随着更多的类被添加到程序中，特别是在维护和/或重构期间，这些类之间的通信问题可能变得更加复杂。 这使得程序更难阅读和维护。 此外，更改程序可能会变得困难，因为任何更改都可能影响其他几个类中的代码。 |

# 复述展开

## What is mediator pattern?

> 💡 中介者模式是一种行为设计模式。它使用一个中介者对象，来封装一组对象的交互，从而将原本交互关系复杂的对象转换成一组松散耦合的中间对象。该模式有利于系统的维护和修改。

## 🌰\*1

**空中交通控制塔：**飞行器驾驶员们在靠近或离开空中管制区域时不会直接相互交流。 但他们会与飞机跑道附近， 塔台中的空管员通话。 如果没有空管员， 驾驶员就需要留意机场附近的所有飞机， 并与数十位飞行员组成的委员会讨论降落顺序。 那恐怕会让飞机坠毁的统计数据一飞冲天吧。

## 🌰\*2

**聊天室**：在一个聊天室中，用户发送的消息是通过服务器（中介者）转发的，而不是直接发送给其他用户。这样，用户就不需要知道其他用户的详细信息，只需要将消息发送给服务器，服务器会负责将消息转发给其他用户。

## 🌰\*3

**图形用户界面（GUI）**：在许多图形用户界面中，各种组件（如按钮、文本框等）之间的交互通常是通过一个中介者对象（如窗口或对话框）来协调的。例如，当一个按钮被点击时，它不会直接改变一个文本框的内容，而是通知中介者对象，然后由中介者对象来改变文本框的内容。

## 中介者模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/09d87bc2-a63c-4581-9510-469ca7d986c6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120415Z&X-Amz-Expires=3600&X-Amz-Signature=7dfe9b42e94e42ccbe4e217acacaf26550e7772e2c5f17b1eeed672308ba191c&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **组件** （Component）  是各种包含业务逻辑的类。  每个组件都有一个指向中介者的引用，  该引用被声明为中介者接口类型。  组件不知道中介者实际所属的类，  因此你可通过将其连接到不同的中介者以使其能在其他程序中复用。
2. **中介者** （Mediator）  接口声明了与组件交流的方法，  但通常仅包括一个通知方法。  组件可将任意上下文  （包括自己的对象）  作为该方法的参数，  只有这样接收组件和发送者类之间才不会耦合。
3. **具体中介者** （Concrete Mediator）  封装了多种组件间的关系。  具体中介者通常会保存所有组件的引用并对其进行管理，  甚至有时会对其生命周期进行管理。
4. 组件并不知道其他组件的情况。  如果组件内发生了重要事件，  它只能通知中介者。  中介者收到通知后能轻易地确定发送者，  这或许已足以判断接下来需要触发的组件了。

   对于组件来说，  中介者看上去完全就是一个黑箱。  发送者不知道最终会由谁来处理自己的请求，  接收者也不知道最初是谁发出了请求。

## 优缺点

**优点：**

- _单一职责原则_。  你可以将多个组件间的交流抽取到同一位置，  使其更易于理解和维护。
- _开闭原则_。  你无需修改实际组件就能增加新的中介者。
- 你可以减轻应用中多个组件间的耦合情况。
- 你可以更方便地复用各个组件。

**缺点：**

- 一段时间后，  中介者可能会演化成为[**上帝对象**](https://refactoringguru.cn/antipatterns/god-object)。

# 理解体会

中介者模式是一种行为设计模式，它通过引入一个中介对象来封装一系列对象之间的交互，使得对象之间不需要显式地相互引用，从而降低它们之间的耦合度。不管是空中交通控制塔的例子，还是聊天室的例子，都可以非常好的使用。

**当一些对象和其他对象紧密耦合以致难以对其进行修改时，  可使用中介者模式。**

**当组件因过于依赖其他组件而无法在不同应用中复用时，  可使用中介者模式。**

**如果为了能在不同情景下复用一些基本行为，  导致你需要被迫创建大量组件子类时，  可使用中介者模式。**

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
