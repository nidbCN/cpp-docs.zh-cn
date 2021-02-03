---
description: 了解详细信息：在 MFC 对话框中承载 Windows 窗体用户控件
title: 在 MFC 对话框中承载 Windows 窗体用户控件
ms.date: 11/04/2016
helpviewer_keywords:
- MFC [C++], Windows Forms support
- hosting Windows Forms control [C++]
- Windows Forms [C++], MFC support
ms.assetid: 9f66ee52-b7cb-4ffd-8306-392a5da990d8
ms.openlocfilehash: 36e4677622e4c1efe1eebcb8cb9f75d03194de78
ms.sourcegitcommit: 3987d9c39f5a5b4824303a48a6215984ce8949e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478126"
---
# <a name="hosting-a-windows-form-user-control-in-an-mfc-dialog-box"></a>在 MFC 对话框中承载 Windows 窗体用户控件

MFC 将 Windows 窗体控件作为一种特殊类型的 ActiveX 控件承载，并使用 ActiveX 接口以及类的属性和方法与控件进行通信 <xref:System.Windows.Forms.Control> 。 建议使用 .NET Framework 属性和方法来操作控件。

有关演示与 MFC 一起使用 Windows 窗体的示例应用程序，请参阅 [mfc 和 Windows 窗体集成](https://download.cnet.com/MFC-and-WinForms-Integration/3000-2383_4-75453644.html)。

> [!NOTE]
> 在当前版本中， `CDialogBar` 对象不能承载 Windows 窗体控件。

## <a name="in-this-section"></a>本节内容

[如何：在对话框中创建用户控件和宿主](../dotnet/how-to-create-the-user-control-and-host-in-a-dialog-box.md)

[如何：执行 DDX/DDV 数据绑定与 Windows 窗体](../dotnet/how-to-do-ddx-ddv-data-binding-with-windows-forms.md)

[如何：接收来自本机 c + + 类的 Windows 窗体事件](../dotnet/how-to-sink-windows-forms-events-from-native-cpp-classes.md)

## <a name="reference"></a>参考

[CWinFormsControl 类](../mfc/reference/cwinformscontrol-class.md)&#124; [CWnd 类](../mfc/reference/cwnd-class.md)&#124; [CDialog 类](../mfc/reference/cdialog-class.md)&#124;<xref:System.Windows.Forms.Control>

## <a name="see-also"></a>请参阅

[在 MFC 中使用 Windows 窗体用户控件](../dotnet/using-a-windows-form-user-control-in-mfc.md)<br/>
[Windows 窗体/MFC 编程差异](../dotnet/windows-forms-mfc-programming-differences.md)<br/>
[以 MFC 视图的形式承载 Windows 窗体用户控件](../dotnet/hosting-a-windows-forms-user-control-as-an-mfc-view.md)<br/>
[将 Windows 窗体用户控件作为 MFC 对话框承载](../dotnet/hosting-a-windows-form-user-control-as-an-mfc-dialog-box.md)
