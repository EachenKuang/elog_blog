---
password: ""
icon: ""
date: "2023-10-31"
type: Post
category: 行业概念
slug: industry-day40
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day40 【概念解析】责任链模式
status: Published
cover: "https://www.notion.so/images/page-cover/met_william_morris_1875.jpg"
urlname: 85ec7872-2079-4fc4-8ece-911ea8411034
updated: "2023-10-31 08:45:00"
---

# 整理定义

中文名称：责任链模式/职责链模式

英文名称：chain of responsibility pattern/CoR/Chain of Command

| 出处           | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 把可以响应请求的对象组织成一条链，并在这条链上传递请求，从而保证多个对象都有机会处理请求并可以避免请求方和响应方的紧密耦合的一种设计模式。                                                                                                                                                                                                                                                                                                                     |
| 设计模式       | **责任链模式**是一种行为设计模式，  允许你将请求沿着处理者链进行发送。  收到请求后，  每个处理者均可对请求进行处理，  或将其传递给链上的下个处理者。                                                                                                                                                                                                                                                                                                           |
| 百度百科       | **责任链模式**是一种设计模式。 在责任链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。 请求在这个链上传递，直到链上的某一个对象决定处理此请求。 发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。                                                                                                                                                                  |
| 维基百科       | **责任链模式**在[面向对象程式设计](https://zh.wikipedia.org/wiki/%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88)里是一种[软件设计模式](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)，它包含了一些命令对象和一系列的处理对象。每一个处理对象决定它能处理哪些命令对象，它也知道如何将它不能处理的命令对象传递给该链中的下一个处理对象。该模式还描述了往该处理链的末尾添加新的处理对象的方法。 |

# 复述展开

## What is chain of responsibility pattern？

> 📌 **责任链模式**是一种行为设计模式。它把可以响应对象组成形成一条链条，使得请求发送时，每个处理者都可以对请求进行处理，或是直接传给下个处理者。这种模式使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。

## 责任链模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/347333ab-02ee-4d3b-a094-b1a537ae1afe/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120425Z&X-Amz-Expires=3600&X-Amz-Signature=a191775453f8eca410684a801fb19127da4469b35163ebb1fa50dc211d8ab3b6&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **处理者** （Handler）  声明了所有具体处理者的通用接口。  该接口通常仅包含单个方法用于请求处理，  但有时其还会包含一个设置链上下个处理者的方法。
2. **基础处理者** （Base Handler）  是一个可选的类，  你可以将所有处理者共用的样本代码放置在其中。

   通常情况下，  该类中定义了一个保存对于下个处理者引用的成员变量。  客户端可通过将处理者传递给上个处理者的构造函数或设定方法来创建链。  该类还可以实现默认的处理行为：  确定下个处理者存在后再将请求传递给它。

3. **具体处理者** （Concrete Handlers）  包含处理请求的实际代码。  每个处理者接收到请求后，  都必须决定是否进行处理，  以及是否沿着链传递请求。

   处理者通常是独立且不可变的，  需要通过构造函数一次性地获得所有必要地数据。

4. **客户端** （Client）  可根据程序逻辑一次性或者动态地生成链。  值得注意的是，  请求可发送给链上的任意一个处理者，  而非必须是第一个处理者。

## 使用场景

🌰 **当程序需要使用不同方式处理不同种类请求，  而且请求类型和顺序预先未知时，  可以使用责任链模式。**

该模式能将多个处理者连接成一条链。  接收到请求后，  它会  “询问”  每个处理者是否能够对其进行处理。  这样所有处理者都有机会来处理请求。

🌰 **当必须按顺序执行多个处理者时，  可以使用该模式。**

无论你以何种顺序将处理者连接成一条链，  所有请求都会严格按照顺序通过链上的处理者。

🌰 **如果所需处理者及其顺序必须在运行时进行改变，  可以使用责任链模式。**

如果在处理者类中有对引用成员变量的设定方法，  你将能动态地插入和移除处理者，  或者改变其顺序。

## 优缺点

优点：

- 你可以控制请求处理的顺序。
- _单一职责原则_。  你可对发起操作和执行操作的类进行解耦。
- _开闭原则_。  你可以在不更改现有代码的情况下在程序中新增处理者。

缺点：

- 部分请求可能未被处理。

# 理解体会

责任链的使用场景还是比较多的：

- 多条件流程判断：权限控制
- ERP 系统流程审批：总经理、人事经理、项目经理
- Java 过滤器的底层实现 Filter

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
