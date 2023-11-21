---
password: ""
icon: ""
date: "2023-10-09"
type: Post
category: 行业概念
slug: industry-day18
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day18【概念解析】插入排序
status: Published
cover: "https://images.unsplash.com/photo-1544383835-bda2bc66a55d?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: f1bbd1b1-e283-40f1-b1de-5ed1007f13ca
updated: "2023-10-27 19:23:00"
---

# 整理定义

中文名称：插入排序

英文名称：insertion sort

| 出处                                                                                                                                                      | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&ID=95366&Type=bkzyb&SubID=81669)                                                                | 一种简单的排序算法。算法将待排序的数据分成两个区域：有序区和无序区，每次将一个无序区的数据按其大小插入到有序区中的适当位置，直到所有无序区中的数据都插入完成为止                                                                                                                                                                                                                                                                                                                                                                       |
| [Insertion sort - Wikipedia](https://en.wikipedia.org/wiki/Insertion_sort)                                                                                | **Insertion sort** is a simple [sorting algorithm](https://en.wikipedia.org/wiki/Sorting_algorithm) that builds the final [sorted array](https://en.wikipedia.org/wiki/Sorted_array) (or list) one item at a time [by comparisons](https://en.wikipedia.org/wiki/Comparison_sort). It is much less efficient on large lists than more advanced algorithms such as [quicksort](https://en.wikipedia.org/wiki/Quicksort), [heapsort](https://en.wikipedia.org/wiki/Heapsort), or [merge sort](https://en.wikipedia.org/wiki/Merge_sort). |
| 【插入排序是一种简单的排序算法，通过比较一次构建最终的排序数组（或列表）。 它在大型列表上的效率比更高级的算法（例如快速排序、堆排序或合并排序）低得多。】 |
| [插入排序\_百度百科 (baidu.com)](https://baike.baidu.com/item/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F?fromModule=lemma_search-box)                           | 插入排序，一般也被称为直接插入排序。对于少量元素的排序，它是一个有效的算法  [1]  。插入排序是一种最简单的[排序](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F/1066239?fromModule=lemma_inlink)方法，它的基本思想是将一个记录插入到已经排好序的有序表中，从而一个新的、记录数增 1 的有序表。在其实现过程使用双层循环，外层循环对除了第一个元素之外的所有元素，内层循环对当前元素前面有序表进行待插入位置查找，并进行移动                                                                                                              |

# 复述展开

**插入排序 (Insertion Sort)**

![%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F_%E5%8E%8B%E7%BC%A9.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/d6d0b359-962f-4af2-b0e4-abcd4de25827/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F_%E5%8E%8B%E7%BC%A9.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120551Z&X-Amz-Expires=3600&X-Amz-Signature=4f4fa8869a8a8310fd8e3f17d8fb9fe658dfdbb69f844bcd466c0049a143fea4&X-Amz-SignedHeaders=host&x-id=GetObject)

> 📌 算法步骤  
> 1.从第一个元素开始，该元素可以认为已经被排序；  
> 2.取出下一个元素，在已经排序的元素序列中从后向前扫描；  
> 3.如果该元素（已排序）大于新元素，将该元素移到下一位置；  
> 4.重复步骤 3，直到找到已排序的元素小于或者等于新元素的位置；  
> 5.将新元素插入到该位置后；  
> 6.重复步骤 2~5。

> 📌 算法分析  
> 稳定性：稳定  
> 时间复杂度 ：最佳：O(n) ，最差：O(n^2)， 平均：O(n^2)  
> 空间复杂度 ：O(1)  
> 排序方式 ：In-place

<details>
<summary>代码分析</summary>

```python
class InsertionSort:
    def insertion_sort(self, nums):
        for i in range(1, len(nums)):
            current = i - 1
            insert_value = nums[i]
            while current >= 0 and insert_value < nums[current]:
                # 调整需要插入的值，找到需要插入的位置，其到 current ~ i 统一右移
                nums[current + 1] = nums[current]
                current -= 1
            nums[current + 1] = insert_value
        return nums


if __name__ == '__main__':
    nums = [1, 1, 2, 3, 3, 4, 4, 7, 9]
    solution = InsertionSort()
    print(solution.insertion_sort(nums))
```

</details>

# 理解体会

插入排序是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

插入排序在实现上，通常采用 in-place 排序（即只需用到 O(1) 的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。

插入排序的代码实现虽然没有冒泡排序和选择排序那么简单粗暴，但它的原理应该是最容易理解的了，因为只要打过扑克牌的人都应该能够秒懂。

插入排序是一种最简单直观的排序算法，它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

插入排序和冒泡排序一样，也有一种优化算法，叫做拆半插入。

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
