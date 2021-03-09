---
description: 详细了解/INFERASANLIBS (使用推理的 sanitizer 库) 链接器选项
title: '/INFERASANLIBS (使用推理的 sanitizer 库) '
ms.date: 03/01/2021
f1_keywords:
- /INFERASANLIBS
- /INFERASANLIBS:NO
helpviewer_keywords:
- /INFERASANLIBS [C++]
- address sanitizer [C++] linker option
ms.openlocfilehash: 82337b5b77bab2a9066427662f3fd0956684c636
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470849"
---
# <a name="inferasanlibs-use-inferred-sanitizer-libs"></a>`/INFERASANLIBS` (使用推理的 sanitizer 库) 

使用 **`/INFERASANLIBS`** 链接器选项可以启用或禁用链接到默认的 AddressSanitizer 库。 在 Visual Studio 2019 16.9 中，唯一受支持的 sanitizer 为 [AddressSanitizer](../../sanitizers/asan.md)。

## <a name="syntax"></a>语法

> **`/INFERASANLIBS`**\[**`:NO`**]

## <a name="remarks"></a>备注

**`/INFERASANLIBS`** 链接器选项启用默认的 [AddressSanitizer](../../sanitizers/asan.md)库。 默认情况下会启用此选项。

**`/INFERASANLIBS`** 和 **`/INFERASANLIBS:NO`** 链接器选项提供对高级用户的支持。 有关详细信息，请参阅 [AddressSanitizer 生成和语言参考](../../sanitizers/asan-building.md)。

**`/INFERASANLIBS`** 从 Visual Studio 2019 版本16.9 开始，可以使用选项。

### <a name="to-set-the-inferasanlibs-linker-option-in-the-visual-studio-development-environment"></a>**`/INFERASANLIBS`** 在 Visual Studio 开发环境中设置链接器选项

1. 打开项目的 " **属性页** " 对话框。

1. 选择“配置属性” > “链接器” > “命令行”属性页    。

1. 修改 " **附加选项** " 属性。 若要启用默认库，请在编辑框中输入 **/INFERASANLIBS** 。 若要禁用默认库，请输入 **/INFERASANLIBS： NO** 。

1. 选择 **"确定" 或 "** **应用** " 保存更改。

### <a name="to-set-this-linker-option-programmatically"></a>以编程方式设置此链接器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCLinkerTool.AdditionalOptions%2A>。

## <a name="see-also"></a>另请参阅

[MSVC 链接器参考](linking.md)\
[MSVC 链接器选项](linker-options.md)\
[`/fsanitize` (启用 sanitizers) ](./fsanitize.md)\
[AddressSanitizer 概述](../../sanitizers/asan.md)\
[AddressSanitizer 的已知问题](../../sanitizers/asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](../../sanitizers/asan-building.md)
