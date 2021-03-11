---
description: 了解详细信息： _vcprintf_s、_vcprintf_s_l、_vcwprintf_s、_vcwprintf_s_l
title: _vcprintf_s、_vcprintf_s_l、_vcwprintf_s、_vcwprintf_s_l
ms.date: 3/9/2021
api_name:
- _vcprintf_s
- _vcprintf_s_l
- _vcwprintf_s
- _vcwprintf_s_l
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
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- vcprintf_s
- vcwprintf_s_l
- _vcwprintf_s
- _vcwprintf_s_l
- _vcprintf_s_l
- _vtcprintf_s
- vcwprintf_s
- vcprintf_s_l
- _vcprintf_s
helpviewer_keywords:
- _vtcprintf_s_l function
- _vcwprintf_s_l function
- _vtcprintf_s function
- vtcprintf_s_l function
- vcprintf_s_l function
- _vcprintf_s function
- _vcwprintf_s function
- vcwprintf_s_l function
- vcwprintf_s function
- vcprintf_s function
- _vcprintf_s_l function
- vtcprintf_s function
- formatted text [C++]
ms.openlocfilehash: 1b427d0e56d6d1866f72d101e3f7c2c3562d3d42
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102621487"
---
# <a name="_vcprintf_s-_vcprintf_s_l-_vcwprintf_s-_vcwprintf_s_l"></a>_vcprintf_s、_vcprintf_s_l、_vcwprintf_s、_vcwprintf_s_l

使用指向参数列表的指针编写格式化输出到控制台。 这些版本的 [_vcprintf、_vcprintf_l、_vcwprintf、_vcwprintf_l](vcprintf-vcprintf-l-vcwprintf-vcwprintf-l.md) 具有安全增强功能，如 [CRT 中的安全功能](../../c-runtime-library/security-features-in-the-crt.md)所述。

> [!IMPORTANT]
> 此 API 不能用于在 Windows 运行时中执行的应用程序。 有关详细信息，请参阅[通用 Windows 平台应用中不支持的 CRT 函数](../../cppcx/crt-functions-not-supported-in-universal-windows-platform-apps.md)。

## <a name="syntax"></a>语法

```C
int _vcprintf(
   const char* format,
   va_list argptr
);
int _vcprintf(
   const char* format,
   locale_t locale,
   va_list argptr
);
int _vcwprintf_s(
   const wchar_t* format,
   va_list argptr
);
int _vcwprintf_s_l(
   const wchar_t* format,
   locale_t locale,
   va_list argptr
);
```

### <a name="parameters"></a>参数

*format*<br/>
格式规范。

*argptr*<br/>
指向参数列表的指针。

*locale*<br/>
要使用的区域设置。

有关详细信息，请参阅[格式规范语法：printf 和 wprintf 函数](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)。

## <a name="return-value"></a>返回值

写入的字符数，如果发生输出错误，则为一个负值。

与这些函数的不安全版本一样，如果 *format* 为空指针，则将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 此外，与这些函数的不太安全版本不同的是，如果 *格式* 不指定有效的格式，则会生成无效的参数异常。 如果允许执行继续，则这些函数将返回错误代码并将 **errno** 设置为该错误代码。 如果更具体的值不适用，则默认错误代码为 **EINVAL** 。

## <a name="remarks"></a>注解

每个函数均采用一个指向参数列表的指针，然后将给定数据格式化并写入到控制台。 **_vcwprintf_s** 是 **_vcprintf_s** 的宽字符版本。 它将采用一个宽字符字符串作为参数。

这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前区域设置。

> [!IMPORTANT]
> 确保 format 不是用户定义的字符串。 有关详细信息，请参阅 [避免缓冲区溢出](/windows/win32/SecBP/avoiding-buffer-overruns)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_vtcprintf_s**|**_vcprintf_s**|**_vcprintf_s**|**_vcwprintf_s**|
|**_vtcprintf_s_l**|**_vcprintf_s_l**|**_vcprintf_s_l**|**_vcwprintf_s_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|----------------------|
|**_vcprintf_s**， **_vcprintf_s_l**|\<conio.h> 和 \<stdarg.h>|\<varargs.h>*|
|**_vcwprintf_s**， **_vcwprintf_s_l**|\<conio.h> 或 \<wchar.h> 、和 \<stdarg.h>|\<varargs.h>*|

\* 仅对 UNIX V 兼容性是必需的。

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

> [!IMPORTANT]
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用链接 [`legacy_stdio_float_rounding.obj`](../link-options.md) 。

## <a name="example"></a>示例

```cpp
// crt_vcprintf_s.cpp
#include <conio.h>
#include <stdarg.h>

// An error formatting function used to print to the console.
int eprintf_s(const char* format, ...)
{
    va_list args;
    va_start(args, format);
    int result = _vcprintf_s(format, args);
    va_end(args);
    return result;
}

int main()
{
    eprintf_s("(%d:%d): Error %s%d : %s\n", 10, 23, "C", 2111,
              "<some error text>");
    eprintf_s("    (Related to symbol '%s' defined on line %d).\n",
              "<symbol>", 5 );
}
```

```Output
(10,23): Error C2111 : <some error text>
    (Related to symbol '<symbol>' defined on line 5).
```

## <a name="see-also"></a>另请参阅

[流 I/O](../../c-runtime-library/stream-i-o.md)<br/>
[vprintf 函数](../../c-runtime-library/vprintf-functions.md)<br/>
[_cprintf、_cprintf_l、_cwprintf、_cwprintf_l](cprintf-cprintf-l-cwprintf-cwprintf-l.md)<br/>
[fprintf、_fprintf_l、fwprintf、_fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md)<br/>
[printf、_printf_l、wprintf、_wprintf_l](printf-printf-l-wprintf-wprintf-l.md)<br/>
[sprintf、_sprintf_l、swprintf、_swprintf_l、 \_ _swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)<br/>
[va_arg、va_copy、va_end、va_start](va-arg-va-copy-va-end-va-start.md)<br/>
