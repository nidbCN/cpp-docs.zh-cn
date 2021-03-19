---
description: 了解详细信息：修饰名
title: 修饰名
ms.date: 09/05/2018
helpviewer_keywords:
- decorated names, definition
- name decoration [C++]
- names [C++], decorated
ms.assetid: a4e9ae8e-b239-4454-b401-4102793cb344
ms.openlocfilehash: 95b98bf9a9bde876cfb403351197ab2ef1b9b5b8
ms.sourcegitcommit: 92120d0c418b6d3d85d32ffc050c59c5f5f9d5b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104670775"
---
# <a name="decorated-names"></a>修饰名

C 和 C++ 程序中的函数、数据和对象均在内部由其修饰名表示。 *修饰名* 是编译器在对象、数据或函数定义的编译过程中创建的编码字符串。 它记录名称以及调用约定、类型、函数参数和其他信息。 此名称修饰（也称为 *名称重整*）有助于链接器在链接可执行文件时查找正确的函数和对象。

修饰命名约定在各种版本的 Visual Studio 中已发生更改，也可以在不同的目标体系结构上有所不同。 若要与使用 Visual Studio 创建的源文件正确链接，应使用相同的编译器工具集、标志和目标体系结构编译 C 和 c + + Dll 和库。

> [!NOTE]
> 使用 Visual Studio 2015 生成的库可由使用 Visual Studio 2017 或 Visual Studio 2019 构建的应用程序使用。

## <a name="using-decorated-names"></a><a name="Using"></a> 使用修饰名

通常情况下，无需知道修饰名也可编写已成功编译和链接的代码。 修饰名是编译器和链接器内部的实现详细信息。 通常，这些工具可以处理未修饰形式的名称。 但是，在向链接器和其他工具指定函数名时，有时则需要修饰名。 例如，若要匹配重载的 C++ 函数、命名空间成员、类构造函数、析构函数和特殊成员函数，则必须指定修饰名。 有关选项标志和其他需要修饰名的情形的详细信息，请参阅正在使用的工具和选项的相关文档。

如果你更改函数名、类、调用约定、返回类型或任何参数，则修饰名也会改变。 在这种情况下，你必须获取新的修饰名，并将其用于指定了修饰名的任何位置。

在链接到用其他编程语言编写的或使用其他编译器的代码时，名称修饰也很重要。 编译器不同，则使用名称修饰约定不同。 在可执行文件链接到用另一种语言编写的代码时，必须特别留意将导出和导入的名称与调用约定相匹配。 汇编语言代码必须使用 MSVC 修饰名称并调用约定，才能链接到使用 MSVC 编写的源代码。

## <a name="format-of-a-c-decorated-name"></a><a name="Format"></a> C + + 修饰名的格式

C + + 函数的修饰名包含以下信息：

- 函数名称。

- 函数所属的类（如果该函数为成员函数）。 这可能包括含有函数所属的类的类，等等。

- 函数所属的命名空间（如果该函数是命名空间的一部分）。

- 函数参数的类型。

- 调用约定。

- 函数的返回类型。

修饰名中编码有函数名和类名。 修饰名的其余部分是具有仅用于编译器和链接器的内部意义的代码。 以下示例介绍的是未修饰和修饰的 C++ 名称。

|未修饰名|修饰名|
|----------------------|--------------------|
|`int a(char){int i=3;return i;};`|`?a@@YAHD@Z`|
|`void __stdcall b::c(float){};`|`?c@b@@AAGXM@Z`|

## <a name="format-of-a-c-decorated-name"></a><a name="FormatC"></a> C 修饰名的格式

C 函数的修饰形式取决于其声明中使用的调用约定，如下表所示。 这也是在将 C++ 代码声明为具有 `extern "C"` 链接时使用的修饰格式。 默认调用约定是 **`__cdecl`** 。 请注意，在 64 位环境中，不修饰函数。

|调用约定|字体效果|
|------------------------|----------------|
|**`__cdecl`**|前导下划线 (**`_`**) |
|**`__stdcall`**|前导下划线 (**`_`**) 和尾随 at 符号 (**`@`**) 后跟参数列表中的字节数（以十进制为单位）|
|**`__fastcall`**|前导和尾随 at 符号 (**`@`**) 后跟一个表示参数列表中的字节数的十进制数|
|**`__vectorcall`**|两个尾随 at 符号 (**`@@`**) 后接参数列表中的十进制字节数|

## <a name="viewing-decorated-names"></a><a name="Viewing"></a> 查看修饰名

在编译包含数据、对象或者函数定义或原型的源文件之后，可以获取符号名称的修饰形式。 若要检查你的程序中的修饰名，可以使用以下方法之一：

#### <a name="to-use-a-listing-to-view-decorated-names"></a>若要使用列表查看修饰名

1. 通过编译包含数据、对象或者函数定义或原型的源文件，并将 [列表文件类型](fa-fa-listing-file.md) 编译器选项设置为带有源代码 (**/FAs**) 的程序集，从而生成列表。

   例如， `cl /c /FAs example.cpp` 在开发人员命令提示符下输入以生成列表文件，例如 .asm。

2. 在生成的列表文件中，找到以 PUBLIC 开头、分号结尾且后接未修饰的数据或函数名的行。 PUBLIC 和分号之间的符号即为修饰名。

#### <a name="to-use-dumpbin-to-view-decorated-names"></a>若要使用 DUMPBIN 查看修饰名

1. 若要查看 .obj 或 .lib 文件中的导出符号，请 `dumpbin /exports <obj-or-lib-file>` 在开发人员命令提示符下输入。

2. 若要查找符号的修饰形式，请查找括号中的未修饰名。 修饰名在同一行上，在未修饰名称之前。

## <a name="viewing-undecorated-names"></a><a name="Undecorated"></a> 查看未修饰名称

可以使用 undname.exe 将修饰名转换为其未修饰形式。 此示例演示其工作原理：

```
C:\>undname ?func1@a@@AAEXH@Z
Microsoft (R) C++ Name Undecorator
Copyright (C) Microsoft Corporation. All rights reserved.

Undecoration of :- "?func1@a@@AAEXH@Z"
is :- "private: void __thiscall a::func1(int)"
```

## <a name="see-also"></a>请参阅

[其他 MSVC 生成工具](c-cpp-build-tools.md)<br/>
[使用 extern 指定链接](../../cpp/extern-cpp.md)
