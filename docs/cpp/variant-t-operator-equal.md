---
description: 了解详细信息： _variant_t：： operator =
title: _variant_t：： operator =
ms.date: 03/23/2021
f1_keywords:
- _variant_t::operator=
helpviewer_keywords:
- operator= [C++], variant
- operator = [C++], variant
- = operator [C++], with specific Visual C++ objects
ms.openlocfilehash: c1cd581b386a3cf5d551fe8341ba588fc56b9c2e
ms.sourcegitcommit: bb35a6c22d896c4640cff00a7321442c544ca219
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105744759"
---
# `_variant_t::operator=`

将新值分配给 `_variant_t` 实例。

`_variant_t`类及其 `operator=` 成员是 **Microsoft 特定** 的。

## <a name="syntax"></a>语法

```cpp
_variant_t& operator=( const VARIANT& varSrc );
_variant_t& operator=( const VARIANT* pVarSrc );
_variant_t& operator=( const _variant_t& var_t_Src );
_variant_t& operator=( short sSrc );
_variant_t& operator=( long lSrc );
_variant_t& operator=( float fltSrc );
_variant_t& operator=( double dblSrc );
_variant_t& operator=( const CY& cySrc );
_variant_t& operator=( const _bstr_t& bstrSrc );
_variant_t& operator=( const wchar_t* wstrSrc );
_variant_t& operator=( const char* strSrc );
_variant_t& operator=( IDispatch* pDispSrc );
_variant_t& operator=( bool bSrc );
_variant_t& operator=( IUnknown* pSrc );
_variant_t& operator=( const DECIMAL& decSrc );
_variant_t& operator=( BYTE byteSrc );
_variant_t& operator=( char cSrc );
_variant_t& operator=( unsigned short usSrc );
_variant_t& operator=( unsigned long ulSrc );
_variant_t& operator=( int iSrc );
_variant_t& operator=( unsigned int uiSrc );
_variant_t& operator=( __int64 i8Src );
_variant_t& operator=( unsigned __int64 ui8Src );
```

### <a name="parameters"></a>parameters

*`varSrc`*\
对要 `VARIANT` 从中复制内容和类型的的引用 `VT_*` 。

*`pVarSrc`*\
指向 `VARIANT` 要从中复制内容和类型的的指针 `VT_*` 。

*`var_t_Src`*\
对要 `_variant_t` 从中复制内容和类型的的引用 `VT_*` 。

*`sSrc`*\
**`short`** 要复制的整数值。 `VT_BOOL`如果 `*this` 的类型为，则为给定类型 `VT_BOOL` 。 否则，将为其给定的类型 `VT_I2` 。

*`lSrc`*\
**`long`** 要复制的整数值。 `VT_BOOL`如果 `*this` 的类型为，则为给定类型 `VT_BOOL` 。 `VT_ERROR`如果 `*this` 的类型为，则为给定类型 `VT_ERROR` 。 否则为给定类型 `VT_I4` 。

*`fltSrc`*\
**`float`** 要复制的数值。 给定 `VT_R4` 的类型。

*`dblSrc`*\
**`double`** 要复制的数值。 `VT_DATE`如果 `this` 的类型为，则为给定类型 `VT_DATE` 。 否则为给定类型 `VT_R8` 。

*`cySrc`*\
要复制的 `CY` 对象。 给定 `VT_CY` 的类型。

*`bstrSrc`*\
要复制的 `BSTR` 对象。 给定 `VT_BSTR` 的类型。

*`wstrSrc`*\
要复制的 Unicode 字符串，存储为 `BSTR` 和给定类型 `VT_BSTR` 。

*`strSrc`*\
要复制的多字节字符串，存储为 `BSTR` 和给定类型 `VT_BSTR` 。

*`pDispSrc`*\
`IDispatch`要使用对的调用进行复制的指针 `AddRef` 。 给定 `VT_DISPATCH` 的类型。

*`bSrc`*\
**`bool`** 要复制的值。 给定 `VT_BOOL` 的类型。

*`pSrc`*\
`IUnknown`要使用对的调用进行复制的指针 `AddRef` 。 给定  `VT_UNKNOWN` 的类型。

*`decSrc`*\
要复制的 `DECIMAL` 对象。 给定 `VT_DECIMAL` 的类型。

*`byteSrc`*\
`BYTE`要复制的值。 给定 `VT_UI1` 的类型。

*`cSrc`*\
**`char`** 要复制的值。 给定 `VT_I1` 的类型。

*`usSrc`*\
**`unsigned short`** 要复制的值。 给定 `VT_UI2` 的类型。

*`ulSrc`*\
**`unsigned long`** 要复制的值。 给定 `VT_UI4` 的类型。

*`iSrc`*\
**`int`** 要复制的值。 给定 `VT_INT` 的类型。

*`uiSrc`*\
**`unsigned int`** 要复制的值。 给定 `VT_UINT` 的类型。

*`i8Src`*\
**`__int64`** **`long long`** 要复制的或值。 给定 `VT_I8` 的类型。

*`ui8Src`*\
**`unsigned __int64`** **`unsigned long long`** 要复制的或值。 给定 `VT_UI8` 的类型。

## <a name="remarks"></a>备注

`operator=`赋值运算符将清除任何删除对象类型或调用和类型的现有值 `Release` `IDispatch*` `IUnknown*` 。 然后，它将新值复制到 `_variant_t` 对象中。 它会将 `_variant_t` 类型更改为与指定的值相匹配，但对于 **`short`** 、 **`long`** 和 **`double`** 参数没有说明。 直接复制值类型。 `VARIANT`或 `_variant_t` 指针或引用参数复制已分配对象的内容和类型。 其他指针或引用类型参数创建已分配对象的副本。 赋值运算符对 `AddRef` `IDispatch*` 和参数调用 `IUnknown*` 。

`operator=`[`_com_raise_error`](../cpp/com-raise-error.md)如果发生错误，则调用。

`operator=` 返回对已更新的对象的引用 `_variant_t` 。

## <a name="see-also"></a>另请参阅

[`_variant_t` 类](../cpp/variant-t-class.md)
