---
title: 兼容性
description: 介绍 Microsoft 通用 C 运行库 (UCRT) 与标准 C 库、POSIX、安全 CRT 和应用商店应用的兼容性。
ms.date: 9/11/2020
ms.topic: conceptual
helpviewer_keywords:
- CRT, compatibility
- compatibility, C runtime libraries
- compatibility
ms.assetid: 346709cb-edda-4909-9a19-3d253eddb6b7
ms.openlocfilehash: aea87c3a7b82292a5c9c45a960a5a890a1fff3fd
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514611"
---
# <a name="compatibility"></a>兼容性

通用 C 运行库 (UCRT) 支持 c + + 一致性所需的大多数 C 标准库。 它实现 C99 (ISO/IEC 9899:1999) 库，但有一些例外情况：

- 中的 strict 类型兼容性 \<complex.h> 。
- `aligned_alloc`，这可能不会实现，因为 Windows 操作系统不支持对齐分配。 改为使用非标准的 `_aligned_malloc` 。
- `strerrorlen_s`
- 中的原子支持 \<stdatomic.h>
- 线程支持 \<threads.h>

UCRT 还实现了 POSIX (ISO/IEC 9945-1:1996，POSIX 系统应用程序编程接口) C 库的大型子集。 但是，它并不完全符合任何特定的 POSIX 标准。 UCRT 还实现了几个特定于 Microsoft 的函数和不属于标准的宏。

在 vcruntime 库中找到了特定于 Visual C++ 的 Microsoft 实现的函数。  其中的许多函数都供内部使用，用户代码无法调用这些函数。 记录了一些函数，以供调试和实现兼容性时使用。

C++ 标准将全局命名空间中以下划线开头的名称保留到实现中。 POSIX 函数和 Microsoft 特定的运行时库函数都在全局命名空间中，但不属于标准 C 运行时库。 这就是为什么这些函数的首选 Microsoft 实现具有前导下划线的原因。 为了便于移植，UCRT 还支持默认名称，但使用它们编译代码时，Microsoft C++ 编译器会发出弃用警告。 只会弃用默认名称，而不会弃用函数本身。 若要取消警告，在使用原始 POSIX 名称的代码中包含任何标头之前，请定义 `_CRT_NONSTDC_NO_WARNINGS` 。

由于误用参数和未检查缓冲区，标准 C 库中的某些函数具有不安全的使用情况历史记录。 这些函数通常是代码中出现的安全问题的来源。 Microsoft 创建了这些函数的一组更加安全的版本来验证参数用法。 当在运行时检测到问题时，它们将调用无效参数处理程序。  默认情况下，如果使用的函数具有更加安全的变体，Microsoft C++ 编译器会发出弃用警告。 将代码编译为 c + + 时，可以 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 将定义为1以消除大多数警告。 此宏允许模板重载调用更安全的变体，同时保持可移植的源代码。 若要取消警告，在使用这些函数的代码中包含任何标头之前，请定义 `_CRT_SECURE_NO_WARNINGS` 。 有关详细信息，请参阅 [CRT 中的安全功能](../c-runtime-library/security-features-in-the-crt.md)。

除非文档中对特定函数另有注明，否则 UCRT 与 Windows API 可兼容。  在 Windows 应用商店或通用 Windows 平台 ([UWP](/uwp)) 应用中不支持某些功能。 [通用 Windows 平台应用中不支持的 CRT 函数](../cppcx/crt-functions-not-supported-in-universal-windows-platform-apps.md)中列出了这些函数。

## <a name="related-articles"></a>相关文章

|Title|说明|
|-----------|-----------------|
|[UWP 应用、Windows 运行时和 C 运行时](../c-runtime-library/windows-store-apps-the-windows-runtime-and-the-c-run-time.md)|说明 UCRT 例程何时与通用 Windows 应用或 Microsoft Store 应用不兼容。|
|[ANSI C 遵从性](../c-runtime-library/ansi-c-compliance.md)|说明 UCRT 中符合标准的命名。|
|[UNIX](../c-runtime-library/unix.md)|提供将程序移植到 UNIX 的指南。|
|[Windows 平台 (CRT) ](../c-runtime-library/windows-platforms-crt.md)|列出 CRT 支持的操作系统。|
|[向后兼容性](../c-runtime-library/backward-compatibility.md)|描述如何将旧 CRT 名称映射到新名称。|
|[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)|提供 CRT 库 (.lib) 文件和关联的编译器选项的概述。|
