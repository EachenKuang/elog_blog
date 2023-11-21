---
password: ""
icon: ""
date: "2023-10-30"
type: Post
category: 行业概念
slug: industry-day39
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day39【概念解析】代理模式
status: Published
cover: "https://images.unsplash.com/photo-1597915869221-a9eaaa1e7700?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 99aba18c-c1a5-4159-bd80-31f91b704033
updated: "2023-10-30 22:12:00"
---

# 整理定义

中文名称：代理模式

英文名称：proxy pattern

| 出处                                                                                                                                     | 定义                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                           | 为一个子系统的所有接口对外提供一致的<u>高层访问接口</u>，使子系统<u>更便于使用</u>的一种设计模式                                                                                                                                                                                                                                   |
| 设计模式                                                                                                                                 | **代理模式**是一种结构型设计模式，  让你能够提供对象的替代品或其占位符。  代理控制着对于原对象的访问，  并允许在将请求提交给对象前后进行一些处理。                                                                                                                                                                                 |
| 百度百科                                                                                                                                 | 代理模式（英语：Proxy Pattern）是程序设计中的一种设计模式。                                                                                                                                                                                                                                                                        |
| 所谓的代理者是指一个类别可以作为其它东西的接口。代理者可以作任何东西的接口：网上连接、存储器中的大对象、文件或其它昂贵或无法复制的资源。 |
| 维基百科                                                                                                                                 | 在计算机编程中，代理模式是一种软件设计模式。 代理，以其最一般的形式，是一个充当其他东西的接口的类。 代理可以与任何东西交互：网络连接、内存中的大对象、文件或其他一些昂贵或无法复制的资源。 简而言之，代理是一个包装器或代理对象，客户端调用它来访问幕后的真实服务对象。 使用代理可以简单地转发到真实对象，或者可以提供额外的逻辑。 |

# 复述展开

## What is proxy pattern?

> 📌 代理模式是一种结构型设计模式，它为其他对象提供一种代理以控制对这个对象的访问。在代理模式中，一个类代表另一个类的功能。

## 🌰\*1

信用卡是银行账户的代理， 银行账户则是一大捆现金的代理。 它们都实现了同样的接口， 均可用于进行支付。 消费者会非常满意， 因为不必随身携带大量现金； 商店老板同样会十分高兴， 因为交易收入能以电子化的方式进入商店的银行账户中， 无需担心存款时出现现金丢失或被抢劫的情况。

## 🌰\*2

一个大图像的加载可能会非常耗费资源，我们可以在图像加载完毕之前使用一个轻量级的代理对象来代替真实的图像。只有当用户真正需要查看图像时，我们才使用真实的图像替换代理对象，这就是虚拟代理的例子。

## 代理模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/992912a5-bf0d-4691-8a6a-934f17fe24fd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120427Z&X-Amz-Expires=3600&X-Amz-Signature=cb673326c1e5607ed5fd902f7d5678414c7cf02edc8be090febcbcf4ab5203b0&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **服务接口** （Service Interface）  声明了服务接口。  代理必须遵循该接口才能伪装成服务对象。
2. **服务** （Service）  类提供了一些实用的业务逻辑。
3. **代理** （Proxy）  类包含一个指向服务对象的引用成员变量。  代理完成其任务  （例如延迟初始化、  记录日志、  访问控制和缓存等）  后会将请求传递给服务对象。

   通常情况下，  代理会对其服务对象的整个生命周期进行管理。

4. **客户端** （Client）  能通过同一接口与服务或代理进行交互，  所以你可在一切需要服务对象的代码中使用代理。

## 优缺点

优点：

- 你可以在客户端毫无察觉的情况下控制服务对象。
- 如果客户端对服务对象的生命周期没有特殊要求，  你可以对生命周期进行管理。
- 即使服务对象还未准备好或不存在，  代理也可以正常工作。
- _开闭原则_。  你可以在不对服务或客户端做出修改的情况下创建新代理。

缺点：

- 代码可能会变得复杂，  因为需要新建许多类。
- 服务响应可能会延迟。

# 理解体会

代理模式在需要控制对对象的访问，或者需要在访问对象时添加额外功能的时候非常有用。但是，使用代理模式也需要谨慎，因为它可能会导致系统变得复杂，特别是如果管理和维护代理对象和真实对象之间的关系变得复杂的话。

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
