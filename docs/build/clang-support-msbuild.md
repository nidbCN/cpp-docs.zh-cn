---
description: 详细了解：Visual Studio 项目中的 Clang/LLVM 支持
title: Visual Studio 项目中的 Clang/LLVM 支持
ms.date: 02/05/2021
ms.description: Configure a Visual Studio MSBuild project to use the Clang/LLVM toolchain.
helpviewer_keywords:
- Clang support for C++ MSBuild projects
ms.openlocfilehash: 7492d2c75e458fb4a8a94e009f40b49edf1a8204
ms.sourcegitcommit: 77235bff6a7b2621c501938e30d93cb15f5733cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006055"
---
# <a name="clangllvm-support-in-visual-studio-projects"></a>Visual Studio 项目中的 Clang/LLVM 支持

::: moniker range="<=msvc-150"

Visual Studio 2019 中提供对 CMake 和 MSBuild 项目的 Clang 支持。

::: moniker-end

::: moniker range="msvc-160"

可以将 Visual Studio 2019 版本 16.2 与 Clang 配合使用来编辑、生成和调试面向 Windows 或 Linux 的 C++ Visual Studio 项目 (MSBuild)。

## <a name="install"></a>安装

若要在 Visual Studio 中获得最佳 IDE 支持，建议使用适用于 Windows 的最新 Clang 编译器工具。 如果尚未安装这些工具，在可以通过打开 Visual Studio 安装程序并在“使用 C++ 的桌面开发”可选组件下选择“适用于 Windows 的 C++ Clang 工具”来安装它们 。 你可能希望使用计算机上的现有 Clang 安装；如果是这样，请选择“适用于 v142 生成工具的 C++ Clang-cl”。 可选组件。

Microsoft C++ 标准库当前至少需要 Clang 8.0.0。 Clang 的捆绑版本会自动更新，以随时了解标准库的 Microsoft 实现中的更新。

![已选中“单个组件”选项卡并使 C++ Clang 组件可见的 Visual Studio 安装程序的屏幕截图。](media/clang-install-vs2019.png)

## <a name="configure-a-windows-project-to-use-clang-tools"></a>配置 Windows 项目以使用 Clang 工具

若要配置 Visual Studio 项目以使用 Clang，请右键单击“解决方案资源管理器”中的项目节点，然后选择“属性” 。 通常，应首先选择对话框顶部的“所有配置”。 然后，在“常规” > “平台工具集”下，选择“LLVM (clang-cl)”，然后选择“确定”。

![已选中“配置属性”>“常规”且已突出显示“平台工具集和 L L V M (clang c l)”选项的“属性页”对话框的屏幕截图。](media/clang-msbuild-prop-page.png)

如果使用与 Visual Studio 捆绑的 Clang 工具，则无需执行其他步骤。 对于 Windows 项目，Visual Studio 默认在 [clang-cl](https://llvm.org/devmtg/2014-04/PDFs/Talks/clang-cl.pdf) 模式下调用 Clang。 它链接到标准库的 Microsoft 实现。 默认情况下，clang-cl.exe 位于 %VCINSTALLDIR%\\Tools\\Llvm\\bin\\ 和 %VCINSTALLDIR%\\Tools\\Llvm\\x64\\bin\\。

如果使用自定义 Clang 安装，则可以修改“项目” > “属性” > “VC++ 目录” > “配置属性” > “可执行文件目录”，在其中将自定义 Clang 安装根添加为第一个目录，或是更改 `LLVMInstallDir` 属性的值。 有关详细信息，请参阅[设置自定义 LLVM 位置](#custom_llvm_location)。

## <a name="configure-a-linux-project-to-use-clang-tools"></a>配置 Linux 项目以使用 Clang 工具

对于 Linux 项目，Visual Studio 使用 Clang GCC 兼容前端。 项目属性和几乎所有编译器标志都相同

配置 Visual Studio Linux 项目配置以使用 Clang：

1. 右键单击“解决方案资源管理器”中的项目节点，并选择“属性” 。
1. 通常，应首先选择对话框顶部的“所有配置”。
1. 如果使用适用于 Linux 的 Windows 子系统 (WSL)，请在“常规” >  “平台工具集”中选择“WSL_Clang_1_0”。 如果使用远程计算机或 VM，请选择“Remote_Clang_1_0”。
1. 按“确定”。

![已选中“配置属性”>“常规”且已突出显示“平台工具集和 L L V M (clang c l)”选项的控制台应用 clang Visual Studio 2019“属性页”对话框的屏幕截图。](media/clang-msbuild-prop-page.png)

在 Linux 上，Visual Studio 在默认情况下使用在 PATH 环境属性中找到的第一个 Clang 位置。 如果使用自定义 Clang 安装，可更改 `LLVMInstallDir` 属性的值，或在“项目” > “属性” > “配置属性” > “VC++ 目录” > “可执行文件目录”下输入路径。 有关详细信息，请参阅[设置自定义 LLVM 位置](#custom_llvm_location)。

## <a name="set-a-custom-llvm-location"></a><a name="custom_llvm_location"></a> 设置自定义 LLVM 位置

可以通过创建 Directory.build.props 文件，为一个或多个项目设置 LLVM 的自定义路径。 然后，将该文件添加到任意项目的根文件夹。 可以将它添加到根解决方案文件夹，以将它应用于解决方案中的所有项目。 文件应如下所示（但使用实际 LLVM 路径）：

```xml
<Project>
  <PropertyGroup>
    <LLVMInstallDir>C:\MyLLVMRootDir</LLVMInstallDir>
  </PropertyGroup>
</Project>
```

可以将此属性与自定义 LLVM 工具集版本结合使用。 有关详细信息，请参阅[设置自定义 LLVM 工具集版本](#custom_llvm_toolset)。

## <a name="set-a-custom-llvm-toolset-version"></a><a name="custom_llvm_toolset"></a> 设置自定义 LLVM 工具集版本

从 Visual Studio 2019 版本 16.9 开始，你可以为 LLVM 设置自定义工具集版本。 若要在 Visual Studio 中的项目中设置此属性，请执行以下操作：

1. 打开项目的“属性页”  对话框。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](./working-with-project-properties.md)。

1. 选择“配置属性”“常规”属性页 > 。

1. 如果尚未设置“平台工具集”属性，请将其修改为“LLVM (clang-cl)”。

1. 选择“配置属性”>“高级”属性页 。

1. 将“LLVM 工具集版本”属性修改为首选版本，然后选择“确定”保存更改。 

“LLVM 工具集版本”属性仅在选择 LLVM 平台工具集时显示。

可以通过创建 Directory.build.props 文件，为一个或多个项目设置工具集版本。 然后，将该文件添加到任意项目的根文件夹。 可以将它添加到根解决方案文件夹，以将它应用于解决方案中的所有项目。 文件应如下所示（但使用实际 LLVM 路径）：

```xml
<Project>
  <PropertyGroup>
    <LLVMToolsVersion>11.0.0</LLVMToolsVersion>
  </PropertyGroup>
</Project>
```

还可以将此属性与自定义 LLVM 位置结合使用。 例如，Directory.build.props 文件可能如下所示：

```xml
<Project>
  <PropertyGroup>
    <LLVMInstallDir>C:\MyLLVMRootDir</LLVMInstallDir>
    <LLVMToolsVersion>11.0.0</LLVMToolsVersion>
  </PropertyGroup>
</Project>
```

在项目“属性页”对话框中添加 Directory.build.props 文件时，设置将显示为默认值。 但是，在 Visual Studio 中对这些属性的更改将覆盖 Directory.build.props 文件中的设置。

## <a name="set-additional-properties-edit-build-and-debug"></a>设置其他属性、编辑、生成和调试

设置 Clang 配置之后，再次右键单击项目节点，然后选择“重新加载项目”。 现在可以使用 Clang 工具生成和调试项目。 Visual Studio 会检测到在使用 Clang 编译器，并提供 IntelliSense、突出显示、导航和其他编辑功能。 错误和警告会显示在“输出窗口”中。 Clang 配置的项目属性页与 MSVC 的类似。 但是，某些依赖于编译器的功能（如“编辑”和“继续”）不可用于 Clang 配置。 可以设置在属性页中不可用的 Clang 编译器或链接器选项。 可在“配置属性” > “C/ C++ (或链接器)” > “命令行” > “其他选项”下的属性页中手动添加它。

调试时，可以使用断点、内存和数据可视化以及大多数其他调试功能。  

![Clang 调试](media/clang-debug-msbuild.png)

::: moniker-end
