---
title: 错误：堆栈-返回后使用
description: 返回错误后用于堆栈的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- stack-use-after-return
helpviewer_keywords:
- stack-use-after-return error
- AddressSanitizer error stack-use-after-return
ms.openlocfilehash: 37d950b0c3b4da880524c0c5825d86d6feec9654
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470901"
---
# <a name="error-stack-use-after-return"></a>错误：`stack-use-after-return`

> 地址 Sanitizer 错误：返回后使用堆栈内存

此检查要求代码生成由额外的编译器选项、 [`/fsanitize-address-use-after-return`](../build/reference/fsanitize.md) 和设置环境变量激活 `ASAN_OPTIONS=detect_stack_use_after_return=1` 。

此检查可大大降低应用程序的性能。 请考虑在返回后支持使用的算法的 [Clang 摘要](https://github.com/google/sanitizers/wiki/AddressSanitizerUseAfterReturn) ，以及更大的性能成本。

> [!IMPORTANT]
> 如果使用额外的编译器选项创建对象文件，则 **`/fsanitize-address-use-after-return`** 编译器生成的代码会向运行时决定如何分配堆栈帧。 如果 `ASAN_OPTIONS` 未将环境变量设置为 `detect_stack_use_after_return` ，则代码仍比自行使用更慢 [`/fsanitize=address`](../build/reference/fsanitize.md) 。 这会变慢，因为某些堆栈帧仍有额外的开销，因为它通过使用为帧的各部分分配空间 `alloca()` 。 最好在处理完 "使用后-返回" 错误后删除这些对象文件。

## <a name="example---simple-c"></a>示例-简单 C

```cpp
// example1.cpp
// stack-use-after-return error
char* x;

void foo() {
    char stack_buffer[42];
    x = &stack_buffer[13];
}

int main() {

    foo();
    *x = 42; // Boom!

    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /fsanitize-address-use-after-return /Zi
set ASAN_OPTIONS=detect_stack_use_after_return=1
devenv /debugexe example1.exe
```

### <a name="resulting-error---simple-c"></a>产生的错误-简单 C

:::image type="content" source="media/stack-use-after-return-example-1.png" alt-text="示例1中的调试器显示堆栈-使用后错误的屏幕截图。":::

## <a name="example---c-and-templates"></a>示例-c + + 和模板

```cpp
// example2.cpp
// stack-use-after-return error
#include <stdlib.h>

enum ReadOrWrite { Read = 0, Write = 1 };

struct S32 {
    char x[32];
};

template<class T>
T* LeakStack() {
    T t[100];
    static volatile T* x;
    x = &t[0];
    return (T*)x;
}

template<class T>
void StackUseAfterReturn(int Idx, ReadOrWrite w) {
    static T sink;
    T* t = LeakStack<T>();
    if (w)
        t[100 + Idx] = T();
    else
        sink = t[100 + Idx];
}

int main(int argc, char* argv[]) {

    if (argc != 2) return 1;
    int kind = atoi(argv[1]);

    switch (kind) {
    case 1: StackUseAfterReturn<char>(0, Read); break;
    case 2: StackUseAfterReturn<S32>(0, Write); break;
    }
    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example2.cpp /fsanitize=address /fsanitize-address-use-after-return /Zi
set ASAN_OPTIONS=detect_stack_use_after_return=1
devenv /debugexe example2.exe 1
```

### <a name="resulting-error---c-and-templates"></a>产生的错误-c + + 和模板

:::image type="content" source="media/stack-use-after-return-example-2.png" alt-text="示例2中显示堆栈-使用后错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
