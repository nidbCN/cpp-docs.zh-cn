---
title: SAL 批注
description: " (SAL) 的 Microsoft 源代码批注语言的简短说明。"
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- __z annotation
- ref annotation
- _opt annotation
- __checkreturn annotatioin
- __deref_opt annotation
- deref_opt annotation
- __deref annotation
- __in annotation
- annotations [C++]
- z annotation
- _inout annotation
- __ref annotation
- full annotation
- _in annotation
- _ref annotation
- __out annotation
- _ecount annotation
- SAL annotations
- __opt annotation
- inout annotation
- in annotation
- _CA_SHOULD_CHECK_RETURN
- __bcount annotation
- _full annotation
- _bcount annotation
- deref annotation
- part annotation
- _out annotation
- __nz annotation
- __part annotation
- opt annotation
- __full annotation
- _nz annotation
- _z annotation
- out annotation
- __ecount annotation
- __inout annotation
- SAL annotations, _CA_SHOULD_CHECK_RETURN
- _deref_opt annotation
- _deref annotation
- nz annotation
- _part annotation
- ecount annotation
- bcount annotation
ms.assetid: 81893638-010c-41a0-9cb3-666fe360f3e0
ms.openlocfilehash: c4c75213daa69c433b379fb78a0649fa1473de1b
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514455"
---
# <a name="sal-annotations"></a>SAL 批注

如果检查库的标头文件，您可能会注意一些特殊的注释，例如，`_In_z` 和 `_Out_z_cap_(_Size)`。 这些是 Microsoft 源代码注释语言 (SAL) 的示例。SAL 提供一组描述函数如何使用其参数的注释，例如，它关于参数的假设以及关于完成的保证。 标头文件 \<sal.h> 定义注释。

有关在 Visual Studio 中使用 SAL 注释的详细信息，请参阅[使用 SAL 注释减少 C/C++ 代码缺陷](../code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects.md)。

## <a name="see-also"></a>另请参阅

[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
