---
password: ""
icon: ""
date: "2021-09-14"
type: Post
category: 技术分享
slug: resp
tags:
  - Redis
  - 开发
summary: Redis客户端和服务器端通信使用名为 RESP (REdis Serialization Protocol) 的协议。
title: Redis 协议详细规范
status: Published
cover: "https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/33d23e03-f43d-4c01-84ae-9b4733c2aad2/redis.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120308Z&X-Amz-Expires=3600&X-Amz-Signature=3134d7d3840a4de311ef2cd43acf1c5d403c14328760dc78977b4365aa9af1bc&X-Amz-SignedHeaders=host&x-id=GetObject"
urlname: b1eecfd0-b513-46a9-a50b-5cffc35e6542
updated: "2023-08-07 20:29:00"
---

> 😀 [**Redis Protocol specification – Redis**](https://redis.io/topics/protocol)  
> Redis 客户端和服务器端通信使用名为 **RESP** (REdis Serialization Protocol) 的协议。虽然这个协议是专门为 Redis 设计的，它也可以用在其它 client-server 通信模式的软件上。
>
> RESP 是下面条件的折中：
>
> - 实现起来简单。
> - 解析速度快。
> - 有可读性。
>
> `RESP` 能序列化不同的数据类型，例如整型(integers)、字符串(strings)、数组(arrays)。额外还有特殊的错误类型。请求从客户端以字符串数组的形式发送到 redis 服务器，这些字符串表示要执行的命令的参数。Redis 用特定于命令的数据类型回复。
>
> `RESP` 是**二进制安全**的，并且不需要处理从一个进程发到另外一个进程的批量数据，因为它使用前缀长度来传输批量数据。
>
> 注意：这里概述的协议仅用于**客户机-服务器**通信。Redis 集群使用不同的二进制协议在节点之间交换消息。

    Redis客户端和服务器端通信使用名为 **RESP** (REdis Serialization Protocol) 的协议。虽然这个协议是专门为Redis设计的，它也可以用在其它 client-server 通信模式的软件上。


    RESP 是下面条件的折中：

    - 实现起来简单。
    - 解析速度快。
    - 有可读性。

    `RESP` 能序列化不同的数据类型，例如整型(integers)、字符串(strings)、数组(arrays)。额外还有特殊的错误类型。请求从客户端以字符串数组的形式发送到redis服务器，这些字符串表示要执行的命令的参数。Redis用特定于命令的数据类型回复。


    `RESP` 是**二进制安全**的，并且不需要处理从一个进程发到另外一个进程的批量数据，因为它使用前缀长度来传输批量数据。


    注意：这里概述的协议仅用于**客户机-服务器**通信。Redis集群使用不同的二进制协议在节点之间交换消息。

## **网络层（Networking layer）**

默认情况下，连到 Redis 服务器的客户端会建立了一个到`6379`端口的 TCP 连接。

虽然 RESP 在技术上不特定于 TCP，但是在 Redis 的上下文中，该协议仅用于 TCP 连接（或类似的面向流的连接，如 unix 套接字）。

## **请求-响应模型（ Request-Response model）**

Redis 接受由不同参数组成的命令。一旦收到命令，就会对其进行处理，并将应答发送回客户端。

这是最简单的模型，但是有两个例外：

- **管道模式下**。Redis 支持管道（pipelining）。所以，客户端可以一次发送多个命令，然后再等待应答。
- **发布订阅模式下**。当一个 Redis 客户端订阅一个频道，那么协议会改变语义并变成`push protocol`, 也就是说，客户客户端不再需要发送命令，因为服务器端会一收到新消息，就会自动发送给客户端。

除了上面两个例外情况，Redis 协议是一个简单的请求-响应协议。

## **RESP 协议解释**

> RESP 协议在 Redis1.2 被引入，直到 Redis2.0 才成为和 Redis 服务器通信的标准。

`RESP` 是一个支持多种数据类型的序列化协议：

- 简单字符串（Simple Strings），也被翻译为“单行字符串”
- 错误（ Errors）
- 整型（ Integers）
- 大容量字符串（Bulk Strings），也被翻译为“多行字符串”
- 数组（Arrays）

如上图所示，`RESP`在 Redis 中作为一个请求-响应协议以如下方式使用：

- 客户端以`Bulk Strings`组成的`RESP Arrays`的方式发送命令给服务器端。
- 服务器端根据命令的具体实现返回某一种 RESP 数据类型。

## **RESP Simple Strings**

编码方式：

- 加号 与 实际字符串 与 CRLF 结尾 =>
- `+` + `实际字符串`+ `\r\n`

  例如：

  ```text
   +OK\r\n
  ```

> 注意：实际字符串中不允许包含回车换行符

如果需要发送二进制安全的字符串，应该选择使用 RESP 的大容量字符串（Bulk Strings）替代。

## **RESP Errors**

编码方式：

- 减号 与 错误字符串 与 CRLF 结尾 =>
- - `错误字符串` +`\r\n`

  例如：

  ```text
   -Error message\r\n
  ```

Errors 用于在发生异常时传输

## **RESP Integers**

编码方式：

- 冒号 与 字符串形式的数字 与 CRLF 结尾 =>
- `:` + `数字字符串` + `\r\n`

  例如：

  ```text
   :1\r\n
   :0\r\n
  ```

> 注意：返回的整数有效值需要在有符号 64 位整数范围内。

## **RESP Bulk Strings**

编码方式由三部分组成：

- 第一部分：美元符号 与 组成字符串的字节数 与 CRLF 结尾
- 第二部分：实际的字符串数据
- 第三部分：CRLF 即为

  例如：

  ```text
   $6\r\n
   greate
   \r\n
  ```

  为了便于显示，上面以换行处理各部分字符，实际中传输使用的是如下：

  ```text
   $6\r\ngreate\r\n
  ```

注意：

有两种特殊形式的 Bulk Strings：

- 空串（数据分布长度为 0，表示空串""）

  ```text
   $0\r\n
  ```

- **Null Bulk String**（在这种格式里长度值为-1，数据部分不存在，所以为 Null）

  ```text
   $-1\r\n
  ```

> 大容量字符串被用来表示最长 512MB 的二进制安全字符串。

## **RESP Arrays**

编码方式：

- 第一部分：星号 与 数组元素的个数的十进制数 与 CRLF 结尾
- 第二部分：数组中每个 RESP 类型的元素（按照上面提到的各种类型的编码方式）

示例：

- 空数组

  ```text
   *0\r\n
  ```

- 正常数组

  ```text
  *3\r\n:1\r\n:2\r\n:3\r\n
  ```

  对应 [1,2,3]

- 带多种类型的嵌套数组

  ```text
  *2\r\n
  *3\r\n
  :1\r\n
  :2\r\n
  :3\r\n
  *2\r\n
  +Foo\r\n
  -Bar\r\n
  ```

  (为了方便阅读，分成多行来展示).

  对应 [[1, 2, 3], ["Foo", "Bar"]]

- NULL 数组（由于历史原因，还存在 NULL 值的数组，通常使用 NULL Bulk Strings）

  ```text
  *-1\r\n
  ```

- 数组中的 Null 元素

  ```text
  *3\r\n
  $3\r\n
  foo\r\n
  $-1\r\n
  $3\r\n
  bar\r\n
  ```

  (为了方便阅读，分成多行来展示)

  对应 ["foo", Null, "bar"]

## **发送命令到 Redis 服务器**

至此，我们已经很熟悉 RESP 序列化格式，写一个 Redis 客户端库的实现会变得很容易。我们可以进一步说明客户端和服务端如何交互工作：

- 客户端发送包含只有多行字符串的数组给 Redis 服务器。
- Redis 服务器给客户端发送任意有效的 RESP 数据类型作为应答。

下面是一个典型的交互过程例子：

客户端发送命令 `LLEN mylist` 来获取存储在 `mylist` 键中列表的长读，然后服务器端返回整数应答(C: 代表客户端, S: 代表服务器端).

```text
C: *2\r\n
C: $4\r\n
C: LLEN\r\n
C: $6\r\n
C: mylist\r\n

S: :48293\r\n
```

为了方便理解我们用换行把协议分成不同部分，实际上客户端发送的是一个整体没有换行：

```text
*2\r\n$4\r\nLLEN\r\n$6\r\nmylist\r\n
```

如果想体现使用 RESP 来编写命令的话，可以下载 netcat 工具体验一把。

```text
yum install nc
```

首先需要有一个运行的 redis-server

```text
redis-server
```

然后就可以操作了

```text
> echo -e "*1\r\n\$4\r\nPING\r\n" | nc 127.0.0.1 6379
+PONG
```

## **管道和多个命令**

客户端可以使用同一个连接发送多个命令。通过管道客户端可以一次写操作发送多个命令，发送下一个命令前不需要等待前一个命令的应答。所有应答可以在最后被读取。

关于管道详细参考 [**page about Pipelining**](https://redis.io/topics/pipelining).

## **内联命令**

有时你手边只能操作`telnet` 并且需要给 Redis 服务器端发送命令。虽然 Redis 协议是容易实现的，但并不适合用在交互会话。`redis-cli` 也不是随时都能可用。因此，redis 还以一种特殊的方式接受为人类设计的命令，称为内联命令格式。

以下是使用内联命令进行服务器/客户端聊天的示例（服务器聊天以 s 开头，客户端聊天以 c 开头）。

```text
C: PING
S: +PONG
```

以下是返回整数的内联命令的另一个示例：

```text
C: EXISTS somekey
S: :0
```

基本上，您只需在 telnet 会话中编写空格分隔的参数。由于统一请求协议中没有以\*开头的命令，因此 Redis 能够检测到这种情况并解析您的命令。
