---
description: 详细了解：CMakeSettings.json 架构引用
title: CMakeSettings.json 架构引用
ms.date: 11/22/2019
helpviewer_keywords:
- CMake in Visual C++
ms.assetid: 444d50df-215e-4d31-933a-b41841f186f8
ms.openlocfilehash: e68154d84123d416aa9d9d18f538c4b62fddce18
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465348"
---
# <a name="cmakesettingsjson-schema-reference"></a>CMakeSettings.json 架构引用

::: moniker range="msvc-140"

Visual Studio 2017 及更高版本支持 CMake 项目。

::: moniker-end

::: moniker range=">=msvc-150"

CMakeSettings.json 文件包含 Visual Studio 为 IntelliSense 使用的信息，并为指定的配置和编译器环境构造它传递给 cmake.exe 的命令行参数。 配置指定适用于特定平台和生成类型的属性，例如 `x86-Debug` 或 `Linux-Release`。 每个配置都指定一个环境，该环境封装有关编译器工具集的信息，例如 MSVC、GCC 或 Clang。 CMake 使用命令行参数为项目重新生成根文件 CMakeCache.txt 和其他项目文件。 可以在 CMakeLists.txt 文件中覆盖这些值。

可以在 IDE 中添加或删除配置，然后直接在 JSON 文件中对其进行编辑，或使用“CMake 设置编辑器”（Visual Studio 2019 及更高版本）。 可以在 IDE 中轻松切换配置，以生成各种项目文件。 有关详细信息，请参阅[在 Visual Studio 中自定义 CMake 生成设置](customize-cmake-settings.md)。

## <a name="configurations"></a>配置

`configurations` 数组包含 CMake 项目的所有配置。 有关预定义配置的详细信息，请参阅 [CMake 预定义配置参考](cmake-predefined-configuration-reference.md)。 可以向该文件添加任意数量的预定义或自定义配置。

`configuration` 具有以下属性：

- `addressSanitizerEnabled`：如果 `true` 使用 [AddressSanitizer](../sanitizers/asan.md) 编译程序。 在 Linux 上，使用 -fno-omit-frame-pointer 和编译器优化级别 -Os 或 -Oo 进行编译，以获得最佳结果。
- `addressSanitizerRuntimeFlags`：通过 ASAN_OPTIONS 环境变量传递给 [AddressSanitizer](../sanitizers/asan.md) 的运行时标志。 格式：flag1=value:flag2=value2。
- `buildCommandArgs`：指定在“生成”后传递给 CMake 的本机生成开关。 例如，使用 Ninja 生成器强制输出命令行使用 Ninja 时，传递 -v。 请参阅 [Ninja 命令行参数](#ninja)，详细了解 Ninja 命令。
- `buildRoot`：指定 CMake 生成器要在其中为所选生成器生成脚本的目录。  映射到 -DCMAKE_BINARY_DIR 开关，并指定要创建 CMakeCache.txt 的位置。 如果文件夹不存在，则会创建一个。 支持的宏包括 `${workspaceRoot}`、`${workspaceHash}`、`${projectFile}`、`${projectDir}`、`${thisFile}`、`${thisFileDir}`、`${name}`、`${generator}`、`${env.VARIABLE}`。
- `cacheGenerationCommand`：指定命令行工具和参数，例如，指定 gencache.bat debug 来生成缓存。 当用户显式请求重新生成或者修改了 CMakeLists.txt 或 CMakeSettings.json 文件时，系统会从配置的指定环境中的 shell 运行该命令。
- `cacheRoot`：指定 CMake 缓存的路径。 此目录应包含现有 CMakeCache.txt 文件。
- `clangTidyChecks`：要传递给 clang-tidy 的警告的逗号分隔列表；可使用通配符，“-”前缀将移除检查。
- `cmakeCommandArgs`：指定调用 CMake 以生成项目文件时传递给 CMake 的其他命令行选项。
- `cmakeToolchain`：指定工具链文件。 这通过 -DCMAKE_TOOLCHAIN_FILE 传递到 CMake。
- `codeAnalysisRuleset`：指定运行代码分析时要使用的规则集。 这可以是完整路径或由 Visual Studio 安装的规则集文件的文件名。
- `configurationType`：为选定的生成器指定生成类型配置。 可能是以下其中之一：

  - 调试
  - Release
  - MinSizeRel
  - RelWithDebInfo
  
- `ctestCommandArgs`：指定运行测试时传递给 CTest 的其他命令行选项。
- `description`：将在菜单中显示的此配置的说明。
- `enableClangTidyCodeAnalysis`：使用 Clang-Tidy 进行代码分析。
- `enableMicrosoftCodeAnalysis`：使用 Microsoft 代码分析工具进行代码分析。
- `generator`：指定将在此配置中使用的 CMake 生成器。 可能是以下其中之一：
  
  **仅限 Visual Studio 2019：**
  - Visual Studio 16 2019
  - Visual Studio 16 2019 Win64
  - Visual Studio 16 2019 ARM

  **Visual Studio 2017 及更高版本：**
  - Visual Studio 15 2017
  - Visual Studio 15 2017 Win64
  - Visual Studio 15 2017 ARM
  - Visual Studio 14 2015
  - Visual Studio 14 2015 Win64
  - Visual Studio 14 2015 ARM
  - Unix 生成文件
  - Ninja

由于 Ninja 旨在加快生成速度，而不是为了提高灵活性和功能，因此它被设为默认生成器。 但是，某些 CMake 项目可能无法使用 Ninja 正确地进行生成。 如果发生这种情况，可以指示 CMake 改为生成 Visual Studio 项目。

若要指定 Visual Studio 2017 中的 Visual Studio 生成器，请选择“CMake”|“更改 CMake 设置”，从主菜单打开设置编辑器。 删除“Ninja”并键入“V”。 这可激活 IntelliSense，从而可让用户选择所需生成器。

若要指定 Visual Studio 2019 中的 Visual Studio 生成器，请右键单击“解决方案资源管理器”中的 CMakeLists.txt 文件，并选择“项目的 CMake 设置”>“显示高级设置”>“CMake 生成器”   。

当活动配置指定一个 Visual Studio 生成器时，默认情况下使用 `-m -v:minimal` 参数调用 MSBuild.exe。 若要自定义生成，可在 CMakeSettings.json 文件中指定要通过 `buildCommandArgs` 属性传递到生成系统的其他 [MSBuild 命令行参数](../build/reference/msbuild-visual-cpp-overview.md)：

   ```json
   "buildCommandArgs": "-m:8 -v:minimal -p:PreferredToolArchitecture=x64"
   ```

- `installRoot`：指定 CMake 生成器要在其中为所选生成器安装目标的目录。 支持的宏包括 `${workspaceRoot}`、`${workspaceHash}`、`${projectFile}`、`${projectDir}`、`${thisFile}`、`${thisFileDir}`、`${name}`、`${generator}`、`${env.VARIABLE}`。
- `inheritEnvironments`：指定此配置依赖的一个或多个编译器环境。 可以是任何自定义环境或预定义环境之一。 有关详细信息，请参阅[环境](#environments)。
- `intelliSenseMode`：指定用于计算 IntelliSense 信息的模式。 可能是以下其中之一：

  - windows-msvc-x86
  - windows-msvc-x64
  - windows-msvc-arm
  - windows-msvc-arm64
  - android-clang-x86
  - android-clang-x64
  - android-clang-arm
  - android-clang-arm64
  - ios-clang-x86
  - ios-clang-x64
  - ios-clang-arm
  - ios-clang-arm64
  - windows-clang-x86
  - windows-clang-x64
  - windows-clang-arm
  - windows-clang-arm64
  - linux-gcc-x86
  - linux-gcc-x64
  - linux-gcc-arm”

- `name`：为配置命名。  有关预定义配置的详细信息，请参阅 [CMake 预定义配置参考](cmake-predefined-configuration-reference.md)。
- `wslPath`：适用于 Linux 的 Windows 子系统的实例的启动器路径。

### <a name="additional-settings-for-cmake-linux-projects"></a>适用于 CMake Linux 项目的其他设置

- `remoteMachineName`：指定托管 CMake、生成和调试程序的远程 Linux 计算机的名称。 使用连接管理器添加新的 Linux 计算机。 支持的宏包括 `${defaultRemoteMachineName}`。
- `remoteCopySourcesOutputVerbosity`：指定将源复制到远程计算机的操作的详细级别。 可以是“常规”、“详细”或“诊断”其中之一。
- `remoteCopySourcesConcurrentCopies`：指定将源同步到远程计算机时使用的并发副本数（仅限 sftp）。
- `remoteCopySourcesMethod`：指定将文件复制到远程计算机的方法。 可以是“rsync”或“sftp”。
- `remoteCMakeListsRoot`：指定包含 CMake 项目的远程计算机上的目录。 支持的宏包括 `${workspaceRoot}`、`${workspaceHash}`、`${projectFile}`、`${projectDir}`、`${thisFile}`、`${thisFileDir}`、`${name}`、`${generator}`、`${env.VARIABLE}`。
- `remoteBuildRoot`：指定 CMake 生成器要在其中为所选生成器生成脚本的远程计算机上的目录。 支持的宏包括 `${workspaceRoot}`、`${workspaceHash}`、`${projectFile}`、`${projectDir}`、`${thisFile}`、`${thisFileDir}`、`${name}`、`${generator}`、`${env.VARIABLE}`。
- `remoteInstallRoot`：指定 CMake 生成器要在其中为所选生成器安装目标的远程计算机上的目录。 支持的宏包括 `${workspaceRoot}`、`${workspaceHash}`、`${projectFile}`、`${projectDir}`、`${thisFile}`、`${thisFileDir}`、`${name}`、`${generator}` 和 `${env.VARIABLE}`，其中最后一项的环境变量为 `VARIABLE`且已在系统、用户或会话级别进行定义。
- `remoteCopySources`：`boolean`，指定 Visual Studio 是否应将源文件复制到远程计算机。 默认值为 true。 如果自行管理文件同步，则设置为 false。
- `remoteCopyBuildOutput`：`boolean`，指定是否要从远程计算机复制生成输出。
- `remoteCopyAdditionalIncludeDirectories`：要从远程计算机复制以支持 IntelliSense 的附加包含目录。 格式为“/path1;/path2...”。
- `remoteCopyExcludeDirectories`：不从远程计算机复制的包含目录。 格式为“/path1;/path2...”。
- `remoteCopyUseCompilerDefaults`：指定是否使用编译器的默认定义并包含 IntelliSense 的路径。 仅当使用的编译器不支持 gcc 样式的参数时，才应为 false。
- `rsyncCommandArgs`：指定传递给 rsync 的附加命令行选项集。
- `remoteCopySourcesExclusionList`：指定复制源文件时要排除的路径列表的 `array`：路径可以是文件/目录的名称，或副本根目录的相对路径。 可使用通配符“\\\"*\\\"”和“\\\"?\\\"”进行 glob 模式匹配。
- `cmakeExecutable`：指定 CMake 程序可执行文件的完整路径，包括文件名和扩展名。
- `remotePreGenerateCommand`：指定在运行 CMake 以分析 CMakeLists.txt 文件之前要运行的命令。
- `remotePrebuildCommand`：指定生成前要在远程计算机上运行的命令。
- `remotePostbuildCommand`：指定生成后要在远程计算机上运行的命令。
- `variables`：包含将以 -D name=value 的形式传递给 CMake 的 CMake 变量名称/值对 。 如果 CMake 项目生成指令指定将任何变量直接添加到 CMakeCache.txt 文件，那么建议改为在这里添加它们。 下面的示例演示如何为 14.14.26428 MSVC 工具集指定名称/值对：

```json
"variables": [
    {
      "name": "CMAKE_CXX_COMPILER",
      "value": "C:/Program Files (x86)/Microsoft Visual Studio/157/Enterprise/VC/Tools/MSVC/14.14.26428/bin/HostX86/x86/cl.exe",
      "type": "FILEPATH"
    },
    {
      "name": "CMAKE_C_COMPILER",
      "value": "C:/Program Files (x86)/Microsoft Visual Studio/157/Enterprise/VC/Tools/MSVC/14.14.26428/bin/HostX86/x86/cl.exe",
      "type": "FILEPATH"
    }
  ]
```

请注意，如果未定义 `"type"`，则默认采用 `"STRING"` 类型。

- `remoteCopyOptimizations`：Visual Studio 2019 16.5 或更高版本的属性，用于对源到远程目标的复制进行控制。 默认情况下启用优化。 包括 `remoteCopyUseOptimizations`、`rsyncSingleDirectoryCommandArgs` 和 `remoteCopySourcesMaxSmallChange`。

## <a name="environments"></a><a name="environments"></a> 环境

环境封装了在 Visual Studio 用来调用 cmake.exe 的进程中设置的环境变量。 对于 MSVC 项目，变量是指在[开发人员命令提示](building-on-the-command-line.md)中为特定平台设置的变量。 例如，`msvc_x64_x64` 环境与运行带有 -arch=amd64 -host_arch=amd64 参数的适用于 VS 2017 的开发人员命令提示或适用于 VS 2019 的开发人员命令提示是一样的  。 可以使用 CMakeSettings.json 中的 `env.{<variable_name>}` 语法来引用各个环境变量，例如用于构造文件夹路径。  下面提供了一些预定义环境：

- linux_arm：远程将 ARM Linux 设为目标。
- linux_x64：远程将 x64 Linux 设为目标。
- linux_x86：远程将 x86 Linux 设为目标。
- msvc_arm：将具有 MSVC 编译器的 ARM Windows 设为目标。
- msvc_arm_x64：将具有 64 位 MSVC 编译器的 ARM Windows 设为目标。
- msvc_arm64：将具有 MSVC 编译器的 ARM64 Windows 设为目标。
- msvc_arm64_x64：将具有 64 位 MSVC 编译器的 ARM64 Windows 设为目标。
- msvc_x64：将具有 MSVC 编译器的 x64 Windows 设为目标。
- msvc_x64_x64：将具有 64 位 MSVC 编译器的 x64 Windows 设为目标。
- msvc_x86：将具有 MSVC 编译器的 x86 Windows 设为目标。
- msvc_x86_x64：将具有 64 位 MSVC 编译器的 x86 Windows 设为目标。

### <a name="accessing-environment-variables-from-cmakeliststxt"></a>从 CMakeLists.txt 访问环境变量

在 CMakeLists.txt 文件中，语法 `$ENV{variable_name}` 可引用所有环境变量。 若要查看环境的可用变量，请打开相应的命令提示，然后键入 `SET`。 环境变量中的某些信息也可以通过 CMake 系统自检变量获得，但你可能会发现使用环境变量更方便。 例如，可以通过环境变量轻松检索 MSVC 编译器版本或 Windows SDK 版本。

### <a name="custom-environment-variables"></a>自定义环境变量

在 `CMakeSettings.json` 中，可在 `environments` 数组中以全局形式或按配置定义自定义环境变量。 通过使用自定义环境，可以轻松地对一组可用于代替预定义环境的属性进行分组，或者扩展或修改预定义环境。 `environments` 数组中的每一项都包含以下内容：

- `namespace`：为环境命令，以便可从窗体 `namespace.variable` 中的配置中引用其变量。 默认环境对象称为 `env` 并由特定系统环境变量（包括 `%USERPROFILE%`）填充。
- `environment`：唯一标识此组变量。 允许稍后在 `inheritEnvironments` 输入中继承该组。
- `groupPriority`：评估变量时指定这些变量的优先级的整数。 优先评估数字较大的项。
- `inheritEnvironments`：指定由该组继承的一组环境的数组值。 使用该功能可以继承默认环境，以及创建在运行时传递给 CMake.exe 的自定义环境变量。

**Visual Studio 2019 版本 16.4 及更高版本：** 调试目标会在你在 CMakeSettings.json 中指定的环境中自动启动。 可以在 [launch.vs.json](launch-vs-schema-reference-cpp.md) 和 [tasks.vs.json](tasks-vs-json-schema-reference-cpp.md) 中按目标或按任务覆盖或添加环境变量。

下面的示例定义一个全局变量“BuildDir”，该变量在 x86-Debug 和 x64-Debug 配置中继承。 每个配置都使用该变量来指定配置的“buildRoot”属性值。 也请注意每个配置如何使用“inheritEnvironments”属性来指定仅应用于该配置的值。

```json
{
  // The "environments" property is an array of key value pairs of the form
  // { "EnvVar1": "Value1", "EnvVar2": "Value2" }
  "environments": [
    {
      "BuildDir": "${env.USERPROFILE}\\CMakeBuilds\\${workspaceHash}\\build",
    }
  ],

  "configurations": [
    {
      "name": "x86-Debug",
      "generator": "Ninja",
      "configurationType": "Debug",
      // Inherit the defaults for using the MSVC x86 compiler.
      "inheritEnvironments": [ "msvc_x86" ],
      "buildRoot": "${env.BuildDir}\\${name}"    },
    {
      "name": "x64-Debug",
      "generator": "Ninja",
      "configurationType": "Debug",
      // Inherit the defaults for using the MSVC x64 compiler.
      "inheritEnvironments": [ "msvc_x64" ],
      "buildRoot": "${env.BuildDir}\\${name}"
    }
  ]
}
```

在下一个示例中，x86-Debug 配置为 BuildDir 属性定义其自己的值。 此值替代由全局 BuildDir 属性设置的值，以便 BuildRoot 评估为 `D:\custom-builddir\x86-Debug` 。

```json
{
  "environments": [
    {
      "BuildDir": "${env.USERPROFILE}\\CMakeBuilds\\${workspaceHash}",
    }
  ],

  "configurations": [
    {
      "name": "x86-Debug",

      // The syntax for this property is the same as the global one above.
      "environments": [
        {
          // Replace the global property entirely.
          "BuildDir": "D:\\custom-builddir"
          // This environment does not specify a namespace, hence by default "env" will be assumed.
          // "namespace" : "name" would require that this variable be referenced with "${name.BuildDir}".
        }
      ],

      "generator": "Ninja",
      "configurationType": "Debug",
      "inheritEnvironments": [ "msvc_x86" ],
      // Evaluates to "D:\custom-builddir\x86-Debug"
      "buildRoot": "${env.BuildDir}\\${name}"
    },
    {
      "name": "x64-Debug",

      "generator": "Ninja",
      "configurationType": "Debug",
      "inheritEnvironments": [ "msvc_x64" ],
      // Since this configuration doesn't modify BuildDir, it inherits
      // from the one defined globally.
      "buildRoot": "${env.BuildDir}\\${name}"
    }
  ]
}
```

## <a name="macros"></a>宏

可以在 CMakeSettings.json 中使用以下宏：

- `${workspaceRoot}`：工作区文件夹的完整路径
- `${workspaceHash}`：工作区位置的哈希；可用于创建当前工作区的唯一标识符（例如用于文件路径）
- `${projectFile}`：根 CMakeLists.txt 文件的完整路径
- `${projectDir}`：根 CMakeLists.txt 文件的文件夹完整路径
- `${thisFile}` - `CMakeSettings.json` 文件的完整路径
- `${name}`：配置的名称
- `${generator}`：配置中使用的 CMake 生成器的名称

对 CMakeSettings.json 中宏和环境变量的所有引用在传递给 cmake.exe 命令行之前都已展开。

## <a name="ninja-command-line-arguments"></a><a name="ninja"></a> Ninja 命令行参数

如果未指定目标，则生成“默认”目标。

```cmd
C:\Program Files (x86)\Microsoft Visual Studio\Preview\Enterprise>ninja -?
ninja: invalid option -- `-?'
usage: ninja [options] [targets...]
```

|选项|描述|
|--------------|------------|
| --version  | 打印 ninja 版本（“1.7.1”）|
|   -C DIR   | 在执行任何其他操作前更改为 DIR|
|   -f FILE  | 指定输入生成文件（默认值为 build.ninja）|
|   -j N     | 以并行方式运行 N 个作业（默认值为 14，派生自可用 CPU）|
|   -k N     | 持续操作，直到 N 个作业失败（默认值为 1）|
|   -l N     | 如果加载平均值大于 N，则不要启动新的作业|
|   -n       | 试运行（不运行命令但像命令行运行成功了一样操作）|
|   -v       | 在生成过程中显示所有命令行|
|   -d MODE  | 启用调试（使用 -d 列表列出模式）|
|   -t TOOL  | 运行次级工具（使用 -t 列表列出次级工具）。 终止顶级选项；有更多标志传递给工具|
|   -w FLAG  | 调整警告（使用-w 列表列出警告）|

::: moniker-end
