---
description: 了解详细信息： `__declspec`
title: __declspec
ms.date: 03/21/2019
f1_keywords:
- __declspec_cpp
- __declspec
- _declspec
helpviewer_keywords:
- __declspec keyword [C++]
ms.openlocfilehash: 1a8644bc05319332967ffd7934e6799408c3d36d
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465523"
---
# `__declspec`

**Microsoft 专用**

用于指定存储类信息的扩展特性语法使用 **`__declspec`** 关键字，该关键字指定给定类型的实例将与下面列出的 Microsoft 特定存储类特性一起存储。 其他存储类修饰符的示例包括 **`static`** 和 **`extern`** 关键字。 但是，这些关键字是 C 和 C++ 语言的 ANSI 规范的一部分，并且本身不包含在扩展特性语法中。 扩展特性语法简化并标准化了 Microsoft 专用的 C 和 C ++ 语言扩展。

## <a name="grammar"></a>语法

*`decl-specifier`*:\
&emsp;**`__declspec (`**  *`extended-decl-modifier-seq`*  **`)`**

*`extended-decl-modifier-seq`*:\
&emsp; *`extended-decl-modifier`* <sub>opt</sub> \
&emsp;*`extended-decl-modifier`* *`extended-decl-modifier-seq`*

*`extended-decl-modifier`*:\
&emsp;**`align(`***编号***`)`**\
&emsp;**`allocate("`***segname***`")`**\
&emsp;**`allocator`**\
&emsp;**`appdomain`**\
&emsp;**`code_seg("`***segname***`")`**\
&emsp;**`deprecated`**\
&emsp;**`dllimport`**\
&emsp;**`dllexport`**\
&emsp;**`jitintrinsic`**\
&emsp;**`naked`**\
&emsp;**`noalias`**\
&emsp;**`noinline`**\
&emsp;**`noreturn`**\
&emsp;**`nothrow`**\
&emsp;**`novtable`**\
&emsp;**`no_sanitize_address`**\
&emsp;**`process`**\
&emsp;**`property(`**{ **`get=`** _get-help-name_ &#124; put- **`,put=`** _name_ }**`)`**\
&emsp;**`restrict`**\
&emsp;**`safebuffers`**\
&emsp;**`selectany`**\
&emsp;**`spectre(nomitigation)`**\
&emsp;**`thread`**\
&emsp;**`uuid("`***ComObjectGUID***`")`**

空格用于分隔声明修饰符序列。 示例显示在后面的部分。

扩展特性语法支持以下特定于 Microsoft 的存储类特性： [`align`](../cpp/align-cpp.md) 、 [`allocate`](../cpp/allocate.md) 、、、、、、、、、、、、、、、、、、、 [`allocator`](../cpp/allocator.md) [`appdomain`](../cpp/appdomain.md) [`code_seg`](../cpp/code-seg-declspec.md) [`deprecated`](../cpp/deprecated-cpp.md) [`dllexport`](../cpp/dllexport-dllimport.md) [`dllimport`](../cpp/dllexport-dllimport.md) [`jitintrinsic`](../cpp/jitintrinsic.md) [`naked`](../cpp/naked-cpp.md) [`noalias`](../cpp/noalias.md) [`noinline`](../cpp/noinline.md) [`noreturn`](../cpp/noreturn.md) [`nothrow`](../cpp/nothrow-cpp.md) [`novtable`](../cpp/novtable.md) [`no_sanitize_address`](../cpp/no-sanitize-address.md) [`process`](../cpp/process.md) [`restrict`](../cpp/restrict.md) [`safebuffers`](../cpp/safebuffers.md) [`selectany`](../cpp/selectany.md) [`spectre`](../cpp/spectre.md) 和 [`thread`](../cpp/thread.md) 。 它还支持以下 COM 对象特性： [`property`](../cpp/property-cpp.md) 和 [`uuid`](../cpp/uuid-cpp.md) 。

、、、、、、、、、、 **`code_seg`** **`dllexport`** **`dllimport`** **`naked`** **`noalias`** **`nothrow`** **`no_sanitize_address`** **`property`** **`restrict`** **`selectany`** **`thread`** 和 **`uuid`** 存储类特性只是它们所应用到的对象或函数的声明的属性。 此 **`thread`** 属性仅影响数据和对象。 **`naked`** 和 **`spectre`** 特性仅影响函数。 **`dllimport`** 和 **`dllexport`** 特性影响函数、数据和对象。 **`property`**、 **`selectany`** 和 **`uuid`** 特性会影响 COM 对象。

为了与早期版本兼容， **`_declspec`** **`__declspec`** 除非指定了编译器选项/Za " [ \( 禁用语言) 扩展](../build/reference/za-ze-disable-language-extensions.md) "，否则将是同义词。

**`__declspec`** 关键字应置于简单声明的开头。 编译器将忽略任何警告， **`__declspec`** 位于 * 或 & 后面、在声明中的变量标识符前面的任何关键字。

**`__declspec`** 用户定义类型声明的开头指定的特性适用于该类型的变量。 例如：

```cpp
__declspec(dllimport) class X {} varX;
```

在本例中，此特性应用于 `varX`。 **`__declspec`** 位于或关键字后的 **`class`** 特性 **`struct`** 适用于用户定义的类型。 例如：

```cpp
class __declspec(dllimport) X {};
```

在本例中，此特性应用于 `X`。

用于简单声明的一般准则 **`__declspec`** 如下所示：

*decl-seq* *init-list*;

*Decl-seq* 应包含一个基类型 (例如 **`int`** ，、 **`float`** 、 **`typedef`** 或类名) 、存储类 (如 **`static`** 、 **`extern`**) 或 **`__declspec`** 扩展）的类型。 *Init-声明符列表* 应包含声明的指针部分。 例如：

```cpp
__declspec(selectany) int * pi1 = 0;   //Recommended, selectany & int both part of decl-specifier
int __declspec(selectany) * pi2 = 0;   //OK, selectany & int both part of decl-specifier
int * __declspec(selectany) pi3 = 0;   //ERROR, selectany is not part of a declarator
```

以下代码声明了一个整数线程本地变量，并用一个值对其进行了初始化：

```cpp
// Example of the __declspec keyword
__declspec( thread ) int tls_i = 1;
```

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[字](../cpp/keywords-cpp.md)\
[C 扩展的存储类特性](../c-language/c-extended-storage-class-attributes.md)
