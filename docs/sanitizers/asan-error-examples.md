---
title: AddressSanitizer 错误示例
description: Microsoft C/c + + 中的 AddressSanitizer 错误和示例的顶层说明。
ms.date: 03/01/2021
helpviewer_keywords:
- ASan error examples
- AddressSanitizer error examples
- Address Sanitizer error examples
- Error examples for AddressSanitizer
ms.openlocfilehash: 6f8036139c48b03fb8300f799fbdf05e5006aa4a
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470963"
---
# <a name="addresssanitizer-error-examples"></a>AddressSanitizer 错误示例

本部分列出了 Microsoft C/c + + (MSVC 中的 AddressSanitizer 支持的错误子集) 。 此列表并不是一个详尽的错误列表。 这会显示在 AddressSanitizer 中看到的多种错误。 在每篇文章中，我们都提供了示例代码，其中包含调试器的生成说明和屏幕截图。 它们将帮助你了解如何在代码中使用 MSVC 支持的 AddressSanitizer 功能。 所有屏幕截图都是使用生成的 **`devenv.exe /debugexe example.exe`** 。 其中一些示例基于 [LLVM 编译器-rt 测试套件](https://github.com/llvm/llvm-project/tree/main/compiler-rt/test/asan/TestCases)中的示例代码。

## <a name="build-the-error-examples"></a>生成错误示例

每个错误示例为命令行生成提供源代码和编译指令。 若要生成每个示例，请打开 " [开发人员命令提示](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)"。 为示例项目创建一个文件夹，然后将其设为当前目录。 然后，将示例代码复制到具有相应名称的源文件中，例如 *`example1.cpp`* 。 按照生成说明，在调试器中生成并运行检测的代码。

## <a name="errors-with-examples"></a>示例错误

- [错误：`alloc-dealloc-mismatch`](./error-alloc-dealloc-mismatch.md)

- [错误：`allocation-size-too-big`](./error-allocation-size-too-big.md)

- [错误：`calloc-overflow`](./error-calloc-overflow.md)

- [错误：`double-free`](./error-double-free.md)

- [错误：`dynamic-stack-buffer-overflow`](./error-dynamic-stack-buffer-overflow.md)

- [错误：`global-overflow`](./error-global-buffer-overflow.md)

- [错误：`heap-buffer-overflow`](./error-heap-buffer-overflow.md)

- [错误：`heap-use-after-free`](./error-heap-use-after-free.md)

- [错误：`invalid-allocation-alignment`](./error-invalid-allocation-alignment.md)

- [错误：`memcpy-param-overlap`](./error-memcpy-param-overlap.md)

- [错误：`new-delete-type-mismatch`](./error-new-delete-type-mismatch.md)

- [错误：`stack-buffer-overflow`](./error-stack-buffer-overflow.md)

- [错误：`stack-buffer-underflow`](./error-stack-buffer-underflow.md)

- [错误：`stack-use-after-return`](./error-stack-use-after-return.md)

- [错误：`stack-use-after-scope`](./error-stack-use-after-scope.md)

- [错误：`strncat-param-overlap`](./error-strncat-param-overlap.md)

- [错误：`use-after-poison`](./error-use-after-poison.md)

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)
