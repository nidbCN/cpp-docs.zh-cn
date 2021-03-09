---
title: 错误：堆缓冲区溢出
description: 堆变量溢出错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- heap-buffer-overflow
helpviewer_keywords:
- heap-buffer-overflow error
- AddressSanitizer error heap-buffer-overflow
ms.openlocfilehash: 7eec8d0fa8c7382a5a4ecea9811298aaaba760a2
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470864"
---
# <a name="error-heap-buffer-overflow"></a>错误：`heap-buffer-overflow`

> 地址 Sanitizer 错误：堆缓冲区溢出

此示例演示当内存访问发生在堆分配的对象的边界之外时产生的错误。

## <a name="example---classic-heap-buffer-overflow"></a>示例-典型堆缓冲区溢出

```cpp
// example1.cpp
// heap-buffer-overflow error
#include <stdlib.h>
#include <string.h>

int main(int argc, char **argv) {

    char *x = (char*)malloc(10 * sizeof(char));
    memset(x, 0, 10);
    int res = x[argc * 10];  // Boom!

    free(x);
    return res;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/heap-buffer-overflow-example-1.png" alt-text="示例1中显示堆缓冲区溢出错误的调试器屏幕截图。":::

## <a name="example---improper-down-cast"></a>示例-不正确的强制转换

```cpp
// example2.cpp
// heap-buffer-overflow error
class Parent {
public:
    int field;
};

class Child : public Parent {
public:
    int extra_field;
};

int main(void) {
    Parent *p = new Parent;
    Child *c = (Child*)p;  // Intentional error here!
    c->extra_field = 42;

    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example2.cpp /fsanitize=address /Zi
devenv /debugexe example2.exe
```

### <a name="resulting-error---improper-down-cast"></a>产生的错误-不正确的强制转换

:::image type="content" source="media/heap-buffer-overflow-example-2.png" alt-text="示例2中显示堆缓冲区溢出错误的调试器屏幕截图。":::

## <a name="example---strncpy-into-heap"></a>示例-strncpy 为堆

```cpp
// example3.cpp
// heap-buffer-overflow error
#include <string.h>
#include <stdlib.h>

int main(int argc, char **argv) {

    char *hello = (char*)malloc(6);
    strcpy(hello, "hello");

    char *short_buffer = (char*)malloc(9);
    strncpy(short_buffer, hello, 10);  // Boom!

    return short_buffer[8];
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example3.cpp /fsanitize=address /Zi
devenv /debugexe example3.exe
```

### <a name="resulting-error---strncpy-into-heap"></a>产生的错误-strncpy 堆

:::image type="content" source="media/heap-buffer-overflow-example-3.png" alt-text="示例3中显示堆缓冲区溢出错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
