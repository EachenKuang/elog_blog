---
password: ""
icon: ""
date: "2023-10-16"
type: Post
category: 行业概念
slug: industry-day25
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day25 【概念解析】 桶排序
status: Published
cover: "https://images.unsplash.com/photo-1606137039116-f771b914a6c7?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 28dc31c1-ffaf-4153-b5f8-9b6f3bffae5c
updated: "2023-10-27 19:24:00"
---

# 整理定义

中文名称：桶排序

英文名称：bucket sort

| 出处           | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 一种简单的排序算法。算法将数组元素映射到有限数量个桶里，每个桶内各自进行排序                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 百度百科       | **桶排序 (Bucket sort)**或所谓的**箱排序**，是一个[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605?fromModule=lemma_inlink)，工作的原理是将数组分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605?fromModule=lemma_inlink)或是以递归方式继续使用桶排序进行排序）。桶排序是[鸽巢排序](https://baike.baidu.com/item/%E9%B8%BD%E5%B7%A2%E6%8E%92%E5%BA%8F/8010555?fromModule=lemma_inlink)的一种[归纳](https://baike.baidu.com/item/%E5%BD%92%E7%BA%B3/7118703?fromModule=lemma_inlink)结果。当要被排序的数组内的数值是均匀分配的时候，桶排序使用线性时间（[Θ](https://baike.baidu.com/item/%CE%98?fromModule=lemma_inlink)（_n_））。但桶排序并不是 比较排序，他不受到 O(n log n) [下限](https://baike.baidu.com/item/%E4%B8%8B%E9%99%90/10215216?fromModule=lemma_inlink)的影响。 |
| 维基百科       | 桶排序（或 箱排序）是一种排序算法，其工作原理是将数组的元素分配到多个桶中。 然后使用不同的排序算法或递归地应用桶排序算法对每个桶进行单独排序。 它是一种分布排序，是鸽笼排序的推广，允许每个存储桶有多个键，并且是从最高到最低有效数字风格的基数排序的表亲。 桶排序可以通过比较来实现，因此也可以被认为是比较排序算法。 计算复杂度取决于用于对每个桶进行排序的算法、要使用的桶的数量以及输入是否均匀分布。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

# 复述展开

桶排序，也叫做箱排序，先将数组分到有限的桶子里面，再对每个桶子分别排序。

工作原理如下：

1. 设置一个最初为空的“桶”数组。
2. 分散：遍历原始数组，将每个对象放入其存储桶中。
3. 对每个非空桶进行排序。
4. 聚集：按顺序访问桶，并将所有元素放回到原始数组中。

**图片展示：**

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/f7680d18-4370-4b06-9031-c57038478875/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120530Z&X-Amz-Expires=3600&X-Amz-Signature=9769369a1d9c862adc2c58cb93561521c199e1dec9cc677031b3990e577205ad&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/4a75355d-42c3-4a76-8502-9ed80a00837b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120531Z&X-Amz-Expires=3600&X-Amz-Signature=0ec75f2f416039fa0591a0f90d77a606e46d79603fdb3a5eb945399283e2d72b&X-Amz-SignedHeaders=host&x-id=GetObject)

时间复杂度：

- 最佳情况：O(n+k)
- 最坏情况：O(n^2)
- 平均情况：O(n+k)

空间复杂度：O(nk)

# 理解体会

桶排序适用于以下场景：

- 待排序的元素分布均匀，且元素的范围已知。
- 待排序的元素是非负整数或浮点数。
- 对于每个桶中的元素，可以使用其他排序算法进行排序。

桶排序的优点：

- 桶排序是一种稳定的排序算法，可以保持相等元素的相对顺序。
- 当待排序元素分布均匀时，桶排序的平均时间复杂度较低，接近线性时间复杂度 O(n)。
- 桶排序可以通过调整桶的数量和范围来平衡时间和空间的消耗。

桶排序的缺点：

- 当待排序元素分布不均匀时，可能导致某些桶中的元素过多，需要额外的排序操作，增加时间复杂度。【最极端，只有分布到一个桶，就是 n^2】
- 桶排序的空间复杂度较高，需要额外的存储空间来存放桶。

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
