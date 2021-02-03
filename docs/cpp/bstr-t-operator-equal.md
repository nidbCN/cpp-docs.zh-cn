---
description: 了解详细信息： _bstr_t：： operator =
title: _bstr_t::operator =
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::operator=
helpviewer_keywords:
- operator = [C++], bstr
- operator= [C++], bstr
ms.openlocfilehash: d021ba013190ddee262b8644e16876401be846dc
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522711"
---
# `_bstr_t::operator =`

**Microsoft 专用**

将新值赋给现有 `_bstr_t` 对象。

## <a name="syntax"></a>语法

```cpp
_bstr_t& operator=(const _bstr_t& s1) throw ( );
_bstr_t& operator=(const char* s2);
_bstr_t& operator=(const wchar_t* s3);
_bstr_t& operator=(const _variant_t& var);
```

### <a name="parameters"></a>参数

*`s1`*\
将分配给现有 `_bstr_t` 对象的 `_bstr_t` 对象。

*`s2`*\
将分配给现有 `_bstr_t` 对象的多字节字符串。

*`s3`*\
将分配给现有 `_bstr_t` 对象的 Unicode 字符串。

*`var`*\
将分配给现有 `_variant_t` 对象的 `_bstr_t` 对象。

**结束 Microsoft 专用**

## <a name="example"></a>示例

[`_bstr_t::Assign`](../cpp/bstr-t-assign.md)有关使用的示例，请参阅 **`operator=`** 。

## <a name="see-also"></a>另请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
