---
password: ""
icon: ""
date: "2023-10-22"
type: Post
category: 行业概念
slug: industry-day31
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day31【概念解析】单例模式
status: Published
cover: "https://www.notion.so/images/page-cover/nasa_space_shuttle_challenger.jpg"
urlname: 81f18f9b-bd50-4c7d-add1-5deb2cf9f805
updated: "2023-10-27 19:25:00"
---

# 概念定义

中文名称：单例模式/单态模式/单件模式

英文名称：singleton pattern

| 出处                                                                       | 定义                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 中国大百科全书                                                             | 一种保证一个类只有一个实例，并提供一个全局性的访问点的设计模式。又称<u>单态模式</u>、<u>单件模式</u>                                                                                                                                                                                                                                                                                                   |
| 设计模式                                                                   | **单例模式**是一种创建型设计模式，  让你能够保证一个类只有一个实例，  并提供一个访问该实例的全局节点。                                                                                                                                                                                                                                                                                                 |
| 百度百科                                                                   | 单例模式，属于创建类型的一种常用的[软件设计模式](https://baike.baidu.com/item/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/2117635?fromModule=lemma_inlink)。通过单例模式的方法创建的类在当前进程中只有一个[实例](https://baike.baidu.com/item/%E5%AE%9E%E4%BE%8B/3794138?fromModule=lemma_inlink)。                                                                                         |
| 维基百科                                                                   | In [software engineering](https://en.wikipedia.org/wiki/Software_engineering), the **singleton pattern** is a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) that restricts the [instantiation](<https://en.wikipedia.org/wiki/Instantiation_(computer_science)>) of a [class](<https://en.wikipedia.org/wiki/Class_(computer_programming)>) to a singular instance. |
| 【在软件工程中，单例模式是一种将类的实例化限制为单个实例的软件设计模式。】 |

# 复述展开

## What is Singleton？

概念延申：单例（singleton ）在不同领域的概念？

在科学技术领域下：

- 数学领域
  - 一个只有一个元素的集合
- 计算机领域
  - 单例模式（singleton pattern），一种设计模式
  - 单例绑定（singleton bound），用于编码理论
  - 单例变量（singleton variable），仅引用一次的变量
- 社会科学领域
  - singleton，一个假设的<u>世界秩序</u>，只有一个决策机构
  - singleton，一个在语言学中不是双子的<u>辅音</u>
  - singleton，指非双胞胎或其他多胞胎的<u>人</u>，也就是单胎人士

> 📌 **singleton**，用人话说就是，**作为名词时**，在某个特定领域，只有一类的事物，可以是政府机构、辅音、人；**作为形容词时**，表示某样事物在某种特定领域下，有且只有一个，具有全局或者当前的<u>唯一性</u>。

## What is Singleton Pattern

单例模式是一种创建型的设计模式，通过使用单例模式，可以保证一个类在某个场景下，有且只有一个实例存在，作为全局唯一的节点。

## Why is Singleton Pattern

为什么要设计？

1. 最常见的原因是控制某些共享资源 （例如数据库或文件） 的访问权限。
2. 保证用户访问都是同一个资源。

## 单例模式架构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/5b4edbb1-8ed2-48b9-9fe2-73c31b717689/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120502Z&X-Amz-Expires=3600&X-Amz-Signature=d8e2d6c3da25d5eece178c4d6c0005819c2386452d6753ae7e9492d69402b25e&X-Amz-SignedHeaders=host&x-id=GetObject)

所有单例的实现都包含以下两个相同的步骤：

- 将默认构造函数设为私有，  防止其他对象使用单例类的  `new`运算符。
- 新建一个静态构建方法作为构造函数。  该函数会  “偷偷”  调用私有构造函数来创建对象，  并将其保存在一个静态成员变量中。  此后所有对于该函数的调用都将返回这一缓存对象。

如果你的代码能够访问单例类，  那它就能调用单例类的静态方法。  无论何时调用该方法，  它总是会返回相同的对象。

## 代码实现

更多 Java 实现的单例模式：[Java Singleton Design Pattern Best Practices with Examples | DigitalOcean](https://www.digitalocean.com/community/tutorials/java-singleton-design-pattern-best-practices-examples)

单线程单例模式

多线程单例模式

线程安全的单例模式

懒汉模式

```java
package com.journaldev.singleton;

public class LazyInitializedSingleton {

    private static LazyInitializedSingleton instance;

    private LazyInitializedSingleton(){}

    public static LazyInitializedSingleton getInstance() {
        if (instance == null) {
            instance = new LazyInitializedSingleton();
        }
        return instance;
    }
}
```

饥汉模式

```java

public class EagerInitializedSingleton {

    private static final EagerInitializedSingleton instance = new EagerInitializedSingleton();

    // private constructor to avoid client applications using the constructor
    private EagerInitializedSingleton(){}

    public static EagerInitializedSingleton getInstance() {
        return instance;
    }
}
```

# 理解体会

单例模式使用在各种语言中都非常常见，像 Java 中的 logging，thread pool。

虽然单例模式很常见，但是它实际上被很多开发者认为是<u>反模式</u>，因为违反了单一职责原理。所以现在，在一些语言中，也在慢慢减少使用单例模式。

在具体使用上，如何需要对共享变量（数据库、文件管理）进行使用，可以使用单例模式，全局共享同一个。

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
