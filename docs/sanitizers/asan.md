---
title: AddressSanitizer
description: 适用于 Microsoft C/c + + 的 AddressSanitizer 功能的顶级说明。
ms.date: 03/05/2021
f1_keywords:
- AddressSanitizer
helpviewer_keywords:
- ASan
- AddressSanitizer
- Address Sanitizer
- compiling for AddressSanitizer
ms.openlocfilehash: db1760a37c610493cd3a3d95ab5e77b29bf89400
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470945"
---
# <a name="addresssanitizer"></a>AddressSanitizer

## <a name="overview"></a>概述

C & c + + 语言功能强大，但可能会受到影响程序的正确性和程序安全的一类错误。 从 Visual Studio 2019 16.9 版开始，Microsoft C/c + + 编译器 (MSVC) 和 IDE 支持 *AddressSanitizer*。 AddressSanitizer (ASan) 是一种编译器和运行时技术，它公开多个具有 **零** 误报的难以发现的 bug：

- [分配/dealloc 不匹配](error-alloc-dealloc-mismatch.md)和[ `new` / `delete` 类型不匹配](error-new-delete-type-mismatch.md)
- [分配对于堆太大](error-allocation-size-too-big.md)
- [ `calloc` 溢出](error-calloc-overflow.md)和[ `alloca` 溢出](error-dynamic-stack-buffer-overflow.md)
- 免费[免费](error-double-free.md)并[使用](error-heap-use-after-free.md)
- [全局变量溢出](error-global-buffer-overflow.md)
- [堆缓冲区溢出](error-heap-buffer-overflow.md)
- [对齐值对齐无效](error-invalid-allocation-alignment.md)
- [`memcpy`](error-memcpy-param-overlap.md)和[ `strncat` 参数重叠](error-strncat-param-overlap.md)
- [堆栈缓冲区溢出](error-stack-buffer-overflow.md) 和 [下溢](error-stack-buffer-underflow.md)
- [堆栈使用之后 `return` ](error-stack-use-after-return.md)并[在作用域后使用](error-stack-use-after-scope.md)
- [中毒后的内存使用情况](error-use-after-poison.md)

使用 AddressSanitizer 缩短你花费的时间：

- 基本正确性
- 跨平台可移植性
- 安全性
- 压力测试
- 集成新代码

AddressSanitizer 最初[由 Google 引入](https://www.usenix.org/conference/atc12/technical-sessions/presentation/serebryany)，是[ `/RTC` (运行时错误检查) ](../build/reference/rtc-run-time-error-checks.md)和[ `/analyze` (静态分析) ](../build/reference/analyze-code-analysis.md)的一种功能强大的替代方法。 它提供运行时 bug 查找技术，这些技术使用现有的生成系统和现有测试资产。

AddressSanitizer 与 Visual Studio 项目系统、CMake 生成系统和 IDE 集成。 项目可通过设置项目属性或使用另外一个编译器选项来启用 AddressSanitizer： **`/fsanitize=address`** 。 新选项与 x86 和 x64 的所有优化级别和配置兼容。 但是，它与 " [编辑并继续](/visualstudio/debugger/edit-and-continue-visual-cpp)"、" [增量链接](../build/reference/incremental-link-incrementally.md)" 和不兼容 [`/RTC`](../build/reference/rtc-run-time-error-checks.md) 。

从 Visual Studio 2019 版本16.9 开始，Microsoft 的 AddressSanitizer 技术可实现与 Visual Studio IDE 的集成。 当 sanitizer 在运行时发现 bug 时，此功能可以选择创建故障转储文件。 如果在 `ASAN_SAVE_DUMPS=MyFileName.dmp` 运行程序之前设置环境变量，则会使用额外的元数据创建故障转储文件，以便对准确诊断的 bug 进行事后的有效 [调试](#crash-dumps) 。 这些转储文件更易于使用 AddressSanitizer：

- 本地计算机测试，
- 本地分布式测试，以及
- 用于测试的基于云的工作流。

### <a name="install-the-addresssanitizer"></a>安装 AddressSanitizer

默认情况下，AddressSanitizer IDE 集成和库默认安装在 Visual Studio 安装程序中的 c + + 工作负荷。 但是，如果要从早期版本的 Visual Studio 2019 升级，请使用安装程序在升级后启用 ASan 支持：

:::image type="content" source="media/asan-installer-option.png" alt-text="突出显示 c + + AddressSanitizer 组件的 Visual Studio 安装程序屏幕快照":::

可以从 Visual Studio 安装程序中选择 " **修改** 现有的 Visual Studio"，以转到上图。

> [!NOTE]
> 如果在新的更新上运行 Visual Studio，但未安装 ASan，则在运行代码时，会收到错误：
>
> LNK1356：找不到库 "clang_rt. asan_dynamic-i386"

### <a name="use-the-addresssanitizer"></a><a name="using-asan"></a> 使用 AddressSanitizer

**`/fsanitize=address`** 使用以下任一常见开发方法，使用编译器选项开始生成可执行文件：

- 命令行生成
- Visual Studio 项目系统
- Visual Studio CMake 集成

 重新编译，然后正常运行您的程序。 此代码生成公开 [了许多类型的准确诊断 bug](#error-types)。 这些错误通过以下三种方式进行报告：在调试器 IDE 中，在命令行上，或存储在一 [种新类型的转储文件](#crash-dumps) 中，用于精确的脱机处理。

Microsoft 建议使用这三个标准工作流中的 AddressSanitizer：

- **开发人员内部循环**
  - Visual Studio- [命令行](#command-prompt)
  - Visual Studio- [项目系统](#ide-msbuild)
  - Visual Studio- [CMake](#ide-cmake)

- **CI/CD** -持续集成/持续开发
  - 错误报告- [新 AddressSanitizer 转储文件](#crash-dumps)

- **模糊** -用 [libFuzzer](https://llvm.org/docs/LibFuzzer.html) 包装器生成
  - [Azure OneFuzz](https://www.microsoft.com/security/blog/2020/09/15/microsoft-onefuzz-framework-open-source-developer-tool-fix-bugs/)
  - 本地计算机

本文介绍了启用上述三个工作流所需的信息。 此信息特定于依赖于 **平台** 的 Windows 10 AddressSanitizer 实现。 本文档补充了已发布的来自 [Google、Apple 和 GCC](#external-docs) 的优秀文档。

> [!NOTE]
> 当前支持仅限于 Windows 10 上的 x86 和 x64。 [向我们发送](https://aka.ms/vsfeedback/browsecpp) 有关你希望在未来版本中看到的内容的反馈。 你的反馈有助于我们为将来的其他 sanitizers 设置优先级，如、、、 **`/fsanitize=thread`** **`/fsanitize=leak`** **`/fsanitize=memory`** **`/fsanitize=undefined`** 或 **`/fsanitize=hwaddress`** 。 如果遇到问题，可以在 [此处报告 bug](https://aka.ms/feedback/report?space=62) 。

## <a name="use-the-addresssanitizer-from-a-developer-command-prompt"></a><a name="command-prompt"></a> 从开发人员命令提示符使用 AddressSanitizer

**`/fsanitize=address`** 在 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)使用编译器选项来启用 AddressSanitizer 运行时的编译。 **`/fsanitize=address`** 选项与所有现有 c + + 或 C 优化级别兼容 (例如，、 `/Od` 、 `/O1` `/O2` 、 `/O2 /GL` 和 `PGO`) 。 选项适用于静态和动态 crt (例如，、、 `/MD` `/MDd` `/MT` 和 `/MTd`) 。 无论是创建 EXE 还是 DLL，都可以使用此方法。 调试信息是调用堆栈的最佳格式设置所必需的。 在下面的示例中，在 `cl /fsanitize=address /Zi` 命令行上传递。

AddressSanitizer 库 ( .lib 文件) 会自动链接。 有关详细信息，请参阅 [AddressSanitizer language，build，and 调试 reference](asan-building.md)。

### <a name="example---basic-global-buffer-overflow"></a>示例-基本全局缓冲区溢出

```cpp
// basic-global-overflow.cpp
#include <stdio.h>
int x[100];
int main() {
    printf("Hello!\n");
    x[100] = 5; // Boom!
    return 0;
}
```

使用 Visual Studio 2019 的开发人员命令提示符，使用进行编译 *`main.cpp`*`/fsanitize=address /Zi`

:::image type="content" source="media/asan-command-basic-global-overflow.png" alt-text="命令提示符屏幕截图，其中显示了要使用 AddressSanitizer 选项编译的命令。":::

当你在 *`main.exe`* 命令行中运行时，它会创建如下所示的格式化错误报告。

请考虑重叠的红色方框，其中突出显示了七个关键信息：

:::image type="content" source="media/asan-basic-global-overflow.png" alt-text="显示基本全局溢出错误的调试器屏幕截图。":::

### <a name="red-highlights-from-top-to-bottom"></a>红色突出显示，从上到下

1. 内存安全错误是全局缓冲区溢出。
2. 有 **4 个字节** (32 位) **存储** 在任何用户定义的变量之外。
3. 存储发生在 `main()` 第7行的文件中定义的函数中 `basic-global-overflow.cpp` 。
4. 名为的变量 `x` 在第3行的 basic-global-overflow 中定义，从第8列开始
5. 此全局变量 `x` 的大小为400个字节
6. 描述存储目标地址的确切 [阴影字节](./asan-shadow-bytes.md) 的值为 `0xf9`
7. 阴影字节图例显示 `0xf9` 在右侧的填充区域 `int x[100]`

> [!NOTE]
> 调用堆栈中的函数名称通过运行时在出错时调用的 [LLVM symbolizer](https://llvm.org/docs/CommandGuide/llvm-symbolizer.html) 生成。

## <a name="use-the-addresssanitizer-in-visual-studio"></a><a name="ide-msbuild"></a> 在 Visual Studio 中使用 AddressSanitizer

AddressSanitizer 与 Visual Studio IDE 集成。 若要打开 MSBuild 项目的 AddressSanitizer，请在解决方案资源管理器中右键单击该项目，然后选择 " **属性**"。 在 "**属性页**" 对话框中，选择 "**配置属性**" "  >  **c/c + +**  >  **常规**"，然后修改 "**启用 AddressSanitizer** " 属性。 选择“确定”以保存更改  。

:::image type="content" source="media/asan-project-system-dialog.png" alt-text="显示 &quot;启用 AddressSanitizer&quot; 属性的 &quot;属性页&quot; 对话框的屏幕截图。":::

若要从 IDE 构建，请选择退出任何 [不兼容的选项](./asan-known-issues.md#incompatible-options)。 对于使用 **`/Od`** (或调试模式) 编译的现有项目，可能需要关闭以下选项：

- 关闭 "[编辑并继续](/visualstudio/debugger/how-to-enable-and-disable-edit-and-continue)"
- 关闭[ `/RTC1` (运行时检查) ](../build/reference/rtc-run-time-error-checks.md)
- 关闭[ `/INCREMENTAL` (增量链接) ](../build/reference/incremental-link-incrementally.md)

若要生成并运行调试器，请输入 **F5**。 你将在 Visual Studio 中看到此窗口：

:::image type="content" source="media/asan-global-buffer-overflow-F5.png" alt-text="显示全局缓冲区溢出错误的调试器屏幕截图。":::

## <a name="using-the-addresssanitizer-from-visual-studio-cmake"></a><a name="ide-cmake"></a> 使用 Visual Studio 中的 AddressSanitizer： CMake

若要为 [创建为面向 Windows 的 CMake 项目](../build/cmake-projects-in-visual-studio.md)启用 AddressSanitizer，请执行以下步骤：

1. 打开 IDE 顶部工具栏中的 " **配置** " 下拉列表，并选择 " **管理配置**"。

   :::image type="content" source="media/asan-cmake-configuration-dropdown.png" alt-text="CMake 配置下拉列表的屏幕截图。":::

   该选择将打开 CMake 项目设置编辑器，该编辑器保存在文件 CMakeSettings.js中。

1. 在编辑器中选择 " **编辑 JSON** " 链接。 此选择会将视图切换到原始 JSON。

1. 添加属性： **"addressSanitizerEnabled"： true**

   此映像在此更改之后 CMakeSettings.js：

   :::image type="content" source="media/asan-cmake-json.png" alt-text="CMakeSettings.js上的的 &quot;文本编辑器&quot; 视图的屏幕截图。":::

1. 输入 **Ctrl + S** 保存此 JSON 文件，然后输入 **F5** 在调试器下重新编译和运行。

此屏幕截图捕获来自 CMake 生成的错误。

:::image type="content" source="media/asan-cmake-error-f5.png" alt-text="CMake 生成错误消息的屏幕截图。":::

## <a name="addresssanitizer-crash-dumps"></a><a name="crash-dumps"></a> AddressSanitizer 故障转储

我们在 AddressSanitizer 中引入了新功能，用于云和分布式工作流。 此功能允许脱机查看 IDE 中的 AddressSanitizer 错误。 错误会在源上重叠，就像在实时调试会话中一样。

在分析 bug 时，这些新的转储文件可能会提高效率。 不需要重新运行或查找远程数据或查找脱机的计算机。

生成新类型的转储文件，该文件可在以后在另一台计算机上的 Visual Studio 中查看：

```cmd
set ASAN_SAVE_DUMPS=MyFileName.dmp
```

从 Visual Studio 16.9 开始，你可以在你的源代码的顶部显示 **一个确切诊断错误，该错误** 存储在 *`*.dmp`* 文件中。

[这种新的故障转储功能](./asan-offline-crash-dumps.md) 可启用基于云的工作流或分布式测试。 它还可用于在任何情况下为详细的可操作 bug 进行归档。

## <a name="example-errors"></a><a name="error-types"></a> 示例错误

AddressSanitizer 可以检测到多种类型的内存错误。 下面是运行使用 AddressSanitizer () 编译器选项编译的二进制文件时所报告的多个运行时错误 **`/fsanitize=address`** ：

- [`alloc-dealloc-mismatch`](error-alloc-dealloc-mismatch.md)
- [`allocation-size-too-big`](error-allocation-size-too-big.md)
- [`calloc-overflow`](error-calloc-overflow.md)
- [`double-free`](error-double-free.md)
- [`dynamic-stack-buffer-overflow`](error-dynamic-stack-buffer-overflow.md)
- [`global-buffer-overflow`](error-global-buffer-overflow.md)
- [`heap-buffer-overflow`](error-heap-buffer-overflow.md)
- [`heap-use-after-free`](error-heap-use-after-free.md)
- [`invalid-allocation-alignment`](error-invalid-allocation-alignment.md)
- [`memcpy-param-overlap`](error-memcpy-param-overlap.md)
- [`new-delete-type-mismatch`](error-new-delete-type-mismatch.md)
- [`stack-buffer-overflow`](error-stack-buffer-overflow.md)
- [`stack-buffer-underflow`](error-stack-buffer-underflow.md)
- [`stack-use-after-return`](error-stack-use-after-return.md)
- [`stack-use-after-scope`](error-stack-use-after-scope.md)
- [`strncat-param-overlap`](error-strncat-param-overlap.md)
- [`use-after-poison`](error-use-after-poison.md)

有关示例的详细信息，请参阅 [AddressSanitizer 错误示例](./asan-error-examples.md)。

## <a name="differences-with-clang-120"></a><a name="differences"></a> 与 Clang 12.0 的差异

MSVC 当前在两个功能区域中与 Clang 12.0 不同：

- **stack-作用域后** -默认情况下，此设置处于启用状态，并且不能关闭。
- **stack-返回** --此功能需要额外的编译器选项，并且仅可通过设置来使用 `ASAN_OPTIONS` 。

为了减少交付此第一个版本所需的测试矩阵，已做出这些决定。

在 Visual Studio 2019 16.9 中可能导致误报的功能不包括在内。 在考虑与数十年的现有代码互操作时，这一学科强制实施有效的测试完整性。 更多功能可能会在以后的版本中考虑：

- [初始化顺序 Fiasco](https://github.com/google/sanitizers/wiki/AddressSanitizerInitializationOrderFiasco)
- [对象溢出](https://github.com/google/sanitizers/wiki/AddressSanitizerIntraObjectOverflow)
- [容器溢出](https://github.com/google/sanitizers/wiki/AddressSanitizerContainerOverflow)
- [指针减法/比较](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)

有关详细信息，请参阅 [通过 MSVC 生成 AddressSanitizer](./asan-building.md)。

## <a name="existing-industry-documentation"></a><a name="external-docs"></a> 现有行业文档

对于这些语言和平台相关的 AddressSanitizer 技术实现，已经存在广泛的文档。

- [Google](https://github.com/google/sanitizers/wiki/AddressSanitizer)
- [Apple](https://developer.apple.com/documentation/xcode/diagnosing_memory_thread_and_crash_issues_early)
- [GCC](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)

本 [AddressSanitizer](https://www.usenix.org/system/files/conference/atc12/atc12-final39.pdf) 上的 box 文章介绍了实现。

## <a name="see-also"></a>另请参阅

[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
