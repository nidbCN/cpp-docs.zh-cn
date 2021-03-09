---
description: 了解更多：编译器和链接器中的 Unicode 支持
title: 编译器和链接器中的 Unicode 支持
ms.date: 03/07/2021
f1_keywords:
- VC.Project.VCLinkerTool.UseUnicodeResponseFiles
- VC.Project.VCLibrarianTool.UseUnicodeResponseFiles
- VC.Project.VCCLCompilerTool.UseUnicodeResponseFiles
- VC.Project.VCXDCMakeTool.UseUnicodeResponseFiles
helpviewer_keywords:
- Unicode, Visual C++
ms.openlocfilehash: e1795a5a9b9d4a3a1672b2661aa598d0ef6e059f
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465335"
---
# <a name="unicode-support-in-the-compiler-and-linker"></a>编译器和链接器中的 Unicode 支持

大多数 Microsoft C/c + + (MSVC) 生成工具支持 Unicode 输入和输出。

## <a name="filenames"></a>文件名

命令行或编译器指令中指定的文件名 (如 `#include`) 可能包含 Unicode 字符。

## <a name="source-code-files"></a>源代码文件

标识符、宏、字符串和字符文本以及注释中支持 Unicode 字符。  还支持通用字符名称。

Unicode 可以在源代码文件中输入以下编码：

- 具有或不具有字节顺序标记 (BOM 的 UTF-16 little endian) 

- 带有或不带 BOM 的 UTF-16 大 endian

- UTF-8 with BOM

在 Visual Studio IDE 中，可以采用多种编码格式保存文件，包括 Unicode 文件。 使用 "**保存**" 按钮上的下拉列表，将其保存在 "将 **文件另存为**" 对话框中。 选择下拉列表中的 " **用编码保存** "。 然后，在 " **高级保存选项** " 对话框中，从下拉列表中选择一种编码。 选择 **"确定"** 以保存文件。

## <a name="output"></a>输出

在编译期间，编译器会将诊断输出到 UTF-16 中的控制台。  可以在控制台中显示的字符取决于控制台窗口属性。  重定向到文件的编译器输出在当前 ANSI 控制台代码页中。

## <a name="linker-response-files-and-def-files"></a>链接器响应文件和 `.DEF` 文件

响应文件和 *`.DEF`* 文件可以是 utf-16 或带 BOM 的 utf-8，或者 ANSI。

## <a name="asm-and-cod-dumps"></a>`.asm` 和 `.cod` 转储

*`.asm`**`.cod`* 默认情况下，和转储在 ANSI 中是为了与 MASM 兼容。 用于 [`/FAu`](fa-fa-listing-file.md) 输出 utf-8。

如果指定 **`/FAs`** ，将直接打印混合的源。 例如，如果源代码为 UTF-8，而你未指定，则可能会出现乱码 **`/FAsu`** 。

## <a name="see-also"></a>请参阅

[通过命令行使用 MSVC 工具集](../building-on-the-command-line.md)
