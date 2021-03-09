---
description: 了解详细信息： _set_SSE2_enable
title: _set_SSE2_enable
ms.date: 04/05/2018
api_name:
- _set_SSE2_enable
api_location:
- msvcrt.dll
- msvcr80.dll
- msvcr90.dll
- msvcr100.dll
- msvcr100_clr0400.dll
- msvcr110.dll
- msvcr110_clr0400.dll
- msvcr120.dll
- msvcr120_clr0400.dll
- ucrtbase.dll
- api-ms-win-crt-math-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _set_SSE2_enable
- set_SSE2_enable
helpviewer_keywords:
- _set_SSE2_enable function
- Streaming SIMD Extensions 2 instructions
- set_SSE2_enable function
ms.assetid: 55db895d-fc1e-475a-9110-b781a9bb51c5
ms.openlocfilehash: f5b7bc8c0d18423ac981275e08dceef85197af4e
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514507"
---
# <a name="_set_sse2_enable"></a>_set_SSE2_enable

启用或禁用在 CRT 数学例程中使用流式处理 SIMD 扩展 2 (SSE2) 指令。 （此函数在 x64 体系结构上不可用，因为默认情况下将启用 SSE2。）

## <a name="syntax"></a>语法

```C
int _set_SSE2_enable(
   int flag
);
```

### <a name="parameters"></a>参数

flag <br/>
1 表示启用 SSE2 实现；0 表示禁用 SSE2 实现。 默认情况下，在支持 SSE2 实现的处理器上启用 SSE2 实现。

## <a name="return-value"></a>返回值

如果启用 SSE2 实现，则值为非零；如果禁用 SSE2 实现，则值为零。

## <a name="remarks"></a>注解

以下函数具有可使用 **_set_SSE2_enable** 启用的 SSE2 实现：

- [atan](atan-atanf-atanl-atan2-atan2f-atan2l.md)

- [ceil](ceil-ceilf-ceill.md)

- [exp](exp-expf.md)

- [floor](floor-floorf-floorl.md)

- [日志](log-logf-log10-log10f.md)

- [log10](log-logf-log10-log10f.md)

- [modf](modf-modff-modfl.md)

- [pow](pow-powf-powl.md)

这些函数的 SSE2 实现可能会产生与默认实现稍微不同的答案，因为 SSE2 中间值为 64 位浮点数，而默认实现中间值为 80 位浮点数。

> [!NOTE]
> 如果使用 [/Oi (生成内部函数)](../../build/reference/oi-generate-intrinsic-functions.md) 编译器选项编译项目，则可能会显示 **_set_SSE2_enable** 不起作用。 **/Oi** 编译器选项为编译器提供使用内部函数的权限来替换 CRT 调用;此行为将覆盖 **_set_SSE2_enable** 的效果。 如果要确保 **/Oi** 不会重写 **_set_SSE2_enable**，请使用 **/Oi-** 来编译项目。 当你使用其他表示 **/Oi** 的编译器开关时，这也可能是一种很好的做法。

只有在屏蔽所有异常时才可使用 SSE2 实现。 使用 [_control87、_controlfp](control87-controlfp-control87-2.md) 来屏蔽异常。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|**_set_SSE2_enable**|\<math.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_set_SSE2_enable.c
// processor: x86
#include <math.h>
#include <stdio.h>

int main()
{
   int i = _set_SSE2_enable(1);

   if (i)
      printf("SSE2 enabled.\n");
   else
      printf("SSE2 not enabled; processor does not support SSE2.\n");
}
```

```Output
SSE2 enabled.
```

## <a name="see-also"></a>另请参阅

[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../../c-runtime-library/crt-library-features.md)
