---
title: Visual Studio 中的 C++ 新变化
description: Visual Studio 2019 中的 Microsoft C/C++ 编译器和工具的新增功能和修复。
ms.date: 03/03/2021
ms.technology: cpp-ide
ms.openlocfilehash: c48b733ad18d57917220b443bd18830441dc693c
ms.sourcegitcommit: f8ba5db09d05683b24c58505f0e57c21f85545dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103087176"
---
# <a name="whats-new-for-c-in-visual-studio"></a>Visual Studio 中的 C++ 新变化

Visual Studio 2019 向 Microsoft C++ 环境引入了许多更新和修补程序。 我们修复了编译器和工具中存在的许多 bug 和问题。 其中许多问题是客户通过“发送反馈”下的[报告问题](/visualstudio/ide/how-to-report-a-problem-with-visual-studio?view=vs-2019&preserve-view=true)和[提供建议](https://aka.ms/feedback/suggest?space=62)选项提交的。 感谢你报告 bug！

有关所有 Visual Studio 中新增功能的详细信息，请访问 [Visual Studio 2019 中的新增功能](/visualstudio/ide/whats-new-visual-studio-2019)。 有关 Visual Studio 2017 中 C++ 新增功能的信息，请参阅 [Visual Studio 2017 中 C++ 的新增功能](what-s-new-for-cpp-2017.md)。 有关 Visual Studio 2015 及更低版本中 C++ 新增功能的信息，请参阅 [Visual C++ 新增功能 (2003 - 2015)](../porting/visual-cpp-what-s-new-2003-through-2015.md)。 有关 C++ 文档中新增功能的信息，请[参阅 Microsoft C++ 文档：新增功能](whats-new-cpp-docs.md)。

## <a name="whats-new-for-c-in-visual-studio-version-169"></a>Visual Studio 版本 16.9 中 C++ 的新增功能

有关 Visual Studio 版本 16.9 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本 16.9 中的新增功能](/visualstudio/releases/2019/release-notes#--visual-studio-2019-version-1690-)。

- [地址擦除器](../sanitizers/asan.md)：

  - Windows 上的地址擦除器支持已不处于实验性阶段，而是进入了正式发布阶段。

  - 扩展了 `RtlAllocateHeap` 支持，修复了在创建可执行内存池时使用 `RtlCreateHeap` 和 `RtlAllocateHeap` 侦听器的兼容性问题。

  - 添加了对旧 `GlobalAlloc` 和 `LocalAlloc` 内存函数系列的支持。 可以通过设置环境标志 `ASAN_OPTIONS=windows_hook_legacy_allocators=true` 来启用这些侦听器。

  - 更新了影子内存交错和侦听失败的错误消息，以显式显示问题和解决方法。

  - IDE 集成现在可以处理 ASan 会报告的异常完整集合。

  - 如果编译器和链接器检测到你正在使用 ASan 生成但未发出调试信息，它们将建议发出调试信息。

- 现在可以将 OpenMP 运行时的 LLVM 版本定位到新的 CL 开关 `/openmp:llvm`。 这增加了对 `#pragma omp` 部分的 `lastprivate` 子句以及并行 `for` 循环中的无符号索引变量的支持。 `/openmp:llvm` 开关目前仅适用于 amd64 目标，仍处于试验阶段。

- Visual Studio CMake 项目现提供对远程 Windows 开发的一流支持。 这包括将 CMake 项目配置为面向 Windows ARM64，将项目部署到远程 Windows 计算机，以及从 Visual Studio 调试远程 Windows 计算机上的项目。

- Windows 上的 Visual Studio 附带的 Ninja 版本已更新到版本 1.10。 有关包含的内容的详细信息，请参阅 [Ninja 1.10 发行说明](https://groups.google.com/g/ninja-build/c/piOltAhywFA/m/zPfkrTtRCwAJ?pli=1)。

- Visual Studio 附带的 CMake 版本已更新到版本 3.19。 有关包含的内容的详细信息，请参阅 [CMake 3.19 发行说明](https://cmake.org/cmake/help/latest/release/3.19.html)。

- [在 STL 中将多个 lock/guard 类型标记为 `nodiscard`](https://github.com/microsoft/STL/pull/1495)。

- IntelliSense：

  - 提高了在 IntelliSense 中提供导入模块和标头单元完成的稳定性和功能。

  - 增加了对模块导入的跳转到定义、对`export {...}` 的索引支持，以及对同名模块更准确的模块引用。

  - 提高了 C++ IntelliSense 的语言符合性，方法是添加对[引用直接初始化的临时复制初始化](https://wg21.link/P1358R0)、`__builtin_memcpy` 和 `__builtin_memmove`、[修复 `constexpr` 和 `consteval` 函数之间的不一致性](https://wg21.link/P1937R2)、[常数表达式的生存期扩展临时项](https://wg21.link/P1968R0)以及[类似类型和引用绑定](https://wg21.link/P1358R0)的支持。

  - 为 make_unique、make_shared、emplace 和 emplace_back 添加了完成，这将根据指定的类型参数提供完成。

- MSVC 现在确定了二进制文件所需的正确地址擦除系统运行时。 Visual Studio 项目会自动获取新的更改。 在命令行上使用地址擦除器时，现在只需将 [`/fsanitize=address`](../build/reference/fsanitize.md) 传递到编译器即可。

- Visual Studio 的连接管理器现支持使用 ECDSA 公钥算法的私钥。

- 已将安装程序中提供的 LLVM 和 Clang 版本更新为 v11。 有关详细信息，请参阅 [LLVM](https://releases.llvm.org/11.0.0/docs/ReleaseNotes.html) 和 [Clang](https://releases.llvm.org/11.0.0/tools/clang/docs/ReleaseNotes.html) 的发行说明。

- Visual Studio 现在将使用工具链文件中的 CMake 变量来配置 IntelliSense。 这将为嵌入式开发和 Android 开发提供更好的体验。

- 实现了[更多 Constexpr 容器建议](https://wg21.link/P0784R7)，可以将析构函数和新表达式用作 `constexpr`。 这为诸如 `constexpr` `std::vector` 和 `std::string` 之类的实用工具做好了准备。

- 对 C++20 模块 IntelliSense 的扩展支持，包括“转到定义”、“转到模块”和成员完成。

- MSVC 编译器现在支持[缩写的函数模板](https://en.cppreference.com/w/cpp/language/function_template#Abbreviated_function_template)。

## <a name="whats-new-for-c-in-visual-studio-version-168"></a>Visual Studio 版本 16.8 中 C++ 的新增功能

有关 Visual Studio 版本 16.8 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.8 中的新增功能](/visualstudio/releases/2019/release-notes-v16.8)。

- [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 和 \<coroutine> 标头中现支持 C++20 协同例程。

- IntelliSense 现提供对 C++20 \<concepts> 和 \<ranges> 标头的支持，还支持重命名和浏览概念定义。

- STL 现支持大多数 C++20 范围。

- MSVC 中现支持[有条件的平凡特殊成员函数](https://wg21.link/P0848R3)。

- [`/std:c11` 和 `/std:c17`](../build/reference/std-specify-language-standard-version.md) 开关下现支持 C11 和 C17。

- 其他 STL 改进包括对 [`std::atomic_ref`](https://wg21.link/p0019r8)、[`std::midpoint` 和 `std::lerp`](https://wg21.link/p0811r3) 和 [`std::execution::unseq`](https://wg21.link/p1001r2) 的完全支持，以及针对 [`std::reverse_copy`](../standard-library/algorithm-functions.md#reverse_copy) 的优化等等。

- 将 Visual Studio 附带的 CMake 版本升级到了 [CMake 3.18](https://cmake.org/cmake/help/latest/release/3.18.html)。

- 我们的代码分析工具现支持 SARIF 2.1 标准（即行业标准静态分析格式）。

- 如果 Linux 项目中缺少生成工具，现将在工具栏中发出一个警告并在错误列表中明确描述缺少的工具。

- 你现可直接通过 Visual Studio 在远程 Linux 系统或 WSL 上调试 Linux 核心转储。

- 对于 C++ Doxygen 注释生成，我们添加了其他注释样式选项（`/*!` 和 `//!`）。

- 其他 [vcpkg 公告](https://aka.ms/vcpkg/team)。

- 在未求值的上下文中对 lambda 的编译器支持。

- 通过创建多线程 PDB 改进了 [`/DEBUG:FULL`](../build/reference/debug-generate-debug-info.md) 链接性能。 几个大型应用程序和 AAA 游戏的链接速度提高了 2 到 4 倍。

- Visual Studio 调试器现在支持 `char8_t`。

- 支持使用 clang-cl 的 ARM64 项目。

- [Intel AMX 内部函数](https://software.intel.com/content/www/us/en/develop/documentation/cpp-compiler-developer-guide-and-reference/top/compiler-reference/intrinsics/intrinsics-for-intel-advanced-matrix-extensions-intel-amx-instructions.html)支持。

## <a name="whats-new-for-c-in-visual-studio-version-167"></a>Visual Studio 版本 16.7 中 C++ 的新增功能

有关 Visual Studio 版本 16.7 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.7 中的新增功能](/visualstudio/releases/2019/release-notes-v16.7)。

- 我们的远程 C++ 支持现涵盖更广泛的 Linux 发行版和 Shell，包括 sh、csh、bash、tsch、ksh、zsh 和 dash。 可通过 ConnectionManager.exe 修改新的“shell”属性，覆盖为远程连接选择的 Shell。 此支持已通过基于 MSBuild 的 Linux 项目以及面向远程 Linux 系统或 WSL 的 CMake 项目进行测试。

- 你现可使用 Ninja（一种可超快速评估增量生成的生成系统）缩短基于 MSBuild 的 Linux 项目的增量生成时间。 你可通过在“常规属性”页面中将“启用增量生成”设置为“使用 Ninja”，选择使用此功能。 Ninja (ninja-build) 必须安装在远程 Linux 系统或 WSL 上。

- 实现了新的 C++20 标准库功能。 有关详细列表，请参阅 [GitHub 上的 STL 更改日志](https://github.com/microsoft/STL/wiki/Changelog)。

- 现可在[连接管理器](../linux/connect-to-your-remote-linux-computer.md#set-up-the-remote-connection)中编辑和设置默认远程 SSH 连接。 这意味着你可以编辑现有远程连接（例如在其 IP 地址更改时），还可设置在 CMakeSettings.json 和 launch.vs.json 中使用默认连接。 借助远程 SSH 连接，可直接通过 Visual Studio 在远程 Linux 系统上生成和调试 C++ 项目。

- 增强了对 Visual Studio 中 Clang on Windows (clang-cl) 的 IntelliSense 支持。 clang include 路径现包含 clang 库，我们改进了在使用 std 库时编辑器内的波形曲线显示效果，还添加了对 clang 模式下 C++2a 的支持。

- 现在可以尝试为代码错误加下划线，并在 C++ 项目中查看更多建议的快速修补程序。 为启用此功能，请转到“工具”>“选项”>“文本编辑器”>“C/ C++”>“试验性”。 将“禁用试验性代码 Linter”设置为 false。 通过 [C++ 团队博客](https://aka.ms/cpp/isensecodelinter)了解详细信息。

- 添加了后面 4 条新的代码分析规则，在 C++ 中加入了额外的安全功能：[C26817](../code-quality/c26817.md)、[C26818](../code-quality/c26818.md)、[C26819](../code-quality/c26819.md) 和 [C26820](../code-quality/c26820.md)。

- 我们针对使用 gdbserver 在远程系统上调试 CMake 项目添加了一流的支持。

- 在 Visual Studio 中使用 C++ AddressSanitizer 的试验性实现来轻松查找内存损坏错误，现可用于 x64 本机项目。 我们现在还支持使用调试运行时（`/MTd`、`/MDd`、`/LDd`）。

- IntelliSense 现在具有对概念、指定的初始化表达式和其他几个 C++20 功能的基本支持。

- `.ixx` 和 `.cppm` 文件现在已被视为 C++，并由语法高亮器和 IntelliSense 以这种方式处理。

## <a name="whats-new-for-c-in-visual-studio-version-166"></a>Visual Studio 版本 16.6 中 C++ 的新增功能

有关 Visual Studio 版本 16.6 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.6 中的新增功能](/visualstudio/releases/2019/release-notes-v16.6)。

- 改进了 Doxygen/XML 注释生成：在函数上方键入“`///`”或“`/**`”会自动生成 Doxygen 或 XML 文档注释存根。 它们现在也显示在“快速信息工具提示”中。

- **对 CMake for Linux/WSL 的 Ninja 支持：** 在 WSL 或远程系统上生成 CMake 项目时，请使用 Ninja 作为基础生成器。 添加新的 Linux 或 WSL 配置时，Ninja 是默认生成器。

- **远程 CMake 调试的调试模板：** 我们已简化了用于在远程 Linux 系统或 WSL 上通过 gdb 调试 CMake 项目的模板。

- C++ 20 概念的初始支持：IntelliSense 现可识别 [C++20 概念](https://devblogs.microsoft.com/cppblog/c20-concepts-are-here-in-visual-studio-2019-version-16-3/)并在成员列表中推荐它们。

## <a name="whats-new-for-c-in-visual-studio-version-165"></a>Visual Studio 版本 16.5 中 C++ 的新增功能

有关 Visual Studio 版本 16.5 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.5 中的新增功能](/visualstudio/releases/2019/release-notes-v16.5)。

- IntelliCode 团队完成项模型和成员变量支持：C++ 开发人员现在可以在自己的代码库上定型 IntelliCode 模型。 我们称之为团队完成项模型，因为你可以从团队的实践中获益。 此外，我们还改进了成员变量的 IntelliCode 建议。

- IntelliSense 改进：
  -  在使用标准库时，IntelliSense 现在显示可读性更强的类型名称。
  - 我们添加了切换是否将 Enter、Space 和 Tab 函数作为提交字符的功能，以及切换是否使用 Tab 来插入代码片段的功能。 可在“工具”>“选项”>“文本编辑器”>“C/C++”>“高级”>“IntelliSense”下找到这些设置。

- 通过命令行使用连接管理器：现在可以通过命令行与存储的远程连接进行交互。 这对于预配新开发计算机或在持续集成中设置 Visual Studio 等任务非常有用。

- 调试和部署 WSL：利用 Visual Studio 对 WSL 的本机支持从远程部署系统中分离生成系统。 现在可以在 WSL 上进行本机生成，并将生成项目部署到另一个远程系统进行调试。 CMake 项目和基于 MSBuild 的 Linux 项目都支持此工作流。

- 支持符合 FIPS 140-2 标准的模式：Visual Studio 现在支持符合 FIPS 140-2 标准的模式，可用于开发面向远程 Linux 系统的 C++ 应用程序。

- CMake 语言文件的语言服务和改善的 CMake 项目处理：
  - 面向远程 Linux 系统的 CMake 项目的源文件副本已经过优化。 Visual Studio 现在会保留最近远程复制的一组源的“指纹文件”，并基于已更改的文件数量优化行为。
  -  CMake 脚本文件中的函数、变量和目标现支持转到定义和查找所有引用等代码导航功能。

  - 无需手动编辑 CMake 脚本，从 IDE 中即可添加、删除和重命名 CMake 项目中的源文件和目标。 当你在解决方案资源管理器中添加或删除文件时，Visual Studio 将自动编辑你的 CMake 项目。 你还可以从解决方案资源管理器的目标视图中添加、删除和重命名项目的目标。

- Linux 项目改进：Visual Studio Linux 项目现在具有更准确的 IntelliSense，用户可以按每个项目逐一控制远程标头同步。

## <a name="whats-new-for-c-in-visual-studio-version-164"></a>Visual Studio 版本 16.4 中 C++ 的新增功能

有关 Visual Studio 版本 16.4 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.4 中的新增功能](/visualstudio/releases/2019/release-notes-v16.4)。

- 无论你是使用 Clang 还是 MSVC 工具集，代码分析现在都本机支持 MSBuild 和 CMake 项目的 [`Clang-Tidy`](https://aka.ms/cpp/clangtidy)。 clang-tidy 检查可以作为后台代码分析的一部分运行，显示为编辑器内的警告（波形曲线），并显示在错误列表中。

- Visual Studio CMake 项目现在提供[概述](https://devblogs.microsoft.com/cppblog/usability-improvements-for-cmake-in-visual-studio-2019-version-16-4-launch-target-selection-and-overview-pages/)页面，可帮助你开始进行跨平台开发。 这些页面是动态的，有助于连接到 Linux 系统并将 Linux 或 WSL 配置添加到 CMake 项目。

- [CMake 项目的启动下拉菜单](https://devblogs.microsoft.com/cppblog/usability-improvements-for-cmake-in-visual-studio-2019-version-16-4-launch-target-selection-and-overview-pages/)现在会显示最近使用的目标，并且可以进行筛选。

- [C++/CLI](https://devblogs.microsoft.com/cppblog/an-update-on-cpp-cli-and-dotnet-core/) 现在支持与 Windows 上的 .NET Core 3.1 和更高版本互操作。

- 现可为使用 Windows 上的 MSVC 编译的项目启用 [ASan](https://aka.ms/cpp/asanmsvc) 以进行 C++ 代码的运行时检测，这有助于检测内存错误。

- MSVC 的 C++ 标准库的更新：
  - C++17：已实现 `to_chars()` 常规精度，从而完成 [P0067R5](https://wg21.link/P0067R5) 基本字符串转换 (charconv)。 这完成了 C++17 标准中所有库功能的实现。
  - C++20：已实现 [P1754R1](https://wg21.link/P1754R1) 将概念重命名为 standard_case。 若要在最新的 C++ 工作草案中包含预览功能，可以使用 `/std:c++latest` 编译器选项。 此外，还可以使用“配置属性”>“C/C++”>“语言”项目属性页的“C++ 语言标准”属性来设置该选项。

- 现已推出名为 [C++ Build Insights](../build-insights/get-started-with-cpp-build-insights.md) 的新工具集合。 有关公告的详细信息，请参阅 [C++ 团队博客](https://devblogs.microsoft.com/cppblog/introducing-c-build-insights/)。

## <a name="whats-new-for-c-in-visual-studio-version-163"></a>Visual Studio 版本 16.3 中 C++ 的新增功能

有关 Visual Studio 版本 16.3 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.3 中的新增功能](/visualstudio/releases/2019/release-notes-v16.3)。

- C++ 开发人员现在可以使用键盘快捷方式“Ctrl+K”、“Ctrl+/”来切换行注释。 

- 现在可以基于类型限定符筛选 IntelliSense 成员列表，例如，`const std::vector` 现在将筛选出 `push_back` 等方法。

- 我们添加了以下 C++20 标准库预览版功能（在 `/std:c++latest` 下可用）：
  - [P0487R1](https://wg21.link/P0487R1)：修正 `operator>>(basic_istream&, CharT*)`
  - [P0616R0](https://wg21.link/P0616R0)：在 `<numeric>` 中使用 `move()`
  - [P0758R1](https://wg21.link/P0758R1)：`is_nothrow_convertible`
  - [P0734R0](https://wg21.link/P0734R0)：概念的 C++ 扩展
  - [P0898R3](https://wg21.link/P0898R3)：标准库概念
  - [P0919R3](https://wg21.link/P0919R3)：无序容器的异类查找

- [新 C++ Core 准则检查](https://devblogs.microsoft.com/cppblog/new-c-core-check-rules/)，包括新的“枚举规则”规则集以及其他 `const`、`enum` 和类型规则。

- 新的默认语义着色方案使用户能够一目了然地了解其代码，可将调用堆栈窗口配置为隐藏模板参数，并默认启用 C++ IntelliCode。

- 使用环境变量配置调试目标和自定义任务的方法有：使用 CMakeSettings.json 或 CppProperties.json，或者在 launch.vs.json 和 tasks.vs.json 中的单个目标和任务上使用新的“env”标记。

- 现在，用户可以对丢失的 vcpkg 包使用快速操作，自动打开控制台并将其安装到默认的 vcpkg 安装。

- 优化了 Linux 项目（[CMake](../linux/cmake-linux-project.md) 和 [MSBuild](../linux/configure-a-linux-project.md)）完成的远程标头复制，现在可并行运行。

- Visual Studio [对 WSL 的本机支持](https://devblogs.microsoft.com/cppblog/c-with-visual-studio-2019-and-windows-subsystem-for-linux-wsl/)现在支持基于 MSBuild 的 Linux 项目并行生成。

- 现在，用户可以指定本地生成输出的列表，以便通过 Linux Makefile 项目部署到远程系统。

- 现在，[CMake 设置编辑器](https://devblogs.microsoft.com/cppblog/introducing-the-new-cmake-project-settings-ui/)中的设置说明包含更多的上下文和指向有用文档的链接。

- IntelliCode 的 C++ 基础模型现在默认处于启用状态。 可以通过转到“工具” > “选项” > “IntelliCode”来更改此设置  。

## <a name="whats-new-for-c-in-visual-studio-version-162"></a>Visual Studio 版本 16.2 中 C++ 的新增功能

有关 Visual Studio 版本 16.2 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.2 中的新增功能](/visualstudio/releases/2019/release-notes-v16.2)。

- 对于使用 Clang 配置的本地 CMake 项目，代码分析现在运行 clang-tidy 检查，显示为编辑器警告（波形曲线）和错误列表中的后台代码分析的一部分。

- 更新了 C++17 的 [P0067R5](https://wg21.link/P0067R5) 的 `<charconv>` 标头基本字符串转换：
  - 增加了 `chars_format::fixed` 和 `chars_format::scientific` 精度的浮点 `to_chars()` 工作负载（`chars_format::general precision` 是唯一尚未实现的部分）
  - 优化了 `chars_format::fixed` 最短

- 增加了以下 C++20 标准库预览功能：
  - 在 `/std:c++latest` 下可用：
    - [P0020R6](https://wg21.link/P0020R6)：`atomic<floating-point>`
    - [P0463R1](https://wg21.link/P0463R1)：endian 枚举
    - [P0482R6](https://wg21.link/P0482R6)：`char8_t` 用于 UTF-8 字符和字符串的类型
    - [P0653R2](https://wg21.link/P0653R2)：`to_address()` 用于将指针转换为原始指针
  - 在 `/std:c++17` 和 `/std:c++latest` 下可用：
    - [P0600R1](https://wg21.link/P0600R1)：`[[nodiscard]]` 位于库中
  - 在任何条件下均可用：
    - [P0754R2](https://wg21.link/P0754R2)：`<version>` 标头
    - [P0771R1](https://wg21.link/P0771R1)：`std::function` 移动构造函数应为 `noexcept`

- Windows SDK 不再是用于 Windows 的 CMake 和用于 Linux 的 CMake 组件的依赖项。

- 对 [C++ 链接器](https://aka.ms/cpp/162linker)进行了改进，以显著改善输入大量内容时的迭代生成时间。 `/DEBUG:FAST` 和 `/INCREMENTAL` 的速度平均是以往的两倍，并且 `/DEBUG:FULL` 的速度现在是以往的三到六倍。

## <a name="whats-new-for-c-in-visual-studio-version-161"></a>Visual Studio 版本 16.1 中 C++ 的新增功能

有关 Visual Studio 版本 16.1 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.1 中的新增功能](/visualstudio/releases/2019/release-notes-v16.1)。

### <a name="c-compiler"></a>C++ 编译器

- 这些 C++20 预览功能已在 C++ 编译器中实现，可在 `/std:c++latest` 下获得：
  - 增加了通过针对包含显式模板参数 ([P0846R0](http://wg21.link/p0846r0)) 的函数调用表达式的依赖于自变量的查找来查找函数模板的功能。
  - 指定了初始化 ([P0329R4](https://wg21.link/p0329r4))，它允许在聚合初始化中选择特定成员（例如，通过使用 `Type t { .member = expr }` 语法）。

- 大幅改进了 Lambda 支持，解决了大量历史遗留的 bug。 使用 `/std:c++latest` 时，默认启用此更改。 在 `/std:c++17` 语言模式下，在默认 (`/std:c++14`) 模式下，可以使用 `/experimental:newLambdaProcessor` 来启用新的分析器，例如 `/std:c++17 /experimental:newLambdaProcessor`。

### <a name="c-standard-library-improvements"></a>C++ 标准库改进

- 已向 C++ 标准库的实现添加了以下 C++20 预览功能，这些功能可在 `/std:c++latest` 下获得：
  - 用于 `basic_string` 和 `basic_string_view` 的 `starts_with` 和 `ends_with`。
  - 关联容器的 `contains`。
  - `list` 和 `forward_list` 的 `remove`、`remove_if` 和 `unique` 现在返回 `size_type`。
  - 向 `<algorithm>` 添加了 `shift_left` 和 `shift_right`。

### <a name="c-ide"></a>C++ IDE

#### <a name="intellicode-for-c"></a>适用于 C++ 的 IntelliCode

IntelliCode 现在作为“使用 C++ 的桌面开发”工作负荷中的可选组件提供。 有关详细信息，请参阅 [Visual Studio 2019 现在随附改进后的 C++ IntelliCode](https://devblogs.microsoft.com/cppblog/improved-c-intellicode-now-ships-with-visual-studio-2019/)。

IntelliCode 使用它自己的广泛训练和你的代码上下文，将你最有可能使用的内容置于完成列表的最上面。 它通常可使用户无需向下滚动列表。 对于 C++，IntelliCode 可在使用标准库等热门库时提供最大帮助。

在默认情况下禁用 IntelliCode 预览功能（自定义模型、C++ 支持和 EditorConfig 推理）。 若要启用这些功能，请转到“工具”>“选项”>“IntelliCode”>“通用”。 此版本的 IntelliCode 提高了准确性，并包括对自由函数的支持。 有关详细信息，请参阅 [AI-Assisted Code Completion Suggestions Come to C++ via IntelliCode](https://devblogs.microsoft.com/cppblog/cppintellicode/)（AI 辅助的代码完成建议通过 IntelliCode 提供给 C++）。

#### <a name="quick-info-improvements"></a>快速信息改进

- 快速信息工具提示现在遵循编辑器的语义着色。 它还新增了“联机搜索”链接，可便于搜索联机文档来详细了解鼠标悬停于其上的代码构造。 快速信息为带红色波浪线的代码提供的链接会联机搜索相应错误。 这样，就不需要在浏览器中重新键入消息。 有关详细信息，请参阅 [Visual Studio 2019 中的快速信息改进：着色和联机搜索](https://devblogs.microsoft.com/cppblog/quick-info-improvements-in-visual-studio-2019-colorization-and-search-online/)。

#### <a name="general-improvements"></a>常规改进

- 模板栏可以基于代码库中该模板的实例化情况填充下拉菜单。

- 为可通过 vcpkg 安装的缺失的 `#include` 指令，以及在自动完成 CMake `find_package` 指令的可用包时提供灯泡。

- 修订了 C++ 项目的“常规”属性页。 现在，某些选项在新的“高级”页下列出。 “高级”页还包括首选工具集体系结构、调试库、MSVC 工具集次要版本和 Unity (jumbo) 生成的新属性。

### <a name="cmake-support"></a>CMake 支持

- 我们已将 Visual Studio 随附的 CMake 版本更新到 3.14。 此版本添加了对面向 Visual Studio 2019 项目以及基于文件的 IDE 集成 API 的 MSBuild 生成器的内置支持。

- 我们改进了 CMake 设置编辑器，包括对适用于 Linux 的 Windows 子系统 (WSL) 的支持和现有缓存中的配置；对默认生成和安装根目录的更改，以及对 Linux CMake 配置中的环境变量的支持。

- 使用内置 CMake 命令、变量和属性的完成项和快速信息可以更轻松地编辑 `CMakeLists.txt` 文件。

- 我们提供了对使用 Clang/LLVM 编辑、生成和调试 CMake 项目的综合支持。 有关详细信息，请参阅 [Visual Studio 中的 Clang/LLVM 支持](https://devblogs.microsoft.com/cppblog/clang-llvm-support-in-visual-studio/)。

### <a name="linux-and-the-windows-subsystem-for-linux"></a>Linux 和适用于 Linux 的 Windows 子系统

- 我们现在在 Linux 和 CMake 跨平台项目中支持 [AddressSanitizer (ASan)](https://github.com/google/sanitizers/wiki/AddressSanitizer)。 有关详细信息，请参阅 [Visual Studio 2019 中用于 Linux 工作负荷的 AddressSanitizer (ASan)](https://devblogs.microsoft.com/cppblog/addresssanitizer-asan-for-the-linux-workload-in-visual-studio-2019/)。

- 我们提供了对结合使用 C++ 与适用于 Linux 的 Windows 子系统 (WSL) 的综合 Visual Studio 支持。 现在无需额外配置或 SSH 连接，即可在 Visual Studio 中以本机方式将本地适用于 Linux 的 Windows 子系统 (WSL) 安装与 C++ 结合使用。 有关详细信息，请参阅[结合使用 C++ 与 Visual Studio 2019 和适用于 Linux 的 Windows 子系统 (WSL)](https://devblogs.microsoft.com/cppblog/c-with-visual-studio-2019-and-windows-subsystem-for-linux-wsl/)。

### <a name="code-analysis"></a>代码分析

- 新增了对未初始化变量检查的快速修复。 相关行上的灯泡菜单提供了代码分析警告 [C6001: 正在使用未初始化的内存&lt;变量&gt;](../code-quality/c6001.md)和 [C26494 VAR_USE_BEFORE_INIT](../code-quality/c26494.md)。 默认在 Microsoft 本机最小规则集和 C++ Core Check 类型规则集中分别启用这些修补程序。 有关详细信息，请参阅[新增了对未初始化内存 (C6001) 和初始化前使用 (C26494) 警告的代码分析快速修复](https://devblogs.microsoft.com/cppblog/new-code-analysis-quick-fixes-for-uninitialized-memory-c6001-and-use-before-init-c26494-warnings/)。

### <a name="remote-builds"></a>远程生成

- 现在在 MSBuild 和 CMake 项目中面向 Linux时，用户可将远程生成计算机与远程调试计算机分隔开来。

- 改进了远程连接的日志记录，使用户更易于诊断跨平台开发中的问题。

## <a name="whats-new-for-c-in-visual-studio-version-160"></a>Visual Studio 版本 16.0 中 C++ 的新增功能

有关 Visual Studio 版本 16.0 中的新增功能和 bug 修复的摘要，请参阅 [Visual studio 2019 版本16.0 中的新增功能](/visualstudio/releases/2019/release-notes-v16.0)。

### <a name="c-compiler"></a>C++ 编译器

- 增强了对 C++17 功能和正确性修复的支持，以及对 C++20 功能（如模块和协同例程）的实验性支持。 有关详细信息，请参阅 [Visual Studio 2019 中 C++ 的符合性改进](cpp-conformance-improvements.md)。

- [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 选项现在包含 C++20 功能（不一定完整），包括对用于三向比较的 C++20 运算符 `<=>`（“宇宙飞船”）的初始支持。

- C++ 编译器开关 [`/Gm`](../build/reference/gm-enable-minimal-rebuild.md) 现已弃用。 如果已显式定义 `/Gm` 开关，请考虑在生成脚本中禁用它。 但也可以安全地忽略针对 `/Gm` 的弃用警告，因为在使用“将警告视为错误”([`/WX`](../build/reference/compiler-option-warning-level.md)) 时不会将其视为错误。

- 随着 MSVC 开始在 `/std:c++latest` 标志下实现 C++20 标准草案中的功能，`/std:c++latest` 现与 `/clr`（所有版本）、`/ZW` 和 `/Gm` 不兼容。 在 Visual Studio 2019 中，请在使用 `/clr`、`/ZW` 或 `/Gm` 编译时使用 `/std:c++17` 或 `/std:c++14` 模式（请参阅上一项目符号）。

- 对于 C++ 控制台和桌面应用，默认情况下不再生成预编译头文件。

#### <a name="codegen-security-diagnostics-and-versioning"></a>Codegen、安全性、诊断和版本控制

借助用于为 Spectre Variant 1 提供迁移缓解的 [`/Qspectre`](../build/reference/qspectre.md) 改进分析 ([CVE-2017-5753](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5753))。 有关详细信息，请参阅 [MSVC 中的 Spectre 缓解措施](https://devblogs.microsoft.com/cppblog/spectre-mitigations-in-msvc/)。

### <a name="c-standard-library-improvements"></a>C++ 标准库改进

- 实现了其他 C++17 和 C++20 库功能和正确性修复。 有关详细信息，请参阅 [Visual Studio 2019 中 C++ 的符合性改进](cpp-conformance-improvements.md)。

- Clang-Format 已应用于 C++ 标准库标头，以提高可读性。

- 由于 Visual Studio 现在支持用于 C++ 的“仅我的代码”，因此标准库不再需要为 `std::function` 和 `std::visit` 提供自定义机制，即可实现相同效果。 删除此机制在很大程度上对用户没有明显影响。 只是编译器不再生成诊断来指明 \<type_traits> 或 \<variant> 的行 15732480 或 16707566 存在问题。

### <a name="performancethroughput-improvements-in-the-compiler-and-standard-library"></a>编译器和标准库中的性能提升/吞吐量改进

- 改进生成吞吐量，包括 PDB 类型合并和创建过程中，链接器对文件 I/O 和链接时间的处理方式。

- 添加了对 OpenMP SIMD 矢量化的基本支持。 可以使用新的编译器开关 `/openmp:experimental` 来启用它。 此选项可能使带 `#pragma omp simd` 注释的循环被矢量化。 无法保证矢量化，已注释但未矢量化的循环将收到系统警告。 不支持 SIMD 子句，它们将被忽略并报告警告。

- 新增了内联命令行开关 `/Ob3`，它是 `/Ob2` 的更厉害版本。 `/O2`（优化二进制代码来提高速度）仍默认暗指 `/Ob2`。 如果发现编译器内联得不够厉害，可以考虑传递 `/O2 -Ob3`。

- 现已开始支持 Short Vector Math Library (SVML) 内部函数。 这些函数计算 128 位、256 位或 512 位的向量等效项。 支持此类函数是为了支持手动矢量化循环，循环中包含对数学库函数的调用和某些其他运算（如整数除法）。 有关支持的函数的定义，请参阅 [Intel 内部函数指南](https://software.intel.com/sites/landingpage/IntrinsicsGuide/#!=undefined&techs=SVML)。

- 新的和已改进的优化：

  - 可使用 SIMD 矢量内部函数简化表达式的常量合并和算法（针对浮点和整数形式）。

  - 提供更强大的分析功能，帮助从控制流（if/else/switch 语句）中提取信息以删除被证明为 true 或 false 的分支。

  - 改进了 memset 展开，以使用 SSE2 矢量指令。

  - 改进了无用 struct/class 副本（特别是针对按值传递的 C++ 程序）的删除操作。

  - 使用 `memmove`（例如 `std::copy` 或 `std::vector` 和 `std::string` 构造）优化代码的过程。

- 优化标准库物理设计，以避免不直接包含标准库的编译部分。 此更改将只包含 \<vector> 的空文件的生成时间缩短了一半。 因此，可能需要为之前间接包含的标头添加 `#include` 指令。 例如，现在可能需要为使用 `std::out_of_range` 的代码添加 `#include <stdexcept>`。 现在可能需要为使用流插入运算符的代码添加 `#include <ostream>`。 这样做的好处是，只有真正使用 \<stdexcept> 或 \<ostream> 组件的转换单元，才需要支付吞吐量费用来编译它们。

- `if constexpr` 已应用于标准库中的多个位置，用于在复制操作、反向和旋转等排列操作和并行算法库中提高吞吐量并缩减代码大小。

- 标准库现在内部使用 `if constexpr` 来缩短编译时间，即使是在 C++14 模式下，也不例外。

- 并行算法库的运行时动态链接检测不再使用整个页面来存储函数指针数组。 将此内存设为只读被视为不再与安全目的相关。

- `std::thread` 构造函数不再等待线程启动，并且不再在基础 C 库 `_beginthreadex` 和提供的可调用对象之间插入太多函数调用层。 以前，`std::thread` 在 `_beginthreadex` 和所提供的可调用对象之间放置了六个函数。 这个数字已经减少到只有三个，其中两个就是 `std::invoke`。 此更改还修复了一个难处理的计时 bug，即当系统时钟恰好在创建 `std::thread` 的时刻更改时，`std::thread` 构造函数会停止响应。

- 修复了实现 `std::hash<std::filesystem::path>` 时引入的 `std::hash` 中的性能回归。

- 标准库现在在多个位置使用析构函数（而不是 catch 块）来实现正确性。 此更改改进了调试程序交互：通过标准库在受影响位置上引发的异常，现在显示为从它们的原始引发站点引发，而不是由我们重新引发。 并不是所有标准库 catch 块都被消除了。 我们期望在 MSVC 的后续版本中减少 catch 块的数量。

- `noexcept` 函数内的条件引发导致 `std::bitset` 中出现的不理想 codegen 已通过析出引发路径得以修复。

- `std::list` 和 `std::unordered_*` 系列在多个位置内部使用非调试迭代器。

- 多个 `std::list` 成员已更改为尽可能重用列表节点，而不是解除分配和重新分配它们。 例如，假定 `list<int>` 的大小已经为 3，对 `assign(4, 1729)` 的调用现在覆盖前 3 个列表节点中的 `int` 值，并分配一个包含值 1729 的新列表节点。

- 所有对 `erase(begin(), end())` 的标准库调用都已更改为 `clear()`。

- 在某些情况下，`std::vector` 现在可以更高效地初始化并清除元素。

- 对 `std::variant` 进行了改进，使其更易于优化，从而更好地生成代码。 借助 `std::visit`，现在可更好地进行代码内联。

### <a name="c-ide"></a>C++ IDE

#### <a name="live-share-c-support"></a>Live Share C++ 支持

[Live Share](/visualstudio/liveshare/) 现在支持 C++，使开发者可以使用 Visual Studio 或 Visual Studio Code 进行实时协作。 有关详细信息，请参阅 [Announcing Live Share for C++:Real-Time Sharing and Collaboration](https://devblogs.microsoft.com/cppblog/cppliveshare/)（宣布推出用于 C++ 的 Live Share：实时共享和协作）

#### <a name="template-intellisense"></a>模板 IntelliSense

“模板栏”现使用“速览窗口”用户界面而不是模式窗口，它支持嵌套模板，并且可以将任何默认参数预先填充到“速览窗口”中  。 有关详细信息，请参阅 [Template IntelliSense Improvements for Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/template-intellisense-improvements-for-visual-studio-2019-preview-2/)（Visual Studio 2019 预览版 2 的模板 IntelliSense 改进）。 借助“模板栏”中的“最近使用”下拉列表，可以在以前的示例参数集之间快速切换 。

#### <a name="new-start-window-experience"></a>新启动窗口体验

启动 IDE 时，会看到一个新的“启动”窗口。 在其中可以打开最近使用的项目、从源代码管理克隆代码、将本地代码作为解决方案或文件夹打开，也可以新建项目。 “新项目”对话框也已彻底改造为一种搜索优先的可筛选体验。

#### <a name="new-names-for-some-project-templates"></a>某些项目模板的新名称

我们修改了几个项目模板名称和描述以适应更新的“新建项目”对话框。

#### <a name="various-productivity-improvements"></a>多种工作效率提升

Visual Studio 2019 包含以下功能，有助于使编码更简单、更直观：

- 快速修复：
  - 添加缺少的 `#include`
  - `NULL` 至 `nullptr`
  - 添加缺少的分号
  - 解析缺少的命名空间或范围
  - 替换错误的间接操作数（`*` 替换为 `&` 且 `&` 替换为 `*`）
- 通过悬停在右大括号上，获取块的快速信息
- 速览标题/代码文件
- 使用 `#include` 上的“跳转到定义”可以打开文件

有关详细信息，请参阅 [C++ Productivity Improvements in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/c-productivity-improvements-in-visual-studio-2019-preview-2/)（Visual Studio 2019 预览版 2 中的 C++ 生产力改进）。

### <a name="cmake-support"></a>CMake 支持

- 支持 CMake 3.14

- Visual Studio 现可打开由外部工具（如 CMakeGUI）、自定义元构建系统或自行调用 cmake.exe 的构生成脚本生成的现有 CMake 缓存。

- 改进了 IntelliSense 性能。

- 新的设置编辑器提供了手动编辑 CMakeSettings.json 文件的替代方案，并且可进行一些 CMakeGUI 奇偶校验。

- Visual Studio 可检测 Linux 计算机上是否具有兼容的 CMake 版本，从而帮助你在 Linux 上使用 CMake 启动 C++ 开发。 如果没有兼容的版本，它会自行安装。

- CMakeSettings 中不兼容的设置（例如不匹配的体系结构或不兼容的 CMake 生成器设置）在 JSON 编辑器中显示为波形曲线，并在错误列表中显示错误。

- 对于运行 `vcpkg integrate install` 后在 IDE 中打开的 CMake 项目，将自动为其检测并启用 vcpkg 工具链。 可通过在 CMakeSettings 中指定空工具链文件来关闭此行为。

- CMake 项目现默认启用“仅我的代码”调试。

- 静态分析警告现在后台进行处理，并显示在 CMake 项目的编辑器中。

- 针对 CMake 项目更明晰的有关生成和配置的“开始”及“结束”消息，以及对 Visual Studio 生成进度用户界面的支持。 此外，“工具”>“选项”中现提供 CMake 详细级别设置，用于在输出窗口中自定义 CMake 生成和配置消息的详细级别。

- CMakeSettings.json 现支持 `cmakeToolchain` 设置，无需手动修改 CMake 命令行即可指定工具链。

- 新的“全部生成”菜单快捷方式 Ctrl+Shift+B 。

### <a name="incredibuild-integration"></a>IncrediBuild 集成

IncrediBuild 现在添加为“使用 C++ 的桌面开发”工作负荷中的可选组件。 IncrediBuild 生成监视器已完全集成在 Visual Studio IDE 中。 有关详细信息，请参阅[使用 IncrediBuild 的生成监视器和 Visual Studio 2019 直观呈现生成](https://devblogs.microsoft.com/cppblog/visualize-your-build-with-incredibuilds-build-monitor-and-visual-studio-2019/)。

### <a name="debugging"></a>调试

- 对于在 Windows 上运行的 C++ 应用程序，PDB 文件现可在单独的 64 位进程中加载。 此更改解决了由于调试程序内存不足而导致的一系列故障。 例如，在调试包含大量模块和 PDB 文件的应用程序时。

- “监视”、“自动”和“本地”窗口中启用了搜索功能。

### <a name="windows-desktop-development-with-c"></a>使用 C++ 的 Windows 桌面开发

- 这些 C++ ATL/MFC 向导不再可用：

  - ATL COM+ 1.0 组件向导
  - ATL Active Server Pages 组件向导
  - ATL OLE DB 提供程序向导
  - ATL 属性页向导
  - ATL OLE DB 使用者向导
  - MFC ODBC 使用者
  - ActiveX 控件中的 MFC 类
  - Type Lib 中的 MFC 类。

  这些技术的示例代码存档在 Microsoft Docs 和 VCSamples GitHub 存储库中。

- Visual Studio 安装程序中不再提供 Windows 8.1 软件开发工具包 (SDK)。 建议将 C++ 项目升级到最新的 Windows 10 SDK。 如果在 8.1 上具有硬依赖项，则可以从 Windows SDK 存档下载它。

- Windows XP 目标将不再适用于最新的 C++ 工具集。 仍支持使用 VS 2017 级 MSVC 编译器和库的 XP 目标定向，并且可通过“单个组件”进行安装。

- 我们的文档不推荐使用 Merge 模块部署 Visual C++ 运行时。 此版本执行了额外步骤，将 MSM 标记为已弃用。 请考虑将 VCRuntime 核心部署从 MSM 迁移到可再发行组件包。

### <a name="mobile-development-with-c-android-and-ios"></a>使用 C++ 的移动开发（Android 和 iOS）

现在，C++ Android 体验默认为 Android SDK 25 和 Android NDK 16b。

### <a name="clangc2-platform-toolset"></a>Clang/C2 平台工具集

已删除 Clang/C2 实验性组件。 使用 MSVC 工具集，以完全符合 `/permissive-` 和 `/std:c++17` 的 C++ 标准，或者适用于 Windows 的 Clang/LLVM 工具链。

### <a name="code-analysis"></a>代码分析

- 代码分析现可自动在后台运行。 键入时，警告在编辑器中显示为绿色波形曲线。 有关详细信息，请参阅 [In-editor code analysis in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/in-editor-code-analysis-in-visual-studio-2019-preview-2/)（Visual Studio 2019 预览版 2 中的编辑器内代码分析）。

- 针对 \<mutex> 标头中已知的标准库类型的新实验性 ConcurrencyCheck 规则。 有关详细信息，请参阅 [Concurrency Code Analysis in Visual Studio 2019](https://devblogs.microsoft.com/cppblog/concurrency-code-analysis-in-visual-studio-2019/)（Visual Studio 2019 中的并发代码分析）。

- [生存期配置文件检查器](https://herbsutter.com/2018/09/20/lifetime-profile-v1-0-posted/)的部分更新的实现，可检测无关联指针和引用。 有关详细信息，请参阅 [Lifetime Profile Update in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/lifetime-profile-update-in-visual-studio-2019-preview-2/)（Visual Studio 2019 预览版 2 中的生存期配置文件更新）。

- 更多与协同例程相关的检查，包括 [C26138](../code-quality/c26138.md)、[C26810](../code-quality/c26810.md)、[C26811](../code-quality/c26811.md) 和实验性规则 [C26800](../code-quality/c26800.md)。 有关详细信息，请参阅 [New Code Analysis Checks in Visual Studio 2019: use-after-move and coroutine](https://devblogs.microsoft.com/cppblog/new-code-analysis-checks-in-visual-studio-2019-use-after-move-and-coroutine/)（Visual Studio 2019 中的新代码分析检查：移动后使用和协同程序）。

### <a name="unit-testing"></a>单元测试

托管的 C++ 测试项目模板不再可用。 可在现有项目中继续使用托管的 C++ 测试框架。 对于新的单元测试，请考虑使用 Visual Studio 为其提供模板（MSTest、Google Test）或托管的 C# 测试项目模板的某个本机测试框架。
