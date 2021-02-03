---
description: 了解详细信息： _bstr_t：： Attach
title: _bstr_t::Attach
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::Attach
helpviewer_keywords:
- Attach method [C++]
ms.openlocfilehash: 02717fad98e4d68dde70995abcfad4cb55b8c45c
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522880"
---
# `_bstr_t::Attach`

**Microsoft 专用**

将 `_bstr_t` 包装器链接到 `BSTR`。

## <a name="syntax"></a>语法

```cpp
void Attach(
   BSTR s
);
```

### <a name="parameters"></a>参数

*`s`*\
要与之关联或分配到的 `BSTR`，即 `_bstr_t` 变量。

## <a name="remarks"></a>备注

如果 `_bstr_t` 以前被附加到了另一个 `BSTR`，并且没有其他 `_bstr_t` 变量使用 `BSTR`，`_bstr_t` 将清理 `BSTR` 资源。

## <a name="example"></a>示例

[`_bstr_t::Assign`](../cpp/bstr-t-assign.md)有关使用的示例，请参阅 **`Attach`** 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
