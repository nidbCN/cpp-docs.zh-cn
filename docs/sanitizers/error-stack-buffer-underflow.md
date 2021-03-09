---
title: 错误：堆栈缓冲区-下溢
description: 堆栈缓冲区下溢错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- stack-buffer-underflow
helpviewer_keywords:
- stack-buffer-underflow error
- AddressSanitizer error stack-buffer-underflow
ms.openlocfilehash: 78705933378d7880057d9b7fd13a933f73129fcb
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470854"
---
# <a name="error-stack-buffer-underflow"></a>错误：`stack-buffer-underflow`

> 地址 Sanitizer 错误：堆栈缓冲区下溢

这些错误消息指示对堆栈变量开头之前某个位置的内存访问。

## <a name="example---local-array-underflow"></a>示例-本地阵列下溢

```cpp
// example1.cpp
// stack-buffer-underflow error
#include <stdio.h>

int main() {

    int subscript = -1;
    char buffer[42];
    buffer[subscript] = 42; // Boom!
   
    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/stack-buffer-underflow-example-1.png" alt-text="示例1中显示堆栈缓冲区下溢错误的调试器屏幕截图。":::

## <a name="example---stack-underflow-on-thread"></a>示例-线程上的堆栈下溢

```cpp
// example2.cpp
// stack-buffer-underflow error
#include <windows.h>

DWORD WINAPI thread_proc(void *) {
    int subscript = -1;
    volatile char stack_buffer[42];
    stack_buffer[subscript] = 42;

    return 0;
}

int main() {
    HANDLE thr = CreateThread(NULL, 0, thread_proc, NULL, 0, NULL);

    if (thr == 0) return 0;

    WaitForSingleObject(thr, INFINITE);

    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example2.cpp /fsanitize=address /Zi
devenv /debugexe example2.exe
```

### <a name="resulting-error----stack-underflow-on-thread"></a>线程上生成的错误堆栈下溢

:::image type="content" source="media/stack-buffer-underflow-example-2.png" alt-text="示例2中显示堆栈缓冲区下溢错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
