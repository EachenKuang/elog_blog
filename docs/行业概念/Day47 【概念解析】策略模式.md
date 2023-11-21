---
password: ""
icon: ""
date: "2023-11-07"
type: Post
category: 行业概念
slug: industry-day47
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day47 【概念解析】策略模式
status: Published
cover: "https://images.unsplash.com/photo-1523875194681-bedd468c58bf?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: d89272f7-2653-4485-acd9-2d0da738545c
updated: "2023-11-07 16:47:00"
---

# 整理定义

中文名称：策略模式

英文名称：strategy pattern

| 出处           | 定义                                                                                                                                                                                                        |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 定义并分别组装一组算法，使这些算法可在程序运行时由系统操控，以独立于用户的方式根据情况动态互换的设计模式                                                                                                    |
| 设计模式       | **策略模式**是一种行为设计模式，  它能让你定义一系列算法，  并将每种算法分别放入独立的类中，  以使算法的对象能够相互替换。                                                                                  |
| 百度百科       | 策略模式作为一种[软件设计模式](https://baike.baidu.com/item/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F?fromModule=lemma_inlink)，指对象有某个行为，但是在不同的场景中，该行为有不同的实现算法。 |
| 维基百科       | 在计算机编程中，策略模式（也称为策略模式）是一种行为软件设计模式，可以在运行时选择算法。 代码不是直接实现单个算法，而是接收有关使用一系列算法中的哪些算法的运行时指令。                                     |

# 复述展开

## What is strategy pattern?

> 💡 策略模式是一种行为设计模式，它使你能够在运行时改变对象的行为。在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变的上下文对象。策略对象改变上下文对象的执行算法。  
> 这种模式的主要思想是定义一系列的算法，并将每一个算法封装起来，使它们可以互相替换。策略模式让算法独立于使用它的客户端。

## 🌰\*1

从 A 地到机场，有飞机、火车、轮船等多种交通方式过去，这些就是不同的出游策略。这些交通方式可以互相替换，而且这些交通方式是独立于你从 A 地到机场的方式。

## 🌰\*2

机器学习中，我们可能会有多种模型可以选择，如线性回归、决策树、神经网络等。每种模型都可以看作是一个策略，我们可以根据数据的特性和问题的需求来选择最合适的模型。

## 🌰\*3

在图像处理软件中，可能会有多种滤镜可以选择，如黑白、复古、饱和度增强等。每种滤镜都可以看作是一个策略，用户可以根据需要选择不同的滤镜来处理图片。

## 策略模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/55e5955a-b648-4f26-bcc8-be29e12610db/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120400Z&X-Amz-Expires=3600&X-Amz-Signature=d6cdf1251e5df4ad0fe30aba54ef84c36ee9fe32abaa59ff218c29293e87a057&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **上下文** （Context）  维护指向具体策略的引用，  且仅通过策略接口与该对象进行交流。
2. **策略** （Strategy）  接口是所有具体策略的通用接口，  它声明了一个上下文用于执行策略的方法。
3. **具体策略** （Concrete Strategies）  实现了上下文所用算法的各种不同变体。
4. 当上下文需要运行算法时，  它会在其已连接的策略对象上调用执行方法。  上下文不清楚其所涉及的策略类型与算法的执行方式。
5. **客户端** （Client）  会创建一个特定策略对象并将其传递给上下文。  上下文则会提供一个设置器以便客户端在运行时替换相关联的策略。

## 优缺点

优点：

- 你可以在运行时切换对象内的算法。
- 你可以将算法的实现和使用算法的代码隔离开来。
- 你可以使用组合来代替继承。
- _开闭原则_。  你无需对上下文进行修改就能够引入新的策略。

缺点：

- 如果你的算法极少发生改变，  那么没有任何理由引入新的类和接口。  使用该模式只会让程序过于复杂。
- 客户端必须知晓策略间的不同——它需要选择合适的策略。
- 许多现代编程语言支持函数类型功能，  允许你在一组匿名函数中实现不同版本的算法。  这样，  你使用这些函数的方式就和使用策略对象时完全相同，  无需借助额外的类和接口来保持代码简洁。

# 理解体会

策略模式是一种行为设计模式，它使你能够在运行时改变对象的行为。策略模式的强大之处，它可以让我们的代码更加灵活，更易于扩展和维护。

如果合理利用好策略模式，可以非常灵活使用我们的系统，增强灵活性和易维护性。然而，策略模式也有其缺点。如果策略数量过多，客户端需要了解每个策略的差异，以便选择合适的策略。此外，策略模式会增加程序的复杂性，因为需要创建多个策略类。

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
