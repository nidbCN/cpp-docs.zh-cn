---
title: 预定义宏
description: 列出并说明 Microsoft C++ 编译器预定义预处理器宏。
ms.custom: update_every_version
ms.date: 12/14/2020
f1_keywords:
- _ATL_VER
- __ATOM__
- __AVX__
- __AVX2__
- __AVX512BW__
- __AVX512CD__
- __AVX512DQ__
- __AVX512F__
- __AVX512VL__
- _CHAR_UNSIGNED
- __CLR_VER
- _CONTROL_FLOW_GUARD
- __COUNTER__
- __cplusplus
- __cplusplus_cli
- __cplusplus_winrt
- _CPPRTTI
- _CPPUNWIND
- __DATE__
- _DEBUG
- _DLL
- __FILE__
- __FUNCDNAME__
- __FUNCSIG__
- __FUNCTION__
- _INTEGRAL_MAX_BITS
- _ISO_VOLATILE
- _KERNEL_MODE
- __LINE__
- _M_AMD64
- _M_ARM
- _M_ARM_ARMV7VE
- _M_ARM_FP
- _M_ARM64
- _M_CEE
- _M_CEE_PURE
- _M_CEE_SAFE
- _M_FP_EXCEPT
- _M_FP_FAST
- _M_FP_PRECISE
- _M_FP_STRICT
- _M_IX86
- _M_IX86_FP
- _M_X64
- _MANAGED
- _MFC_VER
- _MSC_BUILD
- _MSC_EXTENSIONS
- _MSC_FULL_VER
- _MSC_VER
- _MSVC_LANG
- __MSVC_RUNTIME_CHECKS
- _MT
- _NATIVE_WCHAR_T_DEFINED
- _NO_SIZED_DEALLOCATION
- _OPENMP
- _PREFAST_
- _RESUMABLE_FUNCTIONS_SUPPORTED
- _RTC_CONVERSION_CHECKS_ENABLED
- __STDC__
- __STDC_HOSTED__
- __STDC_NO_ATOMICS__
- __STDC_NO_COMPLEX__
- __STDC_NO_THREADS__
- __STDC_NO_VLA__
- __STDC_VERSION__
- __STDCPP_THREADS__
- __TIME__
- __TIMESTAMP__
- __VA_ARGS__
- _VC_NODEFAULTLIB
- _WCHAR_T_DEFINED
- _WIN32
- _WIN64
- _WINRT_DLL
helpviewer_keywords:
- timestamps, preprocessor macro
- cl.exe compiler, version number
- version numbers, C/C++ compiler (cl.exe)
- macros, predefined C++
- preprocessor, macros
- predefined macros
- _ATL_VER macro
- __ATOM__ macro
- __AVX__ macro
- __AVX2__ macro
- __AVX512BW__ macro
- __AVX512CD__ macro
- __AVX512DQ__ macro
- __AVX512F__ macro
- __AVX512VL__ macro
- _CHAR_UNSIGNED macro
- __CLR_VER macro
- _CONTROL_FLOW_GUARD macro
- __COUNTER__ macro
- __cplusplus macro
- __cplusplus_cli macro
- __cplusplus_winrt macro
- _CPPRTTI macro
- _CPPUNWIND macro
- __DATE__ macro
- _DEBUG macro
- _DLL macro
- __FILE__ macro
- __FUNCDNAME__ macro
- __FUNCSIG__ macro
- __FUNCTION__ macro
- _INTEGRAL_MAX_BITS macro
- _ISO_VOLATILE macro
- _KERNEL_MODE macro
- __LINE__ macro
- _M_AMD64 macro
- _M_ARM macro
- _M_ARM_ARMV7VE macro
- _M_ARM_FP macro
- _M_ARM64 macro
- _M_CEE macro
- _M_CEE_PURE macro
- _M_CEE_SAFE macro
- _M_FP_EXCEPT macro
- _M_FP_FAST macro
- _M_FP_PRECISE macro
- _M_FP_STRICT macro
- _M_IX86 macro
- _M_IX86_FP macro
- _M_X64 macro
- _MANAGED macro
- _MFC_VER macro
- _MSC_BUILD macro
- _MSC_EXTENSIONS macro
- _MSC_FULL_VER macro
- _MSC_VER macro
- _MSVC_LANG macro
- __MSVC_RUNTIME_CHECKS macro
- _MT macro
- _NATIVE_WCHAR_T_DEFINED macro
- _NO_SIZED_DEALLOCATION macro
- _OPENMP macro
- _PREFAST_ macro
- _RESUMABLE_FUNCTIONS_SUPPORTED macro
- _RTC_CONVERSION_CHECKS_ENABLED macro
- __STDC__ macro
- __STDC_HOSTED__ macro
- __STDC_NO_ATOMICS__ macro
- __STDC_NO_COMPLEX__ macro
- __STDC_NO_THREADS__ macro
- __STDC_NO_VLA__ macro
- __STDC_VERSION__ macro
- __STDCPP_THREADS__ macro
- __TIME__ macro
- __TIMESTAMP__ macro
- __VA_ARGS__ macro
- _VC_NODEFAULTLIB macro
- _WCHAR_T_DEFINED macro
- _WIN32 macro
- _WIN64 macro
- _WINRT_DLL macro
- __func__ identifier
ms.assetid: 1cc5f70a-a225-469c-aed0-fe766238e23f
no-loc:
- _ATL_VER
- __ATOM__
- __AVX__
- __AVX2__
- __AVX512BW__
- __AVX512CD__
- __AVX512DQ__
- __AVX512F__
- __AVX512VL__
- _CHAR_UNSIGNED
- __CLR_VER
- _CONTROL_FLOW_GUARD
- __COUNTER__
- __cplusplus
- __cplusplus_cli
- __cplusplus_winrt
- _CPPRTTI
- _CPPUNWIND
- __DATE__
- _DEBUG
- _DLL
- __FILE__
- __FUNCDNAME__
- __FUNCSIG__
- __FUNCTION__
- _INTEGRAL_MAX_BITS
- _ISO_VOLATILE
- _KERNEL_MODE
- __LINE__
- _M_AMD64
- _M_ARM
- _M_ARM_ARMV7VE
- _M_ARM_FP
- _M_ARM64
- _M_CEE
- _M_CEE_PURE
- _M_CEE_SAFE
- _M_FP_EXCEPT
- _M_FP_FAST
- _M_FP_PRECISE
- _M_FP_STRICT
- _M_IX86
- _M_IX86_FP
- _M_X64
- _MANAGED
- _MFC_VER
- _MSC_BUILD
- _MSC_EXTENSIONS
- _MSC_FULL_VER
- _MSC_VER
- _MSVC_LANG
- __MSVC_RUNTIME_CHECKS
- _MT
- _NATIVE_WCHAR_T_DEFINED
- _NO_SIZED_DEALLOCATION
- _OPENMP
- _PREFAST_
- _RESUMABLE_FUNCTIONS_SUPPORTED
- _RTC_CONVERSION_CHECKS_ENABLED
- __STDC__
- __STDC_HOSTED__
- __STDC_NO_ATOMICS__
- __STDC_NO_COMPLEX__
- __STDC_NO_THREADS__
- __STDC_NO_VLA__
- __STDC_VERSION__
- __STDCPP_THREADS__
- __TIME__
- __TIMESTAMP__
- __VA_ARGS__
- _VC_NODEFAULTLIB
- _WCHAR_T_DEFINED
- _WIN32
- _WIN64
- _WINRT_DLL
- __func__
ms.openlocfilehash: 3e0dbda7ddcd7c428e0f83895d9ed5d4922a7ccb
ms.sourcegitcommit: bf6d8a220f6392f1f19c0c0605d1467d0221ef6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551756"
---
# <a name="predefined-macros"></a>预定义宏

Microsoft C/C++ 编译器 (MSVC) 根据语言（C 或 C++）、编译目标和所选编译器选项预定义某些预处理器宏。

MSVC 支持 ANSI/ISO C99、C11 和 C17 标准以及 ISO C++14 和 C++17 标准要求的预定义预处理器宏。 该实现还支持多个 Microsoft 专用预处理器宏。 部分宏仅针对特定生成环境或编译器选项定义。 除非另行说明，否则这些宏的定义范围适用于整个翻译单元，就像将它们指定为 `/D` 编译器选项参数一样。 定义后，预处理器先将这些宏展开为指定的值。然后再编译。 预定义宏不带参数，不能重新定义。

## <a name="standard-predefined-identifier"></a>标准预定义标识符

编译器支持 ISO C99 和 ISO C++11 指定的以下预定义标识符。

- `__func__`：封闭函数（用作 `char` 的函数局部 static const 数组）的未限定、未修饰名称。

    ```cpp
    void example(){
        printf("%s\n", __func__);
    } // prints "example"
    ```

## <a name="standard-predefined-macros"></a>标准预定义宏

编译器支持 ISO C99、C11、C17 和 ISO C++17 标准指定的以下预定义宏：

- `__cplusplus`：当翻译单元编译为 C++ 时，定义为整数文本值。 其他情况下则不定义。

- `__DATE__`：当前源文件的编译日期。 日期是 Mmm dd yyyy 格式的恒定长度字符串文本  。 月份名 Mmm 与 C 运行时库 (CRT) [asctime](../c-runtime-library/reference/asctime-wasctime.md) 函数生成的缩写月份名相同  。 如果值小于 10，则日期 dd 的第一个字符为空格  。 任何情况下都会定义此宏。

- `__FILE__`：当前源文件的名称。 `__FILE__` 展开为字符型字符串文本。 要确保显示文件的完整路径，请使用 [`/FC`（诊断中源代码文件的完整路径）](../build/reference/fc-full-path-of-source-code-file-in-diagnostics.md)。 任何情况下都会定义此宏。

- `__LINE__`：定义为当前源文件中的整数行号。 可使用 `#line` 指令来更改 `__LINE__` 宏的值。 任何情况下都会定义此宏。

- `__STDC__` 仅在编译为 C，并且指定了 编译器选项时，定义为 1[`/Za`](../build/reference/za-ze-disable-language-extensions.md)。 其他情况下则不定义。

- `__STDC_HOSTED__`：如果实现是托管实现并且支持整个必需的标准库，则定义为 1  。 其他情况下则定义为 0。

- `__STDC_NO_ATOMICS__` 如果实现不支持可选的标准原子，则定义为 1。 当编译为 C 且指定 [`/std`](../build/reference/std-specify-language-standard-version.md) C11 或 C17 选项之一时，MSVC 实现会将其定义为 1。

- `__STDC_NO_COMPLEX__` 如果实现不支持可选的标准复数，则定义为 1。 当编译为 C 且指定 [`/std`](../build/reference/std-specify-language-standard-version.md) C11 或 C17 选项之一时，MSVC 实现会将其定义为 1。

- `__STDC_NO_THREADS__` 如果实现不支持可选的标准线程，则定义为 1。 当编译为 C 且指定 [`/std`](../build/reference/std-specify-language-standard-version.md) C11 或 C17 选项之一时，MSVC 实现会将其定义为 1。

- `__STDC_NO_VLA__` 如果实现不支持可选的可变长度数组，则定义为 1。 当编译为 C 且指定 [`/std`](../build/reference/std-specify-language-standard-version.md) C11 或 C17 选项之一时，MSVC 实现会将其定义为 1。

- `__STDC_VERSION__` 当编译为 C 且指定 `/std` C11 或 C17 选项之一时定义。 对于 [`/std:c11`](../build/reference/std-specify-language-standard-version.md)，它扩展到 `201112L`；对于 [`/std:c17`](../build/reference/std-specify-language-standard-version.md)，则扩展到 `201710L`。

- `__STDCPP_THREADS__`：当且仅当程序可以有多个执行线程并编译为 C++ 时，定义为 1。 其他情况下则不定义。

- `__TIME__`：预处理翻译单元的翻译时间。 时间是 hh:mm:ss 格式的字符型字符串文本，与 CRT [asctime](../c-runtime-library/reference/asctime-wasctime.md) 函数返回的时间相同  。 任何情况下都会定义此宏。

## <a name="microsoft-specific-predefined-macros"></a>Microsoft 专用预定义宏

MSVC 支持以下其他预定义宏。

- `__ATOM__` 当设置了 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1[`/favor:ATOM`](../build/reference/favor-optimize-for-architecture-specifics.md)。 其他情况下则不定义。

- `__AVX__`：当设置了 [`/arch:AVX`](../build/reference/arch-x86.md)、[`/arch:AVX2`](../build/reference/arch-x86.md) 或 [`/arch:AVX512`](../build/reference/arch-x86.md) 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1。 其他情况下则不定义。

- `__AVX2__`：当设置了 [`/arch:AVX2`](../build/reference/arch-x86.md) 或 [`/arch:AVX512`](../build/reference/arch-x86.md) 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1。 其他情况下则不定义。

- `__AVX512BW__` 当设置了 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1[`/arch:AVX512`](../build/reference/arch-x86.md)。 其他情况下则不定义。

- `__AVX512CD__` 当设置了 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1[`/arch:AVX512`](../build/reference/arch-x86.md)。 其他情况下则不定义。

- `__AVX512DQ__` 当设置了 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1[`/arch:AVX512`](../build/reference/arch-x86.md)。 其他情况下则不定义。

- `__AVX512F__` 当设置了 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1[`/arch:AVX512`](../build/reference/arch-x86.md)。 其他情况下则不定义。

- `__AVX512VL__` 当设置了 编译器选项，并且编译器目标为 x86 或 x64 时，定义为 1[`/arch:AVX512`](../build/reference/arch-x86.md)。 其他情况下则不定义。

- `_CHAR_UNSIGNED`：如果默认 `char` 类型为无符号，则定义为 1。 当设置了 [`/J`（默认 char 类型为无符号）](../build/reference/j-default-char-type-is-unsigned.md)编译器选项时，会定义此值。 其他情况下则不定义。

- `__CLR_VER`：定义为整数文本，表示用于编译应用的公共语言运行时 (CLR) 的版本。 此值按 `Mmmbbbbb` 格式编码，其中 `M` 为运行时的主版本，`mm` 为运行时的次版本，`bbbbb` 为生成号。 如果设置了 编译器选项，则定义 `__CLR_VER`[`/clr`](../build/reference/clr-common-language-runtime-compilation.md)。 其他情况下则不定义。

    ```cpp
    // clr_ver.cpp
    // compile with: /clr
    using namespace System;
    int main() {
       Console::WriteLine(__CLR_VER);
    }
    ```

- `_CONTROL_FLOW_GUARD`：如果设置了 [`/guard:cf`（启用控制流防护）](../build/reference/guard-enable-control-flow-guard.md)编译器选项，则定义为 1。 其他情况下则不定义。

- `__COUNTER__`：展开为从 0 开始的整数文本。 每次在源文件或源文件的 included 头中使用时，此值都会递增 1。 当使用预编译头时，`__COUNTER__` 会记住其状态。 任何情况下都会定义此宏。

  此示例使用 `__COUNTER__` 来将唯一标识符分配到同一类型的三个不同对象。 `exampleClass` 构造函数采用整数作为参数。 在 `main` 中，应用程序声明类型为 `exampleClass` 的三个对象，并将 `__COUNTER__` 用作唯一标识符参数：

    ```cpp
    // macro__COUNTER__.cpp
    // Demonstration of __COUNTER__, assigns unique identifiers to
    // different objects of the same type.
    // Compile by using: cl /EHsc /W4 macro__COUNTER__.cpp
    #include <stdio.h>

    class exampleClass {
        int m_nID;
    public:
        // initialize object with a read-only unique ID
        exampleClass(int nID) : m_nID(nID) {}
        int GetID(void) { return m_nID; }
    };

    int main()
    {
        // __COUNTER__ is initially defined as 0
        exampleClass e1(__COUNTER__);

        // On the second reference, __COUNTER__ is now defined as 1
        exampleClass e2(__COUNTER__);

        // __COUNTER__ is now defined as 2
        exampleClass e3(__COUNTER__);

        printf("e1 ID: %i\n", e1.GetID());
        printf("e2 ID: %i\n", e2.GetID());
        printf("e3 ID: %i\n", e3.GetID());

        // Output
        // ------------------------------
        // e1 ID: 0
        // e2 ID: 1
        // e3 ID: 2

        return 0;
    }
    ```

- `__cplusplus_cli`：当编译为 C++ 并设置了 编译器选项时，定义为整数文本值 200406[`/clr`](../build/reference/clr-common-language-runtime-compilation.md)。 其他情况下则不定义。 定义后，`__cplusplus_cli` 的效力范围是整个翻译单元。

    ```cpp
    // cplusplus_cli.cpp
    // compile by using /clr
    #include "stdio.h"
    int main() {
       #ifdef __cplusplus_cli
          printf("%d\n", __cplusplus_cli);
       #else
          printf("not defined\n");
       #endif
    }
    ```

- `__cplusplus_winrt`：当编译为 C++ 并设置了[`/ZW`（Windows 运行时编译）](../build/reference/zw-windows-runtime-compilation.md)编译器选项时，定义为整数文本值 201009。 其他情况下则不定义。

- `_CPPRTTI`：如果设置了 [`/GR`（启用运行时类型信息）](../build/reference/gr-enable-run-time-type-information.md)编译器选项，则定义为 1。 其他情况下则不定义。

- `_CPPUNWIND`：如果设置了一个或多个 [`/GX`（启用异常处理）](../build/reference/gx-enable-exception-handling.md)、[`/clr`（公共语言运行时编译）](../build/reference/clr-common-language-runtime-compilation.md)或 [`/EH`（异常处理模型）](../build/reference/eh-exception-handling-model.md)编译器选项，则定义为 1  。 其他情况下则不定义。

- `_DEBUG`：当设置了 [`/LDd`](../build/reference/md-mt-ld-use-run-time-library.md)、[`/MDd`](../build/reference/md-mt-ld-use-run-time-library.md) 或 [`/MTd`](../build/reference/md-mt-ld-use-run-time-library.md) 编译器选项时，定义为 1。 其他情况下则不定义。

- `_DLL`：当设置了 [`/MD`](../build/reference/md-mt-ld-use-run-time-library.md) 或 [`/MDd`](../build/reference/md-mt-ld-use-run-time-library.md)（多线程 DLL）编译器选项时，定义为 1。 其他情况下则不定义。

- `__FUNCDNAME__`：定义为包含封闭函数[修饰名](../build/reference/decorated-names.md)的字符串文本。 此宏仅在函数中定义。 如果使用 [`/EP`](../build/reference/ep-preprocess-to-stdout-without-hash-line-directives.md) 或 [`/P`](../build/reference/p-preprocess-to-a-file.md) 编译器选项，则不会展开 `__FUNCDNAME__` 宏。

   此示例使用 `__FUNCDNAME__`、`__FUNCSIG__` 和 `__FUNCTION__` 宏来显示函数信息。

   [!code-cpp[NVC_Predefined_Macros_Examples#1](../preprocessor/codesnippet/CPP/predefined-macros_1.cpp)]

- `__FUNCSIG__`：定义为包含封闭函数签名的字符串文本。 此宏仅在函数中定义。 如果使用 [`/EP`](../build/reference/ep-preprocess-to-stdout-without-hash-line-directives.md) 或 [`/P`](../build/reference/p-preprocess-to-a-file.md) 编译器选项，则不会展开 `__FUNCSIG__` 宏。 当针对 64 位目标进行编译时，调用约定默认为 `__cdecl`。 有关用法的示例，请参阅 `__FUNCDNAME__` 宏。

- `__FUNCTION__`：定义为包含封闭函数未修饰名的字符串文本。 此宏仅在函数中定义。 如果使用 [`/EP`](../build/reference/ep-preprocess-to-stdout-without-hash-line-directives.md) 或 [`/P`](../build/reference/p-preprocess-to-a-file.md) 编译器选项，则不会展开 `__FUNCTION__` 宏。 有关用法的示例，请参阅 `__FUNCDNAME__` 宏。

- `_INTEGRAL_MAX_BITS`：定义为整数文本值 64，表示非矢量整型类型的最大大小（位）。 任何情况下都会定义此宏。

   ```cpp
   // integral_max_bits.cpp
   #include <stdio.h>
   int main() {
      printf("%d\n", _INTEGRAL_MAX_BITS);
   }
   ```

- `__INTELLISENSE__`：当在 Visual Studio IDE 中传递 IntelliSense 编译器时，定义为 1。 其他情况下则不定义。 可使用此宏来保护 IntelliSense 编译器不理解的代码，也可使用它在生成和 IntelliSense 编译器之间进行切换。 有关详细信息，请参阅 [IntelliSense 缓慢疑难解答提示](https://devblogs.microsoft.com/cppblog/troubleshooting-tips-for-intellisense-slowness/)。

- `_ISO_VOLATILE`：如果设置了 [`/volatile:iso`](../build/reference/volatile-volatile-keyword-interpretation.md) 编译器选项，则定义为 1。 其他情况下则不定义。

- `_KERNEL_MODE`：如果设置了 [`/kernel`（创建内核模式二进制）](../build/reference/kernel-create-kernel-mode-binary.md)编译器选项，则定义为 1。 其他情况下则不定义。

- `_M_AMD64`：为面向 x64 处理器的编译定义为整数文本值 100。 其他情况下则不定义。

- `_M_ARM`：为面向 ARM 处理器的编译定义为整数文本值 7。 其他情况下则不定义。

- `_M_ARM_ARMV7VE`：如果为面向 ARM 处理器的编译设置了 编译器选项，则定义为 1[`/arch:ARMv7VE`](../build/reference/arch-arm.md)。 其他情况下则不定义。

- `_M_ARM_FP`：定义为整数文本值，表示为面向 ARM 处理器的编译设置的 编译器选项[`/arch`](../build/reference/arch-arm.md)。 其他情况下则不定义。

  - 如果未指定 `/arch` ARM 选项，则该值在 30-39 范围内，表示为 ARM 设置了默认体系结构 (`VFPv3`)。

  - 如果设置了 `/arch:VFPv4`，则该值在 40-49 范围内。

  - 有关详细信息，请参阅 [`/arch` (ARM)](../build/reference/arch-arm.md)。

- `_M_ARM64`：为面向 64 位 ARM 处理器的编译定义为 1。 其他情况下则不定义。

- `_M_CEE`：如果设置了任何 [`/clr`（公共语言运行时编译）](../build/reference/clr-common-language-runtime-compilation.md)编译器选项，则定义为 001。 其他情况下则不定义。

- `_M_CEE_PURE`：从 Visual Studio 2015 开始弃用。 如果设置了 [`/clr:pure`](../build/reference/clr-common-language-runtime-compilation.md) 编译器选项，则定义为 001。 其他情况下则不定义。

- `_M_CEE_SAFE`：从 Visual Studio 2015 开始弃用。 如果设置了 [`/clr:safe`](../build/reference/clr-common-language-runtime-compilation.md) 编译器选项，则定义为 001。 其他情况下则不定义。

- `_M_FP_EXCEPT`：如果设置了 [`/fp:except`](../build/reference/fp-specify-floating-point-behavior.md) 或 [`/fp:strict`](../build/reference/fp-specify-floating-point-behavior.md) 编译器选项，则定义为 1。 其他情况下则不定义。

- `_M_FP_FAST`：如果设置了 [`/fp:fast`](../build/reference/fp-specify-floating-point-behavior.md) 编译器选项，则定义为 1。 其他情况下则不定义。

- `_M_FP_PRECISE`：如果设置了 [`/fp:precise`](../build/reference/fp-specify-floating-point-behavior.md) 编译器选项，则定义为 1。 其他情况下则不定义。

- `_M_FP_STRICT`：如果设置了 [`/fp:strict`](../build/reference/fp-specify-floating-point-behavior.md) 编译器选项，则定义为 1。 其他情况下则不定义。

- `_M_IX86`：为面向 x86 处理器的编译定义为整数文本值 600。 对于面向 x64 或 ARM 处理器的编译，则不定义此宏。

- `_M_IX86_FP`：定义为表示设置了 [`/arch`](../build/reference/arch-arm.md) 编译器选项的整数文本值或默认值。 只要编译目标为 x86 处理器，就会定义此宏。 其他情况下则不定义。 定义后：

  - 如果设置了 `/arch:IA32` 编译器选项，则值为 0。

  - 如果设置了 `/arch:SSE` 编译器选项，则值为 1。

  - 如果设置了 `/arch:SSE2`、`/arch:AVX`、`/arch:AVX2` 或 `/arch:AVX512` 编译器选项，则值为 2。 如果未指定 `/arch` 编译器选项，则为默认值。 当指定了 `/arch:AVX` 时，还会定义宏 `__AVX__`。 当指定了 `/arch:AVX2` 时，还会定义 `__AVX__` 和 `__AVX2__`。 当指定了 `/arch:AVX512` 时，还会定义 `__AVX__`、`__AVX2__`、`__AVX512BW__`、`__AVX512CD__`、`__AVX512DQ__`、`__AVX512F__` 和 `__AVX512VL__`。

  - 有关详细信息，请参阅 [`/arch` (x86)](../build/reference/arch-x86.md)。

- `_M_X64`：为面向 x64 处理器的编译定义为整数文本值 100。 其他情况下则不定义。

- `_MANAGED`：当设置了 [`/clr`](../build/reference/clr-common-language-runtime-compilation.md) 编译器选项时，定义为 1。 其他情况下则不定义。

- `_MSC_BUILD`：定义为包含编译器版本号修订号元素的整数文本。 修订号是用句点分隔的版本号的第四个元素。 例如，如果 Microsoft C/C++ 编译器的版本号为 15.00.20706.01，则 `_MSC_BUILD` 宏计算结果为 1。 任何情况下都会定义此宏。

- `_MSC_EXTENSIONS`：如果设置了默认 [`/Ze`（启用语言扩展）](../build/reference/za-ze-disable-language-extensions.md)编译器选项，则定义为 1。 其他情况下则不定义。

- `_MSC_FULL_VER`：定义为编码编译器版本号的主版本号、次版本号和生成号元素的整数文本。 主版本号是用句点分隔的版本号的第一个元素，次版本号是第二个元素，而生成号是第三个元素。 例如，如果 Microsoft C/C++ 编译器的版本号为 15.00.20706.01，则 `_MSC_FULL_VER` 宏计算结果为 150020706。 在命令行中键入 `cl /?`，查看编译器的版本号。 任何情况下都会定义此宏。

- `_MSC_VER`：定义为编码编译器版本号的主版本号和次版本号元素的整数文本。 主版本号是用句点分隔的版本号的第一个元素，而次版本号是第二个元素。 例如，如果 Microsoft C/C++ 编译器的版本号为 17.00.51106.1，则 `_MSC_VER` 宏计算结果为 1700。 在命令行中键入 `cl /?`，查看编译器的版本号。 任何情况下都会定义此宏。

   | Visual Studio 版本 | `_MSC_VER` |
   |--|--|
   | Visual Studio 6.0 | 1200 |
   | Visual Studio .NET 2002 (7.0) | 1300 |
   | Visual Studio .NET 2003 (7.1) | 1310 |
   | Visual Studio 2005 (8.0) | 1400 |
   | Visual Studio 2008 (9.0) | 1500 |
   | Visual Studio 2010 (10.0) | 1600 |
   | Visual Studio 2012 (11.0) | 1700 |
   | Visual Studio 2013 (12.0) | 1800 |
   | Visual Studio 2015 (14.0) | 1900 |
   | Visual Studio 2017 RTW (15.0) | 1910 |
   | Visual Studio 2017 版本 15.3 | 1911 |
   | Visual Studio 2017 版本 15.5 | 1912 |
   | Visual Studio 2017 版本 15.6 | 1913 |
   | Visual Studio 2017 15.7 版 | 1914 |
   | Visual Studio 2017 版本 15.8 | 1915 |
   | Visual Studio 2017 版本 15.9 | 1916 |
   | Visual Studio 2019 RTW (16.0) | 1920 |
   | Visual Studio 2019 版本 16.1 | 1921 |
   | Visual Studio 2019 版本 16.2 | 1922 |
   | Visual Studio 2019 版本 16.3 | 1923 |
   | Visual Studio 2019 版本 16.4 | 1924 |
   | Visual Studio 2019 版本 16.5 | 1925 |
   | Visual Studio 2019 版本 16.6 | 1926 |
   | Visual Studio 2019 版本 16.7 | 1927 |
   | Visual Studio 2019 v16.8、v16.9 | 1928 |
   | Visual Studio 2019 v16.10 预览版 | 1929 |

   要在指定的 Visual Studio 版本或更高版本中测试编译器版本或更新，请使用 `>=` 运算符。 可在条件指令中使用它来比较 `_MSC_VER` 与已知版本。 如果要比较多个互相排斥的版本，请按版本号的降序顺序进行比较。 例如，此代码将检查 Visual Studio 2017 和更高版本中发布的编译器。 接下来，它会检查 Visual Studio 2015 以及之后发布的编译器。 然后，它会检查 Visual Studio 2015 之前发布的所有编译器：

   ```cpp
   #if _MSC_VER >= 1910
   // . . .
   #elif _MSC_VER >= 1900
   // . . .
   #else
   // . . .
   #endif
   ```

   要测试共享主要和次要版本号的编译器版本，请使用 `_MSC_FULL_VER` 中的主要、次要和内部版本号进行比较。 Visual Studio 2019 v16.9 中的编译器的 `_MSC_FULL_VER` 值为 192829500 或更大。

   有关详细信息，请参阅 Microsoft C++ 团队博客中的 [Visual C++ 编译器版本](https://devblogs.microsoft.com/cppblog/visual-c-compiler-version/)。

- `_MSVC_LANG`：定义为指定编译器面向的 C++ 语言标准的整数文本。 此宏仅在编译为 C++ 的代码中设置。 默认情况下，或者当指定了 [`/std:c++14`](../build/reference/std-specify-language-standard-version.md) 编译器选项时，此宏为整数文本值 201402L。 如果指定了 [`/std:c++17`](../build/reference/std-specify-language-standard-version.md) 编译器选项，则此宏设置为 201703L。 当指定了 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 选项时，它设置为更高的未指定值。 其他情况下则不会定义此宏。 从 Visual Studio 2015 Update 3 开始，提供 `_MSVC_LANG` 宏和 [`/std`（指定语言标准版本）](../build/reference/std-specify-language-standard-version.md)编译器选项。

- `__MSVC_RUNTIME_CHECKS`：当设置了其中一个 [`/RTC`](../build/reference/rtc-run-time-error-checks.md) 编译器选项时，定义为 1。 其他情况下则不定义。

- `_MSVC_TRADITIONAL`:
    - 从 Visual Studio 2017 版本15.8 开始提供：当设置了预处理器一致性模式 [`/experimental:preprocessor`](../build/reference/experimental-preprocessor.md) 编译器选项，定义为 0。 默认情况下，或者当设置了 [`/experimental:preprocessor-`](../build/reference/experimental-preprocessor.md) 编译器选项时，定义为 1，指示正在使用传统预处理器。
    - 从 Visual Studio 2019 版本 16.5 开始提供：当设置了预处理器一致性模式 [`/Zc:preprocessor`](../build/reference/zc-preprocessor.md) 编译器选项，定义为 0。 默认情况下，或者当设置了 `/Zc:preprocessor-` 编译器选项时，定义为 1，指示正在使用传统预处理器（实质上，即 `/Zc:preprocessor` 取代了已弃用的 `/experimental:preprocessor`）。

   ```cpp
   #if defined(_MSVC_TRADITIONAL) && _MSVC_TRADITIONAL
   // Logic using the traditional preprocessor
   #else
   // Logic using cross-platform compatible preprocessor
   #endif
   ```

- `_MT`：当指定了 [`/MD` 或 `/MDd`（多线程 DLL）](../build/reference/md-mt-ld-use-run-time-library.md)，或者指定了 [`/MT` 或 `/MTd`（多线程）](../build/reference/md-mt-ld-use-run-time-library.md)时，定义为 1   。 其他情况下则不定义。

- `_NATIVE_WCHAR_T_DEFINED`：当设置了 [`/Zc:wchar_t`](../build/reference/zc-wchar-t-wchar-t-is-native-type.md) 编译器选项时，定义为 1。 其他情况下则不定义。

- `_OPENMP`：如果设置了 [`/openmp`（启用 OpenMP 2.0 支持）](../build/reference/openmp-enable-openmp-2-0-support.md)编译器选项，则定义为整数文本 200203。 此值表示 MSVC 实现 OpenMP 规范的日期。 其他情况下则不定义。

   ```cpp
   // _OPENMP_dir.cpp
   // compile with: /openmp
   #include <stdio.h>
   int main() {
      printf("%d\n", _OPENMP);
   }
   ```

- `_PREFAST_`：当设置了 [`/analyze`](../build/reference/analyze-code-analysis.md) 编译器选项时，定义为 1。 其他情况下则不定义。

- `__TIMESTAMP__`：定义为包含当前源文件上次修改日期和时间的字符串文本，并采用 CRT [`asctime`](../c-runtime-library/reference/asctime-wasctime.md) 函数返回的恒定长度缩写格式，例如 `Fri 19 Aug 13:32:58 2016`。 任何情况下都会定义此宏。

- `_VC_NODEFAULTLIB`：当设置了 [`/Zl`（省略默认库名称）](../build/reference/zl-omit-default-library-name.md)编译器选项，定义为 1。 其他情况下则不定义。

- `_WCHAR_T_DEFINED`：当设置了默认 [`/Zc:wchar_t`](../build/reference/zc-wchar-t-wchar-t-is-native-type.md) 编译器选项时，定义为 1。 但如果设置了 `/Zc:wchar_t-` 编译器选项并且在项目中包含的系统头文件中定义了 `wchar_t`，则会定义 `_WCHAR_T_DEFINED` 宏，但不会定义其值 。 其他情况下则不定义。

- `_WIN32`：当编译目标为 32 位 ARM、64 位 ARM、x86 或 x64 时，定义为 1。 其他情况下则不定义。

- `_WIN64`：当编译目标为 64 位 ARM 或 x64 时，定义为 1。 其他情况下则不定义。

- `_WINRT_DLL`：当编译为 C++，并且同时设置了 [`/ZW`（Windows 运行时编译）](../build/reference/zw-windows-runtime-compilation.md)和 [`/LD` 或 `/LDd`](../build/reference/md-mt-ld-use-run-time-library.md) 编译器选项时，定义为 1  。 其他情况下则不定义。

编译器未预定义用于识别 ATL 或 MFC 库版本的预处理器宏。 ATL 和 MFC 库头在内部定义这些版本宏。 在包含必需头之前实现的预处理器指令中，它们未定义。

- `_ATL_VER`：在 \<atldef.h> 中定义为编码 ATL 版本号的整数文本。

- `_MFC_VER`：在 \<afxver_.h> 中定义为编码 MFC 版本号的整数文本。

## <a name="see-also"></a>请参阅

[宏 (C/C++)](../preprocessor/macros-c-cpp.md)<br/>
[预处理器运算符](../preprocessor/preprocessor-operators.md)<br/>
[预处理器指令](../preprocessor/preprocessor-directives.md)
