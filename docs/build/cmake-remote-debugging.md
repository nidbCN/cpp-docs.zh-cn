---
title: 教程：在远程 Windows 计算机上调试 CMake 项目
ms.date: 12/4/2020
ms.topic: tutorial
description: 如何在 Windows 上使用 Visual Studio C++ 来创建和生成 CMake 项目。 然后在远程 Windows 计算机上部署和调试它。
ms.openlocfilehash: 575b815559cbe70384b4669c94cf003daa3ee7d7
ms.sourcegitcommit: 5efc34c2b98d4d0d3e41aec38b213f062c19d078
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101844502"
---
# <a name="tutorial-debug-a-cmake-project-on-a-remote-windows-machine"></a>教程：在远程 Windows 计算机上调试 CMake 项目

本教程使用 Windows 上的 Visual Studio C++ 来创建和生成一个可以在远程 Windows 计算机上部署和调试的 CMake 项目。 本教程特定于 WindowsARM64，但可以对这些步骤进行一般化，使其适用于其他体系结构。

在 Visual Studio 中，ARM64 的默认调试体验是远程调试 ARM64 Windows 计算机。 如果尝试调试 ARM64 CMake 项目而不配置本教程中所示的调试设置，则会出现一个错误，即 Visual Studio 找不到远程计算机。

本教程介绍以下操作：

> [!div class="checklist"]
>
> * 创建 CMake 项目
> * 配置要为 ARM64 生成的 CMake 项目
> * 将 CMake 项目配置为在远程 ARM64 Windows 计算机上运行
> * 调试在远程 ARM64 Windows 计算机上运行的 CMake 项目

## <a name="prerequisites"></a>先决条件

### <a name="on-the-host-machine"></a>在主机上

若要设置 Visual Studio 以用于进行跨平台 C++ 开发，请安装目标体系结构的生成工具。 对于本教程，请执行以下操作安装 ARM64 生成工具：

1. 运行 Visual Studio 安装程序。 如果尚未安装 Visual Studio，请参阅[安装 Visual Studio](/visualstudio/install/install-visual-studio)
1. 在 Visual Studio 安装程序主屏幕上，选择“修改”。
1. 在顶部的选项中，选择“单个组件”。
1. 向下滚动到“编译器、生成工具和运行时”部分。
1. 确保选择了以下选项：
    - **用于 Windows 的 C++ CMake 工具**
    - **MSVC v142 - VS 2019 C++ ARM64 生成工具（最新版本）** 关键是选择 `ARM64` 生成工具而不是 `ARM` 生成工具（查找 64），并选择与 `VS 2019` 匹配的版本。
1. 选择“修改”以安装工具。

### <a name="on-the-remote-machine"></a>在远程计算机上

1. 在远程计算机上安装远程工具。 对于本教程，请按照[下载并安装远程工具](/visualstudio/debugger/remote-debugging-cpp#download-and-install-the-remote-tools)中的说明安装 ARM64 工具。
1. 在远程计算机上启动并配置远程调试器。 对于本教程，请遵循远程 Windows 计算机上[设置远程调试器](/visualstudio/debugger/remote-debugging-cpp#BKMK_setup)中的说明进行操作。

## <a name="create-a-cmake-project"></a>创建 CMake 项目

在 Windows 主机上：
1. 运行 Visual Studio
1. 在主菜单中，选择“文件” > “新建” > “项目”  。
1. 选择“CMake 项目” > “下一步” 
1. 为项目提供名称，然后选择一个位置。 然后选择“创建”。

给 Visual Studio 一些时间来创建项目并填充“解决方案资源管理器”。

## <a name="configure-for-arm64"></a>配置 ARM64

若要面向 ARM64 Windows 计算机，需要使用 ARM64 生成工具进行生成。

选择 Visual Studio“配置”下拉列表，然后选择“管理配置” 。

![在 Visual Studio 配置下拉列表中选择“管理配置”](media/vs2019-cmake-manage-configurations.png)

通过选择“添加新配置”（绿色的 + 按钮）添加新配置。\ 
在出现的“CMakeSettings”对话框中，选择“arm64-debug”，然后按“选择”  ：

![添加 arm64-debug 配置](media/cmake-add-config-icon-with-target-dialog.png)

这会将名为“arm64-Debug”的调试配置添加到 `CmakeSettings.json` 文件中。 此配置名称是唯一的、友好的名称，它使你更容易在“配置”下拉列表中标识这些设置。

“工具集”下拉列表设置为 msvc_arm64_x64 。 现在，设置应如下所示：

![CMake 设置对话框](media/cmake-settings-editor2.png)

> [!Note]
> 在“工具集”下拉列表中，“msvc_arm64”选择要交叉编译到 ARM64 的 32 位主机工具，而 msvc_arm64 x64 选择要交叉编译到 ARM64 的 64 位主机工具（这是你将在本教程中执行的操作）  。

保存 `CMakeSettings.json` 文件。 在配置下拉列表中，选择 arm64-debug（保存 `CMakeSettings.json` 文件后可能需要一段时间才能显示在列表中）：

![确保在 Visual Studio 配置下拉列表中选择了 arm64-debug](media/vs2019-cmake-manage-configurations-arm.png) 

## <a name="add-a-debug-configuration-file"></a>添加调试配置文件

接下来，添加告诉 Visual Studio 在哪里可以找到远程计算机的配置信息，以及其他配置详细信息。

通过选择“切换视图”按钮，将“解决方案资源管理器”视图更改为目标视图 ：

![解决方案资源管理器切换视图按钮](media/solution-explorer-switch-view.png)

然后，在“解决方案资源管理器”中，双击“CMake 目标视图”以查看项目 。

打开项目文件夹（在本例中为“CMakeProject3 项目”），然后右键单击可执行文件并选择“添加调试配置” ：

![选择添加调试配置](media/cmake-targets-add-debug-configuration.png)

此操作会在项目中创建 `launch.vs.json` 文件。 打开它并更改以下条目以启用远程调试：

- `projectTarget`：如果按照上述说明从“解决方案资源管理器”目标视图添加调试配置文件，则会设置此选项。
- `remoteMachineName`：设置为远程 ARM64 计算机的 IP 地址或其计算机名称。

有关 `launch.vs.json` 设置的详细信息，请参阅 [launch.vs.json 架构参考](launch-vs-schema-reference-cpp.md)。

> [!Note]
>  如果要在“解决方案资源管理器”中使用文件夹视图而不是目标视图，请右键单击 `CMakeLists.txt` 文件并选择“添加调试配置” 。 此体验与从目标视图添加调试配置的不同之处在于：
> - 系统将要求你选择调试器（选择“C/C++ 远程 Windows 调试”）。
> - Visual Studio 将在 `launch.vs.json` 文件中提供较少的配置模板信息，因此需要自行添加。 需要提供 `remoteMachineName` 和 `projectTarget` 条目。 从目标视图添加配置时，只需指定 `remoteMachineName`。
> - 对于 `projectTarget` 设置值，请查看“启动项”下拉列表以获取目标的唯一名称，例如，在本教程中为“CMakeProject3.exe”。

## <a name="start-the-remote-debugger-monitor-on-the-remote-windows-machine"></a>在远程 Windows 计算机上启动远程调试器监视器

在运行 CMake 项目之前，请确保在远程 Windows 计算机上运行 Visual Studio 2019 远程调试器。  可能需要根据身份验证情况更改远程调试器选项。

例如，在远程计算机上，从 Visual Studio 远程调试器菜单栏中，选择“工具” > “选项” 。 设置“身份验证模式”以匹配环境的设置方式：

![远程调试器身份验证选项](media/remote-debugger-options.png)

然后，在主机上的 Visual Studio 中，更新 `launch.vs.json` 文件以进行匹配。 例如，如果在远程调试器上选择“无需身份验证”，则通过将 "authenticationType": "none" 添加到 `configurations` 部分中的 `launch.vs.json` 来更新项目中的 `launch.vs.json` 文件 。 否则，`"authenticationType"` 默认为 `"windows"`，不需要显式声明。 此示例显示了针对无需身份验证配置的 `launch.vs.json` 文件：

``` XAML
{
  "version": "0.2.1",
  "defaults": {},
  "configurations": [
    {
        "type": "remoteWindows",
        "authenticationType": "none"
        "name": "CMakeLists.txt",
        "project": "CMakeLists.txt",
        "projectTarget": "CMakeProject3.exe",
        "remoteMachineName": "<ip address goes here>",
        "cwd": "${debugInfo.defaultWorkingDirectory}",
        "program": "${debugInfo.fullTargetPath}",
        "deploy": [],
        "args": [],
        "env": {}
    },
    {
      "type": "default",
      "project": "CMakeLists.txt",
      "projectTarget": "CMakeProject3.exe",
      "name": "CMakeProject3.exe"
    }
  ]
}
```

## <a name="debug-the-app"></a>调试应用

在主机上的 Visual Studio“解决方案资源管理器”中，打开 CMake 项目的 CPP 文件。 如果仍处于“CMake 目标视图”，则需要打开“(executable)”节点才能看到该文件 。

默认的 CPP 文件是一个简单的 hello world 控制台应用。 在 `return 0;` 上设置断点。

在 Visual Studio 工具栏上，使用“启动项”下拉列表选择在 `launch.vs.json` 文件中为 `"name"` 指定的名称：

![选择了 CMakeProject3.exe 的“启动项”下拉列表示例](media/startup-item.png)

若要开始调试，请在 Visual Studio 工具栏上选择“调试” > “开始调试”（或按“F5”）  。

如果没有启动，请确保在 `launch.vs.json` 文件中正确设置以下内容：
- `"remoteMachineName"` 应设置为远程 ARM64 Windows 计算机的 IP 地址或计算机名称。
- `"name"` 应与 Visual Studio 启动项下拉列表中的选择相匹配。
- `"projectTarget"` 应与要调试的 CMake 目标的名称匹配。
- `"type"` 应为 `"remoteWindows"`
- 如果远程调试器上的身份验证类型设置为“无需身份验证”，则应该在 `launch.vs.json` 文件中设置 `"authenticationType": "none"`。
- 如果使用的是 Windows 身份验证，请在出现提示时使用远程计算机可识别的帐户登录。

项目生成后，应用应出现在远程 ARM64 Windows 计算机上：

![在远程 Windows ARM64 计算机上运行的 Hello CMake 控制台应用](media/remote-cmake-app.png)

应在 `return 0;` 的断点处停止主机上的 Visual Studio。

## <a name="what-you-learned"></a>已了解的内容

在本教程中，你已创建 CMake 项目，将其配置为在 ARM64 上针对 Windows 进行生成，并在远程 ARM64 Windows 计算机上对其进行了调试。

## <a name="next-steps"></a>后续步骤

了解有关在 Visual Studio 中配置和调试 CMake 项目的更多信息：

> [!div class="nextstepaction"]
> [Visual Studio 中的 CMake 项目](cmake-projects-in-visual-studio.md)\
> [自定义 CMake 生成设置](customize-cmake-settings.md)\
> [配置 CMake 调试会话](configure-cmake-debugging-sessions.md)\
> [CMake 预定义配置引用](cmake-predefined-configuration-reference.md)\
[launch.vs.json 架构引用](launch-vs-schema-reference-cpp.md)
