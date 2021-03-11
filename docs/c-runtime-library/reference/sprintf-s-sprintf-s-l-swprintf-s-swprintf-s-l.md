---
description: 了解详细信息： sprintf_s、_sprintf_s_l、swprintf_s、_swprintf_s_l
title: sprintf_s、_sprintf_s_l、swprintf_s、_swprintf_s_l
ms.date: 3/9/2021
api_name:
- _swprintf_s_l
- _sprintf_s_l
- swprintf_s
- sprintf_s
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
- swprintf_s
- sprintf_s
- stdio/sprintf_s
- stdio/swprintf_s
- stdio/_sprintf_s_l
- stdio/_swprintf_s_l
- _sprintf_s_l
- _swprintf_s_l
helpviewer_keywords:
- stprintf_s function
- stprintf_s_l function
- swprintf_s_l function
- sprintf_s function
- swprintf_s function
- _swprintf_s_l function
- sprintf_s_l function
- _stprintf_s_l function
- _stprintf_s function
- _sprintf_s_l function
- formatted text [C++]
ms.openlocfilehash: 323ac9531a60aaff859c18d0f0b6a3811f4ad59a
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622109"
---
# <a name="sprintf_s-_sprintf_s_l-swprintf_s-_swprintf_s_l"></a>sprintf_s、_sprintf_s_l、swprintf_s、_swprintf_s_l

将设置格式的数据写入字符串。 如 [CRT 中的安全性功能](../../c-runtime-library/security-features-in-the-crt.md)中所述，这些版本的 [sprintf、_sprintf_l、swprintf、_swprintf_l、\__swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md) 具有安全性增强功能。

## <a name="syntax"></a>语法

```C
int sprintf_s(
   char *buffer,
   size_t sizeOfBuffer,
   const char *format,
   ...
);
int _sprintf_s_l(
   char *buffer,
   size_t sizeOfBuffer,
   const char *format,
   locale_t locale,
   ...
);
int swprintf_s(
   wchar_t *buffer,
   size_t sizeOfBuffer,
   const wchar_t *format,
   ...
);
int _swprintf_s_l(
   wchar_t *buffer,
   size_t sizeOfBuffer,
   const wchar_t *format,
   locale_t locale,
   ...
);
template <size_t size>
int sprintf_s(
   char (&buffer)[size],
   const char *format,
   ...
); // C++ only
template <size_t size>
int swprintf_s(
   wchar_t (&buffer)[size],
   const wchar_t *format,
   ...
); // C++ only
```

### <a name="parameters"></a>参数

*宽限*<br/>
输出的存储位置

*sizeOfBuffer*<br/>
可存储的最多字符数。

*format*<br/>
格式控件字符串

*...*<br/>
要设置格式的可选参数

*locale*<br/>
要使用的区域设置。

有关更多信息，请参见 [格式规范](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)。

## <a name="return-value"></a>返回值

写入的字符数; 如果出现错误，则为-1。 如果 *缓冲区* 或 *格式* 为 null 指针，则 **sprintf_s** 和 **swprintf_s** 返回-1，并将 **errno** 设置为 **EINVAL**。

**sprintf_s** 返回 *缓冲区* 中存储的字节数，不包括终止 null 字符。 **swprintf_s** 返回 *缓冲区* 中存储的宽字符数，不包括终止 null 宽字符。

## <a name="remarks"></a>注解

**Sprintf_s** 函数将一系列字符和值存储到 *缓冲区* 中。 如果任何) 根据 *格式* 规范的相应格式规范进行转换和输出，则每个 *参数* (。 该格式包括普通字符，其形式和函数与 [printf](printf-printf-l-wprintf-wprintf-l.md)的 *format* 参数相同。 null 字符追加在写入的最后一个字符后。 如果在重叠的字符串之间发生复制，则此行为不确定。

**Sprintf_s** 与 [sprintf](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)之间的一个主要区别在于 **sprintf_s** 检查格式字符串中的有效格式设置字符，而 [sprintf](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)仅检查格式字符串或缓冲区是否为 **NULL** 指针。 如果任一检查失败，将调用无效参数处理程序，如 [Parameter Validation](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则该函数将返回-1，并将 **errno** 设置为 **EINVAL**。

**Sprintf_s** 和 [sprintf](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)的另一个主要区别在于 **sprintf_s** 使用长度参数来指定输出缓冲区的大小（以字符为间隔）。 如果缓冲区对于格式化文本（包括终止 null）来说太小，则将缓冲区设置为空字符串，方法是在 *缓冲区*[0] 处放置 null 字符，并调用无效的参数处理程序。 与 **_snprintf** 不同， **sprintf_s** 确保缓冲区将以 null 结尾，除非缓冲区大小为零。

**swprintf_s** 是 **sprintf_s** 的宽字符版本; **swprintf_s** 的指针参数是宽字符字符串。 **Swprintf_s** 中的编码错误检测可能与 **sprintf_s** 中的不同。 这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前线程区域设置。

在 C++ 中，使用这些函数由模板重载简化；重载可以自动推导出缓冲区长度（不再需要指定大小参数），并且它们可以自动用以更新、更安全的对应物替换旧的、不安全的函数。 有关详细信息，请参阅[安全模板重载](../../c-runtime-library/secure-template-overloads.md)。

**Sprintf_s** 的版本提供对缓冲区太小后发生的情况的更多控制。 有关更多信息，请参见 [_snprintf_s, _snprintf_s_l, _snwprintf_s, _snwprintf_s_l](snprintf-s-snprintf-s-l-snwprintf-s-snwprintf-s-l.md)。

> [!IMPORTANT]
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_stprintf_s**|**sprintf_s**|**sprintf_s**|**swprintf_s**|
|**_stprintf_s_l**|**_sprintf_s_l**|**_sprintf_s_l**|**_swprintf_s_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|**sprintf_s**， **_sprintf_s_l**|Ansi-c \<stdio.h><br /><br /> C + +： \<cstdio> 或 \<stdio.h>|
|**swprintf_s**， **_swprintf_s_l**|C： \<stdio.h> 或 \<wchar.h><br /><br /> C + +： \<cstdio> 、 \<cwchar> \<stdio.h> 或 \<wchar.h>|

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example-use-sprintf_s-to-format-data"></a>示例：使用 sprintf_s 设置数据格式

```C
// crt_sprintf_s.c
// This program uses sprintf_s to format various
// data and place them in the string named buffer.
//

#include <stdio.h>

int main( void )
{
   char  buffer[200], s[] = "computer", c = 'l';
   int   i = 35, j;
   float fp = 1.7320534f;

   // Format and print various data:
   j  = sprintf_s( buffer, 200,     "   String:    %s\n", s );
   j += sprintf_s( buffer + j, 200 - j, "   Character: %c\n", c );
   j += sprintf_s( buffer + j, 200 - j, "   Integer:   %d\n", i );
   j += sprintf_s( buffer + j, 200 - j, "   Real:      %f\n", fp );

   printf_s( "Output:\n%s\ncharacter count = %d\n", buffer, j );
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
// crt_swprintf_s.c
// wide character example
// also demonstrates swprintf_s returning error code
#include <stdio.h>

int main( void )
{
   wchar_t buf[100];
   int len = swprintf_s( buf, 100, L"%s", L"Hello world" );
   printf( "wrote %d characters\n", len );
   len = swprintf_s( buf, 100, L"%s", L"Hello\xffff world" );
   // swprintf_s fails because string contains WEOF (\xffff)
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
[sprintf、_sprintf_l、swprintf、_swprintf_l、__swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)<br/>
[scanf、_scanf_l、wscanf、_wscanf_l](scanf-scanf-l-wscanf-wscanf-l.md)<br/>
[sscanf、_sscanf_l、swscanf、_swscanf_l](sscanf-sscanf-l-swscanf-swscanf-l.md)<br/>
[vprintf 函数](../../c-runtime-library/vprintf-functions.md)<br/>
