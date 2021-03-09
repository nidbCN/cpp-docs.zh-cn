---
title: AddressSanitizer 云或分布式测试
description: 适用于 Visual Studio 和 Azure 的 AddressSanitizer 功能扩展
ms.date: 03/02/2021
helpviewer_keywords:
- AddressSanitizer cloud or distributed testing
ms.openlocfilehash: 53bbb1ca04a5c4636914591cf10bfcc6bf275d42
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470961"
---
# <a name="addresssanitizer-cloud-or-distributed-testing"></a>AddressSanitizer 云或分布式测试

不需要在发生 AddressSanitizer 错误时进行调试。 将运行时配置为创建在发生错误时存储所有 AddressSanitizer 特定上下文的故障转储。 然后将该故障转储发送到另一台计算机进行调试。 在云中或在分布式测试中运行 AddressSanitizer 时，脱机调试可能是一项重要的节省时间。 你可以在发生故障的测试或生产基础结构上创建转储，稍后在开发人员电脑上对其进行调试。

Visual Studio 调试器提供准确诊断的 AddressSanitizer 错误。 您可以查看这些 bug，而无需重新运行测试、复制大型数据集、发现丢失的数据或查找脱机的测试计算机。 只需加载故障转储。

故障转储是在 AddressSanitizer 故障时通过设置以下环境变量来创建的：

`set ASAN_SAVE_DUMPS=MyFileName.dmp`

> [!NOTE]
> 文件名必须具有后缀 *`.dmp`* 才能遵循 Visual Studio 命名约定。

在另一台计算机上，可以使用 Visual Studio 在以后显示此 [转储文件](/previous-versions/windows/desktop/proc_snap/export-a-process-snapshot-to-a-file) 。

Visual Studio 可以在原始源代码的上下文中显示错误消息。 为此，Visual Studio 需要 [调试符号](/windows/win32/dxtecharts/debugging-with-symbols) 和 [索引源代码](/windows-hardware/drivers/debugger/source-indexing)。 为了获得最佳的调试体验，用于生成这些二进制文件的 EXE、PDB 和源代码必须匹配。

有关存储源和符号的详细信息，请参阅 [源和符号](#source) 部分。 有关实现细节和精细控制的信息，请参阅 [调试器集成](asan-debugger-integration.md)。

## <a name="example---build-test-and-analyze"></a>示例-生成、测试和分析

请考虑三台计算机： A、B 和 C。生成是在计算机 B 上完成的，测试在计算机 C 上运行，并分析计算机 A 上的故障。将根据源代码中的源行号和列号报告这些错误。 可以在使用该 [确切版本的源代码](#source)生成的 PDB 文件中使用一组符号来查看调用堆栈。

以下步骤适用于导致创建 dmp 文件的本地或分布式方案，并用于查看 AddressSanitizer dump 文件脱机。

### <a name="produce-a-dmp-locally"></a>在本地生成 dmp

- 生成
- 测试可执行文件
- 将生成的 dmp 文件复制到生成目录
- 用相同目录中的成对 .pdb 打开 dmp 文件

### <a name="produce-a-dmp-on-a-distributed-system"></a>在分布式系统上生成 dmp

- 为[源索引数据块](/windows/win32/debug/source-server-and-source-indexing)生成并[后处理 PDB](#source)
- 将 () 的原子对复制到测试计算机并运行测试
- 将 ( .pdb、dmp) 的原子对写入 bug 报告数据库
- Visual Studio 将在同一目录中打开具有配对 .pdb 的 dmp 文件

> [!NOTE]
> 用于分析的 Visual Studio 2019 计算机必须具有访问 GitHub 或存储索引源的内部的权限 *`\\Machine\share`* 。

## <a name="view-addresssanitizer-dmp-files"></a>查看 AddressSanitizer 文件

1. 请确保调试器 IDE 可以找到 PDB 和源文件。

1. 打开 Visual Studio 并选择 " **继续而不显示代码**"。 然后选择 "**文件**" "  >  **打开**  >  **文件**" 打开 "文件打开" 对话框。 请确保文件名后缀为 **dmp**。

   :::image type="content" source="./media/asan-open-crash-dump-file.png" alt-text="Visual Studio 中 &quot;文件打开文件&quot; 菜单的屏幕截图。":::

   此处所示的屏幕需要再执行一个步骤，以启用 IDE 对符号和源的访问。

1. 设置符号路径，然后选择 " **仅限本机调试**"。

   :::image type="content" source="./media/asan-dump-file-open.png" alt-text="Visual Studio 中的小型转储摘要显示屏幕截图。":::

此屏幕截图显示最后加载的转储文件，其中加载了源和 AddressSanitizer 元数据。

:::image type="content" source="./media/asan-view-crash-metadata.png" alt-text="显示源文件和 AddressSanitizer 元数据的调试器屏幕截图。":::

## <a name="source-and-symbols"></a><a name="source"></a> 源和符号

源服务器允许客户端检索用于生成应用程序的源文件的 **确切版本** 。 可执行文件或 DLL 的源代码可能会随时间而变化，并且在版本之间发生变化。 您可以使用它来查看生成特定应用程序版本的相同源代码。

当使用其 PDB 文件调试 EXE 时，调试器可以使用嵌入的源服务器数据块从源控件中检索相应的文件。 它将映射到由编译器选项自动置于 PDB 中的完全限定名称的文件 **`/Zi`** 。

若要使用源服务器，应用程序必须通过使用将 *`pdbstr.exe`* 数据块写入 PDB 文件中来 "源索引" `srcsrv` 。 有关详细信息，请参阅源服务器的数据块部分 [和源索引](/windows/win32/debug/source-server-and-source-indexing)。 你将找到 [对源进行索引和发布符号的步骤](/azure/devops/pipelines/tasks/build/index-sources-publish-symbols) ，以及 [如何为调试器指定符号和源代码](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger) 。

有关外部文档，请参阅：

- [带有 Git 的源索引](https://gist.github.com/baldurk/c6feb31b0305125c6d1a)
- [更易于调试的指南](https://www.codeproject.com/articles/115125/source-indexing-and-symbol-servers-a-guide-to-easi)
- [源索引未充分利用卓越](https://randomascii.wordpress.com/2011/11/11/source-indexing-is-underused-awesomeness/)

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
