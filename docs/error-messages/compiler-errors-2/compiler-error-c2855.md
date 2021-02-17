---
description: 了解更多：编译器错误 C2855
title: 编译器错误 C2855
ms.date: 02/16/2021
f1_keywords:
- C2855
helpviewer_keywords:
- C2855
ms.openlocfilehash: cc26dbf92ea385ee05681e5fab8b5c4257c2b525
ms.sourcegitcommit: e99db7c3b5f25ece0e152165066c926751a7c2ed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100643542"
---
# <a name="compiler-error-c2855"></a>编译器错误 C2855

> 命令行选项 "*option*" 与预编译头不一致

当命令行选项与用于创建预编译标头的选项不同时，将出现此错误。

## <a name="remarks"></a>注解

当你在更改编译器选项之后进行增量生成时，可能会发生错误 C2855。 如果为单个源文件设置特定编译器选项，也会发生这种情况。

若要解决此错误，请使用指定的命令行选项重新生成预编译头。 若要重新生成预编译标头，请生成关联的源文件。 例如，Visual Studio 模板创建的项目通常会创建一个名为的源文件 *`pch.cpp`* 来生成预编译标头。  (在较早版本的 Visual Studio 中，此文件命名为 *`stdafx.cpp`* 。 ) 在其他项目中，要重新生成的源文件是使用 [ `/Yc` (创建预编译标头文件)](../../build/reference/yc-create-precompiled-header-file.md)编译器选项生成的。 建议在对预编译标头进行更改后重新生成整个项目。

## <a name="see-also"></a>另请参阅

[`/Y` (预编译标头) ](../../build/reference/y-precompiled-headers.md)\
[预编译标头文件](../../build/creating-precompiled-header-files.md)
