---
password: ""
icon: ""
date: "2023-09-28"
type: Post
category: 行业概念
slug: industry-day7
tags:
  - 行业概念
  - 文字
  - 思考
summary: ""
title: Day7 【概念解析】 需求分析
status: Published
cover: "https://images.unsplash.com/photo-1555929583-b78e544368d3?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 5b80f37f-040b-457f-99c1-ea7d204f41b8
updated: "2023-10-27 16:38:00"
---

# 前言

需求分析，作为软件开发过程中一个非常重要的前置环节，有必要好好研究研究的。

# 整理定义

## 需求分析的定义

| 来源                                                                                                                 | 需求分析的解析                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| [**百度百科**](https://baike.baidu.com/item/%E9%9C%80%E6%B1%82%E5%88%86%E6%9E%90/1072922)                            | 需求分析是软件工程中的一个重要环节，主要是对用户的需求进行详细的调查和研究，明确用户的需求，并将这些需求转化为软件系统的功能需求和性能需求。 |
| [**维基百科**](https://en.wikipedia.org/wiki/Requirements_analysis)                                                  | 需求分析是确定服务、系统或产品的新特性或修改的过程，它涉及到与利益相关者的沟通以及对他们的需求的理解和记录。                                 |
| [**敏捷联盟**](https://www.agilealliance.org/glossary/requirements/)                                                 | 在敏捷开发中，需求分析更注重迭代和持续的反馈，以便更好地理解和满足用户的需求。                                                               |
| [**IBM 知识中心**](https://www.ibm.com/docs/en/rational-reqpro/2003.06.00?topic=processes-requirements-analysis)     | IBM 将需求分析视为一个过程，通过这个过程，可以确定系统或产品的需求，并将这些需求转化为一种形式，以便可以进行系统设计和项目管理。             |
| [**Project Management Institute (PMI)**](https://www.pmi.org/learning/library/requirements-analysis-techniques-7166) | PMI 强调需求分析在项目管理中的重要性，它可以帮助项目经理理解项目的目标，从而更好地管理项目的进度和预算。                                     |
| [**Association for Computing Machinery (ACM)**](https://www.acm.org/education/curricula-recommendations)             | ACM 在其计算机科学课程建议中，将需求分析作为软件工程教育的一个重要部分。                                                                     |
| [**国际需求工程协会 (IREB)**](https://www.ireb.org/en/home/)                                                         | IREB 将需求分析视为需求工程的一个重要部分，它涉及到需求的发现、记录和验证。                                                                  |
| [**国际标准化组织 (ISO)**](https://www.iso.org/standard/34423.html)                                                  | ISO/IEC 25010:2011 标准中，需求分析是确定和理解产品需求的过程，以便满足用户和利益相关者的期望。                                              |
| [**国家软件与服务外包能力成熟度模型 (CMMI)**](https://cmmiinstitute.com/cmmi)                                        | 在 CMMI 模型中，需求分析是确定、记录和维护需求的过程，它是软件开发生命周期的一个重要部分。                                                   |
| [**Scrum 指南**](https://www.scrumguides.org/scrum-guide.html)                                                       | 在 Scrum 框架中，需求分析是通过产品待办事项列表进行的，这是一个持续不断的过程，需要产品负责人、开发团队和利益相关者的紧密合作。              |

## 软件需求的定义以及需求分类

**软件需求**是<u>指用户对目标软件系统在功能、行为、性能、设计约束等方面的期望</u>。通常，这些需求包括功能需求、性能需求、用户或人的因素、环境需求、界面需求、文档需求、数据需求、资源使用需求、安全保密需求、可靠性需求、软件成本消耗与开发进度需求等，并预先估计以后系统可能达到的目标。此外，还需要注意其他非功能性的需求。

——《软件设计师教程》（第 5 版）

| 需求类型                   | 具体内容                                                                                                         |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| 功能需求                   | 系统需要完成的任务，包括在何时执行，何时修改或升级                                                               |
| 性能需求                   | 软件开发的技术性指标，如存储容量限制、执行速度、响应时间及吞吐量                                                 |
| 用户或人的因素             | 用户的类型，用户对使用计算机的熟练程度，需要接受的训练，用户理解、使用系统的难度，用户错误操作系统的可能性等     |
| 环境需求                   | 未来软件应用的环境，包括硬件和软件，如机型、外设、地点、分布、湿度、磁场干扰等，以及操作系统、网络、数据库等     |
| 界面需求                   | 来自其他系统的输入，到其他系统的输出，对数据格式的特殊规定，对数据存储介质的规定                                 |
| 文档需求                   | 需要哪些文档，文档针对哪些读者                                                                                   |
| 数据需求                   | 输入、输出数据的格式，接收、发送数据的频率，数据的准确性和精度，数据流量，数据需保持的时间                       |
| 资源使用需求               | 软件运行时所需要的数据、其他软件、内存空间等资源，软件开发、维护所需的人力、支撑软件、开发设备等                 |
| 安全保密需求               | 是否需要对访问系统或系统信息加以控制，隔离用户数据的方法，用户程序如何与其他程序和操作系统隔离以及系统备份要求等 |
| 可靠性需求                 | 系统的可靠性要求，系统是否必须检测和隔离错误，出错后，重启系统允许的时间等                                       |
| 软件成本消耗与开发进度需求 | 开发是否有规定的时间表，软/硬件投资有无限制等                                                                    |
| 其他非功能性需求           | 如采用某种开发模式，确定质量控制标准、里程碑和评审、验收标准、各种质量要求的优先级等，以及可维护性方面的要求     |

## 需求分析原则

需求分析过程的具体实现有不同的分析方法，这些方法有自己独特的特点。然而，这些分析方法都遵循一组操作原则。
(1)必须能够表示和理解问题的信息域。
(2)必须能够定义软件将完成的任务。
(3)必须能够表示软件的行为(作为外部事件的结束)。
(4)必须划分描述数据、功能和行为的模型，从而可以分层次地揭示细节。
(5)分析过程应该从要素信息移向细节信息。
通过应用这些原则，分析人员将能系统地处理问题。检查信息域可以更完整地理解功能，通过模型可以更简洁地交流功能和行为的特征，应用抽象与分解可减少问题的复杂度。

## 需求工程

**需求工程**是<u>一个不断反复的需求定义、文档记录、需求演进的过程，并最终在验证的基础上冻结需求</u>。需求工程可以细分为需求获取、需求分析与协商、系统建模、需求规约、需求验
证以及需求管理 6 个阶段。

1.  **需求获取**
    在需求获取阶段，系统分析人员通过与用户的交流、对现有系统的观察以及对任务进行分析确定系统或产品范围的限制性描述、与系统或产品有关的人员及特征列表、系统的技术环境的描述、系统功能的列表及应用于每个需求的领域限制、一组描述不同运行条件下系统或产品使用状况的应用场景以及为更好地定义需求市开发的原型。需求获取的工作产品为进行需求分析提供了基础。
2.  **需求分析与协商**
    需求获取结束后，分析活动对需求进行分类组织，分析每个需求与其他需求的关系，以检查需求的一致性、重登和遗漏的情况，并根据用户的需要对需求进行排序。在需求获取阶段，经常出现以下问题:用户提出的要求超出软件系统可以实现的范围或实现能力;不同的用户提出了相互冲突的需求;每个用户在提出自己的需求时都会说“这是至关重要的”，所以系统分析人员需要通过一个谈判过程来调解这些冲突。
3.  **系统建模**
    建模技术可以通过合适的工具和符号系统地描述需求。建模工具的使用在用户和系统分析人员之间建立了统一的语言和理解的“桥梁”，同时系统分析人员借助建模技术对获取的需求信息进行分析，排除错误和弥补不足，确保需求文档正确地反映用户的真实意图。常用的分析和建模方法有面向数据流方法、面向数据结构方法和面向对象方法。
    在观察和研究某一事物或某一系统时，常常把它抽象为一个模型。创建模型是需求分析阶段的重要活动。模型以一种简洁、准确、结构清晰的方式系统地描述了软件需求，从而帮助分析员理解系统的信息、功能和行为，使得需求分析任务更容易实现，结果更系统化，同时易于发现用户描述中的模糊性和不一致性；模型将成为软件设计的基础，为设计者提供软件要素的表示视图，这些表示可被转化到实现的语境中去;更重要的是，模型还可以在分析人员和用户之间建立更快捷的沟通方式，使两者可以用相同的工具分析和理解问题。
    在软件需求分析阶段所创建的模型，要着重于描述系统要做什么，而不是如何去做，目标软件的模型不应涉及软件的实现细节。通常情况下，分析人员使用图形符号来创建模型，将信息、处理、系统行为和其他相关特征描达为各种可识别的符号，同时与符号图形相配套，并辅以文字描述，可使用自然语言或某种特殊的专门用于描述需求的语言来提供辅助的信息描述。
    目前，己存在的多种需求分析方法引用了不同的分析策略，常用的分析方法有以下两种: 1. 面向数据流的结构化分析方法(SA)。 2. 面向对象的分析方法(OOA)。
4.  **需求规约**
    软件需求规约是分析任务的最终产物，通过建立完整的信息描述、详细的功能和行为描述、性能需求和设计约束的说明、合适的验收标准，给出对目标软件的各种需求。需求规约作为用户和开发者之间的一个协议，在之后的软件工程各阶段发挥重要的作用。
    软件需求规约中通常包含以下内容。 1. **引言**。引言陈述软件目标，在基于计算机的系统语境内进行描述。 2. **信息描述**。信息描达给出软件必须解决的问题的详细描述，记录信息内容、信息流和信息结构。 3. **功能描述**。功能描述用来描述解决问题所需要的每个功能。其中包括为每个功能说明一个处理过程:叙述设计约束:叙述性能特征;用一个或多个图形形象地表示软件的整体结构和软件功能与其他系统元素间的相互影响。 4. **行为描述**。行为描述用于描述作为外部事件和内部产生的控制特征的软件操作。 5. **检验标准**。检验标准描述检验系统成功的标志，即对系统进行什么样的测试，得到什么样的结果，就表示系统已经成功实现了。检验标准是“确认测试”的基础。 6. **参考书目**。参考书目包含了对所有和该软件相关的文档的引用，其中包括其他的软件工程文档、技术参考文献、厂商文献和标准。 7. **附录**。附录包含了规约的补充信息，表格数据、算法的详细描述、图表和其他资料。
5.  **需求验证
    需求验证**作为需求开发阶段工作的复查手段，其目的是要检验需求功能的正确性、完整性和清晰性，是否能够反映用户的意愿，由于需求的变化往往使系统的设计和实现也跟着改变，所以由需求问题引起系统变更的成本比修改设计或代码错误的成本高得多。因此，为保证软件需求定义的质量，评审应指定专门的人员负责，并按规程严格进行。需求验证需要对需求文档中定义的需求执行多种检查。开发团队要对用户需求进行“遍访”，逐条解释需求含义:评审团队应该检查需求的有效性、一致性和作为一个整体的完备性。评审人员评审时往往需要检查以下内容: 1. 系统定义的目标是否与用户的要求一致。 2. 系统需求分析阶段提供的文档资料是否齐全;文档中的描述是否完整、清晰、准确地反映了用户要求。 3. 被开发项目的数据流与数据结构是否确定且充足。 4. 主要功能是否己包括在规定的软件范围之内，是否都已充分说明。 5. 设计的约束条件或限制条件是否符合实际。 6. 开发的技术风险是什么。 7. 是否详细地制定了检验标准，它们能否对系统定义进行确认。

        为了保证软件需求定义的质量，验证应该由专门的人员来负责，按照规定严格进行。除分析人员之外，还要有用户，开发部门的管理者，软件设计、实现、测试的人员参加。评审结束应有负责人的结论意见和签宇。
        想要判浙一组需求是否符合用户的需要是很困难的。用户需要描述出系统的操作过程，构
        想出如何让系统加入到他们的工作中去，这种抽象对于一个普通用户来说比较困难。所以，需求验证也不可能发现所有的需求问题。在需求验证之后，对遗漏的补充以及对错误理解的更正是不可避免的，因此需要进行需求管理。

6.  **需求管理**
    在实际的开发过程中，获取、分析、建模、编写规约和验证这些需求开发活动通常是交叉、递增和反复地进行。市且，软件系统的需求会变更，这些变更不仅会存在于项目开发过程，而且会出现在项目己经付诸应用之后。软件需求管理是一组用于帮助项目组在项目进展中的任何时候去标识、控制和跟踪需求的活动，对需求工程所有相关活动的规划和控制。换句话说，需求管理就是一种获取、组织并记录系统需求的系统化方案，以及一个使用户与项目团队对不断变更的系统需求达成并保持一致的过程。
    在需求管理中，每个需求被赋子唯一的标识符，一旦标识出需求，就可以为需求建立跟踪表，每个跟踪表标识需求与其他需求或设计文档、代码、测试用例的不同版本问的关系。例如，特征跟踪表，记录需求如何与产品或系统特征相关联:来源跟踪表，记录每个需求的来源；依赖跟踪表，描述需求间如何关联等。
    这些跟踪表可以用于需求跟踪。在整个开发过程中，进行需求跟踪的目的是为了建立和维护从用户需求开始到测试之间的一致性与完整性，确保所有的实现是以用户需求为基础，所有的输出符合用户需求，并且全面覆盖了用户需求。需求跟踪有两种方式:正向跟踪和逆向跟踪。其中，正向跟踪以用户需求为切入点，检查《需求规约》中的每个需求是否都能在后继工作产品中找到对应点:道向跟踪检查设计文档、代码、测试用例等工作产品是否都能在《需求规约》中找到出处。

# 复述展开

需求分析，是软件工程领域的一个重要的过程。主要是对潜在或者非潜在的用户进行调查，并将需求使用文档化的方式，进行落地成文，最终将这个需求转化为软件的功能需求、非功能性需求等等。需求分析是软件开发的前置要求，在需求分析阶段完成的比较充分，是保证后续软件开发能够有效进行的前提。

需求工程，还是【XX 工程】的形式，也就是使用计算机原理等工程化的方式，将需求分析这个过程系统化、工程化。对应软件工程，需求工程，也就是需求定义、文档记录、需求演进、需求迭代的过程。

# 理解体会

在我的从业生涯中，一般是由产品来完成需求分析的过程，产品同学需要通过调查、 研究市场上的潜在用户和已有用户的数据分析，从而对软件产品进行功能的更新迭代，这个过程就是软件行业中的需求分析，一般最后会转化为产品需求文档，这里一般是原型文档。在产品出完需求的原型文档后，开发可以开始介入，根据所在团队的技术栈和基础架构情况，进行技术方案的设计，随即会出概要设计文档和详细设计文档。开发同学和产品同学可以随时沟通讨论需求的可行性以及后续的迭代过程，当然此时测试同学也可以从测试角度提出自己的一些建议和指导方案。

一般，对齐需求是非常关键的步骤，一个好的需求分析过程能够让产品和开发同学有序进行。

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
