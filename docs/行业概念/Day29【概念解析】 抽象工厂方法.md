---
password: ""
icon: ""
date: "2023-10-20"
type: Post
category: 行业概念
slug: industry-day29
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day29【概念解析】 抽象工厂方法
status: Published
cover: "https://image.kuangyichen.com/image/202310201959847.webp"
urlname: 920702be-9cb2-471c-af7c-30cc884d5905
updated: "2023-10-27 19:25:00"
---

# 整理定义

中文名称：抽象工厂模式

英文名称：abstract factory pattern

| 出处                                                                                                                                           | 定义                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                 | 一种设计模式。提供相关的或相互依赖的一组待创建对象的接口，根据不同的配置或运行环境加载具有相同接口的不同类的实例。                                                                                                                                                                                                                                                                                         |
| 百度百科                                                                                                                                       | 抽象工厂模式（Abstract Factory Pattern）隶属于设计模式中的创建型模式，用于产品族的构建。抽象工厂是所有形态的工厂模式中最为抽象和最具一般性的一种形态。抽象工厂是指当有多个抽象角色时使用的一种工厂模式。抽象工厂模式可以向客户端提供一个接口，使客户端在不必指定产品的具体情况下，创建多个产品族中的产品对象。                                                                                             |
| 维基百科                                                                                                                                       | The **abstract factory pattern** in [software engineering](https://en.wikipedia.org/wiki/Software_engineering) is a design pattern that provides a way to create families of related objects without imposing their concrete classes, by encapsulating a group of individual [factories](https://en.wikipedia.org/wiki/Factory_object) that have a common theme without specifying their concrete classes. |
| 软件工程中的抽象工厂模式是一种设计模式，它提供了一种创建相关对象族而不强加其具体类的方法，通过封装一组具有共同主题的单独工厂而不指定其具体类。 |

# 复述展开

抽象工厂模式属于创建型对象模式

![202310201939862.png](https://image.kuangyichen.com/image/202310201939862.png)

## **抽象工厂模式结构**

![202310201942857.png](https://image.kuangyichen.com/image/202310201942857.png)

1. **抽象产品** （Abstract Product）  为构成系列产品的一组不同但相关的产品声明接口。
2. **具体产品** （Concrete Product）  是抽象产品的多种不同类型实现。  所有变体  （维多利亚/现代）  都必须实现相应的抽象产品  （椅子/沙发）。
3. **抽象工厂** （Abstract Factory）  接口声明了一组创建各种抽象产品的方法。
4. **具体工厂** （Concrete Factory）  实现抽象工厂的构建方法。  每个具体工厂都对应特定产品变体，  且仅创建此种产品变体。
5. 尽管具体工厂会对具体产品进行初始化，  其构建方法签名必须返回相应的*抽象*产品。  这样，  使用工厂类的客户端代码就不会与工厂创建的特定产品变体耦合。 **客户端** （Client）  只需通过抽象接口调用工厂和产品对象，  就能与任何具体工厂/产品变体交互。

## 应用场景

> 💡 **如果代码需要与多个不同系列的相关产品交互，  但是由于无法提前获取相关信息，  或者出于对未来扩展性的考虑，  你不希望代码基于产品的具体类进行构建，  在这种情况下，  你可以使用抽象工厂。**

> 💡 **如果你有一个基于一组**[**抽象方法**](https://refactoringguru.cn/design-patterns/factory-method)**的类，  且其主要功能因此变得不明确，  那么在这种情况下可以考虑使用抽象工厂模式。**

## 优缺点

优点：

- 你可以确保同一工厂生成的产品相互匹配。
- 你可以避免客户端和具体产品代码的耦合。
- _**单一职责原则**_。  你可以将产品生成代码抽取到同一位置，  使得代码易于维护。
- _**开闭原则**_。  向应用程序中引入新产品变体时，  你无需修改客户端代码。

缺点：

- 由于采用该模式需要向应用中引入众多接口和类，  代码可能会比之前更加复杂。

# 理解体会

抽象工厂模式是一种创建型设计模式，它提供了一种方式，可以将一组具有同一主题的单独但相关/依赖的工厂封装起来。在抽象工厂模式中，客户端并不直接创建对象，而是通过调用工厂方法来创建对象。

我的理解是，抽象工厂模式主要用于处理系统中多个产品族【产品矩阵】的情况，而且系统只消费其中某一族的产品。例如，一个应用程序需要支持多种外观主题，每种主题都有一套特定的控件，如按钮、滚动条、窗口等。使用抽象工厂模式，我们可以为每种主题创建一个工厂，这个工厂可以创建该主题的所有控件。

# 参考：

[抽象工厂设计模式 (refactoringguru.cn)](https://refactoringguru.cn/design-patterns/abstract-factory)

[Abstract factory pattern - Wikipedia](https://en.wikipedia.org/wiki/Abstract_factory_pattern)

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
