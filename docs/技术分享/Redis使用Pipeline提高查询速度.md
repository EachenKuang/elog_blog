---
password: ""
icon: ""
date: "2021-09-16"
type: Post
category: 技术分享
slug: redis-usding-pipeline-speed-up
tags:
  - Redis
  - 开发
summary: Redis是一种高性能的内存数据库，它可以快速地存储和检索数据。但是，当需要执行大量的Redis查询时，每个查询都需要与Redis服务器进行通信，这可能会导致性能瓶颈。为了解决这个问题，Redis提供了Pipeline机制，它可以将多个查询打包在一起，一次性发送给Redis服务器，从而提高查询速度。在本文中，我们将介绍如何使用Redis Pipeline机制来提高查询速度，并探讨Pipeline机制的工作原理和使用方法。
title: Redis使用Pipeline提高查询速度
status: Published
cover: "https://images.unsplash.com/photo-1605600659873-d808a13e4d2a?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: c3e54c4b-ad14-484c-aeac-de9296dc24e6
updated: "2023-08-06 16:48:00"
---

> 😀 Redis 是一种高性能的内存数据库，它可以快速地存储和检索数据。但是，当需要执行大量的 Redis 查询时，每个查询都需要与 Redis 服务器进行通信，这可能会导致性能瓶颈。为了解决这个问题，Redis 提供了 Pipeline 机制，它可以将多个查询打包在一起，一次性发送给 Redis 服务器，从而提高查询速度。在本文中，我们将介绍如何使用 Redis Pipeline 机制来提高查询速度，并探讨 Pipeline 机制的工作原理和使用方法。

# 📝 主旨内容

## **什么是 RTT (Round Trip Time 往返时间)？**

在维基百科中，是这样[**介绍**](https://en.wikipedia.org/wiki/Round-trip_delay)的：

在电讯方面， **round-trip delay** (**RTD**) 或 **round-trip time** (**RTT**) 是指发送信号所需的时间，加上确认接收信号所需的时间。此时间包括两个通信端点之间的路径的传播时间。在计算机网络中，信号通常是一个数据包。RTT 也称为 ping 时间，可以通过 ping 命令确定。

可以这样理解，从客户端发送信号开始，到服务端接受信号后，客户端收到服务端返回的信号这一过程所花费的时间。

## **什么是 Redis 中的 Pipelining?**

对于基于 Request/Response 的服务器可以被实现成处理新的请求，即使客户端还没收到旧的响应。这样就可以同时发送多个命令给服务器而不用等待响应，最后统一读回多个返回。

这种方式被叫做 pipelining, 是一个广泛使用多年的技术，批量发送，批量返回。例如 POP3 协议也是这样，来加速从服务器上下载邮件。

## **为什么要使用 Pipeline 提高 Redis 查询速度？**

Redis 在服务器与客户端之间传输信号是存在网络延时的，这个时间就是 RTT。

试想，如果算上 RTT 的网络延迟影响，如果客户端需要连续发送多个请求的情况下，RTT 对性能的影响是很严重的。假如 RTT 的网络延时是 250ms，尽管 Redis 服务端每秒能处理 10 万个请求，但是我们也只能每秒最多处理四个请求。

如果能够将这里请求一并传给服务端处理，那么久不需要每次都因为延时影响传输查询效率了。

所以，对于大量的请求，我们可以使用 Pipeline 来一次性传输请求命令，最后统一读回数据。

> 💡 特别注意: 当客户端使用管道 pipelining 发送命令时，服务器端需要消耗内存来存放响应，所以如果你需要发送大量的命令，最好分批发送，例如一次发送 1 万个，读取回报，再循环发剩余的命令。速度上几乎无差异，但是内存最大消耗 1 万个命令回复结果的内存。

## **基准测试**

使用 Python-Redis 进行基准测试，可以看到结果差异还是比较大的。

```python
 # benchmark.py
 
 from redis_ins.client import Client
 from redis_ins.time_decorator import run_time
 
 loop_round = 1000
 
 
 @run_time
 def with_pipeline(client: Client):
     pipeline = client.instance.pipeline()
     for i in range(loop_round):
         pipeline.ping()
     pipeline.execute()
 
 
 @run_time
 def without_pipeline(client: Client):
     for i in range(loop_round):
         client.instance.ping()
 
 
 if __name__ == '__main__':
     client = Client()
     print("with pipeline")
     with_pipeline(client)
     print("without pipeline")
     without_pipeline(client)

```

循环 1000 次的耗时对比

```text
 with pipeline
 函数with_pipeline运行开始:1631769580.494915
 函数with_pipeline运行结束:1631769580.573953
 函数with_pipeline执行了0.079秒
 without pipeline
 函数without_pipeline运行开始:1631769580.5739899
 函数without_pipeline运行结束:1631769591.982709
 函数without_pipeline执行了11.4087秒
```

循环 10000 次的耗时对比

```text
 with pipeline
 函数with_pipeline运行开始:1631769352.6500359
 函数with_pipeline运行结束:1631769352.873387
 函数with_pipeline执行了0.2234秒
 without pipeline
 函数without_pipeline运行开始:1631769352.873431
 函数without_pipeline运行结束:1631769461.964499
 函数without_pipeline执行了109.0911秒
```

|                 | 1000 次循环 | 10000 次循环 |
| --------------- | ----------- | ------------ |
| 使用 pipeline   | 0.079 秒    | 0.2234 秒    |
| 不使用 pipeline | 11.4087 秒  | 109.0911 秒  |

# 🤗 总结归纳

可以看出，在 Redis 中，Redis 提供了 Pipeline 机制，它可以将多个查询打包在一起，一次性发送给 Redis 服务器，从而提高查询速度。

# 📎 参考文章

- [**Using pipelining to speedup Redis queries – Redis**](https://redis.io/topics/pipelining)
