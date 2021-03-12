---
description: 了解详细信息：确定要重新分发的 Dll
title: 确定要重新分发的 DLL
ms.date: 03/10/2021
helpviewer_keywords:
- redistributing DLLs
- DLLs [C++], redistributing
- dependencies [C++], application deployment and
- application deployment [C++], DLL redistribution
- deploying applications [C++], DLL redistribution
ms.assetid: f7a2cb42-fb48-42ab-abd2-b35e2fd5601a
ms.openlocfilehash: 8adc9d6093cec18f4591f3929a84a55fcfc2f6d7
ms.sourcegitcommit: f8ba5db09d05683b24c58505f0e57c21f85545dc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103087202"
---
# <a name="determining-which-dlls-to-redistribute"></a>确定要重新分发的 DLL

在生成使用由 Visual Studio 提供的库 DLL 的应用程序时，应用程序的用户也必须在运行该应用程序的计算机上安装这些 DLL。 由于大多数用户可能没有安装 Visual Studio，因此你必须为他们提供这些 DLL。 Visual Studio 以可再发行文件的形式提供这些 DLL，可以将其包含到应用程序的安装程序中。

若要更轻松地将可再发行 DLL 包含到你的安装程序，能以独立可再发行组件包的形式获得它们。 这些是特定于体系结构的可执行文件，它们使用集中部署将可再发行文件安装在用户的计算机上。 例如，vcredist \_x86.exe 安装适用于 x86 和 x64 计算机的32位库，vcredist \_x64.exe 安装适用于 x64 计算机的64位库，vcredist \_ARM.exe 为 ARM 计算机安装库。 建议采用集中部署，因为 Microsoft 可以使用 Windows 更新服务来分别更新这些库。 除了 Visual Studio 安装中的副本，还可下载当前可再发行组件包。 若要查看当前和较旧版本工具集支持的最新可再发行组件包的链接，请参阅[最新支持的 Visual C++ 下载](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)。 通过在 [Microsoft 下载中心](https://go.microsoft.com/fwlink/p/?LinkId=158431)中搜索“Visual C++ 可再发行组件包”，也许能找到特定较早版本的可再发行组件包。

仅支持使用与工具集版本相同或更高版本的可再发行组件包。 所部署的可再发行组件包的主版本号必须与创建应用程序的 Visual Studio 工具集的版本相匹配，并且次版本必须为相同版本或更高版本。 Visual Studio 2019、2017和2015都具有兼容的工具集版本号，这意味着，使用较旧版本的工具集生成的应用程序可能会使用较新的 Visual Studio 可再发行文件。 例如，使用 Visual Studio 2017 或2015工具集生成的应用可能会使用 Visual Studio 2019 可再发行文件。 尽管它们可能是兼容的，但我们不支持使用较新的工具集生成的应用程序中使用较旧的可再发行文件。 例如，不支持在使用2019工具集生成的应用中使用2017可再发行文件。

将可再发行 DLL 包含到你的安装程序的另一种方式是使用合并模块。 这些 Microsoft 安装程序模块由应用程序安装程序包括并安装。 可再发行 Dll 的合并模块位于 \\ VC \\ Redist\MSVC \\ *版本* \\ MergeModules 下的 Visual Studio 安装目录中 \\ 。 在早期版本的 Visual Studio 中，这些文件位于 \\Program Files 或 \\Program Files (x86) 目录的 Common Files\\Merge Modules 子目录中。 有关使用这些文件的详细信息，请参阅[使用合并模块重新分发组件](redistributing-components-by-using-merge-modules.md)。

这些单独的可再发行 DLL也随 Visual Studio 安装。 默认情况下，它们都安装在 \\VC\\Redist\\MSVC\\version 文件夹中的 Visual Studio 安装目录中。 版本号可能表示通用的一组可再发行文件的不同次要版本号。 使用在这些目录中找到的任何库 DLL 文件、可再发行组件包或合并模块的最新版本。 可将这些库安装在与应用程序相同的目录中，以便将它们用于本地部署。 不建议进行本地部署，因为这样做就需要由你将更新内容传递到所部署的应用程序。 最好选择使用可再发行组件包进行集中部署。

若要确定必须与应用程序一起重新发布的 DLL，请收集应用程序所依赖的 DLL 列表。 这些通常列为链接器的导入库输入。 默认包括一些特定的库（例如 vcruntime 和通用 C 运行时库 (UCRT)）。 如果你的应用或应用的某个依赖项使用 LoadLibrary 来动态加载 DLL，此 DLL 可能不会列入对链接器的输入。 若要收集动态加载的 DLL 的列表，有一种方式在应用上运行依赖项查看器 (depends.exe)，如[了解 Visual C++ 应用程序的依赖项](understanding-the-dependencies-of-a-visual-cpp-application.md)中所述。 遗憾的是此工具已过时，并且可能会报告无法找到特定的 DLL。

具有依赖项列表时，将该列表与在 Microsoft Visual Studio 安装目录下找到的 Redist.txt 文件中链接的列表进行比较，或者与你的 Visual Studio 副本的 Microsoft 软件许可条款中“可分发代码”文件部分引用的可再发行 DLL“REDIST 列表”进行比较。 对于 Visual Studio 2019，请参阅 [Visual studio 2019 的可分发代码](/visualstudio/releases/2019/redistribution)。  对于 Visual Studio 2017，请参阅 [Microsoft Visual Studio 2017 的可分发代码（包括实用程序、可扩展性和 BuildServer 文件）](/visualstudio/productinfo/2017-redistribution-vs)。 对于 Visual Studio 2015，请参阅 [Microsoft Visual Studio 2015 和 Microsoft Visual Studio 2015 SDK 的可分发代码（包括实用程序和 BuildServer 文件）](/visualstudio/productinfo/2015-redistribution-vs)。 对于 Visual Studio 2013，列表可以在 [Microsoft Visual Studio 2013 和 Microsoft Visual Studio 2013 SDK 的可分发代码](/visualstudio/productinfo/2013-redistribution-vs)中联机获得。

在 Visual Studio 2015 以前的版本中，C 运行时库 (CRT) 作为可再发行 DLL 包含在 msvcversion.dll 中。 从 Visual Studio 2015 开始，CRT 中的函数已重构到 vcruntime 和 UCRT 中。 UCRT 现在是 Windows 10 中由 Windows 更新托管的系统组件。 它在所有 Windows 10 操作系统上都可用。 若要将应用程序部署到较早的操作系统，可能还需要重新分发 UCRT。 Visual Studio 可再发行文件中包括较早版本的 UCRT，它仅安装在 Windows 10 以前的操作系统上，且仅当未安装任何版本的 UCRT 时才会安装它。 若要获取能作为 Microsoft 系统更新包安装在下层系统的 UCRT 版本，请参阅 Microsoft 下载中心中的 [Windows 10 通用 C 运行时](https://www.microsoft.com/download/details.aspx?id=48234)。

无法重新发布 Visual Studio 中包含的所有文件；只允许重新发布 Redist.txt 或在线“REDIST 列表”中指定的文件。 调试版本的应用程序和各种 Visual C++ 调试 DLL 是不可再发行的。 有关详细信息，请参阅[选择部署方法](choosing-a-deployment-method.md)。

下表描述了应用程序可能依赖的一些 Visual C++ DLL。

|Visual C++ 库|描述|适用于|
|--------------------------|-----------------|----------------|
|vcruntimeversion.dll|本机代码的运行库。|使用常规 C 和 C++ 语言启用和终止服务的应用程序。|
|vccorlibversion.dll|托管代码的运行库。|使用托管代码的 C++ 语言服务的应用程序。|
|msvcpversion.dll 和 msvcpversion_dotnumber.dll|本机代码的 C++ 标准库。|使用 [C++ 标准库](../standard-library/cpp-standard-library-reference.md)的应用程序。|
|concrtversion.dll|本机代码的并发运行库。|使用[并发运行时](../parallel/concrt/concurrency-runtime.md)的应用程序。|
|mfcversion.dll|Microsoft 基础类 (MFC) 库。|使用 [MFC 库](../mfc/mfc-desktop-applications.md)的应用程序。|
| mfcversionlanguage.dll|Microsoft 基础类 (MFC) 库资源。|使用 MFC 特定语言资源的应用程序。|
|mfcversionu.dll|具有 Unicode 支持的 MFC 库。|使用 [MFC 库](../mfc/mfc-desktop-applications.md)并需要 Unicode 支持的应用程序。|
|mfcmifc80.dll|MFC 托管接口库。|使用具有 [Windows 窗体控件](../mfc/mfc-desktop-applications.md)的 [MFC 库](/dotnet/framework/winforms/controls/index)的应用程序。|
|mfcmversion.dll|MFC 托管库。|使用具有 [Windows 窗体控件](../mfc/mfc-desktop-applications.md)的 [MFC 库](/dotnet/framework/winforms/controls/index)的应用程序。|
|mfcmversionu.dll|具有 Unicode 支持的 MFC 托管库。|使用具有 [Windows 窗体控件](../mfc/mfc-desktop-applications.md)的 [MFC 库](/dotnet/framework/winforms/controls/index)并需要 Unicode 支持的应用程序。|
|vcampversion.dll|本机代码的 AMP 库。|使用 [C++ AMP 库](../parallel/amp/cpp-amp-cpp-accelerated-massive-parallelism.md)代码的应用程序。|
|vcompversion.dll|本机代码的 OpenMP 库。|使用 [C++ OpenMP 库](../parallel/openmp/openmp-in-visual-cpp.md)代码的应用程序。|

> [!NOTE]
> 不再需要将活动模板库重新发布为单独的 DLL。 其功能已移至标头和静态库。

有关如何使用应用程序重新分布这些 DLL 的更多信息，请参阅[重新分布 Visual C++ 文件](redistributing-visual-cpp-files.md)。 有关示例，请参阅[部署示例](deployment-examples.md)。

通常情况下，无需重新发布系统 DLL，因为它们是操作系统的一部分。 但可能存在例外，例如，当应用程序将在几个版本的 Microsoft 操作系统上运行时。 在这种情况下，请务必阅读相应的许可条款。 此外，请尝试通过 Microsoft 提供的 Windows 更新、Service Pack 或使用可再发行组件包来升级系统 DLL。

## <a name="see-also"></a>另请参阅

[选择部署方法](choosing-a-deployment-method.md)<br/>
[部署桌面应用程序](deploying-native-desktop-applications-visual-cpp.md)
