---
description: 了解详细信息：将 Windows 窗体用户控件作为 MFC 对话框承载
title: 以 MFC 对话框的形式承载 Windows 窗体用户控件
ms.date: 11/04/2016
helpviewer_keywords:
- MFC [C++], Windows Forms support
- Windows Forms [C++], hosting as MFC Dialog
- hosting Windows Forms control [C++]
ms.assetid: 0434a9d7-8b14-48e6-ad69-9ba9a684677a
ms.openlocfilehash: 64c68ede565b4248a812d46963f072fbab8bdc3c
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522815"
---
# <a name="hosting-a-windows-form-user-control-as-an-mfc-dialog-box"></a>以 MFC 对话框的形式承载 Windows 窗体用户控件

MFC 提供模板类 [CWinFormsDialog](../mfc/reference/cwinformsdialog-class.md) ，以便可以 <xref:System.Windows.Forms.UserControl> 在模式或无模式 MFC 对话框中 () 承载 Windows 窗体用户控件。 `CWinFormsDialog` 派生自 MFC 类 [CDialog](../mfc/reference/cdialog-class.md)，因此可以将该对话框作为模式或无模式启动。

`CWinFormsDialog`使用承载用户控件的进程类似于在[MFC 对话框中承载 Windows 窗体用户控件](../dotnet/hosting-a-windows-form-user-control-in-an-mfc-dialog-box.md)中所述的过程。 不过， `CWinFormsDialog` 管理用户控件的初始化和承载，以便不必手动对其进行编程。

### <a name="to-create-the-mfc-host-application"></a>创建 MFC 宿主应用程序

1. 创建 MFC 应用程序项目。

   在 " **文件** " 菜单上，选择 " **新建**"，然后单击 " **项目**"。 在 **Visual C++** 文件夹中，选择 " **MFC 应用程序**"。

   在 " **名称** " 框中，输入 `MFC03` 并更改要 **添加到解决方案** 的解决方案设置。单击 **"确定"**。

   在 **MFC 应用程序向导** 中，接受所有默认值，然后单击 " **完成**"。 这会创建包含多个文档界面的 MFC 应用程序。

1. 配置项目。

   在 **解决方案资源管理器** 中，右键单击 **MFC03** 项目节点，然后选择 " **属性**"。 " **属性页** " 对话框随即出现。

   在 " **属性页** " 对话框的 " **配置属性** " 树控件中，选择 " **常规**"，然后在 " **项目默认值** " 部分中，将 " **公共语言运行时支持** " 设置为 **公共语言运行时支持 (/clr)**。 单击“确定”。

1. 添加对 .NET 控件的引用。

   在 **解决方案资源管理器** 中，右键单击 **MFC03** 项目节点，然后选择 " **添加**"、" **引用**"。 在 **属性页** 中，单击 **"添加新引用**"，选择 " **项目** " 选项卡下的 "WindowsControlLibrary1 (") ，然后单击 **"确定"**。 这会添加一个 [/FU](../build/reference/fu-name-forced-hash-using-file.md) 编译器选项形式的引用，以便程序进行编译;它还会将 WindowsControlLibrary1.dll 复制到 `MFC03` 项目目录中，以便程序运行。

1. 在 `#include <afxwinforms.h>` 现有语句的末尾，添加到 (Visual Studio 2017 和更早) 版本中的 *stdafx.h。* `#include`

1. 添加子类为的新类 `CDialog` 。

   右键单击 "项目名称"，并添加一个名为 CHostForWinForm 的 MFC 类 () 该子类 `CDialog` 。 由于不需要对话框资源，因此可以删除资源 ID (选择 " **资源视图**"，展开 **对话框** 文件夹并删除 " `IDD_HOSTFORWINFORM` 资源"。  然后，在代码中删除对该 ID 的所有引用。 ) 。

1. 将 `CDialog` CHostForWinForm 和 CHostForWinForm 文件中的替换为 `CWinFormsDialog<WindowsControlLibrary1::UserControl1>` 。

1. 在 CHostForWinForm 类上调用 DoModal。

   在 MFC03 中，添加 `#include "HostForWinForm.h"` 。

   在 CMFC03App：： InitInstance 的定义中的 return 语句之前，请添加：

    ```cpp
    CHostForWinForm m_HostForWinForm;
    m_HostForWinForm.DoModal();
    ```

1. 生成并运行该项目。

   在 **“生成”** 菜单上，单击 **“生成解决方案”** 。

   在 " **调试** " 菜单上，单击 " **启动（不调试**）"。

   接下来，您将添加代码，以便从 MFC 应用程序监视 Windows 窗体上的控件的状态。

1. 添加 OnInitDialog 的处理程序。

   显示 " **属性** " 窗口 (F4) 。 在 **类视图** 中，选择 "CHostForWinForm"。 在 " **属性** " 窗口中，选择 "替代"，并在 OnInitDialog 的行中，单击左侧列并选择 \< Add > 。 这会将以下行添加到 CHostForWinForm：

    ```cpp
    virtual BOOL OnInitDialog();
    ```

1. 定义 CHostForWinForm 中的 OnInitDialog (，如下所示) ：

    ```cpp
    BOOL CHostForWinForm::OnInitDialog() {
       CWinFormsDialog<WindowsControlLibrary1::UserControl1>::OnInitDialog();
       GetControl()->button1->Click += MAKE_DELEGATE(System::EventHandler, OnButton1);
       return TRUE;
    }
    ```

1. 接下来，添加 OnButton1 处理程序。 将以下行添加到 CHostForWinForm 中 CHostForWinForm 类的 public 节：

    ```cpp
    virtual void OnButton1( System::Object^ sender, System::EventArgs^ e );

    BEGIN_DELEGATE_MAP( CHostForWinForm )
       EVENT_DELEGATE_ENTRY( OnButton1, System::Object^, System::EventArgs^ );
    END_DELEGATE_MAP()
    ```

   在 CHostForWinForm 中，添加以下定义：

    ```cpp
    void CHostForWinForm::OnButton1( System::Object^ sender, System::EventArgs^ e )
    {
       System::Windows::Forms::MessageBox::Show("test");
    }
    ```

1. 生成并运行该项目。 单击 Windows 窗体上的按钮时，MFC 应用程序中的代码将运行。

    接下来，您将添加代码，以便在 Windows 窗体上的文本框中显示 MFC 代码中的值。

1. 在 CHostForWinForm 中 CHostForWinForm 类的 public 节中，添加以下声明：

    ```cpp
    CString m_sEditBoxOnWinForm;
    ```

1. 在 CHostForWinForm 中 DoDataExchange 的定义中，在函数的末尾添加以下三行：

    ```cpp
    if (pDX->m_bSaveAndValidate)
       m_sEditBoxOnWinForm = CString( GetControl()->textBox1->Text);
    else
       GetControl()->textBox1->Text = gcnew System::String(m_sEditBoxOnWinForm);
    ```

1. 在 CHostForWinForm 中 OnButton1 的定义中，在函数的末尾添加以下三行：

    ```cpp
    this->UpdateData(TRUE);
    System::String ^ z = gcnew System::String(m_sEditBoxOnWinForm);
    System::Windows::Forms::MessageBox::Show(z);
    ```

1. 生成并运行该项目。

## <a name="see-also"></a>另请参阅

<xref:System.Windows.Forms.UserControl?displayProperty=fullName>
[在 MFC 中使用 Windows 窗体用户控件](../dotnet/using-a-windows-form-user-control-in-mfc.md)
