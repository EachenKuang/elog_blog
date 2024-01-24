---
password: ""
icon: ""
date: "2024-01-17"
type: Post
category: 技术分享
slug: github-connect
tags:
  - 工具
summary: ""
title: Github连接不上怎么办？
status: Published
urlname: a3b5727f-b0ee-4ee7-926e-0c1af377edbf
updated: "2024-01-18 08:41:00"
---

[bookmark](https://segmentfault.com/a/1190000040896781)

如果你电脑上的 git 能在大部分地方进行同步，但是在某处地方的网络下无法同步，并且运行`git pull`或`git push`长久没有反映，最后出现`ssh: connect to host github.com port 22: Connection timed out`，很可能是你的网络供应商（比如广电网）在出口防火墙上屏蔽了 22 端口，这意味着你将无法访问其他主机的 22 端口。

对此，github 提供了一种解决方案，允许你使用 443 端口进行 ssh 连接，因为 443 端口是访问 https 网站所必须的，大部分防火墙都会允许通过，但如果使用代理服务器可能产生干扰。如果连 443 端口都被屏蔽，那你应该无法浏览这篇文章。

运行这段命令，看看是否有成功提示，如果成功，则可以使用这个解决方案。

```text
ssh -T -p 443 git@ssh.github.com
```

简单地配置一下，让你每次 ssh 连接 github 都通过 443 端口。如果你使用 Linux，在`~/.ssh/config`内，添加这些内容，指明 ssh 连接`git@github.com`或`git@ssh.github.com`走 443 端口。

```sql
Host github.com
Hostname ssh.github.com
Port 443
User git
```
