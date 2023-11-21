---
password: ""
icon: ""
date: "2023-10-07"
type: Post
category: 行业概念
slug: industry-day16
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day16【概念解析】冒泡排序
status: Published
cover: "https://images.unsplash.com/photo-1612855190135-bdbacda2a2b5?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: a0d4b94c-8e73-4eb0-872b-4a76f3fc18cf
updated: "2023-10-27 19:23:00"
---

# 整理定义

中文名称：冒泡排序

英文名称：bubble sort

| 出处                                                                                                                                                                                                                                                                                         | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&ID=95344&Type=bkzyb&SubID=81669)                                                                                                                                                                                                   | 冒泡排序：一种典型的交换排序算法，通过交换数据元素的位置进行排序。算法重复地遍历需要排序的元素列，依次比较两个相邻的元素，如果它们的顺序（如从大到小、首字母从 A 到 Z）为逆序就把它们交换过来。比较元素的操作重复进行，直到没有相邻元素需要交换为止。                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [维基百科](https://en.wikipedia.org/wiki/Bubble_sort)                                                                                                                                                                                                                                        | **Bubble sort**, sometimes referred to as **sinking sort**, is a simple [sorting algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm) that repeatedly steps through the input list element by element, comparing the current element with the one after it, [swapping](<https://en.wikipedia.org/wiki/Swap_(computer_science)>) their values if needed. These passes through the list are repeated until no swaps had to be performed during a pass, meaning that the list has become fully sorted. The algorithm, which is a [comparison sort](https://en.wikipedia.org/wiki/Comparison_sort), is named for the way the larger elements "bubble" up to the top of the list. |
| 【冒泡排序（有时称为下沉排序）是一种简单的排序算法，它逐个元素地重复遍历输入列表，将当前元素与其后面的元素进行比较，并在需要时交换它们的值。 重复对列表的这些遍历，直到在遍历期间不必执行交换，这意味着列表已完全排序。 该算法是一种比较排序，因其较大的元素“冒泡”到列表顶部的方式而得名。】 |
| [百度百科](https://baike.baidu.com/item/%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F?fromModule=lemma_search-box)                                                                                                                                                                                    | 冒泡排序（Bubble Sort），是一种计算机科学领域的较简单的排序算法。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

它重复地走访过要排序的元素列，依次比较两个相邻的元素，如果顺序（如从大到小、首字母从 Z 到 A）错误就把他们交换过来。走访元素的工作是重复地进行，直到没有相邻元素需要交换，也就是说该元素列已经排序完成。
这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端（升序或降序排列），就如同碳酸饮料中二氧化碳的气泡最终会上浮到顶端一样，故名“冒泡排序”。 |

# 复述展开

**冒泡排序 (Bubble Sort)**
冒泡排序是一种简单的排序算法。它重复地遍历要排序的序列，依次比较两个元素，如果它们的顺序错误就把它们交换过来。
遍历序列的工作是重复地进行直到没有再需要交换为止，此时说明该序列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢 “浮” 到数列的顶端。

> 📌 **算法步骤**
>
> 1. 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
>
> 2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
>
> 3. 针对所有的元素重复以上的步骤，除了最后一个；
>
> 4. 重复步骤 1~3，直到排序完成。
>
> **此处对代码做了一个小优化，加入了 is_sorted Flag，目的是将算法的最佳时间复杂度优化为 O(n)，即当原输入序列就是排序好的情况下，该算法的时间复杂度就是 O(n)。**

    1. 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
    2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
    3. 针对所有的元素重复以上的步骤，除了最后一个；
    4. 重复步骤 1~3，直到排序完成。

    **此处对代码做了一个小优化，加入了 is_sorted Flag，目的是将算法的最佳时间复杂度优化为 O(n)，即当原输入序列就是排序好的情况下，该算法的时间复杂度就是 O(n)。**

> 📌 **算法分析**  
> 稳定性：稳定  
> 时间复杂度 ：最佳：O(n) ，最差：O(n^2)， 平均：O(n^2)  
> 空间复杂度 ：O(1)  
> 排序方式 ：In-place

    稳定性：稳定
    时间复杂度 ：最佳：O(n) ，最差：O(n^2)， 平均：O(n^2)
    空间复杂度 ：O(1)
    排序方式 ：In-place

**冒泡算法示意图**

![%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F_%E8%BD%BB%E5%BA%A6.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/875e02b5-c61a-4366-856f-700a0817104d/%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F_%E8%BD%BB%E5%BA%A6.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120557Z&X-Amz-Expires=3600&X-Amz-Signature=6be55ec47c662e82c987153dab6e2c47973a37f662f6d50deb3b8d714cf44f0d&X-Amz-SignedHeaders=host&x-id=GetObject)

<details>
<summary>**代码展示**</summary>

```python
def bubble_sort(nums: List[int]) -> List[int]:
    """
    冒泡排序 (Bubble Sort)
    冒泡排序是一种简单的排序算法。它重复地遍历要排序的序列，依次比较两个元素，如果它们的顺序错误就把它们交换过来。
    遍历序列的工作是重复地进行直到没有再需要交换为止，此时说明该序列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢 “浮” 到数列的顶端。

    算法步骤
    1.比较相邻的元素。如果第一个比第二个大，就交换它们两个；
    2.对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
    3.针对所有的元素重复以上的步骤，除了最后一个；
    4.重复步骤 1~3，直到排序完成。

    此处对代码做了一个小优化，加入了 is_sorted Flag，目的是将算法的最佳时间复杂度优化为 O(n)，
    即当原输入序列就是排序好的情况下，该算法的时间复杂度就是 O(n)。

    算法分析
    稳定性：稳定
    时间复杂度 ：最佳：O(n) ，最差：O(n^2)， 平均：O(n^2)
    空间复杂度 ：O(1)
    排序方式 ：In-place
    """
    length = len(nums)
    for i in range(length):
        is_sorted = True
        for j in range(0, length - 1 - i):
            if nums[j] > nums[j + 1]:
                # 交换相邻的位置
                swap(nums, j, j + 1)
                is_sorted = False
        # 如果没有交换，说明整体的顺序是排好序的，可以直接跳出循环
        if is_sorted:
            break
    return nums
```

</details>

# 理解体会

1、冒泡排序作为简单排序算法，顾名思义，就是通过比较两两之间元素的大小，通过冒泡的方式将最大的沉底，所以也叫做沉底排序，然后经过 N-1 次冒泡，达到所有元素都排好序。

2、在写概念的过程中，发现如果使用 GIF 图来描述冒泡的过程会比较清晰，所以学习了如何制作 GIF 图。这个过程中，使用了【[截图](https://apps.apple.com/cn/app/%E6%88%AA%E5%9B%BE-jietu-%E5%BF%AB%E9%80%9F%E6%A0%87%E6%B3%A8-%E4%BE%BF%E6%8D%B7%E5%88%86%E4%BA%AB%E7%9A%84%E6%88%AA%E5%B1%8F%E5%B7%A5%E5%85%B7/id1059334054?mt=12%3Fmt%3D12)】软件来实现。有个排序的可视化网站，可以在上面设置模型的长度、排序类型就可以看到可视化的动态排序过程。

[bookmark](https://visualgo.net/zh/sorting)

通过使用截图软件进行视频录制，然后导出成 GIF 图即可。

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
