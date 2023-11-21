---
password: ""
icon: ""
date: "2023-10-28"
type: Post
category: 行业概念
slug: industry-day37
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day37【概念解析】外观模式
status: Published
cover: "https://images.unsplash.com/photo-1523350305669-8b45966e3e89?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: bf82b3a4-6b39-455c-a42a-386e8158129b
updated: "2023-10-28 21:29:00"
---

# 整理定义

中文名称：外观模式

英文名称：facade pattern

| 出处           | 定义                                                                                                                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 为一个子系统的所有接口对外提供一致的高层访问接口，使该子系统更便于使用的一种设计模式。                                                                  |
| 设计模式       | **外观模式**是一种结构型设计模式，  能为程序库、  框架或其他复杂类提供一个简单的接口。                                                                  |
| 百度百科       | **Facade（外观）模式**为子系统中的各类（或结构与方法）提供一个简明一致的界面，隐藏子系统的复杂性，使子系统更加容易使用。                                |
| 维基百科       | **外观模式**（也拼写为 façade）是面向对象编程中常用的软件设计模式。 与建筑中的外观类似，外观是一个充当前端界面的对象，掩盖了更复杂的底层或结构代码。ode |

# 复述展开

## What is facade pattern？

> 📌 外观模式使一种结构型对象设计模式。它通过为对象的所有接口对外提供一致的高层次访问结构，使子系统更容易使用。它可以掩藏更复杂的底层代码，隐藏子系统的复杂性。

## 外观模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/9e4b6a6c-18f1-49e6-9528-1c0404a98a50/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120440Z&X-Amz-Expires=3600&X-Amz-Signature=3c9f43c67d579020af114ef4e758512de5be5ea76248bc7e2ae8e3bf4aef00df&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **外观** （Facade）  提供了一种访问特定子系统功能的便捷方式，  其了解如何重定向客户端请求，  知晓如何操作一切活动部件。
2. 创建**附加外观** （Additional Facade）  类可以避免多种不相关的功能污染单一外观，  使其变成又一个复杂结构。  客户端和其他外观都可使用附加外观。
3. **复杂子系统** （Complex Subsystem）  由数十个不同对象构成。  如果要用这些对象完成有意义的工作，  你必须深入了解子系统的实现细节，  比如按照正确顺序初始化对象和为其提供正确格式的数据。

   子系统类不会意识到外观的存在，  它们在系统内运作并且相互之间可直接进行交互。

4. **客户端** （Client）  使用外观代替对子系统对象的直接调用。

## 优缺点

**优点：**

- 你可以让自己的代码独立于复杂子系统。

**缺点：**

- 外观可能成为与程序中所有类都耦合的[**上帝对象**](https://refactoringguru.cn/antipatterns/god-object)。

# 理解体会

通常外观模式使为了使系统更加简单，所以一个对象即可。

外观模式需要对系统对象的所有接口进行作用，可能会增加耦合度。

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
