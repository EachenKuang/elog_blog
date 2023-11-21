---
password: ""
icon: ""
date: "2023-11-06"
type: Post
category: 行业概念
slug: industry-day46
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day46 【概念解析】状态模式
status: Published
cover: "https://images.unsplash.com/photo-1547016148-13c0aa169865?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 7ab7e26a-2540-402d-870c-dab572759979
updated: "2023-11-06 15:54:00"
---

# 整理定义

中文名称：状态模式

英文名称：state pattern

| 出处                                                                                                                                           | 定义                                                                                                                                                                                 |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 中国大百科全书                                                                                                                                 | 允许一个对象在内部状态改变时改变它的行为的设计模式                                                                                                                                   |
| 设计模式                                                                                                                                       | **状态模式**是一种行为设计模式，  让你能在一个对象的内部状态变化时改变其行为，  使其看上去就像改变了自身所属的类一样。                                                               |
| 百度百科                                                                                                                                       | 当一个对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类。                                                                                                             |
| 状态模式主要解决的是当控制一个对象状态的条件表达式过于复杂时的情况。把状态的判断逻辑转移到表示不同状态的一系列类中，可以把复杂的判断逻辑简化。 |
| 维基百科                                                                                                                                       | 状态模式是一种行为软件设计模式，允许对象在其内部状态发生变化时改变其行为。 这种模式接近有限状态机的概念。 状态模式可以解释为策略模式，它能够通过调用模式接口中定义的方法来切换策略。 |

# 复述展开

## What is state pattern?

> 💡 状态模式是一种行为设计模式。它允许一个对象在其内部状态改变时改变其行为。这种模式主要用于实现一个对象在不同状态下可能有的不同行为，并且可以在运行时根据对象的状态改变其行为。

## 🌰\*1

**电梯系统**：电梯的运行可以看作是状态模式的一个实例。电梯有多种状态，如停止状态、上升状态、下降状态等。当电梯在某个楼层停止时，如果有人按下上行按钮，电梯就会切换到上升状态；如果有人按下下行按钮，电梯就会切换到下降状态。在每种状态下，电梯的行为都是不同的。

## 🌰\*2

**音乐播放器**：音乐播放器也是状态模式的一个实例。播放器有播放状态、暂停状态、停止状态等。在播放状态下，如果用户点击暂停按钮，播放器就会切换到暂停状态；如果用户点击停止按钮，播放器就会切换到停止状态。在每种状态下，播放器的行为都是不同的。

## 🌰\*3

**交通信号灯**：交通信号灯是状态模式的另一个实例。信号灯有红灯状态、黄灯状态、绿灯状态。在红灯状态下，所有车辆都不能通行；在黄灯状态下，警告车辆准备停车；在绿灯状态下，车辆可以通行。信号灯会根据一定的规则自动切换状态，而在每种状态下，它的行为都是不同的。

## 状态模式的结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/bc0650e9-71f3-4575-8868-99aff5aa450e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120401Z&X-Amz-Expires=3600&X-Amz-Signature=5dab4ecd76929823baee5c8a13ce54e4c4fe6a300f89f996f538ed497b5fdf31&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **上下文** （Context）  保存了对于一个具体状态对象的引用，  并会将所有与该状态相关的工作委派给它。  上下文通过状态接口与状态对象交互，  且会提供一个设置器用于传递新的状态对象。
2. **状态** （State）  接口会声明特定于状态的方法。  这些方法应能被其他所有具体状态所理解，  因为你不希望某些状态所拥有的方法永远不会被调用。
3. **具体状态** （Concrete States）  会自行实现特定于状态的方法。  为了避免多个状态中包含相似代码，  你可以提供一个封装有部分通用行为的中间抽象类。
   状态对象可存储对于上下文对象的反向引用。  状态可以通过该引用从上下文处获取所需信息，  并且能触发状态转移。
4. 上下文和具体状态都可以设置上下文的下个状态，  并可通过替换连接到上下文的状态对象来完成实际的状态转换。

## 优缺点

优点：

- _单一职责原则_。  将与特定状态相关的代码放在单独的类中。
- _开闭原则_。  无需修改已有状态类和上下文就能引入新状态。
- 通过消除臃肿的状态机条件语句简化上下文代码。

缺点：

- 如果状态机只有很少的几个状态，  或者很少发生改变，  那么应用该模式可能会显得小题大作。

# 理解体会

状态模式在生活中非常常见，不管是红绿灯、播放器，甚至是电梯系统都是运用了状态模式。在计算机领域，进程的状态，像初始化、执行、结束、中断也都是状态模式的一种具体实现，不同的状态下对于进程的操作行为也是不同的。

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
