---
password: ""
icon: ""
date: "2023-10-06"
type: Post
category: 行业概念
slug: industry-day15
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
  - 必看精选
summary: ""
title: Day15【概念解析】排序算法
status: Published
cover: "https://www.notion.so/images/page-cover/woodcuts_4.jpg"
urlname: c95a471b-d304-47cc-938c-c918d17ba275
updated: "2023-11-09 11:45:00"
---

# 整理定义

| 来源                                                                                                      | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&ID=95394&Type=bkzyb&SubID=81669)                | 排序（sorting）是计算机程序设计中的一种重要操作，它要求将一个数据元素的任意序列，重新排列成一个有序的序列。排序算法（sorting algorithm）将任意顺序的数据元素重新排列成一个有序的序列。                                                                                                                                                                                                                                                                                                                                                                                                                 |
| [百度百科](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95?fromModule=lemma_search-box) | 所谓排序，就是使一串[记录](https://baike.baidu.com/item/%E8%AE%B0%E5%BD%95/1837758?fromModule=lemma_inlink)，按照其中的某个或某些[关键字](https://baike.baidu.com/item/%E5%85%B3%E9%94%AE%E5%AD%97/7105697?fromModule=lemma_inlink)的大小，递增或递减的排列起来的[操作](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C/5797370?fromModule=lemma_inlink)。排序算法，就是如何使得记录按照要求排列的方法。排序算法在很多领域得到相当地重视，尤其是在大量数据的处理方面。一个优秀的算法可以节省大量的资源。在各个领域中考虑到数据的各种限制和规范，要得到一个符合实际的优秀算法，得经过大量的推理和分析。 |
| [维基百科](https://en.wikipedia.org/wiki/Sorting_algorithm)                                               | 在[计算机科学](https://en.wikipedia.org/wiki/Computer_science)中，**排序**算法是一种将[列表](<https://en.wikipedia.org/wiki/List_(computing)>)元素按[顺序](https://en.wikipedia.org/wiki/Total_order)排列[的算法](https://en.wikipedia.org/wiki/Algorithm)。                                                                                                                                                                                                                                                                                                                                           |

# 复述展开

### 算法对比分析

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/8a6e5f2c-bc22-427a-8c0c-270583e6b5e1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120604Z&X-Amz-Expires=3600&X-Amz-Signature=9e04acdda8796562180a03aea6c94202a0ec402742f71bfd73adcf023216c9c9&X-Amz-SignedHeaders=host&x-id=GetObject)

常见的**快速排序**、**归并排序**、**堆排序**以及**冒泡排序**等都属于**比较类排序算法**。比较类排序是通过比较来决定元素间的相对次序，由于其时间复杂度不能突破  `O(nlogn)`，因此也称为非线性时间比较类排序。在冒泡排序之类的排序中，问题规模为  `n`，又因为需要比较  `n`  次，所以平均时间复杂度为  `O(n²)`。在**归并排序**、**快速排序**之类的排序中，问题规模通过**分治法**消减为  `logn`  次，所以时间复杂度平均  `O(nlogn)`。

比较类排序的优势是，适用于各种规模的数据，也不在乎数据的分布，都能进行排序。可以说，比较排序适用于一切需要排序的情况。

而**计数排序**、**基数排序**、**桶排序**则属于**非比较类排序算法**。非比较排序不通过比较来决定元素间的相对次序，而是通过确定每个元素之前，应该有多少个元素来排序。由于它可以突破基于比较排序的时间下界，以线性时间运行，因此称为线性时间非比较类排序。 非比较排序只要确定每个元素之前的已有的元素个数即可，所有一次遍历即可解决。算法时间复杂度  `O(n)`。

非比较排序时间复杂度底，但由于非比较排序需要占用空间来确定唯一位置。所以对数据规模和数据分布有一定的要求。

### 算法对比总结

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/4934b35f-066c-4f25-9979-8b47d318f693/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120604Z&X-Amz-Expires=3600&X-Amz-Signature=4bea38a581869051558a680090e9d15f4121a68313c8193c42cc6b8005eceea5&X-Amz-SignedHeaders=host&x-id=GetObject)

说明：

1. 稳定的排序算法按照元素在输入中出现的相同顺序对相等的元素进行排序。如果一个算法具备稳定性，那么在排序之后，如果两个元素相等，排序结果能够**相同元素的原始的排列顺序**。
2. 排序方式中，in-place 表示可以在已有空间中使用，out-place 表示需要额外的空间进行存储中间数据。
<details>
<summary>→ **十大排序算法超级链接 →**</summary>

[bookmark](https://kuangyichen.com/article/industry-day16)

[bookmark](https://kuangyichen.com/article/industry-day17)

[bookmark](https://kuangyichen.com/article/industry-day18)

[bookmark](https://kuangyichen.com/article/industry-day19)

[bookmark](https://kuangyichen.com/article/industry-day20)

[bookmark](https://kuangyichen.com/article/industry-day21)

[bookmark](https://kuangyichen.com/article/industry-day22)

[bookmark](https://kuangyichen.com/article/industry-day23)

[bookmark](https://kuangyichen.com/article/industry-day24)

[bookmark](https://kuangyichen.com/article/industry-day25)

</details>

# 理解体会

1. 排序算法可以算是计算机科学与技术的基础入门知识，也是在面试中常见的提醒。掌握十大排序算法可以说是面试做题的基础的基础了。在掌握算法各大原理之余，还需要掌握各个算法之间的时间复杂度以及空间复杂度，以及各个算法的排序方式和稳定性。可以在算法对比总结表中查看。
2. 面试过程中，很多时候，简单的算法是决定面试结果的重要因素。试想，如果练如此基础的算法都无法掌握的话，那如何去使用和改进更复杂的算法呢？所以，在学习算法过程中，还需要自己实地去使用代码编写逻辑，知道原理，也知道如果用代码解决算法问题。

## 排序算法总结

    [bookmark](https://kuangyichen.com/article/industry-day15)


    [bookmark](https://kuangyichen.com/article/industry-day16)


    [bookmark](https://kuangyichen.com/article/industry-day17)


    [bookmark](https://kuangyichen.com/article/industry-day18)


    [bookmark](https://kuangyichen.com/article/industry-day19)


    [bookmark](https://kuangyichen.com/article/industry-day20)


    [bookmark](https://kuangyichen.com/article/industry-day21)


    [bookmark](https://kuangyichen.com/article/industry-day22)


    [bookmark](https://kuangyichen.com/article/industry-day23)


    [bookmark](https://kuangyichen.com/article/industry-day24)


    [bookmark](https://kuangyichen.com/article/industry-day25)

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

# 附录

常见排序算法（Python 题解说明）

[embed](https://gist.github.com/EachenKuang/0fa2846430231e774060445398d6b2dc)
