---
password: ""
icon: ""
date: "2023-10-19"
type: Post
category: 行业概念
slug: industry-day28
tags:
  - 行业概念
  - 文字
  - 思考
  - 设计模式
summary: ""
title: Day28 【概念解析】工厂方法模式
status: Published
cover: "https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/85f6c012-2895-49af-99a2-3e005710adbc/cover_%281%29.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120308Z&X-Amz-Expires=3600&X-Amz-Signature=454a57b0c0901b6044ad80fa0229f49bf871780b9849ddb0184529cb0988c8ee&X-Amz-SignedHeaders=host&x-id=GetObject"
urlname: 0af3d43f-8aae-4c4a-b6c8-7266fccd6991
updated: "2023-10-27 19:25:00"
---

# 整理定义

中文名称：工厂方法模式

英文名称：factory method pattern

| 出处                                                                  | 定义                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 中国大百科全书                                                        | 定义一个创建对象的接口，在运行期由子类决定创建哪一个类的实例的一种设计模式                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 维基百科                                                              | **工厂方法模式**（英语：Factory method pattern）是一种实现了“工厂”概念的[面向对象](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1)[设计模式](<https://zh.wikipedia.org/wiki/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F_(%E8%AE%A1%E7%AE%97%E6%9C%BA)>)。就像其他[创建型模式](https://zh.wikipedia.org/wiki/%E5%89%B5%E5%BB%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F)一样，它也是处理在不指定[对象](<https://zh.wikipedia.org/wiki/%E5%AF%B9%E8%B1%A1_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)>)具体[类别](<https://zh.wikipedia.org/wiki/%E7%B1%BB_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)>)的情况下创建对象的问题。工厂方法模式的实质是“定义一个创建对象的接口，但让实现这个接口的类别来决定实例化哪个类别。工厂方法让类别的实例化推迟到子类中进行。” |
| [设计模式](https://refactoringguru.cn/design-patterns/factory-method) | **工厂方法模式**是一种创建型设计模式，  其在父类中提供一个创建对象的方法，  允许子类决定实例化对象的类型。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

# 复述展开

工厂方法模式属于设计模式中的，创建型模式。

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/e13ef64c-1abd-42b6-a6db-792fa60fa7f9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120516Z&X-Amz-Expires=3600&X-Amz-Signature=007f4c1bcc3f91082d47949608436683bae30970b85907df424aadda9410dc05&X-Amz-SignedHeaders=host&x-id=GetObject)

## 工厂方法模式结构

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/8fd0b99d-1c79-4949-849d-39bf9a2449e3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120516Z&X-Amz-Expires=3600&X-Amz-Signature=7f66848871979c8ffe1f69ce7d8109b9385d9e4a7e9f87e5148a8d0d0ae1c98f&X-Amz-SignedHeaders=host&x-id=GetObject)

1. **产品** （Product）  将会对接口进行声明。  对于所有由创建者及其子类构建的对象，  这些接口都是通用的。
2. **具体产品** （Concrete Products）  是产品接口的不同实现。
3. **创建者** （Creator）  类声明返回产品对象的工厂方法。  该方法的返回对象类型必须与产品接口相匹配。
   你可以将工厂方法声明为抽象方法，  强制要求每个子类以不同方式实现该方法。  或者，  你也可以在基础工厂方法中返回默认产品类型。
   注意，  尽管它的名字是创建者，  但它最主要的职责并**不是**创建产品。  一般来说，  创建者类包含一些与产品相关的核心业务逻辑。  工厂方法将这些逻辑处理从具体产品类中分离出来。  打个比方，  大型软件开发公司拥有程序员培训部门。  但是，  这些公司的主要工作还是编写代码，  而非生产程序员。
4. **具体创建者** （Concrete Creators）  将会重写基础工厂方法，  使其返回不同类型的产品。

注意，  并不一定每次调用工厂方法都会**创建**新的实例。  工厂方法也可以返回缓存、  对象池或其他来源的已有对象。

## 工厂方法模式适合的应用场景

> ❓ **当你在编写代码的过程中，  如果无法预知对象确切类别及其依赖关系时，  可使用工厂方法。**  
> 工厂方法将创建产品的代码与实际使用产品的代码分离，  从而能在不影响其他代码的情况下扩展产品创建部分代码。
>
> 例如，  如果需要向应用中添加一种新产品，  你只需要开发新的创建者子类，  然后重写其工厂方法即可。

    工厂方法将创建产品的代码与实际使用产品的代码分离， 从而能在不影响其他代码的情况下扩展产品创建部分代码。


    例如， 如果需要向应用中添加一种新产品， 你只需要开发新的创建者子类， 然后重写其工厂方法即可。

> ❓ **如果你希望用户能扩展你软件库或框架的内部组件，  可使用工厂方法。**  
> 继承可能是扩展软件库或框架默认行为的最简单方法。  但是当你使用子类替代标准组件时，  框架如何辨识出该子类？
>
> 解决方案是将各框架中构造组件的代码集中到单个工厂方法中，  并在继承该组件之外允许任何人对该方法进行重写。
>
> 让我们看看具体是如何实现的。  假设你使用开源 UI 框架编写自己的应用。  你希望在应用中使用圆形按钮，  但是原框架仅支持矩形按钮。  你可以使用  `圆形按钮`Round­Button 子类来继承标准的  `按钮`Button 类。  但是，  你需要告诉  `UI框架`UIFramework 类使用新的子类按钮代替默认按钮。
>
> 为了实现这个功能，  你可以根据基础框架类开发子类  `圆形按钮 UI`UIWith­Round­Buttons ，  并且重写其  `create­Button`创建按钮方法。  基类中的该方法返回  `按钮`对象，  而你开发的子类返回  `圆形按钮`对象。  现在，  你就可以使用  `圆形按钮 UI`类代替  `UI框架`类。  就是这么简单！

    继承可能是扩展软件库或框架默认行为的最简单方法。 但是当你使用子类替代标准组件时， 框架如何辨识出该子类？


    解决方案是将各框架中构造组件的代码集中到单个工厂方法中， 并在继承该组件之外允许任何人对该方法进行重写。


    让我们看看具体是如何实现的。 假设你使用开源 UI 框架编写自己的应用。 你希望在应用中使用圆形按钮， 但是原框架仅支持矩形按钮。 你可以使用 `圆形按钮`Round­Button子类来继承标准的 `按钮`Button类。 但是， 你需要告诉 `UI框架`UIFramework类使用新的子类按钮代替默认按钮。


    为了实现这个功能， 你可以根据基础框架类开发子类 `圆形按钮 UI`UIWith­Round­Buttons ， 并且重写其 `create­Button`创建按钮方法。 基类中的该方法返回 `按钮`对象， 而你开发的子类返回 `圆形按钮`对象。 现在， 你就可以使用 `圆形按钮 UI`类代替 `UI框架`类。 就是这么简单！

> ❓ **如果你希望复用现有对象来节省系统资源，  而不是每次都重新创建对象，  可使用工厂方法。**  
> 在处理大型资源密集型对象  （比如数据库连接、  文件系统和网络资源）  时，  你会经常碰到这种资源需求。
>
> 让我们思考复用现有对象的方法：
>
> 1. 首先，  你需要创建存储空间来存放所有已经创建的对象。
>
> 2. 当他人请求一个对象时，  程序将在对象池中搜索可用对象。
>
> 3. …  然后将其返回给客户端代码。
>
> 4. 如果没有可用对象，  程序则创建一个新对象  （并将其添加到对象池中）。
>
> 这些代码可不少！  而且它们必须位于同一处，  这样才能确保重复代码不会污染程序。
>
> 可能最显而易见，  也是最方便的方式，  就是将这些代码放置在我们试图重用的对象类的构造函数中。  但是从定义上来讲，  构造函数始终返回的是**新对象**，  其无法返回现有实例。
>
> 因此，  你需要有一个既能够创建新对象，  又可以重用现有对象的普通方法。  这听上去和工厂方法非常相像。

    在处理大型资源密集型对象 （比如数据库连接、 文件系统和网络资源） 时， 你会经常碰到这种资源需求。


    让我们思考复用现有对象的方法：

    1. 首先， 你需要创建存储空间来存放所有已经创建的对象。
    2. 当他人请求一个对象时， 程序将在对象池中搜索可用对象。
    3. … 然后将其返回给客户端代码。
    4. 如果没有可用对象， 程序则创建一个新对象 （并将其添加到对象池中）。

    这些代码可不少！ 而且它们必须位于同一处， 这样才能确保重复代码不会污染程序。


    可能最显而易见， 也是最方便的方式， 就是将这些代码放置在我们试图重用的对象类的构造函数中。 但是从定义上来讲， 构造函数始终返回的是**新对象**， 其无法返回现有实例。


    因此， 你需要有一个既能够创建新对象， 又可以重用现有对象的普通方法。 这听上去和工厂方法非常相像。

## 工厂方法模式的优缺点

**优点：**

- 你可以避免创建者和具体产品之间的紧密耦合。
- _**单一职责原则**_。  你可以将产品创建代码放在程序的单一位置，  从而使得代码更容易维护。
- _**开闭原则**_。  无需更改现有客户端代码，  你就可以在程序中引入新的产品类型。

**缺点：**

- 应用工厂方法模式需要引入许多新的子类，  代码可能会因此变得更复杂。  最好的情况是将该模式引入创建者类的现有层次结构中。

# 理解体会

**在理解工厂方法模式以及工厂相关的设计模式之前，需要对以下概念做一些区分和了解。**

> 工厂：**工厂**是一个含义模糊的术语，  表示可以创建一些东西的函数、  方法或类。  最常见的情况下，  工厂创建的是对象。  但是它们也可以创建文件和数据库记录等其他东西。

> 简单工厂模式：描述了一个类， 它拥有一个包含大量条件语句的构建方法， 可根据方法的参数来选择对何种产品进行初始化并将其返回。**简单工厂通常没有子类**。  但当从一个简单工厂中抽取出子类后，  它看上去就会更像经典的*工厂方法*模式了。

```javascript
// 简单工厂的例子
class UserFactory {
    public static function create($type) {
        switch ($type) {
            case 'user': return new User();
            case 'customer': return new Customer();
            case 'admin': return new Admin();
            default:
                throw new Exception('传递的用户类型错误。');
        }
    }
}
```

> 工厂方法模式：是一种创建型设计模式， 其在父类中提供一个创建对象的方法， 允许子类决定实例化对象的类型。

```javascript
// 工厂个方法模式的例子
abstract class Department {
    public abstract function createEmployee($id);

    public function fire($id) {
        $employee = $this->createEmployee($id);
        $employee->paySalary();
        $employee->dismiss();
    }
}

class ITDepartment extends Department {
    public function createEmployee($id) {
        return new Programmer($id);
    }
}

class AccountingDepartment extends Department {
    public function createEmployee($id) {
        return new Accountant($id);
    }
}
```

> 抽象工厂模式：是一种创建型设计模式， 它能创建一系列相关或相互依赖的对象， 而无需指定其具体类。

# 参考：

[工厂方法设计模式 (refactoringguru.cn)](https://refactoringguru.cn/design-patterns/factory-method)

[工厂模式和抽象工厂模式的区别 (refactoringguru.cn)](https://refactoringguru.cn/design-patterns/factory-comparison)

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
