---
description: 了解详细信息： fprintf_s、_fprintf_s_l、fwprintf_s、_fwprintf_s_l
title: fprintf_s、_fprintf_s_l、fwprintf_s、_fwprintf_s_l
ms.date: 3/9/2021
api_name:
- _fprintf_s_l
- fwprintf_s
- fprintf_s
- _fwprintf_s_l
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
- _ftprintf_s
- fprintf_s
- fwprintf_s
helpviewer_keywords:
- ftprintf_s_l function
- ftprintf_s function
- _fprintf_s_l function
- _ftprintf_s function
- _ftprintf_s_l function
- fwprintf_s_l function
- fwprintf_s function
- fprintf_s_l function
- fprintf_s function
- _fwprintf_s_l function
- print formatted data to streams
ms.openlocfilehash: bb4c9d7ff137ee019e439014181578615ac439e7
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102621797"
---
# <a name="fprintf_s-_fprintf_s_l-fwprintf_s-_fwprintf_s_l"></a>fprintf_s、_fprintf_s_l、fwprintf_s、_fwprintf_s_l

将格式化数据输出到流。 如 [CRT 中的安全功能](../../c-runtime-library/security-features-in-the-crt.md)中所述，这些版本的 [fprintf、_fprintf_l、fwprintf、_fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md) 具有安全增强功能。

## <a name="syntax"></a>语法

```C
int fprintf_s(
   FILE *stream,
   const char *format [,
   argument_list ]
);
int _fprintf_s_l(
   FILE *stream,
   const char *format,
   locale_t locale [,
   argument_list ]
);
int fwprintf_s(
   FILE *stream,
   const wchar_t *format [,
   argument_list ]
);
int _fwprintf_s_l(
   FILE *stream,
   const wchar_t *format,
   locale_t locale [,
   argument_list ]
);
```

### <a name="parameters"></a>参数

*流*<br/>
指向 **文件** 结构的指针。

*format*<br/>
窗体控件字符串。

*argument_list*<br/>
格式字符串的可选参数。

*locale*<br/>
要使用的区域设置。

## <a name="return-value"></a>返回值

**fprintf_s** 返回写入的字节数。 **fwprintf_s** 返回已写入的宽字符数。 其中每个函数在出现输出错误时返回一个负值。

## <a name="remarks"></a>注解

**fprintf_s** 设置格式并将一系列字符和值输出到输出 *流* 中。 *Argument_list* 中的每个参数 (根据 *格式* 规范中的相应格式规范转换和输出任何) 。 *Format* 参数将 [格式规范语法用于 printf 和 wprintf 函数](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)。

**fwprintf_s** 是 **fprintf_s** 的宽字符版本;在 **fwprintf_s** 中， *format* 是宽字符字符串。 如果在 ANSI 模式下打开流，则这些函数行为相同。 **fprintf_s** 当前不支持输出到 UNICODE 流中。

这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前区域设置。

> [!IMPORTANT]
> 确保 format 不是用户定义的字符串。
>
>
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

与不安全版本一样 (参阅 [fprintf、_fprintf_l、fwprintf _fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md)) ，这些函数将验证其参数并调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述（如果 *stream* 或 *format* 为空指针）。 格式字符串本身也会进行验证。 如果有任何未知或格式错误的格式化说明符，则这些函数将生成无效参数异常。 在所有情况下，如果允许执行继续，则函数将返回-1，并将 **errno** 设置为 **EINVAL**。 有关这些代码以及其他错误代码的详细信息，请参阅 [_doserrno、errno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_ftprintf_s**|**fprintf_s**|**fprintf_s**|**fwprintf_s**|
|**_ftprintf_s_l**|**_fprintf_s_l**|**_fprintf_s_l**|**_fwprintf_s_l**|

有关更多信息，请参见 [格式规范](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)。

## <a name="requirements"></a>要求

|函数|必需的标头|
|--------------|---------------------|
|**fprintf_s**， **_fprintf_s_l**|\<stdio.h>|
|**fwprintf_s**， **_fwprintf_s_l**|\<stdio.h> 或 \<wchar.h>|

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_fprintf_s.c
// This program uses fprintf_s to format various
// data and print it to the file named FPRINTF_S.OUT. It
// then displays FPRINTF_S.OUT on the screen using the system
// function to invoke the operating-system TYPE command.

#include <stdio.h>
#include <process.h>

FILE *stream;

int main( void )
{
   int    i = 10;
   double fp = 1.5;
   char   s[] = "this is a string";
   char   c = '\n';

   fopen_s( &stream, "fprintf_s.out", "w" );
   fprintf_s( stream, "%s%c", s, c );
   fprintf_s( stream, "%d\n", i );
   fprintf_s( stream, "%f\n", fp );
   fclose( stream );
   system( "type fprintf_s.out" );
}
```

```Output
this is a string
10
1.500000
```

## <a name="see-also"></a>另请参阅

[流 I/O](../../c-runtime-library/stream-i-o.md)<br/>
[_cprintf、_cprintf_l、_cwprintf、_cwprintf_l](cprintf-cprintf-l-cwprintf-cwprintf-l.md)<br/>
[fscanf、_fscanf_l、fwscanf、_fwscanf_l](fscanf-fscanf-l-fwscanf-fwscanf-l.md)<br/>
[sprintf、_sprintf_l、swprintf、_swprintf_l、 \_ _swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)<br/>
