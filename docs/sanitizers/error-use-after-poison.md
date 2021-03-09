---
title: 错误：-有害后使用
description: 在出现病毒错误后使用的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- use-after-poison
helpviewer_keywords:
- use-after-poison error
- AddressSanitizer error use-after-poison
ms.openlocfilehash: 0175b79df493dc60c19d78f045ba1829dc0b6b27
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470900"
---
# <a name="error-use-after-poison"></a>错误：`use-after-poison`

> 地址 Sanitizer 错误：使用中毒内存

开发人员可以手动对内存进行自定义调试。

## <a name="example"></a>示例：

```cpp
// example1.cpp
// use-after-poison error
#include <stdlib.h>

extern "C" void __asan_poison_memory_region(void *, size_t);

int main(int argc, char **argv) {
    char *x = new char[16];
    x[10] = 0;
    __asan_poison_memory_region(x, 16);

    int res = x[argc * 10];              // Boom!
 
    delete [] x;
    return res;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/use-after-poison-example-1.png" alt-text="示例1中显示 &quot;使用-病毒后&quot; 错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
