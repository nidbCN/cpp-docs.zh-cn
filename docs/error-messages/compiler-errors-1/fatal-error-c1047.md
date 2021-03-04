---
description: 了解详细信息：严重错误 C1047
title: 错误 C1047
ms.date: 02/17/2021
f1_keywords:
- C1047
helpviewer_keywords:
- C1047
ms.openlocfilehash: f22d7c7591e2c2c477d09f1637641cc351eeafb5
ms.sourcegitcommit: 5efc34c2b98d4d0d3e41aec38b213f062c19d078
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101844476"
---
# <a name="fatal-error-c1047"></a>错误 C1047

> 对象或库文件 "*filename*" 是使用比其他对象更早的编译器创建的;重新生成旧的对象和库

如果使用新版本的编译器生成项目，但不完全重新生成现有对象文件或库，则会发生此错误。

## <a name="remarks"></a>备注

当使用 **`/GL`** 或 **`/LTCG`** 在不同版本的 Visual Studio C/c + + 编译器工具集生成的对象文件或库链接在一起时，会导致 C1047。 例如，不能将 **`/LTCG`** 使用 Visual studio 2019 版本16.7 生成的库链接到使用 Visual studio 2019 版本16.8 构建的应用程序。 用于编译对象和库的工具集的主要和次要更新号必须完全匹配。

若要解析 C1047，请使用相同版本的工具集重新生成所有对象文件或库。

## <a name="see-also"></a>另请参阅

[`/GL` (完全程序优化) ](../../build/reference/gl-whole-program-optimization.md)\
[`/LTCG` (的链接时间代码生成) ](../../build/reference/ltcg-link-time-code-generation.md)\
[C++ 二进制兼容性 (2015-2019)](../../porting/binary-compat-2015-2017.md)
