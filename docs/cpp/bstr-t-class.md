---
title: _bstr_t 类
description: 了解详细信息： _bstr_t 类
ms.date: 02/02/2021
f1_keywords:
- _bstr_t
helpviewer_keywords:
- BSTR object
- _bstr_t class
- BSTR object [C++], COM encapsulation
ms.openlocfilehash: 71f5f0a27989b416cf4f13873254890db09ce334
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522867"
---
# <a name="_bstr_t-class"></a>`_bstr_t` 类

**Microsoft 专用**

`_bstr_t`对象封装[BSTR 数据类型](/previous-versions/windows/desktop/automat/bstr)。 类 [`SysAllocString`](/windows/win32/api/oleauto/nf-oleauto-sysallocstring) [`SysFreeString`](/windows/win32/api/oleauto/nf-oleauto-sysfreestring) 在适当时通过对和和其他 api 的函数调用来管理资源分配和释放 `BSTR` 。 `_bstr_t`类使用引用计数来避免开销过大。

## <a name="members"></a>成员

### <a name="construction"></a>建筑

| 构造函数 | 说明 |
|--|--|
| [`_bstr_t`](../cpp/bstr-t-bstr-t.md) | 构造 `_bstr_t` 对象。 |

### <a name="operations"></a>操作

| 功能 | 说明 |
|--|--|
| [`Assign`](../cpp/bstr-t-assign.md) | 将 `BSTR` 复制到 `BSTR` 包装的 `_bstr_t` 中。 |
| [`Attach`](../cpp/bstr-t-attach.md) | 将 `_bstr_t` 包装器链接到 `BSTR`。 |
| [`copy`](../cpp/bstr-t-copy.md) | 构造封装的 `BSTR` 的副本。 |
| [`Detach`](../cpp/bstr-t-detach.md) | 返回 `BSTR` 包装的 `_bstr_t` 并从 `BSTR` 中分离 `_bstr_t`。 |
| [`GetAddress`](../cpp/bstr-t-getaddress.md) | 指向 `BSTR` 包装的 `_bstr_t`。 |
| [`GetBSTR`](../cpp/bstr-t-getbstr.md) | 指向 `BSTR` 包装的 `_bstr_t` 的开头。 |
| [`length`](../cpp/bstr-t-length.md) | 返回 `_bstr_t` 中的字符数。 |

### <a name="operators"></a>运算符

| 操作员 | 说明 |
|--|--|
| [`operator =`](../cpp/bstr-t-operator-equal.md) | 将新值赋给现有 `_bstr_t` 对象。 |
| [`operator +=`](../cpp/bstr-t-operator-add-equal-plus.md) | 将字符附加到 `_bstr_t` 对象的结尾。 |
| [`operator +`](../cpp/bstr-t-operator-add-equal-plus.md) | 串联两个字符串。 |
| [`operator !`](../cpp/bstr-t-operator-logical-not.md) | 检查封装的 `BSTR` 是否为 NULL 字符串。 |
| [`operator ==`](../cpp/bstr-t-relational-operators.md)<br/>[`operator !=`](../cpp/bstr-t-relational-operators.md)<br/>[`operator <`](../cpp/bstr-t-relational-operators.md)<br/>[`operator >`](../cpp/bstr-t-relational-operators.md)<br/>[`operator <=`](../cpp/bstr-t-relational-operators.md)<br/>[`operator >=`](../cpp/bstr-t-relational-operators.md) | 比较两个 `_bstr_t` 对象。 |
| [`operator wchar_t*`](../cpp/bstr-t-wchar-t-star-bstr-t-char-star.md)<br/>[`operator char*`](../cpp/bstr-t-wchar-t-star-bstr-t-char-star.md)  | 提取指向封装的 Unicode 或多字节 `BSTR` 对象的指针。 |

**结束 Microsoft 专用**

## <a name="requirements"></a>要求

**标头：**\<comutil.h>

**Lib：** *`comsuppw.lib`* 或 *`comsuppwd.lib`* (有关详细信息，请参阅 [ `/Zc:wchar_t` (wchar_t 是本机类型)](../build/reference/zc-wchar-t-wchar-t-is-native-type.md)) 

## <a name="see-also"></a>另请参阅

[编译器 COM 支持类](../cpp/compiler-com-support-classes.md)
