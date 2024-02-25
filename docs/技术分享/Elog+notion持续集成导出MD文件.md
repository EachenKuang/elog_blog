---
password: ""
icon: ""
date: "2023-11-16"
type: Post
category: 技术分享
slug: elog
tags:
  - 建站
  - 工具
  - 必看精选
summary: ""
title: Elog+notion持续集成导出MD文件
status: Published
urlname: c366b1ea-ae10-4b95-b3b8-bc49b517487b
updated: "2024-02-01 12:39:00"
---

# 前言

截止到现在，已经持续使用了 Notion 近两年，也基本上将之前在 Typora 上写的 Markdown 文章搬运过来了。但是，有个问题，**极端一点思考，如果 Notion 突然不维护了，那么我的这些数据该怎么办？**

尽管，Notion 也提供了导出 Markdown 格式的功能，但是，只能一个一个导出，后期文档数量上去的话，导出都挺折腾时间的。

于是，寻求一种能够批量、快速、自动化导出 Notion 文章的方法就成了最近关心的事情。好在，在[Matrixcore](https://matrixcore.top/) 的帮助下，发现一件神器 —— [Elog](https://elog.1874.cool/)。

使用 Notion+Elog+Slack+Pipedream 这一套组合拳就解决了持续集成的方式来自动化更新 Notion 文档。

# 本地实践开始

因为我的主要目的是能够使用 Elog 定时备份 NotionNext 下的文档，并且定期归档，所以实践这块以完成持续集成部署为结果展开流程。

## 安装依赖

参考[快速开始](https://elog.1874.cool/notion/start)

前置依赖，需要安装好 [node](https://nodejs.org/en)

```shell
# 使用 npm 安装 CLI
npm install @elog/cli -g

# 使用 yarn 安装 CLI
yarn global add @elog/cli

# 使用 pnpm 安装 CLI
pnpm install @elog/cli -g

# 安装指定版本
npm install @elog/cli@0.9.0 -g
```

## 配置 Elog

在本地目录执行初始化，我这边是在云服务器上，地址为： `/data/server/Myblog_backup`

```shell
cd /data/server/Myblog_backup
elog init
```

根据提示初始化成功后，会在根目录生成一份  `elog.config.js`  配置文件和本地调试用的`.elog.env`环境变量配置文件。

其中 `elog.config.js` 是关键的配置文件，而 `.elog.env` 是用于保存本地关键配置的文件。

关于具体参考 [目录结构 | Elog (1874.cool)](https://elog.1874.cool/notion/config-catalog)

主要关注以下三个字段的内容：

- write 写作平台详细配置
- deploy 博客平台详细配置
- image 图床平台详情配置

我本地的配置是：

```javascript
module.exports = {
	write: {
		platform: 'notion',
		notion: {
		    token: process.env.NOTION_TOKEN,
		    databaseId: process.env.NOTION_DATABASE_ID,
		    filter:  {
		      property: 'type',
		      select: {
		        equals: 'Post'
		      }
		    },
		    sorts: true,
		    catalog: {
		      enable: true,
		      property: "category",
		    }
		},
		deploy: {
	    platform: 'local',
	    local: {
	      outputDir: './docs',
	      filename: 'title',
	      format: 'matter-markdown',
	      catalog: true,
	      formatExt: '',
	    },
		}
		image: {
	    enable: true,
	    platform: 'local',
	    local: {
	      outputDir: './docs/images',
	      prefixKey: '/docs/images',
	      pathFollowDoc: true,
	      imagePathExt: '',
	    },
		}
}
```

我这边就是需要将 notion 数据库中的 `type` 字段 为 `Post` 的文章导出到 ./docs 目录下，Markdown 名称以 `title` 字段作为名称，按照 `catalog` 进行文件夹归类。文章的图片下载到本地 `./docs/images` 目录下，并使用相对路径替换掉原始的图片。

## 同步

```shell
elog sync -e .elog.env
```

**本地效果**【左边为 Notion 数据库部分截图，右边为生成的 MD 文档】：

![](https://image.kuangyichen.com/image/202311162036134.webp)

![](https://image.kuangyichen.com/image/202311162034131.png)

# 持续集成部署

> 🎈 **说了这么多，一直都是本地在操作，如何实现自动化持续集成呢？**

需要借助 Notion+Slack+Pipedream+GitHub 了

notion ⇒ slack ⇒ pipedream ⇒ github action

## slack 设置

### 1、注册好之后，创建频道（channel）

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/d57cceec-abea-4500-b629-1f699baed808/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=c911eed0f1bd7b8101e482d73712dc8651dadba91450356d2dcf13e2c3f1f371&X-Amz-SignedHeaders=host&x-id=GetObject)

### 2、添加应用 Notion

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/d59711a2-85d7-4c0d-9db5-e3cabdd024e1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=b4e4cac70486c5191054b6dba2c4f94667e60cc01bc0217452ddb41419f64bc6&X-Amz-SignedHeaders=host&x-id=GetObject)

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/da136fb8-4852-46fe-94e9-13a367699e7c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=2aa0127f0aefe5f06760e47f35cc722cc7298057f788d838d9d169fe46d55a0d&X-Amz-SignedHeaders=host&x-id=GetObject)

## notion 设置

### 1、创建 automation

在数据库页面点击闪电按钮，可以创建 `automation` ，增加触发器以及执行器

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/361acabd-7c2b-4a9e-a26c-ee109bf480cf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=41a03e635f8a244680dbc9e4dd837c1c2d57b8adc414a7d62a53c2441edd5aa4&X-Amz-SignedHeaders=host&x-id=GetObject)

触发器设置为 状态变为 `Published` 触发。因为在 slack 上设置添加了应用 Notion，所以可以在执行器中看到 推送消息至频道，选择好在 Slack 中创建的频道，这里是 notion

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/41e958bc-8b6d-4950-96d0-4dc5981dbc3b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=0d2bf24de0f6e2ae5b404a4d6ff6d15572bc9b479a1d80101bf4da97dc25b6f7&X-Amz-SignedHeaders=host&x-id=GetObject)

## pipedream 设置

### 1、注册

### 2、创建 project

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/b435a34b-dead-41a8-967f-28fc7affff2f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=3210c204d666f7450bc4ff9ef6a0f4b0a0b3eb408f257851ba99d33e5a478a0c&X-Amz-SignedHeaders=host&x-id=GetObject)

### 3、在 project 下创建 workflow

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/9228da9e-1e24-4aed-9cdf-dfbd67e0a74a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=f8b5be8de7d5a84719debac203b4d906c27b61ab176d6b43dd7ed1d477513a80&X-Amz-SignedHeaders=host&x-id=GetObject)

### 4、设置 workflow

trigger 触发器选择 Slack

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/10cfdbce-2d41-4bea-8fdd-ceee79321d59/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=6e7b7471711549bfc77c1b61a19c2d5e02f9ac0fce42e7415712cc0f8de62b6e&X-Amz-SignedHeaders=host&x-id=GetObject)

选择 `New Message In Channels`

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/5c1250e0-809d-430b-80cc-25c3deffc073/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=038ad0bd95940592c730c997929b08ab6935dda6618e9ff246ef47c83f2c1ce1&X-Amz-SignedHeaders=host&x-id=GetObject)

按照如下设置

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/94e1fd3b-5637-40fb-927c-6436b782035b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=c40f27518a4a0c5adbac8daf72bce76e69f9d0eb9b69ab81d6e5c22956010f90&X-Amz-SignedHeaders=host&x-id=GetObject)

增加一个 Step Python

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/2c83c076-600f-4af0-98af-ae0de5297964/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=fbd1e2ed86a4d9a44094a42d2ce5ef775cd44a61118cc364ccb8df6ed4820829&X-Amz-SignedHeaders=host&x-id=GetObject)

按照如下编写即可：

```python
import os
import requests
import json


def handler(pd: "pidedream"):
    try:

        token = os.environ['GITHUB_TOKEN']
        user = "github_username"
        repo = "github_repo"
        event_type = "deploy"
        headers = {
            "User-Agent": "@elog/serverless-api",
            "Accept": '*/*',
            "Authorization": f"token {token}",
        }
        response = requests.post(
            f"https://api.github.com/repos/{user}/{repo}/dispatches",
            headers=headers,
            data=json.dumps({"event_type": event_type})
        )
        # response.raise_for_status()
        print(response.text, response.json())
        return {"message": response.json() or 'Success!'}
    except Exception as e:
        print(e)
```

配置环境变量，用于 code 中使用，在环境中通过 `os.environ['your_token']`来使用

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/bcc75a11-13bf-47f2-9922-e88ea235d9f0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=b3aef7572326970198b45f04662f0ebf43afd7e85f8c6d03f006195de78bea60&X-Amz-SignedHeaders=host&x-id=GetObject)

至此，已完成一半的设置。

注意：

> ✅ **以下三个变量需要在 GitHub 获取**  
> **github_username**: 你的 github 账户名称  
> **github_repo**：你需要同步的 github 仓库，后续会将生成的 markdown 推送到这个仓库  
> **token**：github token

## github 设置

在 Github 上新建一个仓库，将本地与远程仓库链接上。

在 `.github/workflows/`**`main.yaml`** **添加如下**

```javascript
name: Deplo To Github Pages

on:
  # 允许手动push触发
  push:
    branches:
      - master
  # 允许外部仓库事件触发
  repository_dispatch:
    types:
  # api中的event_type就是这个
      - deploy

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: 检查分支
        uses: actions/checkout@master

      - name: 安装node环境
        uses: actions/setup-node@master
        with:
          node-version: "16.x"

      - name: 安装依赖
        run: |
          export TZ='Asia/Shanghai'
          npm install --prod

      - name: 拉取语雀/Notion的文章
        env:
          # 语雀相关环境变量
          YUQUE_TOKEN: ${{ secrets.YUQUE_TOKEN }}
          YUQUE_LOGIN: ${{ secrets.YUQUE_LOGIN }}
          YUQUE_REPO: ${{ secrets.YUQUE_REPO }}
          # Notion相关环境变量
          NOTION_TOKEN: ${{ secrets.NOTION_TOKEN }}
          NOTION_DATABASE_ID: ${{ secrets.NOTION_DATABASE_ID }}
          # 图床相关环境变量，以腾讯云COS为例
          COS_SECRET_ID: ${{ secrets.COS_SECRET_ID }}
          COS_SECRET_KEY: ${{ secrets.COS_SECRET_KEY }}
          COS_IMAGE_BUCKET: ${{ secrets.COS_IMAGE_BUCKET }}
          COS_IMAGE_REGION: ${{ secrets.COS_IMAGE_REGION }}
        run: |
          # 对应package.json中的script.sync
          npm run sync

      - name: 配置Git用户名邮箱
        run: |
          git config --global user.name "Eachen"
          git config --global user.email "695089605@qq.com"

      - name: 提交yuque拉取的文章到GitHub仓库
        run: |
          echo `date +"%Y-%m-%d %H:%M:%S"` begin > time.txt
          git add .
          git commit -m "更新文档" -a

      - name: 推送文章到仓库
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}

      - name: 生成静态文件
        run: |
          # 对应package.json中的script.build
          npm run build
```

**添加环境变量**：

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/4fee4106-f3b5-49d0-a1b8-8a0d4e679bd0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=72def79453c8818e4af384f33e2c12cc174c4bf106503266c13d1a38f91334f8&X-Amz-SignedHeaders=host&x-id=GetObject)

**增加执行权限：**

`Setting` 下 `Actions` Tab 的 Workflow permissions 需要选中 `Read and write permissions`,否则会报错，没有权限更新。

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/0c2ecb6d-fbb8-44de-8f67-b10fbbabfe7e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=d17094e95487413ca56b367e08035d0129102f04581287be58897df6d9aec868&X-Amz-SignedHeaders=host&x-id=GetObject)

## 执行效果：

当在 Notion 数据库中，将文章的状态从 `Draft` 转为 `Published` ，随即触发执行

**Slack:**

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/e5efc5a5-132c-4a20-b64b-ce308723cefc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=0b802d4028437772556dff67a6141872a03ab65cefdfa280d9afc46720ddca3c&X-Amz-SignedHeaders=host&x-id=GetObject)

**Pipedream:**

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/aaa638ce-ea8a-4213-b940-4fa90e513e47/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=66df07706da36281d104eb76a8c2010d3deed44f68351821b7c1055cc95c9af6&X-Amz-SignedHeaders=host&x-id=GetObject)

**Github:**

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/2985fb3d-08cc-4617-be3f-5db47f2f298a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=38a1fcbba592b03bf6b66ca084ddda376e15fa54c369be6128abaaa95e97cf90&X-Amz-SignedHeaders=host&x-id=GetObject)

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/949dbf46-9a8d-4637-8c60-e05ef9d3114a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20240225%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20240225T014612Z&X-Amz-Expires=3600&X-Amz-Signature=9e5efebd522388779c6117685aa932edf51cff1db6083c73ee8018d0125be36a&X-Amz-SignedHeaders=host&x-id=GetObject)

# 关于 Elog

## 什么是 Elog

> `Elog`名为  `Easy Blogging`，简单、轻松的书写&部署博客  
> 它是一种开放式跨端博客解决方案，随意组合写作平台（语雀/飞书/Notion/FlowUs）和博客平台（Hexo/Vitepress/Confluence/WordPress）等

用我的话来说，就是 Elog 支持从多个**写作平台**中导出 Markdown 文件格式，然后生成不同**博客平台**的静态博客文件部署。就是方便不懂技术的小伙伴也能在不同的写作平台以及博客平台自由切换，体验一条龙服务。

> 💡 Elog 支持的写作平台与博客平台  
> **写作平台：**
>
> - Notion
> - 语雀
> - FlowUs
> - 飞书云文档
>
> **博客平台：**
>
> - Hexo
> - Vitepress
> - HuGo
> - Docusaurus
> - Docz
> - Confluence
> - WordPress
>
> **支持图床**
>
> - 本地
> - 腾讯云 COS
> - 阿里云 OSS
> - Github 图床
> - 七牛云
> - 又拍云

# 注意事项：

> ❓ 【github】如何将本地仓库与远程仓库建立连接

- 【**空仓库】创建完空仓库就将本地项目关联到远程**

  1.  **如果本地项目已经通过 git 管理：**

      ```shell
      git remote add origin
      git push -u origin master
      ```

  2.  **本地项目未通过 git 管理：**

      ```shell
      # (将项目通过git初始化)
      git init
      # (添加所有修改到暂存区)
      git add .
      # (提交commit)
      git commit -m "first commit remark"
      # 与远程仓库地址建立连接
      git remote add origin [仓库地址]
      # (推送到远程github)
      git push -u origin master
      ```

- 【**非空仓库】创建完非空仓库就将本地项目关联到远程**

  1.  远程非空仓库的关联，就要涉及到 2 个不同项目的关联了；得先将远程的代码合并下来，才能提交；我们先假设项目已经通过 git 管理了。缺少关联 push 远程仓库的步骤讲：

      ```shell
      # 与远程仓库地址建立连接
      git remote add origin [仓库地址]
      # 让两个不同项目的没关联提交允许拉取合并
      git pull origin master --allow-unrelated-histories
      ```

> ❓ 【github】如何获取 github token

在页面下设置 [Personal Access Tokens (Classic) (github.com)](https://github.com/settings/tokens)

![](https://image.kuangyichen.com/image/202311171004131.png)

![](https://image.kuangyichen.com/image/202311171006864.png)

# 参考：

[Elog 介绍 | Elog 文档 (1874.cool)](https://elog.1874.cool/)

[[跨域协同] elog+notion 实现 md 优雅备份 | MatrixCore](https://matrixcore.top/article/elog)

[https://pipedream.com/](https://pipedream.com/)
