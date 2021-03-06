---
description: 了解详细信息： CMFCRibbonEdit 类
title: CMFCRibbonEdit 类
ms.date: 11/04/2016
f1_keywords:
- CMFCRibbonEdit
- AFXRIBBONEDIT/CMFCRibbonEdit
- AFXRIBBONEDIT/CMFCRibbonEdit::CMFCRibbonEdit
- AFXRIBBONEDIT/CMFCRibbonEdit::CanBeStretched
- AFXRIBBONEDIT/CMFCRibbonEdit::CopyFrom
- AFXRIBBONEDIT/CMFCRibbonEdit::CreateEdit
- AFXRIBBONEDIT/CMFCRibbonEdit::DestroyCtrl
- AFXRIBBONEDIT/CMFCRibbonEdit::DropDownList
- AFXRIBBONEDIT/CMFCRibbonEdit::EnableSpinButtons
- AFXRIBBONEDIT/CMFCRibbonEdit::GetCompactSize
- AFXRIBBONEDIT/CMFCRibbonEdit::GetEditText
- AFXRIBBONEDIT/CMFCRibbonEdit::GetIntermediateSize
- AFXRIBBONEDIT/CMFCRibbonEdit::GetTextAlign
- AFXRIBBONEDIT/CMFCRibbonEdit::GetWidth
- AFXRIBBONEDIT/CMFCRibbonEdit::HasCompactMode
- AFXRIBBONEDIT/CMFCRibbonEdit::HasFocus
- AFXRIBBONEDIT/CMFCRibbonEdit::HasLargeMode
- AFXRIBBONEDIT/CMFCRibbonEdit::HasSpinButtons
- AFXRIBBONEDIT/CMFCRibbonEdit::IsHighlighted
- AFXRIBBONEDIT/CMFCRibbonEdit::OnAfterChangeRect
- AFXRIBBONEDIT/CMFCRibbonEdit::OnDraw
- AFXRIBBONEDIT/CMFCRibbonEdit::OnDrawLabelAndImage
- AFXRIBBONEDIT/CMFCRibbonEdit::OnDrawOnList
- AFXRIBBONEDIT/CMFCRibbonEdit::OnEnable
- AFXRIBBONEDIT/CMFCRibbonEdit::OnHighlight
- AFXRIBBONEDIT/CMFCRibbonEdit::OnKey
- AFXRIBBONEDIT/CMFCRibbonEdit::OnLButtonDown
- AFXRIBBONEDIT/CMFCRibbonEdit::OnLButtonUp
- AFXRIBBONEDIT/CMFCRibbonEdit::OnRTLChanged
- AFXRIBBONEDIT/CMFCRibbonEdit::OnShow
- AFXRIBBONEDIT/CMFCRibbonEdit::Redraw
- AFXRIBBONEDIT/CMFCRibbonEdit::SetACCData
- AFXRIBBONEDIT/CMFCRibbonEdit::SetEditText
- AFXRIBBONEDIT/CMFCRibbonEdit::SetTextAlign
- AFXRIBBONEDIT/CMFCRibbonEdit::SetWidth
helpviewer_keywords:
- CMFCRibbonEdit [MFC], CMFCRibbonEdit
- CMFCRibbonEdit [MFC], CanBeStretched
- CMFCRibbonEdit [MFC], CMFCRibbonEdit
- CMFCRibbonEdit [MFC], CopyFrom
- CMFCRibbonEdit [MFC], CreateEdit
- CMFCRibbonEdit [MFC], DestroyCtrl
- CMFCRibbonEdit [MFC], DropDownList
- CMFCRibbonEdit [MFC], EnableSpinButtons
- CMFCRibbonEdit [MFC], GetCompactSize
- CMFCRibbonEdit [MFC], GetEditText
- CMFCRibbonEdit [MFC], GetIntermediateSize
- CMFCRibbonEdit [MFC], GetTextAlign
- CMFCRibbonEdit [MFC], GetWidth
- CMFCRibbonEdit [MFC], HasCompactMode
- CMFCRibbonEdit [MFC], HasFocus
- CMFCRibbonEdit [MFC], HasLargeMode
- CMFCRibbonEdit [MFC], HasSpinButtons
- CMFCRibbonEdit [MFC], IsHighlighted
- CMFCRibbonEdit [MFC], OnAfterChangeRect
- CMFCRibbonEdit [MFC], OnDraw
- CMFCRibbonEdit [MFC], OnDrawLabelAndImage
- CMFCRibbonEdit [MFC], OnDrawOnList
- CMFCRibbonEdit [MFC], OnEnable
- CMFCRibbonEdit [MFC], OnHighlight
- CMFCRibbonEdit [MFC], OnKey
- CMFCRibbonEdit [MFC], OnLButtonDown
- CMFCRibbonEdit [MFC], OnLButtonUp
- CMFCRibbonEdit [MFC], OnRTLChanged
- CMFCRibbonEdit [MFC], OnShow
- CMFCRibbonEdit [MFC], Redraw
- CMFCRibbonEdit [MFC], SetACCData
- CMFCRibbonEdit [MFC], SetEditText
- CMFCRibbonEdit [MFC], SetTextAlign
- CMFCRibbonEdit [MFC], SetWidth
ms.assetid: 9b85f1f2-446b-454e-9af9-104fdad8a897
ms.openlocfilehash: be88698de9f98c590efebd4e2e8780d810aa8b96
ms.sourcegitcommit: c0c9cdae79f19655e809a4979227c51bb19cff63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102236558"
---
# <a name="cmfcribbonedit-class"></a>CMFCRibbonEdit 类

实现位于功能区栏上的编辑控件。

## <a name="syntax"></a>语法

```cpp
class CMFCRibbonEdit : public CMFCRibbonButton
```

## <a name="members"></a>成员

### <a name="public-constructors"></a>公共构造函数

|“属性”|描述|
|----------|-----------------|
|[CMFCRibbonEdit::CMFCRibbonEdit](#cmfcribbonedit)|构造 `CMFCRibbonEdit` 对象。|

### <a name="public-methods"></a>公共方法

|“属性”|描述|
|----------|-----------------|
|[CMFCRibbonEdit::CanBeStretched](#canbestretched)|指示控件的高度是否 `CMFCRibbonEdit` 可以垂直增加到功能区行的高度。|
|[CMFCRibbonEdit::CMFCRibbonEdit](#cmfcribbonedit)|构造 `CMFCRibbonEdit` 对象。|
|[CMFCRibbonEdit：： CopyFrom](#copyfrom)|将指定对象的状态复制 `CMFCRibbonEdit` 到当前 `CMFCRibbonEdit` 对象。|
|[CMFCRibbonEdit::CreateEdit](#createedit)|为对象创建一个新文本框 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit：:D estroyCtrl](#destroyctrl)|销毁 `CMFCRibbonEdit` 对象。|
|[CMFCRibbonEdit：:D ropDownList](#dropdownlist)|下拉列表框。|
|[CMFCRibbonEdit::EnableSpinButtons](#enablespinbuttons)|启用和设置文本框的数值调节钮的范围。|
|[CMFCRibbonEdit：： GetCompactSize](#getcompactsize)|检索对象的压缩大小 `CFMCRibbonEdit` 。|
|[CMFCRibbonEdit::GetEditText](#getedittext)|检索文本框中的文本。|
|[CMFCRibbonEdit：： GetIntermediateSize](#getintermediatesize)|检索对象的中间大小 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit::GetTextAlign](#gettextalign)|检索文本框中文本的对齐方式。|
|[CMFCRibbonEdit::GetWidth](#getwidth)|检索控件的宽度（以像素为单位） `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit::HasCompactMode](#hascompactmode)|指示控件的显示大小是否 `CMFCRibbonEdit` 可以压缩。|
|[CMFCRibbonEdit::HasFocus](#hasfocus)|指示控件是否 `CMFCRIbbonEdit` 具有焦点。|
|[CMFCRibbonEdit::HasLargeMode](#haslargemode)|指示控件的显示大小是否 `CMFCRibbonEdit` 可能很大。|
|[CMFCRibbonEdit::HasSpinButtons](#hasspinbuttons)|指示文本框是否有数值调节钮。|
|[CMFCRibbonEdit::IsHighlighted](#ishighlighted)|指示是否 `CMFCRibbonEdit` 突出显示该控件。|
|[CMFCRibbonEdit::OnAfterChangeRect](#onafterchangerect)|当控件的显示矩形的尺寸更改时由框架调用 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit：： OnDraw](#ondraw)|由框架调用以绘制 `CMFCRibbonEdit` 控件。|
|[CMFCRibbonEdit::OnDrawLabelAndImage](#ondrawlabelandimage)|由框架调用，用于绘制控件的标签和图像 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit::OnDrawOnList](#ondrawonlist)|由框架调用，用于 `CMFCRibbonEdit` 在命令列表框中绘制控件。|
|[CMFCRibbonEdit::OnEnable](#onenable)|由框架调用，用于启用或禁用 `CMFCRibbonEdit` 控件。|
|[CMFCRibbonEdit::OnHighlight](#onhighlight)|当指针进入或离开控件的边界时由框架调用 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit::OnKey](#onkey)|当用户按下 keytip 并且控件具有焦点时由框架调用 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit::OnLButtonDown](#onlbuttondown)|`CMFCRibbonEdit`当用户在控件上按下鼠标左键时，由框架调用以更新控件。|
|[CMFCRibbonEdit::OnLButtonUp](#onlbuttonup)|当用户释放鼠标左键时由框架调用。|
|[CMFCRibbonEdit::OnRTLChanged](#onrtlchanged)|当布局更改方向时，由框架调用以更新 `CMFCRibbonEdit` 控件。|
|[CMFCRibbonEdit::OnShow](#onshow)|由框架调用以显示或隐藏 `CMFCRibbonEdit` 控件。|
|[CMFCRibbonEdit：：重绘](#redraw)|更新控件的显示 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit：： SetACCData](#setaccdata)|设置对象的可访问性数据 `CMFCRibbonEdit` 。|
|[CMFCRibbonEdit::SetEditText](#setedittext)|设置文本框中的文本。|
|[CMFCRibbonEdit::SetTextAlign](#settextalign)|设置文本框的文本对齐方式。|
|[CMFCRibbonEdit::SetWidth](#setwidth)|设置控件文本框的宽度 `CMFCRibbonEdit` 。|

## <a name="remarks"></a>备注

## <a name="example"></a>示例

下面的示例演示如何构造 `CMFCRibbonEdit` 对象，在编辑控件旁显示数值调节钮，并设置编辑控件的文本。 此代码片段是 [MS Office 2007 演示示例](../../overview/visual-cpp-samples.md)的一部分。

[!code-cpp[NVC_MFC_MSOffice2007Demo#7](../../mfc/reference/codesnippet/cpp/cmfcribbonedit-class_1.cpp)]

## <a name="requirements"></a>要求

**标头：** afxRibbonEdit

## <a name="cmfcribboneditcanbestretched"></a><a name="canbestretched"></a> CMFCRibbonEdit::CanBeStretched

指示 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的高度是否可以垂直增加到功能区行的高度。

```cpp
virtual BOOL CanBeStretched();
```

### <a name="return-value"></a>返回值

始终返回 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditcmfcribbonedit"></a><a name="cmfcribbonedit"></a> CMFCRibbonEdit::CMFCRibbonEdit

构造 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象。

```cpp
CMFCRibbonEdit(
    UINT nID,
    int nWidth,
    LPCTSTR lpszLabel = NULL,
    int nImage = -1);

CMFCRibbonEdit();
```

### <a name="parameters"></a>参数

*nID*<br/>
中控件的命令 ID `CMFCRibbonEdit` 。

*nWidth*<br/>
中控件的文本框的宽度（以像素为单位） `CMFCRibbonEdit` 。

*lpszLabel*<br/>
中控件的标签 `CMFCRibbonEdit` 。

*n*<br/>
中用于控件的小图像的索引 `CMFCRibbonEdit` 。 小型图像的集合由父功能区类别维护。

### <a name="remarks"></a>备注

`CMFCRibbonEdit`控件不使用大图像。

## <a name="cmfcribboneditcopyfrom"></a><a name="copyfrom"></a> CMFCRibbonEdit：： CopyFrom

将指定 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象的状态复制到当前 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象。

```cpp
virtual void CopyFrom(const CMFCRibbonBaseElement& src);
```

### <a name="parameters"></a>参数

*src*<br/>
中源 `CMFCRibbonEdit` 对象。

### <a name="remarks"></a>备注

*Src* 参数的类型必须为 `CMFCRibbonEdit` 。

## <a name="cmfcribboneditcreateedit"></a><a name="createedit"></a> CMFCRibbonEdit::CreateEdit

为 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象创建一个新文本框。

```cpp
virtual CMFCRibbonRichEditCtrl* CreateEdit(
    CWnd* pWndParent,
    DWORD dwEditStyle);
```

### <a name="parameters"></a>参数

*pWndParent*<br/>
中指向对象的父窗口的指针 `CMFCRibbonEdit` 。

*dwEditStyle*<br/>
中指定文本框的样式。 可以将 "备注" 部分中列出的窗口样式与 Windows SDK 中描述的 [编辑控件样式](/windows/win32/Controls/edit-control-styles) 组合在一起。

### <a name="return-value"></a>返回值

如果方法成功，则为指向新文本框的指针;否则为 NULL。

### <a name="remarks"></a>备注

在派生类中重写此方法以创建自定义文本框。

可以将以下 [窗口样式](../../mfc/reference/styles-used-by-mfc.md#window-styles) 应用于文本框：

- **WS_CHILD**

- **WS_VISIBLE**

- **WS_DISABLED**

- **WS_GROUP**

- **WS_TABSTOP**

## <a name="cmfcribboneditdestroyctrl"></a><a name="destroyctrl"></a> CMFCRibbonEdit：:D estroyCtrl

销毁 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象。

```cpp
virtual void DestroyCtrl();
```

### <a name="remarks"></a>备注

## <a name="cmfcribboneditdropdownlist"></a><a name="dropdownlist"></a> CMFCRibbonEdit：:D ropDownList

下拉列表框。

```cpp
virtual void DropDownList();
```

### <a name="remarks"></a>备注

默认情况下，此方法不执行任何操作。 重写此方法以下拉列表框。

## <a name="cmfcribboneditenablespinbuttons"></a><a name="enablespinbuttons"></a> CMFCRibbonEdit::EnableSpinButtons

启用和设置文本框的数值调节钮的范围。

```cpp
void EnableSpinButtons(
    int nMin,
    int nMax);
```

### <a name="parameters"></a>参数

*nMin*<br/>
中数值调节钮的最小值。

*N 每天*<br/>
中数值调节钮的最大值。

### <a name="remarks"></a>备注

数值调节钮显示上箭头和下箭头，使用户能够通过一组固定的值进行移动。

## <a name="cmfcribboneditgetcompactsize"></a><a name="getcompactsize"></a> CMFCRibbonEdit：： GetCompactSize

检索 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象的压缩大小。

```cpp
virtual CSize GetCompactSize(CDC* pDC);
```

### <a name="parameters"></a>参数

*pDC*<br/>
中指向对象的设备上下文的指针 `CMFCRibbonEdit` 。

### <a name="return-value"></a>返回值

对象的压缩大小 `CMFCRibbonEdit` 。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditgetedittext"></a><a name="getedittext"></a> CMFCRibbonEdit::GetEditText

检索文本框中的文本。

```cpp
CString GetEditText() const;
```

### <a name="return-value"></a>返回值

文本框中的文本。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditgetintermediatesize"></a><a name="getintermediatesize"></a> CMFCRibbonEdit：： GetIntermediateSize

检索 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象的中间大小。

```cpp
virtual CSize GetIntermediateSize(CDC* pDC);
```

### <a name="parameters"></a>参数

*pDC*<br/>
中指向对象的设备上下文的指针 `CMFCRibbonEdit` 。

### <a name="return-value"></a>返回值

对象的中间大小 `CMFCRibbonEdit` 。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditgettextalign"></a><a name="gettextalign"></a> CMFCRibbonEdit::GetTextAlign

检索文本框中文本的对齐方式。

```cpp
int GetTextAlign() const;
```

### <a name="return-value"></a>返回值

文本对齐枚举值。 有关可能的值，请参阅备注部分。

### <a name="remarks"></a>备注

返回的值为以下编辑控件样式之一：

- 左对齐 **ES_LEFT**

- 居中对齐 **ES_CENTER**

- 右对齐 **ES_RIGHT**

有关这些样式的详细信息，请参阅 [编辑控件样式](/windows/win32/Controls/edit-control-styles)。

## <a name="cmfcribboneditgetwidth"></a><a name="getwidth"></a> CMFCRibbonEdit::GetWidth

检索 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的宽度（以像素为单位）。

```cpp
int GetWidth(BOOL bInFloatyMode = FALSE) const;
```

### <a name="parameters"></a>参数

*bInFloatyMode*<br/>
中如果控件处于 `CMFCRibbonEdit` 浮动模式，则为 TRUE; 否则为 FALSE。

### <a name="return-value"></a>返回值

控件的宽度（以像素为单位） `CMFCRibbonEdit` 。

### <a name="remarks"></a>备注

## <a name="cmfcribbonedithascompactmode"></a><a name="hascompactmode"></a> CMFCRibbonEdit::HasCompactMode

指示 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的显示大小是否可以压缩。

```cpp
virtual BOOL HasCompactMode() const;
```

### <a name="return-value"></a>返回值

始终返回 TRUE。

### <a name="remarks"></a>备注

默认情况下，此方法始终返回 TRUE。 重写此方法以指示显示大小是否可以压缩。

## <a name="cmfcribbonedithasfocus"></a><a name="hasfocus"></a> CMFCRibbonEdit::HasFocus

指示 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件是否有焦点。

```cpp
virtual BOOL HasFocus() const;
```

### <a name="return-value"></a>返回值

如果控件具有焦点，则为 TRUE `CMFCRibbonEdit` ; 否则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribbonedithaslargemode"></a><a name="haslargemode"></a> CMFCRibbonEdit::HasLargeMode

指示 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的显示大小是否可能很大。

```cpp
virtual BOOL HasLargeMode() const;
```

### <a name="return-value"></a>返回值

始终返回 FALSE。

### <a name="remarks"></a>备注

默认情况下，此方法始终返回 FALSE。 重写此方法以指示显示大小是否可以很大。

## <a name="cmfcribbonedithasspinbuttons"></a><a name="hasspinbuttons"></a> CMFCRibbonEdit::HasSpinButtons

指示文本框是否有数值调节钮。

```cpp
virtual BOOL HasSpinButtons() const;
```

### <a name="return-value"></a>返回值

如果文本框具有数值调节钮，则为 TRUE;否则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditishighlighted"></a><a name="ishighlighted"></a> CMFCRibbonEdit::IsHighlighted

指示是否突出显示 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件。

```cpp
virtual BOOL IsHighlighted() const;
```

### <a name="return-value"></a>返回值

如果突出显示该控件，则为 TRUE `CMFCRibbonEdit` ; 否则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonafterchangerect"></a><a name="onafterchangerect"></a> CMFCRibbonEdit::OnAfterChangeRect

当 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的显示矩形的尺寸更改时由框架调用。

```cpp
virtual void OnAfterChangeRect(CDC* pDC);
```

### <a name="parameters"></a>参数

*pDC*<br/>
中指向控件的设备上下文的指针 `CMFCRibbonEdit` 。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditondraw"></a><a name="ondraw"></a> CMFCRibbonEdit：： OnDraw

由框架调用，用于绘制 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件。

```cpp
virtual void OnDraw(CDC* pDC);
```

### <a name="parameters"></a>参数

*pDC*<br/>
中指向控件的设备上下文的指针 `CMFCRibbonEdit` 。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditondrawlabelandimage"></a><a name="ondrawlabelandimage"></a> CMFCRibbonEdit::OnDrawLabelAndImage

由框架调用，用于绘制 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的标签和图像。

```cpp
virtual void OnDrawLabelAndImage(CDC* pDC);
```

### <a name="parameters"></a>参数

*pDC*<br/>
中指向控件的设备上下文的指针 `CMFCRibbonEdit` 。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditondrawonlist"></a><a name="ondrawonlist"></a> CMFCRibbonEdit::OnDrawOnList

由框架调用，用于在命令列表框中绘制 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件。

```cpp
virtual void OnDrawOnList(
    CDC* pDC,
    CString strText,
    int nTextOffset,
    CRect rect,
    BOOL bIsSelected,
    BOOL bHighlighted);
```

### <a name="parameters"></a>参数

*pDC*<br/>
中指向控件的设备上下文的指针 `CMFCRibbonEdit` 。

*strText*<br/>
中显示文本。

*nTextOffset*<br/>
中从列表框左侧到显示文本的距离（以像素为单位）。

*rect*<br/>
中控件的显示矩形 `CMFCRibbonEdit` 。

*bIsSelected*<br/>
中未使用此参数。

*bHighlighted*<br/>
中未使用此参数。

### <a name="remarks"></a>备注

"命令" 列表框显示功能区控件，使用户可以自定义快速访问工具栏。

## <a name="cmfcribboneditonenable"></a><a name="onenable"></a> CMFCRibbonEdit::OnEnable

由框架调用，用于启用或禁用 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件。

```cpp
virtual void OnEnable(BOOL bEnable);
```

### <a name="parameters"></a>参数

*bEnable*<br/>
中如果启用控件，则为 TRUE;若要禁用控件，则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonhighlight"></a><a name="onhighlight"></a> CMFCRibbonEdit::OnHighlight

当指针进入或离开 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的边界时由框架调用。

```cpp
virtual void OnHighlight(BOOL bHighlight);
```

### <a name="parameters"></a>参数

*bHighlight*<br/>
中如果指针在控件的边界内，则为 TRUE `CMFCRibbonEdit` ; 否则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonkey"></a><a name="onkey"></a> CMFCRibbonEdit::OnKey

当用户按下 keytip 并且 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件具有焦点时由框架调用。

```cpp
virtual BOOL OnKey(BOOL bIsMenuKey);
```

### <a name="parameters"></a>参数

*bIsMenuKey*<br/>
中如果 keytip 显示一个弹出菜单，则为 TRUE;否则为 FALSE。

### <a name="return-value"></a>返回值

如果已处理事件，则为 TRUE;否则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonlbuttondown"></a><a name="onlbuttondown"></a> CMFCRibbonEdit::OnLButtonDown

当用户在控件上按下鼠标左键时更新 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件时，由框架调用。

```cpp
virtual void OnLButtonDown(CPoint point);
```

### <a name="parameters"></a>参数

*情况*<br/>
中未使用此参数。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonlbuttonup"></a><a name="onlbuttonup"></a> CMFCRibbonEdit::OnLButtonUp

当用户释放鼠标左键时由框架调用。

```cpp
virtual void OnLButtonUp(CPoint point);
```

### <a name="parameters"></a>参数

*情况*<br/>
中未使用此参数。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonrtlchanged"></a><a name="onrtlchanged"></a> CMFCRibbonEdit::OnRTLChanged

当布局更改方向时，由框架调用以更新 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件。

```cpp
virtual void OnRTLChanged(BOOL bIsRTL);
```

### <a name="parameters"></a>参数

*bIsRTL*<br/>
中如果布局为从右向左，则为 TRUE; 否则为。如果从左到右布局布局，则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditonshow"></a><a name="onshow"></a> CMFCRibbonEdit::OnShow

由框架调用以显示或隐藏 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件。

```cpp
virtual void OnShow(BOOL bShow);
```

### <a name="parameters"></a>参数

*bShow*<br/>
中如果显示控件，则为 TRUE;如果隐藏控件，则为 FALSE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditredraw"></a><a name="redraw"></a> CMFCRibbonEdit：：重绘

更新 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的显示。

```cpp
virtual void Redraw();
```

### <a name="remarks"></a>备注

此方法 `CMFCRibbonEdit` 通过使用 RDW_INVALIDATE、RDW_ERASE 和 RDW_UPDATENOW 标志集间接调用 [CWnd：： RedrawWindow](/windows/win32/api/winuser/nf-winuser-redrawwindow) 来重绘对象的显示矩形。

## <a name="cmfcribboneditsetaccdata"></a><a name="setaccdata"></a> CMFCRibbonEdit：： SetACCData

设置 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 对象的可访问性数据。

```cpp
virtual BOOL SetACCData(
    CWnd* pParent,
    CAccessibilityData& data);
```

### <a name="parameters"></a>参数

*pParent*<br/>
指向对象的父窗口的指针 `CMFCRibbonEdit` 。

*data*<br/>
对象的辅助功能数据 `CMFCRibbonEdit` 。

### <a name="return-value"></a>返回值

始终返回 TRUE。

### <a name="remarks"></a>备注

## <a name="cmfcribboneditsetedittext"></a><a name="setedittext"></a> CMFCRibbonEdit::SetEditText

设置文本框中的文本。

```cpp
void SetEditText(CString strText);
```

### <a name="parameters"></a>参数

*strText*<br/>
中文本框的文本。

## <a name="cmfcribboneditsettextalign"></a><a name="settextalign"></a> CMFCRibbonEdit::SetTextAlign

设置文本框的文本对齐方式。

```cpp
void SetTextAlign(int nAlign);
```

### <a name="parameters"></a>参数

*nAlign*<br/>
中文本对齐枚举值。 有关可能的值，请参阅备注部分。

### <a name="remarks"></a>备注

参数 *nAlign* 是以下编辑控件样式之一：

- 左对齐 ES_LEFT

- 居中对齐 ES_CENTER

- 右对齐 ES_RIGHT

有关这些样式的详细信息，请参阅 [编辑控件样式](/windows/win32/Controls/edit-control-styles)。

## <a name="cmfcribboneditsetwidth"></a><a name="setwidth"></a> CMFCRibbonEdit::SetWidth

设置 [CMFCRibbonEdit](../../mfc/reference/cmfcribbonedit-class.md) 控件的文本框宽度。

```cpp
void SetWidth(
    int nWidth,
    BOOL bInFloatyMode = FALSE);
```

### <a name="parameters"></a>参数

*nWidth*<br/>
中文本框的宽度（以像素为单位）。

*bInFloatyMode*<br/>
若要设置浮动模式的宽度，则为 TRUE;若为 FALSE，则设置常规模式的宽度。

### <a name="remarks"></a>备注

`CMFCRibbonEdit`控件有两个宽度，具体取决于其显示模式：浮动模式和常规模式。

## <a name="see-also"></a>请参阅

[层次结构图](../../mfc/hierarchy-chart.md)<br/>
[类](../../mfc/reference/mfc-classes.md)<br/>
[CMFCRibbonButton 类](../../mfc/reference/cmfcribbonbutton-class.md)<br/>
[CMFCRibbonBar 类](../../mfc/reference/cmfcribbonbar-class.md)
