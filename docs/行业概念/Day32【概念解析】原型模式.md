---
password: ""
icon: ""
date: "2023-10-23"
type: Post
category: 行业概念
slug: industry-day32
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day32【概念解析】原型模式
status: Published
cover: "https://www.notion.so/images/page-cover/webb3.jpg"
urlname: ceb6329e-8aef-419b-86ef-1521eed6940b
updated: "2023-10-27 19:25:00"
---

# 整理定义

中文名称：原型模式

英文名称：prototype pattern

| 出处                                                                                                                                       | 定义                                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                             | 使用一个原型来刻画要创建的类型，通过复制这个原型得到新的类的一种设计模式                                                                                |
| 设计模式                                                                                                                                   | **原型模式**是一种创建型设计模式，  使你能够复制已有对象，  而又无需使代码依赖它们所属的类。                                                            |
| 百度百科                                                                                                                                   | 用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。                                                                                        |
| 维基百科                                                                                                                                   | 原型模式是创建型模式的一种，其特点在于通过“复制”一个已经存在的实例来返回新的实例,而不是新建实例。被复制的实例就是我们所称的“原型”，这个原型是可定制的。 |
| 原型模式多用于创建复杂的或者耗时的实例，因为这种情况下，复制一个已经存在的实例使程序运行更高效；或者创建值相等，只是命名不一样的同类数据。 |

# 复述展开

## What is prototype pattern

> 💡 原型模式作为一种创建型对象的设计模式，它主要通过原型类提供的克隆接口来创建与原型一致的对象。不需要知道原型的类也可以创建一个一模一样的对象。

## 原型模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/c034b258-1c2e-4d72-9536-5efb005f29e3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120458Z&X-Amz-Expires=3600&X-Amz-Signature=314d5e12c00fda1b1b58fcd3691b275315d45fd272b6f45d838c0e72afb2e903&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **原型** （Prototype）  接口将对克隆方法进行声明。  在绝大多数情况下，  其中只会有一个名为  `clone`克隆的方法。
2. **具体原型** （Concrete Prototype）  类将实现克隆方法。  除了将原始对象的数据复制到克隆体中之外，  该方法有时还需处理克隆过程中的极端情况，  例如克隆关联对象和梳理递归依赖等等。
3. **客户端** （Client）  可以复制实现了原型接口的任何对象。

# 理解体会

## 原型模式的优缺点：

**优点**

1. 性能优良：当对象创建成本较高时，克隆可以显著提高性能，因为它通常比实例化一个全新对象更快。
2. 简化对象创建：如果对象的创建过程复杂或者需要很多步骤，使用原型模式可以简化这个过程。
3. 动态添加或删除对象：原型模式允许在运行时动态地添加或删除对象。

**缺点**

1. 克隆复杂对象可能很复杂：如果对象有循环引用或者复杂的构造逻辑，克隆可能会变得困难。
2. 克隆可能导致数据一致性问题：如果对象的某些属性是引用类型，那么浅克隆可能会导致数据一致性问题。

## 原型模式的应用场景包括：

1. 当一个系统需要独立于其产品的创建、组合和表示时，可以使用原型模式。
2. 当要实例化的类在运行时刻被指定时，可以使用原型模式。
3. 当需要避免创建一个与产品类层次平行的工厂类层次时，可以使用原型模式。
4. 当类的实例只能有几个不同状态组合中的一种时，通过克隆原型对象得到新实例可能比手工实例化更方便。

## 具体到语言

1. Python 的  `Cloneable`  （克隆）  组件就是一个典型的原型模式。
2. Java 的  `Cloneable`  （可克隆）  接口也是原型模式。

任何类都可通过实现该接口来实现可被克隆的性质。

[**`java.lang.Object#clone()`**](http://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#clone--) （类必须实现  [**`java.lang.Cloneable`**](http://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html)  接口）

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
