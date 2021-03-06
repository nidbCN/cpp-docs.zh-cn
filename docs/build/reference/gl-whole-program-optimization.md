---
description: '了解有关以下内容的详细信息：/GL (完全程序优化) '
title: /GL（全程序优化）
ms.date: 03/05/2021
f1_keywords:
- /GL
helpviewer_keywords:
- /GL compiler option [C++]
- whole program optimizations and C++ compiler
- -GL compiler option [C++]
- GL compiler option [C++]
ms.openlocfilehash: 509deaae8881c4875a9ec2ddf4d5f1ee7744a2cf
ms.sourcegitcommit: c0c9cdae79f19655e809a4979227c51bb19cff63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102236545"
---
# <a name="gl-whole-program-optimization"></a>`/GL` (完全程序优化) 

启用全程序优化。

## <a name="syntax"></a>语法

> **`/GL`**[**`-`**]

## <a name="remarks"></a>备注

全程序优化允许编译器对程序中的所有模块的信息进行优化。 如果没有完整的程序优化，将基于每个模块 (编译单位) 来执行优化。

全程序优化默认情况下处于关闭状态，必须显式启用。 但是，也可以通过显式禁用它 **`/GL-`** 。

对于所有模块的信息，编译器可以：

- 优化跨函数边界使用寄存器。

- 更好地跟踪全局数据的修改，从而减少加载和存储的数量。

- 跟踪由指针取消引用修改的可能项集，从而减少所需的加载和存储。

- 即使在另一个模块中定义函数时，也会在模块中内联该函数。

*`.obj`* 使用生成的文件 **`/GL`** 无法通过链接器实用工具（如和）使用 [`EDITBIN`](editbin-reference.md) [`DUMPBIN`](dumpbin-reference.md) 。

如果使用和编译程序 **`/GL`** [`/c`](c-compile-without-linking.md) ，则应使用/ltcg 链接器选项来创建输出文件。

[`/ZI`](z7-zi-zi-debug-information-format.md) 不能与一起使用 **`/GL`**

在当前版本中生成的文件的格式 **`/GL`** 通常不能由较新版本的 Visual Studio 和 MSVC 工具集读取。 除非你愿意为 *`.lib`* Visual Studio 的所有版本提供文件的副本，但你希望用户在将来使用，而不是提供 *`.lib`* *`.obj`* 由生成的文件生成的文件 **`/GL`** 。 有关详细信息，请参阅 [二进制兼容性的限制](../../porting/binary-compat-2015-2017.md#restrictions)。

*`.obj`***`/GL`** 不应使用生成的文件和预编译头文件来生成 *`.lib`* 文件，除非该 *`.lib`* 文件在生成该文件的同一计算机上链接 **`/GL`** *`.obj`* 。 在 *`.obj`* 链接时，需要来自文件的预编译头文件的信息。

有关中提供的优化和全程序优化的限制的详细信息，请参阅 [`/LTCG`](ltcg-link-time-code-generation.md) 。  **`/GL`** 还可提供按配置优化。 在针对按配置优化进行编译时，如果希望按配置优化执行函数排序，则必须使用 [`/Gy`](gy-enable-function-level-linking.md) 或表示/Gy. 的编译器选项进行编译

### <a name="to-set-this-linker-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此链接器选项

有关如何 **`/GL`** 在开发环境中指定的详细信息，请参阅 [ `/LTCG` () 的链接时间代码生成](ltcg-link-time-code-generation.md)。

### <a name="to-set-this-linker-option-programmatically"></a>以编程方式设置此链接器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCCLCompilerTool.WholeProgramOptimization%2A>。

## <a name="see-also"></a>另请参阅

[MSVC 编译器选项](compiler-options.md)\
[MSVC 编译器命令行语法](compiler-command-line-syntax.md)
