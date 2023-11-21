---
password: ""
icon: ""
date: "2023-10-10"
type: Post
category: 行业概念
slug: industry-day19
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day19 【概念解析】选择排序
status: Published
cover: "https://images.unsplash.com/photo-1600074169098-16a54d791d0d?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 38552ee5-4e35-4caa-a61c-ebb87296bf05
updated: "2023-10-27 19:23:00"
---

# 整理定义

| 出处                                                                                       | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&ID=95375&Type=bkzyb&SubID=81669) | 一种简单直观的排序算法。它的工作原理是每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，直到全部待排序的数据元素有序。                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| [百度百科](https://baike.baidu.com/item/%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F/9762418)      | 选择排序（Selection sort）是一种简单直观的[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605?fromModule=lemma_inlink)。它的工作原理是：第一次从待排序的[数据元素](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%85%83%E7%B4%A0/715313?fromModule=lemma_inlink)中选出最小（或最大）的一个元素，存放在序列的起始位置，然后再从剩余的未排序元素中寻找到最小（大）元素，然后放到已排序的序列的末尾。以此类推，直到全部待排序的数据元素的个数为零。选择[排序](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F/1066239?fromModule=lemma_inlink)是不稳定的排序方法。 |

# 复述展开

**选择排序（selection sort）**

- 稳定性：不稳定
- 时间复杂度：O(n^2)
- 空间复杂度：O(1)
- 排序方式：in-place

选择排序（Selection Sort）是一种简单直观的排序算法。它的基本思想是每次从待排序的元素中选择最小（或最大）的元素，放到已排序序列的末尾，直到所有元素都排序完成。

选择排序的步骤如下：

1. 遍历待排序序列，将第一个元素设为当前最小（或最大）元素。
2. 从剩余的未排序元素中找到最小（或最大）的元素，与当前最小（或最大）元素进行交换。
3. 将当前最小（或最大）元素放到已排序序列的末尾。
4. 重复步骤 2 和步骤 3，直到所有元素都排序完成。

选择排序的特点是每次选择最小（或最大）元素放到已排序序列的末尾，因此每一轮选择排序都会确定一个元素的最终位置。选择排序的时间复杂度为 O(n^2)，其中 n 是待排序序列的长度。尽管选择排序的时间复杂度较高，但它的实现简单，对于小规模的数据集仍然是一个有效的排序算法。

需要注意的是，选择排序是一种不稳定的排序算法，即相等元素的相对顺序可能会发生改变。如果需要稳定的排序算法，可以考虑其他算法，如插入排序或归并排序。

**动态图展示**

![%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F_%E5%8E%8B%E7%BC%A9.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/a7b6d6e6-20ca-443d-9ddb-822e6dc636ab/%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F_%E5%8E%8B%E7%BC%A9.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120547Z&X-Amz-Expires=3600&X-Amz-Signature=6913ec366a6ef5c7f8f578331a692f0b193272b8302e608cda2f1dc3b1b1e58a&X-Amz-SignedHeaders=host&x-id=GetObject)

**代码演示**

```python
class SelectionSort:
    def selection_sort(self, nums):
        for i in range(len(nums)-1):
            min_index = i
            for j in range(i+1, len(nums)):
                # 寻找 i~n 中最小的数，然后放置到 i
                if nums[min_index] > nums[j]:
                    min_index = j
            # 交换
            if min_index != i:
                nums[min_index], nums[i] = nums[i], nums[min_index]
        return nums


if __name__ == '__main__':
    nums = [10, 13, 2, 33, 3, 4, 4, 7, 9]
    solution = SelectSort()
    print(solution.select_sort(nums))
```

# 理解体会

首先，选择排序是一种简单但有效的排序算法。它的基本思想是每次从待排序的元素中选择最小（或最大）的元素，将其放置在已排序序列的末尾，逐步构建有序序列。选择排序的过程可以分为两个子操作：选择和交换。

选择排序的优点是实现简单，代码量少，不需要额外的空间。它适用于小规模的数据集，对于简单的排序任务是一个有效的选择。此外，选择排序是一种不稳定的排序算法。当存在相等元素时，选择排序可能会改变它们的相对顺序。这是因为选择排序每次选择最小（或最大）元素进行交换，可能会导致相等元素的位置发生变化。

总结来说，选择排序是一种简单但有效的排序算法。它通过选择和交换操作逐步构建有序序列。尽管选择排序的时间复杂度较高且不稳定，但在小规模数据集上仍然是一个可行的排序算法。对于理解排序算法的基本原理和实现细节，选择排序是一个很好的起点。

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
