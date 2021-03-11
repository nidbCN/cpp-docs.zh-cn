---
description: 了解详细信息： _scprintf_p、_scprintf_p_l、_scwprintf_p、_scwprintf_p_l
title: _scprintf_p、_scprintf_p_l、_scwprintf_p、_scwprintf_p_l
ms.date: 3/9/2021
api_name:
- _scwprintf_p
- _scprintf_p_l
- _scwprintf_p_l
- _scprintf_p
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
- _scwprintf_p_l
- _sctprintf_p
- scprintf_p_l
- scprintf_p
- _sctprintf_p_l
- scwprintf_p
- _scprintf_p_l
- scwprintf_p_l
- _scprintf_p
- _scwprintf_p
helpviewer_keywords:
- sctprintf_p_l function
- _scwprintf_p_l function
- scprintf_p_l function
- _scprintf_p function
- _scprintf_p_l function
- scprintf_p function
- sctprintf_p function
- _scwprintf_p function
- _sctprintf_p function
- scwprintf_p function
- scwprintf_p_l function
- _sctprintf_p_l function
ms.openlocfilehash: 3f47eacfcef8fec0b06363d66020d6fed936f3dd
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102621732"
---
# <a name="_scprintf_p-_scprintf_p_l-_scwprintf_p-_scwprintf_p_l"></a>_scprintf_p、_scprintf_p_l、_scwprintf_p、_scwprintf_p_l

返回格式化字符串的字符数量，并且能够指定参数在格式字符串中的使用顺序。

## <a name="syntax"></a>语法

```C
int _scprintf_p(
   const char *format [,
   argument] ...
);
int _scprintf_p_l(
   const char *format,
   locale_t locale [,
   argument] ...
);
int _scwprintf_p (
   const wchar_t *format [,
   argument] ...
);
int _scwprintf_p _l(
   const wchar_t *format,
   locale_t locale [,
   argument] ...
);
```

### <a name="parameters"></a>参数

*format*<br/>
窗体控件字符串。

argument <br/>
可选参数。

*locale*<br/>
要使用的区域设置。

## <a name="return-value"></a>返回值

返回在使用指定的格式化代码打印字符串或将其发送到文件或缓冲区时生成的字符的数目。 返回的值不包括终止 null 字符。 **_scwprintf_p** 对宽字符执行相同的功能。

**_Scprintf_p** 和 **_scprintf** 之间的区别在于 **_scprintf_p** 支持位置参数，这允许指定在格式字符串中使用参数的顺序。 有关详细信息，请参阅 [printf_p 位置参数](../../c-runtime-library/printf-p-positional-parameters.md)。

如果 *format* 为 **空** 指针，则将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数将返回-1，并将 **errno** 设置为 **EINVAL**。

有关这些及其他错误代码的信息，请参阅 [_doserrno、errno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

## <a name="remarks"></a>注解

如果根据 *格式* 的相应格式规范转换了任何) ，则每个 *参数* (。 该格式包括普通字符，其形式和函数与 [printf](printf-printf-l-wprintf-wprintf-l.md)的 *format* 参数相同。

这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前线程区域设置。

> [!IMPORTANT]
> 确保 format 不是用户定义的字符串。
>
>
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|Tchar.h 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|--------------------------------------|--------------------|-----------------------|
|**_sctprintf_p**|**_scprintf_p**|**_scprintf_p**|**_scwprintf_p**|
|**_sctprintf_p_l**|**_scprintf_p_l**|**_scprintf_p_l**|**_scwprintf_p_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|**_scprintf_p**， **_scprintf_p_l**|\<stdio.h>|
|**_scwprintf_p**， **_scwprintf_p_l**|\<stdio.h> 或 \<wchar.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="see-also"></a>另请参阅

[流 I/O](../../c-runtime-library/stream-i-o.md)<br/>
[_scprintf、_scprintf_l、_scwprintf、_scwprintf_l](scprintf-scprintf-l-scwprintf-scwprintf-l.md)<br/>
[_printf_p、_printf_p_l、_wprintf_p、_wprintf_p_l](printf-p-printf-p-l-wprintf-p-wprintf-p-l.md)<br/>
