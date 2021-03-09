---
title: 错误：堆-使用后-免费
description: 在出现免费错误后，堆使用的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- heap-use-after-free
helpviewer_keywords:
- heap-use-after-free error
- AddressSanitizer error heap-use-after-free
ms.openlocfilehash: 86e64537d40a86b1867e9ba16781f10b6ea9b417
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470862"
---
# <a name="error-heap-use-after-free"></a>错误：`heap-use-after-free`

> 地址 Sanitizer 错误：使用了释放的内存

我们会显示三个示例，其中，可以通过 `malloc` 、 `realloc` (c) 和 (c + +) 分配堆中的存储，并 `new` 在使用时使用 `volatile` 。

## <a name="example---malloc"></a>示例 - `malloc`

```cpp
// example1.cpp
// heap-use-after-free error
#include <stdlib.h>

int main() {
  char *x = (char*)malloc(10 * sizeof(char));
  free(x);

  // ...

  return x[5];   // Boom!
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/heap-use-after-free-example-1.png" alt-text="在示例1中显示 &quot;堆使用不足&quot; 错误的调试器屏幕截图。":::

## <a name="example---operator-new"></a>示例 - `operator new`

```cpp
// example2.cpp
// heap-use-after-free error
#include <windows.h>

int main() {
  char *buffer = new char[42];
  delete [] buffer;

  // ...

  buffer[0] = 42;  // Boom!
  return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example2.cpp /fsanitize=address /Zi
devenv /debugexe example2.exe
```

### <a name="resulting-error---operator-new"></a>产生的错误-new 运算符

:::image type="content" source="media/heap-use-after-free-example-2.png" alt-text="在示例2中显示 &quot;堆使用不足&quot; 错误的调试器屏幕截图。":::

## <a name="example---realloc"></a>示例 - `realloc`

```cpp
// example3.cpp
// heap-use-after-free error
#include <malloc.h>

int main() {
  char *buffer = (char*)realloc(0, 42);
  free(buffer);

  // ...

  buffer[0] = 42;  // Boom!
  return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example3.cpp /fsanitize=address /Zi
devenv /debugexe example3.exe
```

### <a name="resulting-error---realloc"></a>产生的错误-realloc

:::image type="content" source="media/heap-use-after-free-example-3.png" alt-text="显示示例3中的 &quot;堆使用后&quot; 错误的调试器屏幕截图。":::

## <a name="example---volatile"></a>示例-volatile

```cpp
// example4.cpp
// heap-use-after-free error
#include <stdlib.h>

int main() {

  volatile char *x = (char*)malloc(sizeof(char));
  free((void*)x);

      //...

  *x = 42;        // Boom!
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example4.cpp /fsanitize=address /Zi
devenv /debugexe example4.exe
```

### <a name="resulting-error---volatile"></a>产生的错误-volatile

:::image type="content" source="media/heap-use-after-free-example-4.png" alt-text="示例4中的调试器显示错误的屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
