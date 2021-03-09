---
description: 了解详细信息： no_sanitize_address
title: no_sanitize_address
ms.date: 11/04/2016
f1_keywords:
- no_sanitize_address__cpp
helpviewer_keywords:
- __declspec keyword [C++], no_sanitize_address
- no_sanitize_address __declspec keyword
ms.openlocfilehash: a18b60f666bc53ef72db3a6d230dc7531cc6bc1f
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470895"
---
# `no_sanitize_address`

**Microsoft 专用**

**`__declspec(no_sanitize_address)`** 说明符指示编译器禁用函数、局部变量或全局变量的地址 sanitizer。 此说明符与 [AddressSanitizer](../sanitizers/asan.md)结合使用。

> [!NOTE]
> **`__declspec(no_sanitize_address)`** 禁用 _编译器_ 行为，而不是 *运行时* 行为。

## <a name="example"></a>示例：

有关示例，请参阅 [AddressSanitizer build 引用](../sanitizers/asan-building.md#__declspecno_sanitize_address) 。

**结束 Microsoft 专用**

## <a name="see-also"></a>请参阅

[`__declspec`](../cpp/declspec.md)\
[字](../cpp/keywords-cpp.md)\
[AddressSanitizer](../sanitizers/asan.md)
