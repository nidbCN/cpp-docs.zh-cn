---
title: 参数验证
description: Microsoft C 运行时库中的参数验证的说明。
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- parameters, validation
ms.assetid: 019dd5f0-dc61-4d2e-b4e9-b66409ddf1f2
ms.openlocfilehash: 6926da8ccc1974352b926227daba0eea1d8a560d
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514650"
---
# <a name="parameter-validation"></a>参数验证

大多数安全性增强的 CRT 函数和很多不安全的 CRT 函数验证其参数，如检查指针是否为 **NULL**、整数位于有效范围内，或者枚举值有效。 如果找到无效的参数，则调用无效的参数处理程序。

## <a name="invalid-parameter-handler-routine"></a>无效参数处理程序例程

当 C 运行时库函数检测到无效的参数时，它会捕获有关错误的某些信息，然后调用包装无效参数处理程序调度函数的宏。 这将是 [_invalid_parameter](../c-runtime-library/reference/invalid-parameter-functions.md)、 [_invalid_parameter_noinfo](../c-runtime-library/reference/invalid-parameter-functions.md)或 [_invalid_parameter_noinfo_noreturn](../c-runtime-library/reference/invalid-parameter-functions.md)之一。 调用哪个调度函数取决于您的代码是分别是调试版本、零售版本还是不视为可恢复。

在调试版本中，无效的参数宏通常在调用调度函数之前引发失败的断言和调试器断点。 执行代码时，可以在具有 "中止"、"重试" 和 "继续" 的对话框中，根据操作系统和运行库版本向用户报告断言。 使用这些选项，用户可以立即终止程序、附加调试器，或者允许现有代码继续运行，从而调用调度函数。

无效参数处理程序调度函数调用当前分配的无效参数处理程序。 默认情况下，无效参数 `_invoke_watson` 会调用，这会导致应用程序关闭并生成小型转储。 如果操作系统启用了此功能，则会出现一个对话框，询问用户是否要将故障转储发送到 Microsoft 进行分析。

您可以使用函数 [_set_invalid_parameter_handler](../c-runtime-library/reference/set-invalid-parameter-handler-set-thread-local-invalid-parameter-handler.md) 或 [_set_thread_local_invalid_parameter_handler](../c-runtime-library/reference/set-invalid-parameter-handler-set-thread-local-invalid-parameter-handler.md) 来更改此行为，以将无效参数处理程序设置为您自己的函数。 如果指定的函数未终止应用程序，控制权就会返回至已接收无效参数的函数处。 在 CRT 中，这些函数通常会停止执行函数，将设置 `errno` 为错误代码，并返回错误代码。 在许多情况下， `errno` 值和返回值都是 `EINVAL` ，用于指示参数无效。 有时候会返回更具体的错误代码，如作为参数传递的错误的文件指针的 `EBADF`。

有关 `errno` 的详细信息，请参阅 [errno、_doserrno、_sys_errlist 和 _sys_nerr](../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

## <a name="see-also"></a>另请参阅

[CRT 中的安全功能](../c-runtime-library/security-features-in-the-crt.md)\
[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
