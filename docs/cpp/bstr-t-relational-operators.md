---
description: 了解详细信息： _bstr_t 关系运算符
title: _bstr_t 关系运算符
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::operator>
- _bstr_t::operator==
- _bstr_t::operator>=
- _bstr_t::operator!=
- _bstr_t::operator<
- _bstr_t::operator<=
helpviewer_keywords:
- _bstr_t relational operators[C++]
ms.openlocfilehash: eef66f8165fc1dfbb29730507ee8d3b996800b7b
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522581"
---
# <a name="_bstr_t-relational-operators"></a>`_bstr_t` 关系运算符

**Microsoft 专用**

比较两个 `_bstr_t` 对象。

## <a name="syntax"></a>语法

```cpp
bool operator==(const _bstr_t& str) const throw( );
bool operator!=(const _bstr_t& str) const throw( );
bool operator<(const _bstr_t& str) const throw( );
bool operator>(const _bstr_t& str) const throw( );
bool operator<=(const _bstr_t& str) const throw( );
bool operator>=(const _bstr_t& str) const throw( );
```

## <a name="remarks"></a>备注

这些运算符按字典顺序比较两个 `_bstr_t` 对象。 **`true`** 如果比较保持，则运算符返回; 否则返回 **`false`** 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
