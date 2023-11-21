---
password: ""
icon: ""
date: "2023-11-09"
type: Post
category: 行业概念
slug: industry-day49
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day49 【概念解析】访问者模式
status: Published
cover: "https://images.unsplash.com/photo-1569547033055-1379ab87d439?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: c2f5659b-6463-4491-83fd-3d48a26330ec
updated: "2023-11-12 20:10:00"
---

# 整理定义

中文定义：访问者模式

英文定义：visitor pattern

| 出处           | 定义                                                                                                                                                                                                |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 描述一个对象结构中某个对象元素需要执行的操作一种设计模式，使用该设计模式可以在不改变操作元素的类的前提下定义作用于这些元素的新操作                                                                  |
| 设计模式       | **访问者模式**是一种行为设计模式，  它能将算法与其所作用的对象隔离开来。                                                                                                                            |
| 百度百科       | 表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素类的前提[下定义](https://baike.baidu.com/item/%E4%B8%8B%E5%AE%9A%E4%B9%89/658188?fromModule=lemma_inlink)作用于这些元素的新操作。 |
| 维基百科       | 访问者模式是一种将算法与对象结构分开的软件设计模式。 由于这种分离，可以将新操作添加到现有对象结构中，而无需修改结构。 它是面向对象编程和软件工程中遵循开放/封闭原则的一种方法。                     |

# 复述展开

## What is visitor pattern?

> 🎈 访问者模式是一种行为设计模式，它允许你在不改变类的情况下增加新的操作。这种模式主要包含访问者（Visitor）和元素（Element）两种角色。元素接口定义了一个`accept`方法，该方法通常以一个访问者作为参数；访问者接口则定义了一组访问方法，用于处理不同类型的元素。  
> 访问者模式的主要思想是将数据结构和数据操作分离。数据结构负责存储数据，而数据操作则由访问者来完成。这样，当我们需要增加新的操作时，只需要增加新的访问者，而无需修改数据结构。

    访问者模式的主要思想是将数据结构和数据操作分离。数据结构负责存储数据，而数据操作则由访问者来完成。这样，当我们需要增加新的操作时，只需要增加新的访问者，而无需修改数据结构。

## 🌰\*1

**文档编辑器**：在一个文档编辑器中，文档可以包含多种元素，如文本、图片、表格等。如果我们想要实现一些操作，如渲染、打印、检查拼写等，可以使用访问者模式。每种操作可以定义为一个访问者，元素则提供一个`accept`方法，用于接受访问者的访问。

## 🌰\*2

**编译器**：在编译器中，源代码可以被解析为一个抽象语法树（AST）。如果我们想要实现一些操作，如类型检查、代码生成、优化等，可以使用访问者模式。每种操作可以定义为一个访问者，AST 节点则提供一个`accept`方法，用于接受访问者的访问。

## 🌰\*3

**电商平台**：在一个电商平台中，可以有多种商品，如书籍、电子产品、服装等。如果我们想要实现一些操作，如打折、计算运费、生成描述等，可以使用访问者模式。每种操作可以定义为一个访问者，商品则提供一个`accept`方法，用于接受访问者的访问。

## 访问者模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/96415895-366f-48c7-b696-5441fbe4833a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120350Z&X-Amz-Expires=3600&X-Amz-Signature=8e07b4da2b607b40bb799c57464fbc924bf8b1fd92266857e43fc86f31823a81&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **访问者** （Visitor）  接口声明了一系列以对象结构的具体元素为参数的访问者方法。  如果编程语言支持重载，  这些方法的名称可以是相同的，  但是其参数一定是不同的。
2. **具体访问者** （Concrete Visitor）  会为不同的具体元素类实现相同行为的几个不同版本。
3. **元素** （Element）  接口声明了一个方法来  “接收”  访问者。  该方法必须有一个参数被声明为访问者接口类型。
4. **具体元素** （Concrete Element）  必须实现接收方法。  该方法的目的是根据当前元素类将其调用重定向到相应访问者的方法。  请注意，  即使元素基类实现了该方法，  所有子类都必须对其进行重写并调用访问者对象中的合适方法。
5. **客户端** （Client）  通常会作为集合或其他复杂对象  （例如一个[**组合**](https://refactoringguru.cn/design-patterns/composite)树）  的代表。  客户端通常不知晓所有的具体元素类，  因为它们会通过抽象接口与集合中的对象进行交互。

## 优缺点

优点：

- _开闭原则_。  你可以引入在不同类对象上执行的新行为，  且无需对这些类做出修改。
- _单一职责原则_。  可将同一行为的不同版本移到同一个类中。
- 访问者对象可以在与各种对象交互时收集一些有用的信息。  当你想要遍历一些复杂的对象结构  （例如对象树），  并在结构中的每个对象上应用访问者时，  这些信息可能会有所帮助。

缺点：

- 每次在元素层次结构中添加或移除一个类时，  你都要更新所有的访问者。
- 在访问者同某个元素进行交互时，  它们可能没有访问元素私有成员变量和方法的必要权限。

# 理解体会

我们可以看出，访问者模式是一种**非常强大**的设计模式，它可以让我们的代码更加灵活，更易于扩展和维护。然而，我们也需要注意它的缺点，即增加新的数据结构比较困难。因此，在使用访问者模式时，我们需要根据实际情况来权衡。

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
