---
description: 了解详细信息： vsprintf_s、_vsprintf_s_l、vswprintf_s、_vswprintf_s_l
title: vsprintf_s、_vsprintf_s_l、vswprintf_s、_vswprintf_s_l
ms.date: 3/9/2021
api_name:
- _vswprintf_s_l
- vsprintf_s
- vswprintf_s
- _vsprintf_s_l
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
- ntoskrnl.exe
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- vswprintf_s
- vsprintf_s
- _vstprintf_s
helpviewer_keywords:
- _vstprintf_s_l function
- vsprintf_s_l function
- _vstprintf_s function
- vswprintf_s function
- vstprintf_s function
- vstprintf_s_l function
- vswprintf_s_l function
- vsprintf_s function
- _vsprintf_s_l function
- formatted text [C++]
- _vswprintf_s_l function
ms.openlocfilehash: fe3b320f28f54824033782bab3416e7b4623f0d0
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622083"
---
# <a name="vsprintf_s-_vsprintf_s_l-vswprintf_s-_vswprintf_s_l"></a>vsprintf_s、_vsprintf_s_l、vswprintf_s、_vswprintf_s_l

使用指向参数列表的指针写入格式化的输出。 这些函数是版本的 [vsprintf、_vsprintf_l、vswprintf、_vswprintf_l、 \_ _vswprintf_l，](vsprintf-vsprintf-l-vswprintf-vswprintf-l-vswprintf-l.md) 具有安全增强功能，如 [CRT 中的安全功能](../../c-runtime-library/security-features-in-the-crt.md)中所述。

## <a name="syntax"></a>语法

```C
int vsprintf_s(
   char *buffer,
   size_t numberOfElements,
   const char *format,
   va_list argptr
);
int _vsprintf_s_l(
   char *buffer,
   size_t numberOfElements,
   const char *format,
   locale_t locale,
   va_list argptr
);
int vswprintf_s(
   wchar_t *buffer,
   size_t numberOfElements,
   const wchar_t *format,
   va_list argptr
);
int _vswprintf_s_l(
   wchar_t *buffer,
   size_t numberOfElements,
   const wchar_t *format,
   locale_t locale,
   va_list argptr
);
template <size_t size>
int vsprintf_s(
   char (&buffer)[size],
   const char *format,
   va_list argptr
); // C++ only
template <size_t size>
int vswprintf_s(
   wchar_t (&buffer)[size],
   const wchar_t *format,
   va_list argptr
); // C++ only
```

### <a name="parameters"></a>参数

*宽限*<br/>
输出的存储位置

*numberOfElements*<br/>
*缓冲区* 的大小（以字符为限）。

*format*<br/>
格式规范。

*argptr*<br/>
指向参数列表的指针。

*locale*<br/>
要使用的区域设置。

## <a name="return-value"></a>返回值

**vsprintf_s** 和 **vswprintf_s** 返回写入的字符数，不包括终止 null 字符，或者在出现输出错误时返回一个负值。 如果 *缓冲区* 或 *格式* 为 Null 指针，如果 *numberOfElements* 为零，或者格式字符串包含无效的格式字符，则将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则函数将返回-1，并将 **errno** 设置为 **EINVAL**。

有关这些代码及其他错误代码的信息，请参阅 [_doserrno、errno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

## <a name="remarks"></a>注解

其中每个函数都采用一个指向参数列表的指针，然后将给定数据格式化并写入 *缓冲区* 指向的内存。

**vswprintf_s** 符合 **Vswprintf** 的 ISO C 标准，该标准需要 **size_t** 类型的第二个参数 *count*。

这些函数与不安全版本的差异仅在于安全版本支持位置参数。 有关详细信息，请参阅 [printf_p 位置参数](../../c-runtime-library/printf-p-positional-parameters.md)。

这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前线程区域设置。

在 c + + 中，模板重载简化了这些函数的使用。 重载可以自动推断缓冲区长度，从而无需指定大小自变量。 而且，它们可以自动将不安全的函数替换为其安全的对应函数。 有关详细信息，请参阅[安全模板重载](../../c-runtime-library/secure-template-overloads.md)。

> [!IMPORTANT]
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_vstprintf_s**|**vsprintf_s**|**vsprintf_s**|**vswprintf_s**|
|**_vstprintf_s_l**|**_vsprintf_s_l**|**_vsprintf_s_l**|**_vswprintf_s_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|----------------------|
|**vsprintf_s**， **_vsprintf_s_l**|\<stdio.h> 和 \<stdarg.h>|\<varargs.h>*|
|**vswprintf_s**， **_vswprintf_s_l**|\<stdio.h> 或 \<wchar.h> 、和 \<stdarg.h>|\<varargs.h>*|

\* 仅对 UNIX V 兼容性是必需的。

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_vsprintf_s.c
// Compile with: cl /W4 crt_vsprintf_s.c
// This program uses vsprintf_s to write to a buffer.
// The size of the buffer is determined by _vscprintf.

#include <stdlib.h>
#include <stdio.h>
#include <stdarg.h>

void test( char const * const format, ... )
{
   va_list args;
   int len;
   char * buffer;

   va_start( args, format );
   len = _vscprintf( format, args ) // _vscprintf doesn't count
                               + 1; // terminating '\0'
   buffer = (char *) malloc( len * sizeof(char) );
   if ( NULL != buffer )
   {
      vsprintf_s( buffer, len, format, args );
      puts( buffer );
      free( buffer );
   }
   va_end( args );
}

int main( void )
{
   test( "%d %c %d", 123, '<', 456 );
   test( "%s", "This is a string" );
}
```

```Output
123 < 456
This is a string
```

## <a name="see-also"></a>另请参阅

[流 I/O](../../c-runtime-library/stream-i-o.md)<br/>
[vprintf 函数](../../c-runtime-library/vprintf-functions.md)<br/>
[格式规范语法： printf 和 wprintf 函数](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)<br/>
[fprintf、_fprintf_l、fwprintf、_fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md)<br/>
[printf、_printf_l、wprintf、_wprintf_l](printf-printf-l-wprintf-wprintf-l.md)<br/>
[sprintf、_sprintf_l、swprintf、_swprintf_l、 \_ _swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)<br/>
[va_arg、va_copy、va_end、va_start](va-arg-va-copy-va-end-va-start.md)
