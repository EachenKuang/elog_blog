---
password: ""
icon: ""
date: "2023-11-04"
type: Post
category: 行业概念
slug: industry-day44
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day44【概念解析】备忘录模式
status: Published
cover: "https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/86f96968-f617-4740-a539-e383d7d7f980/jon-tyson-P2aOvMMUJnY-unsplash.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120308Z&X-Amz-Expires=3600&X-Amz-Signature=21e3e7ede9289ce7551d5db4344101c489f818dae962c8e06b2475a25f80027e&X-Amz-SignedHeaders=host&x-id=GetObject"
urlname: bcb84456-5b0d-4bf3-8cd1-040bbcfe3505
updated: "2023-11-04 10:09:00"
---

# 整理定义

中午名称：备忘录模式/快照模式

英文名称：memento pattern

| 出处           | 定义                                                                                                                                                                                                                                            |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 一种设计模式。在不被破坏封装的条件下，获得一个对象的内部状态并保存在对象之外，以便在需要时恢复对象状态。                                                                                                                                        |
| 设计模式       | **备忘录模式**是一种行为设计模式，  允许在不暴露对象实现细节的情况下保存和恢复对象之前的状态。                                                                                                                                                  |
| 百度百科       | 备忘录模式是一种软件[设计模式](https://baike.baidu.com/item/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F?fromModule=lemma_inlink)：在不破坏封闭的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。 |
| 维基百科       | 备忘录模式是一种公开对象私有内部状态的软件设计模式。 如何使用此功能的一个示例是将对象恢复到其先前状态（通过回滚撤消），另一个是版本控制，另一个是自定义序列化。                                                                                 |

# 复述展开

## What is memento pattern?

> 📌 备忘录模式是一种行为设计模式，它允许在不暴露对象实现细节的情况下<u>保存和恢复</u>对象之前的装填。它通过捕获对象的内部状态，并在对象之外保存这个状态来实现。

## 🌰\*1

Word 软件在编辑时按 Ctrl+Z 组合键时能撤销当前操作，使文档恢复到之前的状态。

## 🌰\*2

Photoshop 在编辑时，可以查看历史操作信息，并且对其进行恢复，这就是备忘录模式。

## 🌰\*3

游戏存档，比如原来的单机小游戏，可以存档当前的进度，在下次进入游戏时读取历史存档来继续完成游戏。这个也是备忘录模式。

## 备忘录模式结构

### 基于嵌套类实现

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/df5900f9-a872-49cd-a5eb-a55c0a894acb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120413Z&X-Amz-Expires=3600&X-Amz-Signature=e14029db6458a3bd0070bf42b109f4b04a72ba94a7f8ebb23685f68972e89d8e&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **原发器** （Originator）  类可以生成自身状态的快照，  也可以在需要时通过快照恢复自身状态。
2. **备忘录** （Memento）  是原发器状态快照的值对象  （value object）。  通常做法是将备忘录设为不可变的，  并通过构造函数一次性传递数据。
3. **负责人** （Caretaker）  仅知道  “何时”  和  “为何”  捕捉原发器的状态，  以及何时恢复状态。

   负责人通过保存备忘录栈来记录原发器的历史状态。  当原发器需要回溯历史状态时，  负责人将从栈中获取最顶部的备忘录，  并将其传递给原发器的恢复  （restoration）  方法。

4. 在该实现方法中，  备忘录类将被嵌套在原发器中。  这样原发器就可访问备忘录的成员变量和方法，  即使这些方法被声明为私有。  另一方面，  负责人对于备忘录的成员变量和方法的访问权限非常有限：  它们只能在栈中保存备忘录，  而不能修改其状态。

### 基于中间接口实现

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/7781b823-f563-42ec-80ca-397a9f855474/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120413Z&X-Amz-Expires=3600&X-Amz-Signature=ec45a19ac624477dfa33038ffd1c3dc53eb6baa9a2937643b9b4c636b0bbb6d4&X-Amz-SignedHeaders=host&x-id=GetObject)

1. 在没有嵌套类的情况下，  你可以规定负责人仅可通过明确声明的中间接口与备忘录互动，  该接口仅声明与备忘录元数据相关的方法，  限制其对备忘录成员变量的直接访问权限。
2. 另一方面，  原发器可以直接与备忘录对象进行交互，  访问备忘录类中声明的成员变量和方法。  这种方式的缺点在于你需要将备忘录的所有成员变量声明为公有。

### **封装更加严格的实现**

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/68057d87-51d2-476b-9532-26a50bead354/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120413Z&X-Amz-Expires=3600&X-Amz-Signature=b8a2303b5266537b94ca5896212f563695293990dee049d58d506f00dcc19a5e&X-Amz-SignedHeaders=host&x-id=GetObject)

1. 这种实现方式允许存在多种不同类型的原发器和备忘录。  每种原发器都和其相应的备忘录类进行交互。  原发器和备忘录都不会将其状态暴露给其他类。
2. 负责人此时被明确禁止修改存储在备忘录中的状态。  但负责人类将独立于原发器，  因为此时恢复方法被定义在了备忘录类中。
3. 每个备忘录将与创建了自身的原发器连接。  原发器会将自己及状态传递给备忘录的构造函数。  由于这些类之间的紧密联系，  只要原发器定义了合适的设置器  （setter），  备忘录就能恢复其状态。

## 优缺点

优点：

- 你可以在不破坏对象封装情况的前提下创建对象状态快照。
- 你可以通过让负责人维护原发器状态历史记录来简化原发器代码。

缺点：

- 如果客户端过于频繁地创建备忘录，  程序将消耗大量内存。
- 负责人必须完整跟踪原发器的生命周期，  这样才能销毁弃用的备忘录。
- 绝大部分动态编程语言  （例如 PHP、 Python 和 JavaScript）  不能确保备忘录中的状态不被修改。

# 理解体会

**当你需要创建对象状态快照来恢复其之前的状态时，  可以使用备忘录模式。**

**当直接访问对象的成员变量、  获取器或设置器将导致封装被突破时，  可以使用该模式。**

备忘录模式一般在编辑器，游戏存档中使用的比较多。

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
