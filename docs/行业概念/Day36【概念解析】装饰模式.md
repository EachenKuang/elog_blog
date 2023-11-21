---
password: ""
icon: ""
date: "2023-10-27"
type: Post
category: 行业概念
slug: industry-day36
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day36【概念解析】装饰模式
status: Published
cover: "https://images.unsplash.com/photo-1632119289059-793dd347950f?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 1a622523-99f1-40e3-aeae-ce93db356ceb
updated: "2023-10-27 20:14:00"
---

# 整理定义

中文名称：装饰模式/装饰器模式/装饰者模式

英文名称：**Decorator Pattern**

| 出处                                                                                                                                                                                                                                                                                                                 | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                                                                                                                                                                                       | 在对象中动态地加入新的职责，同时避免继承出大量的子类的一种设计模式，从而提供了一种<u>动态扩展类</u>的功能的方法                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 设计模式                                                                                                                                                                                                                                                                                                             | **装饰模式**是一种结构型设计模式，  允许你通过将对象放入包含行为的特殊封装对象中来为原对象<u>绑定新的行为</u>。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 百度百科                                                                                                                                                                                                                                                                                                             | **装饰模式**指的是在<u>不必改变原类文件</u>和<u>使用继承</u>的情况下，<u>动态地扩展</u>一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 维基百科                                                                                                                                                                                                                                                                                                             | In [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming), the **decorator pattern** is a [design pattern](<https://en.wikipedia.org/wiki/Design_pattern_(computer_science)>) that allows behavior to be added to an individual [object](<https://en.wikipedia.org/wiki/Object_(computer_science)>), dynamically, without affecting the behavior of other objects from the same [class](<https://en.wikipedia.org/wiki/Class_(computer_science)>).[[1]](https://en.wikipedia.org/wiki/Decorator_pattern#cite_note-1) The decorator pattern is often useful for adhering to the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), as it allows functionality to be divided between classes with unique areas of concern[[2]](https://en.wikipedia.org/wiki/Decorator_pattern#cite_note-2) as well as to the [Open-Closed Principle](https://en.wikipedia.org/wiki/Open-closed_principle), by allowing the functionality of a class to be extended without being modified.[[3]](https://en.wikipedia.org/wiki/Decorator_pattern#cite_note-3) Decorator use can be more efficient than subclassing, because an object's behavior can be augmented without defining an entirely new object. |
| 【在面向对象编程中，装饰器模式是一种设计模式，允许动态地将行为添加到单个对象，而不影响同一类中其他对象的行为。 装饰器模式通常对于遵守单一职责原则很有用，因为它允许在具有独特关注领域的类之间划分功能，并且通过允许类的功能被 扩展而不修改。装饰器的使用比子类化更有效，因为无需定义全新的对象即可增强对象的行为。】 |

# 复述展开

## What is decorator pattern?

【用自己的话进行复述】

> 📌 **装饰模式**是一种结构性对象设计模式，它允许动态地将行为添加到单个对象上，从而为类提供了扩展的功能。它的作用在于能够避免继承，同时不影响其他对象的行为。它的实现是通过创建一个包装对象来实现的，也就是对原有对象进行装饰和包装。

## 🌰\*1

穿衣服就是一个典型的装饰模式的例子。你可以先穿打底衫，如果觉得冷，还可以套一件大外套，如果碰上下雨，再披个雨衣。所有的衣服都扩展了你的行为，但它们并不构成人的本身，如果不需要了，则可以随时脱掉。

## 🌰\*2

在 gRPC 中的 filter（过滤器）实际上也是一种装饰模式，它可以嵌套多个 filter，每个 filter 都实现的是同一种接口，所有 filter 并不在意自己到底是纯粹的对象还是包装后的对象，所有的 filter 排列成一个栈，按照顺序执行各自的逻辑。

## 装饰模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/efb149b8-d397-402d-90c0-b4d670fd8a03/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120443Z&X-Amz-Expires=3600&X-Amz-Signature=5ce5b795fb1917416ddd6560ba0ee806f5ef16947664b73b3518302857af9594&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **部件** （Component）  声明封装器和被封装对象的公用接口。
2. **具体部件** （Concrete Component）  类是被封装对象所属的类。  它定义了基础行为，  但装饰类可以改变这些行为。
3. **基础装饰** （Base Decorator）  类拥有一个指向被封装对象的引用成员变量。  该变量的类型应当被声明为通用部件接口，  这样它就可以引用具体的部件和装饰。  装饰基类会将所有操作委派给被封装的对象。
4. **具体装饰类** （Concrete Decorators）  定义了可动态添加到部件的额外行为。  具体装饰类会重写装饰基类的方法，  并在调用父类方法之前或之后进行额外的行为。
5. **客户端** （Client）  可以使用多层装饰来封装部件，  只要它能使用通用接口与所有对象互动即可。

## 优缺点

优点：

- 你无需创建新子类即可扩展对象的行为。
- 你可以在运行时添加或删除对象的功能。
- 你可以用多个装饰封装对象来组合几种行为。
- _单一职责原则_。  你可以将实现了许多不同行为的一个大类拆分为多个较小的类。

缺点：

- 在封装器栈中删除特定封装器比较困难。
- 实现行为不受装饰栈顺序影响的装饰比较困难。
- 各层的初始化配置代码看上去可能会很糟糕。

# 理解体会

装饰模式实际上就是一种不改变原有对象，却能通过包装的方式增强原有对象的行为的设计模式。

在实际应用中，装饰模式常常用于以下场景：需要在运行时动态地添加功能到对象，或者需要在运行时动态地删除功能；不能使用继承来扩展功能，或者使用继承不方便的情况。

总的来说，装饰模式是一种非常有用的设计模式，它可以让我们在不修改原始类的情况下，动态地扩展对象的功能。然而，使用装饰模式也需要谨慎，因为它可能会增加系统的复杂性和理解难度。

# 参考

[装饰设计（装饰者模式 / 装饰器模式） (refactoringguru.cn)](https://refactoringguru.cn/design-patterns/decorator)

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
