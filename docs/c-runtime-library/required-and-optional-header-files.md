---
title: 必需和可选的头文件
description: 何时使用 Microsoft C 运行时库中的必需和可选标头文件。
ms.date: 11/04/2016
ms.topic: conceptual
f1_keywords:
- c.headers
helpviewer_keywords:
- include files, required in run time
- header files, required in run time
ms.assetid: f64d0bf5-e2c3-4b42-97d0-443b3d901d9f
ms.openlocfilehash: e7947c1f601a8051de5c28fba682e90c0bdce7b6
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514624"
---
# <a name="required-and-optional-header-files"></a>必需和可选的头文件

每个运行时例程的说明包括一个针对该例程的必选和可选包含文件或标头 (.H) 文件的列表。 需包括必需的标头文件以获取此例程的函数声明或由另一个内部调用的例程使用的定义。 通常包括可选标头文件以利用预定义的常量、类型定义或内联宏。 下表列出了可选标头内容的一些示例：

|定义|示例|
|----------------|-------------|
|宏定义|如果将库例程作为宏实现，则宏定义可能在原始例程的标头文件之外的其他标头文件中。 例如，在标头文件 CTYPE.H 中定义 `_toupper` 宏，而在 STDLIB.H 中声明函数 `toupper`。|
|预定义的常量|许多库例程都引用在标头文件中定义的常量。 例如，`_open` 例程使用常量，如在标头文件 FCNTL.H 中定义的 `_O_CREAT`。|
|类型定义|某些库例程返回结构或将结构用作参数。 例如，流输入/输出例程使用在 STDIO.H 中定义的 `FILE` 类型的结构。|

运行库标头文件提供 ANSI/ISO C 标准推荐样式的函数声明。 编译器对在其关联的函数声明后发生的任何例程引用执行类型检查。 函数声明对于返回除之外的某种类型的值的例程尤为重要 **`int`** ，这是默认值。 编译器将认为未在其声明中指定适当返回值的例程将返回 **`int`** ，这可能会导致意外的结果。 有关详细信息，请参阅[类型检查](../c-runtime-library/type-checking-crt.md)。

## <a name="see-also"></a>另请参阅

[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
