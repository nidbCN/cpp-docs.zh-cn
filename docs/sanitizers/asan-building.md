---
title: AddressSanitizer 语言、生成和调试引用
description: 针对 AddressSanitizer 生成的技术说明
ms.date: 03/02/2021
f1_keywords:
- __SANITIZE_ADDRESS__
- ASAN_VCASAN_DEBUGGING
helpviewer_keywords:
- ASan reference
- AddressSanitizer reference
- Address Sanitizer reference
ms.openlocfilehash: f2f173da6d39b460098afe123a7537e36cbdf6a7
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470894"
---
# <a name="addresssanitizer-language-build-and-debugging-reference"></a>AddressSanitizer 语言、生成和调试引用

本文中的部分介绍 AddressSanitizer 语言规范、编译器选项和链接器选项。 它们还介绍了控制 Visual Studio 调试器集成的特定于 AddressSanitizer 的选项。

有关 AddressSanitizer 运行时、截获函数以及如何挂钩自定义分配器的详细信息，请参阅 [运行时引用](./asan-runtime.md)。 有关保存 AddressSanitizer 故障的故障转储的详细信息，请参阅 [故障转储参考](./asan-offline-crash-dumps.md)。

## <a name="language-specification"></a>语言规范

### `__SANITIZE_ADDRESS__`

`__SANITIZE_ADDRESS__`预处理器宏的定义方式 `1` 为 [`/fsanitize=address`](../build/reference/fsanitize.md) ：设置时。 此宏适用于高级用户有条件地指定存在 AddressSanitizer 运行时的源代码。

```cpp
#include <cstdio>

int main() {
    #ifdef __SANITIZE_ADDRESS__
        printf("Address sanitizer enabled");
    #else
        printf("Address sanitizer not enabled");
    #endif
    return 1;
}
```

### `__declspec(no_sanitize_address)`

[`__declspec(no_sanitize_address)`](../cpp/no-sanitize-address.md)说明符可用于有选择地禁用函数、局部变量或全局变量的 sanitizer。 这 `__declspec` 会影响 _编译器_ 行为，而不会影响 _运行时_ 行为。

```cpp
__declspec(no_sanitize_address)
void test1() {
    int x[100];
    x[100] = 5; // ASan exception not caught
}

void test2() {
    __declspec(no_sanitize_address) int x[100];
    x[100] = 5; // ASan exception not caught
}

__declspec(no_sanitize_address) int g[100];
void test3() {
    g[100] = 5; // ASan exception not caught
}
```

## <a name="compiler"></a>编译器

### <a name="fsanitizeaddress-compiler-option"></a>`/fsanitize=address` 编译器选项

[`/fsanitize=address`](../build/reference/fsanitize.md)编译器选项检测代码中的内存引用，以便在运行时捕获内存安全错误。 检测挂钩加载、存储、范围、alloca 和 CRT 函数。 它可以检测隐藏的 bug，如超出范围、使用后、使用后使用-作用域等。 有关在运行时检测到的错误的非详尽列表，请参阅 [AddressSanitizer 错误示例](./asan-error-examples.md)。

**`/fsanitize=address`** 与所有现有 c + + 或 C 优化级别兼容 (例如，、、、 **`/Od`** **`/O1`** **`/O2`** **`/O2 /GL`** 和按配置优化) 。 使用此选项生成的代码适用于静态和动态 crt (例如，、 **`/MD`** 、 **`/MDd`** **`/MT`** 和 **`/MTd`**) 。 此编译器选项可用于创建。EXE 或。面向 x86 或 x64 的 DLL。 调试信息是调用堆栈的最佳格式设置所必需的。

有关演示多种错误检测的代码示例，请参阅 [AddressSanitizer 错误示例](asan-error-examples.md)。

### <a name="fsanitize-address-use-after-return-compiler-option-experimental"></a>`/fsanitize-address-use-after-return` 试验)  (编译器选项

默认情况下，MSVC 编译器 (与 Clang 不同的是，) 不会生成代码来分配堆中的帧来捕获返回后错误。 若要使用 AddressSanitizer 捕获这些错误，您必须：

1. 使用选项编译 [`/fsanitize-address-use-after-return`](../build/reference/fsanitize.md) 。
2. 执行程序之前，请运行 `set ASAN_OPTIONS=detect_stack_use_after_return=1` 设置运行时检查选项。

**`/fsanitize-address-use-after-return`** 当局部变量被视为 "带地址" 时，选项将使编译器生成代码以在堆中使用双堆栈帧。 此代码比只单独使用 *更慢* **`/fsanitize=address`** 。 有关详细信息和示例，请参阅[错误： `stack-use-after-return` ](error-stack-use-after-return.md)。

从创建它的函数返回后，堆中的双堆栈帧仍将保留。 假设有一个示例，其中，分配给堆中的槽的本地地址在返回后使用。 与伪堆帧关联的阴影字节包含值0xF9。 在运行时报告错误时，该0xF9 表示堆栈使用后返回错误。

堆栈帧在堆中分配，并在函数返回后保留。 在特定时间间隔后，运行时使用垃圾回收来异步释放这些伪调用帧对象。 局部变量的地址会传输到堆中的持久性帧。 这是在定义函数返回后，系统可以检测的任何局部变量。 有关详细信息，请参阅按 Google 记录的 [返回后用于堆栈的算法](https://github.com/google/sanitizers/wiki/AddressSanitizerUseAfterReturn) 。

## <a name="linker"></a><a name="linker"></a> Linker

### <a name="inferasanlibsno-linker-option"></a>`/INFERASANLIBS[:NO]` 链接器选项

**`/fsanitize=address`** 编译器选项将对象标记为指定要链接到可执行文件的 AddressSanitizer 库。 这些库的名称以开头 *`clang_rt.asan*`* 。 [`/INFERASANLIBS`](../build/reference/inferasanlibs.md)默认情况下，链接器选项 () 会自动链接这些库的默认位置。 下面是所选和自动链接的库：

| CRT 选项 | DLL 或 EXE | DEBUG.EXE? | AddressSanitizer 运行时库 |
|--|--|--|--|
| MT | EXE | 是 | *`clang_rt.asan-{arch}`*, *`clang_rt.asan_cxx-{arch}`* |
| MT | DLL | 是 | *`clang_rt.asan_dll_thunk-{arch}`* |
| MD | 要么 | 是 | *`clang_rt.asan_dynamic-{arch}`*, *`clang_rt.asan_dynamic_runtime_thunk-{arch}`* |
| MT | EXE | YES | *`clang_rt.asan_dbg-{arch}`*, *`clang_rt.asan_dbg_cxx-{arch}`* |
| MT | DLL | YES | *`clang_rt.asan_dbg_dll_thunk-{arch}`* |
| MD | 要么 | YES | *`clang_rt.asan_dbg_dynamic-{arch}`*, *`clang_rt.asan_dbg_dynamic_runtime_thunk-{arch}`* |

链接器选项 [`/INFERASANLIBS:NO`](../build/reference/inferasanlibs.md) 可防止链接器从 *`clang_rt.asan*`* 默认位置链接库文件。 如果使用此选项，请将库路径添加到生成脚本中。 否则，链接器会报告无法解析的外部符号错误。

## <a name="visual-studio-integration"></a>Visual Studio 集成

### <a name="fno-sanitize-address-vcasan-lib-compiler-option"></a>`/fno-sanitize-address-vcasan-lib` 编译器选项

**`/fsanitize=address`** 当引发 AddressSanitizer 异常时，选项将在额外库中链接，以改善 Visual Studio 调试体验。 这些库称为 **VCAsan**。 使用这些库，Visual Studio 可以在源代码中显示 AddressSanitizer 错误。 它们还使可执行文件能够在创建 AddressSanitizer 错误报告时生成故障转储。 有关详细信息，请参阅 [Visual Studio AddressSanitizer 扩展功能库](./asan-debugger-integration.md)。

选择的库依赖于编译器选项，并在中自动链接。

| 运行时选项 | VCAsan 版本 |
|--------------|----------------|
| **`/MT`**        | *`libvcasan.lib`*  |
| **`/MD`**        | *`vcasan.lib`*     |
| **`/MTd`**       | *`libvcasand.lib`* |
| **`/MDd`**       | *`vcasand.lib`*    |

但是，如果使用 **`/Zl`** (省略默认库名称) 进行编译，则需要手动指定库。 如果没有，则会出现未解析的外部符号链接错误。 下面是一些典型示例：

```Output
error LNK2001: unresolved external symbol __you_must_link_with_VCAsan_lib
error LNK2001: unresolved external symbol ___you_must_link_with_VCAsan_lib
```

通过使用选项，可以在编译时禁用改进的调试 [`/fno-sanitize-address-vcasan-lib`](../build/reference/fsanitize.md) 。

### <a name="asan_vcasan_debugging-environment-variable"></a>`ASAN_VCASAN_DEBUGGING` 环境变量

**`/fsanitize=address`** 编译器选项生成的二进制文件在运行时公开内存安全错误。 从命令行启动二进制文件时，运行时将报告错误详细信息。 然后退出进程。 在 `ASAN_VCASAN_DEBUGGING` 运行时报告错误时，可以将环境变量设置为立即启动 Visual STUDIO IDE。 通过此编译器选项，您可以在导致错误的精确行和列中查看错误，并将其叠加在源代码中。

若要启用此行为，请在 `set ASAN_VCASAN_DEBUGGING=1` 运行应用程序之前运行该命令。 可以通过运行来禁用增强的调试体验 `set ASAN_VCASAN_DEBUGGING=0` 。

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
