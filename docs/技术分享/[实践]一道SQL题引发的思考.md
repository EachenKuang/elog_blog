---
password: ""
icon: ""
date: "2023-12-29"
type: Post
category: 技术分享
slug: a-sql-problem-thinking
tags:
  - SQL
  - MySQL
summary: ""
title: "[实践]一道SQL题引发的思考"
status: Published
urlname: dfa743c9-c01d-435a-9339-c29995b7ca57
updated: "2023-12-29 11:55:00"
---

# 题目

> **下面的 t1 表中的数据为用户 A 的访问记录，请计算每个月的访问的天数。返回 year,month,days 格式。**

```sql
CREATE TABLE t1 (year YEAR, month INT UNSIGNED,
             day INT UNSIGNED);
INSERT INTO t1 VALUES(2000,1,1),(2000,1,20),(2000,1,30),(2000,2,2),
            (2000,2,23),(2000,2,23);
```

正确结果返回应该为：

```sql
+------+-------+------+
| year | month | days |
+------+-------+------+
| 2000 |     1 |    3 |
| 2000 |     2 |    2 |
+------+-------+------+
```

## 答案一

```sql
SELECT year,month,BIT_COUNT(BIT_OR(1<<day)) AS days FROM t1 GROUP BY year,month;
```

## 答案二

```sql
SELECT year, month, COUNT(day) AS days FROM (SELECT DISTINCT year,month,day FROM t1) as t2 GROUP BY year, month;
```

# 分析

## 我的答案

一开始，我想着，先对年月日进行一次去重，因为只需要计算一个月的天数，所以重复的数据是没有用的。

```sql
SELECT DISTINCT year,month,day FROM t1;
```

然后，在上面的结果的基础上，根据年月进行分组计算日的数量。

```sql
SELECT year, month, COUNT(day) AS days FROM (SELECT DISTINCT year,month,day FROM t1) as t2 GROUP BY year, month;
```

## 标准答案

```sql
SELECT year, month, COUNT(day) AS days FROM (SELECT DISTINCT year,month,day FROM t1) as t2 GROUP BY year, month;
```

然而，看了一眼标准答案，惊为天人，用了两个位运算的函数 `BIT_COUNT` 与 `BIT_OR` ，还有左移符号 `<<` 。

看第一眼，没看懂了，看第二眼，还是没有看懂。

- `BIT_COUNT` 计算对应的数的二进制中的 1 的数量，例如

```sql
mysql> SELECT BIT_COUNT(4);
+--------------+
| BIT_COUNT(4) |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)

mysql> SELECT BIT_COUNT("64");
+-----------------+
| BIT_COUNT("64") |
+-----------------+
|               1 |
+-----------------+
1 row in set (0.00 sec)
```

- `BIT_OR` 可以理解为按位或，就是对 select 出来的字段的值的列表，进行所有按位或操作。

```sql
可以这样理解，BIT_OR(column) # column 包含 1,3,4,5
那么最终结果为 1|3|4|5 = 7
```

**继续分析：**

1. `1<<day`：这是一个位左移操作。它将数字 1 向左移动`day`位。在这里，`day`很可能是一个代表月份中某一天的字段。例如，如果`day`是 2，那么`1<<day`的结果是 4（二进制中的`100`），这相当于 2 的`day`次方。
2. `BIT_OR(1<<day)`：`BIT_OR`是一个聚合函数，它对组内所有行执行位或（bitwise OR）操作。在这个查询中，它将同一个`year`和`month`组内的所有`1<<day`的结果进行位或操作。这样，每个不同的`day`都会在结果中设置一个位。因为每个月的天数是唯一的，所以这个操作最终会产生一个整数，其二进制表示中的位数表示该月中出现的不同天数。【不超过 32 天，位数不会超过 31，可以使用`BIT_OR`】
3. `BIT_COUNT(BIT_OR(1<<day))`：`BIT_COUNT`函数计算一个数字的二进制表示中 1 的数量。在这个查询中，它计算上一步`BIT_OR`操作结果中的位数，这个位数就是该月中有记录的不同天数。
4. `GROUP BY year,month`：这个语句指示 MySQL 按照年和月来分组记录。对于每个组，上述的位运算将单独执行。

综上所述，这个查询对于每个月份，计算了`t1`表中有记录的不同天数。这种方法特别适用于处理稀疏数据，即不是每一天都有记录的情况。通过位运算，它能高效地对存在记录的天数进行计数，而不需要存储一个完整的日期列表。

wei

[MySQL :: MySQL 8.0 Reference Manual :: 3.6.8 Calculating Visits Per Day](https://dev.mysql.com/doc/refman/8.0/en/calculating-days.html)

[MySQL :: MySQL 8.0 Reference Manual :: 12.12 Bit Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html)
