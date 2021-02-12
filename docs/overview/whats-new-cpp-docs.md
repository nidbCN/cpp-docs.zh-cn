---
title: C++ 文档的新增内容
description: Microsoft C/ C++ 编译器、ATL/MFC、C 运行时和标准库文档的新增文档和文档更新。
ms.date: 02/08/2021
ms.openlocfilehash: 3b6d248b99415c91b4086b00ecc54fc39d2c348a
ms.sourcegitcommit: 77235bff6a7b2621c501938e30d93cb15f5733cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006045"
---
# <a name="microsoft-c-docs-whats-new-for-visual-studio-168"></a>Microsoft C++ 文档：Visual Studio 16.8 的新增功能

本文列出了对 Visual Studio 16.8 文档的一些重大更改。 

有关 Visual Studio 中新增功能的信息，请参阅 [Visual Studio 中的 C++ 新增功能](what-s-new-for-visual-cpp-in-visual-studio.md)。

有关最新 C++ 一致性状态，请参阅 [Visual Studio 中 C++ 的一致性改进](cpp-conformance-improvements.md)

## <a name="c-language"></a>C 语言

### <a name="new-articles"></a>新文章

- [`_Noreturn` 关键字和 `noreturn` 宏 (C11)](../c-language/noreturn.md)
- [_Static_assert 关键字和 static_assert 宏 (C11)](../c-language/static-assert-c.md)

### <a name="updated-articles"></a>更新的文章

- [分析 C 命令行参数](../c-language/parsing-c-command-line-arguments.md) - 记录 C 参数分析规则的例外
- [类型限定符](../c-language/type-qualifiers.md) - 已添加 `restrict`
- [C 赋值运算符](../c-language/c-assignment-operators.md) - C17 的词法语法更新
- [C 关键字](../c-language/c-keywords.md) - C17 的词法语法更新
- [C 词法语法](../c-language/lexical-grammar.md) - C17 的词法语法更新
- [声明摘要](../c-language/summary-of-declarations.md) - C17 的词法语法更新
- [表达式摘要](../c-language/summary-of-expressions.md) - C17 的词法语法更新
- [C 枚举声明](../c-language/c-enumeration-declarations.md) - 修复了词法语法
- [C 语句摘要](../c-language/summary-of-statements.md) - `__leave`、`__try` 关键字的更新

## <a name="c-runtime-library"></a>C 运行时库

### <a name="new-articles"></a>新文章

- [泛型类型数学](../c-runtime-library/tgmath.md)

### <a name="updated-articles"></a>更新的文章

- [`qsort`](../c-runtime-library/reference/qsort.md) - 添加了有关稳定性的注释
- [`_cwait`](../c-runtime-library/reference/cwait.md) - 修复了代码示例
- [函数系列概述](../c-runtime-library/function-family-overviews.md) - 添加了运算符 `new` 和 `delete`
- [`round, roundf, roundl`](../c-runtime-library/reference/round-roundf-roundl.md) - 阐明了舍入代码示例
- [兼容性](../c-runtime-library/compatibility.md) - 添加了 C99 一致性说明
- [`realloc`](../c-runtime-library/reference/realloc.md) - 添加了 C99 一致性说明
- [CRT 库功能](../c-runtime-library/crt-library-features.md) - 添加 C99 一致性说明
- [`assert Macro, _assert, _wassert`](../c-runtime-library/reference/assert-macro-assert-wassert.md) - 阐明了断言行为
- [`vsnprintf_s, _vsnprintf_s, _vsnprintf_s_l, _vsnwprintf_s, _vsnwprintf_s_l`](../c-runtime-library/reference/vsnprintf-s-vsnprintf-s-vsnprintf-s-l-vsnwprintf-s-vsnwprintf-s-l.md) - 阐明了返回值
- [`setlocale, _wsetlocale`](../c-runtime-library/reference/setlocale-wsetlocale.md) - 添加了 C 运行时 UTF-8 支持信息

## <a name="cc-preprocessor-reference"></a>C/C++ 预处理器参考

### <a name="updated-articles"></a>更新的文章

- [预定义的宏](../preprocessor/predefined-macros.md) - 更新了 16.8 发行说明、C11/C17 `/std` 支持和 SDK 安装文档
- [MSVC 新版预处理器概述](../preprocessor/preprocessor-experimental-overview.md) - 更新了预处理器内容

## <a name="code-quality"></a>代码质量

### <a name="new-articles"></a>新文章

- [C33001](../code-quality/c33001.md) - VC Code Analysis - 为 16.8 中的新规则添加
- [C33004](../code-quality/c33004.md) - VC Code Analysis - 为 16.8 中的新规则添加
- [C33005](../code-quality/c33005.md) - VC Code Analysis - 为 16.8 中的新规则添加
- [C33010](../code-quality/c33010.md) - VC Code Analysis - 为 16.8 中的新规则添加
- [C33011](../code-quality/c33011.md) - VC Code Analysis - 为 16.8 中的新规则添加
- [C33020](../code-quality/c33020.md) - VC Code Analysis - 为 16.8 中的新规则添加
- [C33022](../code-quality/c33022.md) - VC Code Analysis - 为 16.8 中的新规则添加

### <a name="updated-articles"></a>更新的文章

- [`C6262`](../code-quality/c6262.md) - 解决 cpp-docs.zh-tw 问题 20
- [`C26497 USE_CONSTEXPR_FOR_FUNCTION`](../code-quality/c26497.md) - 已向 C26497 添加示例
- [`C26496 USE_CONST_FOR_VARIABLE`](../code-quality/c26496.md) - 已向 C26496 添加示例
- [`C26495 MEMBER_UNINIT`](../code-quality/c26495.md) - 更新了 C26495 中的示例和链接
- [`C26483 STATIC_INDEX_OUT_OF_RANGE`](../code-quality/c26483.md) - 已向 C26483 添加示例
- [`C26462 USE_CONST_POINTER_FOR_VARIABLE`](../code-quality/c26462.md) - 已向 C26462 添加说明和示例
- [`C26461 USE_CONST_POINTER_ARGUMENTS:`](../code-quality/c26461.md) - 已向 C26461 添加说明和示例
- [`C26460 USE_CONST_REFERENCE_ARGUMENTS`](../code-quality/c26460.md) - 已向 C26460 添加说明和示例
- [`C26440 DECLARE_NOEXCEPT`](../code-quality/c26440.md) - 已向 C26440 添加示例和核心指南链接
- [`C26439 SPECIAL_NOEXCEPT`](../code-quality/c26439.md) - 已向 C26439 添加示例和核心指南链接
- [`C26436 NEED_VIRTUAL_DTOR`](../code-quality/c26436.md) - 已向 C26436 添加示例和核心指南链接
- [`C26408 NO_MALLOC_FREE`](../code-quality/c26408.md) - 已向 C26408 添加示例和核心指南链接
- [`C26401 DONT_DELETE_NON_OWNER`](../code-quality/c26401.md) - 已向 C26401 添加示例和核心指南链接
- [`C26494 VAR_USE_BEFORE_INIT`](../code-quality/c26494.md) - 已向 C26494 添加示例
- [`C26493 NO_CSTYLE_CAST`](../code-quality/c26493.md) - 已向 C26493 添加示例
- [`C26492 NO_CONST_CAST`](../code-quality/c26492.md) - 已向 C26492 添加示例
- [`C26490 NO_REINTERPRET_CAST`](../code-quality/c26490.md) - 已向 C26490 添加示例
- [`C26482 NO_DYNAMIC_ARRAY_INDEXING`](../code-quality/c26482.md) - 已向 C26482 添加示例
- [`C26471 NO_REINTERPRET_CAST_FROM_VOID_PTR`](../code-quality/c26471.md) - 已向 C26471 添加示例
- [`C26466 NO_STATIC_DOWNCAST_POLYMORPHIC`](../code-quality/c26466.md) - 已向 C26466 添加示例
- [`C26465 NO_CONST_CAST_UNNECESSARY`](../code-quality/c26465.md) - 已向 C26465 添加示例
- [`C26447 DONT_THROW_IN_NOEXCEPT`](../code-quality/c26447.md) - 向 C26447 添加示例
- [`C26446 USE_GSL_AT`](../code-quality/c26446.md) - 已向 C26446 添加示例
- [`C26434 DONT_HIDE_METHODS`](../code-quality/c26434.md) - 已向 C26434 添加示例
- [`C26432 DEFINE_OR_DELETE_SPECIAL_OPS`](../code-quality/c26432.md) - 已向 C26432 添加示例
- [`C26402 DONT_HEAP_ALLOCATE_MOVABLE_RESULT`](../code-quality/c26402.md) - 已向 C26402 添加示例
- [`C26409 NO_NEW_DELETE`](../code-quality/c26409.md) - 已向 C26409 添加示例
- [`C26474 NO_IMPLICIT_CAST`](../code-quality/c26474.md) - 更新了 C26474 以阐明基/派生事例
## <a name="linux-with-microsoft-c-in-visual-studio"></a>在 Visual Studio 中使用 Microsoft C/C++ 实现 Linux

### <a name="new-articles"></a>新文章

- [在 Visual Studio 中配置 Linux CMake 项目](../linux/cmake-linux-configure.md)

### <a name="updated-articles"></a>更新的文章

- [在 Visual Studio 中创建 Linux MSBuild C++ 项目](../linux/create-a-new-linux-project.md) - 更新了对创建 linux 项目的说明
- [ConnectionManager 引用](../linux/connectionmanager-reference.md) 添加了用于修改、清理的命令
- [在 Visual Studio 中配置 Linux CMake 项目](../linux/cmake-linux-configure.md) - 已更新以反映最新 UI
- [部署、运行和调试 Linux MSBuild 项目](../linux/deploy-run-and-debug-your-linux-project.md) - 已添加 `GDB Path`

## <a name="cc-compiler-and-tools-errors-and-warnings"></a>C/C++ 编译器和工具错误与警告

### <a name="new-articles"></a>新文章

- [编译器警告（等级 4）C4388](../error-messages/compiler-warnings/c4388.md) - 添加了警告 C4388，更新了 16.7 警告

### <a name="updated-articles"></a>更新的文章

- [编译器警告（等级 3）C4018](../error-messages/compiler-warnings/compiler-warning-level-3-c4018.md) - 更新了 16.7 警告
- [编译器警告（等级 4）C4389](../error-messages/compiler-warnings/compiler-warning-level-4-c4389.md) - 更新了 16.7 警告
- [各编译器版本的编译器警告](../error-messages/compiler-warnings/compiler-warnings-by-compiler-version.md) - 更新了 16.7 警告
- [编译器警告 C4800 到 C5999](../error-messages/compiler-warnings/compiler-warnings-c4800-through-c4999.md) - 更新了 16.7 警告
- [编译器错误 C3381](../error-messages/compiler-errors-2/compiler-error-c3381.md) - 解决 cpp-docs 2493；更新注解和示例

## <a name="cc-compiler-intrinsics-and-assembly-language"></a>C/C++ 编译器内部函数和程序集语言

### <a name="updated-articles"></a>更新的文章

- [Visual Studio 中的 C++ 一致性改进](../overview/cpp-conformance-improvements.md) - 更新了 16.8 发行说明
- [Microsoft C/C++ 帮助和社区](../overview/visual-cpp-help-and-community.md) - 更新了 DevCom 和 Microsoft Docs 问答链接
- [Visual Studio 中的 C++](../overview/visual-cpp-in-visual-studio.md) - 更新了 DevCom 和 Microsoft Docs 问答链接
- [Microsoft C++ 语言一致性表](../overview/visual-cpp-language-conformance.md) - 更新了 C++ 20 库一致性表，更新 16.7 的语言功能表

## <a name="c-in-visual-studio"></a>Visual Studio 中的 C++

### <a name="updated-articles"></a>更新的文章

- [`__restrict`](../cpp/extension-restrict.md)
- [if-else 语句 (C++)](../cpp/if-else-statement-cpp.md) - 添加了对 `if/else` 语法的描述
- [`union`](../cpp/unions.md) - 修复了代码段

## <a name="cc-projects-and-build-systems"></a>C/C++ 项目和生成系统

### <a name="new-articles"></a>新文章

- [`.vcxproj` 文件和通配符](../build/reference/vcxproj-files-and-wildcards.md)
- [`/headerUnit` （使用标头单元 IFC）](../build/reference/headerunit.md)
- [`/module:exportHeader`（创建标头单元）](../build/reference/module-exportheader.md)
- [`/module:reference`（使用命名模块 IFC）](../build/reference/module-reference.md)
- [`/translateInclude`（将 include 指令转换为 import 指令）](../build/reference/translateinclude.md)
- [`/Zc:preprocessor`（启用预处理器一致性模式）](../build/reference/zc-preprocessor.md)

### <a name="updated-articles"></a>更新的文章

- [`/permissive-`（标准一致性）](../build/reference/permissive-standards-conformance.md) - 更新了 16.8 发行说明
- [`/clr`（公共语言运行时编译）](../build/reference/clr-common-language-runtime-compilation.md) - 添加了对 `/clr` 的描述
- [pgosweep](../build/pgosweep.md) - 添加了更多 pgosweep 选项
- [使用 `__declspec(dllimport)` 导入数据](../build/importing-data-using-declspec-dllimport.md) - 更新了示例

## <a name="c-porting-and-upgrade-guide"></a>C++ 移植和升级指南

### <a name="updated-articles"></a>更新的文章

- [如何：在通用 Windows 平台应用中使用现有 C++ 代码](../porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app.md) - 已重制以阐明并更新示例

## <a name="c-standard-library-stl-reference"></a>C++ 标准库 (STL) 参考

### <a name="new-articles"></a>新文章

- [`<bit>`](../standard-library/bit.md)
- [`<bit>` 函数](../standard-library/bit-functions.md)
- [`endian` enum](../standard-library/bit-enum.md)

### <a name="updated-articles"></a>更新的文章

- [`<ios>` typedef](../standard-library/ios-typedefs.md) - 更新了每个 GitHub 的示例 #2514
- [`basic_string` 类](../standard-library/basic-string-class.md) - 添加了 `startswith()`、`endswith()`
- [`ios_base Class`](../standard-library/ios-base-class.md)
- [`map` 类](../standard-library/map-class.md)
- [`multimap` 类](../standard-library/multimap-class.md) - 添加了 `contains()`
- [`multiset` 类](../standard-library/multiset-class.md) - 添加了 `contains()`
- [`set` 类](../standard-library/set-class.md) - 添加了 `contains()`
- [`unordered_map` 类](../standard-library/unordered-map-class.md) - 添加了 `contains()`
- [`unordered_multimap` 类](../standard-library/unordered-multimap-class.md) - 添加了 `contains()`
- [`unordered_multiset` 类](../standard-library/unordered-multiset-class.md) - 添加了 `contains()`
- [`unordered_set` 类](../standard-library/unordered-set-class.md) - 添加了 `contains()`
- [`basic_string_view` 类](../standard-library/basic-string-view-class.md) - 添加了 `startswith()`、`endswith()`
- [`<bit>` 函数](../standard-library/bit-functions.md) - 更新了 `nodiscard` 语法

## <a name="community-contributors"></a>社区参与者

在此期间，以下人员参与编著了 C++、C 和汇编程序文档。 谢谢！ 如果想要了解如何参与，请参阅 [Microsoft Docs 参与者指南概述](https://docs.microsoft.com/contribute/)。

- [yecril71pl](https://github.com/yecril71pl) - Christopher Yeleighton (4)
- [definedrisk](https://github.com/definedrisk) - Ben (3)
- [BeardedFish](https://github.com/BeardedFish) - Darian B.(1)
- [codevenkat](https://github.com/codevenkat) (1)
- [eltociear](https://github.com/eltociear) - Ikko Ashimine (1)
- [fsb4000](https://github.com/fsb4000) - Igor Zhukov (1)
- [Jaiganeshkumaran](https://github.com/Jaiganeshkumaran) - Jaiganesh Kumaran (1)
- [jogo-](https://github.com/jogo-) (1)
- [justanotheranonymoususer](https://github.com/justanotheranonymoususer) (1)
- [matrohin](https://github.com/matrohin) - Dmitry Matrokhin (1)
- [mhemmit](https://github.com/mhemmit) (1)
- [MSDN-WhiteKnight](https://github.com/MSDN-WhiteKnight) - MSDN.WhiteKnight (1)
- [OdinTemple](https://github.com/OdinTemple) - Odin Temple (1)
- [r00tdr4g0n](https://github.com/r00tdr4g0n) - r00tdr4g0n (1)
- [sebkraemer](https://github.com/sebkraemer) - Sebastian Krämer (1)
- [vtjnash](https://github.com/vtjnash) - Jameson Nash (1)
- [Youssef1313](https://github.com/Youssef1313) - Youssef Victor (1)
- [zecozephyr](https://github.com/zecozephyr) - Jonathan Bailey (1)