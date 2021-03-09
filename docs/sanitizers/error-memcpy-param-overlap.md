---
title: 错误： memcpy-参数重叠
description: Memcpy 参数重叠错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- memcpy-param-overlap
helpviewer_keywords:
- memcpy-param-overlap error
- AddressSanitizer error memcpy-param-overlap
ms.openlocfilehash: 1df0310ab2abb326cf895a72afbdffa7c239d9da
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470932"
---
# <a name="error-memcpy-param-overlap"></a>错误：`memcpy-param-overlap`

> 地址 Sanitizer 错误： memcpy-参数-重叠

CRT 函数 [`memcpy`](../c-runtime-library/reference/memcpy-wmemcpy.md) **不支持** 重叠内存。 CRT 提供的替代方法 `memcpy` 支持重叠内存： [`memmove`](../c-runtime-library/reference/memmove-wmemmove.md) 。

常见的错误是将视为 `memmove` 在语义上等效于 `memcpy` 。

## <a name="example"></a>示例：

```cpp
// example1.cpp
// memcpy-param-overlap error
#include <string.h>

__declspec(noinline) void bad_function() {
    char buffer[] = "hello";

    memcpy(buffer, buffer + 1, 5); // BOOM!
}

int main(int argc, char **argv) {
    bad_function();
    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/memcpy-param-overlap-example-1.png" alt-text="示例1中显示 memcpy-参数重叠错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
