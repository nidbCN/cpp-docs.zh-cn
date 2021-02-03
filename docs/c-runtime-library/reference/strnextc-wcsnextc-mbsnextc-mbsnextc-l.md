---
description: 了解详细信息： _strnextc、_wcsnextc、_mbsnextc、_mbsnextc_l
title: _strnextc、_wcsnextc、_mbsnextc、_mbsnextc_l
ms.date: 4/2/2020
api_name:
- _strnextc
- _mbsnextc_l
- _mbsnextc
- _wcsnextc
- _o__mbsnextc
- _o__mbsnextc_l
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
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- strnextc
- tcsnextc
- _mbsnextc_l
- _mbsnextc
- mbsnextc_l
- ftcsnextc
- mbsnextc
- _tcsnextc
- _wcsnextc
- _ftcsnextc
- _strnextc
- wcsnextc
helpviewer_keywords:
- _mbsnextc function
- _tcsnextc function
- _wcsnextc function
- tcsnextc function
- strnextc function
- mbsnextc function
- _strnextc function
- _mbsnextc_l function
- mbsnextc_l function
- wcsnextc function
ms.assetid: e3086173-9eb5-4540-a23a-5d866bd05340
ms.openlocfilehash: 7cbe291d19fe09b14cf71989923656f44acff1c7
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522373"
---
# <a name="_strnextc-_wcsnextc-_mbsnextc-_mbsnextc_l"></a>_strnextc、_wcsnextc、_mbsnextc、_mbsnextc_l

查找字符串中的下一个字符。

> [!IMPORTANT]
> 不能在 Windows 运行时中执行的应用程序中使用 **_mbsnextc** 和 **_mbsnextc_l** 。 有关详细信息，请参阅[通用 Windows 平台应用中不支持的 CRT 函数](../../cppcx/crt-functions-not-supported-in-universal-windows-platform-apps.md)。

## <a name="syntax"></a>语法

```C
unsigned int _strnextc(
   const char *str
);
unsigned int _wcsnextc(
   const wchar_t *str
);
unsigned int _mbsnextc(
   const unsigned char *str
);
unsigned int _mbsnextc_l(
   const unsigned char *str,
   _locale_t locale
);
```

### <a name="parameters"></a>参数

*str*<br/>
资源字符串。

*locale*<br/>
要使用的区域设置。

## <a name="return-value"></a>返回值

其中每个函数返回 *str* 中下一个字符的整数值。

## <a name="remarks"></a>备注

**_Mbsnextc** 函数返回 *str* 中下一个多字节字符的整数值，而不向前移动字符串指针。 **_mbsnextc** 根据当前使用的 [多字节代码页](../../c-runtime-library/code-pages.md) 识别多字节字符序列。

如果 *str* 为 **NULL**，则将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则将 **errno** 设置为 **EINVAL** ，并且该函数将返回0。

**安全说明** 此 API 会引发由缓冲区溢出问题带来的潜在威胁。 缓冲区溢出问题是常见的系统攻击方法，使权限的提升不能确保。 有关详细信息，请参阅 [避免缓冲区溢出](/windows/win32/SecBP/avoiding-buffer-overruns)。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|Tchar.h 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|--------------------------------------|--------------------|-----------------------|
|**_tcsnextc**|**_strnextc**|**_mbsnextc**|**_wcsnextc**|

**_strnextc** 和 **_wcsnextc** 是 **_mbsnextc** 的单字节字符字符串和宽字符字符串版本。 **_wcsnextc** 返回 *str* 中下一个宽字符的整数值; **_strnextc** 返回 *str* 中下一个单字节字符的整数值。 仅为此映射提供 **_strnextc** 和 **_wcsnextc** ，因此不应使用。 有关详细信息，请参阅[使用一般文本映射](../../c-runtime-library/using-generic-text-mappings.md)和[一般文本映射](../../c-runtime-library/generic-text-mappings.md)。

**_mbsnextc_l** 相同，只不过它使用传入的区域设置参数。 有关详细信息，请参阅 [Locale](../../c-runtime-library/locale.md)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|**_mbsnextc**|\<mbstring.h>|
|**_mbsnextc_l**|\<mbstring.h>|
|**_strnextc**|\<tchar.h>|
|**_wcsnextc**|\<tchar.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="see-also"></a>另请参阅

[字符串操作](../../c-runtime-library/string-manipulation-crt.md)<br/>
[区域设置](../../c-runtime-library/locale.md)<br/>
[Multibyte-Character 序列的解释](../../c-runtime-library/interpretation-of-multibyte-character-sequences.md)<br/>
[_strdec、_wcsdec、_mbsdec、_mbsdec_l](strdec-wcsdec-mbsdec-mbsdec-l.md)<br/>
[_strinc、_wcsinc、_mbsinc、_mbsinc_l](strinc-wcsinc-mbsinc-mbsinc-l.md)<br/>
[_strninc、_wcsninc、_mbsninc、_mbsninc_l](strninc-wcsninc-mbsninc-mbsninc-l.md)<br/>
