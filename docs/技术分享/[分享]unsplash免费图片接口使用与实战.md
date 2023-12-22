---
password: ""
icon: ""
date: "2023-12-22"
type: Post
category: 技术分享
slug: unsplash
tags:
  - 工具
  - 开发
summary: "最近发现一个不错的网站封面制作的网站——《https://coverview.vercel.app/》，通过使用它，我生成了非常多的自定义的图片封面。一次偶然间，发现里面的图片都是 unsplash 提供的。为何我不试试看自己实现一个获取 unsplash 免费图片的接口呢？于是就有了这篇文章。"
title: "[分享]unsplash免费图片接口使用与实战"
status: Published
urlname: e49a71ec-c929-47bf-8116-aab954b01885
updated: "2023-12-22 08:33:00"
---

> 😀 最近发现一个不错的网站封面制作的网站——《[https://coverview.vercel.app/](https://coverview.vercel.app/)》，通过使用它，我生成了非常多的自定义的图片封面。一次偶然间，发现里面的图片都是 unsplash 提供的。为何我不试试看自己实现一个获取 unsplash 免费图片的接口呢？于是就有了这篇文章。

# 什么是 **unsplash？**

## 网站欣赏——**Unsplash**

[bookmark](https://unsplash.com/)

下面我们重点欣赏下这个网站：

> **Photos for everyone**

    Over 3 million free high-resolution images brought to you by the world’s most generous community of photographers.

![](https://image.kuangyichen.com/image/unsplash1.webp)

![](https://image.kuangyichen.com/image/unsplash2.webp)

## 维基百科

**从维基百科上也可以看出：**

[bookmark](https://en.wikipedia.org/wiki/Unsplash)

> **Unsplash** is a website dedicated to proprietary [stock photography](https://en.wikipedia.org/wiki/Stock_photography). Since 2021, it has been owned by [Getty Images](https://en.wikipedia.org/wiki/Getty_Images). The website claims over 330,000 contributing [photographers](https://en.wikipedia.org/wiki/Photographer) and generates more than 13 billion photo impressions per month on their growing library of over 5 million photos (as of April 2023).[[1]](https://en.wikipedia.org/wiki/Unsplash#cite_note-unsplashhistory-1)[[2]](https://en.wikipedia.org/wiki/Unsplash#cite_note-2) Unsplash has been cited as one of the world's leading photography websites by [_Forbes_](https://en.wikipedia.org/wiki/Forbes), Design Hub, [CNET](https://en.wikipedia.org/wiki/CNET), [Medium](<https://en.wikipedia.org/wiki/Medium_(website)>) and [The Next Web](https://en.wikipedia.org/wiki/The_Next_Web).[[3]](https://en.wikipedia.org/wiki/Unsplash#cite_note-3)[[4]](https://en.wikipedia.org/wiki/Unsplash#cite_note-4)[[5]](https://en.wikipedia.org/wiki/Unsplash#cite_note-5)[[6]](https://en.wikipedia.org/wiki/Unsplash#cite_note-6)[[7]](https://en.wikipedia.org/wiki/Unsplash#cite_note-7)

Unsplash 是一个致力于专有图库摄影的网站。 自 2021 年起，它归盖蒂图片社所有。 该网站声称拥有超过**330,000**名贡献摄影师，其不断增长的超过**500 万**张照片库（截至 2023 年 4 月）每月产生超过**130 亿**张照片展示。**Unsplash 被福布斯、Design Hub、CNET、Medium 和 The Next Web 评为世界领先的摄影网站之一**。

## 听听 ChatGPT 怎么说

> 【**ChatGPT4-turbo**的回答】

    Unsplash 是一个互联网上的免费（商用）照片分享平台，它提供高分辨率的照片，由全球的摄影师无偿上传，并可以由任何人免费下载和使用。这些照片涵盖了广泛的主题和场景，包括自然风光、城市景观、人物肖像、办公环境等等。


    Unsplash 的优势和特点包括：

    1. **高质量的图片**：Unsplash 上的图片都是由专业人士或有才华的摄影爱好者拍摄的，因此图片质量通常很高，适合用于商业项目、博客文章、网站设计等。
    2. **免费使用**：Unsplash 上的图片都可以免费下载和使用，甚至用于商业用途，而无需支付版权费用或署名摄影师（尽管给予信用和感谢总是受到鼓励的）。
    3. **简单的使用条款**：Unsplash 提供了非常简单和宽松的使用条款，用户可以对图片进行复制、修改、分发和使用，而不需要请求许可。
    4. **易于访问和搜索**：Unsplash 网站提供了一个简单直观的界面，用户可以轻松地搜索和浏览不同类型的图片。
    5. **API支持**：Unsplash 提供了一个功能强大的API，允许开发者在自己的应用程序中集成Unsplash 图库的功能，如搜索图片、获取随机图片等。
    6. **社区支持**：Unsplash 拥有一个活跃的社区，摄影师们可以分享他们的作品，用户可以与创作者互动，这为平台带来了持续的内容更新和多样性。
    7. **创意灵感**：对于设计师和创意工作者来说，Unsplash 是一个寻找灵感的好地方。它的广泛图片类别和高质量内容可以激发新的创意想法。

    Unsplash 的这些特点使其成为设计师、博客作者、企业家和创意专业人士寻找高质量图片的热门选择。

# 如何用起来呢？

这个网站这么强，如何使用起来呢？

很巧，它提供了可供使用的开发者 API 使用。下面说下如何快速使用 unsplash API。

**下面是 Unsplash API 文档**

[bookmark](https://unsplash.com/documentation)

## 如何快速实现获取免费图片接口

> 💡 **步骤说明**：
>
> 1. 创建一个 **Unsplash 账户**
>
> 2. 创建一个 APP  
>    创建之后未 Demo 状态，请求限制在**50 次一个小时，**如果需要进阶，可以进行升级，按需使用
>
> 3. 获取账户提供的密钥： 主要是 AccessKey
>
> 4. 编写一个简单接口调用代码【代码附在文末】  
>    Search Photo: [Unsplash API Documentation | Free HD Photo API | Unsplash](https://unsplash.com/documentation#search-photos)  
>    Get Random Photo: [Unsplash API Documentation | Free HD Photo API | Unsplash](https://unsplash.com/documentation#get-a-random-photo)

## 图片使用

图片的使用方式，可以在附录中看到示例，一般从 urls 字段中获取，可以按需使用，可供选择的有：

    1. raw (原图)返回一个基本图像 URL，其中仅包含照片路径和 API 应用程序的“ixid”参数。 使用它可以轻松添加其他图像参数来构建您自己的图像 URL。
    2. full （全幅，没有裁剪）返回 jpg 格式的照片及其最大尺寸。 出于性能目的，我们不建议使用此选项，因为用户的照片加载速度会很慢。
    3. regular（宽度为 `1080 pixels` 的图）
    4. small（宽度为 `400 pixels` 的图）
    5. thumb （宽度为 `200 pixels` 长度的图）

> 【[Unsplash API Documentation | Free HD Photo API | Unsplash](https://unsplash.com/documentation#example-image-use)】

    - `full` returns the photo in jpg format with its maximum dimensions. For performance purposes, we don’t recommend using this as the photos will load slowly for your users.
    - `regular` returns the photo in jpg format with a width of 1080 pixels.
    - `small` returns the photo in jpg format with a width of 400 pixels.
    - `thumb` returns the photo in jpg format with a width of 200 pixels.
    - `raw` returns a base image URL with just the photo path and the `ixid` parameter for your API application. Use this to easily add additional image parameters to construct your own image URL.

**示例说明：**

如果您的应用程序需要宽度为 1500px、DPR 为 2 的图像，请获取原始 URL 并添加 w=1500 和 dpr=2 参数来创建新图像：

```json
photo.urls.raw + "&w=1500&dpr=2";
// => [https://images.unsplash.com/photo-1461988320302-91bde64fc8e4?ixid=2yJhcHBfaWQiOjEyMDd9&w=1500&dpr=2](https://images.unsplash.com/photo-1461988320302-91bde64fc8e4?ixid=2yJhcHBfaWQiOjEyMDd9&w=1500&dpr=2)
```

如果应用程序的另一部分需要相同的图像，但宽度为一半，您可以轻松构建另一个 URL，而无需再次访问 API：

```json
photo.urls.raw + "&w=750&dpr=2";
// => [https://images.unsplash.com/photo-1461988320302-91bde64fc8e4?ixid=2yJhcHBfaWQiOjEyMDd9&w=750&dpr=2](https://images.unsplash.com/photo-1461988320302-91bde64fc8e4?ixid=2yJhcHBfaWQiOjEyMDd9&w=750&dpr=2)
```

> 🚫 **注意事项**
>
> 1. 在创建 Unsplash 账户时，可能会出现【**reCaptcha 人机验证无法显示**】，这个问题一度困扰我很久。  
>    参考这篇文章《[reCaptcha 人机验证无法显示和 CSP 问题解决方案 – Azure Zeng Blog](https://blog.azurezeng.com/recaptcha-use-in-china/comment-page-10/)》基本可以解决

# 总结

注册好 unsplash 网站的账户之后，新建一个 APP 用于图片接口的开发，就能自己使用接口来做一些事情了。例如根据自己需要获取相关的图片，使用各种大小格式的图片进行网站的使用，甚至是二次加工使用，都非常方便。

# 附录

## unsplash_client.py 示例代码展示

[embed](https://gist.github.com/EachenKuang/e302c07c6accd070e48b2f2ec90e0208)

## 随机图片返回示例

```json
{
  "id": "Dwu85P9SOIk",
  "created_at": "2016-05-03T11:00:28-04:00",
  "updated_at": "2016-07-10T11:00:01-05:00",
  "width": 2448,
  "height": 3264,
  "color": "#6E633A",
  "blur_hash": "LFC$yHwc8^$yIAS$%M%00KxukYIp",
  "downloads": 1345,
  "likes": 24,
  "liked_by_user": false,
  "description": "A man drinking a coffee.",
  "exif": {
    "make": "Canon",
    "model": "Canon EOS 40D",
    "exposure_time": "0.011111111111111112",
    "aperture": "4.970854",
    "focal_length": "37",
    "iso": 100
  },
  "location": {
    "name": "Montreal, Canada",
    "city": "Montreal",
    "country": "Canada",
    "position": {
      "latitude": 45.473298,
      "longitude": -73.638488
    }
  },
  "current_user_collections": [ // The *current user's* collections that this photo belongs to.
    {
      "id": 206,
      "title": "Makers: Cat and Ben",
      "published_at": "2016-01-12T18:16:09-05:00",
      "last_collected_at": "2016-06-02T13:10:03-04:00",
      "updated_at": "2016-07-10T11:00:01-05:00",
      "cover_photo": null,
      "user": null
    },
    // ... more collections
  ],
  "urls": {
    "raw": "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d",
    "full": "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg",
    "regular": "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg&w=1080&fit=max",
    "small": "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg&w=400&fit=max",
    "thumb": "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg&w=200&fit=max"
  },
  "links": {
    "self": "https://api.unsplash.com/photos/Dwu85P9SOIk",
    "html": "https://unsplash.com/photos/Dwu85P9SOIk",
    "download": "https://unsplash.com/photos/Dwu85P9SOIk/download"
    "download_location": "https://api.unsplash.com/photos/Dwu85P9SOIk/download"
  },
  "user": {
    "id": "QPxL2MGqfrw",
    "updated_at": "2016-07-10T11:00:01-05:00",
    "username": "exampleuser",
    "name": "Joe Example",
    "portfolio_url": "https://example.com/",
    "bio": "Just an everyday Joe",
    "location": "Montreal",
    "total_likes": 5,
    "total_photos": 10,
    "total_collections": 13,
    "instagram_username": "instantgrammer",
    "twitter_username": "crew",
    "links": {
      "self": "https://api.unsplash.com/users/exampleuser",
      "html": "https://unsplash.com/exampleuser",
      "photos": "https://api.unsplash.com/users/exampleuser/photos",
      "likes": "https://api.unsplash.com/users/exampleuser/likes",
      "portfolio": "https://api.unsplash.com/users/exampleuser/portfolio"
    }
  }
}
```

## 搜索图片返回示例

```json
{
  "total": 133,
  "total_pages": 7,
  "results": [
    {
      "id": "eOLpJytrbsQ",
      "created_at": "2014-11-18T14:35:36-05:00",
      "width": 4000,
      "height": 3000,
      "color": "#A7A2A1",
      "blur_hash": "LaLXMa9Fx[D%~q%MtQM|kDRjtRIU",
      "likes": 286,
      "liked_by_user": false,
      "description": "A man drinking a coffee.",
      "user": {
        "id": "Ul0QVz12Goo",
        "username": "ugmonk",
        "name": "Jeff Sheldon",
        "first_name": "Jeff",
        "last_name": "Sheldon",
        "instagram_username": "instantgrammer",
        "twitter_username": "ugmonk",
        "portfolio_url": "http://ugmonk.com/",
        "profile_image": {
          "small": "https://images.unsplash.com/profile-1441298803695-accd94000cac?ixlib=rb-0.3.5&q=80&fm=jpg&crop=faces&cs=tinysrgb&fit=crop&h=32&w=32&s=7cfe3b93750cb0c93e2f7caec08b5a41",
          "medium": "https://images.unsplash.com/profile-1441298803695-accd94000cac?ixlib=rb-0.3.5&q=80&fm=jpg&crop=faces&cs=tinysrgb&fit=crop&h=64&w=64&s=5a9dc749c43ce5bd60870b129a40902f",
          "large": "https://images.unsplash.com/profile-1441298803695-accd94000cac?ixlib=rb-0.3.5&q=80&fm=jpg&crop=faces&cs=tinysrgb&fit=crop&h=128&w=128&s=32085a077889586df88bfbe406692202"
        },
        "links": {
          "self": "https://api.unsplash.com/users/ugmonk",
          "html": "http://unsplash.com/@ugmonk",
          "photos": "https://api.unsplash.com/users/ugmonk/photos",
          "likes": "https://api.unsplash.com/users/ugmonk/likes"
        }
      },
      "current_user_collections": [],
      "urls": {
        "raw": "https://images.unsplash.com/photo-1416339306562-f3d12fefd36f",
        "full": "https://hd.unsplash.com/photo-1416339306562-f3d12fefd36f",
        "regular": "https://images.unsplash.com/photo-1416339306562-f3d12fefd36f?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&s=92f3e02f63678acc8416d044e189f515",
        "small": "https://images.unsplash.com/photo-1416339306562-f3d12fefd36f?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=400&fit=max&s=263af33585f9d32af39d165b000845eb",
        "thumb": "https://images.unsplash.com/photo-1416339306562-f3d12fefd36f?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=200&fit=max&s=8aae34cf35df31a592f0bef16e6342ef"
      },
      "links": {
        "self": "https://api.unsplash.com/photos/eOLpJytrbsQ",
        "html": "http://unsplash.com/photos/eOLpJytrbsQ",
        "download": "http://unsplash.com/photos/eOLpJytrbsQ/download"
      }
    }
    // more photos ...
  ]
}
```
