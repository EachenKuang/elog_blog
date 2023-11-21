---
password: ""
icon: ""
date: "2023-10-21"
type: Post
category: 行业概念
slug: industry-day30
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day30【概念解析】建造者模式
status: Published
cover: "https://www.notion.so/images/page-cover/met_georges_seurat_1884.jpg"
urlname: 56de3f1f-0d9c-4f3a-ace8-0f528afebc8a
updated: "2023-10-27 19:25:00"
---

# 整理定义

中文名称：建造者模式/生成器模式

英文名称：builder pattern

| 出处                                                                                                                                          | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 中国大百科全书                                                                                                                                | 一种设计模式。把复杂对象的创建与表示分离，使得同样的创建过程可以创建不同的表示。                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 百度百科                                                                                                                                      | 建造者模式是[设计模式](https://baike.baidu.com/item/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/1212549?fromModule=lemma_inlink)的一种，将一个[复杂对象](https://baike.baidu.com/item/%E5%A4%8D%E6%9D%82%E5%AF%B9%E8%B1%A1/53551848?fromModule=lemma_inlink)的构建与它的表示[分离](https://baike.baidu.com/item/%E5%88%86%E7%A6%BB/6369610?fromModule=lemma_inlink)，使得同样的构建过程可以创建不同的表示。                                                                                                                                                        |
| 维基百科                                                                                                                                      | The **builder pattern** is a [design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) designed to provide a flexible solution to various object creation problems in [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming). The intent of the builder design pattern is to [separate](https://en.wikipedia.org/wiki/Separation_of_concerns) the construction of a complex object from its representation. It is one of the [Gang of Four design patterns](https://en.wikipedia.org/wiki/Design_Patterns). |
| 【构建器模式是一种设计模式，旨在为面向对象编程中的各种对象创建问题提供灵活的解决方案。 构建器设计模式的目的是将复杂对象的构造与其表示分离。】 |
| 设计模式                                                                                                                                      | **生成器模式**是一种创建型设计模式，  使你能够分步骤创建复杂对象。  该模式允许你使用相同的创建代码生成不同类型和形式的对象。                                                                                                                                                                                                                                                                                                                                                                                                                                |

# 复述展开

**建造者模式是创建型对象模式**

![202310201939862.png](https://image.kuangyichen.com/image/202310201939862.png)

## 建造者模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/a34ef52c-0a18-44bb-b168-e76677660a46/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120504Z&X-Amz-Expires=3600&X-Amz-Signature=3069b542cb92caa57bccdbcce4f85b314af61aed30ccdf0603c625ba91cdbc9a&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **生成器** （Builder）  接口声明在所有类型生成器中通用的产品构造步骤。
2. **具体生成器** （Concrete Builders）  提供构造过程的不同实现。  具体生成器也可以构造不遵循通用接口的产品。
3. **产品** （Products）  是最终生成的对象。  由不同生成器构造的产品无需属于同一类层次结构或接口。
4. **主管** （Director）  类定义调用构造步骤的顺序，  这样你就可以创建和复用特定的产品配置。
5. **客户端** （Client）  必须将某个生成器对象与主管类关联。  一般情况下，  你只需通过主管类构造函数的参数进行一次性关联即可。  此后主管类就能使用生成器对象完成后续所有的构造任务。  但在客户端将生成器对象传递给主管类制造方法时还有另一种方式。  在这种情况下，  你在使用主管类生产产品时每次都可以使用不同的生成器。

## 建造者模式的适用场景

> 📌 **使用生成器模式可避免  “重叠构造函数  （telescoping constructor）”  的出现。**

> 📌 **当你希望使用代码创建不同形式的产品  （例如石头或木头房屋）  时，  可使用生成器模式。**

> 📌 **使用生成器构造**[**组合**](https://refactoringguru.cn/design-patterns/composite)**树或其他复杂对象。**

## 建造者模式的优缺点

**优点：**

- 你可以分步创建对象，  暂缓创建步骤或递归运行创建步骤。
- 生成不同形式的产品时，  你可以复用相同的制造代码。
- _单一职责原则_。  你可以将复杂构造代码从产品的业务逻辑中分离出来。

**缺点：**

- 由于该模式需要新增多个类，  因此代码整体复杂程度会有所增加。

```python
from __future__ import annotations
from abc import ABC, abstractmethod
from typing import Any


class Builder(ABC):
    """
    The Builder interface specifies methods for creating the different parts of
    the Product objects.
    """

    @property
    @abstractmethod
    def product(self) -> None:
        pass

    @abstractmethod
    def produce_part_a(self) -> None:
        pass

    @abstractmethod
    def produce_part_b(self) -> None:
        pass

    @abstractmethod
    def produce_part_c(self) -> None:
        pass


class ConcreteBuilder1(Builder):
    """
    The Concrete Builder classes follow the Builder interface and provide
    specific implementations of the building steps. Your program may have
    several variations of Builders, implemented differently.
    """

    def __init__(self) -> None:
        """
        A fresh builder instance should contain a blank product object, which is
        used in further assembly.
        """
        self.reset()

    def reset(self) -> None:
        self._product = Product1()

    @property
    def product(self) -> Product1:
        """
        Concrete Builders are supposed to provide their own methods for
        retrieving results. That's because various types of builders may create
        entirely different products that don't follow the same interface.
        Therefore, such methods cannot be declared in the base Builder interface
        (at least in a statically typed programming language).

        Usually, after returning the end result to the client, a builder
        instance is expected to be ready to start producing another product.
        That's why it's a usual practice to call the reset method at the end of
        the `getProduct` method body. However, this behavior is not mandatory,
        and you can make your builders wait for an explicit reset call from the
        client code before disposing of the previous result.
        """
        product = self._product
        self.reset()
        return product

    def produce_part_a(self) -> None:
        self._product.add("PartA1")

    def produce_part_b(self) -> None:
        self._product.add("PartB1")

    def produce_part_c(self) -> None:
        self._product.add("PartC1")


class Product1():
    """
    It makes sense to use the Builder pattern only when your products are quite
    complex and require extensive configuration.

    Unlike in other creational patterns, different concrete builders can produce
    unrelated products. In other words, results of various builders may not
    always follow the same interface.
    """

    def __init__(self) -> None:
        self.parts = []

    def add(self, part: Any) -> None:
        self.parts.append(part)

    def list_parts(self) -> None:
        print(f"Product parts: {', '.join(self.parts)}", end="")


class Director:
    """
    The Director is only responsible for executing the building steps in a
    particular sequence. It is helpful when producing products according to a
    specific order or configuration. Strictly speaking, the Director class is
    optional, since the client can control builders directly.
    """

    def __init__(self) -> None:
        self._builder = None

    @property
    def builder(self) -> Builder:
        return self._builder

    @builder.setter
    def builder(self, builder: Builder) -> None:
        """
        The Director works with any builder instance that the client code passes
        to it. This way, the client code may alter the final type of the newly
        assembled product.
        """
        self._builder = builder

    """
    The Director can construct several product variations using the same
    building steps.
    """

    def build_minimal_viable_product(self) -> None:
        self.builder.produce_part_a()

    def build_full_featured_product(self) -> None:
        self.builder.produce_part_a()
        self.builder.produce_part_b()
        self.builder.produce_part_c()


if __name__ == "__main__":
    """
    The client code creates a builder object, passes it to the director and then
    initiates the construction process. The end result is retrieved from the
    builder object.
    """

    director = Director()
    builder = ConcreteBuilder1()
    director.builder = builder

    print("Standard basic product: ")
    director.build_minimal_viable_product()
    builder.product.list_parts()

    print("\n")

    print("Standard full featured product: ")
    director.build_full_featured_product()
    builder.product.list_parts()

    print("\n")

    # Remember, the Builder pattern can be used without a Director class.
    print("Custom product: ")
    builder.produce_part_a()
    builder.produce_part_b()
    builder.product.list_parts()
```

# 理解体会

建造者模式，也叫生成器模式，可以在构建多个步骤时使用。可以通过不同的组合创建方式来构建复杂的对象。

生成器模式是 Python 中的一个著名模式。 当你需要创建一个可能有许多配置选项的对象时， 该模式会特别有用。

生成器模式可以通过类来识别，  它拥有一个构建方法和多个配置结果对象的方法。  生成器方法通常支持方法链  （例如  `someBuilder.setValueA(1).setValueB(2).create()`）。

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
