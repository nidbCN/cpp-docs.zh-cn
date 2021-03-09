---
title: 错误： calloc-溢出
description: Calloc 溢出错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- calloc-overflow
helpviewer_keywords:
- calloc-overflow error
- AddressSanitizer error calloc-overflow
ms.openlocfilehash: 63af733061d255924f0b0fbd5f54e4d77359c436
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470866"
---
# <a name="error-calloc-overflow"></a>错误：`calloc-overflow`

> 地址 Sanitizer 错误： calloc-溢出

CRT 函数 [`calloc`](../c-runtime-library/reference/calloc.md) 在内存中创建一个数组，其中的元素被初始化为0。 参数可以创建一个内部错误，该错误将导致 NULL 指针作为返回值。

## <a name="example"></a>示例：

```cpp
// example1.cpp
// calloc-overflow error
#include <stdio.h>
#include <stdlib.h>

int number = -1;
int element_size = 1000;

int main() {

    void *p = calloc(number, element_size);      // Boom!

    printf("calloc returned: %zu\n", (size_t)p);

    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/calloc-overflow-example-1.png" alt-text="示例1中显示 calloc 错误的调试器的屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
