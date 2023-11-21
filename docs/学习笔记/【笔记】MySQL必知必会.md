---
password: ""
icon: ""
date: "2022-01-06"
type: Post
category: 学习笔记
slug: mysql-crash-course
tags:
  - SQL
  - MySQL
  - 数据库
  - 必看精选
summary: 最近重读了一遍《MySQL必知必会》，然后对重要的部分摘抄了一些笔记，希望能够作重温下经典的基础上，巩固下自己对于MySQL的理解。篇幅可能有点长，可以通过目录快速查看。
title: 【笔记】MySQL必知必会
status: Published
cover: "https://images.unsplash.com/photo-1662026911591-335639b11db6?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 1a7db346-40d8-4c8e-9b61-7ef9ab008dc2
updated: "2023-09-26 15:51:00"
---

> 😀 最近重读了一遍《MySQL 必知必会》，然后对重要的部分摘抄了一些笔记，希望能够作重温下经典的基础上，巩固下自己对于 MySQL 的理解。篇幅可能有点长，可以通过目录快速查看。

# **第一章 了解数据库**

## **1.1 数据库基础**

### **1.1.1 数据库**

> 数据库（database） ：保存有组织的数据的容器（通常是一个文件或一组文件）

DBMS(Database Manager System，数据库管理系统)，确切地说，数据库软件应称为`DBMS`

数据库 是通过 DBMS 创建和操纵的容器。数据库可以是保存在硬设备 上的文件，但也可以不是。在很大程度上说，数据库究竟是 文件还是别的什么东西并不重要，因为你并不直接访问数据 库;你使用的是 DBMS，它替你访问数据库。

### **1.1.2 表**

> 表(table) ：某种特定类型数据的结构化清单

关于表名的唯一性

> 表名： 表名的唯一性取决于多个因素，如数据库名和表名等的 结合。这表示，虽然在相同数据库中不能两次使用相同的表名， 但在不同的数据库中却可以使用相同的表名。

=> 同一个数据库中的表不可重复，不同数据库中的表可以重名

> 模式(schema) ：关于数据库和表的布局及特性的信息。

表具有一些特性，这些特性定义了数据在表中如何存储，如可以存 储什么样的数据，数据如何分解，各部分信息如何命名，等等。描述表的这组信息就是所谓的模式，模式可以用来描述数据库中特定的表以及 整个数据库(和其中表的关系)。

### **1.1.3 列和数据类型**

> 列(column)： 表中的一个字段。所有表都是由一个或多个列组 成的。

> 数据类型(datatype) ：所容许的数据的类型。每个表列都有相 应的数据类型，它限制(或容许)该列中存储的数据。

不同的数据库类型有着不同的数据类型，大体含有 int、float、double、date、text 等

### **1.1.4 行**

> 行(row)： 表中的一个记录。

表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

### **1.1.5 主键**

注：也叫主码

> 主键(primary key)：一列(或一组列)，其值能够唯一区分表中每个行。

主键用来表示 一个特定的行。没有主键，更新或删除表中特定行很困难，因为没有安 全的方法保证只涉及相关的行。

表中的任何列都可以作为主键，只要它满足以下条件:

- 任意两行都不具有相同的主键值
- 每个行都必须具有一个主键值(主键列不允许 NULL 值)

**主键的最好习惯**

除 MySQL 强制实施的规则外，应该坚持的 几个普遍认可的最好习惯为:

- 不更新主键列中的值;
- 不重用主键列的值;
- 不在主键列中使用可能会更改的值。(例如，如果使用一个名字作为主键以标识某个供应商，当该供应商合并和更改其名字时，必须更改这个主键。)

## **1.2 什么是 SQL**

> SQL(发音为字母 S-Q-L 或 sequel)是结构化查询语言(Structured Query Language)的缩写。SQL 是一种专门用来与数据库通信的语言。

SQL 有如下的优点。

- SQL 不是某个特定数据库供应商专有的语言。几乎所有重要的 DBMS 都支持 SQL，所以，学习此语言使你几乎能与所有数据库 打交道。
- SQL 简单易学。它的语句全都是由描述性很强的英语单词组成， 而且这些单词的数目不多。
- SQL 尽管看上去很简单，但它实际上是一种强有力的语言，灵活 使用其语言元素，可以进行非常复杂和高级的数据库操作。

# **第二章 MySQL 简介**

MySQL、Oracle 以及 Microsoft SQL Server 等数据库是基于客户机-服务器的数据库。客户机-服务器应用分为两个不同的部分。

- `服务器`部分（Server）是负责所有数据访问和处理的一个软件。这个软件运行在称为数据库服务器的计算机上。
- `客户机`部分（Client）是与用户打交道的软件。

服务器软件为 MySQL DBMS。你可以在本地安装的副本上运行， 也可以连接到运行在你具有访问权的远程服务器上的一个副本。

客户机可以是 MySQL 提供的工具、脚本语言(如 Perl)、Web 应用 开发语言(如 ASP、ColdFusion、JSP 和 PHP)、程序设计语言(如 C、C++、Java)等。

# **第三章 使用 MySQL**

## **3.1 连接**

- 主机名
- 端口
- 用户名
- 用户口令

**连接数据库通用格式:**`mysql -P <端口号> -h <mysql主机名或ip地址> -u <用户名> -p <口令>`

1. 本地连接
   如果是命令行是 mysql 所在的本机,而且用默认的端口 3306 时,可以简化语句为:

```sql
 mysql -u root -p
```

1. 远程连接
   注意: 使用远程连接时,使用的连接用户和该用户现在的 ip 地址应该是远程数据库中允许的用户和允许的 ip,否则是不允许连接的.

```sql
 mysql -P 3306 -h 192.168.1.104 -u root -p
```

## **3.2 选择数据库**

```sql
 use <database-name>;
```

使用`USE`关键字

## **3.3 了解数据库和表**

1. 查看所有数据库

```sql
 show databases;
```

1. 创建数据库

```sql
 create database db_eachen;
```

1. 使用数据库

```sql
 use db_eachen;
```

1. 显示数据库中所有表

```sql
 show tables;
```

1. 查看表结构

```sql
 show columns from customers;
```

或者使用快捷方式:

```sql
 DESCRIBE customers;
```

> DESCRIBE 语句 MySQL 支持用 DESCRIBE 作为 SHOW COLUMNS FROM 的一种快捷方式。换句话说，DESCRIBE customers;是 SHOW COLUMNS FROM customers;的一种快捷方式。

其他关于`SHOW`的语句

- `SHOW STATUS`，用于显示广泛的服务器状态信息;
- `SHOW CREATE DATABASE`和`SHOW CREATE TABLE`，分别用来显示创建特定数据库或表的 MySQL 语句;
- `SHOW GRANTS`，用来显示授予用户(所有用户或特定用户)的安全权限;
- `SHOW ERRORS`和`SHOW WARNINGS`，用来显示服务器错误或警告消息。

# **第四章 检索数据**

需要注意的 tips:

> SQL 是不区分大小写的【不过，一定要认识到虽然 SQL 是不区分大小写的，但有些标识 符(如数据库名、表名、列名)可能不同:在 MySQL 4.1 及之 前的版本中，这些标识符默认是区分大小写的;在 MySQL 4.1.1 版本中，这些标识符默认是不区分大小写的。】建议还是区分大小写，对于 SQL 关键字大写，对于所有列和表明使用小写，这样做使代码更易于阅读和调试。使用空格 在处理 SQL 语句时，其中所有空格都被忽略。SQL 语句可以在一行上给出，也可以分成许多行。多数 SQL 开发人 员认为将 SQL 语句分成多行更容易阅读和调试。

注意 1：

> 如果没有 明确排序查询结果(下一章介绍)，则返回的数据的顺序没有 特殊意义。返回数据的顺序可能是数据被添加到表中的顺序， 也可能不是。只要返回相同数目的行，就是正常的。

使用方法：

```sql
 # 检索一个字段
 SELECT <field> FROM <table>;
 # 检索多个字段
 SELECT <field1>, <field1> FROM <table>;
 # 检索所有字段
 SELECT * FROM <table>;
 # 检索去重 (见注意2)
 SELECT distinct <field> FROM <table>;
 # 限制结果 （见注意3）
 SELECT * FROM <table> LIMIT 5;
```

注意 2：

> 不能部分使用 DISTINCT DISTINCT 关键字应用于所有列而 不仅是前置它的列。如果给出 SELECT DISTINCT vend_id, prod_price，除非指定的两个列都不同，否则所有行都将被 检索出来。

注意 3：

> LIMIT 关键字使用方式

    `LIMIT 1`  默认从0开始计数，可以理解为 `LIMIT 0,1`


    `LIMIT 3,4`  == `LIMIT 4 OFFSET 3` （两者等价）

    - 检索出来的第一行为行0而不是行1。
    - LIMIT中指定要检索的行数为检索的最大行数。
    - MySQL 5支持LIMIT的另一种替代语法 ——`LIMIT 4 OFFSET 3`

# **第五章 排序检索数据**

`ORDER BY`

默认升序 `ASC`（可不写），降序使用 `DESC` 关键字

```sql
 # 可以指定多个字段进行排序
 # 如果想在多个列上进行降序排序，必须 对每个列指定DESC关键字。
 SELECT * FROM <table> ORDER BY <field>;
 SELECT * FROM <table> ORDER BY <field1>, <field2>;
 SELECT * FROM <table> ORDER BY <field1> DESC, <field2>;
```

注意 1：

> 区分大小写和排序顺序 在对文本性的数据进行排序时，A 与 a 相同吗?a 位于 B 之前还是位于 Z 之后?这些问题不是理论问 题，其答案取决于数据库如何设置。

    在字典(dictionary)排序顺序中，A被视为与a相同，这是MySQL (和大多数数据库管理系统)的默认行为。但是，许多数据库 管理员能够在需要时改变这种行为(如果你的数据库包含大量


    外语字符，可能必须这样做)。


    这里，关键的问题是，如果确实需要改变这种排序顺序，用简 单的ORDER BY子句做不到。你必须请求数据库管理员的帮助。

```sql
 # 找出最贵的物品的价格
 select prod_price from products order by prod_price DESC LIMIT 1;
```

# **第六章 过滤数据**

`WHERE`

WHERE 子句操作符

```sql
 = 等于
 <> 不等于
 != 不等于
 < 小于
 <= 小于等于
 > 大于
 >= 大于等于
 BETWEEN 在指定的两个值之间
```

注意 1：

> 何时使用引号 如果仔细观察上述 WHERE 子句中使用的条件， 会看到有的值括在单引号内(如前面使用的'fuses')，而有 的值未括起来。单引号用来限定字符串。如果将值与串类型的 列进行比较，则需要限定引号。用来与数值列进行比较的值不 用引号。

```sql
 SELECT prod_price FROM products WHERE prod_price = 1;
 SELECT prod_price FROM products WHERE prod_price <> 1;
 SELECT prod_price FROM products WHERE prod_price != 1;
 SELECT prod_price FROM products WHERE prod_price <= 1;
 SELECT prod_price FROM products WHERE prod_price >= 1;
 SELECT prod_price FROM products WHERE prod_price BETWEEN 0 AND 2;
```

检查空值

```sql
 SELECT prod_price FROM products WHERE prod_price IS NULL
```

注意 2：

> NULL 与不匹配 在通过过滤选择出不具有特定值的行时，你 可能希望返回具有 NULL 值的行。但是，不行。因为未知具有 特殊的含义，数据库不知道它们是否匹配，所以在匹配过滤 或不匹配过滤时不返回它们。

    因此，在过滤数据时，一定要验证返回数据中确实给出了被 过滤列具有NULL的行。

# **第七章 数据过滤**

`WHERE`

## **逻辑组合**

`AND`

```sql
 SELECT prod_price FROM products WHERE prod_price = 1 AND prod_name = "T";
```

`OR`

```sql
 SELECT prod_price FROM products WHERE prod_price = 1 OR prod_name = "T";
```

注意：

> OR 与 AND 存在优先级计算的问题

    AND 优先级高于 OR


    `<expres1> OR <expres2> AND <expres3>`


    等价于 `<expres1> OR (<expres2> AND <expres3)>`


    如果要先算前两者，则需要修改为 `(<expres1> OR <expres2>) AND <expres3>`


    **建议**：任何时候在具有AND以及OR的操作符的子句中，都应该使用圆括号明确分组操作符。

`IN`

```sql
 SELECT prod_price FROM products WHERE prod_name in ("T", "X");
```

这个等价于

```sql
 SELECT prod_price FROM products WHERE prod_name = "T" OR prod_name = "X";
```

优点：

- 在使用长的合法选项清单时，IN 操作符的语法更清楚且更直观。
- 在使用 IN 时，计算的次序更容易管理(因为使用的操作符更少)。
- IN 操作符一般比 OR 操作符清单执行更快
- IN 的最大优点是可以包含其他 SELECT 语句，使得能够更动态地建

  立 WHERE 子句

`NOT`

```sql
 SELECT prod_price FROM products WHERE prod_name NOT in ("T", "X");
```

注意 2：

> MySQL 中的 NOT MySQL 支持使用 NOT 对 IN、BETWEEN 和 EXISTS 子句取反，这与多数其他 DBMS 允许使用 NOT 对各种条件 取反有很大的差别。

# **第八章 用通配符进行过滤**

## **`LIKE`**

```sql
 SELECT prod_id, prod_name FROM products WHERE prod_name LIKE 'jet%';
```

```text
 +---------+--------------+
 | prod_id | prod_name    |
 +---------+--------------+
 | JP1000  | JetPack 1000 |
 | JP2000  | JetPack 2000 |
 +---------+--------------+
 2 rows in set (0.0097 sec)
```

## **`%`\*\***通配符\*\*

匹配任意次数的字符，包括 0 个

`%` 代表搜索模式中给定位置的 0 个、1 个或多个字符。

注意 1：

> 注意 NULL 虽然似乎%通配符可以匹配任何东西，但有一个例 外，即 NULL。即使是 WHERE prod_name LIKE '%'也不能匹配 用值 NULL 作为产品名的行。

## **`_`\*\***通配符\*\*

匹配单个字符

```sql
SELECT prod_id, prod_name FROM products WHERE prod_name LIKE 'Jet_';
```

## **使用技巧**

- 不要过度使用通配符。如果其他操作符能达到相同的目的，应该 使用其他操作符。
- 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的**开始处**，搜索起来是**最慢**的。【这里要特别记住哦】
- 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。

# **第九章 用正则表达式进行搜索**

## **介绍**

注意 1：

> MySQL 只支持正则表达式的部分内容

## **MySQL 正则表达式**

### **基本字符匹配**

`REGEXP`

使用方式

```sql
 REGEXP 'your-pattern'
```

例如：

```sql
SELECT prod_name FROM products WHERE prod_name REGEXP '1000';
```

```text
 +--------------+
 | prod_name    |
 +--------------+
 | JetPack 1000 |
 +--------------+
 1 row in set (0.00 sec)
```

逻辑：如果该列存在字符匹配，则被检索出来

注意 2：

> LIKE 与 REGEXP 的区别

    LIKE匹配整个列。如果被匹配的文本在列值 中出现，LIKE将不会找到它，相应的行也不被返回(除非使用 通配符)。而REGEXP在列值内进行匹配，如果被匹配的文本在 列值中出现，REGEXP将会找到它，相应的行将被返回。这是一个非常重要的差别。使用^和$定位符(anchor)即可完全匹配

### **进行 OR 匹配**

为搜索两个或多个串之一(或者为这个串，或者为另一个串)，使用`|`

```sql
 SELECT prod_name FROM products WHERE prod_name REGEXP '1000|2000';
```

```text
 +--------------+
 | prod_name    |
 +--------------+
 | JetPack 1000 |
 | JetPack 2000 |
 +--------------+
 2 rows in set (0.00 sec)
```

使用`|`从功能上类似于在 SELECT 语句中使用 OR 语句，多个 OR 条件可并 入单个正则表达式。

### **匹配几个字符之一**

匹配任何单一字符。可通过指定一组用`[`和`]`括起来的字符来完成

```sql
 SELECT prod_name FROM products WHERE prod_name REGEXP '[123] Ton';
```

```text
 +-------------+
 | prod_name   |
 +-------------+
 | 1 ton anvil |
 | 2 ton anvil |
 +-------------+
```

### **匹配范围**

[0-9] = [0123456789]

[a-z]

[A-Z]

### **匹配特殊字符(转义)**

为了匹配特殊字符，必须用`\\`为前导。`\\-`表示查找`-`，`\\.`表示查找`.`

```sql
 SELECT prod_name FROM products WHERE prod_name REGEXP '\\.';
```

```text
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
+--------------+
```

`\\`也用来引用元字符(具有特殊含义的字符)

```text
\\f 换页
\\n 换行
\\r 回车
\\t 制表符
\\v 纵向制表符
```

注意 1：

> 匹配\ 为了匹配反斜杠(\)字符本身，需要使用\\\。

这里需要三个反斜杠

### **匹配字符类**

| **类**     | **说明**                                                         |
| ---------- | ---------------------------------------------------------------- |
| [:alnum:]  | 任意数字和字母。相当于[a-zA-Z0-9]                                |
| [:alpha:]  | 任意字符。相当于[a-zA-z]                                         |
| [:blank:]  | 空格和制表。相当于[（双斜杠，segmentfault 这里双斜杠打不出来）t] |
| [:cntrl:]  | ASCII 控制字符（ASCII 0 到 31 和 127）                           |
| [:digit:]  | 任意数字。相当于[0-9]                                            |
| [:graph:]  | 与[:print:]相同，但是不包含空格                                  |
| [:lower:]  | 任意的小写字母。相当于[a-z]                                      |
| [:print:]  | 任意可打印字符                                                   |
| [:punct:]  | 既不在[:alnum:]又不在[:cntrl:]中的任意字符                       |
| [:space:]  | 包括空格在内的任意空白字符。                                     |
| [:upper:]  | 任意大写字母。相当于[A-Z]                                        |
| [:xdigit:] | 任意十六进制的数字。相当于[a-fA-F0-9]                            |

### **匹配元字符**

| **元字符** | **作用**                     |
| ---------- | ---------------------------- |
| \*         | 重复 0 次或者多次            |
| +          | 重复一次或者多次。相当于{1,} |
| ?          | 重复 0 次或者 1 次           |
| {n}        | 重复 n 次                    |
| {n,}       | 重复至少 n 次                |
| {n,m}      | 重复 n-m 次                  |

```sql
SELECT prod_name FROM products WHERE prod_name REGEXP '\\([0-9] sticks?\\)';
```

```text
+----------------+
| prod_name      |
+----------------+
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
```

### **匹配定位符**

除了之前的重复元字符，正则还有一种特殊的定位元字符

| **元字符** | **作用** |
| ---------- | -------- |
| ^          | 文本开始 |
| $          | 文本结尾 |
| [[:<:]]    | 词的开始 |
| [[:>:]]    | 词的结尾 |

```text
^的双重用途 ^有两种用法。在集合中(用[和]定义)，用它 来否定该集合，否则，用来指串的开始处。
```

注意：

> 简单的正则表达式测试 可以在不使用数据库表的情况下用 SELECT 来测试正则表达式。REGEXP 检查总是返回 0(没有匹配) 或 1(匹配)。可以用带文字串的 REGEXP 来测试表达式，并试 验它们。相应的语法如下:

    `SELECT 'hello' REGEXP '[0-9]'`


    这个例子显然将返回0(因为文本hello中没有数字)。

# **第十章 创建计算字段**

## **拼接**

在 MySQL 的 SELECT 语句中，可使用`Concat()`函数来拼接两个列

```sql
 SELECT Concat(vend_name, ' (', vend_country, ')')
 FROM vendors
 ORDER BY vend_name;
```

```text
 +--------------------------------------------+
 | Concat(vend_name, ' (', vend_country, ')') |
 +--------------------------------------------+
 | ACME (USA)                                 |
 | Anvils R Us (USA)                          |
 | Furball Inc. (USA)                         |
 | Jet Set (England)                          |
 | Jouets Et Ours (France)                    |
 | LT Supplies (USA)                          |
 +--------------------------------------------+
```

`Trim函数` MySQL 除了支持`RTrim()`(正如刚才所见，它去掉 串右边的空格)，还支持`LTrim()`(去掉串左边的空格)以及 `Trim()`(去掉串左右两边的空格)。

## **使用别名**

`AS`

```sql
 SELECT Concat(vend_name, ' (', vend_country, ')') AS vend_title FROM vendors ORDER BY vend_name;
```

```text
 +-------------------------+
 | vend_title              |
 +-------------------------+
 | ACME (USA)              |
 | Anvils R Us (USA)       |
 | Furball Inc. (USA)      |
 | Jet Set (England)       |
 | Jouets Et Ours (France) |
 | LT Supplies (USA)       |
 +-------------------------+
```

## **执行算术计算**

算术操作符

```sql
 SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price FROM orderitems WHERE order_num = 20005
```

```text
 +---------+----------+------------+----------------+
 | prod_id | quantity | item_price | expanded_price |
 +---------+----------+------------+----------------+
 | ANV01   |       10 |       5.99 |          59.90 |
 | ANV02   |        3 |       9.99 |          29.97 |
 | TNT2    |        5 |      10.00 |          50.00 |
 | FB      |        1 |      10.00 |          10.00 |
 +---------+----------+------------+----------------+
```

| **操作符** | **说明** |
| ---------- | -------- |
| +          | 加       |
| -          | 减       |
| \*         | 乘       |
| /          | 除       |

> 如何测试计算 SELECT 提供了测试和试验函数与计算的一个 很好的办法。虽然 SELECT 通常用来从表中检索数据，但可以 省略 FROM 子句以便简单地访问和处理表达式。例如，SELECT 3\*2;将返回 6，SELECT Trim('abc');将返回 abc，而 SELECT Now()利用 Now()函数返回当前日期和时间。通过这些例子， 可以明白如何根据需要使用 SELECT 进行试验。

# **第十一章 使用数据处理函数**

- 用于处理文本串(如删除或填充值，转换值为大写或小写)的文本函数。
- 用于在数值数据上进行算术操作(如返回绝对值，进行代数运算)的数值函数。
- 用于处理日期和时间值并从这些值中提取特定成分(例如，返回两个日期之差，检查日期有效性等)的日期和时间函数。
- 返回 DBMS 正使用的特殊信息(如返回用户登录信息，检查版本细节)的系统函数。

## **文本处理函数**

```text
Left() 返回串左边的字符
Length() 返回串的长度
Locate() 找出串的一个子串
Lower() 将串转换为小写
LTrim() 去掉串左边的空格
Right() 返回串右边的字符
RTrim() 去掉串右边的空格
Soundex() 返回串的SOUNDEX值
SubString() 返回子串的字符
Upper() 将串转换为大写
```

## **日期和时间处理函数**

| **函数**      | **说明**                       |
| ------------- | ------------------------------ |
| AddDate()     | 增加一个日期(天、周等)         |
| AddTime()     | 增加一个时间(时、分等)         |
| CurTime()     | 返回当前时间                   |
| CurDate()     | 返回当前日期                   |
| Date()        | 返回日期时间的日期部分         |
| DateDiff()    | 计算两个日期之差               |
| Date_Add()    | 高度灵活的日期运算函数         |
| Date_Format() | 返回一个格式化的日期或时间串   |
| Day()         | 返回一个日期的天数部分         |
| DayOfWeek()   | 对于一个日期，返回对应的星期几 |
| Hour()        | 返回一个时间的小时部分         |
| Minute()      | 返回一个时间的分钟部分         |
| Month()       | 返回一个日期的月份部分         |
| Now()         | 返回当前日期和时间             |
| Second()      | 返回一个时间的秒部分           |
| Time()        | 返回一个日期时间的时间部分     |
| Year()        | 返回一个日期的年份部分         |

## **数值处理函数**

数值处理函数仅处理数值数据。这些函数一般主要用于代数、三角或几何运算，因此没有串或日期—时间处理函数的使用那么频繁。

| **函数** | **说明**           |
| -------- | ------------------ |
| Abs()    | 返回一个数的绝对值 |
| Cos()    | 返回一个角度的余弦 |
| Exp()    | 返回一个数的指数值 |
| Mod()    | 返回除操作的余数   |
| Pi()     | 返回圆周率         |
| Rand()   | 返回一个随机数     |
| Sin()    | 返回一个角度的正弦 |
| Sqrt()   | 返回一个数的平方根 |
| Tan()    | 返回一个角度的正切 |

# **第十二章 汇总数据**

`聚集函数(aggregate function)`运行在行组上，计算和返回单个值的函数。

| **函数** | **说明**         |
| -------- | ---------------- |
| AVG()    | 返回某列的平均值 |
| COUNT()  | 返回某列的行数   |
| MAX()    | 返回某列的最大值 |
| MIN()    | 返回某列的最小值 |
| SUM()    | 返回某列值之和   |

注意 1：

> NULL 值

    `COUNT`：如果指定列名，则指定列的值为空的行被COUNT() 函数忽略，但如果COUNT()函数中用的是星号(*)，则不忽 略。


    `MAX`,`MIN`,`SUM`,`AVG`：忽略NULL值

```sql
 SELCET COUNT(*) AS num_count FROM customers;  # 返回5
 SELCET COUNT(cust_email) AS num_count FROM customers;  # 返回3，有两个NULL
```

# **第十三章 分组数据**

`GROUP BY`子句和`HAVING`子句

## **GROUP BY**

```sql
 SELECT vend_id, COUNT(*) AS num_prods FROM products GROUP BY vend_id;
```

```text
 +---------+-----------+
 | vend_id | num_prods |
 +---------+-----------+
 |    1001 |         3 |
 |    1002 |         2 |
 |    1003 |         7 |
 |    1005 |         2 |
 +---------+-----------+
```

在具体使用 GROUP BY 子句前，需要知道一些重要的规定。

- GROUP BY 子句可以包含任意数目的列。这使得能对分组进行嵌套， 为数据分组提供更细致的控制。
- 如果在 GROUP BY 子句中嵌套了分组，数据将在最后规定的分组上 进行汇总。换句话说，在建立分组时，指定的所有列都一起计算(所以不能从个别的列取回数据)。
- GROUP BY 子句中列出的每个列都必须是检索列或有效的表达式(但不能是聚集函数)。如果在 SELECT 中使用表达式，则必须在 GROUP BY 子句中指定相同的表达式。不能使用别名。
- 除聚集计算语句外，SELECT 语句中的每个列都必须在 GROUP BY 子 句中给出。
- 如果分组列中具有 NULL 值，则 NULL 将作为一个分组返回。如果列 中有多行 NULL 值，它们将分为一组。
- GROUP BY 子句必须出现在 WHERE 子句之后，ORDER BY 子句之前。

使用 ROLLUP 使用 WITHROLLUP 关键字，可以得到每个分组以 及每个分组汇总级别(针对每个分组)的值

```sql
 SELECT vend_id, COUNT(*) AS num_prods FROM products GROUP BY vend_id WITH ROLLUP;
```

```text
 +---------+-----------+
 | vend_id | num_prods |
 +---------+-----------+
 |    1001 |         3 |
 |    1002 |         2 |
 |    1003 |         7 |
 |    1005 |         2 |
 |    NULL |        14 |
 +---------+-----------+
```

可以看到，多了一个 NULL 的分组

## **HAVING**

`HAVING`支持所有`WHERE`操作符

> HAVING 和 WHERE 的差别 WHERE 在数据 分组前进行过滤，HAVING 在数据分组后进行过滤。这是一个重要的区别，WHERE 排除的行不包括在分组中。这可能会改变计 算值，从而影响 HAVING 子句中基于这些值过滤掉的分组。

```sql
 SELECT vend_id, COUNT(*) AS num_prods
 FROM products
 GROUP BY vend_id WITH ROLLUP
 HAVING num_prods > 2;
```

```text
+---------+-----------+
| vend_id | num_prods |
+---------+-----------+
|    1001 |         3 |
|    1003 |         7 |
|    NULL |        14 |
+---------+-----------+
```

## **排序结合分组**

如果想要对于分组的顺序进行排序呢？

这里又要用到`ORDER BY`

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id WITH ROLLUP
HAVING num_prods > 2
ORDER BY num_prods DESC;
```

```text
+---------+-----------+
| vend_id | num_prods |
+---------+-----------+
|    NULL |        14 |
|    1003 |         7 |
|    1001 |         3 |
+---------+-----------+
```

## **总结 select 子句顺序**

| **子句** | **说明**           | **是否必须使用**       |
| -------- | ------------------ | ---------------------- |
| SELECT   | 要返回的列或表达式 | 是                     |
| FROM     | 从中检索数据的表   | 仅在从表选择数据时使用 |
| WHERE    | 行级过滤           | 否                     |
| GROUP BY | 分组说明           | 仅在按组计算聚集时使用 |
| HAVING   | 组级过滤           | 否                     |
| ORDER BY | 输出排序顺序       | 否                     |
| LIMIT    | 要检索的行数       | 否                     |

# **第十四章 使用子查询**

## **子查询进行过滤**

一般在 where 中嵌套子查询，常用于 where <field> in (select xxx)

> 列必须匹配 在 WHERE 子句中使用子查询(如这里所示)，应 该保证 SELECT 语句具有与 WHERE 子句中相同数目的列。通常， 子查询将返回单个列并且与单个列匹配，但如果需要也可以 使用多个列。

虽然子查询一般与 IN 操作符结合使用，但也可以用于测试等于(=)、 不等于(<>)等。

## **子查询作为计算字段**

假如需要显示 customers 表中每个客户的订单总数。订单与相应的客户 ID 存储在 orders 表中。

```sql
SELECT cust_name,
	cust_state,
	(SELECT COUNT(*) FROM orders WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name;
```

```text
+----------------+------------+--------+
| cust_name      | cust_state | orders |
+----------------+------------+--------+
| Coyote Inc.    | MI         |      2 |
| E Fudd         | IL         |      1 |
| Mouse House    | OH         |      0 |
| Wascals        | IN         |      1 |
| Yosemite Place | AZ         |      1 |
+----------------+------------+--------+
```

> 相关子查询(correlated subquery)涉及外部查询的子查询。

# **第十五章 联结表**

## **Where 联结(等值联结)**

```sql
 SELECT vend_name, prod_name, prod_price FROM vendors, products WHERE vendors.vend_id = products.vend_id ORDER BY vend_name, prod_name;
```

```text
 +-------------+----------------+------------+
 | vend_name   | prod_name      | prod_price |
 +-------------+----------------+------------+
 | ACME        | Bird seed      |      10.00 |
 | ACME        | Carrots        |       2.50 |
 | ACME        | Detonator      |      13.00 |
 | ACME        | Safe           |      50.00 |
 | ACME        | Sling          |       4.49 |
 | ACME        | TNT (1 stick)  |       2.50 |
 | ACME        | TNT (5 sticks) |      10.00 |
 | Anvils R Us | .5 ton anvil   |       5.99 |
 | Anvils R Us | 1 ton anvil    |       9.99 |
 | Anvils R Us | 2 ton anvil    |      14.99 |
 | Jet Set     | JetPack 1000   |      35.00 |
 | Jet Set     | JetPack 2000   |      55.00 |
 | LT Supplies | Fuses          |       3.42 |
 | LT Supplies | Oil can        |       8.99 |
 +-------------+----------------+------------+
```

> 笛卡儿积(cartesian product)由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘 以第二个表中的行数。

注意：

> 不要忘了 WHERE 子句 应该保证所有联结都有 WHERE 子句，否则 MySQL 将返回比想要的数据多得多的数据。同理，应该保证 WHERE 子句的正确性。不正确的过滤条件将导致 MySQL 返回不正确的数据。

## **内部联结**

```sql
 SELECT vend_name, prod_name, prod_price FROM vendors INNER JOIN products ON vendors.vend_id = products.vend_id ORDER BY vend_name, prod_name;
```

```text
 +-------------+----------------+------------+
 | vend_name   | prod_name      | prod_price |
 +-------------+----------------+------------+
 | ACME        | Bird seed      |      10.00 |
 | ACME        | Carrots        |       2.50 |
 | ACME        | Detonator      |      13.00 |
 | ACME        | Safe           |      50.00 |
 | ACME        | Sling          |       4.49 |
 | ACME        | TNT (1 stick)  |       2.50 |
 | ACME        | TNT (5 sticks) |      10.00 |
 | Anvils R Us | .5 ton anvil   |       5.99 |
 | Anvils R Us | 1 ton anvil    |       9.99 |
 | Anvils R Us | 2 ton anvil    |      14.99 |
 | Jet Set     | JetPack 1000   |      35.00 |
 | Jet Set     | JetPack 2000   |      55.00 |
 | LT Supplies | Fuses          |       3.42 |
 | LT Supplies | Oil can        |       8.99 |
 +-------------+----------------+------------+
```

该语句与上面的结果是一致的，只不过这里用例`INNER JOIN`内部联结关键字

> 使用哪种语法 ANSI SQL 规范首选 INNER JOIN 语法。此外， 尽管使用 WHERE 子句定义联结的确比较简单，但是使用明确的 联结语法能够确保不会忘记联结条件，有时候这样做也能影响 性能。

# **第十六章 创建高级联结**

## **`表别名`**

```sql
 SELECT cust_name, cust_contact
 FROM customers AS c, orders AS o, orderitems AS oi
 WHERE c.cust_id = o.cust_id AND oi.order_num = o.order_num AND prod_id = 'TNT2';
```

```text
 +----------------+--------------+
 | cust_name      | cust_contact |
 +----------------+--------------+
 | Coyote Inc.    | Y Lee        |
 | Yosemite Place | Y Sam        |
 +----------------+--------------+
```

## **其他类型的联结**

### **自联结**

假如你发现某物品(其 ID 为 DTNTR)存在问题，因此想知道生产该物 品的供应商生产的其他物品是否也存在这些问题。此查询要求首先找到 生产 ID 为 DTNTR 的物品的供应商，然后找出这个供应商生产的其他物品。 下面是解决此问题的一种方法:

- 子查询实现

```sql
 SELECT prod_id,prod_name
 FROM products
 WHERE vend_id = ( SELECT vend_id
                     FROM products
                     WHERE prod_id = 'DTNTR');
```

```text
 +---------+----------------+
 | prod_id | prod_name      |
 +---------+----------------+
 | DTNTR   | Detonator      |
 | FB      | Bird seed      |
 | FC      | Carrots        |
 | SAFE    | Safe           |
 | SLING   | Sling          |
 | TNT1    | TNT (1 stick)  |
 | TNT2    | TNT (5 sticks) |
 +---------+----------------+
```

- 自联结实现

```sql
 SELECT p1.prod_id, p1.prod_name
 FROM products AS p1, products AS p2
 WHERE p1.vend_id = p2.vend_id
 AND p2.prod_id = 'DTNTR';
```

```text
 +---------+----------------+
 | prod_id | prod_name      |
 +---------+----------------+
 | DTNTR   | Detonator      |
 | FB      | Bird seed      |
 | FC      | Carrots        |
 | SAFE    | Safe           |
 | SLING   | Sling          |
 | TNT1    | TNT (1 stick)  |
 | TNT2    | TNT (5 sticks) |
 +---------+----------------+
```

结果是一致的

注意：

> 用自联结而不用子查询 自联结通常作为外部语句用来替代 从相同表中检索数据时使用的子查询语句。虽然最终的结果是 相同的，但有时候处理联结远比处理子查询快得多。应该试一 下两种方法，以确定哪一种的性能更好。

### **自然联结**

自然联结排除多次出现，使每个列只返回一次

事实上，迄今为止我们建立的每个内部联结都是自然联结，很可能 我们永远都不会用到不是自然联结的内部联结。

### **外部联结**

与内部联结关联两个表中的行不同的是，外部联结还包括没 有关联行的行

`OUT JOIN`

`LEFT OUT JOIN`

`RIGHT OUT JOIN`

左右外部联结的区别在于所关联的表的顺序不同

# **第十七章 组合查询**

需要使用到组合查询

- 在单个查询中从不同的表返回类似结构的数据;
- 对单个表执行多个查询，按单个查询返回数据

## **使用\*\***`UNION`\*\*

```sql
 SELECT vend_id, prod_id, prod_price FROM products
 WHERE prod_price <= 5
 UNION
 SELECT vend_id, prod_id, prod_price FROM products
 WHERE vend_id IN (1001, 1002);
```

```text
 +---------+---------+------------+
 | vend_id | prod_id | prod_price |
 +---------+---------+------------+
 |    1003 | FC      |       2.50 |
 |    1002 | FU1     |       3.42 |
 |    1003 | SLING   |       4.49 |
 |    1003 | TNT1    |       2.50 |
 |    1001 | ANV01   |       5.99 |
 |    1001 | ANV02   |       9.99 |
 |    1001 | ANV03   |      14.99 |
 |    1002 | OL1     |       8.99 |
 +---------+---------+------------+
```

与之相同的是

```sql
 SELECT vend_id, prod_id, prod_price FROM products
 WHERE prod_price <= 5 OR vend_id IN (1001, 1002);
```

```text
 +---------+---------+------------+
 | vend_id | prod_id | prod_price |
 +---------+---------+------------+
 |    1001 | ANV01   |       5.99 |
 |    1001 | ANV02   |       9.99 |
 |    1001 | ANV03   |      14.99 |
 |    1003 | FC      |       2.50 |
 |    1002 | FU1     |       3.42 |
 |    1002 | OL1     |       8.99 |
 |    1003 | SLING   |       4.49 |
 |    1003 | TNT1    |       2.50 |
 +---------+---------+------------+
```

数据是一致的，只是顺序不同

一般 UNION 用于多个表的情况

## **UNION 使用规则**

- UNION 必须由两条或两条以上的 SELECT 语句组成，语句之间用关 键字 UNION 分隔（因此，如果组合 4 条 SELECT 语句，将要使用 3 个 UNION 关键字）。
- UNION 中的每个查询必须包含相同的列、表达式或聚集函数（列次序不做要求）。
- 列数据类型必须兼容：类型不必完全相同，但必须是 DBMS 可以隐含地转换的类型（例如，不同的数值类型或不同的日期类型）。

> UNION 默认过滤重复的行，如果想保留，则需要使用 UNION ALL

排序的`ORDER BY`语句只能放在最后。

# **第十八章 全文本搜索**

两个最常使用的引擎为 MyISAM 和 InnoDB， 前者支持全文本搜索，而后者不支持。

这章不作要求

MySQL 5.5.5 in 2010 之后 `InnoDB`作为 MySQL 的默认引擎

# **第十九章 插入数据**

`INSERT`

## **插入完整的行**

第一种情况，缺省列，需要添加所以插入的字段参数

```sql
 INSERT INTO Customers
 VALUES(NULL,
      'Pep E. LaPew',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA',
      NULL，
      NULL
 )；
```

第二种情况，以一种指定顺序的列，添加列名顺序的变量

```sql
 INSERT INTO customers(
      cust_name,
      cust_email,
      cust_address,
      cust_city,
      cust_state,
      cust_zip,
      cust_country
 )
 VALUES(
      'Pep E. LaPew',
      '123@163.com',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA'
 );
```

也可以插入部分行，省略部分列，不过需要满足以下某个条件

> 省略列 如果表的定义允许，则可以在 INSERT 操作中省略某 些列。省略的列必须满足以下某个条件。

    - 该列定义为允许NULL值(无值或空值)。
    - 在表定义中给出默认值。这表示如果不给出值，将使用默认值。

    如果对表中不允许NULL值且没有默认值的列不给出值，则 MySQL将产生一条错误消息，并且相应的行插入不成功。

## **插入多行**

```sql
 INSERT INTO Customers(
      cust_name,
      cust_email,
      cust_address,
      cust_city,
      cust_state,
      cust_zip,
      cust_country
 )
 VALUES(NULL,
      'Pep E. LaPew',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA',
      NULL，
      NULL
 ),(NULL,
      'Pep E. LaPew',
      '100 Main Street',
      'Los Angeles',
      'CA',
      '90046',
      'USA',
      NULL，
      NULL
 )；
```

## **插入某些查询的结果**

`INSERT`一般用来给表插入一个指定列值的行。但是，`INSERT`还存在 另一种形式，可以利用它将一条`SELECT`语句的结果插入表中。这就是所 谓的 I`NSERT SELECT`，顾名思义，它是由一条`INSERT`语句和一条`SELECT` 语句组成的。

```sql
 INSERT INTO Customers(
      cust_name,
      cust_email,
      cust_address,
      cust_city,
      cust_state,
      cust_zip,
      cust_country
 ) SELECT cust_name,
      cust_email,
      cust_address,
      cust_city,
      cust_state,
      cust_zip,
      cust_country
 FROM custnew;
```

# **第二十章 更新和删除数据**

## **更新**

更新语句由三部分组成：

- 要更新的表
- 列名和它们的新值
- 确定要更新行的过滤条件

```sql
 UPDATE customers
 SET cust_email = 'elmer@fudd.com'
 WHERE cust_id = 10005
```

### **更新多个列的情况**

```sql
 UPDATE customers
 SET cust_email = 'elmer@fudd.com', cust_name = 'The Fudd'
 WHERE cust_id = 10005
```

### **允许删除某个列的值，也就是可以直接更新为 NULL**

```sql
 UPDATE customers
 SET cust_email = 'elmer@fudd.com', cust_name = NULL
 WHERE cust_id = 10005
```

> IGNORE 关键字 如果用 UPDATE 语句更新多行，并且在更新这些行中的一行或多行时出一个现错误，则整个 UPDATE 操作被取消 (错误发生前更新的所有行被恢复到它们原来的值)。为即使是发生错误，也继续进行更新，可使用 IGNORE 关键字，如下所示: UPDATE IGNORE customers...

## **删除**

### **从表中删除特定的行**

```sql
 DELECT FROM customers
 WHERE cust_id = 10006;
```

### **从表中删除所有的行**

```sql
 TRUNCATE TABLE <your-table-name>
```

> 更快的删除 如果想从表中删除所有行，不要使用 DELETE。 可使用 TRUNCATE TABLE 语句，它完成相同的工作，但速度更 快(TRUNCATE 实际是删除原来的表并重新创建一个表，而不 是逐行删除表中的数据)。

### **指导原则**

下面是许多 SQL 程序员使用 UPDATE 或 DELETE 时所遵循的习惯：

- 除非确实打算更新和删除每一行，否则绝对不要使用不带 WHERE 子句的 UPDATE 或 DELETE 语句。
- 保证每个表都有主键(如果忘记这个内容，请参阅第 15 章)，尽可能像 WHERE 子句那样使用它(可以指定各主键、多个值或值的范围)。
- 在对 UPDATE 或 DELETE 语句使用 WHERE 子句前，应该先用 SELECT 进行测试，保证它过滤的是正确的记录，以防编写的 WHERE 子句不 正确。
- 使用强制实施引用完整性的数据库(关于这个内容，请参阅第 15 章)，这样 MySQL 将不允许删除具有与其他表相关联的数据的行。

# **第二十一章 创建和操控表**

## **创建表**

```sql
 CREATE TABLE customers
 (
   cust_id      int       NOT NULL AUTO_INCREMENT,
   cust_name    char(50)  NOT NULL ,
   cust_address char(50)  NULL ,
   cust_city    char(50)  NULL ,
   cust_state   char(5)   NULL ,
   cust_zip     char(10)  NULL ,
   cust_country char(50)  NULL ,
   cust_contact char(50)  NULL ,
   cust_email   char(255) NULL ,
   PRIMARY KEY (cust_id)
 ) ENGINE=InnoDB;
```

> 处理现有的表 在创建新表时，指定的表名必须不存在，否则 将出错。如果要防止意外覆盖已有的表，SQL 要求首先手工删 除该表(请参阅后面的小节)，然后再重建它，而不是简单地 用创建表语句覆盖它。

    如果你仅想在一个表不存在时创建它，应该在表名后给出IF NOT EXISTS。这样做不检查已有表的模式是否与你打算创建 的表模式相匹配。它只是查看表名是否存在，并且仅在表名不 存在时创建它。

### **NULL 值**

NULL 值就是没有值或缺值。允许 NULL 值的列也允许在 插入行时不给出该列的值。不允许 NULL 值的列不接受该列没有值的行， 换句话说，在插入或更新行时，该列必须有值。

每个表列或者是 NULL 列，或者是 NOT NULL 列，这种状态在创建时由表的定义规定。NULL 为默认设置，如果不指定 NOT NULL，则认为指定的是 NULL。

### **主键 PK**

主键值必须唯一。表中的每个行必须具有唯一的主键值。如果主键使用单个列，则它的值必须唯一。如果使用多个列，则这些列的组合值必须唯一。

```text
 # 单个值的主键
 PRIMARY KEY (cust_id)
 # 多个值的主键
 PRIMARY KEY (cust_name, cust_city)
```

> 主键中只能使用不允许 NULL 值的列。允许 NULL 值的 列不能作为唯一标识。

### **AUTO_INCREMENT**

`AUTO_INCREMENT`告诉 MySQL，本列每当增加一行时自动增量。每次 执行一个`INSERT`操作时，MySQL 自动对该列增量(从而才有这个关键字 `AUTO_INCREMENT`)，给该列赋予下一个可用的值。

注意：每个表只允许一个 AUTO_INCREMENT 列，而且它必须被索引(如，通过使它成为主键)。

### **指定默认值**

```sql
 CREATE TABLE orderitems
 (
   order_num  int          NOT NULL ,
   order_item int          NOT NULL ,
   prod_id    char(10)     NOT NULL ,
   quantity   int          NOT NULL DEFAULT 1,
   item_price decimal(8,2) NOT NULL ,
   PRIMARY KEY (order_num, order_item)
 ) ENGINE=InnoDB;
```

`不允许函数` 与大多数 DBMS 不一样，MySQL 不允许使用函 数作为默认值，它只支持常量。

`使用默认值而不是NULL值` 许多数据库开发人员使用默认 值而不是 NULL 列，特别是对用于计算或数据分组的列更是如 此。

### **引擎**

- InnoDB 是一个可靠的事务处理引擎，它不支持全文本搜索（`5.6`后支持）; [**介绍**](https://dev.mysql.com/doc/refman/5.6/en/innodb-introduction.html)
- MEMORY 在功能等同于 MyISAM，但由于数据存储在内存(不是磁盘) 中，速度很快(特别适合于临时表);
- MyISAM 是一个性能极高的引擎，它支持全文本搜索(参见第 18 章)， 但不支持事务处理。

两种基本引擎的对比

| **Feature**                           | **MyisAm Support** | **InnoDB Support** |
| ------------------------------------- | ------------------ | ------------------ |
| B-tree indexes                        | Yes                | Yes                |
| Backup/point-in-time recovery 1       | Yes                | Yes                |
| Cluster database support              | No                 | No                 |
| Clustered indexes                     | No                 | Yes                |
| Compressed data                       | Yes2               | Yes                |
| Data caches                           | No                 | Yes                |
| Encrypted data                        | Yes3               | Yes4               |
| Foreign key support                   | No                 | Yes                |
| Full-text search indexes              | Yes                | Yes5               |
| Geospatial data type support          | Yes                | Yes                |
| Geospatial indexing support           | Yes                | Yes6               |
| Hash indexes                          | No                 | No 7               |
| Index caches                          | Yes                | Yes                |
| Locking granularity                   | Table              | Row                |
| MVCC                                  | No                 | Yes                |
| Replication support 1                 | Yes                | Yes                |
| Storage limits                        | 256TB              | 64TB               |
| T-tree indexes                        | No                 | No                 |
| Transactions                          | No                 | Yes                |
| Update statistics for data dictionary | Yes                | Yes                |

[**1**]  Implemented in the server, rather than in the storage engine.

[**2**]  Compressed MyISAM tables are supported only when using the compressed row format. Tables using the compressed row format with MyISAM are read only.

[**3**]  Implemented in the server via encryption functions.

[**4**]  Implemented in the server via encryption functions; In MySQL 5.7 and later, data-at-rest encryption is supported.

[**5**]  Support for FULLTEXT indexes is available in MySQL 5.6 and later.

[**6**]  Support for geospatial indexing is available in MySQL 5.7 and later.

[**7**]  InnoDB utilizes hash indexes internally for its Adaptive Hash Index feature.

## **更新表**

为更新表定义，可使用 ALTER TABLE 语句。但是，理想状态下，当表 中存储数据以后，该表就不应该再被更新。在表的设计过程中需要花费 大量时间来考虑，以便后期不对该表进行大的改动。

ALTER TABLE 的一种常见用途是定义外键

> 小心使用 ALTER TABLE 使用 ALTER TABLE 要极为小心，应该 在进行改动前做一个完整的备份(模式和数据的备份)。数据 库表的更改不能撤销，如果增加了不需要的列，可能不能删 除它们。类似地，如果删除了不应该删除的列，可能会丢失 该列中的所有数据。

## **删除表**

```sql
 DROP TABLE <your-table-name>
```

记住与 DELETE 语句不同

## **重命名表**

```sql
 RENAME TABLE <original-table-name> TO <new-table-name>
```

# **第二十二章 使用视图**

## **视图**

视图是虚拟的表。与包含数据的表不一样，视图只包含使用时动态检索数据的查询。

### **Why**

视图的常见应用

- 重用 SQL 语句。
- 简化复杂的 SQL 操作。在编写查询后，可以方便地重用它而不必知道它的基本查询细节。
- 使用表的组成部分而不是整个表。
- 保护数据。可以给用户授予表的特定部分的访问权限而不是整个表的访问权限。
- 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

在视图创建之后，可以用与表基本相同的方式利用它们。可以对视 图执行 SELECT 操作，过滤和排序数据，将视图联结到其他视图或表，甚 至能添加和更新数据

重要的是知道视图仅仅是用来查看存储在别处的数据的一种设施。 视图本身不包含数据，因此它们返回的数据是从其他表中检索出来的。在添加或更改这些表中的数据时，视图将返回改变过的数据。

> 性能问题 因为视图不包含数据，所以每次使用视图时，都 必须处理查询执行时所需的任一个检索。如果你用多个联结 和过滤创建了复杂的视图或者嵌套了视图，可能会发现性能 下降得很厉害。因此，在部署使用了大量视图的应用前，应 该进行测试。

=> 视图实际上还是需要进行查询操作

### **Rules and Limitations——使用规则和限制**

- 与表一样，视图必须唯一命名(不能给视图取与别的视图或表相 同的名字)。
- 对于可以创建的视图数目没有限制。
- 为了创建视图，必须具有足够的访问权限。这些限制通常由数据库管理人员授予。
- 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造一个视图。
- ORDER BY 可以用在视图中，但如果从该视图检索数据 SELECT 中也含有 ORDER BY，那么该视图中的 ORDER BY 将被覆盖。
- 视图不能索引，也不能有关联的触发器或默认值。
- 视图可以和表一起使用。例如，编写一条联结表和视图的 SELECT 语句。

## **使用**

- 创建

  ```sql
   CREATE VIEW ...
  ```

- 查看

  ```sql
   SHOW CREATE VIEW viewname;
  ```

- 删除

  ```text
   DROP VIEW viewname;
  ```

- 更新

  方法一：

  ```sql
   DROP VIEW viewname;
   CREATE VIEW ...
  ```

  方法二：

  ```sql
   CREATE OR REPLACE VIEW ...
  ```

### **利用视图简化复杂的联结**

- 通过创建视图来隐藏复杂的查询语句，使暴露给业务的视图能够通过简单的语句进行查询
- 对于经常使用的复杂查询语句，可以创建对应的视图

> 创建可重用的视图 创建不受特定数据限制的视图是一种 好办法。例如，上面创建的视图返回生产所有产品的客户而 不仅仅是生产 TNT2 的客户。扩展视图的范围不仅使得它能被 重用，而且甚至更有用。这样做不需要创建和维护多个类似 视图。

### **利用视图重新格式化检索出的数据**

```sql
 SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') AS vend_title
 FROM vendors
 ORDER BY vend_name;
```

```text
 +-------------------------+
 | vend_title              |
 +-------------------------+
 | ACME (USA)              |
 | Anvils R Us (USA)       |
 | Furball Inc. (USA)      |
 | Jet Set (England)       |
 | Jouets Et Ours (France) |
 | LT Supplies (USA)       |
 +-------------------------+
```

如果经常使用这个

```sql
 CREATE VIEW vendorlocation AS
 SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') AS vend_title
 FROM vendors
 ORDER BY vend_name;
```

```text
 +-------------------+
 | Tables_in_my_test |
 +-------------------+
 | customers         |
 | orderitems        |
 | orders            |
 | productnotes      |
 | products          |
 | vendorlocation    |
 | vendors           |
 +-------------------+
```

可以看到使用`show tables`多了一个`vendorlocation`

### **利用视图过滤不想要的数据**

使用视图生成一个含有 where 子句的查询

> WHERE 子句与 WHERE 子句 如果从视图检索数据时使用了一条 WHERE 子句，则两组子句(一组在视图中，另一组是传递给视 图的)将自动组合。

### **利用视图与计算字段**

```sql
 SELECT order_num, prod_id, quantity, item_price, quantity*item_price AS expanded_price FROM orderitems WHERE order_num = 20005
```

创建对应视图

```sql
 CREATE OR REPLACE VIEW orderitemsexpanded AS
 SELECT order_num, prod_id, quantity, item_price, quantity*item_price AS expanded_price FROM orderitems WHERE order_num = 20005;
```

```sql
 SELECT * FROM orderitemsexpanded
 WHERE order_num = 20005;
```

```text
 +-----------+---------+----------+------------+----------------+
 | order_num | prod_id | quantity | item_price | expanded_price |
 +-----------+---------+----------+------------+----------------+
 |     20005 | ANV01   |       10 |       5.99 |          59.90 |
 |     20005 | ANV02   |        3 |       9.99 |          29.97 |
 |     20005 | TNT2    |        5 |      10.00 |          50.00 |
 |     20005 | FB      |        1 |      10.00 |          10.00 |
 +-----------+---------+----------+------------+----------------+
```

### **更新视图**

如果存在以下操作，则不能进行视图的更新

- 分组(使用 GROUP BY 和 HAVING);
- 联结;
- 子查询;
- 并;
- 聚集函数(Min()、Count()、Sum()等);
- DISTINCT;
- 导出(计算)列。

> 一般来说，视图应该用于检索（select）而不是更新

# **第二十三章 存储过程**

（主要了解 what，why 即可）

What ——存储过程简单来说，就是为以后的使用而保存 的一条或多条 MySQL 语句的集合

Why ——

- 通过把处理封装在容易使用的单元中，简化复杂的操作(正如前 面例子所述)。
- 由于不要求反复建立一系列处理步骤，这保证了数据的完整性。 如果所有开发人员和应用程序都使用同一(试验和测试)存储过程，则所使用的代码都是相同的。 这一点的延伸就是防止错误。需要执行的步骤越多，出错的可能 性就越大。防止错误保证了数据的一致性。
- 简化对变动的管理。如果表名、列名或业务逻辑(或别的内容) 有变化，只需要更改存储过程的代码。使用它的人员甚至不需要知道这些变化。这一点的延伸就是安全性。通过存储过程限制对基础数据的访问减 少了数据讹误(无意识的或别的原因所导致的数据讹误)的机会。
- 提高性能。因为使用存储过程比使用单独的 SQL 语句要快。
- 存在一些只能用在单个请求中的 MySQL 元素和特性，存储过程可 以使用它们来编写功能更强更灵活的代码

  换句话说，使用存储过程有 3 个主要的好处，即简单、安全、高性能。 显然，它们都很重要。不过，在将 SQL 代码转换为存储过程前，也必须知 道它的一些缺陷。

- 一般来说，存储过程的编写比基本 SQL 语句复杂，编写存储过程需要更高的技能，更丰富的经验。
- 你可能没有创建存储过程的安全访问权限。许多数据库管理员限 制存储过程的创建权限，允许用户使用存储过程，但不允许他们 创建存储过程。

# **第二十四章 游标**

`CURSE`

游标是和存储过程一起使用的，这里只需要知道 What 和 Why

`游标(cursor)`是一个存储在 MySQL 服务器上的数据库查询， 它不是一条 SELECT 语句，而是被该语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

游标主要用于交互式应用，其中用户需要滚动屏幕上的数据，并对 数据进行浏览或做出更改。

> 只能用于存储过程 不像多数 DBMS，MySQL 游标只能用于 存储过程(和函数)。

使用（略）

# **第二十五章 使用触发器**

触发器是 MySQL 响应以下任意语句而自动执行的一条 MySQL 语句(或位于 BEGIN 和 END 语句之间的一组语句):

- DELETE
- INSERT
- UPDATE

其他 MySQL 语句不支持触发器。

## **创建触发器**

在创建触发器时，需要给出 4 条信息:

- 唯一的触发器名;
- 触发器关联的表;
- 触发器应该响应的活动(DELETE、INSERT 或 UPDATE);
- 触发器何时执行(处理之前或之后)。

> 保持每个数据库的触发器名唯一 在 MySQL5 中，触发器名必 须在每个表中唯一，但不是在每个数据库中唯一。这表示同一 数据库中的两个表可具有相同名字的触发器。这在其他每个数 据库触发器名必须唯一的 DBMS 中是不允许的，而且以后的 MySQL 版本很可能会使命名规则更为严格。因此，现在最好 是在数据库范围内使用唯一的触发器名。

```sql
 CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added';
```

特别注意，mysql 触发器是不允许返回结果集的，这里的使用方法在 MySQL 5.6 之后是错误的。5.0 之前还是可以使用的。

```sql
  MySQL  43.128.60.36:3306 ssl  my_test  SQL > CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added';
 ERROR: 1415 (0A000): Not allowed to return a result set from a trigger
```

```sql
 CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added'INTO @temp_value;
```

变量名用@引导即可

> 仅支持表 只有表才支持触发器，视图不支持(临时表也不 支持)。

触发器按每个表每个事件每次地定义，每个表每个事件每次只允许一个触发器。因此，**每个表最多支持 6 个触发器**(每条 INSERT、UPDATE 和 DELETE 的之前和之后)。单一触发器不能与多个事件或多个表关联，所 以，如果你需要一个对 INSERT 和 UPDATE 操作执行的触发器，则应该定义 两个触发器。

> 触发器失败 如果 BEFORE 触发器失败，则 MySQL 将不执行请 求的操作。此外，如果 BEFORE 触发器或语句本身失败，MySQL 将不执行 AFTER 触发器(如果有的话)。

## **删除触发器**

```sql
 DROP TRIGGER <your-trigger>
```

触发器不能更新或覆盖

## **使用触发器**

### **INSERT 触发器**

> MYSQL5 以后，不允许触发器返回任何结果，因此使用 into @变量名，将结果赋值到变量中，用 select 调用即可

```sql
 CREATE TRIGGER neworder AFTER INSERT ON orders
 FOR EACH ROW SELECT NEW.order_num;
```

```sql
 CREATE TRIGGER neworder AFTER INSERT ON orders
 FOR EACH ROW SELECT NEW.order_num INTO @temp;
```

```text
 +-----------+---------------------+---------+
 | order_num | order_date          | cust_id |
 +-----------+---------------------+---------+
 |     20005 | 2005-09-01 00:00:00 |   10001 |
 |     20006 | 2005-09-12 00:00:00 |   10003 |
 |     20007 | 2005-09-30 00:00:00 |   10004 |
 |     20008 | 2005-10-03 00:00:00 |   10005 |
 |     20009 | 2005-10-08 00:00:00 |   10001 |
 +-----------+---------------------+---------+
```

原始表如上

```sql
 INSERT INTO orders(order_num, order_date, cust_id)
 VALUES(20010, '2005-09-01', 10001);
```

```sql
 SELECT @temp;
```

```text
 +-------+
 | @temp |
 +-------+
 | 20010 |
 +-------+
```

### **UPDATE 触发器**

UPDATE 触发器在 UPDATE 语句执行之前或之后执行

- 在 UPDATE 触发器代码中，你可以引用一个名为 OLD 的虚拟表访问
  以前（UPDATE 语句前）的值，引用一个名为 NEW 的虚拟表访问新
  更新的值；
- 在 BEFORE UPDATE 触发器中，NEW 中的值可能也被更新（允许更改
  将要用于 UPDATE 语句中的值）
- OLD 中的值全都是只读的，不能更新。

```sql
 CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
 FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
```

### **DELETE 触发器**

DELETE 触发器在 DELETE 语句执行之前或之后执行：

- 在 DELETE 触发器代码内，可以引用一个名为 OLD 的虚拟表，访问被删除的行
- OLD 中的值全部都是只能读的，不能更新

```sql
 CREATE TRIGGER deleteorder BEFORE DELETE ON orders
 FOR EACH ROW
 BEGIN
     INSERT INTO archive_orders(order_num, order_date, cust_id)
     VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
 END;
```

在任意订单被删除前将执行此触发器。

- 它使用一条 INSERT 语句将 OLD 中的值（要被删除的订单）保存到一个名为 archive_orders 的存档表中（为实际使用这个例子，你需要用与 orders 相同的列创建一个名为 archive_orders 的表）。

使用 BEFORE DELETE 触发器的优点（相对于 AFTER DELETE 触发器
来说）为，如果由于某种原因，订单不能存档，DELETE 本身将被放弃。

## **关于触发器**

- 与其他 DBMS 相比，MySQL 5 中支持的触发器相当初级。未来的
  MySQL 版本中有一些改进和增强触发器支持的计划。
- 创建触发器可能需要特殊的安全访问权限，但是，触发器的执行
  是自动的。如果 INSERT、UPDATE 或 DELETE 语句能够执行，则相关
  的触发器也能执行。
- 应该用触发器来保证数据的一致性（大小写、格式等）。在触发器
  中执行这种类型的处理的优点是它总是进行这种处理，而且是透
  明地进行，与客户机应用无关。
- 触发器的一种非常有意义的使用是创建审计跟踪。使用触发器，
  把更改（如果需要，甚至还有之前和之后的状态）记录到另一个
  表非常容易。
- 遗憾的是，MySQL 触发器中不支持 CALL 语句。这表示不能从触发
  器内调用存储过程。所需的存储过程代码需要复制到触发器内。

# **第二十六章 管理事务**

`事务处理(transaction processing)`可以用来维护数据库的完整性，它 保证成批的 MySQL 操作要么完全执行，要么完全不执行。

关于事务的几个术语：

- 事务(transaction)指一组 SQL 语句;
- 回退(rollback)指撤销指定 SQL 语句的过程;
- 提交(commit)指将未存储的 SQL 语句结果写入数据库表;
- 保留点(savepoint)指事务处理中设置的临时占位符(place-holder)，你可以对它发布回退(与回退整个事务处理不同)。

## **控制事务处理**

开始事务

```sql
 START TRANSACTION
```

回退

```sql
 ROLLBACK;
```

> 哪些语句可以回退? 事务处理用来管理 INSERT、UPDATE 和 DELETE 语句。你不能回退 SELECT 语句。(这样做也没有什么意 义。)你不能回退 CREATE 或 DROP 操作。事务处理块中可以使用 这两条语句，但如果你执行回退，它们不会被撤销。

### **使用 COMMIT**

一般的 MySQL 语句都是直接针对数据库表执行和编写的。这就是 所谓的隐含提交(implicit commit)，即提交(写或保存)操作是自动 进行的。

但是，在事务处理块中，提交不会隐含地进行。为进行明确的提交， 使用 COMMIT 语句

### **使用保留点**

简单的 ROLLBACK 和 COMMIT 语句就可以写入或撤销整个事务处理。但 是，只是对简单的事务处理才能这样做，更复杂的事务处理可能需要部 分提交或回退。

为了支持回退部分事务处理，必须能在事务处理块中合适的位置放 置占位符。这样，如果需要回退，可以回退到某个占位符。

这些占位符称为保留点。为了创建占位符，可如下使用 SAVEPOINT 语句:

```sql
 SAVEPOINT delete1;
```

> 保留点越多越好 可以在 MySQL 代码中设置任意多的保留 点，越多越好。为什么呢?因为保留点越多，你就越能按自己 的意愿灵活地进行回退。

> 释放保留点 保留点在事务处理完成(执行一条 ROLLBACK 或 COMMIT)后自动释放。自 MySQL 5 以来，也可以用 RELEASE SAVEPOINT 明确地释放保留点。

### **更改默认的提交行为**

正如所述，默认的 MySQL 行为是自动提交所有更改。换句话说，任何 时候你执行一条 MySQL 语句，该语句实际上都是针对表执行的，而且所做 的更改立即生效。为指示 MySQL 不自动提交更改，需要使用以下语句:

```sql
 SET autocommit=0;
```

> 标志为连接专用 autocommit 标志是针对每个连接而不是服 务器的。

# **第二十七章 全球化和本地化**

## **字符集和校对顺序**

数据库表被用来存储和检索数据。不同的语言和字符集需要以不同 的方式存储和检索。

- 字符集为字母和符号的集合;
- 编码为某个字符集成员的内部表示;
- 校对为规定字符如何比较的指令。

查看所有支持校对的完整列表

```sql
 SHOW COLLATION
```

```text
 +----------+---------------------------------+---------------------+--------+
 | Charset  | Description                     | Default collation   | Maxlen |
 +----------+---------------------------------+---------------------+--------+
 | armscii8 | ARMSCII-8 Armenian              | armscii8_general_ci |      1 |
 | ascii    | US ASCII                        | ascii_general_ci    |      1 |
 | big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
 | binary   | Binary pseudo charset           | binary              |      1 |
 | cp1250   | Windows Central European        | cp1250_general_ci   |      1 |
 | cp1251   | Windows Cyrillic                | cp1251_general_ci   |      1 |
 | cp1256   | Windows Arabic                  | cp1256_general_ci   |      1 |
 | cp1257   | Windows Baltic                  | cp1257_general_ci   |      1 |
 | cp850    | DOS West European               | cp850_general_ci    |      1 |
 | cp852    | DOS Central European            | cp852_general_ci    |      1 |
 | cp866    | DOS Russian                     | cp866_general_ci    |      1 |
 | cp932    | SJIS for Windows Japanese       | cp932_japanese_ci   |      2 |
 | dec8     | DEC West European               | dec8_swedish_ci     |      1 |
 | eucjpms  | UJIS for Windows Japanese       | eucjpms_japanese_ci |      3 |
 | euckr    | EUC-KR Korean                   | euckr_korean_ci     |      2 |
 | gb18030  | China National Standard GB18030 | gb18030_chinese_ci  |      4 |
 | gb2312   | GB2312 Simplified Chinese       | gb2312_chinese_ci   |      2 |
 | gbk      | GBK Simplified Chinese          | gbk_chinese_ci      |      2 |
 | geostd8  | GEOSTD8 Georgian                | geostd8_general_ci  |      1 |
 | greek    | ISO 8859-7 Greek                | greek_general_ci    |      1 |
 | hebrew   | ISO 8859-8 Hebrew               | hebrew_general_ci   |      1 |
 | hp8      | HP West European                | hp8_english_ci      |      1 |
 | keybcs2  | DOS Kamenicky Czech-Slovak      | keybcs2_general_ci  |      1 |
 | koi8r    | KOI8-R Relcom Russian           | koi8r_general_ci    |      1 |
 | koi8u    | KOI8-U Ukrainian                | koi8u_general_ci    |      1 |
 | latin1   | cp1252 West European            | latin1_swedish_ci   |      1 |
 | latin2   | ISO 8859-2 Central European     | latin2_general_ci   |      1 |
 | latin5   | ISO 8859-9 Turkish              | latin5_turkish_ci   |      1 |
 | latin7   | ISO 8859-13 Baltic              | latin7_general_ci   |      1 |
 | macce    | Mac Central European            | macce_general_ci    |      1 |
 | macroman | Mac West European               | macroman_general_ci |      1 |
 | sjis     | Shift-JIS Japanese              | sjis_japanese_ci    |      2 |
 | swe7     | 7bit Swedish                    | swe7_swedish_ci     |      1 |
 | tis620   | TIS620 Thai                     | tis620_thai_ci      |      1 |
 | ucs2     | UCS-2 Unicode                   | ucs2_general_ci     |      2 |
 | ujis     | EUC-JP Japanese                 | ujis_japanese_ci    |      3 |
 | utf16    | UTF-16 Unicode                  | utf16_general_ci    |      4 |
 | utf16le  | UTF-16LE Unicode                | utf16le_general_ci  |      4 |
 | utf32    | UTF-32 Unicode                  | utf32_general_ci    |      4 |
 | utf8     | UTF-8 Unicode                   | utf8_general_ci     |      3 |
 | utf8mb4  | UTF-8 Unicode                   | utf8mb4_0900_ai_ci  |      4 |
 +----------+---------------------------------+---------------------+--------+
```

如前所述，校对在对用 ORDER BY 子句检索出来的数据排序时起重要 的作用。如果你需要用与创建表时不同的校对顺序排序特定的 SELECT 语句

最后，值得注意的是，如果绝对需要，串可以在字符集之间进行转 换。为此，使用 Cast()或 Convert()函数。

# **第二十八章 安全管理**

(略)

# **第二十九章 数据库维护**

（略）

# **第三十章 改善性能**

（略）
