---
title: CRT 中的安全功能
description: Microsoft C 运行时中的安全 CRT 函数的概述。
ms.date: 09/29/2020
ms.topic: conceptual
f1_keywords:
- _CRT_SECURE_NO_DEPRECATE
- _CRT_NONSTDC_NO_WARNINGS
- _CRT_SECURE_NO_WARNINGS
helpviewer_keywords:
- security deprecation warnings [C++]
- CRT_NONSTDC_NO_DEPRECATE
- buffers [C++], buffer overruns
- deprecation warnings (security-related), disabling
- _CRT_NONSTDC_NO_WARNINGS
- security [CRT]
- _CRT_SECURE_NO_WARNINGS
- _CRT_NONSTDC_NO_DEPRECATE
- _CRT_SECURE_NO_DEPRECATE
- security-enhanced CRT
- CRT_SECURE_NO_WARNINGS
- CRT_SECURE_NO_DEPRECATE
- deprecation warnings (security-related)
- buffer overruns
- CRT_NONSTDC_NO_WARNINGS
- CRT, security enhancements
- parameters [C++], validation
ms.assetid: d9568b08-9514-49cd-b3dc-2454ded195a3
ms.openlocfilehash: 96642e5e79f9df7c0a063582408c0463600be20f
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514676"
---
# <a name="security-features-in-the-crt"></a>CRT 中的安全功能

许多旧 CRT 函数具有更新、更安全的版本。 如果存在安全函数，则较旧的、安全性更低的版本将标记为已弃用，并且新版本具有 `_s`（“安全”）后缀。

在此上下文中，"已弃用" 表示不建议使用函数。 这并不意味着计划从 CRT 中删除该函数。

安全函数不会阻止或更正安全错误。 相反，它们会在出现错误时捕获它们。 它们对错误条件进行额外检查。 如果出现错误，它们将调用错误处理程序， (参见 [参数验证](../c-runtime-library/parameter-validation.md)) 。

例如， `strcpy` 函数无法判断它所复制的字符串对于目标缓冲区是否太大。 它的安全对应 `strcpy_s` 项将缓冲区的大小作为参数使用。 因此，它可以确定是否将发生缓冲区溢出。 如果你使用将 `strcpy_s` 11 个字符复制到10个字符的缓冲区中，这就是一个错误， `strcpy_s` 无法更正你的错误。 但是，它可以检测错误，并通过调用无效参数处理程序来通知您。

## <a name="eliminating-deprecation-warnings"></a>消除弃用警告

可通过多种方式消除针对较旧的、安全性更低的函数的弃用警告。 最简单的方法是定义 `_CRT_SECURE_NO_WARNINGS` 或使用[警告](../preprocessor/warning.md)杂注。 将禁用弃用警告，但导致出现警告的安全问题仍存在。 最好将弃用警告保持启用状态并利用新的 CRT 安全功能。

在 c + + 中，执行此操作的最简单方法是使用 [安全模板重载](../c-runtime-library/secure-template-overloads.md)。 在许多情况下，这会通过将对不推荐使用的函数的调用替换为对这些函数的安全版本的调用来消除弃用警告。 例如，考虑此对 `strcpy` 的已弃用调用：

```
char szBuf[10];
strcpy(szBuf, "test"); // warning: deprecated
```

将 `_CRT_SECURE_CPP_OVERLOAD_STANDARD_NAMES` 定义为 1 可通过将 `strcpy` 调用更改为 `strcpy_s`（这将阻止缓冲区溢出）来消除警告。 有关详细信息，请参阅[安全模板重载](../c-runtime-library/secure-template-overloads.md)。

对于那些不带安全模板重载的已弃用的函数，你应考虑手动更新代码以使用安全版本。

弃用警告的另一个源（与安全性无关）为 POSIX 函数。 将 POSIX 函数名称替换为它们的标准等效项（例如，将 [access](../c-runtime-library/reference/access-crt.md) 更改为 [_access](../c-runtime-library/reference/access-waccess.md)），或通过定义 `_CRT_NONSTDC_NO_WARNINGS` 来禁用与 POSIX 相关的弃用警告。 有关详细信息，请参阅[兼容性](compatibility.md)。

## <a name="additional-security-features"></a>其他安全功能

一些安全功能包括：

- `Parameter Validation`. 安全函数及其很多不安全的功能，验证参数。 验证可能包括：

  - 检查是否为 **NULL** 值。
  - 检查枚举值的有效性。
  - 检查整数值是否在有效范围内。

- 有关详细信息，请参阅[参数验证](../c-runtime-library/parameter-validation.md)。

- 开发人员也可访问无效参数的处理程序。 当函数遇到无效参数时，CRT 使你可以通过 [_set_invalid_parameter_handler _set_thread_local_invalid_parameter_handler](../c-runtime-library/reference/set-invalid-parameter-handler-set-thread-local-invalid-parameter-handler.md)来查看这些问题，而不是断言并退出应用程序。

- `Sized Buffers`. 必须将缓冲区大小传递到任何写入缓冲区的安全函数。 安全版本会在写入缓冲区之前验证缓冲区是否足够大。 这有助于避免可能允许恶意代码执行的危险缓冲区溢出错误。 `errno`如果缓冲区的大小太小，这些函数通常会返回错误代码并调用无效参数处理程序。 从输入缓冲区读取的函数（如 `gets`）具有需要您指定最大大小的安全版本。

- `Null termination`. 某些可能包含非终止字符串的函数具有安全版本，这可确保字符串正确地以 null 结尾。

- `Enhanced error reporting`. 安全函数返回的错误代码包含的错误信息超出了预先存在的函数。 现在，安全函数和许多预先存在的函数会设置 `errno` 并经常返回 `errno` 代码类型，以便提供更好的错误报告。

- `Filesystem security`. 默认情况下，安全文件 I/O API 支持安全文件访问。

- `Windows security`. 安全进程 API 强制安全策略并允许指定 ACL。

- `Format string syntax checking`. 检测到无效字符串，例如，在 `printf` 格式字符串中使用了不正确的类型字段字符。

## <a name="see-also"></a>另请参阅

[参数验证](../c-runtime-library/parameter-validation.md)<br/>
[安全模板重载](../c-runtime-library/secure-template-overloads.md)<br/>
[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
