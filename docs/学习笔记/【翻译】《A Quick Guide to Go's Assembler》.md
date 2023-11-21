---
password: ""
icon: ""
date: "2021-10-02"
type: Post
category: 学习笔记
slug: quick-guide-to-ga-asm
tags:
  - Go
  - 开发
summary: "快速学习Go汇编，本文翻译《A Quick Guide to Go's Assembler》 https://golang.org/doc/asm"
title: 【翻译】《A Quick Guide to Go's Assembler》
status: Published
cover: "https://images.unsplash.com/photo-1506918836024-2d1ded7ce188?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb"
urlname: 4212a457-2728-4e14-bac7-95602e9848ba
updated: "2023-08-10 15:23:00"
---

> 😀 本文翻译**《A Quick Guide to Go's Assembler》** [**https://golang.org/doc/asm**](https://golang.org/doc/asm)

## **A Quick Guide to Go's Assembler**

本文档是 gc Go 编译器使用的汇编语言的不寻常形式的快速概述。这份文件并不全面。

汇编程序是基于 [**Plan 9 汇编程序**](https://9p.io/sys/doc/asm.html)的输入样式。如果您计划编写汇编语言，您应该阅读该文档，尽管其中大部分是特定于 Plan 9 的。当前文档提供了语法摘要以及与该文档中所解释内容的差异，并描述了编写汇编代码以与 Go 交互时所适用的特性。

**关于 Go 的汇编程序，最重要的事情，它不是底层机器的直接表示**。有些细节精确地映射到机器上，但有些则不然。这是因为编译器套件（见此[**描述**](https://9p.io/sys/doc/compiler.html)）在常规管道中不需要汇编程序。相反，编译器工作在一种半抽象的指令集上，指令选择（instruction selection）部分发生在代码生成之后。汇编程序以半抽象形式上工作，因此当您看到诸如 MOV 这样的指令时，工具链实际为该操作生成的可能根本不是移动指令，可能是清除指令或加载指令。或者它可能恰好与指令的名字一致。通常，特定于机器的操作倾向于自己出现，而更通用的概念（如内存移动和子程序调用和返回）则更抽象。细节因架构而异，我们为不精确而道歉; 情况没有很好地定义。

汇编程序是一种解析半抽象指令集的描述并将其转换为输入到链接器的指令的方法。如果您想了解给定体系结构的汇编指令（比如 amd64）是什么样子的，那么在标准库的源代码中有许多示例，包括 [**runtime**](https://golang.org/pkg/runtime/) 和 [**math/big**](notion://www.notion.so/eachenkuang/math/big)。您还可以检查编译器发出的汇编代码(实际的输出可能与您在这里看到的不同) :

```go
 $ cat hello.go
 package main
 
 func main() {
      println(3)
 }
 $ GOOS=darwin GOARCH=amd64 go tool compile -S hello.go        # or: go build -gcflags -S hello.go
 "".main STEXT size=77 args=0x0 locals=0x10
         0x0000 00000 (hello.go:3)       TEXT    "".main(SB), ABIInternal, $16-0
         0x0000 00000 (hello.go:3)       MOVQ    (TLS), CX
         0x0009 00009 (hello.go:3)       CMPQ    SP, 16(CX)
         0x000d 00013 (hello.go:3)       PCDATA  $0, $-2
         0x000d 00013 (hello.go:3)       JLS     70
         0x000f 00015 (hello.go:3)       PCDATA  $0, $-1
         0x000f 00015 (hello.go:3)       SUBQ    $16, SP
         0x0013 00019 (hello.go:3)       MOVQ    BP, 8(SP)
         0x0018 00024 (hello.go:3)       LEAQ    8(SP), BP
         0x001d 00029 (hello.go:3)       FUNCDATA        $0, gclocals·33cdeccccebe80329f1fdbee7f5874cb(SB)
         0x001d 00029 (hello.go:3)       FUNCDATA        $1, gclocals·33cdeccccebe80329f1fdbee7f5874cb(SB)
         0x001d 00029 (hello.go:4)       PCDATA  $1, $0
         0x001d 00029 (hello.go:4)       NOP
         0x0020 00032 (hello.go:4)       CALL    runtime.printlock(SB)
         0x0025 00037 (hello.go:4)       MOVQ    $3, (SP)
         0x002d 00045 (hello.go:4)       CALL    runtime.printint(SB)
         0x0032 00050 (hello.go:4)       CALL    runtime.printnl(SB)
         0x0037 00055 (hello.go:4)       CALL    runtime.printunlock(SB)
         0x003c 00060 (hello.go:5)       MOVQ    8(SP), BP
         0x0041 00065 (hello.go:5)       ADDQ    $16, SP
         0x0045 00069 (hello.go:5)       RET
         0x0046 00070 (hello.go:5)       NOP
         0x0046 00070 (hello.go:3)       PCDATA  $1, $-1
         0x0046 00070 (hello.go:3)       PCDATA  $0, $-2
         0x0046 00070 (hello.go:3)       CALL    runtime.morestack_noctxt(SB)
         0x004b 00075 (hello.go:3)       PCDATA  $0, $-1
         0x004b 00075 (hello.go:3)       JMP     0
         0x0000 65 48 8b 0c 25 00 00 00 00 48 3b 61 10 76 37 48  eH..%....H;a.v7H
         0x0010 83 ec 10 48 89 6c 24 08 48 8d 6c 24 08 0f 1f 00  ...H.l$.H.l$....
         0x0020 e8 00 00 00 00 48 c7 04 24 03 00 00 00 e8 00 00  .....H..$.......
         0x0030 00 00 e8 00 00 00 00 e8 00 00 00 00 48 8b 6c 24  ............H.l$
         0x0040 08 48 83 c4 10 c3 e8 00 00 00 00 eb b3           .H...........
```

`FUNCDATA`和`PCDATA`伪指令包含供垃圾收集器使用的信息。它们由编译器引入。

要查看链接后的二进制文件中放入了什么，可以使用 `go tool objdump`:

```go
 ➜  GoGoGo git:(master) ✗ go build -o x.exe test/asm/hello.go
 ➜  GoGoGo git:(master) ✗ go tool objdump -s main.main x.exe
 TEXT main.main(SB) /Users/eachen/Repository/MySelf/GoGoGo/test/asm/hello.go
   hello.go:3            0x105c8c0               65488b0c2530000000      MOVQ GS:0x30, CX
   hello.go:3            0x105c8c9               483b6110                CMPQ 0x10(CX), SP
   hello.go:3            0x105c8cd               7637                    JBE 0x105c906
   hello.go:3            0x105c8cf               4883ec10                SUBQ $0x10, SP
   hello.go:3            0x105c8d3               48896c2408              MOVQ BP, 0x8(SP)
   hello.go:3            0x105c8d8               488d6c2408              LEAQ 0x8(SP), BP
   hello.go:4            0x105c8dd               0f1f00                  NOPL 0(AX)
   hello.go:4            0x105c8e0               e89b15fdff              CALL runtime.printlock(SB)
   hello.go:4            0x105c8e5               48c7042403000000        MOVQ $0x3, 0(SP)
   hello.go:4            0x105c8ed               e8ae1dfdff              CALL runtime.printint(SB)
   hello.go:4            0x105c8f2               e84918fdff              CALL runtime.printnl(SB)
   hello.go:4            0x105c8f7               e80416fdff              CALL runtime.printunlock(SB)
   hello.go:5            0x105c8fc               488b6c2408              MOVQ 0x8(SP), BP
   hello.go:5            0x105c901               4883c410                ADDQ $0x10, SP
   hello.go:5            0x105c905               c3                      RET
   hello.go:3            0x105c906               e855b2ffff              CALL runtime.morestack_noctxt(SB)
   hello.go:3            0x105c90b               ebb3                    JMP main.main(SB)
```

### **Constants 常量**

尽管汇编程序从 Plan 9 汇编程序获得指导，但这是一个独立的程序，因此存在一些差异。一种是不断的评估。汇编程序中的常量表达式使用 Go 的运算符优先级进行解析，而不是原始的类似 C 的优先级。因此，3 & 1 < 2 的结果是 4，而不是 0。它解析为(3 & 1) < 2，而不是 3 & (1 < 2)。此外，常量始终作为 64 位无符号整数进行计算。因此 -2 不是整数值负二，而是具有相同位模式的无符号 64 位整数。这种区分很少有影响，但为了避免模糊性、除法或右移，右操作数的高位被设置为拒绝。

### **Symbols 符号**

有些符号，如 R1 或 LR，是预先定义的，并引用寄存器。确切的符号集取决于架构。

有四个预先声明的符号引用伪寄存器。这些不是真正的寄存器，而是由工具链维护的虚拟寄存器，例如帧指针。伪寄存器的集合对于所有体系结构都是相同的：

- `FP`(Frame pointer): 框架指针: 参数和局部变量
- `PC`(Program counter): 程序计数器: 跳跃和分支
- `SB`(Static base pointer): 静态基准指针: 全局符号
- `SP`(Stack pointer): 堆栈指针: 堆栈顶部

所有用户定义的符号都作为偏移量写入伪寄存器 FP（参数和局部变量）和 SB（全局变量）。

`SB 伪寄存器`可以被认为是内存的起源，所以符号 `foo (SB)`是作为内存中地址的名称 `foo`。这种表达方式用于命名全局函数和数据。在名称中添加 `< >` ，就像在 `foo < > (SB)`中一样，使得名称只在当前源文件中可见，就像 c 文件中的顶级静态声明。向名称添加偏移量指的是与符号地址的偏移量，因此 foo + 4(SB)是 foo 开始之后的四个字节。

`FP 伪寄存器`是一个虚拟帧指针，用于引用函数参数。编译器维护一个虚拟帧指针，并将堆栈上的参数作为伪寄存器的偏移量引用。因此，`0(FP)`是函数的第一个参数，`8(FP)`是第二个参数(在 64 位机器上) ，依此类推。但是，当以这种方式引用函数参数时，有必要在开头放置一个名称，如 `first _ arg + 0(FP)`和 `second _ arg + 8(FP)`。(偏移量的含义——从帧指针开始的的偏移量——不同于它与 SB 的使用，SB 是与符号的偏移量。)汇编程序强制执行这个约定，拒绝普通的`0(FP)`和`8(FP)`。实际名称在语义上是不相关的，但应该用于记录参数的名称。值得强调的是，即使在具有硬件帧指针的体系结构上，FP 始终是伪寄存器，而不是硬件寄存器。

对于具有 Go 原型的汇编函数，`go vet`将检查参数名称和偏移量是否匹配。在 32 位系统上，通过在名称中添加`_lo`或`_hi`后缀来区分 64 位值的低 32 位和高 32 位，如`arg_lo + 0(FP)`或`arg_hi + 4(FP)`一样。如果 Go 原型未命名其结果，则预期的程序集名称为`ret`。

`SP 伪寄存器`是一个虚拟堆栈指针，用于引用帧局部变量和为函数调用准备的参数。它指向本地堆栈帧的顶部，因此引用应该在`[-framesize，0)` x-8(SP) ，y-4(SP)等范围内使用负偏移量。

在具有名为 SP 的硬件寄存器的体系结构中，名称前缀将对虚拟堆栈指针的引用与对体系结构 SP 寄存器的引用区分开来。也就是说，`x-8(SP)`和`-8(SP)`是不同的内存位置: 前者指向虚拟堆栈指针伪寄存器，后者指向硬件的 SP 寄存器。

在传统上 SP 和 PC 是物理的、编号的寄存器的别名的机器上，在 Go 汇编程序中，SP 和 PC 的名称仍然被特殊处理; 例如，对 SP 的引用需要一个符号，就像 FP。要访问实际的硬件寄存器，请使用真正的 R 名称。例如，在 ARM 体系结构中，SP 和 PC 的硬件可以通过 R13 和 R15 访问。

分支和直接跳转始终被写入 PC 的偏移量或标签的跳转：

```text
 label:
      MOVW $0, R1
      JMP label
```

每个标签仅在定义它的函数中可见。因此，允许文件中的多个函数定义和使用相同的标签名称。直接跳转和调用指令可以针对文本符号(如`name(SB)`) ，但不能偏移符号(如`name+4(SB)`)。

指令、寄存器和汇编程序指令总是使用大写字母，以提醒您汇编编程是一件令人担忧的工作。(例外: ARM 上的 g 寄存器重命名。)

在 Go 对象文件和二进制文件中，符号的完整名称是包路径，后跟句点和符号名：`fmt.Printf`或`math / rand.Int`。因为汇编程序的解析器将句号和斜杠视为标点符号，所以这些字符串不能直接用作标识符名称。相反，汇编程序允许在标识符中使用中间点字符 `U+00B7`和除法斜杠 `U+2215`，并将它们重写为纯句号和斜杠。在汇编程序源文件中，上面的符号被写成 `fmt·Printf` and `math∕rand·Int`. 。编译器在使用`-s` 标志时生成的程序集清单直接显示句点和斜杠，而不是编译器所需的 Unicode 替换。

大多数手写的汇编文件都没有在符号名称中包含完整的程序包路径，因为链接器会在以句点开头的任何名称的开头插入当前目标文件的程序包路径：在`math/rand`包实现的的汇编源文件中，包的`Int`函数可以称为`·Int`。此约定避免了在其自身的源代码中对包的导入路径进行硬编码的需要，从而使将代码从一个位置移动到另一个位置变得更加容易。

### **Directives 指令**

汇编程序使用各种指令将文本和数据绑定到符号名。例如，下面是一个简单的完整函数定义。`TEXT` 指令声明符号`runtime·profileloop`和函数体后面的指令。`TEXT` 块中的最后一条指令必须是某种类型的跳转，通常是 RET (伪)指令。(如果不是，链接器将追加一条跳转到自身的指令; 在`TEXT` 不存在 `fallthrough`。）在符号之后，参数是标志(见下文)和帧大小(一个常量) :

```text
 TEXT runtime·profileloop(SB),NOSPLIT,$8
      MOVQ $runtime·profileloop1(SB), CX
      MOVQ CX, 0(SP)
      CALL runtime·externalthreadhandler(SB)
      RET
```

在一般情况下，帧大小后跟一个参数大小，以减号分隔。（这不是减法，只是特有的语法。）帧大小`$24-8`声明该函数具有 24 字节的帧，并使用 8 字节的参数调用，该参数驻留在调用者的帧上。如果未为`TEXT`指定`NOSPLIT`，则必须提供参数大小。对于带有 Go 原型的汇编函数，`go vet`将检查参数大小是否正确。

注意，符号名使用一个中间点来分隔组件，并被指定为静态基准伪寄存器 SB 的偏移量。这个函数将使用简单的名称 `profileoop` 从包`runtime`的 Go source 中调用。

全局数据符号是由一系列初始化 DATA 指令后跟一个 GLOBL 指令定义的。每个 `DATA` 指令初始化相应内存的一部分。未显式初始化的内存已归零。DATA 指令的一般形式是

```text
 DATA symbol+offset(SB)/width, value
```

它使用给定的值在给定的偏移量和宽度初始化符号内存。给定符号的 DATA 指令必须使用增加的偏移量来写入。

`GLOBL` 指令将一个符号声明为全局符号。参数是可选的标志，数据的大小声明为全局的，除非 DATA 指令初始化了它，否则全局的初始值都是零。`GLOBL` 指令必须遵循任何相应的 DATA 指令。

```text
 DATA divtab<>+0x00(SB)/4, $0xf4f8fcff
 DATA divtab<>+0x04(SB)/4, $0xe6eaedf0
 ...
 DATA divtab<>+0x3c(SB)/4, $0x81828384
 GLOBL divtab<>(SB), RODATA, $64
 
 GLOBL runtime·tlsoffset(SB), NOPTR, $4
```

声明并初始化 divtab <>（4 字节整数值的只读 64 字节表），并声明 runtime_tlsoffset（4 字节隐式零变量，不包含指针）。

这些指令可能有一两个参数。如果有两个，第一个是一个标志的位掩码，它可以写成数字表达式，添加或者叠加在一起，或者可以象征性地设置，以便人类更容易地理解。在标准 `# include file textflag.h` 中定义的值是:

- `NOPROF` = 1
  (For (适用于`TEXT` items.) Don't profile the marked function. This flag is deprecated. )不要分析标记的函数。此标志不推荐使用
- `DUPOK` = 2
  It is legal to have multiple instances of this symbol in a single binary. The linker will choose one of the duplicates to use. 在一个二进制文件中包含这个符号的多个实例是合法的。链接器将选择要使用的副本之一
- `NOSPLIT` = 4
  (For (适用于`TEXT` items.) Don't insert the preamble to check if the stack must be split. The frame for the routine, plus anything it calls, must fit in the spare space at the top of the stack segment. Used to protect routines such as the stack splitting code itself. 项目)不要插入序言以检查堆栈是否必须拆分。例程的框架以及它调用的任何内容都必须适合堆栈段顶部的空闲空间。用于保护例程，如堆栈分割代码本身
- `RODATA` = 8
  (For (适用于`DATA` and 及`GLOBL` items.) Put this data in a read-only section. )将此数据放入只读部分
- `NOPTR` = 16
  (For (适用于`DATA` and 及`GLOBL` items.) This data contains no pointers and therefore does not need to be scanned by the garbage collector. )此数据不包含指针，因此不需要由垃圾收集器扫描
- `WRAPPER` = 32
  (For (适用于`TEXT` items.) This is a wrapper function and should not count as disabling 这是一个包装函式，不应该被视为禁用`recover`.
- `NEEDCTXT` = 64
  (For (适用于`TEXT` items.) This function is a closure so it uses its incoming context register. )这个函数是一个闭包，所以它使用传入的上下文寄存器
- `LOCAL` = 128
  This symbol is local to the dynamic shared object. 此符号是动态共享对象的本地符号
- `TLSBSS` = 256
  (For (适用于`DATA` and 及`GLOBL` items.) Put this data in thread local storage. )将此数据放入线程本地存储中
- `NOFRAME` = 512
  (For (适用于`TEXT` items.) Do not insert instructions to allocate a stack frame and save/restore the return address, even if this is not a leaf function. Only valid on functions that declare a frame size of 0. 项目)不要插入指令来分配堆栈帧和保存/恢复返回地址，即使这不是一个叶函数。只对声明帧大小为 0 的函数有效
- `TOPFRAME` = 2048
  (For (适用于`TEXT` items.) Function is the top of the call stack. Traceback should stop at this function. 函数是调用堆栈的顶部。回溯应该在这个函数处停止

### **Interacting with Go types and constants 与 Go 类型和常量进行交互**

如果一个包有任何 .s 文件，那么 go build 将指示编译器发出一个名为 go_asm.h 的特殊头文件，然后 .s 文件可以#include。 该文件包含用于 Go 结构字段偏移量的符号 #define 常量、Go 结构类型的大小以及当前包中定义的大多数 Go const 声明。 Go 程序集应该避免对 Go 类型的布局进行假设，而是使用这些常量。 这提高了汇编代码的可读性，并使其对 Go 类型定义或 Go 编译器使用的布局规则中的数据布局变化保持稳健。

常量的形式是 `const_name`。 例如，给定 Go 声明 `const bufSize = 1024`，汇编代码可以将此常量的值称为 `const_bufSize`。

字段偏移的形式为 `type_field`。 结构大小的形式为`type__size`。 例如，考虑以下 Go 定义：

```go
 type reader struct {
      buf [bufSize]byte
      r   int
 }
```

汇编可以将这个结构体的大小称为 reader\_\_size，将两个字段的偏移量称为 reader_buf 和 reader_r。 因此，如果寄存器 R1 包含一个指向读取器的指针，程序集可以将 r 字段引用为 reader_r(R1)。

如果这些 `#define` 名称中的任何一个不明确（例如，具有 \_size 字段的结构），`#include "go_asm.h"` 将失败并出现 `"redefinition of macro"` 错误。

### **Runtime Coordination 运行时协调**

为了正确运行垃圾回收，运行时必须知道指针在所有全局数据和大多数堆栈帧中的位置。 Go 编译器在编译 Go 源文件时会发出此信息，但汇编程序必须明确定义它。

标有 `NOPTR` 标志（见上文）的数据符号被视为不包含指向运行时分配数据的指针。带有 `RODATA`标志的数据符号被分配在只读存储器中，因此被视为隐式标记的 NOPTR。总大小小于指针的数据符号也被视为隐式标记的 NOPTR。不能在汇编源文件中定义包含指针的符号；这样的符号必须在 Go 源文件中定义。即使没有 `DATA` 和 `GLOBL` 指令，汇编源仍然可以按名称引用符号。一个好的通用经验法则是在 Go 中而不是在汇编中定义所有非 `RODATA` 符号。

每个函数还需要注释，在其参数、结果和本地堆栈帧中给出实时指针的位置。对于没有指针结果和没有本地堆栈帧或没有函数调用的汇编函数，唯一的要求是在同一个包中的 Go 源文件中为该函数定义一个 Go 原型。汇编函数的名称不得包含包名称组件（例如，包 `syscall` 中的函数 `Syscall` 应在其 `TEXT` 指令中使用名称·Syscall 而不是等效名称 syscall·Syscall）。对于更复杂的情况，需要显式注释。这些注释使用标准`#include` 文件`funcdata.h` 中定义的伪指令。

如果函数没有参数和结果，则可以省略指针信息。这由 `TEXT` 指令上的 `$n-0` 参数大小注释指示。否则，指针信息必须由 Go 源文件中函数的 Go 原型提供，即使对于不是直接从 Go 调用的汇编函数也是如此。 （原型还将让`go` `vet`检查参数引用。）在函数开始时，假定参数已初始化，但假定结果未初始化。如果结果将在调用指令期间保存实时指针，则该函数应首先将结果归零，然后执行伪指令 `GO_RESULTS_INITIALIZED`。该指令记录了结果现在已经初始化并且应该在堆栈移动和垃圾收集期间进行扫描。通常更容易安排汇编函数不返回指针或不包含调用指令；标准库中没有汇编函数使用 `GO_RESULTS_INITIALIZED`。

如果函数没有本地堆栈帧，则可以省略指针信息。这由 TEXT 指令上的 `$0-n` 本地帧大小注释指示。如果函数不包含调用指令，也可以省略指针信息。否则，本地堆栈帧不得包含指针，程序集必须通过执行伪指令 `NO_LOCAL_POINTERS` 来确认这一事实。由于堆栈大小调整是通过移动堆栈来实现的，因此在任何函数调用期间堆栈指针都可能发生变化：即使是指向堆栈数据的指针也不能保存在局部变量中。

应始终为汇编函数提供 Go 原型，以便为参数和结果提供指针信息，并检查用于访问它们的偏移量是否正确。

## **特定架构的细节**

列出每台机器的所有说明和其他详细信息是不切实际的。 要查看为给定机器（例如 ARM）定义了哪些指令，请查看该架构的 obj 支持库的源代码，该库位于目录 src/cmd/internal/obj/arm 中。 在那个目录中有一个文件 a.out.go; 它包含一长串以 A 开头的常量，如下所示：

```text
 const (
      AAND = obj.ABaseARM + obj.A_ARCHSPECIFIC + iota
      AEOR
      ASUB
      ARSB
      AADD
      AADC
      ASBC
      ARSC
      ATST
      ATEQ
      ACMP
      ACMN
      AORR
      ABIC
 
      AMVN
```

这是该体系结构的汇编器和链接器已知的指令列表及其拼写。在此列表中，每条指令都以首字母大写 A 开头，因此 AAND 表示按位和指令 AND（没有前导 A），并在汇编源代码中编写为 AND。枚举主要按字母顺序排列。 （在 cmd/internal/obj 包中定义的与体系结构无关的 AXXX 表示无效指令）。 A 名称的顺序与机器指令的实际编码无关。 cmd/internal/obj 包负责处理这些细节。

cmd/internal/obj/x86/a.out.go 中列出了 386 和 AMD64 架构的指令。

这些架构共享常见寻址模式的语法，例如 (R1)（间接寄存器）、4(R1)（带偏移量的间接寄存器）和 $foo(SB)（绝对地址）。汇编器还支持某些（不一定是全部）特定于每个体系结构的寻址模式。下面的部分列出了这些。

前面部分示例中的一个明显细节是指令中的数据从左到右流动：MOVQ $0，CX 清除 CX。该规则甚至适用于传统符号使用相反方向的体系结构。

下面是对受支持架构的关键 Go 特定细节的一些描述。

### **32-bit Intel 386**

指向 g 结构的运行时指针通过 MMU 中未使用的（就 Go 而言）寄存器的值来维护。 在 runtime 包中，汇编代码可以包含 go_tls.h，它定义了一个依赖于操作系统和体系结构的宏 get_tls 来访问这个寄存器。 get_tls 宏接受一个参数，即加载 g 指针的寄存器。

例如，使用 CX 加载 g 和 m 的顺序如下所示：

```text
 #include "go_tls.h"
 #include "go_asm.h"
 ...
 get_tls(CX)
 MOVL g(CX), AX     // Move g into AX.
 MOVL g_m(AX), BX   // Move g.m into BX.
```

get_tls 宏也在 amd64 上定义。

**寻址模式：**

- `(DI)(BX*2)`：地址 DI 加上 BX2 的位置。
- `64(DI)(BX*2)`：地址 DI 加上 BX2 加上 64 的位置。这些模式只接受 1、2、4 和 8 作为比例因子。

当使用编译器和汇编器的 `-dynlink` 或 `-shared` 模式时，任何固定内存位置（例如全局变量）的加载或存储都必须假定覆盖 CX。 因此，为了安全使用这些模式，汇编源通常应避免 CX，除非在内存引用之间。

### **64-bit Intel 386 (又名 amd64)**

这两种体系结构在汇编程序级别的行为大致相同。 在 64 位版本上访问 m 和 g 指针的汇编代码与在 32 位 386 上相同，除了它使用 MOVQ 而不是 MOVL：

```text
 get_tls(CX)
 MOVQ g(CX), AX     // Move g into AX.
 MOVQ g_m(AX), BX   // Move g.m into BX.
```

注册 BP 是被调用者保存的。 当帧大小大于零时，汇编器会自动插入 BP 保存/恢复。 允许使用 BP 作为通用寄存器，但它会干扰基于采样的分析。

### **ARM**

寄存器 R10 和 R11 由编译器和链接器保留。

R10 指向 g（goroutine）结构。在汇编源代码中，这个指针必须被称为 g；名称 R10 无法识别。

为了让人们和编译器更容易编写汇编，ARM 链接器允许使用单个硬件指令可能无法表达的通用寻址形式和伪操作，如 DIV 或 MOD。它将这些形式实现为多条指令，通常使用 R11 寄存器来保存临时值。手写汇编可以使用 R11，但这样做需要确保链接器不会同时使用它来实现函数中的任何其他指令。

定义 TEXT 时，指定帧大小 `$-4` 告诉链接器这是一个叶函数，不需要在入口处保存 LR。

名称 SP 总是指前面描述的虚拟堆栈指针。对于硬件寄存器，使用 R13。

条件代码语法是在指令后附加一个句点和一个或两个字母的代码，如 MOVW.EQ。可以附加多个代码：MOVM.IA.W。代码修饰符的顺序无关紧要。

**寻址模式：**

- R0->16
  R0>>16
  R0<<16
  R0@>16：对于<<，R0 左移 16 位。其他代码是 ->（算术右移）、>>（逻辑右移）和 @>（右移）。
- R0->R1
  R0>>R1
  R0<<R1
  R0@>R1：对于<<，将 R0 左移 R1 中的计数。其他代码是 ->（算术右移）、>>（逻辑右移）和 @>（右移）。
- [R0,g,R12-R15]：对于多寄存器指令，包括 R0、g 和 R12 到 R15 的集合。
- (R5, R6)：目标寄存器对。

### **ARM64**

ARM64 端口处于实验状态。

R18 是“平台寄存器”，保留在苹果平台上。 为防止意外误用，该寄存器命名为 R18_PLATFORM。 R27 和 R28 由编译器和链接器保留。 R29 是帧指针。 R30 是链接寄存器。

指令修饰符在句点后附加到指令。 唯一的修饰符是 P（后增量）和 W（前增量）：MOVW.P, MOVW.W

**寻址模式：**

- R0->16
  R0>>16
  R0<<16
  R0@>16：这些与 32 位 ARM 上的相同。
- $(8<<12)：立即数 8 左移 12 位。
- 8(R0)：将 R0 和 8 的值相加。
- (R2)(R0)：R0 加 R2 处的位置。
- R0.UXTB
  R0.UXTB<<imm：UXTB：从 R0 的低位中提取一个 8 位的值并零扩展到 R0 的大小。 R0.UXTB<<imm：将 R0.UXTB 的结果左移 imm 位。 imm 值可以是 0、1、2、3 或 4。其他扩展包括 UXTH（16 位）、UXTW（32 位）和 UXTX（64 位）。
- R0.SXTB
  R0.SXTB<<imm：SXTB：从 R0 的低位中提取一个 8 位的值，符号扩展到 R0 的大小。 R0.SXTB<<imm：将 R0.SXTB 的结果左移 imm 位。 imm 值可以是 0、1、2、3 或 4。其他扩展名包括 SXTH（16 位）、SXTW（32 位）和 SXTX（64 位）。
- (R5, R6)：LDAXP/LDP/LDXP/STLXP/STP/STP 的寄存器对。

详情参考：[**Go ARM64 Assembly Instructions Reference Manual**](https://golang.org/pkg/cmd/internal/obj/arm64)

### **PPC64**

该汇编器由 GOARCH 值 ppc64 和 ppc64le 使用。

详情参考：[**Go PPC64 Assembly Instructions Reference Manual**](https://golang.org/pkg/cmd/internal/obj/ppc64)

### **IBM z/Architecture，（又名 s390x）**

寄存器 R10 和 R11 被保留。汇编器在汇编某些指令时使用它们来保存临时值。

R13 指向 g（goroutine）结构。这个寄存器必须被称为 g；名称 R13 无法识别。

R15 指向堆栈帧，通常只能使用虚拟寄存器 SP 和 FP 访问。

加载和存储多个指令对一系列寄存器进行操作。寄存器的范围由开始寄存器和结束寄存器指定。例如，LMG (R9)、R5、R7 将分别使用 0(R9)、8(R9) 和 16(R9) 处的 64 位值加载 R5、R6 和 R7。

MVC 和 XC 等存储和存储指令都是以长度作为第一个参数编写的。例如，XC $8, (R9), (R9) 将清除 R9 中指定地址处的八个字节。

如果向量指令将长度或索引作为参数，那么它将是第一个参数。例如，VLEIF 1, 16, V2 会将值 16 加载到 V2 的索引一中。使用向量指令时应注意确保它们在运行时可用。要使用向量指令，机器必须同时具有向量工具（工具列表中的第 129 位）和内核支持。如果没有内核支持，向量指令将无效（它相当于一条 NOP 指令）。

**寻址模式：**

- (R5)(R6\*1)：R5 加 R6 处的位置。它是 x86 上的缩放模式，但唯一允许的缩放比例是 1。

### **MIPS、MIPS64**

通用寄存器命名为 R0 到 R31，浮点寄存器命名为 F0 到 F31。

R30 保留指向 g。 R23 用作临时寄存器。

在 TEXT 指令中，MIPS 的帧大小`$-4` 或 MIPS64 的 `$-8` ，指示链接器不保存 LR。

SP 指的是虚拟堆栈指针。对于硬件寄存器，使用 R29。

**寻址模式：**

- 16(R1)：R1 处的位置加 16。
- (R1)：0(R1) 的别名。

通过预定义 GOMIPS_hardfloat 或 GOMIPS_softfloat，GOMIPS 环境变量（hardfloat 或 softfloat）的值可用于汇编代码。

### **不支持的操作码**

汇编器旨在支持编译器，因此并非所有硬件指令都针对所有体系结构定义：如果编译器不生成它，则它可能不存在。如果您需要使用缺失的指令，有两种方法可以继续。一种是更新汇编器以支持该指令，这很简单，但只有在该指令可能会再次使用时才值得。相反，对于简单的一次性情况，可以使用 BYTE 和 WORD 指令将显式数据放置到 TEXT 内的指令流中。以下是 386 运行时定义 64 位原子加载函数的方式。

```text
 // uint64 atomicload64(uint64 volatile* addr);
 // so actually
 // void atomicload64(uint64 *res, uint64 volatile *addr);
 TEXT runtime·atomicload64(SB), NOSPLIT, $0-12
      MOVL ptr+0(FP), AX
      TESTL     $7, AX
      JZ   2(PC)
      MOVL 0, AX // crash with nil ptr deref
      LEAL ret_lo+4(FP), BX
      // MOVQ (%EAX), %MM0
      BYTE $0x0f; BYTE $0x6f; BYTE $0x00
      // MOVQ %MM0, 0(%EBX)
      BYTE $0x0f; BYTE $0x7f; BYTE $0x03
      // EMMS
      BYTE $0x0F; BYTE $0x77
      RET
```
