---
description: 了解详细信息： _bstr_t：： operator！
title: _bstr_t：： operator！
ms.date: 02/02/2021
f1_keywords:
- _bstr_t::operator!
helpviewer_keywords:
- '! operator'
- operator!, bstr
- operator !, bstr
ms.openlocfilehash: 712aeafd26fda6c8291a54a9b897d31fa77ac02e
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522594"
---
# `_bstr_t::operator !`

**Microsoft 专用**

检查封装的 `BSTR` 是否为 NULL 字符串。

## <a name="syntax"></a>语法

```cpp
bool operator!( ) const throw( );
```

## <a name="return-value"></a>返回值

**`true`** 如果封装的 `BSTR` 为空字符串，则返回; 否则返回 **`false`** 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`_bstr_t` 班级](../cpp/bstr-t-class.md)
