---
title: AddressSanitizer 运行时
description: 适用于 Microsoft C/c + + 的 AddressSanitizer 运行时的技术说明。
ms.date: 03/02/2021
helpviewer_keywords:
- AddressSanitizer runtime
- Address Sanitizer runtime
- clang_rt.asan
- Clang runtime
- ASan runtime
ms.openlocfilehash: 6ab27365ba6841dd5314f1eac65abd71b7d4170d
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470886"
---
# <a name="addresssanitizer-runtime"></a>AddressSanitizer 运行时

AddressSanitizer 运行时库会截获常见的内存分配函数和操作，以启用内存访问检查。 有几个不同的运行时库支持编译器可能生成的各种类型的可执行文件。 只要在编译时传递选项，编译器和链接器就会自动链接相应的运行时库 [`/fsanitize=address`](../build/reference/fsanitize.md) 。 您可以通过在链接时使用选项来重写默认行为 **`/NODEFAULTLIB`** 。 有关详细信息，请参阅[AddressSanitizer language、build 和调试参考](./asan-building.md)中有关[链接](./asan-building.md#linker)的部分。

下面是用于链接到 AddressSanitizer 运行时的运行库的清单，其中 *`{arch}`* 是 *`i386`* 或 *`x86_64`* 。

> [!NOTE]
> 这些库保留了体系结构名称的 Clang 约定。 MSVC 约定通常是 x86 和 x64，而不是 i386 和 x86_64。 它们引用相同的体系结构。

| CRT 选项 | DLL 或 EXE | DEBUG.EXE? | AddressSanitizer 运行时二进制库 |
|--|--|--|--|
| MT | EXE | 是 | *`clang_rt.asan-{arch}`*, *`clang_rt.asan_cxx-{arch}`* |
| MT | DLL | 是 | *`clang_rt.asan_dll_thunk-{arch}`* |
| MD | 要么 | 是 | *`clang_rt.asan_dynamic-{arch}`*, *`clang_rt.asan_dynamic_runtime_thunk-{arch}`* |
| MT | EXE | YES | *`clang_rt.asan_dbg-{arch}`*, *`clang_rt.asan_dbg_cxx-{arch}`* |
| MT | DLL | YES | *`clang_rt.asan_dbg_dll_thunk-{arch}`* |
| MD | 要么 | YES | *`clang_rt.asan_dbg_dynamic-{arch}`*, *`clang_rt.asan_dbg_dynamic_runtime_thunk-{arch}`* |

在编译时 `cl /fsanitize=address` ，编译器将生成说明以管理和检查 [阴影字节](./asan-shadow-bytes.md)。 程序使用此检测来检查堆栈、堆或全局范围内的内存访问。 编译器还生成描述 stack 和全局变量的元数据。 此元数据使运行时能够生成精确的错误诊断：源代码中的函数名、行和列。 组合，如果在运行时遇到多种类型的 [内存安全错误](./asan-error-examples.md) ，编译器检查和运行时库可以精确诊断它们。

## <a name="function-interception"></a>函数截获

AddressSanitizer 实现函数通过许多热修补技术进行侦听。 这些技术在 [源代码本身中进行了最佳记录](https://github.com/llvm/llvm-project/blob/1a2eaebc09c6a200f93b8beb37130c8b8aab3934/compiler-rt/lib/interception/interception_win.cpp#L11)。

运行时库截取许多常见内存管理和内存操作函数。 有关列表，请参阅 [AddressSanitizer 列表](#intercepted_functions)。 分配侦听器管理与每个分配调用相关的元数据和影子字节。 每次调用 CRT 函数（如 `malloc` 或 `delete` ）时，侦听器会在 AddressSanitizer 影子内存区域中设置特定的值，以指示这些堆集当前是否可访问以及分配的界限。 这些影子字节允许编译器生成的 [阴影字节](./asan-shadow-bytes.md) 检查，以确定负载或存储是否有效。

不保证拦截成功。 如果函数序言太短而 `jmp` 无法写入，则截获可能会失败。 如果发生侦听失败，程序将引发 `debugbreak` 并暂停。 如果附加调试器，它会使拦截问题的原因变得清楚。 如果遇到此问题，请 [报告一个错误](https://aka.ms/feedback/report?space=62)。

> [!NOTE]
> 用户可以选择通过将环境变量设置为任何值，尝试继续越过失败的截取 `ASAN_WIN_CONTINUE_ON_INTERCEPTION_FAILURE` 。 如果继续，拦截失败可能会导致该函数缺少 bug 报告。

## <a name="custom-allocators-and-the-addresssanitizer-runtime"></a>自定义分配器和 AddressSanitizer 运行时

AddressSanitizer 运行时 `malloc` / `free` `new` / `delete` `HeapAlloc` / `HeapFree` 通过 `RtlAllocateHeap` / `RtlFreeHeap`) 为公用分配器接口、、 (提供侦听器。 许多程序使用自定义分配器的原因有一种，另一个示例是使用 `dlmalloc` 接口和解决方案的任何程序 `std::allocator` `VirtualAlloc()` 。 编译器无法将影子内存管理调用自动添加到自定义分配器。 用户需要负责使用 [提供的手动中毒接口](#poisoning)。 此 API 使这些分配器能够正常地与现有的 AddressSanitizer 运行时和 [影子字节](./asan-shadow-bytes.md) 约定一起工作。

## <a name="manual-addresssanitizer-poisoning-interface"></a><a name="poisoning"></a> 手动 AddressSanitizer 中毒接口

有启迪作用的接口很简单，但它对用户施加了对齐限制。 用户可以通过导入来导入这些原型 [`sanitizer/asan_interface.h`](https://github.com/llvm/llvm-project/blob/main/compiler-rt/include/sanitizer/asan_interface.h) 。 下面是接口函数原型：

```cpp
void __asan_poison_memory_region(void const volatile *addr, size_t size);
void __asan_unpoison_memory_region(void const volatile *addr, size_t size);
```

为方便起见， [AddressSanitizer 接口头文件](https://github.com/llvm/llvm-project/blob/main/compiler-rt/include/sanitizer/asan_interface.h) 提供了包装宏。 这些宏检查是否在编译期间启用了 AddressSanitizer 功能。 它们允许您的源代码在不需要时省略中毒函数调用。 应首选这些宏，而不是直接调用上述函数：

```cpp
#define ASAN_POISON_MEMORY_REGION(addr, size)
#define ASAN_UNPOISON_MEMORY_REGION(addr, size)
```

## <a name="alignment-requirements-for-addresssanitizer-poisoning"></a>AddressSanitizer 中毒的对齐要求

任何手动破坏影子字节都必须考虑对齐要求。 用户必须在必要时添加填充，以便阴影字节在影子内存中以字节边界结束。 AddressSanitizer 影子内存中的每个位对应用程序内存中的单个字节的状态进行编码。 此编码意味着每个分配的总大小（包括任何填充）必须与8字节边界对齐。 如果不满足对齐要求，则可能导致错误报告错误。 不正确的报告可能会表现为缺少的报告 (误报) 或报告非错误 (误报) 。

有关对齐要求和潜在问题的说明，请参阅提供的 [ASan 对齐示例](https://github.com/mcgov/asan_alignment_example)。 一个是小程序，用于显示手动隐藏内存损坏时可能会出现的错误。 第二种是使用接口进行手动破坏的示例实现 `std::allocator` 。

## <a name="run-time-options"></a>运行时选项

Microsoft C/c + + (MSVC) 使用基于 [llvm 项目存储库中的 Clang AddressSanitizer 运行](https://github.com/llvm/llvm-project)时的运行时。 因此，大多数运行时选项在两个版本之间共享。 [此处提供公共 Clang 运行时选项的完整列表](https://github.com/google/sanitizers/wiki/SanitizerCommonFlags)。 我们记录了以下部分中的一些差异。 如果发现不能按预期方式工作的选项，请 [报告一个 bug](https://aka.ms/feedback/report?space=62)。

### <a name="unsupported-addresssanitizer-options"></a>不支持的 AddressSanitizer 选项

- detect_container_overflow
- unmap_shadow_on_exit

> [!NOTE]
> AddressSanitizer 运行时选项 `halt_on_error` 无法按预期方式工作。 在 Clang 和 MSVC 运行时库中，许多错误类型都被视为 **不可** 持续性，包括大多数内存损坏错误。

有关详细信息，请参阅 [与 Clang 12.0 的差异](./asan.md#differences) 部分。

### <a name="msvc-specific-addresssanitizer-runtime-options"></a>MSVC 特定的 AddressSanitizer 运行时选项

- `windows_hook_legacy_allocators` 布尔值，设置为 `true` 以启用 [`GlobalAlloc`](/windows/win32/api/winbase/nf-winbase-globalalloc) 和分配器的侦听 [`LocalAlloc`](/windows/win32/api/winbase/nf-winbase-localalloc) 。

> [!NOTE]
> `windows_hook_legacy_allocators`编写本文时，此选项在公共 llvm 运行时中不可用。 该选项最终可能会被退回到公共项目;不过，它依赖于代码评审和社区接受。
>
> `windows_hook_rtl_allocators`默认情况下，在 AddressSanitizer 为试验时，该选项为 "选择加入" 功能。

## <a name="addresssanitizer-list-of-intercepted-functions-windows"></a><a name="intercepted_functions"></a> AddressSanitizer Windows)  (截获的函数的列表

AddressSanitizer 运行时热修补了许多功能，可在运行时启用内存安全检查。 下面是 AddressSanitizer 运行时监视的函数的非详尽列表。

### <a name="default-interceptors"></a>默认侦听器

- [`__C_specific_handler` 仅 (x64) ](/windows/win32/devnotes/--c-specific-handler2)
- [`_aligned_free`](../c-runtime-library/reference/aligned-free.md)
- [`_aligned_malloc`](../c-runtime-library/reference/aligned-malloc.md)
- [`_aligned_msize`](../c-runtime-library/reference/aligned-msize.md)
- [`_aligned_realloc`](../c-runtime-library/reference/aligned-realloc.md)
- [`_calloc_base`](../c-runtime-library/reference/calloc.md)
- [`_calloc_crt`](../c-runtime-library/reference/calloc.md)
- [`_calloc_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/calloc-dbg.md)
- [`_except_handler3` 仅 (x86) ](../c-runtime-library/except-handler3.md)
- [ `_except_handler4` 仅 (x86) ](../c-runtime-library/internal-crt-globals-and-functions.md) (未记录) 
- [`_expand`](../c-runtime-library/reference/expand.md)
- `_expand_base` (未记录的) 
- [`_expand_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/expand-dbg.md)
- [`_free_base`](../c-runtime-library/internal-crt-globals-and-functions.md) (未记录的) 
- [`_free_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/free-dbg.md)
- [`_malloc_base`](../c-runtime-library/internal-crt-globals-and-functions.md) (未记录的) 
- `_malloc_crt` (未记录的) 
- [`_malloc_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/malloc-dbg.md)
- [`_msize`](../c-runtime-library/reference/msize.md)
- `_msize_base` (未记录的) 
- [`_msize_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/msize-dbg.md)
- [`_realloc_base`](../c-runtime-library/internal-crt-globals-and-functions.md) (未记录的) 
- `_realloc_crt` (未记录的) 
- [`_realloc_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/realloc-dbg.md)
- [`_recalloc`](../c-runtime-library/reference/recalloc.md)
- `_recalloc_base` (未记录的) 
- `_recalloc_crt` (未记录的) 
- [`_recalloc_dbg` 仅 (调试运行时) ](../c-runtime-library/reference/recalloc-dbg.md)
- [`_strdup`](../c-runtime-library/reference/strdup-wcsdup-mbsdup.md)
- [`atoi`](../c-runtime-library/reference/atoi-atoi-l-wtoi-wtoi-l.md)
- [`atol`](../c-runtime-library/reference/atoi-atoi-l-wtoi-wtoi-l.md)
- [`calloc`](../c-runtime-library/reference/calloc.md)
- [`CreateThread`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread)
- [`free`](../c-runtime-library/reference/free.md)
- [`frexp`](../c-runtime-library/reference/frexp.md)
- [`longjmp`](../c-runtime-library/reference/longjmp.md)
- [`malloc`](../c-runtime-library/reference/malloc.md)
- [`memchr`](../c-runtime-library/reference/memchr-wmemchr.md)
- [`memcmp`](../c-runtime-library/reference/memcmp-wmemcmp.md)
- [`memcpy`](../c-runtime-library/reference/memcpy-wmemcpy.md)
- [`memmove`](../c-runtime-library/reference/memmove-wmemmove.md)
- [`memset`](../c-runtime-library/reference/memset-wmemset.md)
- [`RaiseException`](/windows/win32/api/errhandlingapi/nf-errhandlingapi-raiseexception)
- [`realloc`](../c-runtime-library/reference/realloc.md)
- [`RtlAllocateHeap`](/windows-hardware/drivers/ddi/ntifs/nf-ntifs-rtlallocateheap)
- [`RtlCreateHeap`](/windows-hardware/drivers/ddi/ntifs/nf-ntifs-rtlcreateheap)
- [`RtlDestroyHeap`](/windows-hardware/drivers/ddi/ntifs/nf-ntifs-rtlcreateheap)
- [`RtlFreeHeap`](/windows-hardware/drivers/ddi/ntifs/nf-ntifs-rtlfreeheap)
- [`RtlRaiseException`](/windows/win32/api/rtlsupportapi/nf-rtlsupportapi-rtlraiseexception)
- `RtlReAllocateHeap` (未记录的) 
- `RtlSizeHeap` (未记录的) 
- [`SetUnhandledExceptionFilter`](/windows/win32/api/errhandlingapi/nf-errhandlingapi-setunhandledexceptionfilter)
- [`strcat`](../c-runtime-library/reference/strcat-wcscat-mbscat.md)
- [`strchr`](../c-runtime-library/reference/strchr-wcschr-mbschr-mbschr-l.md)
- [`strcmp`](../c-runtime-library/reference/strcmp-wcscmp-mbscmp.md)
- [`strcpy`](../c-runtime-library/reference/strcpy-wcscpy-mbscpy.md)
- [`strcspn`](../c-runtime-library/reference/strcspn-wcscspn-mbscspn-mbscspn-l.md)
- [`strdup`](../c-runtime-library/reference/strdup-wcsdup.md)
- [`strlen`](../c-runtime-library/reference/strlen-wcslen-mbslen-mbslen-l-mbstrlen-mbstrlen-l.md)
- [`strncat`](../c-runtime-library/reference/strncat-strncat-l-wcsncat-wcsncat-l-mbsncat-mbsncat-l.md)
- [`strncmp`](../c-runtime-library/reference/strncmp-wcsncmp-mbsncmp-mbsncmp-l.md)
- [`strncpy`](../c-runtime-library/reference/strncpy-strncpy-l-wcsncpy-wcsncpy-l-mbsncpy-mbsncpy-l.md)
- [`strnlen`](../c-runtime-library/reference/strnlen-strnlen-s.md)
- [`strpbrk`](../c-runtime-library/reference/strpbrk-wcspbrk-mbspbrk-mbspbrk-l.md)
- [`strspn`](../c-runtime-library/reference/strspn-wcsspn-mbsspn-mbsspn-l.md)
- [`strstr`](../c-runtime-library/reference/strstr-wcsstr-mbsstr-mbsstr-l.md)
- [`strtok`](../c-runtime-library/reference/strtok-strtok-l-wcstok-wcstok-l-mbstok-mbstok-l.md)
- [`strtol`](../c-runtime-library/reference/strtol-wcstol-strtol-l-wcstol-l.md)
- [`wcslen`](../c-runtime-library/reference/strlen-wcslen-mbslen-mbslen-l-mbstrlen-mbstrlen-l.md)
- [`wcsnlen`](../c-runtime-library/reference/strnlen-strnlen-s.md)

### <a name="optional-interceptors"></a>可选侦听器

此处列出的侦听器仅当启用了 AddressSanitizer 运行时选项时才会安装。 设置 `windows_hook_legacy_allocators` 为 `true` 以启用旧分配器侦听。
`set ASAN_OPTIONS=windows_hook_legacy_allocators=true`

- [`GlobalAlloc`](/windows/win32/api/winbase/nf-winbase-globalalloc)
- [`GlobalFree`](/windows/win32/api/winbase/nf-winbase-GlobalFree)
- [`GlobalHandle`](/windows/win32/api/winbase/nf-winbase-GlobalHandle)
- [`GlobalLock`](/windows/win32/api/winbase/nf-winbase-GlobalLock)
- [`GlobalReAlloc`](/windows/win32/api/winbase/nf-winbase-GlobalReAlloc)
- [`GlobalSize`](/windows/win32/api/winbase/nf-winbase-GlobalSize)
- [`GlobalUnlock`](/windows/win32/api/winbase/nf-winbase-GlobalUnlock)
- [`LocalAlloc`](/windows/win32/api/winbase/nf-winbase-LocalAlloc)
- [`LocalFree`](/windows/win32/api/winbase/nf-winbase-LocalFree)
- [`LocalHandle`](/windows/win32/api/winbase/nf-winbase-LocalHandle)
- [`LocalLock`](/windows/win32/api/winbase/nf-winbase-LocalLock)
- [`LocalReAlloc`](/windows/win32/api/winbase/nf-winbase-LocalReAlloc)
- [`LocalSize`](/windows/win32/api/winbase/nf-winbase-LocalSize)
- [`LocalUnlock`](/windows/win32/api/winbase/nf-winbase-LocalUnlock)

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
