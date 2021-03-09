---
title: 错误：分配-大小-太大
description: 分配-大小-错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- allocation-size-too-big
helpviewer_keywords:
- allocation-size-too-big error
- AddressSanitizer error allocation-size-too-big
ms.openlocfilehash: 3320064f52a4d41ca556b9525760997f52e1385b
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470883"
---
# <a name="error-allocation-size-too-big"></a>错误：`allocation-size-too-big`

> 地址 Sanitizer 错误：分配-大小-太大

此示例显示在分配对于堆太大时发现的错误。 源自 [LLVM 编译器-rt 测试套件](https://github.com/llvm/llvm-project/tree/main/compiler-rt/test/asan/TestCases)的示例。

## <a name="example"></a>示例：

```cpp
// example1.cpp
// allocation-size-too-big error
#include <stdio.h>
#include <malloc.h>
#include <memory.h>

int x = 1000;
int y = 1000;

__declspec(noinline) void bad_function() {

  char* buffer = (char*)malloc(x * y * x * y); //Boom!

  memcpy(buffer, buffer + 8, 8); 
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

:::image type="content" source="media/allocation-size-too-big-example-1.png" alt-text="示例1中显示分配-大小太大错误的调试器的屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
