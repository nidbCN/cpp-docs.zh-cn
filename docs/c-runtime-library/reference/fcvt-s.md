---
description: 了解详细信息： _fcvt_s
title: _fcvt_s
ms.date: 4/2/2020
api_name:
- _fcvt_s
- _o__fcvt_s
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
- api-ms-win-crt-convert-l1-1-0.dll
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- fcvt_s
- _fcvt_s
helpviewer_keywords:
- fcvt_s function
- converting floating point, to strings
- floating-point functions, converting number to string
- _fcvt_s function
ms.assetid: 48671197-1d29-4c2b-a5d8-d2368f5f68a1
ms.openlocfilehash: 62b72a9f71f967077169086371ebf36542b154ed
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514442"
---
# <a name="_fcvt_s"></a>_fcvt_s

将浮点数转换为字符串。 这是 [_fcvt](fcvt.md) 版本，具有 [CRT 中的安全功能](../../c-runtime-library/security-features-in-the-crt.md)中所述的安全增强功能。

## <a name="syntax"></a>语法

```C
errno_t _fcvt_s(
   char* buffer,
   size_t sizeInBytes,
   double value,
   int count,
   int *dec,
   int *sign
);
template <size_t size>
errno_t _fcvt_s(
   char (&buffer)[size],
   double value,
   int count,
   int *dec,
   int *sign
); // C++ only
```

### <a name="parameters"></a>参数

*宽限*<br/>
所提供的缓冲区将保留转换的结果。

*sizeInBytes*<br/>
缓冲区的大小（以字节为单位）。

*value*<br/>
要转换的数字。

*计数*<br/>
小数点后面的数字位数。

*十进制*<br/>
指向存储的小数点位置的指针。

*sign*<br/>
指向存储的符号指示符的指针。

## <a name="return-value"></a>返回值

如果成功，则返回 0。 如果失败，则返回值为错误代码。 错误代码是在 Errno.h 中定义。 有关这些错误的列表，请参阅 [errno、_doserrno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

对于无效参数（如下表中所列），此函数调用无效参数处理程序，如[参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则此函数会将 **errno** 设置为 **EINVAL** 并返回 **EINVAL**。

### <a name="error-conditions"></a>错误条件

|*宽限*|*sizeInBytes*|value|计数|dec|签名|返回|*缓冲区* 中的值|
|--------------|-------------------|-----------|-----------|---------|----------|------------|-----------------------|
|**NULL**|any|any|any|any|any|**EINVAL**|未修改。|
|Not **NULL** (指向有效内存) |<=0|any|any|any|any|**EINVAL**|未修改。|
|any|any|any|any|**NULL**|any|**EINVAL**|未修改。|
|any|any|any|any|any|**NULL**|**EINVAL**|未修改。|

## <a name="security-issues"></a>安全问题

如果 *缓冲区* 未指向有效内存且不为 **NULL**，则 **_fcvt_s** 可能会产生访问冲突。

## <a name="remarks"></a>注解

**_Fcvt_s** 函数将浮点数转换为以 null 值结束的字符串。 *值* 参数是要转换的浮点数。 **_fcvt_s** 将 *值* 的位数存储为字符串，并在 ( "\ 0" ) 追加 null 字符。 *Count* 参数指定小数点后要存储的位数。 多余的数字被舍入到 *计数* 位置。 如果精度小于 *计数* 位数，则用零填充字符串。

字符串中仅存储位数。 在调用后，可以从 *dec* 和 *符号* 获取小数点的位置和 *值* 的符号。 *Dec* 参数指向整数值;此整数值提供小数点相对于字符串开头的位置。 零或负整数值表示小数点位于第一个数字的左侧。 参数 *sign* 指向一个整数，该整数指示 *值* 的符号。 如果 *值* 为正，则整数设置为 0; 如果 *值* 为负，则设置为非零数字。

长度 **_CVTBUFSIZE** 的缓冲区足以满足任何浮点值。

**_Ecvt_s** 和 **_fcvt_s** 之间的区别在于 *count* 参数的解释。 **_ecvt_s** 将 *计数**解释为输出* 字符串中的总位数，并 **_fcvt_s** 解释为小数点后的位数。

在 C++ 中，通过模板重载简化此函数的使用；重载可以自动推导出缓冲区长度，不再需要指定大小参数。 有关详细信息，请参阅[安全模板重载](../../c-runtime-library/secure-template-overloads.md)。

此函数的调试版本首先用0xFE 填充缓冲区。 若要禁用此行为，请使用 [_CrtSetDebugFillThreshold](crtsetdebugfillthreshold.md)。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

## <a name="requirements"></a>要求

|函数|必需的标头|可选标头|
|--------------|---------------------|---------------------|
|**_fcvt_s**|\<stdlib.h>|\<errno.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

**库：** 所有版本的 [C 运行](../../c-runtime-library/crt-library-features.md)库。

## <a name="example"></a>示例

```C
// fcvt_s.c
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>

int main()
{
    char * buf = 0;
    int decimal;
    int sign;
    int err;

    buf = (char*) malloc(_CVTBUFSIZE);
    err = _fcvt_s(buf, _CVTBUFSIZE, 1.2, 5, &decimal, &sign);

    if (err != 0)
    {
        printf("_fcvt_s failed with error code %d\n", err);
        exit(1);
    }

    printf("Converted value: %s\n", buf);
}
```

```Output
Converted value: 120000
```

## <a name="see-also"></a>另请参阅

[数据转换](../../c-runtime-library/data-conversion.md)<br/>
[浮点支持](../../c-runtime-library/floating-point-support.md)<br/>
[atof、_atof_l、_wtof、_wtof_l](atof-atof-l-wtof-wtof-l.md)<br/>
[_ecvt_s](ecvt-s.md)<br/>
[_gcvt_s](gcvt-s.md)<br/>
[_fcvt](fcvt.md)<br/>
