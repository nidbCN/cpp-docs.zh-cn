---
description: 了解详细信息： Windows 窗体/MFC 编程差异
title: Windows 窗体-MFC 编程差异
ms.date: 11/04/2016
helpviewer_keywords:
- MFC [C++], Windows Forms support
- Windows Forms [C++], compared to MFC
ms.assetid: f3bfcf45-cfd4-45a4-8cde-5f4dbb18ee51
ms.openlocfilehash: abb41a64a894eed682c26b2460259fc6bbc43b56
ms.sourcegitcommit: 3987d9c39f5a5b4824303a48a6215984ce8949e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478014"
---
# <a name="windows-formsmfc-programming-differences"></a>Windows 窗体/MFC 编程差异

在 [mfc 中使用 Windows 窗体用户控件](../dotnet/using-a-windows-form-user-control-in-mfc.md) 中的主题介绍了对 WINDOWS 窗体的 MFC 支持。 如果不熟悉 .NET Framework 或 MFC 编程，本主题提供了有关这两者之间的编程差异的背景信息。

Windows 窗体用于在 .NET Framework 上创建 Microsoft Windows 应用程序。 此框架提供了一组新式的、面向对象的可扩展类，可用于开发基于 Windows 的丰富应用程序。 使用 Windows 窗体，你可以创建一个丰富的客户端应用程序，该应用程序可以访问各种数据源，并使用 Windows 窗体控件提供数据显示和数据编辑功能。

但是，如果您习惯于 MFC，则可以使用创建特定类型的应用程序，这些应用程序在 Windows 窗体中尚未显式支持。 Windows 窗体应用程序等效于 MFC 对话框应用程序。 但是，它们不提供直接支持其他 MFC 应用程序类型（如 OLE 文档服务器/容器、ActiveX 文档、单文档界面的文档/视图支持 (SDI) 、多文档界面 (MDI) 和多个顶级接口 (MTI) 的基础结构。 可以编写自己的逻辑来创建这些应用程序。

有关 Windows 窗体应用程序的详细信息，请参阅 [Windows 窗体简介](/dotnet/framework/winforms/windows-forms-overview)。

有关演示与 MFC 一起使用 Windows 窗体的示例应用程序，请参阅 [mfc 和 Windows 窗体集成](https://download.cnet.com/MFC-and-WinForms-Integration/3000-2383_4-75453644.html)。

以下 MFC 视图或文档和命令路由功能在 Windows 窗体中没有等效项：

- Shell 集成

   MFC 处理动态数据交换 (DDE) 命令以及 shell 在您右键单击文档时使用的命令行参数，并选择打开、编辑或打印这样的谓词。 Windows 窗体没有 shell 集成，并且不响应 shell 谓词。

- 文档模板

   在 MFC 中，文档模板将视图（包含在框架窗口中 (在 MDI、SDI 或 MTI 模式) 中，以及打开的文档。 Windows 窗体与文档模板不等效。

- 文档

   在从 shell 打开文档时，MFC 注册文档文件类型并处理文档类型。 Windows 窗体没有文档支持。

- 文档状态

   MFC 维护文档的脏状态。 因此，在关闭应用程序时，关闭包含该应用程序的最后一个视图，或者退出 Windows，MFC 会提示您保存该文档。 Windows 窗体没有等效的支持。

- 命令

   MFC 具有命令的概念。 菜单栏、工具栏和上下文菜单都可以调用相同的命令，例如，"剪切" 和 "复制"。 在 Windows 窗体中，命令紧密绑定来自特定 UI 元素的事件 (例如菜单项) ;因此，你必须显式挂钩所有命令事件。 你还可以使用 Windows 窗体中的单个处理程序处理多个事件。 有关详细信息，请参阅 [在 Windows 窗体中将多个事件连接到单个事件处理程序](/dotnet/framework/winforms/how-to-connect-multiple-events-to-a-single-event-handler-in-windows-forms)。

- 命令路由

   MFC 命令路由使活动视图或文档可以处理命令。 因为相同的命令对于不同视图通常具有不同的含义 (例如，在文本编辑视图中，复制行为与图形编辑器) 中的行为不同，因此，这些命令需要由活动视图处理。 由于 Windows 窗体菜单和工具栏对活动视图不具有内在了解，因此您不能为 MenuItem 的每个视图类型都有不同的处理程序 **。单击** 事件，无需编写额外的内部代码。

- 命令更新机制

   MFC 具有命令更新机制。 因此，活动视图或文档负责 UI 元素的状态 (例如，启用或禁用菜单项或工具按钮，并) 选中状态。 Windows 窗体没有等效于命令更新机制。

## <a name="see-also"></a>请参阅

[在 MFC 中使用 Windows 窗体用户控件](../dotnet/using-a-windows-form-user-control-in-mfc.md)
