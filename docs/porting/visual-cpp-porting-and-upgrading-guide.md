---
title: Microsoft c + + 移植和升级指南
description: 将 Microsoft c + + 代码升级到最新版本的 Visual Studio。
ms.date: 09/10/2020
ms.assetid: f5fbcc3d-aa72-41a6-ad9a-a706af2166fb
ms.topic: overview
ms.openlocfilehash: 624d5a7d85cfc6387f7668cd2fcf6f8f8746a402
ms.sourcegitcommit: 92120d0c418b6d3d85d32ffc050c59c5f5f9d5b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104670812"
---
# <a name="microsoft-c-porting-and-upgrading-guide"></a>Microsoft c + + 移植和升级指南

本文提供将 Microsoft c + + 代码升级到最新版本的 Visual Studio 的指南。 对于在 Visual Studio 2010 到2017中创建的项目，只需在 Visual Studio 2019 中打开该项目。 可以通过两个步骤升级 Visual Studio 2008 或更早版本的项目。 首先，使用 Visual Studio 2010 将项目转换为 MSBuild 格式。 然后在 Visual Studio 2019 中打开该项目。 有关完整说明，请参阅 [从早期版本的 Visual Studio 升级 c + + 项目](upgrading-projects-from-earlier-versions-of-visual-cpp.md)。

Visual Studio 2015、Visual Studio 2017 和 Visual Studio 2019 中的工具集与二进制兼容。 现在，你可以升级到较新版本的编译器，而不必升级库依赖项。 有关详细信息，请参阅 [c + + 二进制兼容性 2015-2019](binary-compat-2015-2017.md)。

升级使用开源库的项目或要在多个平台上运行的项目时，我们建议迁移到基于 CMake 的项目。 有关详细信息，请参阅 [Visual Studio 中的 CMake 项目](../build/cmake-projects-in-visual-studio.md)

## <a name="reasons-to-upgrade-c-code"></a>升级 c + + 代码的原因

如果旧的应用程序运行在安全的环境中，而不是处于活动状态，则可能无需再进行升级。 但在这种情况下，请考虑升级：应用程序需要持续维护。 或者，您要进行全新的功能开发，或者进行性能或安全性改进。 升级带来了以下好处：

- 这一代码可以更快地运行，因为我们已经改进了编译器优化。

- 新式 c + + 功能和编程做法消除了许多常见的 bug 原因，并生成的代码比旧的 C 样式惯例更易于维护。

- 由于编译器和链接器中的性能改进，生成时间更快。

- 更好的标准一致性。 [/Permissive-](../build/reference/permissive-standards-conformance.md)编译器选项有助于识别不符合当前 c + + 标准的代码。 [新的预处理器](../preprocessor/preprocessor-experimental-overview.md)也支持代码一致性。

- 更好的运行时安全性，包括更安全的 [C 运行时库](../c-runtime-library/security-features-in-the-crt.md) 功能。 此外，在 Visual Studio 2019 版本 16.4) 中 (新增功能，例如， [防护检查](../build/reference/guard-enable-guard-checks.md) 和地址 sanitizers 等编译器功能。

## <a name="multitargeting-vs-upgrading"></a>多定向与升级

或许您不能将基本代码升级到新的工具集。 你仍可以使用最新的 Visual Studio 来生成和编辑使用较旧工具集和库的项目。 在 Visual Studio 2019 中，可以利用如下功能：

- 新式静态分析工具，包括 C++ Core Guidelines 跳棋和 Clang，以帮助确定你的源代码中可能存在的问题。

- 根据您选择的新式样式自动设置格式可帮助使旧代码更具可读性。

有关详细信息，请参阅 [使用 Visual Studio 中的本机多重目标生成旧项目](use-native-multi-targeting.md)。

## <a name="in-this-section"></a>本节内容

|Title|说明|
|-----------|-----------------|
|[从 Visual Studio 的早期版本升级 c + + 项目](upgrading-projects-from-earlier-versions-of-visual-cpp.md)|如何将基本代码升级到 Visual Studio 2019 和编译器的 v142。|
|[用于升级 C++ 代码的 IDE 工具](ide-tools-for-upgrading-code.md)|有助于升级过程的有用 IDE 功能。|
|[C++ 二进制兼容性 (2015-2019)](binary-compat-2015-2017.md)|从 v142 项目按原样使用 v140 和 v141 库。|
|[使用 Visual Studio 中的本机多目标来生成旧项目](use-native-multi-targeting.md)|将 Visual Studio 2019 与较旧的编译器和库配合使用。|
|[Visual C++ 更改历史记录（2003 - 2015）](visual-cpp-change-history-2003-2015.md)|Microsoft c + + 库中的所有更改的列表，以及 Visual Studio 2003 到2015中的生成工具，可能需要在代码中进行更改。|
|[Visual C++ 新增功能 (2003 - 2015)](visual-cpp-what-s-new-2003-through-2015.md)|Visual Studio 2003 到 Visual Studio 2015 的 Microsoft c + + 的所有 "新增功能" 信息。|
|[移植和升级：示例和案例研究](porting-and-upgrading-examples-and-case-studies.md)|本部分中，我们移植和升级了多个示例和应用程序并讨论了体验和结果。 通过这些文章可了解移植和升级过程中所涉及的内容。 在整个过程中，我们讨论了升级所用的提示和技巧，并演示如何修复特定错误。|
|[移植到通用 Windows 平台](porting-to-the-universal-windows-platform-cpp.md)|包含有关移植代码到 Windows 10 的信息|
|[Visual C++ 简介（针对 UNIX 用户）](introduction-to-visual-cpp-for-unix-users.md)|为不熟悉 Visual C++ 并想要有效率的使用它的 UNIX 用户提供信息。|
|[在 Windows 上运行 Linux 程序](porting-from-unix-to-win32.md)|讨论用于将 UNIX 应用程序迁移到 Windows 的选项。|

## <a name="see-also"></a>请参阅

[Visual Studio 中的 C++](../overview/visual-cpp-in-visual-studio.md)<br/>
[Visual Studio 中 C++ 编译器的新增功能](../overview/what-s-new-for-visual-cpp-in-visual-studio.md)<br/>
[Visual Studio 中的 C++ 符合性改进](../overview/cpp-conformance-improvements.md)<br/>
