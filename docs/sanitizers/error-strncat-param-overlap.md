---
title: 错误： strncat-参数重叠
description: Strcat 参数重叠错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- strncat-param-overlap
helpviewer_keywords:
- strncat-param-overlap error
- AddressSanitizer error strcat-param-overlap
ms.openlocfilehash: 9ae5b6f602d61f26e1c5d3d9eded35d3f587c53e
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470850"
---
# <a name="error-strncat-param-overlap"></a>错误：`strncat-param-overlap`

> 地址 Sanitizer 错误： strncat-参数-重叠

移动重叠缓冲区中的内存的代码可能会导致难以诊断的错误。

## <a name="example"></a>示例：

此示例演示 AddressSanitizer 如何捕获由与 CRT 函数的重叠参数引起的错误。

基于 [llvm-project/compiler-rt/test/asan/测试用例/strncat-overlap](https://github.com/llvm/llvm-project/blob/62ec4ac90738a5f2d209ed28c822223e58aaaeb7/compiler-rt/test/asan/TestCases/strncat-overlap.cpp)的 (。 ) 

```cpp
// example1.cpp
// strncat-param-overlap error
#include <string.h>

void bad_function() {
    char buffer[] = "hello\0XXX";
    strncat(buffer, buffer + 1, 3); // BOOM
    return;
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

:::image type="content" source="media/strcat-param-overlap-example-1.png" alt-text="示例1中显示 strncat-参数重叠错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
