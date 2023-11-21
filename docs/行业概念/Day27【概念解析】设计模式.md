---
password: ""
icon: ""
date: "2023-10-18"
type: Post
category: 行业概念
slug: industry-day27
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
  - 必看精选
summary: ""
title: Day27【概念解析】设计模式
status: Published
cover: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 6aac3d5a-68a3-4b2f-8f26-174f57d4dc9a
updated: "2023-11-09 11:47:00"
---

# 整理定义

中文名称：设计模式 / 软件设计模式 / 代码设计模式

英文名称：design mode / **design patterns**

| 出处             | 定义                                                                                                                                                                                                                                                                                                                                                                                          |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书   | 对设计经验的显示表示。每个设计模式描述了一个反复出现的问题及其解法的核心内容，它命名、抽象并标识了一个通用设计结构的关键部分，使之可用来创建一个可复用的设计。                                                                                                                                                                                                                                |
| 百度百科         | 软件设计模式（[Design pattern](https://baike.baidu.com/item/Design%20pattern/10186718?fromModule=lemma_inlink)），又称设计模式，是一套被反复使用、多数人知晓的、经过分类编目的、[代码设计](https://baike.baidu.com/item/%E4%BB%A3%E7%A0%81%E8%AE%BE%E8%AE%A1/5499787?fromModule=lemma_inlink)经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性、程序的重用性。 |
| 维基百科         | 在软件工程中，软件设计模式是针对软件设计中给定上下文中常见问题的通用的、可重用的解决方案。 它不是可以直接转换为源代码或机器代码的成品设计。 相反，它是如何解决可在许多不同情况下使用的问题的描述或模板。 设计模式是程序员在设计应用程序或系统时可以用来解决常见问题的形式化最佳实践。                                                                                                         |
| 廖雪峰的官方网站 | 设计模式，即 Design Patterns，是指在软件设计中，被反复使用的一种代码设计经验。使用设计模式的目的是为了可重用代码，提高代码的可扩展性和可维护性。                                                                                                                                                                                                                                              |

> 🔥 说明：  
> 《设计模式》也特指 GoF（“[四人帮](https://baike.baidu.com/item/%E5%9B%9B%E4%BA%BA%E5%B8%AE/0?fromModule=lemma_inlink)”Gang of Four，指 Erich Gamma, Richard Helm, Ralph Johnson & John Vlissides 四人）的《设计模式》（1995 年出版）是第一次将设计模式提升到理论高度，并将之规范化。本书提出了 23 种基本设计模式，自此，在可复用[面向对象](https://baike.baidu.com/item/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/0?fromModule=lemma_inlink)软件的发展过程中，新的大量的设计模式不断出现。

# 复述展开

**设计模式**是软件设计中常见问题的典型解决方案。设计模式是程序员在设计应用程序或系统时可以用来解决常见问题的形式化最佳实践。

## 设计模式的四个要素

- 模式名称
- 问题
- 解决方案
- 效果

## 设计模式的分类

**一共 23 个常用的设计模式，GoF 把它们分成了三类：创建型模式、结构型模式、行为模式**

**创建型模式**

- 工厂方法
- 抽象工厂
- 生成器
- 原型
- 单例

**结构性模式**

- 适配器
- 桥接
- 组合
- 装饰器
- 外观
- 享元
- 代理

**行为模式**

- 责任链
- 命令
- 解释器
- 迭代器
- 中介者
- 备忘录
- 观察者
- 状态
- 策略
- 模板方法
- 访问者

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/3c50b52b-a761-40aa-b188-37d74999321b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120518Z&X-Amz-Expires=3600&X-Amz-Signature=85a774740166b77424ea1811de85ac75f060c8c8afc32919b210c84bd78ba51b&X-Amz-SignedHeaders=host&x-id=GetObject)

## 模式原则

1. 单一职责原则（Single Responsibility Principle，SRP）：一个类应该只有一个引起它变化的原因。每个类应该只负责一项职责，这样可以提高类的内聚性，降低类之间的耦合度。
2. 开放封闭原则（Open-Closed Principle，OCP）：软件实体（类、模块、函数等）应该对扩展开放，对修改关闭。通过使用抽象和接口，可以使系统在不修改现有代码的情况下进行扩展。
3. 里氏替换原则（Liskov Substitution Principle，LSP）：子类应该能够替换掉父类并且不会破坏程序的正确性。子类应该遵循父类的契约和行为，保持一致性。
4. 依赖倒置原则（Dependency Inversion Principle，DIP）：高层模块不应该依赖于低层模块，二者都应该依赖于抽象。抽象不应该依赖于具体实现细节，而是应该依赖于抽象接口。
5. 接口隔离原则（Interface Segregation Principle，ISP）：客户端不应该依赖于它不需要的接口。一个类不应该强迫依赖于它不使用的方法，应该将接口拆分为更小的、更具体的接口。
6. 迪米特法则（Law of Demeter，LoD）：一个对象应该对其他对象有尽可能少的了解。一个类应该只与其直接的朋友进行通信，而不与朋友的朋友进行通信。
7. 组合/聚合复用原则（Composition/Aggregation Reuse Principle，CARP）：优先使用组合和聚合关系，而不是继承关系来实现代码的复用。通过将对象组合或聚合在一起，可以更灵活地构建对象之间的关系，避免继承关系的紧耦合。

# 理解体会

学习设计模式，关键是学习**设计思想**，不能简单地**生搬硬套**，也不能为了使用设计模式而**过度设计**，要合理平衡设计的复杂度和灵活性，并意识到设计模式也并**不是万能的**。

设计模式也需要按照不同的语言特性来灵活适配使用。设计模式与具体的编程语言并没有直接的关系，它是一种独立于编程语言的设计思想。不过，不同的编程语言可能会有不同的实现方式。例如，在面向对象的编程语言中，我们可以使用类和对象来实现设计模式，而在函数式编程语言中，我们可能需要使用函数和闭包来实现设计模式。

在项目中科学运用设计模式，首先需要对设计模式有深入的理解，知道每种设计模式的适用场景和优缺点。其次，需要根据项目的具体需求和上下文来选择合适的设计模式。最后，需要灵活地使用设计模式，不应该生搬硬套，而应该根据实际情况对设计模式进行适当的修改和扩展。

总的来说，设计模式是我们解决设计问题的一种重要工具，但它并不是万能的。我们需要根据实际情况来灵活地使用设计模式，而不是生搬硬套。同时，我们也需要不断地学习和实践，以提高我们使用设计模式的能力。

# 参考文档：

[Software design pattern - Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern)

[设计模式 - 《中国大百科全书》第三版网络版 (zgbk.com)](https://www.zgbk.com/ecph/words?SiteID=1&ID=47683&Type=bkzyb&SubID=81497)

[常用设计模式有哪些？ (refactoringguru.cn)](https://refactoringguru.cn/design-patterns?_gl=1*ymni1y*_ga*MTQzNzM2NjExNC4xNjk3NjA0Njc3*_ga_SR8Y3GYQYC*MTY5NzYwNDY3Ny4xLjAuMTY5NzYwNDY4NC41My4wLjA.)

## 设计模式总结

    [bookmark](https://kuangyichen.com/article/industry-day27)


    ### 创建型设计模式


    	[bookmark](https://kuangyichen.com/article/industry-day28)


    	[bookmark](https://kuangyichen.com/article/industry-day29)


    	[bookmark](https://kuangyichen.com/article/industry-day30)


    	[bookmark](https://kuangyichen.com/article/industry-day31)


    	[bookmark](https://kuangyichen.com/article/industry-day32)


    ### 结构型设计模式


    	[bookmark](https://kuangyichen.com/article/industry-day33)


    	[bookmark](https://kuangyichen.com/article/industry-day34)


    	[bookmark](https://kuangyichen.com/article/industry-day35)


    	[bookmark](https://kuangyichen.com/article/industry-day36)


    	[bookmark](https://kuangyichen.com/article/industry-day37)


    	[bookmark](https://kuangyichen.com/article/industry-day38)


    	[bookmark](https://kuangyichen.com/article/industry-day39)


    ### 行为设计模式


    	[bookmark](https://kuangyichen.com/article/industry-day40)


    	[bookmark](https://kuangyichen.com/article/industry-day41)


    	[bookmark](https://kuangyichen.com/article/industry-day42)


    	[bookmark](https://kuangyichen.com/article/industry-day43)


    	[bookmark](https://kuangyichen.com/article/industry-day44)


    	[bookmark](https://kuangyichen.com/article/industry-day45)


    	[bookmark](https://kuangyichen.com/article/industry-day46)


    	[bookmark](https://kuangyichen.com/article/industry-day47)

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
