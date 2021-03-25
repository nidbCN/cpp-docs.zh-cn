---
description: 了解详细信息： _variant_t：： operator =
title: _variant_t::operator =
ms.date: 11/04/2016
f1_keywords:
- _variant_t::operator=
helpviewer_keywords:
- operator= [C++], variant
- operator = [C++], variant
- = operator [C++], with specific Visual C++ objects
ms.assetid: 77622723-6e49-4dec-9e0f-fa74028f1a3c
ms.openlocfilehash: 3218d15f5b787709f1759bfda3a2501bcdda5406
ms.sourcegitcommit: d775eac25552bd0fb7276703efa6931d6a1a59fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105099679"
---
# <a name="_variant_toperator-"></a>_variant_t::operator =

**Microsoft 专用**

## <a name="syntax"></a>语法

```
_variant_t& operator=(
   const VARIANT& varSrc
);

_variant_t& operator=(
   const VARIANT* pVarSrc
);

_variant_t& operator=(
   const _variant_t& var_t_Src
);

_variant_t& operator=(
   short sSrc
);

_variant_t& operator=(
   long lSrc
);

_variant_t& operator=(
   float fltSrc
);

_variant_t& operator=(
   double dblSrc
);

_variant_t& operator=(
   const CY& cySrc
);

_variant_t& operator=(
   const _bstr_t& bstrSrc
);

_variant_t& operator=(
   const wchar_t* wstrSrc
);

_variant_t& operator=(
   const char* strSrc
);

_variant_t& operator=(
   IDispatch* pDispSrc
);

_variant_t& operator=(
   bool bSrc
);

_variant_t& operator=(
   IUnknown* pSrc
);

_variant_t& operator=(
   const DECIMAL& decSrc
);

_variant_t& operator=(
   BYTE bSrc
);

_variant_t& operator=(
   char cSrc
);

_variant_t& operator=(
   unsigned short usSrc
);

_variant_t& operator=(
   unsigned long ulSrc
);

_variant_t& operator=(
   int iSrc
);

_variant_t& operator=(
   unsigned int uiSrc
);

_variant_t& operator=(
   __int64 i8Src
);

_variant_t& operator=(
   unsigned __int64 ui8Src
);
```

## <a name="remarks"></a>备注

此运算符将向 `_variant_t` 对象赋予新值：

- **operator = (** *varSrc* **)** 将现有分配 `VARIANT` 给 `_variant_t` 对象。    

- **operator = (** *pVarSrc* **)** 将现有分配 `VARIANT` 给 `_variant_t` 对象。    

- **operator = (** *var_t_Src* **)** 将现有 `_variant_t` 对象赋给 `_variant_t` 对象。    

- **operator = (** *sSrc* **)** 将 **`short`** 整数值分配给 `_variant_t` 对象。    

- **operator = (** `lSrc`**)****`long`** 为对象分配一个整数值 `_variant_t` 。    

- **operator = (** *fltSrc* **)** 将 **`float`** 数值分配给 `_variant_t` 对象。    

- **operator = (** *dblSrc* **)** 将 **`double`** 数值分配给 `_variant_t` 对象。    

- **operator = (** *cySrc* **)** 将 `CY` 对象分配给 `_variant_t` 对象。    

- **operator = (** *bstrSrc* **)** 将 `BSTR` 对象分配给 `_variant_t` 对象。    

- **operator = (** *WstrSrc* **)** 将 Unicode 字符串分配给 `_variant_t` 对象。    

- **operator = (** `strSrc`**)** 将多字节字符串分配给 `_variant_t` 对象。    

- **operator = (** `bSrc`**)****`bool`** 为 `_variant_t` 对象赋值。  

- **operator = (** *pDispSrc* **)** 将 `IDispatch*` 对象分配给 `_variant_t` 对象并调用 `AddRef` 。    

- **operator = (** *pIUnknownSrc* **)** 将 `IUnknown*` 对象分配给 `_variant_t` 对象并调用 `AddRef` 。    

- **operator = (** *decSrc* **)** 将值赋 `DECIMAL` 给 `_variant_t` 对象。    

- **operator = (** `bSrc`**)**`BYTE`为 `_variant_t` 对象赋值。  

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[_variant_t 类](../cpp/variant-t-class.md)
