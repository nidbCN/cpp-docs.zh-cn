---
title: Visual Studio 2017 中的 C++ 新增功能
description: Visual Studio 2017 中的 Microsoft C/C++ 编译器和工具的新增功能和修复。
ms.date: 03/08/2021
ms.technology: cpp-ide
ms.openlocfilehash: bb99f98482484f46824d16f4e202989b0ec6b8ca
ms.sourcegitcommit: f8ba5db09d05683b24c58505f0e57c21f85545dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103147267"
---
# <a name="whats-new-for-c-in-visual-studio-2017"></a>Visual Studio 2017 中的 C++ 新增功能

Visual Studio 2017 向 C++ 环境引入了许多更新和修补程序。 我们修复了编译器和工具中存在的 250 多个缺陷和报告的问题。 许多问题是客户通过“发送反馈”下的[“报告问题和提供建议”](/visualstudio/ide/how-to-report-a-problem-with-visual-studio?view=vs-2017&preserve-view=true)选项提交的。 感谢你报告 bug！

有关整个 Visual Studio 中新增功能的详细信息，请参阅 [Visual Studio 2017 中的新增功能](/visualstudio/ide/whats-new-visual-studio-2017?view=vs-2017&preserve-view=true)。 有关 Visual Studio 2019 中 C++ 新增功能的信息，请参阅 [Visual Studio 中 C++ 的新增功能](what-s-new-for-visual-cpp-in-visual-studio.md?preserve-view=true&view=msvc-160)。 有关 Visual Studio 2015 及更低版本中 C++ 新增功能的信息，请参阅 [Visual C++ 新增功能 (2003 - 2015)](../porting/visual-cpp-what-s-new-2003-through-2015.md)。 有关 C++ 文档中新增功能的信息，请[参阅 Microsoft C++ 文档：新增功能](whats-new-cpp-docs.md)。

## <a name="visual-studio-2017-c-compiler"></a>Visual Studio 2017 C++ 编译器

### <a name="c-conformance-improvements"></a>C++ 的符合性改进

我们在此版本中更新了 C++ 编译器和标准库，增强了对 C++11 和 C++14 功能的支持。 它还包括对 C++17 标准中预计会有的某些功能的初步支持。 有关详细信息，请参阅 [Visual Studio 2017 中 C++ 的符合性改进](cpp-conformance-improvements.md)。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

编译器支持 C++17 中约 75% 的新功能，其中包括结构化绑定、`constexpr` Lambda、`if constexpr`、内联变量、折叠表达式以及将 `noexcept` 添加到类型系统。 可通过 `/std:c++17` 选项使用这些功能。 有关详细信息，请参阅 [Visual Studio 2017 中 C++ 的符合性改进](cpp-conformance-improvements.md)

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

Visual Studio 15.7 版中的 MSVC 编译器工具集现符合 C++ 标准。 有关详细信息，请参阅[公告：MSVC 符合 C++ 标准](https://devblogs.microsoft.com/cppblog/announcing-msvc-conforms-to-the-c-standard/)和 [Microsoft C++ 语言合规性](./visual-cpp-language-conformance.md)。

##### <a name="visual-studio-2017-version-158"></a>Visual Studio 2017 版本 15.8

[`/experimental:preprocessor`](../build/reference/experimental-preprocessor.md) 编译器开关启用了新的实验性 MSVC 预处理器，它最终将符合所有适用的 C 和 C++ 标准。 有关详细信息，请参阅 [MSVC 新预处理器概述](../preprocessor/preprocessor-experimental-overview.md)。

### <a name="new-compiler-options"></a>新的编译器选项

- [`/permissive-`](../build/reference/permissive-standards-conformance.md)：启用所有严格标准符合性编译器选项，并禁用大部分特定于 Microsoft 的编译器扩展（但有一些例外，比如 `__declspec(dllimport)`）。 在 Visual Studio 2017 15.5 版中此选项默认为开启状态。  `/permissive-` 符合性模式支持两阶段名称查找。 有关详细信息，请参阅 [Visual Studio 中 C++ 的符合性改进](cpp-conformance-improvements.md)。

- [`/diagnostics`](../build/reference/diagnostics-compiler-diagnostic-options.md)：可通过三种不同的方式来显示诊断错误或警告位置：只显示行号；显示行号和列；或显示行号和列，并在违规代码行下方显示脱字号。

- [`/debug:fastlink`](../build/reference/debug-generate-debug-info.md)：通过避免将所有调试信息复制到 PDB 文件，实现最高达 30％ 更快的增量链接时间（与Visual Studio 2015 相比）。 PDB 文件改为指向用于创建可执行文件的对象和库文件的调试信息。 请参阅[使用 `/Debug:fastlink` 在 VS “15”中缩短 C++ 生成周期](https://devblogs.microsoft.com/cppblog/faster-c-build-cycle-in-vs-15-with-debugfastlink/)和[在 Visual Studio 中加速 C++ 生成的建议](https://devblogs.microsoft.com/cppblog/recommendations-to-speed-c-builds-in-visual-studio/)。

- Visual Studio 2017 允许结合使用 [`/sdl`](../build/reference/sdl-enable-additional-security-checks.md) 和 [`/await`](../build/reference/await-enable-coroutine-support.md)。 删除了针对协同例程的 [`/RTC`](../build/reference/rtc-run-time-error-checks.md) 限制。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- [`/std:c++14` 和 `/std:c++latest`](../build/reference/std-specify-language-standard-version.md)：通过这些编译器开关可选择在项目中加入特定版本的 ISO C++ 编程语言。 大多数新的草案标准功能由 `/std:c++latest` 选项保护。

- 借助 [`/std:c++17`](../build/reference/std-specify-language-standard-version.md)，可以使用编译器实现的一组 C++17 功能。 此选项禁用编译器和标准库对 C++17 之后功能的支持：在工作草案的后续版本中更改或新增的功能，以及 C++ 标准的缺陷更新。 若要启用这些功能，请使用 `/std:c++latest`。

### <a name="codegen-security-diagnostics-and-versioning"></a>Codegen、安全性、诊断和版本控制

此版本在优化、代码生成、工具集版本控制和诊断方面做出了若干改进。 显著改进包括：

- 改进了循环的代码生成：支持常量整数除法的自动矢量化，优化了 memset 模式的识别。
- 提高了代码安全性：改进了缓冲区溢出编辑器诊断的输出，[`/guard:cf`](../build/reference/guard-enable-control-flow-guard.md) 现在保护生成转移表的 switch 语句。
- 版本控制：现在，每次更新 Visual C++ 工具集时都将单调更新内置预处理器宏 \_MSC\_VER 的值。 有关详细信息，请参阅 [Visual C++ Compiler Version](https://devblogs.microsoft.com/cppblog/visual-c-compiler-version/)（Visual C++ 编译器版本）。
- 新工具集布局：编译器和相关生成工具在开发计算机上有了新的位置和目录结构。 新布局支持并行安装多个版本的编译器。 有关详细信息，请参阅 [Visual Studio 2017 中的编译器工具布局](https://devblogs.microsoft.com/cppblog/compiler-tools-layout-in-visual-studio-15/)。
- 改进了诊断：输出窗口现在会显示发生错误的列。 有关详细信息，请参阅 [C++ compiler diagnostics improvements in VS “15” Preview 5](https://devblogs.microsoft.com/cppblog/c-compiler-diagnostics-improvements-in-vs-15-rc/)（VS“15”预览版 5 中的 C++ 编译器诊断改进）。
- 当使用协同例程时，（“`/await`”选项下的）实验性关键字“yield”已被删除。 应更新代码，以改为使用 `co_yield`。 有关详细信息，请参阅 [VS 2017 中的 `yield` 关键字变为 `co_yield`](https://devblogs.microsoft.com/cppblog/yield-keyword-to-become-co_yield-in-vs-2017/)。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

针对编辑器中的诊断的其他改进。 有关详细信息，请参阅 [Visual Studio 2017 15.3.0 中的诊断改进](https://devblogs.microsoft.com/cppblog/diagnostic-improvements-in-vs2017-15-3-0/)。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

通过提高生成的代码质量，Visual C++ 运行时性能不断提升。 现在只需重新编译代码，即可加快应用的运行速度。 编译器的某些优化是前所未有的，例如条件标量存储的矢量化，将 `sin(x)` 和 `cos(x)` 调用合并到新的 `sincos(x)` 中，以及删减了 SSA 优化器中多余的说明。 其他编译器优化是对现有功能进行了改进，如条件表达式的矢量化器启发、改进了循环优化和浮动最小/最大 CodeGen。 链接器有一个更快的新 `/OPT:ICF` 实现，最多可带来 9% 的链接时间加速，并且在增量链接中还有其他性能修复。 有关详细信息，请参阅 [/OPT（优化）](../build/reference/opt-optimizations.md)和 [/INCREMENTAL（增量链接）](../build/reference/incremental-link-incrementally.md)。

Microsoft C++ 编译器支持 Intel AVX-512。 它有矢量长度指令，可以将 AVX-512 中的新函数引入 128 位宽和 256 位宽的寄存器。

通常，在使用 C++17 模式时，可使用 [`/Zc:noexceptTypes-`](../build/reference/zc-noexcepttypes.md) 选项恢复到 `noexcept` 的 C++14 版本。 此选项可以将源代码更新为符合 C++17，而无需同时重写所有 `throw()` 代码。 有关详细信息，请参阅[动态异常规范的删除和 noexcept](cpp-conformance-improvements-2017.md#noexcept_removal)。

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

- 新编译器开关 [/Qspectrum](../build/reference/qspectre.md) 可帮助缓解潜在的执行旁道攻击。 有关详细信息，请参阅 [MSVC 中的 Spectre 缓解措施](https://devblogs.microsoft.com/cppblog/spectre-mitigations-in-msvc/)。
- Spectre 缓解的新诊断警告。 有关详细信息，请参阅 [Visual Studio 2017 15.7 版预览版 4 中的 Spectre 诊断](https://devblogs.microsoft.com/cppblog/spectre-diagnostic-in-visual-studio-2017-version-15-7-preview-4/)。
- 通过 /Zc 的新值 `/Zc:__cplusplus`，可启用 C++ 标准支持的正确报告。 例如，如果设置了开关，且编译器处于 `/std:c++17` 模式，那么此值会扩展为 `201703L`。 有关详细信息，请参阅 [MSVC 现可正确报告 __cplusplus](https://devblogs.microsoft.com/cppblog/msvc-now-correctly-reports-__cplusplus/)。

## <a name="c-standard-library"></a>C++ 标准库

### <a name="correctness-improvements"></a>正确性改进

##### <a name="visual-studio-2017-rtm-version-150"></a>Visual Studio 2017 RTM（版本 15.0）

- 次要 `basic_string``_ITERATOR_DEBUG_LEVEL != 0` 诊断改进。 当 IDL 检查在字符串机制中失误时，它现在会报告导致失误的特定行为。 例如，现在会收到“无法取消引用字符串迭代器，因为其已超出范围（例如末尾迭代器）”，而不是“字符串迭代器不可取消引用”。
- 修复了会导致代码永久受阻的 `std::promise` 移动赋值运算符。
- 修复了编译器错误，将 `atomic<T*>` 隐式转换为 `T*`。
- `pointer_traits<Ptr>` 现可正确检测 `Ptr::rebind<U>`。
- 修复了 `move_iterator` 减法运算符中缺少 `const` 限定符的问题。
- 在需要 `propagate_on_container_copy_assignment` 和 `propagate_on_container_move_assignment` 的用户定义的有状态分配器中，修复了损坏的无效 codegen。
- `atomic<T>` 现可容忍重载的 `operator&()`。
- 对于错误的 `bind()` 调用，略微改进了其编译器诊断。

Visual Studio 2017 RTM 中有更多的标准库改进。 有关完整列表，请参阅 C++ 团队博客条目 [VS 2017 RTM 中的标准库修补程序](https://devblogs.microsoft.com/cppblog/stl-fixes-in-vs-2017-rtm/)。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 标准库容器现将其 `max_size()`（而非 `size_type` 的 `max()`）固定到 `numeric_limits<difference_type>::max()` 中。 此更改可确保迭代器上来自该容器的 `distance()` 的结果可使用 `distance()` 的返回类型表示。
- 修复了缺少专用化 `auto_ptr<void>` 的问题。
- 以前，如果 length 参数不是整型类型，`for_each_n()`、`generate_n()` 和 `search_n()` 算法就无法进行编译。 现在，它们尝试将非整型长度转换为迭代器的 `difference_type`。
- `normal_distribution<float>` 不再在标准库中发出关于从双精度型收缩为浮点型的警告。
- 修复了一些在检查最大大小溢出时使用 `npos` 而不是 `max_size()` 的 `basic_string` 操作。
- 以前，`condition_variable::wait_for(lock, relative_time, predicate)` 会在出现虚假唤醒时等待整个相对时间。 现在，它只等待单个相对时间间隔。
- `future::get()` 现按标准版要求，使 `future` 失效。
- `iterator_traits<void *>` 曾经是一个硬错误，它尝试形成 `void&`；而现在，它完全变成了一个空结构，允许在 “is iterator”SFINAE 条件中使用 `iterator_traits`。
- 修复了 Clang **-Wsystem-headers** 报告的一些警告。
- 还修复了 Clang **-Wmicrosoft-exception-spec** 报告的“声明中的异常规范与以前的声明不匹配”这一问题。
- 还修复了 Clang 和 C1XX 报告的 mem-initializer-list 排序警告。
- 当容器本身交换时，无序容器不会交换其哈希函数或谓词。 现在它们会进行交换。
- 现在，许多容器交换操作都被标记为 `noexcept`（因为标准库从未打算在检测到非 `propagate_on_container_swap` 不相等分配器未定义的行为条件时抛出异常）。
- 许多 `vector<bool>` 操作现在都被标记为 `noexcept`。
- 标准库现在将使用选择退出的转义交错线来强制执行匹配的分配器 `value_type`（在 C++17 模式中）。
- 修复了一些将 self-range 插入 `basic_string` 会打乱字符串内容的情况。 （注意：标准库仍禁止将 self-range 插入到矢量）。
- `basic_string::shrink_to_fit()` 不再受分配器的 `propagate_on_container_swap` 影响。
- `std::decay` 现可处理令人烦恼的函数类型（即 cv 限定和/或 ref 限定的函数类型）。
- 更改了 include 指令以正确使用区分大小写和正斜杠，从而提高可移植性。
- 修复了警告 C4061“枚举‘*enumeration*’的 switch 中的枚举器‘*enumerator*’未按 case 标签进行显式处理”。 此警告默认关闭，并且已作为标准库警告常规策略的一个异常进行了修复。 （标准库是不含 `/W4`，但并不会试图不含 `/Wall`。 许多默认关闭的警告的干扰性都异常高，不打算经常使用。）
- 改进了 `std::list` 调用检查。 列表迭代器现检查 `operator->()`，且 `list::unique()` 现将迭代器标记为无效。
- 修复了元组中的 `tuple` 元编程。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- `std::partition` 现在调用谓词 `N` 次，而不是 `N + 1` 次（按标准要求）。
- 已在版本 15.5 中修复版本 15.3 中避免神奇的静态对象的尝试。
- `std::atomic<T>` 不再需要 `T` 默认可构造。
- 在迭代器调试启用后，需要使用对数时间的堆算法的行为有所不同。 它们不再做输入实际上是堆的线性时间断言。
- `__declspec(allocator)` 现仅针对 C1XX 进行保护，防止出现来自不了解此 declspec 的 Clang 的警告。
- `basic_string::npos` 现在可作为编译时常数提供。
- C++17 模式中的 `std::allocator` 现在正确处理过度对齐的类型（即其对齐高于 `max_align_t` 的类型）的分配，除非由 `/Zc:alignedNew-` 禁用。  例如，对于 SSE 和 AVX 说明，现在将正确对齐采用 16 或 32 字节对齐的对象的矢量。

### <a name="conformance-improvements"></a>符合性改进

- 我们添加了 \<any\>、\<string_view\>、`apply()`、`make_from_tuple()`。
- 添加了 \<optional\>、\<variant\>、`shared_ptr::weak_type` 和 \<cstdalign\>。
- 在 `min(initializer_list)`、`max(initializer_list)`、`minmax(initializer_list)`、`min_element()`、`max_element()` 和 `minmax_element()` 中启用了 C++14 `constexpr`。

有关详细信息，请参阅 [Microsoft C++ 语言一致性表](./visual-cpp-language-conformance.md)。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 已经实现了几个其他的 C++17 功能。 有关详细信息，请参阅 [Microsoft C++ 语言一致性表](cpp-conformance-improvements-2017.md#improvements_153)。
- 实现了 P0602R0“变体和可选项应传播副本/移动琐碎事项”。
- 现在，标准库正式允许通过 [/GR-](../build/reference/gr-enable-run-time-type-information.md) 选项禁用动态 RTTI。 由于 `dynamic_pointer_cast()` 和 `rethrow_if_nested()` 本质上都需要 `dynamic_cast`，因此标准库现在将它们标记为 `/GR-` 下的 `=delete`。
- 即使已通过 `/GR-` 禁用了动态 RTTI，“静态 RTTI”（采用 `typeid(SomeType)` 形式）仍可用，并为多个标准库组件提供技术支持。 现在，标准库也支持通过 `/D_HAS_STATIC_RTTI=0` 禁用此功能。 此标志还将禁用 `std::any`、`std::function` 的 `target()` 和 `target_type()` 成员函数，以及 `std::shared_ptr` 和 `std::weak_ptr` 的 `get_deleter()` 友元成员函数。
- 标准库现在无条件地使用 C++14 `constexpr`，而不是有条件定义的宏。
- 标准库现在内部使用别名模板。
- 标准库现在内部使用 `nullptr`，而不是 `nullptr_t{}`。 （已根除 NULL 的内部使用。 正在逐渐清理 0 作为 null 的内部使用。）
- 标准库现在内部使用 `std::move()`，而不是在风格上误用 `std::forward()`。
- 将 `static_assert(false, "message")` 更改为了 `#error message`。 此更改提高了编译器诊断，因为 `#error` 立即停止编译。
- 标准库不再将函数标记为 `__declspec(dllimport)`。 新式链接器技术不再需要此操作。
- 已将 SFINAE 提取到默认模板参数，与返回类型和函数参数类型相比，这可以减少混乱。
- \<random\> 中的调试检查现在使用标准库的常用机制，而不是使用将 `fputs()` 调用到 stderr 的内部函数 `_Rng_abort()`。 此函数的实现是为了二进制兼容性而保留的。 我们将在下一个二进制不兼容版本的标准库中删除它。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- 根据 C++17 标准，添加、弃用或删除了多个标准库功能。 有关详细信息，请参阅 [Visual Studio 中 C++ 的符合性改进](cpp-conformance-improvements-2017.md#improvements_155)。
- 以下并行算法的实验支持：
  - `all_of`
  - `any_of`
  - `for_each`
  - `for_each_n`
  - `none_of`
  - `reduce`
  - `replace`
  - `replace_if`
  - `sort`
- 添加了以下并行算法的签名，但暂未进行并行化。 分析表明，只移动或重新排列元素的并行算法并无益处：
  - `copy`
  - `copy_n`
  - `fill`
  - `fill_n`
  - `move`
  - `reverse`
  - `reverse_copy`
  - `rotate`
  - `rotate_copy`
  - `swap_ranges`

##### <a name="visual-studio-2017-version-156"></a>Visual Studio 2017 版本 15.6

- \<memory_resource>
- Library Fundamentals V1
- 正在删除 `polymorphic_allocator` 分配
- 改进类模板参数推导

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

- 对并行算法的支持不再是实验性的
- \<filesystem> 的新实现
- 基本字符串转换（部分）
- `std::launder()`
- `std::byte`
- `hypot(x,y,z)`
- 避免不必要的衰减
- 数学特殊函数
- `constexpr char_traits`
- 标准库的推导指南

有关详细信息，请参阅 [Microsoft C++ 语言一致性表](./visual-cpp-language-conformance.md)。

### <a name="performance-and-throughput-fixes"></a>性能和吞吐量修复

- 仅在调用 `traits::find` 一次后执行 `basic_string::find(char)` 重载。 以前会将此操作实施为针对长度为 1 的字符串的常规字符串搜索。
- `basic_string::operator==` 现先检查字符串的大小，然后再比较字符串的内容。
- 删除了 `basic_string` 中编译器优化程序难以分析的控制耦合。 对任意短字符串调用 `reserve` 时，即使不执行操作，也会耗费资源。
- 为了提高正确性和性能，`std::vector` 已全面改进：现可按标准版的要求正确处理在插入和安置操作期间的别名，在标准版需要时现可通过 `move_if_noexcept()` 和其他逻辑提供强异常保证，且减少了插入和安置所执行的元素操作。
- 现在 C++ 标准库会避免取消引用 null 复杂精致指针。
- 提升了 `weak_ptr::lock()` 性能。
- 为提高编译器吞吐量，C++ 标准库标头现不会包含非必需编译器内部函数的声明。
- 将 `std::string` 和 `std::wstring` 移动构造函数的性能提升了三倍以上。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 解决了与 `noexcept` 的交互，避免了将 `std::atomic` 实现内联到使用结构化异常处理 (SEH) 的函数中。
- 更改了标准库的内部 `_Deallocate()` 函数，优化为较小的代码，从而能将其内联到更多的位置。
- 更改了 `std::try_lock()` 以使用包扩展，而不是递归。
- 改进了 `std::lock()` 的死锁避免算法以使用 `lock()` 操作，而不是在所有锁的 `try_lock()` 上旋转。
- 在 `system_category::message()` 中实现了命名返回值优化。
- `conjunction` 和 `disjunction` 现在实例化 `N + 1` 类型，而不是 `2N + 2` 类型。
- `std::function` 不再实例化每个类型擦除的可调用项的分配器支持机制，从而提高吞吐量，并缩减将许多不同的 lambda 传递给 `std::function` 的程序中的 .obj 大小。
- `allocator_traits<std::allocator>` 包含自动内联的 `std::allocator` 操作，从而缩减仅通过 `allocator_traits` 与 `std::allocator` 交互的代码中的代码大小（即在大部分代码中）。
- C++11 最小分配器接口现在由标准库直接调用 `allocator_traits` 进行处理，而不是将分配器包装在内部类 `_Wrap_alloc` 中。 此更改减少了为分配器支持生成的代码大小，提高了优化器在某些情况下对标准库容器进行推理的能力，并提供了更好的调试体验（正如现在看到了分配器类型，而不是调试器中的 `_Wrap_alloc<your_allocator_type>`）。
- 删除了自定义 `allocator::reference` 的元编程，不允许自定义分配器。 （分配器可以使容器使用设想的指针，但不可以使用设想的引用。）
- 编译器前端已学会在基于范围的 for 循环中展开调试迭代器，从而提高调试版本的性能。
- `shrink_to_fit()` 和 `reserve()` 的 `basic_string` 内部收缩路径不再位于重新分配操作的路径中，从而减少所有转变成员的代码大小。
- `basic_string` 的内部增长路径不再位于 `shrink_to_fit()` 的路径中。
- `basic_string` 的转变操作现在已纳入到非分配快速路径和分配慢速路径函数，从而使其更有可能将常见的无重新分配的情况内联到调用方中。
- `basic_string` 的转变操作现在构造首选状态的重新分配的缓冲区，而不是就地调整大小。 例如，如果在字符串开头插入，现在只在插入后移动内容一次。 它要么向下移动，要么移动到新分配的缓冲区。 在重新分配的情况下，它不再移动两次（即先移动到新分配的缓冲区，再向下移动）。
- 在 \<string\> 中调用 C 标准库的操作现在缓存 `errno` 的地址以消除与 TLS 的重复交互。
- 简化了 `is_pointer` 实现。
- 完成了将基于函数的表达式 SFINAE 更改为基于 `struct` 和 `void_t`。
- 标准库算法现在避免使用后增量迭代器。
- 修复了在 64 位系统上使用 32 位分配器时的截断警告。
- 通过重复使用缓冲区（如果可能），在非 POCMA 非等分配器情况下，`std::vector` 移动赋值现在更高效。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- `basic_string<char16_t>` 现与 `basic_string<wchar_t>` 占用相同的 `memcmp`、`memcpy` 和类似优化。
- 已暂时避开阻止函数指针被 Visual Studio 2015 Update 3“避免复制函数”工作内联公开的优化器限制，从而还原 `lower_bound(iter, iter, function pointer)` 的性能。
- 将迭代器解包后再检查顺序，降低了向 `includes`、`set_difference`、`set_symmetric_difference` 和 `set_union` 的输入执行迭代器调试顺序验证的开销。
- `std::inplace_merge` 现在跳过已就位的元素。
- 构造 `std::random_device` 时不再构造再销毁 `std::string`。
- `std::equal` 和 `std::partition` 有跳转线程优化传递，可以免去一次迭代器比较。
- 当 `std::reverse` 将指针传递到完全可复制的 `T` 时，它将分派至一个手写的矢量化实现。
- 过去，`std::fill`、`std::equal` 和 `std::lexicographical_compare` 被指示如何调度到 `std::byte` 和 `gsl::byte` 的 `memset` 和 `memcmp`（及其他类似 char 的枚举和 enum 类）。 由于 `std::copy` 使用 `is_trivially_copyable` 进行调度，因此无需任何更改。
- 标准库不再包含仅有的行为是使类型不完全易损坏的空大括号析构函数。

## <a name="other-libraries"></a>其他库

### <a name="open-source-library-support"></a>开源库支持

Vcpkg 是一款开源命令行工具，能极大简化在 Visual Studio 中获取和生成开源 C++ 静态库和 DLLS 的过程。 有关详细信息，请参阅[vcpkg：适用于 C++ 的包管理器](../build/vcpkg.md)。

### <a name="cpprest-sdk-290"></a>CPPRest SDK 2.9.0

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

CPPRestSDK（C++ 的跨平台 Web API）已更新到版本 2.9.0。 有关详细信息，请参阅 [CppRestSDK 2.9.0 is available on GitHub](https://devblogs.microsoft.com/cppblog/cpprestsdk-2-9-0-is-available-on-github/)（GitHub 上提供了 CppRestSDK 2.9.0）。

### <a name="atl"></a>ATL

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- 另外进行了另一组名称查找符合性修复
- 现有的移动构造函数和移动赋值运算符现已正确地标记为非引发
- 取消禁止有关 atlstr.h 中本地静态变量的线程安全初始化的有效警告 C4640
- 以前，在使用 ATL 生成 DLL 时，本地静态变量的线程安全初始化在 XP 工具集中自动关闭。 现在不是。 如果不需要线程安全初始化，则可以在“项目”设置中添加 `/Zc:threadSafeInit-`。

### <a name="visual-c-runtime"></a>Visual C++ 运行时

- 为控制流防护符号新增了标头“cfguard.h”。

## <a name="visual-studio-2017-c-ide"></a>Visual Studio 2017 C++ IDE

- 现针对 C++ 本机项目和 C++ /CLI 项目有了更佳的配置更改性能，后者的性能增加更为明显。 第一次激活解决方案配置时，现在的速度会更快，且此解决方案配置的所有后续激活几乎可瞬时完成。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 多个项目和代码向导已按照签名对话框样式重新编写。
- “添加类”现在直接启动“添加类向导”。 以前此处的其他所有项现在位于“添加”>“新建项”。
- Win32 项目现位于“新建项目”对话框中的“Windows 桌面”类别下 。
- Windows 控制台和桌面应用程序模板现可在不显示向导的情况下创建项目 。 现在，在相同类别下有一个新的 Windows 桌面向导，显示与旧版 Win32 控制台应用程序向导相同的选项 。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

多种使用 IntelliSense 引擎重构和导航代码的 C++ 操作运行速度更快。 以下数字基于有 3500 个项目的 Visual Studio Chromium 解决方案：

| 功能 | 性能改进 |
|--|--|
| 重命名 | 5.3 倍 |
| 更改签名 | 4.5 倍 |
| 查找所有引用 | 4.7 倍 |

C++ 现在支持通过“Ctrl+单击”转到定义，使利用鼠标导航到定义更轻松。 Productivity Power Tools 包的结构可视化工具现在也默认包含在产品中。

## <a name="intellisense"></a>IntelliSense

- 现在默认使用全新的基于 SQLite 的数据库引擎。 新引擎加快了数据库操作（如“转到定义”和“查找所有引用”）的速度。 它显著缩短了初始解决方案分析时间。 此设置已移动到“工具”>“选项”>“文本编辑器”>“C/C++”>“高级”。 （它以前位于...“C/C++”>“实验性”下。）

- 我们改进了不使用预编译标头的项目和文件的 IntelliSense 性能 - 为当前文件中的标头创建自动预编译标头。

- 还为错误列表中的 IntelliSense 错误添加了错误筛选和帮助。 单击错误列现在允许进行筛选。 此外，单击特定错误或按 F1 将启动错误消息的联机搜索。

  ![错误列表](media/ErrorList1.png "错误列表")

  ![筛选的错误列表](media/ErrorList2.png "筛选的错误列表")

- 增添了按类型筛选“成员列表”项的功能。

  ![成员列表筛选](media/mlfiltering.png "成员列表筛选")

- 添了新的实验性预测 IntelliSense 功能，此功能可根据上下文筛选成员列表中的所示内容。 有关详细信息，请参阅 [C++ IntelliSense 改进 - 预测 IntelliSense 和筛选](https://devblogs.microsoft.com/cppblog/c-intellisense-improvements-predictive-intellisense-filtering/)。
- 即使在复杂基本代码中，“查找所有引用”(Shift+F12) 现在也可帮助轻松进行查找。 它提供高级分组、筛选、排序、在结果中搜索以及（适用于某些语言的）着色，以便你清楚了解自己的引用。 对于 C++ 而言，新的 UI 包括有关是否要从变量读取或向其写入的信息。
- 已将“点到箭头”IntelliSense 功能从实验级提升为高级，且现在为默认启用。 编辑器功能“展开作用域”和“展开优先级”也已从实验级提升为高级 。
- 实验性的重构功能“更改签名”和“提取函数”现默认可用 。
- 添加了面向 C++ 项目的实验性“快速项目加载”功能。 下次打开 C++ 项目时，加载速度将更快，而再下一次的加载速度要快得多！
- 其中一些功能与其他语言通用，有些则特定于 C++。 有关这些新增功能的详细信息，请参阅[发布 Visual Studio“15”预览版 5](https://devblogs.microsoft.com/visualstudio/announcing-visual-studio-15-preview-5/)。

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

- 添加了对 ClangFormat 的支持。 有关详细信息，请参阅 [ClangFormat support in Visual Studio 2017](https://devblogs.microsoft.com/cppblog/clangformat-support-in-visual-studio-2017-15-7-preview-1/)（Visual Studio 2017 中的 ClangFormat 支持）。

## <a name="non-msbuild-projects-with-open-folder"></a>包含“打开文件夹”的非 MSBuild 项目

Visual Studio 2017 引入了“打开文件夹”功能。 这样，可以在包含源代码的文件夹中进行编码、生成和调试，而无需创建任何解决方案或项目。 现在使用 Visual Studio 变得更简单，即使你的项目不基于 MSBuild 也是如此。 使用“打开文件夹”，可以访问强大的代码理解、编辑、生成和调试功能。 这些是 Visual Studio 已为 MSBuild 项目提供的功能。 有关详细信息，请参阅 [C++ 的“打开文件夹”项目](../build/open-folder-projects-cpp.md)。

- 改进了“打开文件夹”体验。 可通过以下 .json 文件自定义体验：
  - 使用 CppProperties.json 可自定义 IntelliSense 和浏览体验。
  - 使用 Tasks.json 可自定义生成步骤。
  - 使用 Launch.json 可自定义调试体验。

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 改进了对备用编译器和生成环境（如 MinGW 和 Cygwin）的支持。 有关详细信息，请参阅[将 MinGW 和 Cygwin 与 Visual C++ 和“打开文件夹”结合使用](https://devblogs.microsoft.com/cppblog/using-mingw-and-cygwin-with-visual-cpp-and-open-folder/)。
- 添加了支持，以定义 CppProperties.json 和 CMakeSettings.json 中的全局和特定于配置的环境变量。 launch.vs.json 中定义的调试配置和 tasks.vs.json 中的任务可以使用这些环境变量。 有关详细信息，请参阅[Customizing your Environment with Visual C++ and Open Folder](https://devblogs.microsoft.com/cppblog/customizing-your-environment-with-visual-c-and-open-folder/)（使用 Visual C++ 和“打开文件夹”自定义环境）。
- 改进了对 CMake 的 Ninja 生成器的支持，包括轻松定位 64 位平台的能力。

## <a name="cmake-support-via-open-folder"></a>通过“打开文件夹”支持 CMake

Visual Studio 2017 支持在不转换为 MSBuild 项目文件 (.vcxproj) 的情况下使用 CMake 项目。 有关详细信息，请参阅 [Visual Studio 中的 CMake 项目](../build/cmake-projects-in-visual-studio.md)。 使用“打开文件夹”打开 CMake 项目会自动配置用于 C++ 编辑、生成和调试的环境。

- 无需在根文件夹中创建 CppProperties.json 文件，C++ IntelliSense 便可正常工作。 我们还增添了一个新的下拉列表，允许用户在分别由 CMake 和 CppProperties.json 文件提供的配置之间轻松切换。

- 通过 CMakeLists.txt 文件所在的同一文件夹中的 CMakeSettings.json 文件提供进一步的配置支持。

  ![CMake 项目使用“打开文件夹”](media/cmake-cpp.png "Cmake 打开文件夹")

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 添加了对 CMake Ninja 生成器的支持。

##### <a name="visual-studio-2017-version-154"></a>Visual Studio 2017 版本 15.4

- 添加了对导入现有 CMake 缓存的支持。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- 添加了对CMake 3.11、CMake 项目中的代码分析、解决方案资源管理器中的“目标”视图、缓存生成选项以及单个文件编译的支持。 有关详细信息，请参阅 [Visual Studio 中的 CMake 支持](https://devblogs.microsoft.com/cppblog/cmake-support-in-visual-studio-targets-view-single-file-compilation-and-cache-generation-settings/)和 [Visual Studio 中的 CMake 项目](../build/cmake-projects-in-visual-studio.md)。

## <a name="windows-desktop-development"></a>Windows 桌面开发

现提供原始 C++ 工作流的更细化的安装体验。 我们添加了可选组件，使你能够仅安装所需工具。 在安装程序 UI 中列出的组件的安装大小指示是不正确的，并低估了总大小。

若要在 C++ 桌面工作负载中成功创建 Win32 项目，则必须安装工具集和 Windows SDK。 安装推荐（选中）的组件“VC++ 2017 v141 工具集（x86、x64）”和“Windows 10 SDK (10.0.nnnnn)”可以确保正常运行 。 如果没有安装必要的工具，就无法成功创建项目，且向导将停止响应。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

Visual C++ 生成工具（以前作为单独的产品提供）现在作为工作负荷包括在 Visual Studio 安装程序中。 此工作负荷仅安装生成 C++ 项目所需的工具，而不安装 Visual Studio IDE。 包括 v140 和 v141 工具集。 v141 工具集包含 Visual Studio 2017 版本 15.5 中的最近改进。 有关详细信息，请参阅 [Visual Studio Build Tools now include the VS2017 and VS2015 MSVC Toolsets](https://devblogs.microsoft.com/cppblog/visual-studio-build-tools-now-include-the-vs2017-and-vs2015-msvc-toolsets/)（Visual Studio 生成工具现在包含 VS2017 和 VS2015 MSVC 工具集）。

## <a name="linux-development-with-c"></a>使用 C++ 的 Linux 开发

热门扩展“[用于 Linux 开发的 Visual C++](https://marketplace.visualstudio.com/items?itemName=VisualCppDevLabs.VisualCforLinuxDevelopment)”现已纳入 Visual Studio。 此安装提供开发和调试运行在 Linux 环境中的 C++ 应用程序所需的一切信息。

##### <a name="visual-studio-2017-version-152"></a>Visual Studio 2017 版本 15.2

在跨平台代码共享和类型可视化方面进行了改进。 有关详细信息，请参阅[跨平台代码共享和类型可视化的 Linux C++ 改进](https://devblogs.microsoft.com/cppblog/linux-cross-platform-and-type-visualization/)。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- Linux 工作负荷添加了对 rsync 作为 sftp 的替代方法将文件同步到远程 Linux 计算机的支持 。
- 添加了对针对 ARM 微控制器的交叉编译的支持。 要在安装中启用此支持，请选择使用 C++ 工作负载的 Linux 开发，并选择“嵌入和 IoT 开发”选项 。 此选项将 ARM GCC 交叉编译工具和 Make 添加到你的安装。 有关详细信息，请参阅 [ARM GCC Cross Compilation in Visual Studio](https://devblogs.microsoft.com/cppblog/arm-gcc-cross-compilation-in-visual-studio/)（Visual Studio 中的 ARM GCC 交叉编译）。
- 添加了对 CMake 的支持。 现在即可处理现有的 CMake 基本代码，无需将其转换为 Visual Studio 项目。 有关详细信息，请参阅[配置 Linux CMake 项目](../linux/cmake-linux-project.md)。
- 添加了对运行远程任务的支持。 此功能允许在 Visual Studio 连接管理器定义的远程系统上运行任意命令。 远程任务还提供将文件复制到远程系统的功能。
有关详细信息，请参阅[配置 Linux CMake 项目](../linux/cmake-linux-project.md)。

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

- 对 Linux 工作负载方案的各种改进。 有关详细信息，请参阅 [Linux C++ Workload improvements to the Project System, Linux Console Window, rsync and Attach to Process](https://devblogs.microsoft.com/cppblog/linux-c-workload-improvements-to-the-project-system-linux-console-window-rsync-and-attach-to-process/)（Linux C++ 工作负载对 Project System、Linux Console Window、rsync 和 Attach to Process 的改进）。
- 远程 Linux 连接上标头的 IntelliSense。 有关详细信息，请参阅[配置 Linux CMake 项目](../linux/cmake-linux-project.md)和 [IntelliSense for Remote Linux Headers](https://devblogs.microsoft.com/cppblog/intellisense-for-remote-linux-headers/)（远程 Linux 标头的 IntelliSense）。

## <a name="game-development-with-c"></a>使用 C++ 的游戏开发

以 DirectX 或 Cocos2d 为后盾，利用 C++ 的强大功能构建专业游戏。

## <a name="mobile-development-with-c-for-android-and-ios"></a>使用适用于 Android 和 iOS 的 C++ 的移动开发

现可利用 Visual Studio 创建和调试面向 Android 和 iOS 的移动应用。

## <a name="universal-windows-apps"></a>通用 Windows 应用

C++ 是通用 Windows 应用工作负荷的可选组件。 目前，必须手动升级 C++ 项目。 可以在 Visual Studio 2017 中打开定目标到 v140 的通用 Windows 平台项目。 不过，如果没有安装 Visual Studio 2015，则需要在项目属性页中选择 v141 平台工具集。

## <a name="new-options-for-c-on-universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP) 上 C++ 的新选项

现可使用新选项编写和打包面向通用 Windows 平台和 Microsoft Store 的 C++ 应用程序：借助桌面桥基础结构，可打包现有的桌面应用程序或 COM 对象，用于通过 Microsoft Store 部署。 或者，用于借助旁加载通过现有通道部署。 Windows 10 中的新功能使你能够以各种方式将 UWP 功能添加到桌面应用程序。 有关详细信息，请参阅[桌面桥](/windows/uwp/porting/desktop-to-uwp-root)。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

添加了“Windows 应用程序打包项目”项目模板，极大地简化了使用桌面桥打包桌面应用程序的工作。 可在“文件 | 新建 | 项目 | 已安装 | Visual C++ | 通用 Windows 平台”下获得此模板。 有关详细信息，请参阅[使用 Visual Studio（桌面桥）打包应用](/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)。

在编写新代码时可使用 C ++ / WinRT，它是 Windows 运行时的标准 C ++ 语言投影，仅在头文件中实现。 它可便于使用任何符合标准的 C++ 编译器来使用和创作 Windows 运行时 API。 C++/WinRT 旨在为 C++ 开发人员提供对新式 Windows API 的优先访问权限。 有关详细信息，请参阅 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)。

自 Windows SDK Insider Preview 内部版本 17025 起，C++/WinRT 包含在 Windows SDK 中。 有关详细信息，请参阅 [C++/WinRT is now included the Windows SDK](https://devblogs.microsoft.com/cppblog/cppwinrt-is-now-included-the-windows-sdk/)（C++/WinRT 现在包含在 Windows SDK 中）。

## <a name="the-clangc2-platform-toolset"></a>Clang/C2 平台工具集

Visual Studio 2017 随附的 Clang/C2 工具集现在支持 `/bigobj` 开关，这对生成大项目来说至关重要。 它还在编译器前端和后端进行了多项重要的缺陷修复。

## <a name="c-code-analysis"></a>C++ 代码分析

用于强制执行 [C++ 核心准则](https://github.com/isocpp/CppCoreGuidelines) 的 C++ 核心检查器现已通过 Visual Studio 分发。 在项目属性页的“代码分析扩展”页中，启用检查器。 然后，在运行代码分析时，包括这些扩展。 有关详细信息，请参阅[使用 C++ 核心准则检查器](../code-quality/using-the-cpp-core-guidelines-checkers.md)。

![显示已选中“配置属性”>“代码分析”>“常规”以及“运行此规则集”部分中列出的核心检查数的“属性页”对话框的屏幕截图。](media/CppCoreCheck.png "CppCoreCheck 属性页")

##### <a name="visual-studio-2017-version-153"></a>Visual Studio 2017 版本 15.3

- 添加了对与资源管理相关的规则的支持。

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

- 新的 C++ Core Guidelines 检查涉及智能指针正确性、全局初始值设定项的正确使用，以及标记构造的使用（如 `goto` 和错误强制转换）。

- 15.3 中可能存在的一些警告编号在 15.5 中不再可用。 这些警告被更具体的检查替换。

##### <a name="visual-studio-2017-version-156"></a>Visual Studio 2017 版本 15.6

- 添加了对单个文件分析的支持，并改进了分析运行时性能。 有关详细信息，请参阅[C++ Static Analysis Improvements for Visual Studio 2017 15.6 Preview 2](https://devblogs.microsoft.com/cppblog/c-static-analysis-improvements-for-visual-studio-2017-15-6-preview-2/)（Visual Studio 2017 15.6 预览版 2 的 C++ 静态分析改进）

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

- 添加了对 [`/analyze:ruleset`](../build/reference/analyze-code-analysis.md) 的支持，它允许指定要运行的代码分析规则。
- 添加了对其他 C++ Core Guidelines 规则的支持。  有关详细信息，请参阅[使用 C++ 核心准则检查器](../code-quality/using-the-cpp-core-guidelines-checkers.md)。

## <a name="unit-testing-in-visual-studio-2017"></a>Visual Studio 2017 中的单元测试

##### <a name="visual-studio-2017-version-155"></a>Visual Studio 2017 版本 15.5

Google Test Adapter 和 Boost.Test Adapter 现在作为“使用 C++ 的桌面开发”工作负载的组件提供。 它们与测试资源管理器集成。 添加了对 CMake 项目（使用“打开文件夹”）的 CTest 支持，尽管与测试资源管理器的完全集成尚不可用。 有关详细信息，请参阅[编写 C/C++ 单元测试](/visualstudio/test/writing-unit-tests-for-c-cpp)。

##### <a name="visual-studio-2017-version-156"></a>Visual Studio 2017 版本 15.6

- 现已开始支持 `Boost.Test` 动态库。
- IDE 中现提供 `Boost.Test` 项模板。

有关详细信息，请参阅 [`Boost.Test` 单元测试：动态库支持和新的项模板](https://devblogs.microsoft.com/cppblog/boost-test-unit-testing-dynamic-library-support-and-new-item-template/)。

##### <a name="visual-studio-2017-version-157"></a>Visual Studio 2017 15.7 版

针对 C++ 单元测试项目添加了对 [CodeLens](/visualstudio/ide/find-code-changes-and-other-history-with-codelens) 的支持。 有关详细信息，请参阅 [Announcing CodeLens for C++ Unit Testing](https://devblogs.microsoft.com/cppblog/announcing-codelens-for-c-unit-testing/)（宣布推出用于 C++ 单元测试的 CodeLens）。

## <a name="visual-studio-graphics-diagnostics"></a>Visual Studio 图形诊断

Visual Studio 图形诊断工具：可以使用它们来记录和分析 Direct3D 应用中的呈现和性能问题。 可以在 Windows 电脑、Windows 设备仿真器或远程电脑/设备上本地运行的应用程序上使用它们。

- **顶点和几何着色的输入和输出：** 查看顶点着色器和几何着色器的输入和输出一直以来是最渴求的功能之一。 工具中现已支持此功能。 在“管道阶段”视图中选择“VS”或“GS”阶段，即可在下面的表中开始检查其输入和输出。

  ![着色器的输入/输出](media/io-shaders.png)

- **在对象表中搜索和筛选：** 提供一种快捷简单的方法来查找所需资源。

  ![已调用“类型”下拉列表和“搜索”文本框的“对象表”部分的屏幕截图。](media/search.png)

- **资源历史记录：** 这种新视图提供了一种简化的方式，以在渲染捕获的帧期间使用资源时查看该资源的整个修改历史记录。 若要调用任何资源的历史记录，请单击任意资源超链接旁边的时钟图标。

  ![资源历史记录](media/resource-history.png)

  它显示新的“资源历史记录”工具窗口，其中填充了资源的更改历史记录。

  ![资源历史记录更改](media/resource-history-change.png)

  可以在启用完整调用堆栈捕获的情况下捕获帧。 这样，可以快速推导每个更改事件的上下文，并在 Visual Studio 项目中检查它。 在 Visual Studio 中“工具”>“选项”对话框的“图形诊断”下，设置完整堆栈捕获选项。

- **API 统计信息**：在帧中查看 API 使用情况的高级摘要。 这样可以轻松发现你可能根本没有意识到正在执行的调用或执行太频繁的调用。 可在 Visual Studio 图形分析器中通过“视图”>“API 统计信息”使用此窗口。

  ![API 统计信息](media/api-stats.png)

- **内存统计信息：** 查看驱动程序为你在帧中所创建的资源分配了多少内存。 可在“Visual Studio 图形分析器”中通过“视图”>“内存统计信息”使用此窗口 。 若要将数据复制到 CSV 文件以便在电子表格中查看，请右键单击，并选择“全部复制”。

  ![内存统计信息](media/memory-stats.png)

- **框架验证：** 新的错误和警告列表提供了一种简单的方式，以导航到基于潜在问题（由 Direct3D 调试层检测）的事件列表。 在 Visual Studio 图形分析器中单击“视图”>“帧验证”，可打开该窗口。 然后单击“运行验证”，开始分析。 这可能耗费数分钟时间，具体取决于帧的复杂性。

  ![帧验证](media/frame-validation.png)

- **对 D3D12 的帧分析：** 使用帧分析来分析具有定向“假设”试验的绘图调用性能。 切换至“帧分析”选项卡，然后运行分析以查看报告。 有关详细信息，请观看[GoingNative 25：Visual Studio 图形帧分析](https://channel9.msdn.com/Shows/C9-GoingNative/GoingNative-25-Offline-Analysis-Graphics-Tool)视频。

  ![帧分析](media/frame-analysis.png)

- **GPU 使用情况改进：** 使用 GPU 视图或 Windows Performance Analyzer (WPA) 工具，打开通过 Visual Studio GPU 使用情况探查器获取的跟踪，以便获取更详细的分析。 如果安装了 Windows Performance Toolkit，则在会话概述的右下方将有两个超链接：一个指向 WPA，另一个指向 GPU 视图。

  ![GPU 使用情况](media/gpu-usage.png)

  通过此链接在 GPU 视图中打开的跟踪支持同步的 VS 和 GPU 视图时间轴缩放和平移。 VS 中的复选框可控制是否启用同步。

  ![GPU 视图](media/gpu-view.png)
