---
password: ""
icon: ""
date: "2023-10-11"
type: Post
category: 行业概念
slug: industry-day20
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day20【概念解析】希尔排序
status: Published
cover: "https://www.notion.so/images/page-cover/met_the_unicorn_in_captivity.jpg"
urlname: 56725f3d-b4b1-4e14-8253-d6b580ba850f
updated: "2023-10-27 19:23:00"
---

# 整理定义

中文名称：希尔排序

英文名称：shellsort

| 出处                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&ID=375969&Type=bkzyb&SubID=81669)                                                                                                                                                                                                                                                                                                                                                                                                | 一种插入排序的改进算法。算法开始排序的元素间隔较大，然后逐步缩小要比较元素之间的间隔                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| [百度百科](https://baike.baidu.com/item/%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F?fromModule=lemma_search-box)                                                                                                                                                                                                                                                                                                                                                                                  | 希尔排序(Shell's Sort)是插入排序的一种又称“缩小增量排序”（Diminishing Increment Sort），是直接插入排序算法的一种更高效的改进版本。希尔排序是非稳定排序算法。该方法因 D.L.Shell 于 1959 年提出而得名。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至 1 时，整个文件恰被分成一组，算法便终止。                                                                                                                                                                                                                                                                                                                      |
| [维基百科](https://en.wikipedia.org/wiki/Shellsort)                                                                                                                                                                                                                                                                                                                                                                                                                                        | **Shellsort**, also known as **Shell sort** or **Shell's method**, is an [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) [comparison sort](https://en.wikipedia.org/wiki/Comparison_sort). It can be seen as either a generalization of sorting by exchange ([bubble sort](https://en.wikipedia.org/wiki/Bubble_sort)) or sorting by insertion ([insertion sort](https://en.wikipedia.org/wiki/Insertion_sort)).[[3]](https://en.wikipedia.org/wiki/Shellsort#cite_note-Knuth-3) The method starts by sorting pairs of elements far apart from each other, then progressively reducing the gap between elements to be compared. By starting with far apart elements, it can move some out-of-place elements into position faster than a simple nearest neighbor exchange. [Donald Shell](https://en.wikipedia.org/wiki/Donald_Shell) published the first version of this sort in 1959.[[4]](https://en.wikipedia.org/wiki/Shellsort#cite_note-Shell-4)[[5]](https://en.wikipedia.org/wiki/Shellsort#cite_note-5) The running time of Shellsort is heavily dependent on the gap sequence it uses. For many practical variants, determining their [time complexity](https://en.wikipedia.org/wiki/Time_complexity) remains an [open problem](https://en.wikipedia.org/wiki/Open_problem). |
| 【希尔排序（Shellsort），也称为希尔排序或希尔法，是一种就地比较排序。 它可以看作是交换排序（冒泡排序）或插入排序（插入排序）的推广。 [3] 该方法首先对彼此相距较远的元素对进行排序，然后逐渐减小要比较的元素之间的差距。 通过从相距较远的元素开始，它可以比简单的最近邻居交换更快地将一些不合适的元素移动到位。 Donald Shell 于 1959 年发布了此类的第一个版本。[4][5] Shellsort 的运行时间很大程度上取决于它使用的间隙序列。 对于许多实际变体，确定其时间复杂度仍然是一个悬而未决的问题。】 |

# 复述展开

**希尔排序 (Shell Sort)**
希尔排序是希尔 (Donald Shell) 于 1959 年提出的一种排序算法。希尔排序也是一种插入排序，它是简单插入排序经过改进之后的一个更高效的版本，
也称为递减增量排序算法，同时该算法是冲破 O(n²) 的第一批算法之一。

希尔排序的基本思想是：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录 “基本有序” 时，再对全体记录进行依次直接插入排序。

**算法步骤**
我们来看下希尔排序的基本步骤，在此我们选择增量 gap=length/2，缩小增量继续以 gap = gap/2 的方式，这种增量选择我们可以用一个序列来表示，{n/2, (n/2)/2, ..., 1}，称为增量序列。希尔排序的增量序列的选择与证明是个数学难题，我们选择的这个增量序列是比较常用的，也是希尔建议的增量，称为希尔增量，但其实这个增量序列不是最优的。此处我们做示例使用希尔增量。

先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述： 1.选择一个增量序列 {t1, t2, …, tk}，其中 (ti>tj, i<j, tk=1)； 2.按增量序列个数 k，对序列进行 k 趟排序； 3.每趟排序，根据对应的增量 t，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

**算法分析**

- 稳定性：不稳定
- 时间复杂度 ：最佳：O(nlogn)， 最差：O(n2) 平均：O(n^1.3)
- 空间复杂度 ：O(n)

```python
class ShellSort:
    def shell_sort(self, nums):
        n = len(nums)
        gap = n // 2
        while gap > 0:
            for i in range(gap, n):
                current = i - gap
                insert_value = nums[i]
                while current >= 0 and insert_value < nums[current]:
                    # 调整需要插入的值，找到需要插入的位置，其到 current ~ i 统一右移
                    nums[current + gap] = nums[current]
                    current -= gap
                nums[current + gap] = insert_value
            gap //= 2
        return nums


if __name__ == '__main__':
    nums = [10, 13, 2, 33, 3, 4, 4, 7, 9]
    solution = ShellSort()
    print(solution.shell_sort(nums))
```

# 理解体会

希尔排序（Shell Sort）是一种基于插入排序的算法，由 Donald Shell 于 1959 年提出，因此得名。插入排序在几乎已经排序的数据集上表现良好，但在大规模乱序数据集上效率较低。希尔排序通过对数据进行预排序，使得插入排序可以工作在"几乎排序"的数据集上，从而提高效率。

**算法概述**

希尔排序的基本思想是将待排序的数据集分成多个子序列，然后对每个子序列进行插入排序。这些子序列是通过选择一个增量（通常是数据集大小的一半）来创建的，每个子序列包含原始数据集中相隔增量距离的元素。然后，增量被减半，重复上述过程，直到增量为 1，此时进行一次完整的插入排序。

**评价**

希尔排序的性能取决于增量序列的选择。在最坏的情况下，希尔排序的时间复杂度可以达到 O(n^2)，但是对于某些增量序列，时间复杂度可以降低到 O(n^(3/2))，甚至更低。希尔排序是一种原地排序算法，不需要额外的存储空间，空间复杂度为 O(1)。然而，希尔排序不是稳定的排序算法，因为相等的元素可能会因为分组而改变相对顺序。

**使用场景**

希尔排序适用于中等大小的数据集。由于其相对简单的实现和良好的平均性能，它在一些需要快速原地排序且不需要稳定性的场景中是一个很好的选择。例如，嵌入式系统和老旧系统，这些系统可能没有足够的资源来执行更复杂的排序算法。

总的来说，希尔排序是一种有效的排序算法，尤其适用于中等大小的数据集。虽然它在最坏的情况下可能不如其他更复杂的排序算法，但是由于其简单的实现和在平均情况下的良好性能，它仍然是一种有用的排序工具。

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
