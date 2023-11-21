---
password: ""
icon: ""
date: "2023-10-08"
type: Post
category: 行业概念
slug: industry-day17
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day17【概念解析】快速排序
status: Published
cover: "https://www.notion.so/images/page-cover/met_william_turner_1835.jpg"
urlname: 08ccfe77-d87f-4674-b6bb-101e5b30a91b
updated: "2023-10-27 19:23:00"
---

# 整理定义

中文名称：快速排序

英文名称：quicksort

| 出处                                                                                                                                                            | 定义                                                                                                                                                      |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [中国大百科全书](https://www.zgbk.com/ecph/words?SiteID=1&Name=%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F&Type=bkzyb&subSourceType=000003000007000014&SourceID=95394) | 一种排序算法。任选一个元素作为主元（pivot），把其余元素与主元比较后分成大小两个部分，再对每个部分继续如法炮制，直到全部元素排好序为止。又称分区交换排序。 |
| [维基百科](https://en.wikipedia.org/wiki/Quicksort)                                                                                                             | **快速排序是一种高效且通用的分治排序算法。**                                                                                                              |

Quicksort is an efficient, general-purpose sorting algorithm. Quicksort was developed by British computer scientist Tony Hoare in 1959[1] and published in 1961.[2] It is still a commonly used algorithm for sorting. Overall, it is slightly faster than merge sort and heapsort for randomized data, particularly on larger distributions.[3]
Quicksort is a divide-and-conquer algorithm. It works by selecting a 'pivot' element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. For this reason, it is sometimes called partition-exchange sort.[4] The sub-arrays are then sorted recursively. This can be done in-place, requiring small additional amounts of memory to perform the sorting.
Quicksort is a comparison sort, meaning that it can sort items of any type for which a "less-than" relation (formally, a total order) is defined. Most implementations of quicksort are not stable, meaning that the relative order of equal sort items is not preserved.
【快速排序是一种高效的通用排序算法。 快速排序由英国计算机科学家 Tony Hoare 于 1959 年开发，并于 1961 年发表。它仍然是一种常用的排序算法。 总的来说，对于随机数据，它比合并排序和堆排序稍快，特别是在较大的分布上。
快速排序是一种分而治之的算法。 它的工作原理是从数组中选择一个“枢轴”元素，并根据其他元素是小于还是大于枢轴将其他元素划分为两个子数组。 因此，有时称为分区交换排序。然后对子数组进行递归排序。 这可以就地完成，需要少量的额外内存来执行排序。
快速排序是一种比较排序，这意味着它可以对定义了“小于”关系（正式称为全序）的任何类型的项目进行排序。 大多数快速排序的实现都不稳定，这意味着相同排序项的相对顺序不会保留。】 |
| [百度百科](https://baike.baidu.com/item/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95?fromtitle=%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F&fromid=2084344&fromModule=lemma_search-box) | 快速排序（Quicksort），计算机科学词汇，适用领域 Pascal，C++等语言，是对[冒泡排序](https://baike.baidu.com/item/%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F/4602306?fromModule=lemma_inlink)算法的一种改进。快速排序采用的是分治思想，即在一个无序的序列中选取一个任意的基准元素 pivot，利用 pivot 将待排序的序列分成两部分，前面部分元素均小于或等于基准元素，后面部分均大于或等于基准元素，然后采用递归的方法分别对前后两部分重复上述操作，直到将无序序列排列成有序序列。 |

# 复述展开

**快速排序 (Quick Sort)**
快速排序用到了分治思想，同样的还有归并排序。乍看起来快速排序和归并排序非常相似，都是将问题变小，先排序子串，最后合并。
不同的是快速排序在划分子问题的时候经过多一步处理，将划分的两组数据划分为一大一小，这样在最后合并的时候就不必像归并排序那样再进行比较。但也正因为如此，划分的不定性使得快速排序的时间复杂度并不稳定。

**快速排序的基本思想**：
通过一趟排序将待排序列分隔成独立的两部分，其中一部分记录的元素均比另一部分的元素小，则可分别对这两部分子序列继续进行排序，以达到整个序列有序。

> 📌 **算法步骤**  
> 快速排序使用分治法（Divide and conquer）策略来把一个序列分为较小和较大的 2 个子序列，然后递回地排序两个子序列。具体算法描述如下：
>
> 1. 从序列中随机挑出一个元素，做为 “基准”(pivot)；
>
> 2. 重新排列序列，将所有比基准值小的元素摆放在基准前面，所有比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个操作结束之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
>
> 3. 递归地把小于基准值元素的子序列和大于基准值元素的子序列进行快速排序。

    1. 从序列中随机挑出一个元素，做为 “基准”(pivot)；
    2. 重新排列序列，将所有比基准值小的元素摆放在基准前面，所有比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个操作结束之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
    3. 递归地把小于基准值元素的子序列和大于基准值元素的子序列进行快速排序。

> 📌 **算法分析**
>
> - 稳定性 ：不稳定
> - 时间复杂度 ：最佳：O(nlogn)， 最差：O(nlogn)，平均：O(nlogn)
> - 空间复杂度 ：O(nlogn)

    - 稳定性 ：不稳定
    - 时间复杂度 ：最佳：O(nlogn)， 最差：O(nlogn)，平均：O(nlogn)
    - 空间复杂度 ：O(nlogn)

<details>
<summary>代码</summary>

```python
class QuickSort:
    def quick_sort(self, nums):
        return self.quick(nums, 0, len(nums) - 1)

    def partition(self, nums, i, j) -> int:
        # 以最前的数作为基准
        pivot = nums[i]
        left = i
        right = j
        while left < right:
            while left < right and nums[right] >= pivot:
                right -= 1
            while left < right and nums[left] <= pivot:
                left += 1
            # 交换
            nums[left], nums[right] = nums[right], nums[left]
        # 交换基准与分割点的位置
        nums[i], nums[left] = nums[left], nums[i]
        return left

    def quick(self, nums, i, j):
        if i >= j:
            # 需要控制结束条件
            return nums
        pivot_index = self.partition(nums, i, j)
        self.quick(nums, i, pivot_index - 1)
        self.quick(nums, pivot_index + 1, j)
        return nums


if __name__ == '__main__':
    nums = [10, 13, 2, 33, 3, 4, 4, 7, 9]
    solution = QuickSort()
    print(solution.quick_sort(nums))
```

</details>

# 理解体会

1、快速排序的最差情况分析，时间复杂度应该是 O(n^2)。【此时作为基准的选择基本上都是选择左侧或者右侧第一位】

![%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F_%E5%8E%8B%E7%BC%A9.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/b581d7ad-e4a0-4283-9910-78956e52f4dd/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F_%E5%8E%8B%E7%BC%A9.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120552Z&X-Amz-Expires=3600&X-Amz-Signature=62949d7a39770e34fb1ef4d26f32b0d7ce01d9ce98fcf2979545a631be8026af&X-Amz-SignedHeaders=host&x-id=GetObject)

最差情况下，逆序情况，每次选择基准都是将两部分分为 0，N，导致最终的时间复杂度退化到 O(n^2)

对于上述的情况，可以通过随机选择基准来避免上述的情况。

所以，在上面，使用策略之后，最坏的情况也能达到 O(nlogn)

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
