---
description: 了解详细信息： _bstr_t：： GetBSTR
title: _bstr_t::GetBSTR
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::GetBSTR
helpviewer_keywords:
- GetBSTR method [C++]
ms.openlocfilehash: b48dd082b21c0f3416c8b58b8ae2669c74d9d227
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522750"
---
# `_bstr_t::GetBSTR`

**Microsoft 专用**

指向 `BSTR` 包装的 `_bstr_t` 的开头。

## <a name="syntax"></a>语法

```cpp
BSTR& GetBSTR( );
```

## <a name="return-value"></a>返回值

`BSTR` 包装的 `_bstr_t` 的开头。

## <a name="remarks"></a>备注

**`GetBSTR`** 影响 `_bstr_t` 共享的所有对象 `BSTR` 。 多个 `_bstr_t` 可 `BSTR` 通过使用复制构造函数和来共享 `operator=` 。

## <a name="example"></a>示例

[`_bstr_t::Assign`](../cpp/bstr-t-assign.md)有关使用的示例，请参阅 **`GetBSTR`** 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)