---
description: 了解详细信息： strlen、wcslen、_mbslen、_mbslen_l、_mbstrlen _mbstrlen_l
title: strlen、wcslen、_mbslen、_mbslen_l、_mbstrlen、_mbstrlen_l
ms.date: 4/2/2020
api_name:
- _mbslen
- _mbslen_l
- _mbstrlen
- wcslen
- _mbstrlen_l
- strlen
- _o__mbslen
- _o__mbslen_l
- _o__mbstrlen
- _o__mbstrlen_l
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
- api-ms-win-crt-multibyte-l1-1-0.dll
- api-ms-win-crt-string-l1-1-0.dll
- ntoskrnl.exe
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _mbstrlen
- wcslen
- _tcslen
- _ftcslen
- strlen
- _mbslen
helpviewer_keywords:
- wcslen function
- string length, getting
- ftcslen function
- lengths, strings
- mbstrlen_l function
- _mbslen_l function
- _tcslen function
- mbslen_l function
- mbslen function
- _mbstrlen function
- strings [C++], getting length
- mbstrlen function
- _mbstrlen_l function
- _ftcslen function
- tcslen function
- strlen function
- _mbslen function
ms.assetid: 16462f2a-1e0f-4eb3-be55-bf1c83f374c2
ms.openlocfilehash: 8958285ed19d8bd38701d5cb58cd18b3c21caaa5
ms.sourcegitcommit: c0c9cdae79f19655e809a4979227c51bb19cff63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102236720"
---
# <a name="strlen-wcslen-_mbslen-_mbslen_l-_mbstrlen-_mbstrlen_l"></a>strlen、wcslen、_mbslen、_mbslen_l、_mbstrlen、_mbstrlen_l

通过使用当前区域设置或指定区域设置获取字符串的长度。 这些函数的更安全版本已经发布，请参阅 [strnlen、strnlen_s、wcsnlen、wcsnlen_s、_mbsnlen、_mbsnlen_l、_mbstrnlen、_mbstrnlen_l](strnlen-strnlen-s.md)

> [!IMPORTANT]
> **_mbslen**、 **_mbslen_l**、 **_mbstrlen** 和 **_mbstrlen_l** 不能用于在 Windows 运行时中执行的应用程序。 有关详细信息，请参阅[通用 Windows 平台应用中不支持的 CRT 函数](../../cppcx/crt-functions-not-supported-in-universal-windows-platform-apps.md)。

## <a name="syntax"></a>语法

```C
size_t strlen(
   const char *str
);
size_t wcslen(
   const wchar_t *str
);
size_t _mbslen(
   const unsigned char *str
);
size_t _mbslen_l(
   const unsigned char *str,
   _locale_t locale
);
size_t _mbstrlen(
   const char *str
);
size_t _mbstrlen_l(
   const char *str,
   _locale_t locale
);
```

### <a name="parameters"></a>参数

*str*<br/>
以 Null 结尾的字符串。

*locale*<br/>
要使用的区域设置。

## <a name="return-value"></a>返回值

其中每个函数均返回 *str* 中的字符数，不包括终端 null。 不保留任何返回值以指示错误， **_mbstrlen** 和 **_mbstrlen_l** 除外， `((size_t)(-1))` 如果字符串包含无效的多字节字符，则返回。

## <a name="remarks"></a>备注

**strlen** 将字符串解释为单字节字符字符串，因此即使字符串包含多字节字符，其返回值也始终等于字节数。 **wcslen** 是 **strlen** 的宽字符版本; **wcslen** 的参数是宽字符字符串，字符计数以宽 (两字节) 字符为范围。 否则， **wcslen** 和 **strlen** 的行为相同。

**安全说明** 这些函数会引发由缓冲区溢出问题带来的潜在威胁。 缓冲区溢出问题是常见的系统攻击方法，使权限的提升不能确保。 有关详细信息，请参阅 [避免缓冲区溢出](/windows/win32/SecBP/avoiding-buffer-overruns)。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_tcslen**|strlen|strlen|**wcslen**|
|**_tcsclen**|strlen|**_mbslen**|**wcslen**|
|**_tcsclen_l**|strlen|**_mbslen_l**|**wcslen**|

**_mbslen** 和 **_mbslen_l** 返回多字节字符字符串中的多字节字符数，但它们不会测试多字节字符的有效性。 **_mbstrlen** 和 **_mbstrlen_l** 测试多字节字符的有效性并识别多字节字符序列。 如果传递到 **_mbstrlen** 或 **_mbstrlen_l** 的字符串包含代码页的无效多字节字符，则该函数将返回-1，并将 **errno** 设置为 **eilseq 且**。

输出值受区域设置的 LC_CTYPE 类别设置影响；有关详细信息，请参阅 [setlocale](setlocale-wsetlocale.md)。 这些不带 **_l** 后缀的函数版本使用此区域设置相关的行为的当前区域设置；带有 **_l** 后缀的版本相同，只不过它们使用传递的区域设置参数。 有关详细信息，请参阅 [Locale](../../c-runtime-library/locale.md)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|strlen|\<string.h>|
|**wcslen**|\<string.h> 或 \<wchar.h>|
|**_mbslen**， **_mbslen_l**|\<mbstring.h>|
|**_mbstrlen**， **_mbstrlen_l**|\<stdlib.h>|

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_strlen.c
// Determine the length of a string. For the multi-byte character
// example to work correctly, the Japanese language support for
// non-Unicode programs must be enabled by the operating system.

#include <string.h>
#include <locale.h>

int main()
{
   char* str1 = "Count.";
   wchar_t* wstr1 = L"Count.";
   char * mbstr1;
   char * locale_string;

   // strlen gives the length of single-byte character string
   printf("Length of '%s' : %d\n", str1, strlen(str1) );

   // wcslen gives the length of a wide character string
   wprintf(L"Length of '%s' : %d\n", wstr1, wcslen(wstr1) );

   // A multibyte string: [A] [B] [C] [katakana A] [D] [\0]
   // in Code Page 932. For this example to work correctly,
   // the Japanese language support must be enabled by the
   // operating system.
   mbstr1 = "ABC" "\x83\x40" "D";

   locale_string = setlocale(LC_CTYPE, "Japanese_Japan");

   if (locale_string == NULL)
   {
      printf("Japanese locale not enabled. Exiting.\n");
      exit(1);
   }
   else
   {
      printf("Locale set to %s\n", locale_string);
   }

   // _mbslen will recognize the Japanese multibyte character if the
   // current locale used by the operating system is Japanese
   printf("Length of '%s' : %d\n", mbstr1, _mbslen(mbstr1) );

   // _mbstrlen will recognize the Japanese multibyte character
   // since the CRT locale is set to Japanese even if the OS locale
   // isnot.
   printf("Length of '%s' : %d\n", mbstr1, _mbstrlen(mbstr1) );
   printf("Bytes in '%s' : %d\n", mbstr1, strlen(mbstr1) );

}
```

```Output
Length of 'Count.' : 6
Length of 'Count.' : 6
Length of 'ABCァD' : 5
Length of 'ABCァD' : 5
Bytes in 'ABCァD' : 6
```

## <a name="see-also"></a>请参阅

[字符串操作](../../c-runtime-library/string-manipulation-crt.md)<br/>
[Multibyte-Character 序列的解释](../../c-runtime-library/interpretation-of-multibyte-character-sequences.md)<br/>
[区域设置](../../c-runtime-library/locale.md)<br/>
[setlocale、_wsetlocale](setlocale-wsetlocale.md)<br/>
[strcat、wcscat、_mbscat](strcat-wcscat-mbscat.md)<br/>
[strcmp、wcscmp、_mbscmp](strcmp-wcscmp-mbscmp.md)<br/>
[strcoll 函数](../../c-runtime-library/strcoll-functions.md)<br/>
[strcpy、wcscpy、_mbscpy](strcpy-wcscpy-mbscpy.md)<br/>
[strrchr、wcsrchr、_mbsrchr、_mbsrchr_l](strrchr-wcsrchr-mbsrchr-mbsrchr-l.md)<br/>
[_strset、_strset_l、_wcsset、_wcsset_l、_mbsset、_mbsset_l](strset-strset-l-wcsset-wcsset-l-mbsset-mbsset-l.md)<br/>
[strspn、wcsspn、_mbsspn、_mbsspn_l](strspn-wcsspn-mbsspn-mbsspn-l.md)<br/>
