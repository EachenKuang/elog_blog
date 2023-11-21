---
password: ""
icon: ""
date: "2023-11-02"
type: Post
category: 行业概念
slug: industry-day42
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day42【概念解析】迭代器模式
status: Published
cover: "https://images.unsplash.com/photo-1680357533402-def806337269?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 9262dea6-2976-46b6-b232-fde19c95db3c
updated: "2023-11-02 11:27:00"
---

# 整理定义

中文名称：迭代器模式

英文名称：iterator pattern

| 出处           | 定义                                                                                                                                                                                         |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书 | 一种设计模式。通过用迭代器访问一个容器中的全体对象，可以把相应的算法从容器中分离出来，以简化容器设计的一种设计模式。但是也有一些与容器特性密切相关的算法，是不能从容器中分离的。又称游标模式 |
| 设计模式       | **迭代器模式**是一种行为设计模式，  让你能在不暴露集合底层表现形式  （列表、  栈和树等）  的情况下遍历集合中所有的元素。                                                                     |
| 百度百科       | 迭代器模式（Iterator），提供一种方法顺序访问一个聚合对象中的各种元素，而又不暴露该对象的内部表示。                                                                                           |
| 维基百科       | 在面向对象编程中，迭代器模式是一种设计模式，其中迭代器用于遍历容器并访问容器的元素。 迭代器模式将算法与容器解耦； 在某些情况下，算法必然是特定于容器的，因此无法解耦。                       |

# 复述展开

## What is iterator pattern?

> 📌 迭代器模式是一种行为设计模式。它通过迭代器来访问容器的全体对象，在不暴露对象的内部结构的同时，提供一种顺序遍历全体对象。它能够将算法与容器解耦。

## 🌰\*1

**图书馆的书籍管理**：在图书馆中，我们可以使用迭代器模式来遍历书架上的所有书籍。每一本书都可以被看作是一个元素，而书架则是一个聚合对象。我们可以创建一个迭代器，用于遍历书架上的每一本书，而无需知道书架的内部结构。

## 🌰\*2

**电视遥控器**：电视遥控器上的频道切换按钮就是一个迭代器模式的例子。当我们按下频道切换按钮时，电视会按照一定的顺序切换到下一个频道。在这个过程中，我们无需知道电视内部如何存储和管理这些频道，只需要使用遥控器上的按钮就可以遍历所有的频道。

## 🌰\*3

在 Java 的容器中，像 ArrayList 就提供了 iterator 方法，提供了可以遍历使用的迭代器。

```java
import java.util.ArrayList;
import java.util.Iterator;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<String>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");

        Iterator<String> it = list.iterator();
        while(it.hasNext()){
            String fruit = it.next();
            System.out.println(fruit);
        }
    }
}
```

使用 ArrayList 的迭代器时，有一些注意事项：

1. **并发修改**：如果在使用迭代器遍历 ArrayList 的过程中，对 ArrayList 进行了结构性修改（如添加、删除元素），那么将会抛出 ConcurrentModificationException。为了避免这种情况，可以使用迭代器的 remove()方法来删除元素，或者使用 CopyOnWriteArrayList。
2. **next()方法的使用**：在调用 next()方法获取下一个元素之前，应该先调用 hasNext()方法检查是否还有下一个元素。如果没有下一个元素，但是仍然调用了 next()方法，那么将会抛出 NoSuchElementException。
3. **迭代器的单向性**：ArrayList 的迭代器是单向的，也就是说，它只能从前向后遍历元素，不能从后向前遍历。如果需要从后向前遍历元素，可以使用 ListIterator。

## 迭代器模式的结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/032e46f3-7bf8-47e5-9275-e8d2c71fc8bc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120417Z&X-Amz-Expires=3600&X-Amz-Signature=49a7e62d50ed6fba5b948fd4f128d85e2edc3d7a19ba1e7e1bc72edabf1e6c02&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **迭代器** （Iterator）  接口声明了遍历集合所需的操作：  获取下一个元素、  获取当前位置和重新开始迭代等。
2. **具体迭代器** （Concrete Iterators）  实现遍历集合的一种特定算法。  迭代器对象必须跟踪自身遍历的进度。  这使得多个迭代器可以相互独立地遍历同一集合。
3. **集合** （Collection）  接口声明一个或多个方法来获取与集合兼容的迭代器。  请注意，  返回方法的类型必须被声明为迭代器接口，  因此具体集合可以返回各种不同种类的迭代器。
4. **具体集合** （Concrete Collections）  会在客户端请求迭代器时返回一个特定的具体迭代器类实体。  你可能会琢磨，  剩下的集合代码在什么地方呢？  不用担心，  它也会在同一个类中。  只是这些细节对于实际模式来说并不重要，  所以我们将其省略了而已。
5. **客户端** （Client）  通过集合和迭代器的接口与两者进行交互。  这样一来客户端无需与具体类进行耦合，  允许同一客户端代码使用各种不同的集合和迭代器。

   客户端通常不会自行创建迭代器，  而是会从集合中获取。  但在特定情况下，  客户端可以直接创建一个迭代器  （例如当客户端需要自定义特殊迭代器时）。

## 优缺点

优点：

- _单一职责原则_。  通过将体积庞大的遍历算法代码抽取为独立的类，  你可对客户端代码和集合进行整理。
- _开闭原则_。  你可实现新型的集合和迭代器并将其传递给现有代码，  无需修改现有代码。
- 你可以并行遍历同一集合，  因为每个迭代器对象都包含其自身的遍历状态。
- 相似的，  你可以暂停遍历并在需要时继续。

缺点：

- 如果你的程序只与简单的集合进行交互，  应用该模式可能会矫枉过正。
- 对于某些特殊集合，  使用迭代器可能比直接遍历的效率低。

# 理解体会

迭代器模式在实际的代码开发中随处可见，Java 所有的容器都需要实现 Iterator 这个接口，并提供 next() 方法进行顺序遍历。

在 Python 中，`for i in range(10)` 就是一个非常简单的迭代器，这个 for 循环可以从 0 遍历到 9。

掌握好迭代器也是非常重要的。

> 📌 **快速跳转链接**  
> 【概念解析】启动
>
> 【概念解析】Day 1 - 10
>
> 【概念解析】Day 11 - 20
>
> 【概念解析】Day 21 - 30
>
> 【概念解析】Day 31 - 40
>
> 【概念解析】Day 41 - 50
>
> 【概念解析】Day 51 - 60
>
> 【概念解析】Day 61 - 70

<details>
<summary>【概念解析】启动</summary>

[bookmark](https://kuangyichen.com/article/industry)

[bookmark](https://kuangyichen.com/article/start-industry-100-words)

</details>

<details>
<summary>【概念解析】Day 1 - 10</summary>

[bookmark](https://kuangyichen.com/article/industry-day1)

[bookmark](https://kuangyichen.com/article/industry-day2)

[bookmark](https://kuangyichen.com/article/industry-day3)

[bookmark](https://kuangyichen.com/article/industry-day4)

[bookmark](https://kuangyichen.com/article/industry-day5)

[bookmark](https://kuangyichen.com/article/industry-day6)

[bookmark](https://kuangyichen.com/article/industry-day7)

[bookmark](https://kuangyichen.com/article/industry-day8)

[bookmark](https://kuangyichen.com/article/industry-day9)

[bookmark](https://kuangyichen.com/article/industry-day10)

</details>

<details>
<summary>【概念解析】Day 11 - 20</summary>

[bookmark](https://kuangyichen.com/article/industry-day11)

[bookmark](https://kuangyichen.com/article/industry-day12)

[bookmark](https://kuangyichen.com/article/industry-day13)

[bookmark](https://kuangyichen.com/article/industry-day14)

[bookmark](https://kuangyichen.com/article/industry-day15)

[bookmark](https://kuangyichen.com/article/industry-day16)

[bookmark](https://kuangyichen.com/article/industry-day17)

[bookmark](https://kuangyichen.com/article/industry-day18)

[bookmark](https://kuangyichen.com/article/industry-day19)

[bookmark](https://kuangyichen.com/article/industry-day20)

</details>

<details>
<summary>【概念解析】Day 21 - 30</summary>

[bookmark](https://kuangyichen.com/article/industry-day21)

[bookmark](https://kuangyichen.com/article/industry-day22)

[bookmark](https://kuangyichen.com/article/industry-day23)

[bookmark](https://kuangyichen.com/article/industry-day24)

[bookmark](https://kuangyichen.com/article/industry-day25)

[bookmark](https://kuangyichen.com/article/industry-day26)

[bookmark](https://kuangyichen.com/article/industry-day27)

[bookmark](https://kuangyichen.com/article/industry-day28)

[bookmark](https://kuangyichen.com/article/industry-day29)

[bookmark](https://kuangyichen.com/article/industry-day30)

</details>

<details>
<summary>【概念解析】Day 31 - 40</summary>

[bookmark](https://kuangyichen.com/article/industry-day31)

[bookmark](https://kuangyichen.com/article/industry-day32)

[bookmark](https://kuangyichen.com/article/industry-day33)

[bookmark](https://kuangyichen.com/article/industry-day34)

[bookmark](https://kuangyichen.com/article/industry-day35)

[bookmark](https://kuangyichen.com/article/industry-day36)

[bookmark](https://kuangyichen.com/article/industry-day37)

[bookmark](https://kuangyichen.com/article/industry-day38)

[bookmark](https://kuangyichen.com/article/industry-day39)

[bookmark](https://kuangyichen.com/article/industry-day40)

</details>

<details>
<summary>【概念解析】Day 41 - 50</summary>

[bookmark](https://kuangyichen.com/article/industry-day41)

[bookmark](https://kuangyichen.com/article/industry-day42)

[bookmark](https://kuangyichen.com/article/industry-day43)

[bookmark](https://kuangyichen.com/article/industry-day44)

[bookmark](https://kuangyichen.com/article/industry-day45)

[bookmark](https://kuangyichen.com/article/industry-day46)

[bookmark](https://kuangyichen.com/article/industry-day47)

[bookmark](https://kuangyichen.com/article/industry-day48)

[bookmark](https://kuangyichen.com/article/industry-day49)

[bookmark](https://kuangyichen.com/article/industry-day50)

</details>

<details>
<summary>【概念解析】Day 51 - 60</summary>

[bookmark](https://kuangyichen.com/article/industry-day51)

[bookmark](https://kuangyichen.com/article/industry-day52)

[bookmark](https://kuangyichen.com/article/industry-day53)

[bookmark](https://kuangyichen.com/article/industry-day54)

[bookmark](https://kuangyichen.com/article/industry-day55)

[bookmark](https://kuangyichen.com/article/industry-day56)

[bookmark](https://kuangyichen.com/article/industry-day57)

[bookmark](https://kuangyichen.com/article/industry-day58)

[bookmark](https://kuangyichen.com/article/industry-day59)

</details>
