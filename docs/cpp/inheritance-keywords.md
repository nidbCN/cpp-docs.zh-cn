---
description: 了解更多：继承关键字
title: 继承关键字
ms.date: 11/04/2016
f1_keywords:
- __multiple_inheritance
- __single_inheritance_cpp
- __virtual_inheritance_cpp
- __virtual_inheritance
- __multiple_inheritance_cpp
- __single_inheritance
helpviewer_keywords:
- __single_inheritance keyword [C++]
- declaring derived classes [C++]
- keywords [C++], inheritance keywords
- __multiple_inheritance keyword [C++]
- __virtual_inheritance keyword [C++]
- inheritance, declaring derived classes
- derived classes [C++], declaring
- inheritance, keywords
ms.assetid: bb810f56-7720-4fea-b8b6-9499edd141df
ms.openlocfilehash: d2cd576d80b3e68eaf2a0d57fecf25309d0719b0
ms.sourcegitcommit: bf6d8a220f6392f1f19c0c0605d1467d0221ef6c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551730"
---
# <a name="inheritance-keywords"></a>继承关键字

**Microsoft 专用**

```
class [__single_inheritance] class-name;
class [__multiple_inheritance] class-name;
class [__virtual_inheritance] class-name;
```

其中：

*类名*<br/>
要声明的类的名称。

C++ 允许您在类定义前声明指向类成员的指针。 例如：

```cpp
class S;
int S::*p;
```

在上面的代码中， `p` 被声明为指向类的整数成员的指针。但 `class S` 在此代码中尚未定义; 它只是声明的。 当编译器遇到此类指针时，它必须生成此指针的泛化表示形式。 表示形式的大小依赖于指定的继承模型。 可通过四种方式指定编译器的继承模型：

- 在 IDE 中指向 **成员的指针表示形式**

- 在命令行中使用 [/vmg](../build/reference/vmb-vmg-representation-method.md) 开关

- 使用 [pointers_to_members](../preprocessor/pointers-to-members.md) 杂注

- 使用继承关键字 **`__single_inheritance`** 、 **`__multiple_inheritance`** 和 **`__virtual_inheritance`** 。 此技术控制每个类的继承模型。

    > [!NOTE]
    >  如果在定义类后始终声明指向类成员的指针，则无需使用上述任何选项。

在类定义之前声明一个指向类成员的指针会影响生成的可执行文件的大小和速度。           类使用的继承越复杂，表示指向此类成员的指针所需的字节数以及解释指针所需的代码数就越大。 单一继承的复杂性最低，虚拟继承的复杂性最高。

如果将上面的示例更改为：

```cpp
class __single_inheritance S;
int S::*p;
```

无论命令行选项或杂注如何，指向 `class S` 的成员的指针都将使用可能的最小表示形式。

> [!NOTE]
> 类的指向成员的指针表示形式的同一向前声明应出现在声明指向该类的成员的指针的每个翻译单元中，并且声明应在声明指向成员的指针之前出现。

为了与早期版本兼容，、和 **`_single_inheritance`** **`_multiple_inheritance`** **`_virtual_inheritance`** 是、和的同义词， **`__single_inheritance`** **`__multiple_inheritance`** **`__virtual_inheritance`** 除非指定了编译器选项 [/za " \( 禁用语言扩展")](../build/reference/za-ze-disable-language-extensions.md) 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[关键字](../cpp/keywords-cpp.md)
