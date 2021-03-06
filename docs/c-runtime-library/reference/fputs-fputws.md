---
description: 了解详细信息： fputs、fputws
title: fputs、fputws
ms.date: 03/02/2021
api_name:
- fputs
- fputws
- _o_fputs
- _o_fputws
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
- api-ms-win-crt-stdio-l1-1-0.dll
- api-ms-win-crt-private-l1-1-0.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- fputs
- fputws
- _fputts
helpviewer_keywords:
- streams, writing strings to
- fputws function
- _fputts function
- fputs function
- fputts function
ms.openlocfilehash: 3b38f3c369a567c00f17a0f4d905de324100a3d4
ms.sourcegitcommit: c0c9cdae79f19655e809a4979227c51bb19cff63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102236785"
---
# <a name="fputs-fputws"></a>`fputs`, `fputws`

将字符串写入流。

## <a name="syntax"></a>语法

```C
int fputs(
   const char *str,
   FILE *stream
);
int fputws(
   const wchar_t *str,
   FILE *stream
);
```

### <a name="parameters"></a>参数

*`str`*\
输出字符串。

*`stream`*\
指向 **`FILE`** 结构的指针。

## <a name="return-value"></a>返回值

如果成功，其中每个函数都将返回一个非负值。 出现错误时， **`fputs`** **`fputws`** 返回 **`EOF`** 。 如果 *`str`* 或 *`stream`* 为 null 指针，则这些函数将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数将设置 **`errno`** 为 **`EINVAL`** ，然后返回 **`EOF`** 。

有关错误代码的详细信息，请参阅 [_doserrno、errno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

## <a name="remarks"></a>备注

其中每个函数都复制 *`str`* 到当前位置的输出 *`stream`* 。 **`fputws`** 将宽字符自变量 *`str`* *`stream`* 作为多字节字符串或宽字符字符串复制到，具体取决于是否 *`stream`* 在文本模式或二进制模式下打开。 函数不会复制终止的 null 字符。

如果在 ANSI 模式下打开流，则这两个函数行为相同。 **`fputs`** 当前不支持到 UNICODE 流中的输出。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改它，请参阅 [CRT 中的全局状态](../global-state.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**`_fputts`**|**`fputs`**|**`fputs`**|**`fputws`**|

## <a name="requirements"></a>要求

|函数|必需的标头|
|--------------|---------------------|
|**`fputs`**|\<stdio.h>|
|**`fputws`**|\<stdio.h> 或 \<wchar.h>|

通用 Windows 平台 (UWP) 应用中不支持控制台。 与控制台、和关联的标准流句柄 **`stdin`** **`stdout`** **`stderr`** 必须重定向，然后 C 运行时函数才能在 UWP 应用中使用它们。 有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_fputs.c
// This program uses fputs to write
// a single line to the stdout stream.

#include <stdio.h>

int main( void )
{
   fputs( "Hello world from fputs.\n", stdout );
}
```

```Output
Hello world from fputs.
```

## <a name="see-also"></a>请参阅

[流 i/o](../../c-runtime-library/stream-i-o.md)\
[`fgets`, `fgetws`](fgets-fgetws.md)\
[`gets`, `_getws`](../../c-runtime-library/gets-getws.md)\
[`puts`, `_putws`](puts-putws.md)
