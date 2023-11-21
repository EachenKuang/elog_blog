---
password: ""
icon: ""
date: "2023-09-27"
type: Post
category: 行业概念
slug: industry-day6
tags:
  - 行业概念
  - 文字
  - 思考
summary: ""
title: Day6 【概念解析】 瀑布模型
status: Published
cover: "https://www.notion.so/images/page-cover/woodcuts_1.jpg"
urlname: 9bc2c552-6da8-4099-9a82-e3f0e680cb3c
updated: "2023-10-27 16:38:00"
---

# 前言

昨天介绍了[软件过程模型](https://kuangyichen.com/article/industry-day5)，并介绍了分类，其中瀑布模型作为最早的也是最经典的模型，值得提一嘴。

# 整理定义

| 描述                                                                                                                                                                                                                                                                                            | 出处                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **瀑布模型**是将软件生存周期中的各个活动规定为依线性顺序连接的若千阶段的模型，包括需求分析、设计、编码、测试、运行与维护。它规定了由前至后、相互街接的固定次序，如同<u>瀑布流水逐</u>级下落                                                                                                     | 《软件设计师》第五版                                                                      |
| **瀑布模型**是一种软件开发模型，按照需求分析、设计、编码、测试、运行和维护的顺序，将软件生命周期划分为一系列阶段，每个阶段的输出都是下一个阶段的输入。                                                                                                                                          | 百度百科                                                                                  |
| **瀑布模型**是将项目活动分解为线性顺序阶段，这意味着它们相互传递，其中每个阶段都取决于前一个阶段的可交付成果，并对应于任务的专业化。                                                                                                                                                            | [维基百科](https://en.wikipedia.org/wiki/Waterfall_model)【English】                      |
| 瀑布模型（Waterfall Model）最早强调软件或系统开发应有完整周期，且软件开发过程中必须依次经过中间的每一个阶段，开发过程中也应充分考量分析与设计的技术、时间和资源的投入等。由于该模式强调开发过程中有完成的规划、分析、设计、测试等过程，因此能有效的确保系统品质，因此它是软件开发界最初的标准。 | [维基百科](https://zh.wikipedia.org/zh-hans/%E7%80%91%E5%B8%83%E6%A8%A1%E5%9E%8B)【中文】 |

| **第一次被引用**
| Royce, Winston (1970), ["Managing the Development of Large Software Systems"](https://www-scf.usc.edu/~csci201/lectures/Lecture11/royce1970.pdf) (PDF), *Proceedings of IEEE WESCON*, **26** (August): 1–9 |

# 复述展开

### 瀑布模型

以上各个出处的概念解析都强调了瀑布模型的线性顺序特性，即每个阶段都依赖于前一个阶段的输出，每个阶段都必须在下一个阶段开始之前完成。不过，有些定义还强调了每个阶段都有明确的开始和结束点，这意味着在瀑布模型中，每个阶段的工作都是明确的，不会有模糊的地方。

**瀑布模型**是将软件生存周期中的各个活动规定为依线性顺序连接的若千阶段的模型，包括需求分析、设计、编码、测试、运行与维护。它规定了由前至后、相互街接的固定次序，如同<u>瀑布流水逐</u>级下落，如下图所示：

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/af077840-9dde-4680-a226-0b14e95c8837/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120636Z&X-Amz-Expires=3600&X-Amz-Signature=2ee30a3e2adb4c7047b8131f297628ee52eb6badc9e7d49348e881ffd1499960&X-Amz-SignedHeaders=host&x-id=GetObject)

瀑布模型为软件的开发和维护提供 了一种有效的管理模式，根据这一模式制定开发计划，进行成本
预算， 组织开发力量 ，以项目的阶段评审和文档控制为手段有效地对整个开发过程进行指导，所以它是以文档作为驱动、适合于软件需求很明确的软件项目的模型。

瀑布模型假设，一个待开发的系统需求是完整的、简明的、一致的，而且可以先于设计和实现完成之前产生。

### 瀑布模型的变体：V 模型

瀑布模型的一个变体是`V模型`，如下图所示。`V模型`描述了质量保证活动和沟通、建模相关活动以及早期构建相关的活动之间的关系。随着软件团队工作沿着 V 模型左侧步骤向下推进，基本问题需求逐步细化，形成问题及解决方案的技术描述。一旦编码结束，团队沿着 V 模型右侧的步骤向上推进工作，其实际上是执行了一系列测试(质量保证活动)，这些测试验证了团队沿着 V 模型左侧步骤向下推进过程中所生成的每个模型。V 模型提供了一种将验证确认活动应用于早期软件工程工作中的方法。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/f65a015a-1eb3-409d-9a69-c3d76a85b479/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120636Z&X-Amz-Expires=3600&X-Amz-Signature=9cf28c4da3bb0c4ecad6fb0fb98408f8ecf1fa2ee77c66e872495a6522aa1027&X-Amz-SignedHeaders=host&x-id=GetObject)

# 理解体会

瀑布模型的优点：

- 容易理解，管理成本低
- 强调开发的阶段性早期计划及需求调查和产品测试。

瀑布模型的不足之处：

- 客户必须能够完整、正确和清晰地表达他们的需要；在开始的两个或了个阶段中，很难评估真正的进度状态；当按近项目结束时，出现了大量的集成和测试工作；直到项目结束之前，都不能演示系统的能力。
- 在瀑布模型中，需求或设计中的错误往往只有到了项日后期才能够被发现，对于项目风险的控制能力较弱，从布导致项目常常延期完成，开发费用超出预算。

尽管瀑布模型存在上述缺点，但在某些特定情况下，它仍然是一种有效的软件开发方法。例如，在需求明确且稳定的项目中，瀑布模型可以提供良好的项目管理和预测能力。此外，在一些高度受规范约束的行业（如航空航天和医疗设备），瀑布模型的严格顺序性和强调文档的特点可能更适合满足这些行业的合规要求。

总之，瀑布模型是一种将软件开发过程划分为一系列线性阶段的模型，具有简单、易于理解和管理的优点。然而，它对需求变更的适应性差，缺乏灵活性，可能不适合需求不断变化的项目。在选择软件开发方法时，应根据项目的具体需求和特点来权衡瀑布模型与其他开发方法（如敏捷开发）的优缺点。

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
