---
password: ""
icon: ""
date: "2021-09-20"
type: Post
category: 技术分享
slug: redis-message-pub-sub
tags:
  - Redis
  - 开发
summary: 在软件架构中，发布-订阅是一种消息传递模式，其中消息的发送者（称为发布者）不会将消息编程为直接发送给特定的接收者（称为订阅者），而是将已发布的消息分类到不知道哪些订阅者的情况下。 类似地，订阅者表示对一个或多个频道感兴趣并且只接收感兴趣的消息，而不知道有哪些发布者。本文讲述了Redis中的消息发布订阅
title: Redis中的消息发布订阅
status: Published
cover: "https://images.unsplash.com/photo-1511707171634-5f897ff02aa9?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 8522b1ba-f2b4-4ac7-b791-482e0959ce27
updated: "2023-08-06 16:48:00"
---

# **什么是发布订阅模式**

首先我们需要了解什么是[**发布订阅模式**](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern)（Publish–Subscribe Pattern）。

Wiki 百科如是说道：

> 在软件架构中，发布-订阅是一种消息传递模式，其中消息的发送者（称为发布者）不会将消息编程为直接发送给特定的接收者（称为订阅者），而是将已发布的消息分类到不知道哪些订阅者的情况下。 类似地，订阅者表示对一个或多个频道感兴趣并且只接收感兴趣的消息，而不知道有哪些发布者。

    发布-订阅是消息队列范式的兄弟，通常是更大的面向消息的中间件系统的一部分。 大多数消息传递系统在其 API 中同时支持发布/订阅和消息队列模型； 例如，Java 消息服务 (JMS)。


    这种模式提供了更大的网络可扩展性和更动态的网络拓扑，从而降低了修改发布者和发布数据结构的灵活性。

# **Redis 中的发布订阅**

在 Redis 中，以下命令

- [**PSUBSCRIBE**](https://redis.io/commands/psubscribe)
- [**PUBLISH**](https://redis.io/commands/publish)
- [**PUBSUB**](https://redis.io/commands/pubsub)
- [**PUNSUBSCRIBE**](https://redis.io/commands/punsubscribe)
- [**SUBSCRIBE**](https://redis.io/commands/subscribe)
- [**UNSUBSCRIBE**](https://redis.io/commands/unsubscribe)

实现了发布订阅模式

例如，对于`SUBSCRIBE`命令，可以通过运行如下命令来订阅频道，后面可以接一个或者多个频道名

```shell
 SUBSCRIBE channel1 channel2
```

取消订阅命令，与订阅命令一致，也可以接一个或者多个频道名

```shell
 UNSUBSCRIBE channel1 channel2
```

当有客户端发送消息到这些频道时，Redis 将会推送传入的消息给所有订阅这些频道的客户端。

正在订阅频道的客户端不应该发送除了订阅或取消订阅以外的命令。订阅和退订操作的执行结果以消息的形式返回，客户端可以读取收到消息的第一个元素来区分收到的是消息类型，还是订阅和退订操作执行结果。

进入订阅模式后，订阅客户端只能执行以下命令：

[**SUBSCRIBE**](https://redis.io/commands/subscribe), [**PSUBSCRIBE**](https://redis.io/commands/psubscribe), [**UNSUBSCRIBE**](https://redis.io/commands/unsubscribe), [**PUNSUBSCRIBE**](https://redis.io/commands/punsubscribe), [**PING**](https://redis.io/commands/ping), [**RESET**](https://redis.io/commands/reset), [**QUIT**](https://redis.io/commands/quit)

> 📌 注意：在>=6.2 版本才能使用 RESET 命令

下图为实例的实际效果

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/3f18a281-69e1-42ca-980d-fd8f5e38ccbb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120752Z&X-Amz-Expires=3600&X-Amz-Signature=2c187dd0aea6105cb63ca074f9ead752716582c2ab3703f8af40e47c8342611c&X-Amz-SignedHeaders=host&x-id=GetObject)

## **推送消息的格式说明**

消息是一个带有三个元素的 Array Reply

第一个元素只能是下面三种之一：

- `subscribe`: 表示成功订阅到以第二个返回元素命名的频道。第三个元素表示客户端当前订阅频道总数。
- `unsubscribe`: 表示成功退订以第二个返回元素命名的频道。第三个元素表示客户端当前订阅频道总数。当第三个参数为零的时候，表示客户端已退出 Pub/Sub 状态，不再订阅任何频道，可以发送订阅和退订以外的 Redis 命令。
- `message`: 表示收到其它某一个客户端用 [**PUBLISH**](https://redis.com.cn/commands/publish) 发布的消息，第二个元素是来源频道的名字，第三个参数是消息实际内容。

## **实际效果**

**客户端 1——发布者**

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/bd6dbc49-676e-42f1-8619-2e641b554075/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120752Z&X-Amz-Expires=3600&X-Amz-Signature=50c3814ec420a68560e1b53745dc22f094cce5b96ddc61b3355f5fc5752cb7b0&X-Amz-SignedHeaders=host&x-id=GetObject)

**客户端 2——订阅者**

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/b1ec6dc0-6113-4ff8-b5c5-8839ec663c30/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120752Z&X-Amz-Expires=3600&X-Amz-Signature=420843ecd752bc93168f12aa38c3249ca2580e7037fd61b3e0c38bf2c9fa42e0&X-Amz-SignedHeaders=host&x-id=GetObject)

可以看到，发布者在 channel1 与 channel 2 频道各发送了两条信息，订阅者因为订阅两个频道，接收到四条信息。四个 Array Reply。

## **带有模式匹配的订阅**

可以看到，只是在`SUBSCRIBE`以及`UNSUBSCRIBE`前面增加了一个`P`，这里的`P`就是 Pattern 的意思，使用方法同`SUBSCRIBE`以及`UNSUBSCRIBE`，只是传参为 Pattern 模式

- [**PSUBSCRIBE**](https://redis.io/commands/psubscribe)
  - `PSUBSCRIBE pattern [pattern ...]`
  - 订阅符合模式的对应的频道，支持 glob
    - `h?llo` subscribes to `hello`, `hallo` and `hxllo`
    - `h*llo` subscribes to `hllo` and `heeeello`
    - `h[ae]llo` subscribes to `hello` and `hallo,` but not `hillo`
- [**PUNSUBSCRIBE**](https://redis.io/commands/punsubscribe)
  - `PUNSUBSCRIBE [pattern [pattern ...]]`
  - 取消订阅符合模式的对应的频道，如果为空，则取消所有

# **实现 Redis 中的发布订阅模式**

**订阅者**

```python
 from redis_ins.client import Client
 
 channel = "eachen_radio"
 client = Client()
 
 while True:
     spub = client.instance.pubsub()
     spub.subscribe(channel)
     for items in spub.listen():
         print(items)
 
```

**发布者**

```python
 from redis_ins.client import Client
 
 channel = "eachen_radio"
 client = Client()
 
 while True:
     my_input = input('请输入发布内容：')
     client.instance.publish(channel, my_input)
```

同时运行两个客户端代码

在【发布者】的命令行中输入消息

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/1104e3ee-424a-46a0-a1f0-4b40a29a8f47/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120752Z&X-Amz-Expires=3600&X-Amz-Signature=117516166e7cd3c32374a361f625bb5ce6cbc12626a94ee73a02950b53ced6ef&X-Amz-SignedHeaders=host&x-id=GetObject)

在【订阅者】的命令行中可以看到收到了消息

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/dc96497c-69e8-4c66-85f2-84556024619f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120752Z&X-Amz-Expires=3600&X-Amz-Signature=91501e7da72c4a6f8bf2dc00116a71ab17b64f72eb4689ac0151647701e546db&X-Amz-SignedHeaders=host&x-id=GetObject)

发布者在频道`eachen_radio`上发布消息，订阅者在同一个频道上接受消息
