---
password: ""
icon: ""
date: "2023-10-25"
type: Post
category: 行业概念
slug: industry-day34
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day34 【概念解析】适配器模式
status: Published
cover: "https://images.unsplash.com/photo-1564517945244-d371c925640b?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 52d2faa6-7c68-48c0-83cc-9b25e55d8677
updated: "2023-10-27 19:25:00"
---

# 整理定义

中文名称：适配器模式

英文名称：adapter pattern

| 出处                                                                                                                                                                                              | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                                                                    | 将一个类的接口转化为用户需要的另一种接口，可以解决系统间<u>接口不相容</u>的问题，从而提高复用性的一种设计模式                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 设计模式                                                                                                                                                                                          | **适配器模式**是一种结构型设计模式，  它能使<u>接口不兼容</u>的对象能够相互合作。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 百度百科                                                                                                                                                                                          | 在计算机编程中，适配器模式（有时候也称包装样式或者包装）将一个类的接口适配成用户所期待的。一个适配允许通常因为接口不兼容而不能在一起工作的类工作在一起，做法是将类自己的接口包裹在一个已存在的类中。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 维基百科                                                                                                                                                                                          | In [software engineering](https://en.wikipedia.org/wiki/Software_engineering), the **adapter pattern** is a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) (also known as [wrapper](https://en.wikipedia.org/wiki/Wrapper_function), an alternative naming shared with the [decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern)) that allows the [interface](<https://en.wikipedia.org/wiki/Interface_(computer_science)>) of an existing [class](<https://en.wikipedia.org/wiki/Class_(computer_science)>) to be used as another interface.[[1]](https://en.wikipedia.org/wiki/Adapter_pattern#cite_note-HeadFirst-1) It is often used to make existing classes work with others without modifying their [source code](https://en.wikipedia.org/wiki/Source_code). |
| 【在软件工程中，适配器模式是一种软件设计模式（也称为包装器，是与装饰器模式共享的另一种命名方式），它允许将现有类的接口用作另一个接口。 它通常用于使现有类与其他类一起工作，而无需修改其源代码。】 |

# 复述展开

## What is adapter pattern?

> 💡 **适配器模式**是一种结构性对象设计模式，它可以将一个类的接口转化为另一个接口，而不需要修改源代码，从而解决系统间不兼容的问题。

## 🌰\*1

例如，如果需要小汽车在铁轨上执行，需要增加一个可以在铁轨上的给汽车使用的适配器，然后通过适配器在铁轨上行驶。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/0e996f1b-15f5-4abf-acbd-6e6c48e990fa/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120451Z&X-Amz-Expires=3600&X-Amz-Signature=d65b66ae86e990c90753e1bd68e2f0669093a507d8545b0b318f6cc176c75991&X-Amz-SignedHeaders=host&x-id=GetObject)

## 🌰\*2

另一个例子，不同国家的交流电的标准是不一样，有双口插头、也有三项插头，那么在出国旅行时，就需要一个电源适配器来使用。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/881bfb35-cc8f-414d-9195-3aa40263d49f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120451Z&X-Amz-Expires=3600&X-Amz-Signature=f5f52a91a9a67c99b4e126ec0bcd066c4a4fa59014af7af528369c99938a218e&X-Amz-SignedHeaders=host&x-id=GetObject)

## 🌰\*3

同样，在软件工程领域，如果不想修改源代码，而有需要为一个新接口进行适配，那就可以用到适配器模式了。

## 适配器模式的结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/60ea1379-e606-4a31-b86a-3e00d892a815/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120451Z&X-Amz-Expires=3600&X-Amz-Signature=d272532ae231442728f0735c515d47cb7531699064b76f9a92e706e5f1540df7&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **客户端** （Client）  是包含当前程序业务逻辑的类。
2. **客户端接口** （Client Interface）  描述了其他类与客户端代码合作时必须遵循的协议。
3. **服务** （Service）  中有一些功能类  （通常来自第三方或遗留系统）。  客户端与其接口不兼容，  因此无法直接调用其功能。
4. **适配器** （Adapter）  是一个可以同时与客户端和服务交互的类：  它在实现客户端接口的同时封装了服务对象。  适配器接受客户端通过适配器接口发起的调用，  并将其转换为适用于被封装服务对象的调用。
5. 客户端代码只需通过接口与适配器交互即可，  无需与具体的适配器类耦合。  因此，  你可以向程序中添加新类型的适配器而无需修改已有代码。  这在服务类的接口被更改或替换时很有用：  你无需修改客户端代码就可以创建新的适配器类。

# 理解体会

适配器模式不管是在计算机软件领域，还是在我们的生活中都是随处可见的，为了不修改原有的接口而通过增加一个适配器的方式来兼容新的接口。

这个设计模式的关键在于 Adapter 类，先对新接口的对象进行转化后，然后调用原始的接口，并且将这些分装在 Adapter 类中，这些细节对于外界都是隐藏的，所以能够保证，在不影响原始接口的情况下，也能兼容新的系统逻辑。

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
