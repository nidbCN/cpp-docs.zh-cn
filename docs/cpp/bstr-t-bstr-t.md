---
description: 了解详细信息： _bstr_t：： _bstr_t
title: _bstr_t::_bstr_t
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::_bstr_t
helpviewer_keywords:
- BSTR object
- _bstr_t method [C++]
- _bstr_t class
ms.openlocfilehash: 0e6913a45b1c4d542e495bc59bc5871f533a1573
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522906"
---
# `_bstr_t::_bstr_t`

**Microsoft 专用**

构造 `_bstr_t` 对象。

## <a name="syntax"></a>语法

```cpp
_bstr_t( ) throw( );
_bstr_t(
   const _bstr_t& s1
) throw( );
_bstr_t(
   const char* s2
);
_bstr_t(
   const wchar_t* s3
);
_bstr_t(
   const _variant_t& var
);
_bstr_t(
   BSTR bstr,
   bool fCopy
);
```

### <a name="parameters"></a>参数

*`s1`*\
要复制的 `_bstr_t` 对象。

*`s2`*\
多字节字符串。

*`s3`*\
Unicode 字符串

*`var`*\
一个 [_variant_t](../cpp/variant-t-class.md) 对象。

*`bstr`*\
一个现有的 `BSTR` 对象。

*`fCopy`*\
如果为 **`false`** ，则 *`bstr`* 通过调用将参数附加到新的对象，而不创建副本 `SysAllocString` 。

## <a name="remarks"></a>备注

`_bstr_t`类提供多个构造函数：

`_bstr_t( )`\
构造 `_bstr_t` 封装 null 对象的默认对象 `BSTR` 。

`_bstr_t( _bstr_t& s1 )`\
构造一个 `_bstr_t` 对象作为另一个对象的副本。 此构造函数将创建一个 *浅表* 复制，这会递增封装对象的引用计数， `BSTR` 而不是创建新的对象。

`_bstr_t( char* s2 )`\
通过调用 `_bstr_t` 来创建一个新的 `SysAllocString` 对象并封装该对象，从而构造一个 `BSTR` 对象。 此构造函数首先执行多字节到 Unicode 的转换。

`_bstr_t( wchar_t* s3 )`\
通过调用 `_bstr_t` 来创建一个新的 `SysAllocString` 对象并封装该对象，从而构造一个 `BSTR` 对象。

`_bstr_t( _variant_t& var )`\
`_bstr_t` `_variant_t` 通过先 `BSTR` 从封装的对象中检索对象，从对象构造对象 `VARIANT` 。

`_bstr_t( BSTR bstr, bool fCopy )`\
从现有 `_bstr_t` 构造一个 `BSTR` 对象（与 `wchar_t*` 字符串相对）。 如果 *`fCopy`* 为 **`false`** ，则将提供的 `BSTR` 附加到新的对象，而无需使用生成新的副本 `SysAllocString` 。 类型库标头中的包装函数使用此构造函数来封装和获取 `BSTR` 接口方法返回的的所有权。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)\
[`_variant_t` 班级](../cpp/variant-t-class.md)
