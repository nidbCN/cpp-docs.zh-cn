---
description: '了解详细信息：/VERBOSE (打印进度消息) '
title: /VERBOSE（打印进度消息）
ms.date: 02/03/2021
f1_keywords:
- /verbose
- VC.Project.VCLinkerTool.ShowProgress
helpviewer_keywords:
- -VERBOSE linker option
- linking [C++], session progress information
- Print Progress Messages linker option
- linker [C++], output dependency information
- /VERBOSE linker option
- dependencies [C++], dependency information in linker output
- VERBOSE linker option
ms.openlocfilehash: d58a6cc8d75021c78f8161cf12957a77bb26483c
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522919"
---
# <a name="verbose-print-progress-messages"></a>`/VERBOSE` (打印进度消息) 

在链接过程中输出进度消息。

## <a name="syntax"></a>语法

> **`/VERBOSE`**\[**`:`**{**`CLR`**|**`ICF`**|**`INCR`**|**`LIB`**|**`REF`**|**`SAFESEH`**|**`UNUSEDDELAYLOAD`**|**`UNUSEDLIBS`**}\]

## <a name="remarks"></a>备注

链接器将有关链接会话进度的信息发送到 " **输出** " 窗口。 在命令行中，信息将发送到标准输出，并可重定向到文件。

| 选项 | 说明 |
| ------------ | ----------------- |
| **`/VERBOSE`** | 显示有关链接进程的详细信息。 |
| **`/VERBOSE:CLR`** | 显示有关特定于使用编译的对象和元数据的链接器活动的信息 [`/clr`](clr-common-language-runtime-compilation.md) 。 |
| **`/VERBOSE:ICF`** | 显示有关使用导致的链接器活动的信息 [`/OPT:ICF`](opt-optimizations.md) 。 |
| **`/VERBOSE:INCR`** | 有关增量链接过程的信息。 |
| **`/VERBOSE:LIB`** | 显示进度消息，该消息仅指示所搜索的库。<br/> 显示的信息包括库搜索过程。 它列出了每个库和对象名称 (，其中包含完整路径) 、要从库解析的符号以及引用该符号的对象的列表。 |
| **`/VERBOSE:REF`** | 显示有关使用导致的链接器活动的信息 [`/OPT:REF`](opt-optimizations.md) 。 |
| **`/VERBOSE:SAFESEH`** | 当未指定时，显示与安全结构化异常处理不兼容的模块的信息 [`/SAFESEH`](safeseh-image-has-safe-exception-handlers.md) 。 |
| **`/VERBOSE:UNUSEDDELAYLOAD`** | 显示有关在创建映像时未使用任何符号的任何延迟加载的 Dll 的信息。 |
| **`/VERBOSE:UNUSEDLIBS`** | 显示有关在创建映像时未使用的任何库文件的信息。 |

### <a name="to-set-this-linker-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此链接器选项

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 选择“配置属性” > “链接器” > “命令行”属性页    。

1. 将选项添加到 " **附加选项** " 框。

### <a name="to-set-this-linker-option-programmatically"></a>以编程方式设置此链接器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCLinkerTool.ShowProgress%2A>。

## <a name="see-also"></a>另请参阅

[MSVC 链接器参考](linking.md)<br/>
[MSVC 链接器选项](linker-options.md)
