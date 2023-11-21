---
password: ""
icon: ""
date: "2023-10-17"
type: Post
category: 行业概念
slug: industry-day26
tags:
  - 行业概念
  - 文字
  - 思考
  - 算法
summary: ""
title: Day26 【概念解析】 WebP
status: Published
cover: "https://www.notion.so/images/page-cover/gradients_11.jpg"
urlname: ba62195d-3617-4d66-a455-a1121a638af0
updated: "2023-10-27 19:24:00"
---

# 整理定义

| 出处     | 定义                                                                                                                                                                                                               |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 维基百科 | WebP 是一种由 Google 开发的图形文件格式，旨在替代 JPEG、PNG 和 GIF 文件格式。 它支持有损和无损压缩，[8] 以及动画和 Alpha 透明度。                                                                                  |
| 百度百科 | WebP（发音：weppy）是一种同时提供了有损压缩与无损压缩（可逆压缩）的图片文件格式，派生自影像编码格式 VP8，被认为是 WebM 多媒体格式的姊妹项目，是由 Google 在购买 On2 Technologies 后发展出来，以 BSD 授权条款发布。 |

WebP 支持的像素最大数量是 16383x16383。有损压缩的 WebP 仅支持 8-bit 的 YUV 4:2:0 格式。而无损压缩（可逆压缩）的 WebP 支持 VP8L 编码与 8-bit 之 ARGB 色彩空间。又无论是有损或无损压缩皆支持 Alpha 透明通道、ICC 色彩配置、XMP 诠释数据。
WebP 有静态与动态两种模式。动态 WebP（Animated WebP）支持有损与无损压缩、ICC 色彩配置、XMP 诠释数据、Alpha 透明通道。 |

# 复述展开

WebP 是一种新型的图片格式，与之类似的有，JPEG、PNG、GIF、SVG 等。

WebP 是一种基于 VP8 视频编码格式的图像格式，它采用了先进的压缩算法，旨在提供**更高的压缩率**和**更好的图像质量**。**使用 WebP，网站站长和 Web 开发者可以制作更小、更丰富的图片，从而提升网页加载速度**。

## Google 给出**效率对比：**

> WebP 无损图片的大小比 PNG 图片[小 26% ](https://developers.google.com/speed/webp/docs/webp_lossless_alpha_study?hl=zh-cn#results)。WebP 有损图片比采用等效  [SSIM](https://en.wikipedia.org/wiki/Structural_similarity)  质量索引的同类 JPEG 图片[缩小 25-34% ](https://developers.google.com/speed/webp/docs/webp_study?hl=zh-cn)。  
> 无损 WebP **支持透明度**（也称为 Alpha 通道），费用仅为  [22% 的额外字节](https://developers.google.com/speed/webp/docs/webp_lossless_alpha_study?hl=zh-cn#results)。在可以接受有损 RGB 压缩的情况下，**有损 WebP 也支持透明度**，其提供的文件大小通常比 PNG 小 3 倍。

**下图是一张 WebP 格式的图片**

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/890ac675-205e-4e45-9160-7d2dac311d45/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120518Z&X-Amz-Expires=3600&X-Amz-Signature=6aa6491c7708ab87a5eefb58dc87e64618a69e6e9a51fede9873f25c660cbd55&X-Amz-SignedHeaders=host&x-id=GetObject)

[网站的图片格式  |  WebP  |  Google for Developers](https://developers.google.com/speed/webp?hl=zh-cn)

**图片转换在线工具**【[PNG 转 WEBP - 在线 (imgonline.tools)](https://imgonline.tools/zh/png-to-webp)】

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/7d2203b9-af9a-4d31-9641-f43cfcf804df/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120521Z&X-Amz-Expires=3600&X-Amz-Signature=9b27b798fda58b7c42bacd0c3762fb2c7a96e75348e12c7ad1dca5a083172b02&X-Amz-SignedHeaders=host&x-id=GetObject)

![Untitled.webp](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/d1345c27-0371-4093-92f7-71870b6548ec/Untitled.webp?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120522Z&X-Amz-Expires=3600&X-Amz-Signature=5b1a9eacf46807d3e23a301bd11478458821b73ab0807a6fb9eaf546bbfa8dfc&X-Amz-SignedHeaders=host&x-id=GetObject)

使用上面的图片转换工具，得出上面的两张图片：左图为 png 格式（4KB），右图为 webp 格式（2KB），视觉效果上看几乎不差。

# 理解体会

首先，WebP 具有出色的压缩性能。相比 JPEG 格式，WebP 可以实现更高的压缩率，从而减小图像文件的大小。这意味着在保持图像质量的同时，可以显著减少图像的加载时间，提升网页的整体性能。此外，WebP 还支持无损压缩，使得图像在压缩过程中不会丢失任何细节，适用于对图像质量要求较高的场景。

其次，WebP 具备广泛的浏览器和平台支持。现在，主流的 Web 浏览器（如 Chrome、Firefox、Edge 等）已经原生支持 WebP 格式，这意味着无需任何额外的插件或工具，就可以直接在网页上使用 WebP 图像。此外，WebP 还可以在移动设备上得到广泛的支持，包括 Android 和 iOS 平台，使得开发者能够在移动端提供更高效的图像加载体验。

此外，WebP 还具备透明度和动画的支持。类似于 PNG 格式，WebP 可以实现图像的透明背景，使得开发者能够创建更加丰富和吸引人的用户界面。此外，WebP 还支持动画图像，类似于 GIF 格式，但具有更好的压缩性能和图像质量。

在 Web 开发中，WebP 的应用广泛而多样。首先，对于需要加载大量图像的网站，使用 WebP 格式可以显著减小图像文件的大小，提高网页的加载速度，从而提升用户体验。其次，对于移动端应用开发，WebP 的高压缩率和广泛支持使得应用能够在移动设备上更快地加载图像，减少用户的流量消耗。此外，对于需要展示透明背景或动画效果的应用，WebP 提供了更好的选择。

然而，WebP 也存在一些挑战和限制。首先，由于 WebP 是一种相对较新的图像格式，与传统的 JPEG 和 PNG 相比，它的兼容性可能稍有不足。尽管现代浏览器已经支持 WebP，但在一些旧版本的浏览器上可能无法正常显示。其次，WebP 的压缩算法相对复杂，对于一些特定的图像内容，可能无法达到预期的压缩效果。

总结起来，WebP 作为一种现代化的图像格式，具备出色的压缩性能、广泛的浏览器和平台支持，以及透明度和动画的特性。它在 Web 开发中的应用潜力巨大，能够提升网页性能。

# 参考文档

[压缩技术  |  WebP  |  Google for Developers](https://developers.google.com/speed/webp/docs/compression?hl=zh-cn)

[WebP 无损比特流规范  |  Google for Developers](https://developers.google.com/speed/webp/docs/webp_lossless_bitstream_specification?hl=zh-cn)

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
