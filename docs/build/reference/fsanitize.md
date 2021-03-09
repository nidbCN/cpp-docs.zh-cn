---
description: 了解有关/fsanitize (enable sanitizers) 编译器选项的详细信息
title: '/fsanitize (启用 sanitizers) '
ms.date: 02/24/2021
f1_keywords:
- /fsanitize
- -fsanitize
- /fsanitize=address
- /fsanitize-address-use-after-return
- -fsanitize-address-use-after-return
- /fno-sanitize-address-vcasan-lib
- -fno-sanitize-address-vcasan-lib
helpviewer_keywords:
- /fsanitize [C++]
- -fsanitize=address [C++]
- address sanitizer compiler option [C++]
- /fsanitize-address-use-after-return
- /fno-sanitize-address-vcasan-lib
ms.openlocfilehash: 820d39e54db29a5e06d119cbefc8a1980447e8cc
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470847"
---
# <a name="fsanitize-enable-sanitizers"></a>`/fsanitize` (启用 sanitizers) 

使用 **`/fsanitize`** 编译器选项来启用 sanitizers。 在 Visual Studio 2019 16.9 中，唯一受支持的 sanitizer 为 [AddressSanitizer](../../sanitizers/asan.md)。

## <a name="syntax"></a>语法

> **`/fsanitize=address`**\
> **`/fsanitize-address-use-after-return`**\
> **`/fno-sanitize-address-vcasan-lib`**

## <a name="remarks"></a>备注

**`/fsanitize=address`** 编译器选项支持 [AddressSanitizer](../../sanitizers/asan.md)，这是一项功能强大的编译器和运行时技术，可使您 [难以发现 bug](../../sanitizers/asan.md#error-types)。

**`/fsanitize-address-use-after-return`** 和 **`/fno-sanitize-address-vcasan-lib`** 编译器选项以及 [ `/INFERASANLIBS` (使用推理的 sanitizer 库)](./inferasanlibs.md)和 **`/INFERASANLIBS:NO`** 链接器选项为高级用户提供支持。 有关详细信息，请参阅 [AddressSanitizer 生成和语言参考](../../sanitizers/asan-building.md)。

**`/fsanitize`** 从 Visual Studio 2019 16.9 版开始，将提供这些选项。

### <a name="to-set-the-fsanitizeaddress-compiler-option-in-the-visual-studio-development-environment"></a>**`/fsanitize=address`** 在 Visual Studio 开发环境中设置编译器选项

1. 打开项目的 " **属性页** " 对话框。

1. 选择“配置属性” > “C/C++” > “常规”属性页    。

1. 修改 **Enable Address Sanitizer** 属性。 若要启用它，请选择 **"是 (/fsanitize = address)**。

1. 选择 **"确定" 或 "** **应用** " 保存更改。

### <a name="to-set-the-advanced-compiler-options"></a>设置高级编译器选项

1. 打开项目的 " **属性页** " 对话框。

1. 选择 "**配置属性**" "  >  **c/c + +**  >  **命令行**" 属性页。

1. 修改 " **附加选项** " 属性以设置 **/fsanitize-address-use-after-return** 或 **/fno-sanitize-address-vcasan-lib**。

1. 选择 **"确定" 或 "** **应用** " 保存更改。

### <a name="to-set-this-compiler-option-programmatically"></a>以编程方式设置此编译器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCCLCompilerTool.AdditionalOptions%2A>。

## <a name="see-also"></a>另请参阅

[MSVC 编译器选项](compiler-options.md)\
[MSVC 编译器命令行语法](compiler-command-line-syntax.md)\
[`/INFERASANLIBS` (使用推理的 sanitizer 库) ](./inferasanlibs.md)\
[AddressSanitizer 概述](../../sanitizers/asan.md)\
[AddressSanitizer 的已知问题](../../sanitizers/asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](../../sanitizers/asan-building.md)
