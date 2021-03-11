---
description: 了解详细信息： vsprintf、_vsprintf_l、vswprintf、_vswprintf_l、__vswprintf_l
title: vsprintf、_vsprintf_l、vswprintf、_vswprintf_l、__vswprintf_l
ms.date: 3/9/2021
api_name:
- _vswprintf_l
- _vsprintf_l
- vsprintf
- vswprintf
- __vswprintf_l
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
- vstprintf
- vswprintf
- _vstprintf
- vsprintf
- __vswprintf_l
- _vsprintf_l
- _vswprintf_l
- vswprintf_l
helpviewer_keywords:
- __vswprintf_l function
- _vstprintf_l function
- formatted text
- vstprintf_l function
- _vswprintf_l function
- vsprintf_l function
- buffers, avoiding overruns
- buffer overruns
- vswprintf_l function
- buffers, buffer overruns
- vstprintf function
- _vsprintf_l function
- vswprintf function
- vsprintf function
- _vstprintf function
ms.openlocfilehash: 299df4fc02e0761d97e8b64d650af0953a866316
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102621745"
---
# <a name="vsprintf-_vsprintf_l-vswprintf-_vswprintf_l-__vswprintf_l"></a>vsprintf、_vsprintf_l、vswprintf、_vswprintf_l、__vswprintf_l

使用指向参数列表的指针写入格式化的输出。 提供这些函数的更多安全版本；请参阅 [vsprintf_s、_vsprintf_s_l、vswprintf_s、_vswprintf_s_l](vsprintf-s-vsprintf-s-l-vswprintf-s-vswprintf-s-l.md)。

## <a name="syntax"></a>语法

```C
int vsprintf(
   char *buffer,
   const char *format,
   va_list argptr
);
int _vsprintf_l(
   char *buffer,
   const char *format,
   locale_t locale,
   va_list argptr
);
int vswprintf(
   wchar_t *buffer,
   size_t count,
   const wchar_t *format,
   va_list argptr
);
int _vswprintf_l(
   wchar_t *buffer,
   size_t count,
   const wchar_t *format,
   locale_t locale,
   va_list argptr
);
int __vswprintf_l(
   wchar_t *buffer,
   const wchar_t *format,
   locale_t locale,
   va_list argptr
);
template <size_t size>
int vsprintf(
   char (&buffer)[size],
   const char *format,
   va_list argptr
); // C++ only
template <size_t size>
int _vsprintf_l(
   char (&buffer)[size],
   const char *format,
   locale_t locale,
   va_list argptr
); // C++ only
template <size_t size>
int vswprintf(
   wchar_t (&buffer)[size],
   const wchar_t *format,
   va_list argptr
); // C++ only
template <size_t size>
int _vswprintf_l(
   wchar_t (&buffer)[size],
   const wchar_t *format,
   locale_t locale,
   va_list argptr
); // C++ only
```

### <a name="parameters"></a>参数

*宽限*\
输出的存储位置

*计*\
此函数的宽字符串版本中要存储的最大字符数。

*形式*\
格式规范。

*argptr*\
指向参数列表的指针。

*本地*\
要使用的区域设置。

## <a name="return-value"></a>返回值

**vsprintf** 和 **vswprintf** 返回写入的字符数，不包括终止 null 字符，或在出现输出错误时返回一个负值。 如果 *缓冲区* 或 *格式* 为 null 指针，则这些函数将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数将返回-1，并将 **errno** 设置为 **EINVAL**。

有关这些代码及其他错误代码的信息，请参阅 [_doserrno、errno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

## <a name="remarks"></a>注解

其中每个函数都采用一个指向参数列表的指针，然后将给定数据格式化并写入 *缓冲区* 指向的内存。

这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前线程区域设置。

> [!IMPORTANT]
> 使用 **vsprintf**，无法限制写入的字符数，这意味着使用此函数的代码容易受到缓冲区溢出的影响。 改用 [_vsnprintf](vsnprintf-vsnprintf-vsnprintf-l-vsnwprintf-vsnwprintf-l.md) 或调用 [_vscprintf](vscprintf-vscprintf-l-vscwprintf-vscwprintf-l.md) 以确定需要多大的缓冲区。 此外，请确保该 *格式* 不是用户定义的字符串。 有关详细信息，请参阅 [避免缓冲区溢出](/windows/win32/SecBP/avoiding-buffer-overruns)。
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

**vswprintf** 符合 ISO C 标准，该标准需要 **size_t** 类型的第二个参数 *count*。 若要强制使用旧的非标准行为，请定义 **_CRT_NON_CONFORMING_SWPRINTFS**。 旧的行为可能不在未来的版本中，因此应将代码更改为使用新的符合的行为。

在 C++ 中，这些函数具有模板重载，以调用这些函数的更新、更安全副本。 有关详细信息，请参阅[安全模板重载](../../c-runtime-library/secure-template-overloads.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_vstprintf**|**vsprintf**|**vsprintf**|**vswprintf**|
|**_vstprintf_l**|**_vsprintf_l**|**_vsprintf_l**|**_vswprintf_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|----------------------|
|**vsprintf**、 **_vsprintf_l**|\<stdio.h> 和 \<stdarg.h>|\<varargs.h>*|
|**vswprintf**、 **_vswprintf_l**|\<stdio.h> 或 \<wchar.h> 、和 \<stdarg.h>|\<varargs.h>*|

\* 仅对 UNIX V 兼容性是必需的。

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_vsprintf.c
// compile with: cl /W4 crt_vsprintf.c
// This program uses vsprintf to write to a buffer.
// The size of the buffer is determined by _vscprintf.

#define _CRT_SECURE_NO_WARNINGS
#include <stdlib.h>
#include <stdio.h>
#include <stdarg.h>

void test( char const * const format, ... )
{
    va_list args;
    int     len;
    char    *buffer;

    // retrieve the variable arguments
    va_start( args, format );

    len = _vscprintf( format, args ) // _vscprintf doesn't count
                                + 1; // terminating '\0'

    buffer = (char*)malloc( len * sizeof(char) );
    if ( 0 != buffer )
    {
        vsprintf( buffer, format, args ); // C4996
        // Note: vsprintf is deprecated; consider using vsprintf_s instead
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

[流 i/o](../../c-runtime-library/stream-i-o.md)\
[vprintf 函数](../../c-runtime-library/vprintf-functions.md)\
[格式规范语法： printf 和 wprintf 函数](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)\
[fprintf、_fprintf_l、fwprintf、_fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md)\
[printf、_printf_l、wprintf、_wprintf_l](printf-printf-l-wprintf-wprintf-l.md)\
[sprintf、_sprintf_l、swprintf、_swprintf_l、 \_ _swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)\
[va_arg、va_copy、va_end、va_start](va-arg-va-copy-va-end-va-start.md)
