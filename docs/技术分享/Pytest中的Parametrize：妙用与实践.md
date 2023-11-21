---
password: ""
icon: ""
date: "2023-11-07"
type: Post
category: 技术分享
slug: parametrize
tags:
  - 推荐
  - 必看精选
  - Python
summary: Parametrize是pytest测试框架中的一个强大特性，它允许我们为测试函数提供多组参数和预期结果，从而轻松地创建多个测试用例。这种方法提高了代码的可读性和可维护性，提高了测试覆盖率和测试效率。然而，参数过多时可能会导致测试函数的复杂性增加，且参数之间的依赖关系可能需要额外处理。Parametrize非常适用于测试同一个函数或方法在不同参数下的行为、创建大量相似的测试用例以及进行参数化的并行测试。
title: Pytest中的Parametrize：妙用与实践
status: Published
cover: "https://source.unsplash.com/random/720x480/?encryption"
urlname: 1aaaa6d6-26d0-498d-a4f6-95bcacac8f57
updated: "2023-11-07 11:06:00"
---

> 😀 Parametrize 是 pytest 测试框架中的一个强大特性，它允许我们为测试函数提供多组参数和预期结果，从而轻松地创建多个测试用例。这种方法提高了代码的可读性和可维护性，提高了测试覆盖率和测试效率。然而，参数过多时可能会导致测试函数的复杂性增加，且参数之间的依赖关系可能需要额外处理。Parametrize 非常适用于测试同一个函数或方法在不同参数下的行为、创建大量相似的测试用例以及进行参数化的并行测试。

# Parametrize 简介

> ❓ Parametrize **是什么？**

> ✅ Parametrize 是 pytest 测试框架中的一个强大特性，它允许我们为测试函数提供多组参数和预期结果，从而可以轻松地创建多个测试用例。这种方法可以让我们轻松地为一个函数创建多个测试用例，而无需为每个用例都写一个单独的测试函数。

> ❓ **为什么要使用** Parametrize**？**

> ✅ 在编写测试用例时，我们经常会遇到需要测试同一个函数或方法在不同参数下的行为的情况。如果手动为每一种参数组合编写一个测试函数，那么工作量将会非常大，而且代码的可读性和可维护性也会大大降低。Parametrize 的出现，解决了这个问题，它允许我们在一个测试函数中定义多组参数和预期结果，从而可以轻松地创建多个测试用例。

# Parametrize 源码分析

参考: 参数化 Fixture 方法和测试函数。

`Metafunc.parametrize(`_`argnames`_`,`_`argvalues`_`,`_`indirect = False`_`,`_`ids = None`_`,`_`scope = None`_`)`

使用给定 argnames 的 argvalues 列表向基础测试函数添加新调用。在收集阶段执行参数化。如果你需要设置昂贵的资源,请参阅设置间接,以便在测试设置时进行。

参数：

- **argnames**以逗号分隔的字符串,表示一个或多个参数名称,或参数字符串的列表/元组。
- **argvalues** **argvalues**列表确定使用不同参数值调用测试的频率。如果只指定了一个 argname,则 argvalues 是值列表。如果指定了 N 个 argnames,则 argvalues 必须是 N 元组的列表,其中每个 tuple-element 为其各自的 argname 指定一个值。
- **indirect** argnames 或 boolean 的列表。参数列表名称(argnames 的子集)。如果为 True,则列表包含 argnames 中的所有名称。对应于此列表中的 argname 的每个 argvalue 将作为 request.param 传递到其各自的 argname fixture 函数,以便它可以在测试的设置阶段而不是在收集时执行更昂贵的设置。
- **ids** 字符串 ID 列表或可调用的列表。如果字符串,则每个字符串对应于 argvalues,以便它们是测试 ID 的一部分。如果将 None 作为特定测试的 id 给出,则将使用该参数的自动生成的 id。如果是可调用的,它应该采用一个参数(单个 argvalue)并返回一个字符串或返回 None。如果为 None,将使用该参数的自动生成的 id。如果没有提供 id,它们将自动从 argvalues 生成。
- **scope** 如果指定,则表示参数的范围。范围用于按参数实例对测试进行分组。它还将覆盖任何 fixture 函数定义的范围,允许使用测试上下文或配置设置动态范围。

# Parametrize 实践

以下是一个简单的例子，展示了如何使用 `pytest.mark.parametrize` 来测试一个函数的多个用例：

```python
import pytest

def add(a, b):
    return a + b

@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),# 第一组测试用例
    (2, 3, 5),# 第二组测试用例
    (3, 5, 8),# 第三组测试用例
    (4, 5, 9) # 第四组测试用例
])
def test_add(a, b, expected):
    assert add(a, b) == expected
```

在这个例子中，我们首先定义了一个简单的 `add` 函数，然后使用 `pytest.mark.parametrize` 来为 `test_add` 测试函数提供了四组参数。
每组参数都包含了两个输入 `a` 和 `b`，以及一个预期的输出 `expected`。当运行这个测试时，pytest 会自动为每组参数创建一个测试用例，然后检查 `add(a, b)` 的返回值是否等于 `expected`。

这种方法可以让你轻松地为一个函数创建多个测试用例，而无需为每个用例都写一个单独的测试函数。

# Parametrize **优缺点**

## 优点：

1. 提高代码的可读性和可维护性：通过使用 parametrize，我们可以在一个测试函数中定义多组参数和预期结果，从而避免了为每一种参数组合都编写一个单独的测试函数。
2. 提高测试覆盖率：parametrize 使得我们可以轻松地为一个函数创建多个测试用例，从而提高了测试覆盖率。
3. 提高测试效率：parametrize 支持并行测试，可以大大提高测试效率。

## 缺点：

1. 参数过多时，可能会导致测试函数的复杂性增加。
2. 如果参数之间有依赖关系，可能需要额外的逻辑来处理。

# Parametrize **使用场景**

Parametrize 非常适合用于以下场景：

1. 需要测试同一个函数或方法在不同参数下的行为。
2. 需要创建大量相似的测试用例。
3. 需要进行参数化的并行测试。

总的来说，pytest 的 parametrize 是一个非常强大的工具，它可以大大提高我们编写和维护测试用例的效率。

# 参考：

[Parametrizing tests — pytest documentation](https://docs.pytest.org/en/7.1.x/example/parametrize.html)
