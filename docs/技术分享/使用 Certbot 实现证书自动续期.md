---
password: ""
icon: ""
date: "2024-12-20"
type: Post
category: 技术分享
slug: certbot-auto-renew
tags:
  - 建站
  - 工具
summary: ""
title: 使用 Certbot 实现证书自动续期
status: Published
urlname: 16254ee3-2891-80df-af79-dce76d77fc2f
updated: "2024-12-20 11:00:00"
---

> 😀 在网络安全日益重要的今天，SSL/TLS 证书对于确保网站的安全性和可信度起着至关重要的作用。Let's Encrypt 是一个免费、自动化和开放的证书颁发机构，为广大网站所有者提供了免费的 SSL/TLS 证书，极大地推动了互联网的安全发展。  
> 然而，Let's Encrypt 颁发的免费证书的有效期曾经为 90 天，现在缩短为 3 个月。这样的短有效期是为了确保证书的安全性和有效性，及时更新可能出现的安全漏洞和问题，同时也鼓励证书使用者保持对证书管理的关注和维护。但对于网站所有者来说，手动每三个月更新一次证书是一项繁琐且容易被遗忘的任务。如果证书过期未及时更新，可能会导致网站出现安全警告，影响用户信任度和访问体验。因此，实现证书的自动续期就变得尤为重要。

# 一、安装步骤

## **1.1 安装 Certbot**

1. 在不同的操作系统上安装 Certbot 的方法会有所不同。以下是一些常见操作系统的安装方法：

- **Ubuntu/Debian**：

```shell
sudo apt-get updatesudo apt-get install certbot
```

- **CentOS/RHEL**：

```shell
sudo yum install epel-releasesudo yum install certbot
```

2. 安装完成后，可以通过运行以下命令来检查 Certbot 是否安装成功：

```shell
certbot --version
```

如果是其他操作系统，可以查看： [https://certbot.eff.org/instructions](https://certbot.eff.org/instructions)

## **1.2 获取证书**

1. 使用 Certbot 获取证书的命令格式如下：

```shell
certbot certonly --nginx -d example.com -d www.example.com
```

- -nginx：表示使用 Nginx 插件来配置证书。如果你的 Web 服务器是其他类型，可以使用相应的插件，如--apache 等。
- d [example.com](http://example.com/) -d [www.example.com](http://www.example.com/)：指定要为哪些域名获取证书。可以指定多个域名，用空格隔开。

1. 运行命令后，Certbot 会引导你进行一些配置，例如同意服务条款、设置邮箱等。按照提示完成操作后，Certbot 会为指定的域名生成证书，并将其存储在相应的位置。

## **1.3 配置自动续期**

1. Certbot 可以通过定时任务来实现证书的自动续期。在大多数系统中，可以使用 cron 来设置定时任务。
2. 打开终端，输入以下命令来编辑 cron 任务：

```shell
crontab -e
```

1. 在打开的文件中添加以下内容：

```shell
0 0,12 * * * /usr/bin/certbot renew --quiet && service nginx reload
```

> 💡 说明
>
> - 0 0,12 \* \* \*：表示每天的 0 点和 12 点执行任务。你可以根据自己的需求调整这个时间。
> - /usr/bin/certbot renew --quiet：执行 Certbot 的续期命令，--quiet 参数表示静默模式，不输出过多的信息。
> - service nginx reload：如果你的 Web 服务器是 Nginx，在续期完成后重新加载 Nginx 服务，使新的证书生效。如果你的 Web 服务器是其他类型，将此命令替换为相应的服务重新加载命令。

1. 保存文件并退出。现在，Certbot 将会按照你设置的时间自动续期证书。

## **1.4 检查证书续期状态**

1. 你可以随时运行以下命令来检查证书的续期状态：

`certbot renew --dry-run`

- -dry-run 参数表示进行一次模拟续期，不会实际更新证书，但会显示如果进行续期会发生的情况。

1. 该命令会输出证书的到期时间、是否需要续期等信息。如果显示“Your existing certificate has not expired yet and there is nothing to do.”，则表示证书目前不需要续期。

> ✅ 通过以上步骤，你可以使用 Certbot 轻松实现证书的自动续期，确保你的网站始终使用有效的 SSL/TLS 证书，提高网站的安全性和可信度。

# 二、通配符域名的安装

> 🚫 然而，很多时候，我们需要对通配符域名（也就是`*.example.com`格式的域名）进行证书续期，这个时候上面的步骤就不够用了。

不管是申请还是续期，只要是通配符证书，只能采用 dns-01 的方式校验申请者的域名，也就是说 certbot 操作者必须手动添加 DNS TXT 记录。

## 2.0 准备工作

这时候需要用到一个脚本： [https://github.com/yangrongzhou/certbot-letencrypt-wildcardcertificates-alydns-au](https://github.com/yangrongzhou/certbot-letencrypt-wildcardcertificates-alydns-au)

只需要将脚本下载到服务器上，并且配置好对应的 服务 Open API 密钥即可

参数说明

- certonly：表示采用验证模式，只会获取证书，不会为 web 服务器配置证书
- -manual：表示插件
- -preferred-challenges dns：表示采用 DNS 验证申请者合法性（是不是域名的管理者）
- -dry-run：在实际申请/更新证书前进行测试，强烈推荐
- d：表示需要为那个域名申请证书，可以有多个。
- -manual-auth-hook：在执行命令的时候调用一个 hook 文件
- -manual-cleanup-hook：清除 DNS 添加的记录

| 参数                  | 参数使用说明                                                  |
| --------------------- | ------------------------------------------------------------- |
| --manual-auth-hook    | 在执行命令的时候调用的 hook 脚本文件，用于鉴权使用            |
| --manual-cleanup-hook | 在执行命令的时候调用的 hook 脚本文件，用于清理 DNS TXT Record |

## **2.1 安装 Certbot**

同 1.1 步骤，这里不赘述了

## **2.2 获取证书**

```shell
certbot certonly -d *.kuangyichen.com --manual --preferred-challenges dns --dry-run --manual-auth-hook "/data/server/certbot-letencrypt-wildcardcertificates-alydns-au/au.sh python txy add" --manual-cleanup-hook "/data/server/certbot-letencrypt-wildcardcertificates-alydns-au/au.sh python txy clean" --logs-dir logs --config-dir config --work-dir work
```

## **2.3 更新证书**

```shell
certbot renew  --manual --preferred-challenges dns --manual-auth-hook "/data/server/certbot-letencrypt-wildcardcertificates-alydns-au/au.sh python txy add" --manual-cleanup-hook "/data/server/certbot-letencrypt-wildcardcertificates-alydns-au/au.sh python txy clean" --logs-dir logs --config-dir config --work-dir work --dry-run
```

# 三、其他

## **3.1 如何查看证书过期时间？**

**命令行方式**

```shell
openssl x509 -in /root/config/live/example.com/fullchain.pem -noout -enddate
```

会打印出证书到期时间，如下所示

```shell
notAfter=Mar 20 07:20:27 2025 GMT
```

**Web 页面直接查看**

打开对应网站，点击浏览器链接输入栏旁的锁图样，点击连接安全，点击证书图样，即可打开证书查看，如下图所示：

![](https://image.kuangyichen.com/image/my_note/202412201832317.png)

![](https://image.kuangyichen.com/image/my_note/202412201832278.png)

![](https://image.kuangyichen.com/image/my_note/202412201831279.png)

# 参考

[https://certbot.eff.org/](https://certbot.eff.org/)

[https://eff-certbot.readthedocs.io/](https://eff-certbot.readthedocs.io/)

[https://certbot.eff.org/glossary](https://certbot.eff.org/glossary)
