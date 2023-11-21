---
password: ""
icon: ""
date: "2023-11-01"
type: Post
category: 行业概念
slug: industry-day41
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day41【概念解析】命令模式
status: Published
cover: "https://images.unsplash.com/photo-1524741978410-350ba91a70d7?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: bab5d2df-faef-44e1-b1d8-bb3de013588b
updated: "2023-11-02 11:27:00"
---

# 整理定义

中文名称：命令模式/动作模式/事务模式

英文名称：command pattern

| 出处     | 定义                                                                                                                                                                                   |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 设计模式 | 命令模式是一种行为设计模式，  它可将请求转换为一个包含与请求相关的所有信息的独立对象。  该转换让你能根据不同的请求将方法参数化、  延迟请求执行或将其放入队列中，  且能实现可撤销操作。 |
| 百度百科 | 在面向对象程式设计的范畴中，命令模式（Command Pattern）是一种设计模式，它尝试以物件来代表实际行动。                                                                                    |
| 维基百科 | 在面向对象编程中，命令模式是一种行为设计模式，其中对象用于封装稍后执行操作或触发事件所需的所有信息。 此信息包括方法名称、拥有该方法的对象以及方法参数的值。                            |

# 复述展开

## What is command pattern?

> 📌 命令模式（Command Pattern）是一种设计模式，它可以帮助我们将一个操作封装成一个对象。这样，我们可以将这个对象传递给其他代码，而不需要关心这个操作是如何实现的。命令模式的核心思想是将请求的发送者和接收者解耦，使得发送者不需要知道接收者的实现细节，只需要知道如何发送请求即可。  
> 想象一下，你正在玩一个电子游戏。游戏中有很多不同的操作，比如移动角色、攻击敌人、使用道具等。我们可以将这些操作看作是命令，每个命令都有一个执行的方法。游戏控制器（发送者）可以发送这些命令，而游戏角色（接收者）会根据命令执行相应的操作。这样，控制器不需要知道角色是如何实现这些操作的，只需要知道如何发送命令即可。

## 命令模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/81250671-8917-4e0d-9201-47428b468f07/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120424Z&X-Amz-Expires=3600&X-Amz-Signature=fc2de891bc9fc5057bce5c972507b2e5a4ba19ae8894aec9b5036e61fbfcc359&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **发送者** （Sender）——亦称  “触发者  （Invoker）”——类负责对请求进行初始化，  其中必须包含一个成员变量来存储对于命令对象的引用。  发送者触发命令，  而不向接收者直接发送请求。  注意，  发送者并不负责创建命令对象：  它通常会通过构造函数从客户端处获得预先生成的命令。
2. **命令** （Command）  接口通常仅声明一个执行命令的方法。
3. **具体命令** （Concrete Commands）  会实现各种类型的请求。  具体命令自身并不完成工作，  而是会将调用委派给一个业务逻辑对象。  但为了简化代码，  这些类可以进行合并。

   接收对象执行方法所需的参数可以声明为具体命令的成员变量。  你可以将命令对象设为不可变，  仅允许通过构造函数对这些成员变量进行初始化。

4. **接收者** （Receiver）  类包含部分业务逻辑。  几乎任何对象都可以作为接收者。  绝大部分命令只处理如何将请求传递到接收者的细节，  接收者自己会完成实际的工作。
5. **客户端** （Client）  会创建并配置具体命令对象。  客户端必须将包括接收者实体在内的所有请求参数传递给命令的构造函数。  此后，  生成的命令就可以与一个或多个发送者相关联了。

## 🌰\*1

**遥控器**：想象一下，你有一个电视遥控器，它有很多按钮，比如开关电视、调节音量、切换频道等。我们可以将这些操作封装成命令对象，当你按下一个按钮时，遥控器就发送一个命令给电视。这样，遥控器不需要知道电视是如何实现这些操作的，只需要知道如何发送命令即可。如果将来我们想要添加新的功能，比如调节亮度，我们只需要创建一个新的命令对象，而不需要修改遥控器的代码 2

## 🌰\*2

**订单系统**：假设你正在开发一个餐厅订单系统。顾客可以点餐、取消订单、支付等。我们可以将这些操作封装成命令对象，当顾客执行一个操作时，系统就发送一个命令给厨房或收银台。这样，系统不需要知道厨房或收银台是如何处理这些操作的，只需要知道如何发送命令即可。如果将来我们想要添加新的功能，比如外卖，我们只需要创建一个新的命令对象，而不需要修改系统的代码。

## 🌰\*3

**文本编辑器**：在一个文本编辑器中，用户可以执行很多操作，比如复制、粘贴、撤销、重做等。我们可以将这些操作封装成命令对象，当用户执行一个操作时，编辑器就发送一个命令给文档。这样，编辑器不需要知道文档是如何实现这些操作的，只需要知道如何发送命令即可。此外，命令模式还可以帮助我们实现撤销和重做功能。我们可以将执行过的命令保存在一个列表中，当用户想要撤销或重做时，我们只需要从列表中取出相应的命令并执行即可。

## 优缺点

**优点**

- _单一职责原则_。  你可以解耦触发和执行操作的类。
- _开闭原则_。  你可以在不修改已有客户端代码的情况下在程序中创建新的命令。
- 你可以实现撤销和恢复功能。
- 你可以实现操作的延迟执行。
- 你可以将一组简单命令组合成一个复杂命令。

**缺点**

- 代码可能会变得更加复杂，  因为你在发送者和接收者之间增加了一个全新的层次

# 理解体会

总之，命令模式可以帮助我们将操作封装成对象，使得发送者和接收者之间解耦。这样，我们可以更容易地添加新的功能，而不需要修改现有的代码。

## 参考：

[命令设计模式 (refactoringguru.cn)](https://refactoringguru.cn/design-patterns/command)

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
