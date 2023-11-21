---
password: ""
icon: ""
date: "2021-10-13"
type: Post
category: 技术分享
slug: go-reflect-laws
tags:
  - Go
  - 开发
  - 必看精选
summary: 最近在研究Go语言的源码，看到反射部分，结合The Go Blog系列的《The Laws of Reflection》，以及Go 1.15 中 src/reflect 部分源码，记录下对于Go 反射的一些见解。
title: 初探Go反射三大定律
status: Published
cover: "https://images.unsplash.com/photo-1642367340318-96fdbc5d30f5?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: c487e880-5297-4731-874f-6100ab24c44a
updated: "2023-09-26 15:51:00"
---

> 😀 最近在研究 Go 语言的源码，看到反射部分，结合 The Go Blog 系列的《The Laws of Reflection》，以及 Go 1.15 中 `src/reflect` 部分源码，记录下对于 Go 反射的一些见解。

# 〇、前情提要

Go 的反射基础是`接口`和`类型系统`。学习之前，最好先了解 Go 接口的实现，另外，反射的 API 也很多，了解其核心部分即可，一些其他 API 可以在通过源码分析来了解。笔者在本文中只是结合源码分析初探 Go 反射的三大定律。

![rId24.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/d45cad76-bdba-4806-862d-7d4091b1272b/rId24.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120748Z&X-Amz-Expires=3600&X-Amz-Signature=c77ce6d9a179194a5be2a8cacfa5521881ee3051538a039727238201989445a2&X-Amz-SignedHeaders=host&x-id=GetObject)

在反射的世界里，我们拥有了获取一个对象的类型，属性及方法的能力。

# 一、反射的基本概念

**什么是反射（reflect）**

维基百科如[是说明](https://en.wikipedia.org/wiki/Reflective_programming)：

> In computer science, reflective programming or reflection is the ability of a process to examine, introspect, and modify its own structure and behavior.

    在计算机科学中，反射式编程或反射是一个过程检查、内省和修改自身结构和行为的能力


    另外还配上了Go语言的一些示例代码


    ```go
    import "reflect"

    // Without reflection
    f := Foo{}
    f.Hello()

    // With reflection
    fT := reflect.TypeOf(Foo{})
    fV := reflect.New(fT)

    m := fV.MethodByName("Hello")
    if m.IsValid() {
     m.Call(nil)
    }
    ```

今天我们重点讲讲 Go 语言的三大反射定律

**为什么要使用反射？**

- Go 不支持泛型，通过反射可以间接实现泛型的需求
- 借助反射可以极大简化设计，不需要对每一种场景做硬编码处理
- 反射提供了一种程序了解自己和改变自己的能力，这为一些测试工具的开发提供了有力的支持。

# 二、反射的两种基本数据结构

Go 的反射巧妙地借助了实例到接口的转换所使用的数据结构，首先将实例传给内部的空接口，实际上是将实例类型转换为接口可以表述的数据结构`emptyInterface`，反射基于这个转换后的数据结构来访问和操作实例的值和类型。实例传递给 interface{} 类型，编译器会进行一个内部的转换，自动创建相关类型数据结构。

## 2.1 `reflect.Type`

```go
// Type 是 Go 类型的表示
//
// 并非所有方法都适用于所有类型。 每种方法的文档中都注明了限制（如果有）。
// 在调用特定于种类的方法之前，使用 Kind 方法找出类型的种类。
// 调用不适合该类型的方法会导致运行时panic。
//
// 类型值是可比较的，例如使用 == 运算符，因此它们可以用作映射键。
// 如果两个 Type 值表示相同的类型，则它们相等。
type Type interface {
    Align() int
    FieldAlign() int
    Method(int) Method
    MethodByName(string) (Method, bool)
    NumMethod() int
    Name() string
    PkgPath() string
    Size() uintptr
    String() string
    Kind() Kind
    Implements(u Type) bool
    AssignableTo(u Type) bool
    ConvertibleTo(u Type) bool
    Comparable() bool
    Bits() int
    ChanDir() ChanDir
    IsVariadic() bool
    Elem() Type
    Field(i int) StructField
    FieldByIndex(index []int) StructField
    FieldByName(name string) (StructField, bool)
    FieldByNameFunc(match func(string) bool) (StructField, bool)
    In(i int) Type
    Key() Type
    Len() int
    NumField() int
    NumIn() int
    NumOut() int
    Out(i int) Type
}
```

源码见`src/reflect/type.go`

可以看到，`Type`是一个接口，它里面定义了 28 个方法

为什么反射接口返回的是一个 Type 接口类型，而不是直接返回具体的类型结构呢

- 一是因为类型信息是一个只读的信息，不可能动态地修改类型的相关信息，那太不安全了；
- 二是因为不同的类型，类型定义也不一样，使用接口这一抽象数据结构能够进行统一的抽象。

## 2.2 `reflect.Value`

```go
// Value 是 Go 值的反射接口。
//
// 并非所有方法都适用于所有类型的值。 每种方法的文档中都注明了限制（如果有）。 在调用种类特定的方法之前，使用 Kind 方法找出值的种类。 调用不适合该类型的方法会导致运行时panic。(这里与Type是一致的)
//
// 零值表示no value。
// 它的 IsValid 方法返回 false，它的 Kind 方法返回 Invalid，它的 String 方法返回“<invalid Value>”，所有其他方法都会 panic。
// 大多数函数和方法从不返回无效值。如果有，其文档明确说明了条件。
//
// 一个值可以被多个 goroutine 同时使用，前提是底层的 Go 值可以同时用于等效的直接操作。
// 要比较两个值，请比较接口方法的结果。
// 在两个值上使用 == 不会比较它们代表的基础值。
type Value struct {
   typ *rtype
   ptr unsafe.Pointer
   flag
}
```

源码见`src/reflect/value.go`

可以发现，`Value`是一个结构体，它包含三个字段

- `typ` 值的类型指针
- `ptr` 指向值的指针
- `flag` 标记字段

另外，Value 还有 68 个方法，这里先就不赘述了，包括 61 个 public 方法和 7 个 private 方法

```text
Addr
Bool
Bytes
CanAddr
CanSet
Call
CallSlice
Cap
Close
Complex
Elem
Field
FieldByIndex
FieldByName
FieldByNameFunc
Float
Index
Int
CanInterface
Interface
InterfaceData
IsNil
IsValid
IsZero
Kind
Len
MapIndex
MapKeys
MapRange
Method
NumMethod
MethodByName
NumField
OverflowComplex
OverflowFloat
OverflowInt
OverflowUint
Pointer
Recv
Send
Set
SetBool
SetBytes
SetComplex
SetFloat
SetInt
SetLen
SetCap
SetMapIndex
SetUint
SetPointer
SetString
Slice
Slice3
String
TryRecv
TrySend
Type
Uint
UnsafeAddr
Convert
pointer
runes
call
recv
send
setRunes
assignTo
```

# 三、反射三定律

> 《[The Laws of Reflection](https://blog.golang.org/laws-of-reflection)》原文提到反射的三个定律

    - Reflection goes from interface value to reflection object.
    - Reflection goes from reflection object to interface value.
    - To modify a reflection object, the value must be settable.

**翻译过来就是**

1. 反射可以从接口值得到反射对象
2. 反射可以从反射对象对到接口值
3. 若要修改一个反射对象，则其值必须可修改

是不是感觉听起来很绕，我们一一解读。

## 3.1 第一定律

> 📌 反射可以从接口值得到反射对象

从接口对象获取对应的反射对象可以使用`reflect.TypeOf()`与`reflect.ValueOf()`分别获取反射的类型对象与反射的值对象。也就是第二节中的 `reflect.Type`与`reflect.Value`

![rId32.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/597473a7-d84b-4454-8635-7071f794db76/rId32.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120749Z&X-Amz-Expires=3600&X-Amz-Signature=d6483e89df7c69675d5208b5d26c1c40a8d57b222388f99a04c012d06965a5ce&X-Amz-SignedHeaders=host&x-id=GetObject)

我们看一个例子：

```go
package main

import (
	"fmt"
	"reflect"
)

type S struct {
	a int
	b float64
}

func main() {
	var x float64 = 3.4
	var y int = 100
	var s = S{
		a: 1,
		b: 90.9,
	}
	fmt.Println("type:", reflect.TypeOf(x))
	fmt.Println("value:", reflect.ValueOf(x).String())
	fmt.Println("type:", reflect.TypeOf(y))
	fmt.Println("value:", reflect.ValueOf(y).String())
	fmt.Println("type:", reflect.TypeOf(s))
	fmt.Println("value:", reflect.ValueOf(s).String())
}
// 结果
type: float64
value: <float64 Value>
type: int
value: <int Value>
type: main.S
value: <main.S Value>
```

- TypeOf()

```go
// TypeOf returns the reflection Type that represents the dynamic type of i.
// If i is a nil interface value, TypeOf returns nil.
func TypeOf(i interface{}) Type {
   eface := *(*emptyInterface)(unsafe.Pointer(&i))
   return toType(eface.typ)
}
```

- ValueOf

```go
// ValueOf returns a new Value initialized to the concrete value
// stored in the interface i. ValueOf(nil) returns the zero Value.
func ValueOf(i interface{}) Value {
	if i == nil {
		return Value{}
	}

	// TODO: Maybe allow contents of a Value to live on the stack.
	// For now we make the contents always escape to the heap. It
	// makes life easier in a few places (see chanrecv/mapassign
	// comment below).
	escapes(i)

	return unpackEface(i)
}
```

可以看到两个函数都是传递的一个空接口类型的值，参数为空接口时，可以接受任何类型。所以这里传入的类型可以任何类型的值。关于空接口的知识点不在本文中阐述。

## 3.2 第二定律

> 📌 反射可以从反射对象对到接口值

刚好和第一定律相反。

![rId34.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/b41b2ff6-574f-4700-92ef-127fd9fe10f6/rId34.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120749Z&X-Amz-Expires=3600&X-Amz-Signature=3bf2a9426073d5873da781abcfe983f159e4ce6dbbe6031b0d3726d6011d6a6a&X-Amz-SignedHeaders=host&x-id=GetObject)

```go
// Interface returns v's current value as an interface{}.
// It is equivalent to:
//	var i interface{} = (v's underlying value)
// It panics if the Value was obtained by accessing
// unexported struct fields.
func (v Value) Interface() (i interface{}) {
	return valueInterface(v, true)
}
```

Value 有方法 Interface() 支持从 reflect.Value 类型 转为 接口变量。

另外，还提供了丰富的方法来实现从 Value 到 接口对象实例的转换。

```go
func (v Value) Bool() bool
func (v Value) Float() float64
func (v Value) Int() int64
func (v Value) Uint() uint64
```

注意：Type 是不支持逆向的，因为里面只包含类型信息，所以无法逆向转化。

我们看一个例子：

```go
package main

import (
	"fmt"
	"reflect"
)

type SS struct {
	a int
	b float64
}

func main() {
	var x float64 = 3.4
	var y int = 100
	var s = SS{
		a: 1,
		b: 90.9,
	}
	v1 := reflect.ValueOf(x)
	v2 := reflect.ValueOf(y)
	v3 := reflect.ValueOf(s)

	i1 := v1.Interface()
	i2 := v2.Interface()
	i3 := v3.Interface()
	fmt.Printf("type: %T, value: %v\n", i1, i1)
	fmt.Printf("type: %T, value: %v\n", i2, i2)
	fmt.Printf("type: %T, value: %v\n", i3, i3)
}
// 结果
type: float64, value: 3.4
type: int, value: 100
type: main.SS, value: {1 90.9}
```

如果想要获取最初的类型，可以用类型断言进行转换。

```go
i3 := v3.Interface().(SS)
```

## 3.3 第三定律

> 📌 若要修改一个反射对象，则其值必须可修改

这里提到一个可修改的概念，也就是`settable`

首先，我们应该了解，在 Go 中所有的传递都是值传递。值变量传递拷贝的值，指针变量传递时的指针地址的拷贝。

Value 值在什么情况下是可以修改？我们知道接口对象传递给接口的是一个完全的值拷贝，如果调用反射方法`reflect.ValueOf()` 传进去的是一个值类型变量，则获得的 Value 实际上是原对象的一个副本，这个 Value 是无法被修改的。如果传进去的是一个指针，那么 Value 是可以修改的。

Value 值的修改涉及如下两个方法：

```go
// CanSet reports whether the value of v can be changed.
// A Value can be changed only if it is addressable and was not
// obtained by the use of unexported struct fields.
// If CanSet returns false, calling Set or any type-specific
// setter (e.g., SetBool, SetInt) will panic.
func (v Value) CanSet() bool {
	return v.flag&(flagAddr|flagRO) == flagAddr
}

// Set assigns x to the value v.
// It panics if CanSet returns false.
// As in Go, x's value must be assignable to v's type.
func (v Value) Set(x Value) {
	v.mustBeAssignable()
	x.mustBeExported() // do not let unexported x leak
	var target unsafe.Pointer
	if v.kind() == Interface {
		target = v.ptr
	}
	x = x.assignTo("reflect.Set", v.typ, target)
	if x.flag&flagIndir != 0 {
		typedmemmove(v.typ, v.ptr, x.ptr)
	} else {
		*(*unsafe.Pointer)(v.ptr) = x.ptr
	}
}
```

CanSet() 可以确定一个 Value 是否可以修改

Set() 方法用于修改 Value，另外还有其他不同类型的 Set 方法

![rId36.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/6a5851e0-62c6-41a1-a820-6ebf24ed2827/rId36.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120749Z&X-Amz-Expires=3600&X-Amz-Signature=75bc99f32d24aafbfccf0e160740f2762dda2b7f0bd0f87f9f7d952e3268fc0d&X-Amz-SignedHeaders=host&x-id=GetObject)

我们看一下例子：

```go
package main

import (
	"fmt"
	"reflect"
)
type User1 struct {
	ID int
	Name string
	Age int
}

func main() {
	u := User1{
		ID:   1,
		Name: "eachen",
		Age:  26,
	}

	va := reflect.ValueOf(u)
	vb := reflect.ValueOf(&u)
	// 值类型是不可修改的
	fmt.Println(va.CanSet(), va.FieldByName("Name").CanSet())
	// 指针类型是可修改的
	fmt.Println(vb.CanSet(), vb.Elem().FieldByName("Name").CanSet())

	fmt.Printf("%v\n\n", vb)
	name := "kuang"
	vc:= reflect.ValueOf(name)

	vb.Elem().FieldByName("Name").Set(vc)
	fmt.Printf("%v\n\n", vb)
}
// 结果
false false
false true
&{1 eachen 26}

&{1 kuang 26}
```

# 四、reflect 转化常用 API

到此，我们已经了解了反射的三大定律。我们来总结下

下图是接口对象、Type、Value 之间的转化关系以及使用到的 API：

![rId38.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/4337d391-b7f2-400b-a371-7fd20be87736/rId38.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120748Z&X-Amz-Expires=3600&X-Amz-Signature=47962ab1b5fd505f76df069094050b8b4e1f9cd0588596d8d655e12539da5e03&X-Amz-SignedHeaders=host&x-id=GetObject)

**图中提到的一些 API**：

1. 从实例到`Value`

   通过实例获取 Value 对象，直接使用 reflect.ValueOf()

   ```go
   func ValueOf(i interface{}) Value
   ```

2. 从实例到`Type`

   通过实例获取反射对象的 Type，直接使用 reflect.TypeOf()

   ```go
   func TypeOf(i interface{}) Type
   ```

3. 从`Type` 到 `Value`

   `Type` 中只有类型信息，所以直接从一个 Type 接口变量里面是无法获取实例的`Value`的，但是可以通过该`Type`构建一个新的实例的 Value。

   ```text
   // New 返回的是一个 Value，该Value 的type 为 PtrTo(typ)，即 Value 的Type是指定 typ 的指针类型
   func New(typ type) Value

   // Zero 返回的是是一个typ 类型的零值，注意返回的Value不能寻址，值不可改变
   func Zero(typ type) Value
   ```

   如果知道一个类型值的底层存放地址，则还有一个函数可以依据 type 和该地址值恢复出 Value 的。

   ```go
   func NewAt(typ Type, p unsafe.Pointer) Value
   ```

4. 从 `Value` 到 `Type`

   从反射对象 Value 到 Type 可以直接调用 Value 的方法，因为 Value 内部存放着到 Type 类型的指针。

   ```go
   func (v Value) Type() Type
   ```

5. 从`Value` 到实例

   Value 本身就包含类型和值信息，reflect 提供了丰富的方法来实现从 Value 到实例的转换

   ```go
   // Interface returns v's current value as an interface{}.
   // It is equivalent to:
   //	var i interface{} = (v's underlying value)
   // It panics if the Value was obtained by accessing
   // unexported struct fields.
   func (v Value) Interface() (i interface{}) {
   	return valueInterface(v, true)
   }

   func (v Value) Bool() bool
   func (v Value) Float() float64
   func (v Value) Int() int64
   func (v Value) Uint() uint64
   ```

6. 从`Value` 的指针到值

   从一个指针类型的 Value 获取值类型 Value 有两种方法

   ```go
   // Elem returns the value that the interface v contains
   // or that the pointer v points to.
   // It panics if v's Kind is not Interface or Ptr.
   // It returns the zero Value if v is nil.
   func (v Value) Elem() Value {
   	k := v.kind()
   	switch k {
   	case Interface:
   		var eface interface{}
   		if v.typ.NumMethod() == 0 {
   			eface = *(*interface{})(v.ptr)
   		} else {
   			eface = (interface{})(*(*interface {
   				M()
   			})(v.ptr))
   		}
   		x := unpackEface(eface)
   		if x.flag != 0 {
   			x.flag |= v.flag.ro()
   		}
   		return x
   	case Ptr:
   		ptr := v.ptr
   		if v.flag&flagIndir != 0 {
   			ptr = *(*unsafe.Pointer)(ptr)
   		}
   		// The returned value's address is v's value.
   		if ptr == nil {
   			return Value{}
   		}
   		tt := (*ptrType)(unsafe.Pointer(v.typ))
   		typ := tt.elem
   		fl := v.flag&flagRO | flagIndir | flagAddr
   		fl |= flag(typ.Kind())
   		return Value{typ, ptr, fl}
   	}
   	panic(&ValueError{"reflect.Value.Elem", v.kind()})
   }

   // Indirect returns the value that v points to.
   // If v is a nil pointer, Indirect returns a zero Value.
   // If v is not a pointer, Indirect returns v.
   func Indirect(v Value) Value {
   	if v.Kind() != Ptr {
   		return v
   	}
   	return v.Elem()
   }
   ```

7. `Type` 指针和值的相互转换

   指针类型 Type 到值类型 Type

   ```go
   	// Elem returns a type's element type.
   	// It panics if the type's Kind is not Array, Chan, Map, Ptr, or Slice.
   	Elem() Type
   ```

   值类型`Type`到指针类型`Type`

   ```go
   // PtrTo returns the pointer type with element t.
   // For example, if t represents type Foo, PtrTo(t) represents *Foo.
   func PtrTo(t Type) Type {
   	return t.(*rtype).ptrTo()
   }
   ```

# 五、深入挖掘 rtype

首先我们看下一个最通用的类型公共信息 `rtype`，它是每一种基础类型的一个成员类型。

如果有同学看过 runtime 的源码，那么可能知道，`rtype` 和 `_type` 是同一个结构体

```go
// rtype is the common implementation of most values.
// It is embedded in other struct types.
//
// rtype must be kept in sync with ../runtime/type.go:/^type._type.
type rtype struct {
	size       uintptr
	ptrdata    uintptr // number of bytes in the type that can contain pointers
	hash       uint32  // hash of type; avoids computation in hash tables
	tflag      tflag   // extra type information flags
	align      uint8   // alignment of variable with this type
	fieldAlign uint8   // alignment of struct field with this type
	kind       uint8   // enumeration for C
	// function for comparing objects of this type
	// (ptr to object A, ptr to object B) -> ==?
	equal     func(unsafe.Pointer, unsafe.Pointer) bool
	gcdata    *byte   // garbage collection data
	str       nameOff // string form
	ptrToThis typeOff // type for pointer to this type, may be zero
}
```

它实现了 `reflect.Type`接口

![rId40.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/dea38628-64dc-40fd-8d17-2efa87e3d554/2f241ec8-26ae-4425-b336-e3faac571754/rId40.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45HZZMZUHI%2F20231121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20231121T120748Z&X-Amz-Expires=3600&X-Amz-Signature=1f46a1e5057c0b6e8c95bace413c62c56a7fe77f0e52966def6456d539b6a87f&X-Amz-SignedHeaders=host&x-id=GetObject)

可以看到，其他的基本类型都有一个`rtype` 类型的成员变量

关于 Type 类型中的主要方法

1. 所有类型通用的方法：

```go
	// Name returns the type's name within its package for a defined type.
	// For other (non-defined) types it returns the empty string.
	Name() string

	// Kind returns the specific kind of this type.
	Kind() Kind

	// Implements reports whether the type implements the interface type u.
	Implements(u Type) bool

	// AssignableTo reports whether a value of the type is assignable to type u.
	AssignableTo(u Type) bool

	// ConvertibleTo reports whether a value of the type is convertible to type u.
	ConvertibleTo(u Type) bool

	// Comparable reports whether values of this type are comparable.
	Comparable() bool

	// Method returns the i'th method in the type's method set.
	// It panics if i is not in the range [0, NumMethod()).
	//
	// For a non-interface type T or *T, the returned Method's Type and Func
	// fields describe a function whose first argument is the receiver.
	//
	// For an interface type, the returned Method's Type field gives the
	// method signature, without a receiver, and the Func field is nil.
	//
	// Only exported methods are accessible and they are sorted in
	// lexicographic order.
	Method(int) Method

	// MethodByName returns the method with that name in the type's
	// method set and a boolean indicating if the method was found.
	//
	// For a non-interface type T or *T, the returned Method's Type and Func
	// fields describe a function whose first argument is the receiver.
	//
	// For an interface type, the returned Method's Type field gives the
	// method signature, without a receiver, and the Func field is nil.
	MethodByName(string) (Method, bool)

	// NumMethod returns the number of exported methods in the type's method set.
	NumMethod() int

	// PkgPath returns a defined type's package path, that is, the import path
	// that uniquely identifies the package, such as "encoding/base64".
	// If the type was predeclared (string, error) or not defined (*T, struct{},
	// []int, or A where A is an alias for a non-defined type), the package path
	// will be the empty string.
	PkgPath() string

	// Size returns the number of bytes needed to store
	// a value of the given type; it is analogous to unsafe.Sizeof.
	Size() uintptr

	// String returns a string representation of the type.
	// The string representation may use shortened package names
	// (e.g., base64 instead of "encoding/base64") and is not
	// guaranteed to be unique among types. To test for type identity,
	// compare the Types directly.
	String() string
```

1. 不同基础类型的专有方法
   - `Int*`,`Uint*`, `Float*`, `Complex*`: Bits
   - Array: Elem, Len
   - Chan: ChanDir, Elem
   - Func: In, NumIn, Out, NumOut, IsVariadic.
   - Map: Key, Elem
   - Ptr: Elem
   - Slice: Elem
   - Struct: Field, FieldByIndex, FieldByName, FieldByNameFunc, NumField

如果调用错误，那么会 panic

```go
	// Bits returns the size of the type in bits.
	// It panics if the type's Kind is not one of the
	// sized or unsized Int, Uint, Float, or Complex kinds.
	Bits() int

	// ChanDir returns a channel type's direction.
	// It panics if the type's Kind is not Chan.
	ChanDir() ChanDir

	// IsVariadic reports whether a function type's final input parameter
	// is a "..." parameter. If so, t.In(t.NumIn() - 1) returns the parameter's
	// implicit actual type []T.
	//
	// For concreteness, if t represents func(x int, y ... float64), then
	//
	//	t.NumIn() == 2
	//	t.In(0) is the reflect.Type for "int"
	//	t.In(1) is the reflect.Type for "[]float64"
	//	t.IsVariadic() == true
	//
	// IsVariadic panics if the type's Kind is not Func.
	IsVariadic() bool

	// Elem returns a type's element type.
	// It panics if the type's Kind is not Array, Chan, Map, Ptr, or Slice.
	Elem() Type

	// Field returns a struct type's i'th field.
	// It panics if the type's Kind is not Struct.
	// It panics if i is not in the range [0, NumField()).
	Field(i int) StructField

	// FieldByIndex returns the nested field corresponding
	// to the index sequence. It is equivalent to calling Field
	// successively for each index i.
	// It panics if the type's Kind is not Struct.
	FieldByIndex(index []int) StructField

	// FieldByName returns the struct field with the given name
	// and a boolean indicating if the field was found.
	FieldByName(name string) (StructField, bool)

	// FieldByNameFunc returns the struct field with a name
	// that satisfies the match function and a boolean indicating if
	// the field was found.
	//
	// FieldByNameFunc considers the fields in the struct itself
	// and then the fields in any embedded structs, in breadth first order,
	// stopping at the shallowest nesting depth containing one or more
	// fields satisfying the match function. If multiple fields at that depth
	// satisfy the match function, they cancel each other
	// and FieldByNameFunc returns no match.
	// This behavior mirrors Go's handling of name lookup in
	// structs containing embedded fields.
	FieldByNameFunc(match func(string) bool) (StructField, bool)

	// In returns the type of a function type's i'th input parameter.
	// It panics if the type's Kind is not Func.
	// It panics if i is not in the range [0, NumIn()).
	In(i int) Type

	// Key returns a map type's key type.
	// It panics if the type's Kind is not Map.
	Key() Type

	// Len returns an array type's length.
	// It panics if the type's Kind is not Array.
	Len() int

	// NumField returns a struct type's field count.
	// It panics if the type's Kind is not Struct.
	NumField() int

	// NumIn returns a function type's input parameter count.
	// It panics if the type's Kind is not Func.
	NumIn() int

	// NumOut returns a function type's output parameter count.
	// It panics if the type's Kind is not Func.
	NumOut() int

	// Out returns the type of a function type's i'th output parameter.
	// It panics if the type's Kind is not Func.
	// It panics if i is not in the range [0, NumOut()).
	Out(i int) Type
```

Go 定义了 26 个基本类型

```go
// A Kind represents the specific kind of type that a Type represents.
// The zero Kind is not a valid kind.
type Kind uint

const (
	Invalid Kind = iota
	Bool
	Int
	Int8
	Int16
	Int32
	Int64
	Uint
	Uint8
	Uint16
	Uint32
	Uint64
	Uintptr
	Float32
	Float64
	Complex64
	Complex128
	Array
	Chan
	Func
	Interface
	Map
	Ptr
	Slice
	String
	Struct
	UnsafePointer
)
```

一些常见基本类型的 Type 实现

- arrayType

```go
// arrayType represents a fixed array type.
type arrayType struct {
	rtype
	elem  *rtype // array element type
	slice *rtype // slice type
	len   uintptr
}
```

- chanType

```go
// chanType represents a channel type.
type chanType struct {
	rtype
	elem *rtype  // channel element type
	dir  uintptr // channel direction (ChanDir)
}
```

- funcType

```go
// funcType represents a function type.
//
// A *rtype for each in and out parameter is stored in an array that
// directly follows the funcType (and possibly its uncommonType). So
// a function type with one method, one input, and one output is:
//
//	struct {
//		funcType
//		uncommonType
//		[2]*rtype    // [0] is in, [1] is out
//	}
type funcType struct {
	rtype
	inCount  uint16
	outCount uint16 // top bit is set if last input parameter is ...
}
```

- interfaceType

```go
// interfaceType represents an interface type.
type interfaceType struct {
	rtype
	pkgPath name      // import path
	methods []imethod // sorted by hash
}
```

- mapType

```go
// mapType represents a map type.
type mapType struct {
	rtype
	key    *rtype // map key type
	elem   *rtype // map element (value) type
	bucket *rtype // internal bucket structure
	// function for hashing keys (ptr to key, seed) -> hash
	hasher     func(unsafe.Pointer, uintptr) uintptr
	keysize    uint8  // size of key slot
	valuesize  uint8  // size of value slot
	bucketsize uint16 // size of bucket
	flags      uint32
}
```

- ptrType

```go
// ptrType represents a pointer type.
type ptrType struct {
	rtype
	elem *rtype // pointer element (pointed at) type
}
```

- sliceType

```go
// sliceType represents a slice type.
type sliceType struct {
	rtype
	elem *rtype // slice element type
}
```

# 六、反射的优缺点

## 6.1 优点

1. **通用性**

   特别是一些类库和框架代码需要一种通用的处理模式，而不是针对每一种场景做硬编码处理，此时借助反射可以极大简化设计

2. **灵活性**

   反射提供了一种程序了解自己和改变自己的能力，这为一些测试工具的开发提供了有力的支持。

## 6.2 缺点

1. **反射是脆弱的**

   由于反射可以在程序运行时修改程序的状态，这种修改没有经过编译器的严格检查，不正确的修改很容易导致程序的崩溃

2. **反射是晦涩难懂的**

   语言的反射接口由于涉及语言的运行时，没有具体的类型系统的约束，接口的抽象级别高但实现细节复杂，导致使用反射的代码难以理解

3. **反射有部分性能损失**

   反射提供动态修改程序状态的能力，必然不是直接的地址引用，而是要借助运行时构造一个抽象层，这种间接访问会有性能的损失

## 6.3 Best Practice

1. 在**库或框架内部**使用反射，而不是把反射接口暴露给调用者，复杂性留在内部，简单性放到接口
2. **框架代码**才考虑使用反射，一般的业务代码没有抽象到反射的层次，这种过度设计会带来复杂度的提升，使得代码难以维护
3. 除非**没有其他办法**，否则**不要使用**反射技术

# 📎 参考文章

1. 《[The Laws of Reflection](https://blog.golang.org/laws-of-reflection)》
2. [src/reflect](https://golang.org/pkg/reflect/)
3. 《[Go 核心编程](https://book.jd.com/writer/Search?keyword=go%E8%AF%AD%E8%A8%80%E6%A0%B8%E5%BF%83%E7%BC%96%E7%A8%8B&enc=utf-8&spm=2.1.8)》
