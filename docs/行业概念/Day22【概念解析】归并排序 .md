---
password: ""
icon: ""
date: "2023-10-13"
type: Post
category: 行业概念
slug: industry-day22
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: "Day22【概念解析】归并排序 "
status: Published
cover: "https://www.notion.so/images/page-cover/woodcuts_15.jpg"
urlname: 5c3eaa22-d890-40db-b60b-722d40c42cf2
updated: "2023-10-27 19:24:00"
---

# 整理定义

| 出处                                                                                                                                                                                                                                                                                                                                 | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&ID=375957&Type=bkzyb&SubID=81669)                                                                                                                                                                                                                                          | 建立在归并操作上的一种有效的排序算法，此算法采用分治法实现序列的排序。又称归并排序。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 百度百科                                                                                                                                                                                                                                                                                                                             | **归并排序**是建立在归并操作上的一种有效，稳定的[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605?fromModule=lemma_inlink)，该算法是采用[分治法](https://baike.baidu.com/item/%E5%88%86%E6%B2%BB%E6%B3%95/2407337?fromModule=lemma_inlink)（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为[二路归并](https://baike.baidu.com/item/%E4%BA%8C%E8%B7%AF%E5%BD%92%E5%B9%B6/53201558?fromModule=lemma_inlink)。                                                                                                                                                                                                                                                                                                                                                                                                         |
| [维基百科](https://en.wikipedia.org/wiki/Merge_sort)                                                                                                                                                                                                                                                                                 | In [computer science](https://en.wikipedia.org/wiki/Computer_science), **merge sort** (also commonly spelled as **mergesort**) is an efficient, general-purpose, and [comparison-based](https://en.wikipedia.org/wiki/Comparison_sort) [sorting algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm). Most implementations produce a [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability), which means that the relative order of equal elements is the same in the input and output. Merge sort is a [divide-and-conquer algorithm](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm) that was invented by [John von Neumann](https://en.wikipedia.org/wiki/John_von_Neumann) in 1945.[[2]](https://en.wikipedia.org/wiki/Merge_sort#cite_note-2) A detailed description and analysis of bottom-up merge sort appeared in a report by [Goldstine](https://en.wikipedia.org/wiki/Herman_Goldstine) and von Neumann as early as 1948. |
| 【在计算机科学中，合并排序（通常也拼写为 mergesort）是一种高效、通用且基于比较的排序算法。 大多数实现都会产生稳定的排序，这意味着相等元素的相对顺序在输入和输出中是相同的。 归并排序是一种分治算法，由约翰·冯·诺依曼于 1945 年发明。[2] 早在 1948 年 Goldstine 和 von Neumann 的报告中就出现了对自下而上归并排序的详细描述和分析。】 |

# 复述展开

归并排序，又称为合并排序，采用分治法进行序列排序，是一种稳定性的排序算法，算法复杂度为 O(nlogn)。

代码演示：

```python
def merge_sort(nums):
    """
    归并排序 (Merge Sort)
    归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法 (Divide and Conquer) 的一个非常典型的应用。归并排序是一种稳定的排序方法。
    将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为 2 - 路归并。

    和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是 O(nlogn) 的时间复杂度。代价是需要额外的内存空间。

    算法步骤
    归并排序算法是一个递归过程，边界条件为当输入序列仅有一个元素时，直接返回，具体过程如下：
    1.如果输入内只有一个元素，则直接返回，否则将长度为 n 的输入序列分成两个长度为 n/2 的子序列；
    2.分别对这两个子序列进行归并排序，使子序列变为有序状态；
    3.设定两个指针，分别指向两个已经排序子序列的起始位置；
    4.比较两个指针所指向的元素，选择相对小的元素放入到合并空间（用于存放排序结果），并移动指针到下一位置；
    5.重复步骤 3 ~4 直到某一指针达到序列尾；
    6.将另一序列剩下的所有元素直接复制到合并序列尾。

    算法分析
    稳定性：稳定
    时间复杂度 ：最佳：O(nlogn)， 最差：O(nlogn)， 平均：O(nlogn)
    空间复杂度 ：O(n)
    """

    def merge(nums_a: List[int], nums_b: List[int]) -> List[int]:
        ans = []
        i = 0
        j = 0
        while i < len(nums_a) and j < len(nums_b):
            if nums_a[i] <= nums_b[j]:
                ans.append(nums_a[i])
                i += 1
            else:
                ans.append(nums_b[j])
                j += 1
        ans.extend(nums_a[i:])
        ans.extend(nums_b[j:])
        return ans

    length = len(nums)
    if length <= 1:
        return nums
    middle = length // 2
    left = merge_sort(nums[:middle])
    right = merge_sort(nums[middle:])
    return merge(left, right)
```

动图演示：

![Merge-sort-example-300px.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/f20cc104-b3af-4f7f-ad73-df3798368dfb/Merge-sort-example-300px.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120538Z&X-Amz-Expires=3600&X-Amz-Signature=7ecea9287cdb26f88fd21b38e7424c81a29622f2421a601da103c07ef606c72d&X-Amz-SignedHeaders=host&x-id=GetObject)

# 理解体会

归并排序是一种分治算法，其基本思想是将两个或两个以上的有序表合并成一个新的有序表。具体步骤如下：

1. 分解：将当前区间一分为二，递归地对它们进行分解，直到分解到一个元素为止。
2. 合并：将分解出的数列合并，合并过程为：将两个有序数列合并成一个有序数列，我们称之为二路归并。

特点：

1. 稳定性：归并排序是一种稳定的排序方法，即相等的元素的顺序不会改变。
2. 非原地排序：归并排序需要额外的空间来存储元素，因此空间复杂度为 O(n)。

优点：

1. 对于大规模数据的排序，归并排序是非常有效的。因为其时间复杂度为 O(nlogn)，这在所有的时间复杂度为 O(nlogn)的排序算法中，性能表现最稳定。
2. 归并排序适合对链表进行排序，因为链表的节点分布可以看作是随机的，归并排序只需要重新排列链表节点的指针即可。

缺点：

1. 需要额外的存储空间，空间复杂度为 O(n)。
2. 对于小规模数据，插入排序等简单排序算法可能更有效。

使用场景：

1. 处理大规模数据：归并排序适合处理大规模数据，因为其时间复杂度为 O(nlogn)。
2. 链表排序：归并排序是链表排序的最佳选择，因为它不需要随机访问元素。

对比同类算法：

1. 快速排序：快速排序在最坏情况下的时间复杂度为 O(n^2)，而归并排序在所有情况下的时间复杂度都是 O(nlogn)。但是，快速排序是原地排序，不需要额外的存储空间，而归并排序需要。
2. 堆排序：堆排序和归并排序的时间复杂度都是 O(nlogn)，但堆排序是原地排序算法，而归并排序不是。然而，堆排序不是稳定的排序算法，而归并排序是。

总结：
归并排序是一种高效、稳定、但非原地的排序算法。它对处理大规模数据和链表排序非常有效。但是，由于其需要额外的存储空间，对于存储空间有限或者数据规模较小的情况，可能不如其他排序算法有效。

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
