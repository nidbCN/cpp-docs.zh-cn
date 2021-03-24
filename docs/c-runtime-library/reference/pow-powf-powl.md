---
title: pow、powf、powl
description: 适用于 pow、powf 和 powl 的 API 参考;计算幂的幂。
ms.date: 08/31/2020
api_name:
- powl
- pow
- powf
- _o_pow
- _o_powf
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
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- powl
- pow
- _powl
- powf
helpviewer_keywords:
- exponential calculations
- powl function
- _powl function
- exponentiation
- powers, calculating
- calculating exponentials
- powf function
- pow function
ms.assetid: e75c33ed-2e59-48b1-be40-81da917324f1
ms.openlocfilehash: 0e05cd243c88f9f3862f6acaa517bee8e7b0bdb8
ms.sourcegitcommit: 977b5151e7dae7584112328bab515fb15622a6cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104883891"
---
# <a name="pow-powf-powl"></a>pow、powf、powl

计算 *x* 的 *y* 次幂。

## <a name="syntax"></a>语法

```C
double pow( double x, double y );
float powf( float x, float y );
long double powl( long double x, long double y );
define pow(X, Y) // Requires C11 or higher

double pow( double x, int y );  // C++ only
float pow( float x, float y );  // C++ only
float pow( float x, int y );  // C++ only
long double pow( long double x, long double y );  // C++ only
long double pow( long double x, int y );  // C++ only
```

### <a name="parameters"></a>参数

*x-blade*\
Base。

*误差*\
Exponent。

## <a name="return-value"></a>返回值

返回 *x*<sup>*y*</sup>的值。 在溢出或下溢时不输出错误消息。

|x 和 y 的值|pow 的返回值|
|-----------------------|-------------------------|
|*x* ！ = 0.0， *y* = = 0。0|1|
|*x* = = 0.0， *y* = = 0。0|1|
|*x* = = 0.0， *y* < 0|INF|

## <a name="remarks"></a>备注

**pow** 不识别大于 2 <sup>64</sup> 的整数浮点值 (例如，1.0 e100) 。

**pow** 具有使用流式处理 simd 扩展 2 (SSE2) 的实现。 有关使用 SSE2 实现的信息和限制，请参阅 [_set_SSE2_enable](set-sse2-enable.md)。

由于 c + + 允许重载，因此可以调用 **pow** 的任何不同重载。 在 C 程序中，除非使用 \<tgmath.h> 宏调用此函数，否则 **pow** 将始终采用两个 **`double`** 值，并返回一个 **`double`** 值。

如果使用 \<tgmath.h> `pow()` 宏，则参数的类型将决定选择哪个版本的函数。 有关详细信息，请参阅 [类型-泛型数学](../../c-runtime-library/tgmath.md) 。

`pow(int, int)` 将不再可用。 如果使用此重载，编译器可能会发出 [C2668](../../error-messages/compiler-errors-2/compiler-error-c2668.md)。 若要避免此问题，请将第一个参数强制转换为 **`double`** 、 **`float`** 或 **`long double`** 。

最初， `pow(T, int)` 重载会将调用展开为 `pow` 一系列内联乘法运算。 尽管速度更快，但在 Visual Studio 2015 Update 1 中删除的准确性也明显降低。 有关详细信息，请参阅 [Visual Studio 2015 Update 1 中的符合性改进](../../porting/visual-cpp-what-s-new-2003-through-2015.md)。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头 (C)|必需的标头 (C++)|
|-|-|-|
|**pow**、 **powf**、 **powl**|\<math.h>|\<math.h> 或 \<cmath>|
|**pow** 宏 | \<tgmath.h> ||

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_pow.c

#include <math.h>
#include <stdio.h>

int main( void )
{
   double x = 2.0, y = 3.0, z;

   z = pow( x, y );
   printf( "%.1f to the power of %.1f is %.1f\n", x, y, z );
}
```

```Output
2.0 to the power of 3.0 is 8.0
```

## <a name="see-also"></a>请参阅

[浮点支持](../../c-runtime-library/floating-point-support.md) <br/>
[`exp`, `expf`, `expl`](exp-expf.md) <br/>
[`log`, `logf`, `log10`, `log10f`](log-logf-log10-log10f.md) <br/>
[`sqrt`, `sqrtf`, `sqrtl`](sqrt-sqrtf-sqrtl.md) <br/>
[`_CIpow`](../../c-runtime-library/cipow.md)<br/>
