---
password: ""
icon: ""
date: "2023-12-27"
type: Post
category: 技术分享
slug: java-compare
tags:
  - Java
summary: ""
title: "[实践]谈Java中的比较"
status: Published
urlname: a657dc01-1842-48f0-99f0-1eec78437ce0
updated: "2023-12-27 09:56:00"
---

> 😀 今天 CR 代码时，看到有人使用 `!=` 来比较 `Integer`类型以及`String`类型的大小，编译器提示：  
> `Number object are compared using ‘!=’, not ‘equals()’` ，今天就来说说 Java 中的类型比较吧。

# 相等比较

为什么不能使用`!=` 或者 `==` 来比较 Integer 和 String 是否相等？

`!=` 或者 `==` 使用时，实际上比较的是两个对象的引用是否相同，如果不是同一个引用的话，会返回不符合预期的结果。

**示例 1：**

```java
jshell> Integer i1 = 128;
i1 ==> 128

jshell> Integer i2 = 128;
i2 ==> 128

jshell> System.out.print(i1==i2)
false
```

**示例 2：**

```java
jshell> String s1 = "kuang";
s1 ==> "kuang"

jshell> String s2 = new String("kuang");
s2 ==> "kuang"

jshell> System.out.print(s1==s2);
false
```

# 原理分析

对于经常使用 C++的同学来说，很容用错。

在 Java 中，对于基本类型的比较，可以直接使用 `!=` 或者 `==` 。`Integer` 是 基本类型 `int` 的包装类型，是一个对象。`String`也是对象，使用 `!=` 或者 `==` 都是比较对象的引用而不是实际的值，所以逻辑上会有问题。

```java
jshell> Integer i5 = 129;
i5 ==> 129

jshell> Integer i6 = 129;
i6 ==> 129

jshell> System.identityHashCode(i5);
$15 ==> 485041780

jshell> System.identityHashCode(i6);
$16 ==> 117244645
```

可以看到，i5 和 i6 两者的地址是不一样的，所以比较的话，使用 `==` 肯定是返回 false 了。

同理，s7 和 s8 也是不一样的。

```java
jshell> String s7 = "kuang";
s7 ==> "kuang"

jshell> String s8 = new String("kuang");
s8 ==> "kuang"

jshell> String s9 = "kuang";
s9 ==> "kuang"

jshell> String s10 = new String("kuang");
s10 ==> "kuang"

jshell> System.identityHashCode(s7);
$19 ==> 832947102

jshell> System.identityHashCode(s8);
$20 ==> 507084503

jshell> System.identityHashCode(s9);
$22 ==> 832947102

jshell> System.identityHashCode(s10);
$24 ==> 1845904670
```

在 Java 中，字符串有两种创建方式：一种是直接使用双引号创建，如 `String s7 = "kuang"`；另一种是通过`new`关键字创建，如 `String s8 = new String("kuang")`。

1. 当我们使用双引号创建字符串时，Java 会首先在字符串常量池中查找是否已经存在值为"kuang"的字符串。如果存在，就直接返回这个字符串的引用；如果不存在，就在字符串常量池中创建一个新的字符串，并返回这个新字符串的引用。**这种方式创建的字符串，相同内容的字符串在内存中只有一份，可以节省内存**。
2. 当我们使用`new`关键字创建字符串时，Java 会在堆内存中**创建一个新的字符串对象**，不管字符串常量池中是否已经存在值为"kuang"的字符串。**这种方式创建的字符串，即使内容相同，也会在内存中创建多份**。

在上面的例子中，`s7`、`s8`、`s9`、`s10`虽然内容相同，但是它们在内存中的位置是不同的。`s7`是在字符串常量池中创建的，而`s8`是在堆内存中创建的。`System.identityHashCode()`方法返回的是对象在内存中的地址，所以`s7`和`s8`的地址是不同的。

> 🔥 **特别注意**  
> 在 Java 中，`Integer`是一个类，用于封装基本数据类型`int`的数据。`Integer`的值是不可变的，一旦创建，其值就不能改变。
>
> Java 为了提高性能和减少内存消耗，对-128 到 127 之间的`Integer`对象进行了缓存，这些对象在 Java 虚拟机启动时就已经创建好了。当我们调用`Integer.valueOf()`方法或者直接赋值（如`Integer i = 127`）时，如果值在-128 到 127 之间，Java 就会直接返回缓存中的对象，而不是新创建一个对象。这就是为什么在这个范围内，两个相同值的`Integer`对象的`System.identityHashCode()`返回的值是相同的。
>
> 然而，对于超过这个范围的值，如 129，Java 会新创建一个`Integer`对象。所以，即使两个`Integer`对象的值相同，它们在内存中的地址也是不同的，`System.identityHashCode()`返回的值也是不同的。
>
> ```java
> jshell> Integer i3 = 12;
> i3 ==> 12
>
> jshell> Integer i4 = 12;
> i4 ==> 12
>
> jshell> System.out.print(i3==i4);
> true
> ```

# 正确使用方法？

使用 `equals` ，它比较的是两个对象的值是否一致。

**Integer 类型：**

```java
Integer i1 = 1200;
Integer i2 = 1200;
boolean isEqual = i1.equals(i2);
```

**String 类型：**

```java
String str1 = "hello";
String str2 = "world";
boolean isEqual = str1.equals(str2); // false
int comparisonResult = str1.compareTo(str2); // negative value, because "hello" is less than "world"
```
