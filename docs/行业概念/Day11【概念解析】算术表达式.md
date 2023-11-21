---
password: ""
icon: ""
date: "2023-10-02"
type: Post
category: 行业概念
slug: industry-day11
tags:
  - 行业概念
  - 文字
  - 思考
summary: ""
title: Day11【概念解析】算术表达式
status: Published
cover: "https://images.unsplash.com/photo-1564867317243-9219c75d28cf?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 20c14a1e-a7ae-4a74-832c-974cdd8c1032
updated: "2023-10-27 19:22:00"
---

# 整理定义

## 前置知识：

树、二叉树

## 概念定义

**算术表达式**是由数字、运算符、括号以及代数变量等组成的式子，例如 `(3 + 4) * 5`。在计算机科学中，算术表达式通常用于描述数学公式和算法。

**逆波兰表达式**（Reverse Polish Notation，RPN），也叫后缀表达式，是一种去掉括号且依然能定义清楚优先级的算术表达式。例如，上述的算术表达式 `(3 + 4) * 5` 在逆波兰表达式中表示为 `3 4 + 5 *`。

**波兰表达式**：也称前缀表达式，是把运算符放在两个操作数之前的表达式方式。例如，上述的算术表达式 `(3 + 4) * 5` 在波兰表达式中表示为 `* + 3 4 5`。

**中缀表达式：**是把运算符放在两个操作数之间的表达式方式。例如，上述的算法表达式`(3 + 4) * 5` 就是中缀表达式。

## 应用场景

1. **计算器**：许多计算器，包括计算机的计算器程序，使用逆波兰表达式来处理算术表达式，因为这种方式可以消除括号，简化计算过程。
2. **编译器**：编译器在解析算术表达式时，通常会将其转换为逆波兰表达式，因为逆波兰表达式更容易转换为机器语言。

**实例说明**：

假设我们有一个算术表达式 `(3 + 4) * 5`，我们想要计算它的值。

1. 首先，我们将这个算术表达式转换为逆波兰表达式，得到 `3 4 + 5 *`。
2. 然后，我们从左到右读取逆波兰表达式，遇到数字就将其压入栈中，遇到运算符就从栈中弹出两个数字进行计算，然后将计算结果再压入栈中。
3. 对于 `3 4 + 5 *`，我们首先将 `3` 和 `4` 压入栈中，然后遇到 `+`，所以我们从栈中弹出 `4` 和 `3`，计算 `4 + 3` 得到 `7`，然后将 `7` 压入栈中。
4. 接着我们将 `5` 压入栈中，然

# 复述展开

## 二叉树的三种遍历方式

为什么要区分，前缀、后缀、中缀呢？这跟表达式中符号的所处的位置有关系。

我们先将表达式`(3 + 4) * 5` 使用二叉树表示出来。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/bd6c60e9-357e-49b7-936c-d5ecc9dd6970/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120615Z&X-Amz-Expires=3600&X-Amz-Signature=bee303b0075134406127935220ba376e0f3015d8db5a9ac938495e1fb0aa85e3&X-Amz-SignedHeaders=host&x-id=GetObject)

根据遍历的不同顺序，我们可以分为前序遍历、中序遍历、后续遍历的方法，以上面的 `3-4` 表达式为例：

- 前序遍历：**根节点**，左子节点，右子节点【`- 3 4`】
- 中序遍历：左子节点，**根节点**，右子节点【`3 - 4`】
- 后续遍历：左子节点，右子节点，**根节点**【`3 4 -`】

可以发现，遍历的顺序与根节点的位置有关系，根节点的位置在前，就叫前序遍历；在后则是后续遍历；在中间则是中序遍历。

## 算术表达式

在了解了二叉树的三种遍历方式之后，就可以很自然地将表达式先转为二叉树，然后得出对应的前缀、后缀表达式了。

```python
def infix_to_postfix(infix_expression):
    precedence = {'+':1, '-':1, '*':2, '/':2, '^':3}
    stack = []  # 用作栈的列表
    postfix = []  # 用来存储后缀表达式的列表
    for char in infix_expression:
        if char not in precedence:  # 如果是操作数，直接添加到后缀表达式
            postfix.append(char)
        else:
            while stack and precedence[char] <= precedence[stack[-1]]:  # 弹出所有优先级更高或相等的运算符
                postfix.append(stack.pop())
            stack.append(char)  # 将当前运算符压入栈
    while stack:  # 弹出栈中剩余的运算符
        postfix.append(stack.pop())
    return ' '.join(postfix)  # 返回后缀表达式

print(infix_to_postfix("3 + 4 * 5"))  # 输出 "3 4 5 * +"
```

# 理解体会

算术表达式是我们在编程和数学中经常遇到的一种结构，它包含了操作数（如数字或变量）和运算符（如加、减、乘、除）。理解和处理算术表达式是编程中的一个基本技能。

在我看来，算术表达式的一个重要特性是它的结构性。无论是前缀、中缀还是后缀表达式，都有其特定的规则和结构，这使得我们可以通过编程来解析和处理它们。例如，我们可以使用栈来处理后缀表达式，或者使用递归来处理前缀和中缀表达式。

此外，我也认为理解不同类型的表达式（前缀、中缀和后缀）对于理解和使用数据结构和算法非常有帮助。例如，前缀和后缀表达式与栈的使用密切相关，而中缀表达式则与树的使用密切相关。通过理解和处理这些表达式，我们可以更好地理解和使用这些数据结构和算法。

最后，我认为处理算术表达式是一个很好的编程练习。它可以帮助我们提高对数据结构和算法的理解，提高我们的编程技能，以及提高我们解决问题的能力。

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
