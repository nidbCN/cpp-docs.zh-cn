---
title: C++ 二进制兼容性 (2015-2019)
description: 介绍 Visual Studio 2015、2017和2019中编译的 c + + 文件之间的二进制兼容性。 一个 Microsoft Visual C++ 可再发行组件包适用于所有三个版本。
ms.date: 02/17/2021
helpviewer_keywords:
- binary compatibility, Visual C++
ms.openlocfilehash: d8c4c0312003496db522e59dba84a9633e94b1b4
ms.sourcegitcommit: 5efc34c2b98d4d0d3e41aec38b213f062c19d078
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101844515"
---
# <a name="c-binary-compatibility-between-visual-studio-2015-2017-and-2019"></a>Visual Studio 2015、2017和2019之间的 c + + 二进制兼容性

Visual Studio 2013 和更早版本中的 Microsoft c + + (MSVC) 编译器工具集不保证跨主要版本的二进制兼容性。 不能链接对象文件、静态库、动态库以及由这些工具集的不同版本生成的可执行文件。 Abi、对象格式和运行库是不兼容的。

我们已在 Visual Studio 2015、2017和2019中更改了此行为。 任何这些版本的编译器编译的运行时库和应用都是二进制兼容的。 它反映在 c + + 工具集的主编号（从14开始，适用于所有三个版本）。  (工具集版本是 v140 for Visual Studio 2015、v141 for 2017 和 v142 for 2019) 。 假设你有 Visual Studio 2015 生成的第三方库。 你仍可以在 Visual Studio 2017 或2019生成的应用程序中使用它们。 无需使用匹配的工具集进行重新编译。 最新版本的 Microsoft Visual C++ 可再发行组件包 (可再发行组件) 适用于所有这些包。

## <a name="restrictions-on-binary-compatibility"></a>二进制兼容性的限制

对于 v140、v141 和 v142 工具集和次要编号版本更新之间的二进制兼容性，有三个重要的限制：

- 可以混合使用不同版本的 v140、v141 和 v142 工具集生成的二进制文件。 但是，必须使用最新的工具集至少与应用中的最新二进制链接。 下面是一个示例：可以将使用任何2017工具集的应用程序 (v141，版本15.0 到 15.9) 链接到使用版本16.2 或更高版本的工具集进行编译的静态库，如 Visual Studio 2019 版本 16.2 (v142) 。 只要你使用的是16.4 或更高版本的工具集，你就可以将版本16.2 库链接到16.4 版应用。

- 应用使用的可再发行组件具有类似的二进制兼容性限制。 当混合使用不同的受支持版本的工具集生成的二进制文件时，可再发行版本必须至少与任何应用组件所用的最新工具集相同。

- 使用 [ `/GL` (完全程序优化)](../build/reference/gl-whole-program-optimization.md)编译器开关或使用 [ `/LTCG` (链接时代码生成](../build/reference/ltcg-link-time-code-generation.md)进行链接的静态库或对象文件) 在不同版本之间 *不* 兼容二进制，包括次要版本更新。 使用和编译的所有对象文件和库 **`/GL`** **`/LTCG`** 必须使用完全相同的用于编译和最终链接的工具集。 例如， **`/GL`** 在 Visual studio 2019 版本16.7 工具集中使用生成的代码无法链接到使用 **`/GL`** visual studio 2019 版本16.8 工具集中生成的代码。 编译器发出 [错误 C1047](../error-messages/compiler-errors-1/fatal-error-c1047.md)。

## <a name="upgrade-the-microsoft-visual-c-redistributable-from-visual-studio-2015-or-2017-to-visual-studio-2019"></a>将 Microsoft Visual C++ 可再发行组件从 Visual Studio 2015 或2017升级到 Visual Studio 2019

我们为 Visual Studio 2015、2017和2019保留了相同的 Microsoft Visual C++ 可再发行组件主版本号。 这意味着一次只能安装一个可再发行的实例。 较新的版本将覆盖已安装的任何较旧版本。 例如，一个应用可以安装 Visual Studio 2015 中的可再发行组件。 然后，另一个应用从 Visual Studio 2019 安装可再发行组件。 2019版本覆盖了较旧的版本，但由于它们是二进制兼容的，所以前面的应用程序仍能正常工作。 我们确保最新版本的可再发行组件包含所有最新功能、安全更新和 bug 修复。 这就是我们始终建议升级到最新可用版本的原因。

同样，如果已安装较新版本，则无法安装较旧的可再发行组件。 如果尝试，安装程序将报告错误。 如果在已有2019版本的计算机上安装2015或2017可再发行组件，会看到类似于下面的错误：

```Output
0x80070666 - Another version of this product is already installed. Installation of this version cannot continue. To configure or remove the existing version of this product, use Add/Remove Programs on the Control Panel.
```

此错误是由设计决定的。 建议保留最新版本。 请确保安装程序可在不提示的情况下从此错误恢复。

## <a name="see-also"></a>另请参阅

[Visual C++ 更改历史记录](../porting/visual-cpp-change-history-2003-2015.md)\
[最新支持的 Visual C++ 可再发行组件下载](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
