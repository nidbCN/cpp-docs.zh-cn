---
description: 了解详细信息： _bstr_t：： operator + =、_bstr_t：： operator +
title: _bstr_t：： operator + =，_bstr_t：： operator +
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::operator+
- _bstr_t::operator+=
helpviewer_keywords:
- += operator [C++], appending strings
- + operator [C++], _bstr_t objects
ms.openlocfilehash: 44a525891ee072ea797026bd022ecae7b62fd6d1
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522516"
---
# <a name="_bstr_toperator--_bstr_toperator-"></a>`_bstr_t::operator +=`, `_bstr_t::operator +`

**Microsoft 专用**

将字符追加到对象的末尾 `_bstr_t` ，或连接两个字符串。

## <a name="syntax"></a>语法

```cpp
_bstr_t& operator+=( const _bstr_t& s1 );
_bstr_t operator+( const _bstr_t& s1 );
friend _bstr_t operator+( const char* s2, const _bstr_t& s1);
friend _bstr_t operator+( const wchar_t* s3, const _bstr_t& s1);
```

### <a name="parameters"></a>参数

*`s1`*\
`_bstr_t` 对象。

*`s2`*\
多字节字符串。

*`s3`*\
一个 Unicode 字符串。

## <a name="remarks"></a>备注

以下运算符将执行字符串串联：

- `operator+=( s1 )` 将封装的中的字符 `BSTR` 追加 *`s1`* 到该对象的封装末尾 `BSTR` 。

- `operator+( s1 )` 返回通过连接 `_bstr_t` 此对象的和生成的新 `BSTR` *`s1`* 。

- `operator+( s2, s1 )` 返回一个新 `_bstr_t` 的，它通过将多字节字符串连接 *`s2`* 、转换为 Unicode 和 `BSTR` 封装在中而形成 *`s1`* 。

- `operator+( s3, s1 )` 返回一个新 `_bstr_t` 的，它通过将 Unicode 字符串 *`s3`* 与 `BSTR` 封装在中而形成 *`s1`* 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
