---
description: 了解详细信息：将 Windows 窗体用户控件作为 MFC 视图承载
title: 以 MFC 视图的形式承载 Windows 窗体用户控件
ms.date: 11/04/2016
helpviewer_keywords:
- MFC [C++], Windows Forms support
- Windows Forms controls [C++], hosting as an MFC view
- hosting Windows Forms control [C++]
ms.assetid: 43c02ab4-1366-434c-a980-0b19326d6ea0
ms.openlocfilehash: 2db44bd4d85150b0a2b8fc1f2206af765770e9ed
ms.sourcegitcommit: 3987d9c39f5a5b4824303a48a6215984ce8949e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478121"
---
# <a name="hosting-a-windows-forms-user-control-as-an-mfc-view"></a>以 MFC 视图的形式承载 Windows 窗体用户控件

MFC 使用 CWinFormsView 类在 MFC 视图中承载 Windows 窗体用户控件。 MFC Windows 窗体视图是 ActiveX 控件。 用户控件作为本机视图的子级承载，并占据本机视图的整个工作区。

最终结果类似于 [CFormView 类](../mfc/reference/cformview-class.md)使用的模型。 这使您可以利用 Windows 窗体设计器和运行时来创建基于窗体的丰富视图。

由于 MFC Windows 窗体视图是 ActiveX 控件，因此它们 `hwnd` 与 MFC 视图不同。 此外，不能将它们作为指向 [CView](../mfc/reference/cview-class.md) 视图的指针传递。 通常，使用 .NET Framework 方法来处理 Windows 窗体视图，并在 Win32 上依赖更少。

有关演示与 MFC 一起使用 Windows 窗体的示例应用程序，请参阅 [mfc 和 Windows 窗体集成](https://download.cnet.com/MFC-and-WinForms-Integration/3000-2383_4-75453644.html)。

## <a name="in-this-section"></a>本节内容

[如何：创建用户控件并承载 MDI 视图](../dotnet/how-to-create-the-user-control-and-host-mdi-view.md)

[如何：将命令路由添加到 Windows 窗体控件](../dotnet/how-to-add-command-routing-to-the-windows-forms-control.md)

[如何：调用 Windows 窗体控件的属性和方法](../dotnet/how-to-call-properties-and-methods-of-the-windows-forms-control.md)

## <a name="see-also"></a>请参阅

[在 MFC 中使用 Windows 窗体用户控件](../dotnet/using-a-windows-form-user-control-in-mfc.md)<br/>
[如何：创作复合控件](/dotnet/framework/winforms/controls/how-to-author-composite-controls)
