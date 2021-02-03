---
description: 了解详细信息： _bstr_t：： wchar_t *、_bstr_t：： char*
title: _bstr_t::wchar_t *、_bstr_t::char*
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::wchar_t*
- _bstr_t::char*
helpviewer_keywords:
- operator wchar_t* [C++]
- operator char* [C++]
ms.openlocfilehash: b2e98ea4b62d4a2e346b552d31d66d23f301817c
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522542"
---
# <a name="_bstr_twchar_t-_bstr_tchar"></a>`_bstr_t::wchar_t*`, `_bstr_t::char*`

**Microsoft 专用**

将 `BSTR` 字符作为窄或宽字符数组返回。

## <a name="syntax"></a>语法

```cpp
operator const wchar_t*( ) const throw( );
operator wchar_t*( ) const throw( );
operator const char*( ) const;
operator char*( ) const;
```

## <a name="remarks"></a>备注

这些运算符可用于提取由对象封装的字符数据 `BSTR` 。 将新值分配给返回的指针不会修改原始 `BSTR` 数据。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
