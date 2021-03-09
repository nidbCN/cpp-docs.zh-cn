---
title: 关于选择函数和宏的建议
description: '说明在 Microsoft C 运行时库中使用宏与函数 (CRT 的不同之处) '
ms.date: 11/04/2016
ms.topic: conceptual
f1_keywords:
- c.functions
helpviewer_keywords:
- functions [CRT], vs. macros
- macros, vs. functions
ms.assetid: 18a633d6-cf1c-470c-a649-fa7677473e2b
ms.openlocfilehash: 9a2190d417ef004ab9a0d07f19214cb29f623244
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514533"
---
# <a name="recommendations-for-choosing-between-functions-and-macros"></a>关于选择函数和宏的建议

大多数 Microsoft 运行库例程都是编译的或汇编的函数，但有些例程是作为宏实现的。 当标头文件声明例程的函数和宏版本时，将优先考虑宏定义，因为它始终显示在函数声明之后。 在调用作为函数和宏实现的例程时，您可以通过两种方法强制编译器使用函数版本：

- 将例程名称用括号括起来。

    ```C
    #include <ctype.h>
    a = _toupper(a);    // Use macro version of toupper.
    a = (_toupper)(a);  // Force compiler to use
                        // function version of toupper.
    ```

- 使用 `#undef` 指令“取消定义”宏：

    ```C
    #include <ctype.h>
    #undef _toupper
    ```

如果您需要在库例程的函数和宏实现之间进行选择，请考虑以下折衷方案：

- **速度与大小** 使用宏的主要好处是执行速度更快。 在预处理期间，宏在被使用时将内联展开（由其定义替换）。 无论调用函数定义多少次，它仅执行一次。 宏可能会增加代码大小，但不会产生与函数调用关联的开销。

- **函数计算** 函数的计算结果为地址；而宏不是。 因此，您不能在需要指针的上下文中使用宏名称。 例如，您可以声明指向函数的指针而不是指向宏的指针。

- **类型检查** 在声明函数时，编译器会检查参数类型。 由于你不能声明宏，因此编译器无法检查宏自变量类型；尽管它可以检查传递给宏的自变量的数目。

## <a name="see-also"></a>另请参阅

[类型-泛型数学](tgmath.md)\
[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
