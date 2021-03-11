---
description: 了解详细信息： sprintf、_sprintf_l、swprintf、_swprintf_l、__swprintf_l
title: sprintf、_sprintf_l、swprintf、_swprintf_l、__swprintf_l
ms.date: 3/9/2021
api_name:
- __swprintf_l
- sprintf
- _sprintf_l
- _swprintf_l
- swprintf
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
- ntdll.dll
- ucrtbase.dll
- ntoskrnl.exe
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _stprintf_l
- __swprintf_l
- sprintf_l
- swprintf
- _sprintf_l
- sprintf
- _stprintf
- stprintf_l
helpviewer_keywords:
- _swprintf_l function
- _stprintf function
- __swprintf_l function
- stprintf function
- sprintf function
- _sprintf_l function
- _stprintf_l function
- swprintf function
- strings [C++], writing to
- _CRT_NON_CONFORMING_SWPRINTFS
- swprintf_l function
- stprintf_l function
- sprintf_l function
- formatted text [C++]
ms.openlocfilehash: e8a83a8c92ab873c250e94e9e9317a18c36673a2
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622096"
---
# <a name="sprintf-_sprintf_l-swprintf-_swprintf_l-__swprintf_l"></a>sprintf、_sprintf_l、swprintf、_swprintf_l、__swprintf_l

将设置格式的数据写入字符串。 可提供某些函数的更多安全版本，请参阅 [sprintf_s、_sprintf_s_l、swprintf_s 和 _swprintf_s_l](sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l.md)。 安全版本的 **swprintf** 和 **_swprintf_l** 采用缓冲区的大小作为参数。

## <a name="syntax"></a>语法

```C
int sprintf(
   char *buffer,
   const char *format [,
   argument] ...
);
int _sprintf_l(
   char *buffer,
   const char *format,
   locale_t locale [,
   argument] ...
);
int swprintf(
   wchar_t *buffer,
   size_t count,
   const wchar_t *format [,
   argument]...
);
int _swprintf_l(
   wchar_t *buffer,
   size_t count,
   const wchar_t *format,
   locale_t locale [,
   argument] ...
);
int __swprintf_l(
   wchar_t *buffer,
   const wchar_t *format,
   locale_t locale [,
   argument] ...
);
template <size_t size>
int sprintf(
   char (&buffer)[size],
   const char *format [,
   argument] ...
); // C++ only
template <size_t size>
int _sprintf_l(
   char (&buffer)[size],
   const char *format,
   locale_t locale [,
   argument] ...
); // C++ only
```

### <a name="parameters"></a>参数

*宽限*<br/>
输出的存储位置

*计数*<br/>
要存储在 Unicode 版的此函数中的最大字符数。

*format*<br/>
格式控件字符串

argument <br/>
可选自变量

*locale*<br/>
要使用的区域设置。

有关更多信息，请参见 [格式规范](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)。

## <a name="return-value"></a>返回值

写入的字符数; 如果出现错误，则为-1。 如果 *缓冲区* 或 *格式* 为 null 指针，则将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数将返回-1，并将 **errno** 设置为 **EINVAL**。

**sprintf** 返回 *缓冲区* 中存储的字节数，不包括终止 null 字符。 **swprintf** 返回存储在 *缓冲区* 中的宽字符数，不包括终止 null 宽字符。

## <a name="remarks"></a>注解

**Sprintf** 函数将一系列字符和值存储到 *缓冲区* 中。 如果任何) 根据 *格式* 规范的相应格式规范进行转换和输出，则每个 *参数* (。 该格式包括普通字符，其形式和函数与 [printf](printf-printf-l-wprintf-wprintf-l.md)的 *format* 参数相同。 null 字符追加在写入的最后一个字符后。 如果在重叠的字符串之间发生复制，则此行为不确定。

> [!IMPORTANT]
> 使用 **sprintf**，无法限制写入的字符数，这意味着使用 **sprintf** 的代码容易受到缓冲区溢出的影响。 请考虑使用相关的函数 [_snprintf](snprintf-snprintf-snprintf-l-snwprintf-snwprintf-l.md)，该函数指定要写入 *缓冲区* 的最大字符数，或使用 [_scprintf](scprintf-scprintf-l-scwprintf-scwprintf-l.md) 确定缓冲区大小。 此外，请确保该 *格式* 不是用户定义的字符串。
>
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

**swprintf** 是 **sprintf** 的宽字符版本; **swprintf** 的指针参数是宽字符字符串。 **Swprintf** 中的编码错误检测可能与 **sprintf** 不同。 **swprintf** 和 **fwprintf** 的行为相同，只是 **swprintf** 将输出写入字符串而不是类型 **文件** 的目标，而 **swprintf** 要求使用 *count* 参数来指定要写入的最大字符数。 这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前线程区域设置。

**swprintf** 符合 ISO C 标准，该标准需要 **size_t** 类型的第二个参数 *count*。 若要强制使用旧的非标准行为，请定义 **_CRT_NON_CONFORMING_SWPRINTFS**。 在未来版本中，可能会删除旧行为，因此应将代码更改为使用新的符合标准行为。

在 C++ 中，这些函数具有模板重载，以调用这些函数的更新、更安全副本。 有关详细信息，请参阅[安全模板重载](../../c-runtime-library/secure-template-overloads.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_stprintf**|**sprintf**|**sprintf**|**_swprintf**|
|**_stprintf_l**|**_sprintf_l**|**_sprintf_l**|**__swprintf_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|**sprintf**、 **_sprintf_l**|\<stdio.h>|
|**swprintf**、 **_swprintf_l**|\<stdio.h> 或 \<wchar.h>|

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example-use-sprintf-to-format-data"></a>示例：使用 sprintf 设置数据格式

```C
// crt_sprintf.c
// compile with: /W3
// This program uses sprintf to format various
// data and place them in the string named buffer.

#include <stdio.h>

int main( void )
{
   char  buffer[200], s[] = "computer", c = 'l';
   int   i = 35, j;
   float fp = 1.7320534f;

   // Format and print various data:
   j  = sprintf( buffer,     "   String:    %s\n", s ); // C4996
   j += sprintf( buffer + j, "   Character: %c\n", c ); // C4996
   j += sprintf( buffer + j, "   Integer:   %d\n", i ); // C4996
   j += sprintf( buffer + j, "   Real:      %f\n", fp );// C4996
   // Note: sprintf is deprecated; consider using sprintf_s instead

   printf( "Output:\n%s\ncharacter count = %d\n", buffer, j );
}
```

```Output
Output:
   String:    computer
   Character: l
   Integer:   35
   Real:      1.732053

character count = 79
```

## <a name="example-error-code-handling"></a>示例：错误代码处理

```C
// crt_swprintf.c
// wide character example
// also demonstrates swprintf returning error code
#include <stdio.h>

int main( void )
{
   wchar_t buf[100];
   int len = swprintf( buf, 100, L"%s", L"Hello world" );
   printf( "wrote %d characters\n", len );
   len = swprintf( buf, 100, L"%s", L"Hello\xffff world" );
   // swprintf fails because string contains WEOF (\xffff)
   printf( "wrote %d characters\n", len );
}
```

```Output
wrote 11 characters
wrote -1 characters
```

## <a name="see-also"></a>另请参阅

[流 I/O](../../c-runtime-library/stream-i-o.md)<br/>
[fprintf、_fprintf_l、fwprintf、_fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md)<br/>
[printf、_printf_l、wprintf、_wprintf_l](printf-printf-l-wprintf-wprintf-l.md)<br/>
[scanf、_scanf_l、wscanf、_wscanf_l](scanf-scanf-l-wscanf-wscanf-l.md)<br/>
[sscanf、_sscanf_l、swscanf、_swscanf_l](sscanf-sscanf-l-swscanf-swscanf-l.md)<br/>
[vprintf 函数](../../c-runtime-library/vprintf-functions.md)<br/>
