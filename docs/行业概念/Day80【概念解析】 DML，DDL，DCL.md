---
password: ""
icon: ""
date: "2023-12-10"
type: Post
category: 行业概念
slug: industry-day80
tags:
  - 行业概念
  - 文字
  - 思考
  - MySQL
summary: ""
title: Day80【概念解析】 DML，DDL，DCL
status: Published
urlname: 2a4573be-2451-438d-89b2-1b5b4906d687
updated: "2023-12-11 12:08:00"
---

# 整理定义

SQL 中的三种语言，DML，DDL，DCL。

**DML**(Data manipulation language) 数据操作语言，包含 `INSERT` `UPDATE` `DELETE` `SELECT` 的 SQL 语句，统称为 DML。

**DDL**(Data definition language) 数据定义语言，包含 `CREATE` `ALTER` `DROP` `TRUNCATE` 的 SQL 语句，是用于管理数据库本身而不是单独的表行的 SQL 语句。

**DCL**(Data control language) 数据控制语言，包含 `GRANT` 与 `REVOKE`，是管理权限的 SQL 语句

> **DML(**Data manipulation language**)**

    Data manipulation language, a set of _**SQL**_ statements for performing [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html), [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html), and [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html) operations. The [`SELECT`](https://dev.mysql.com/doc/refman/8.0/en/select.html) statement is sometimes considered as a DML statement, because the `SELECT ... FOR UPDATE` form is subject to the same considerations for _**locking**_ as [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html), [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html), and [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html).


    DML statements for an `InnoDB` table operate in the context of a _**transaction**_, so their effects can be _**committed**_ or _**rolled back**_ as a single unit.

> **DDL(**Data definition language**)**

    Data definition language, a set of _**SQL**_ statements for manipulating the database itself rather than individual table rows. Includes all forms of the `CREATE`, `ALTER`, and `DROP` statements. Also includes the `TRUNCATE` statement, because it works differently than a `DELETE FROM` _**`table_name`**_ statement, even though the ultimate effect is similar.


    DDL statements automatically _**commit**_ the current _**transaction**_; they cannot be _**rolled back**_.

> **DCL(**Data control language**)**  
> Data control language, a set of ***SQL*** statements for managing privileges. In MySQL, consists of the [`GRANT`](https://dev.mysql.com/doc/refman/8.0/en/grant.html) and [`REVOKE`](https://dev.mysql.com/doc/refman/8.0/en/revoke.html) statements. Contrast with ***DDL*** and ***DML***.

# 复述展开

下面是一个简单的表格，概述了 DML、DDL 和 DCL 的定义、区别、使用方式以及示例：

| 类别                             | 定义                                         | 区别                                     | 使用方式                                                   | 示例                                                               |
| -------------------------------- | -------------------------------------------- | ---------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------ |
| DML (Data Manipulation Language) | 用于添加、删除、更新和查询数据库记录的语言。 | 直接与数据记录进行交互，不会改变表结构。 | 通常在常规数据库操作中使用，如插入、更新、删除和查询数据。 | INSERT INTO table_name (column1, column2) VALUES (value1, value2); |

UPDATE table_name SET column1 = value1 WHERE condition;
DELETE FROM table_name WHERE condition;
SELECT \* FROM table_name; |
| DDL (Data Definition Language) | 用于定义和修改数据库结构的语言。 | 改变表结构，如创建或删除表和索引。 | 在数据库设计和建模阶段使用，或者在需要修改表结构时使用。 | CREATE TABLE table_name (column1 datatype, column2 datatype);
ALTER TABLE table_name ADD column_name datatype;
DROP TABLE table_name;
CREATE INDEX index_name ON table_name (column_name); |
| DCL (Data Control Language) | 用于控制数据库中的数据访问权限的语言。 | 与数据的安全性、访问权限和控制相关。 | 在需要管理用户权限和控制对数据库对象访问时使用。 | GRANT SELECT ON database.table TO 'user'@'host';
REVOKE SELECT ON database.table FROM 'user'@'host'; |

# 理解体会

理解这三者的定义以及区别，能够更快地了解不同 SQL 语句的差异，方便后续对于 SQL 的操作。
