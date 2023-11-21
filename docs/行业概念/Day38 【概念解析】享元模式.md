---
password: ""
icon: ""
date: "2023-10-29"
type: Post
category: 行业概念
slug: industry-day38
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day38 【概念解析】享元模式
status: Published
cover: "https://images.unsplash.com/photo-1585504198199-20277593b94f?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 1417f3a8-1e53-4a77-ab33-48eecc251e42
updated: "2023-10-29 11:19:00"
---

# 整理定义

中文名称：享元模式

英文名称：flyweight pattern

| 出处           | 定义                                                                                                                                                                                                                                                                                                          |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 一种设计模式。通过提供一个可以被大量对象共享的细粒度对象，以节省创建这些对象所需要的存储空间。                                                                                                                                                                                                                |
| 设计模式       | **享元模式**是一种结构型设计模式，  它摒弃了在每个对象中保存所有数据的方式，  通过共享多个对象所共有的相同状态，  让你能在有限的内存容量中载入更多对象。                                                                                                                                                      |
| 百度百科       | **享元模式** （英语：Flyweight Pattern）是一种软件 设计模式 。 它使用共享物件，用来尽可能减少内存使用量以及分享资讯给尽可能多的相似物件；它适合用于只是因重复而导致使用无法令人接受的大量内存的大量物件。 通常物件中的部分状态是可以分享。 常见做法是把它们放在外部数据结构，当需要使用时再将它们传递给享元。 |
| 维基百科       | 在计算机编程中，享元软件设计模式是指通过与其他类似对象共享一些数据来最小化内存使用的对象。                                                                                                                                                                                                                    |

# 复述展开

## What is **`flyweight`**?

剑桥英语字典：

> flyweight :a boxer who is in the lightest weight group, weighing 51 kilograms or less

> 蝇量级：体重最轻的拳击手，体重 51 公斤或以下

在设计模式中，使用了意译的方式进行翻译，唤作“享元模式”。

**享元 = 共享元素**

该模式的核心在于对于大量出现的细粒度的元素进行共享，从而减少内存的消耗。

## What is **`Flyweight pattern`**?

> 📌 享元模式，是一种结构型对象设计模式，它通过提供可以被大量共享的细粒度对象，从而减少存储空间。该模式适用于有可以大量共享复用的元素物件的场景，例如游戏、编辑器。

## 🌰\*1 **富文本编辑器的字母对象**

富文本编辑器在英文环境下，其中的文本由大量字母组成，为了便于做统一的格式化、计算等处理，需要将每个字母都存储为对象，但这样存储的代价太大了。

已知英文字母一共 26 个，所以文档中存在大量重复使用的字母，而每个字母除了位置信息外，其它信息都是相同且只读的，那么有办法降低富文本场景巨大的字母对象数量吗？

## 🌰\*2 **网盘存储**

当我们上传一部电影时，有时候几十 GB 的内容不到一秒就上传完了，这是网盘提示你，“已采用极速技术秒传”，你会不会心生疑惑，这么厉害的技术为什么不能每次都生效？

另外，网盘存储时，同一部电影可能都会存放在不同用户的不同文件夹中，而且电影文件又特别巨大，和富文本类似，电影文件也只有存放位置是不同的，而其余内容都特别巨大且只读，有什么办法能优化存储呢？

## 🌰\*3 大型多人游戏

玩多人游戏时，为了防止外挂，一般对象的创建与计算是在服务器完成的，那如何保证一个玩家拾取物品后，另一个玩家看到的物品会消失？

其实道理已经不言而喻了，虽然在不同客户端之间，游戏对象是相互独立的，但在一局游戏中，所有玩家的对象在服务器是共享的。

## 享元模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/9af8aff2-a8d4-4ce2-bf97-e4a214817fbc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120437Z&X-Amz-Expires=3600&X-Amz-Signature=5c69faf61a4cdec978739b663650f0651e7748ee63dcb45b157f19ab89cd95bf&X-Amz-SignedHeaders=host&x-id=GetObject)

1. 享元模式只是一种优化。  在应用该模式之前，  你要确定程序中存在与大量类似对象同时占用内存相关的内存消耗问题，  并且确保该问题无法使用其他更好的方式来解决。
2. **享元** （Flyweight）  类包含原始对象中部分能在多个对象中共享的状态。  同一享元对象可在许多不同情景中使用。  享元中存储的状态被称为  “内在状态”。  传递给享元方法的状态被称为  “外在状态”。
3. **情景** （Context）  类包含原始对象中各不相同的外在状态。  情景与享元对象组合在一起就能表示原始对象的全部状态。
4. 通常情况下，  原始对象的行为会保留在享元类中。  因此调用享元方法必须提供部分外在状态作为参数。  但你也可将行为移动到情景类中，  然后将连入的享元作为单纯的数据对象。
5. **客户端** （Client）  负责计算或存储享元的外在状态。  在客户端看来，  享元是一种可在运行时进行配置的模板对象，  具体的配置方式为向其方法中传入一些情景数据参数。
6. **享元工厂** （Flyweight Factory）  会对已有享元的缓存池进行管理。  有了工厂后，  客户端就无需直接创建享元，  它们只需调用工厂并向其传递目标享元的一些内在状态即可。  工厂会根据参数在之前已创建的享元中进行查找，  如果找到满足条件的享元就将其返回；  如果没有找到就根据参数新建享元。

# 理解体会

享元模式，顾名思义，就是将元素共享的模式。“共享” 就是享元模式的精髓，将那些大量的，具有很多内部状态而外部状态很少的对象进行共享，就是享元模式的使用方式。

不管使网盘使用的场景，还是大型在线游戏中，都使用了享元模式。就比如转存在自己网盘，顷刻间就完成了存储，实质上就是保存了一个网盘的一个索引，当要下载时都是从一个唯一的源进行下载。想想看，如果每个人都有 1TB 内存，百度云要多少云盘来存储，可能时进行了共享元素的优化。

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
