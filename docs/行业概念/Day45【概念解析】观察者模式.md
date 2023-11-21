---
password: ""
icon: ""
date: "2023-11-05"
type: Post
category: 行业概念
slug: industry-day45
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day45【概念解析】观察者模式
status: Published
cover: "https://images.unsplash.com/photo-1619884889432-b242fdee532a?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 74ad67a9-8873-469e-990a-1c80663b9fb6
updated: "2023-11-05 22:26:00"
---

# 整理定义

中文名称：观察者模式/依赖模式/发布订阅模式

英文名称：observer pattern/dependents pattern/publish-subscribe pattern

| 出处           | 定义                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 一种设计模式。定义一个对象与其他对象之间的一对多的依赖关系，当一个对象改变状态时，所有与它有依赖关系的对象都被通知并自动更新。又称依赖模式、发布订阅模式。                                                                                                                                                                                                                                                             |
| 设计模式       | **观察者模式**是一种行为设计模式，  允许你定义一种订阅机制，  可在对象事件发生时通知多个  “观察”  该对象的其他对象。                                                                                                                                                                                                                                                                                                   |
| 百度百科       | 观察者模式（有时又被称为模型（Model）-视图（View）模式、源-收听者(Listener)模式或从属者模式）是[软件设计模式](https://baike.baidu.com/item/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/2117635?fromModule=lemma_inlink)的一种。在此种模式中，一个目标物件管理所有相依于它的观察者物件，并且在它本身的状态改变时主动发出通知。这通常透过呼叫各观察者所提供的方法来实现。此种模式通常被用来实现事件处理系统。 |
| 维基百科       | 在软件设计和工程中，观察者模式是一种软件设计模式，其中称为主题的对象维护其依赖项（称为观察者）的列表，并通常通过调用其中一个方法来自动通知它们任何状态更改。                                                                                                                                                                                                                                                           |

# 复述展开

## What is observer patter?

> 📌 观察者模式是一种行为设计模式，对象(主题)维护了一个依赖(观察者)列表，以便主题可以使用观察者定义的任何方法通知所有观察者它所发生的变化。

## 🌰\*1

订阅杂志或者报纸，就是一种观察者模式。每次有新的一期杂志出现，就会邮寄到邮箱中。

## 🌰\*2

股票市场也是观察者模式的一个大型场景

## 🌰\*3

在分布式系统中实现事件服务

## 观察者模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/1edcbc56-66cc-4c3c-ab1b-25d963033158/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120407Z&X-Amz-Expires=3600&X-Amz-Signature=28bc22fd609394f3003f05540e683de31999c0c8c2270f2bea1571a9aa82ea20&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **发布者** （Publisher）  会向其他对象发送值得关注的事件。  事件会在发布者自身状态改变或执行特定行为后发生。  发布者中包含一个允许新订阅者加入和当前订阅者离开列表的订阅构架。
2. 当新事件发生时，  发送者会遍历订阅列表并调用每个订阅者对象的通知方法。  该方法是在订阅者接口中声明的。
3. **订阅者** （Subscriber）  接口声明了通知接口。  在绝大多数情况下，  该接口仅包含一个  `update`更新方法。  该方法可以拥有多个参数，  使发布者能在更新时传递事件的详细信息。
4. **具体订阅者** （Concrete Subscribers）  可以执行一些操作来回应发布者的通知。  所有具体订阅者类都实现了同样的接口，  因此发布者不需要与具体类相耦合。
5. 订阅者通常需要一些上下文信息来正确地处理更新。  因此，  发布者通常会将一些上下文数据作为通知方法的参数进行传递。  发布者也可将自身作为参数进行传递，  使订阅者直接获取所需的数据。
6. **客户端** （Client）  会分别创建发布者和订阅者对象，  然后为订阅者注册发布者更新。

## 优缺点

优点：

- _开闭原则_。  你无需修改发布者代码就能引入新的订阅者类  （如果是发布者接口则可轻松引入发布者类）。
- 你可以在运行时建立对象之间的联系。

缺点：

- 订阅者的通知顺序是随机的。

# 理解体会

观察者模式，也就是发布订阅模式，经常用于分布式应用的场景下，一般可以通过消息队列来实现。

对象(主题)维护了一个依赖(观察者)列表，以便主题可以使用观察者定义的任何方法通知所有观察者它所发生的变化

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
