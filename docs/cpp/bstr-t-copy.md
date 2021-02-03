---
description: 了解详细信息： _bstr_t：： copy
title: _bstr_t::copy
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::copy
helpviewer_keywords:
- Copy method [C++]
- BSTR object [C++], copy
ms.openlocfilehash: 98726e0c3100851d1496e532310ece2209d71ae0
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522854"
---
# `_bstr_t::copy`

**Microsoft 专用**

构造封装的 `BSTR` 的副本。

## <a name="syntax"></a>语法

```cpp
BSTR copy( bool fCopy = true ) const;
```

### <a name="parameters"></a>参数

*`fCopy`*\
如果为 **`true`** ，则 **`copy`** 返回包含的的副本 `BSTR` ，否则 **`copy`** 返回实际的 `BSTR` 。

## <a name="remarks"></a>备注

返回封装对象的新分配副本 `BSTR` ，或返回封装的对象本身，具体取决于参数。

## <a name="example"></a>示例

```cpp
STDMETHODIMP CAlertMsg::get_ConnectionStr(BSTR *pVal){ //  m_bsConStr is _bstr_t
   *pVal = m_bsConStr.copy();
}
```

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
