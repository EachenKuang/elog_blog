---
password: ""
icon: ""
date: "2023-10-12"
type: Post
category: 行业概念
slug: industry-day21
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day21【概念解析】堆排序
status: Published
cover: "https://images.unsplash.com/photo-1547637589-f54c34f5d7a4?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 65271d5d-d66e-4534-9bfd-71a8a7e871c0
updated: "2023-10-27 19:23:00"
---

# 整理定义

| 出处                                                                         | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 中国大百科全书                                                               | 堆是一种近似完全二叉树的结构，并同时满足堆的性质：即子节点的键值或索引总是小于（或者大于）它的父节点。堆排序是指利用堆这种数据结构所设计的一种排序算法。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| [百度百科](https://baike.baidu.com/item/%E5%A0%86%E6%8E%92%E5%BA%8F/2840151) | **堆排序**（英语:Heapsort）是指利用堆这种数据结构所设计的一种[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95?fromModule=lemma_inlink)。堆是一个近似[完全二叉树](https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91?fromModule=lemma_inlink)的结构，并同时满足**堆积的性质**：即子结点的键值或索引总是小于（或者大于）它的父节点。                                                                                                                                                                                                                                               |
| [维基百科](https://en.wikipedia.org/wiki/Heapsort)                           | In computer science, heapsort is a comparison-based sorting algorithm which can be thought of as "an implementation of selection sort using the right data structure."[3] Like selection sort, heapsort divides its input into a sorted and an unsorted region, and it iteratively shrinks the unsorted region by extracting the largest element from it and inserting it into the sorted region. Unlike selection sort, heapsort does not waste time with a linear-time scan of the unsorted region; rather, heap sort maintains the unsorted region in a heap data structure to efficiently find the largest element in each step. |

Although somewhat slower in practice on most machines than a well-implemented quicksort, it has the advantages of very simple implementation and a more favorable worst-case O(n log n) runtime. Most real-world quicksort variants include an implementation of heapsort as a fallback should they detect that quicksort is becoming degenerate.
【在计算机科学中，堆排序是一种基于比较的排序算法，可以将其视为“使用正确数据结构的选择排序的实现”。与选择排序一样，堆排序将其输入分为已排序区域和未排序区域， 它通过从中提取最大元素并将其插入已排序区域来迭代地缩小未排序区域。 与选择排序不同，堆排序不会浪费时间对未排序区域进行线性时间扫描； 相反，堆排序维护堆数据结构中的未排序区域，以有效地找到每个步骤中的最大元素。
虽然在大多数机器上实践中比实现良好的快速排序要慢一些，但它具有实现非常简单和更有利的最坏情况 O(nlogn) 运行时间的优点。 大多数现实世界的快速排序变体都包含堆排序的实现，作为它们检测到快速排序正在退化的后备方案。 堆排序是一种就地算法，但它不是一种稳定的排序。】 |

# 复述展开

## 前置概念

在理解堆排序之前，需要了解下什么是堆？

**堆**：堆(Heap)是[计算机科学](https://baike.baidu.com/item/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6/9132?fromModule=lemma_inlink)中一类特殊的[数据结构](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1450?fromModule=lemma_inlink)，是最高效的[优先级队列](https://baike.baidu.com/item/%E4%BC%98%E5%85%88%E7%BA%A7%E9%98%9F%E5%88%97/6737671?fromModule=lemma_inlink)。堆通常是一个可以被看作一棵[完全二叉树](https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91/7773232?fromModule=lemma_inlink)的数组对象。——百度百科

另外，还有两个概念需要了解：

**大顶堆（最大堆，大根堆）**：指的是最大元素在根节点（root）的堆，它的根节点始终大于它的子节点

**小顶堆（最小堆，小根对）**：指的是最小元素在根节点（root）的堆，它的根节点始终小于它的子节点

```text
  iLeftChild(i)  = 2⋅i + 1
  iRightChild(i) = 2⋅i + 2
  iParent(i)     = floor((i−1) / 2)
```

**堆排序就是利用堆的这一特性进行数据的排序。**

## 算法原理

```text
算法步骤
1.将初始待排序列 (R1, R2, ……, Rn) 构建成大顶堆，此堆为初始的无序区；
2.将堆顶元素 R[1] 与最后一个元素 R[n] 交换，此时得到新的无序区 (R1, R2, ……, Rn-1) 和新的有序区 (Rn), 且满足 R[1, 2, ……, n-1]<=R[n]；
3.由于交换后新的堆顶 R[1] 可能违反堆的性质，因此需要对当前无序区 (R1, R2, ……, Rn-1) 调整为新堆，然后再次将 R [1] 与无序区最后一个元素交换，
得到新的无序区 (R1, R2, ……, Rn-2) 和新的有序区 (Rn-1, Rn)。不断重复此过程直到有序区的元素个数为 n-1，则整个排序过程完成。
```

1. 建堆，将数据转化为一维数组表示的初始化堆（大顶堆），长度为 N
2. 将根节点（最大值）与节点 N 交换值，然后调整堆，使之满足大顶堆。
3. N = N-1
4. 重复 2-3 步骤，直至 N=0，此时所有节点都排好序。

![Untitled.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/6510e9a7-779c-479f-8eb0-070a156111af/Untitled.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120543Z&X-Amz-Expires=3600&X-Amz-Signature=d662726b1d0a77725a3827717e523a76352c425732a0030d1d6488d063a7ca74&X-Amz-SignedHeaders=host&x-id=GetObject)

## 代码实现

```python
def swap(nums, i, j):
    nums[i], nums[j] = nums[j], nums[i]


class HeapSort:

    def __init__(self, nums):
        # 初始化全局变量，堆剩下还需要排序的长度
        self.heap_len = len(nums)

    def heapify(self, nums, i):
        """
        调整以 i 为根节点的堆，使之成为大根堆
        这里使用递归的方式
        """
        largest = i
        left = i * 2 + 1
        right = i * 2 + 2
        if left < self.heap_len and nums[largest] < nums[left]:
            largest = left
        if right < self.heap_len and nums[largest] < nums[right]:
            largest = right
        if largest != i:
            # 交换
            swap(nums, largest, i)
            self.heapify(nums, largest)

    def build_heap(self, nums):
        for i in range(self.heap_len // 2 - 1, -1, -1):
            self.heapify(nums, i)

    def heap_sort(self, nums):
        self.build_heap(nums)
        while self.heap_len > 0:
            # 交换
            swap(nums, self.heap_len - 1, 0)
            self.heap_len -= 1
            self.heapify(nums, 0)
        return nums


if __name__ == '__main__':
    nums = [3, 4, 1, 2, 7, 9, 3, 4, 1]
    solution = HeapSort(nums)
    print(solution.heap_sort(nums))
```

## 算法分析

- 稳定性 ：不稳定
- 时间复杂度 ：最佳：O(nlogn)， 最差：O(nlogn)， 平均：O(nlogn)
- 空间复杂度 ：O(1)

建堆，最多遍历所有的数据，时间复杂度是 O(n)

交换 N 次，每次交换的时间复杂度为 O(logn)，所以是 O(nlogn)

最终是 O(nlogn+n) 也就是 O(nlogn)

# 理解体会

堆排序是一种基于二叉堆数据结构的排序算法。它的核心思想是将待排序的序列构建成一个最大堆（或最小堆），然后逐步将堆顶元素与堆的最后一个元素交换，并重新调整堆，使得剩余元素仍满足堆的性质。通过不断重复这个过程，最终得到一个有序的序列。

堆排序的过程可以分为两个主要的步骤：构建堆和堆的调整。

首先，构建堆的过程是将待排序的序列转化为一个堆。可以从最后一个非叶子节点开始，逐个向前进行调整，保证每个节点都满足堆的性质。这个过程称为堆的建立或堆化。

其次，堆的调整是在每次交换堆顶元素与堆的最后一个元素后，重新调整堆，使得剩余元素仍满足堆的性质。通常，堆的调整是从堆顶开始，将堆顶元素与其子节点进行比较，找到最大（或最小）的元素，并将其与堆顶元素交换。然后，继续向下调整交换后的子节点，直到整个序列重新满足堆的性质。

重复进行堆的调整，直到堆中只剩下一个元素，整个序列就变成了有序的。

堆排序的优点是具有较好的时间复杂度，平均和最坏情况下的时间复杂度均为 O(nlogn)，其中 n 是待排序序列的长度。堆排序也是一种原地排序算法，不需要额外的空间。此外，堆排序适用于大规模数据集，对于需要稳定排序的情况，可以通过一些额外的操作来实现。

然而，堆排序的缺点是不稳定，即在排序过程中相等元素的相对顺序可能会改变。此外，堆排序的实现较为复杂。

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
