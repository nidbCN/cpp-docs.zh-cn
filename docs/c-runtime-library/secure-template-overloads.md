---
title: 安全模板重载
description: 提供安全增强功能的 Microsoft C 运行时模板重载说明。
ms.date: 11/04/2016
ms.topic: conceptual
f1_keywords:
- _CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES
- _CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES
- _CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES_COUNT
helpviewer_keywords:
- _CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES
- _CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES
- _CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES_COUNT
- secure template overloads
ms.assetid: 562741d0-39c0-485e-8529-73d740f29f8f
ms.openlocfilehash: 69ddeed831bde26de23e0a29ece9f5550de8cf9f
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514520"
---
# <a name="secure-template-overloads"></a>安全模板重载

为了支持增强安全机制的版本，Microsoft 已弃用许多 C 运行时库 (CRT) 函数。 例如，`strcpy_s` 是 `strcpy` 更安全的替代函数。 弃用的函数是安全错误的常见来源，因为它们不会阻止可以覆盖内存的操作。 默认情况下，编译器在你使用这些函数之一时生成弃用警告。 CRT 为这些函数提供了 C++ 模板重载，以帮助你轻松地过渡到使用更安全的变体。

例如，下面的代码片段生成警告，因为 `strcpy` 已遭弃用：

```cpp
char szBuf[10];
strcpy(szBuf, "test"); // warning: deprecated
```

你会看到弃用警告，提醒你代码可能不安全。 如果已验证代码无法覆盖内存，则有几个选择。 可以选择忽略弃用警告，也可以在 include 语句之前定义符号 `_CRT_SECURE_NO_WARNINGS` 以便 CRT 头能够禁用弃用警告，或者将代码更新为使用 `strcpy_s`：

```cpp
char szBuf[10];
strcpy_s(szBuf, 10, "test"); // security-enhanced _s function
```

模板重载提供了其他选择。 如果将 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 定义为 1，则会启用标准 CRT 函数的模板重载，从而自动调用更安全的变体。 如果 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 为 1，则无需更改代码。 在后台，对 `strcpy` 的调用会变成对 `strcpy_s` 的调用（使用自动提供的大小自变量）。

```cpp
#define _CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES 1

// ...

char szBuf[10];
strcpy(szBuf, "test"); // ==> strcpy_s(szBuf, 10, "test")
```

宏 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 不会影响采用计数的函数，例如 `strncpy` 。 若要为计数函数启用模板重载，请将 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES_COUNT` 定义为 1。 但是，在这样做之前，请确保您的代码将传递字符的计数，而不是传递缓冲区的大小（常见错误）。 此外，如果调用安全变体，则不再需要用来在函数调用之后在缓冲区末尾显式写入一个 null 终止符的代码。 如果需要截断行为，请参阅 [_TRUNCATE](../c-runtime-library/truncate.md)。

> [!NOTE]
> 宏 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES_COUNT` 要求 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 也定义为 1。 如果将 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES_COUNT` 定义为 1 并且将 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 定义为 0，则应用程序将不会执行任何模板重载。

将 `_CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES` 定义为 1 后，可启用安全变体（名称以“_s”结尾）的模板重载。 在这种情况下，如果 `_CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES` 为 1，则必须对原始代码进行一个小更改：

```cpp
#define _CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES 1

// ...

char szBuf[10];
strcpy_s(szBuf, "test"); // ==> strcpy_s(szBuf, 10, "test")
```

只需要更改函数的名称（添加“_s”）；模板重载负责提供大小自变量。

默认情况下，`_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 和 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES_COUNT` 将定义为 0（已禁用），而且 `_CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES` 将定义为 1（已启用）。

这些模板重载只适用于静态数组。 动态分配的缓冲区需要其他的源代码更改。 再次查看上面的示例：

```cpp
#define _CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES 1

// ...

char *szBuf = (char*)malloc(10);
strcpy(szBuf, "test"); // still deprecated; change it to
                       // strcpy_s(szBuf, 10, "test");
```

以及此示例：

```cpp
#define _CRT_SECURE_CPP_OVERLOAD_SECURE_NAMES 1

// ...

char *szBuf = (char*)malloc(10);
strcpy_s(szBuf, "test"); // doesn't compile; change it to
                         // strcpy_s(szBuf, 10, "test");
```

## <a name="see-also"></a>另请参阅

[CRT 中的安全功能](../c-runtime-library/security-features-in-the-crt.md)<br/>
[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
