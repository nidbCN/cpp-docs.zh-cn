---
description: 了解详细信息： "高级" 属性页
title: "\"高级\" 属性页 (项目) "
ms.date: 02/05/2021
f1_keywords:
- VC.Project.VCConfiguration.VCToolsVersion
- VC.Project.VCConfiguration.LLVMToolsVersion
ms.description: Use the Advanced property page in Visual Studio 2019 to set various properties for C++ projects.
ms.openlocfilehash: 420dd34088f47e8fa7bfe56fd529769b90401355
ms.sourcegitcommit: 77235bff6a7b2621c501938e30d93cb15f5733cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006024"
---
# <a name="advanced-property-page"></a>"高级" 属性页

::: moniker range="<=msvc-150"

Visual Studio 2019 和更高版本中提供了 "高级" 属性页。 若要查看此版本对应的文档，请将本文的 Visual Studio“版本”选择器控件设置为“Visual Studio 2019”。 它位于此页面上目录表的顶部。

::: moniker-end

::: moniker range="msvc-160"

Visual Studio 2019 和更高版本中提供了 "高级" 属性页。

## <a name="advanced-properties"></a>高级属性

- **目标文件扩展名**

   指定要用于生成输出的文件扩展名。 对于应用程序，默认值为; 对于 *`.exe`* *`.lib`* 静态库和 dll，默认值为 *`.dll`* 。

- **清除时要删除的扩展名**

   “清理”选项（“生成”菜单）从生成项目的配置的中间目录中删除文件。 此属性中指定的扩展名的文件将在运行 " **清理** " 或重新生成时被删除。 生成系统删除中间目录中具有这些扩展名的任何文件。 它还会删除任何已知的生成输出，无论其位于何处。  (包含中间输出，如 *`.obj`* 文件。 ) 可以在此属性中指定通配符。

   若要以编程方式访问此属性，请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCConfiguration.DeleteExtensionsOnClean%2A>。

- **生成日志文件**

   允许您指定在每次生成项目时创建的日志文件的非默认位置。 默认位置由宏指定 `$(IntDir)$(MSBuildProjectName).log` 。

   可使用项目宏来更改目录位置。 请参阅 [用于生成命令和属性的常见宏](common-macros-for-build-commands-and-properties.md)。

- **首选生成工具体系结构**

   指定是否使用 x86 或 x64 生成工具。

- **使用调试库**

   指定是创建调试版本还是发布版本。

- **启用 Unity (超长) 生成**

   实现更快的生成过程，该过程在编译之前将许多 c + + 源文件合并到一个或多个文件中。 这些合并文件称为 *unity* 文件。 它们与 Unity 游戏引擎无关。

- **将内容复制到 OutDir**

   将标记为 *内容* 的项中的项复制到项目的输出目录中， (`$(OutDir)`) 。 此设置可简化部署。 此属性可从 Visual Studio 2019 版本16.7 开始使用。

- **将项目引用复制到 OutDir**

    () ，将可执行文件 (DLL 和 EXE 文件) 项目引用项复制到项目的输出目录中 `$(OutDir)` 。 在 c + +/CLI ([`/clr`](clr-common-language-runtime-compilation.md)) 项目中，将忽略此属性。 相反，每个项目上的 " **复制本地** " 属性都控制是否将其复制到输出目录。 此设置可简化本地部署。 可从 Visual Studio 2019 版本16.7 开始使用。

- **将项目引用符号复制到 OutDir**

   将项目引用项的 PDB 文件以及项目引用可执行项复制到项目的输出目录中 (`$(OutDir)`) 。 对于 c + +/CLI 项目，始终启用此属性。 此设置可简化调试部署。 可从 Visual Studio 2019 版本16.7 开始使用。

- **将 c + + 运行时复制到 OutDir**

   将运行时 Dll 复制到项目的输出目录中 (`$(OutDir)`) 。 此设置可简化本地部署。 可从 Visual Studio 2019 版本16.7 开始使用。

- **MFC 的使用**

   指定 MFC 项目是否静态或动态链接到 MFC DLL。 在非 MFC 项目中，选择 " **使用标准 Windows 库** " 链接 MFC 包括的 Win32 库。

   若要以编程方式访问此属性，请参阅 <xref:Microsoft.VisualStudio.VCProject.VCProjectConfigurationProperties.useOfMfc%2A>。

- **字符集**

   定义是否 `_UNICODE` `_MBCS` 设置或。 在适当的情况下，还会影响链接器入口点。

   若要以编程方式访问此属性，请参阅 <xref:Microsoft.VisualStudio.VCProject.VCProjectConfigurationProperties.CharacterSet%2A>。

- **全程序优化**

   指定 [`/GL`](gl-whole-program-optimization.md) 编译器选项和 [`/LTCG`](ltcg-link-time-code-generation.md) 链接器选项。 默认情况下，为调试配置禁用了全程序优化，并为发布配置启用了。

- **MSVC 工具集版本**

   指定用于生成项目的 MSVC 工具集的完整版本。 可能已安装了不同的工具集更新和预览版本。 可以在此处指定要使用的项。

- **LLVM 工具集版本**

   指定用于生成项目的 LLVM 工具集的完整版本。 在 Visual Studio 2019 版本16.9 中选择 **LLVM (clang-cl)** 作为平台工具集时，此属性可用。 有关详细信息，请参阅 [设置自定义 LLVM 工具集版本](..\clang-support-msbuild.md#custom_llvm_toolset)。

## <a name="ccli-properties"></a>C + +/CLI 属性

- **公共语言运行时支持**

   导致 [`/clr`](clr-common-language-runtime-compilation.md) 使用编译器选项。

   若要以编程方式访问此属性，请参阅 <xref:Microsoft.VisualStudio.VCProject.VCProjectConfigurationProperties.ManagedExtensions%2A>。

- **.NET 目标框架版本**

   在托管项目中指定要面向的 .NET Framework 版本。

- **启用托管增量生成**

   对于托管项目，此选项允许在生成程序集时检测外部可见性。 如果对托管项目的更改对其他项目不可见，则不会重新生成依赖项目。 托管增量生成可以显著提高包含托管项目的解决方案中的生成时间。

::: moniker-end
