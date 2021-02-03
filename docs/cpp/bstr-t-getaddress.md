---
description: 了解详细信息： _bstr_t：： GetAddress
title: _bstr_t::GetAddress
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::GetAddress
helpviewer_keywords:
- GetAddress method [C++]
ms.openlocfilehash: 23013a6666b8e268a6437532b69050933ffe6b42
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522828"
---
# `_bstr_t::GetAddress`

**Microsoft 专用**

释放所有现有字符串并返回一个新分配的字符串的地址。

## <a name="syntax"></a>语法

```cpp
BSTR* GetAddress( );
```

## <a name="return-value"></a>返回值

指向由 `BSTR` 包装的 `_bstr_t` 的指针。

## <a name="remarks"></a>备注

**`GetAddress`** 影响 `_bstr_t` 共享的所有对象 `BSTR` 。 多个 `_bstr_t` 可 `BSTR` 通过使用复制构造函数和来共享 **`operator=`** 。

## <a name="example"></a>示例

[`_bstr_t::Assign`](../cpp/bstr-t-assign.md)有关使用的示例，请参阅 **`GetAddress`** 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)