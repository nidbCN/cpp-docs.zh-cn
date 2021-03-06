---
description: 了解详细信息： freopen、_wfreopen
title: freopen、_wfreopen
ms.date: 2/23/2021
api_name:
- freopen
- _wfreopen
- _o__wfreopen
- _o_freopen
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
- _wfreopen
- _tfreopen
- freopen
helpviewer_keywords:
- _wfreopen function
- file pointers [C++], reassigning
- _tfreopen function
- freopen function
- tfreopen function
- wfreopen function
ms.openlocfilehash: 7b90be37154e2ceb951623e130cb1a45ae0a71bf
ms.sourcegitcommit: c0c9cdae79f19655e809a4979227c51bb19cff63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102236655"
---
# <a name="freopen-_wfreopen"></a>`freopen`, `_wfreopen`

重新分配文件指针。 提供这些函数的更多安全版本;请参阅 [`freopen_s, _wfreopen_s`](freopen-s-wfreopen-s.md) 。

## <a name="syntax"></a>语法

```C
FILE *freopen(
   const char *path,
   const char *mode,
   FILE *stream
);
FILE *_wfreopen(
   const wchar_t *path,
   const wchar_t *mode,
   FILE *stream
);
```

### <a name="parameters"></a>参数

*`path`*\
新文件的路径。

*`mode`*\
允许的访问类型。

*`stream`*\
指向 **`FILE`** 结构的指针。

## <a name="return-value"></a>返回值

这些函数均返回指向新打开的文件的指针。 如果发生错误，则关闭原始文件，且该函数返回 **`NULL`** 指针值。 如果 *`path`* 、 *`mode`* 或 *`stream`* 是 null 指针，或者 *filename* 为空字符串，则这些函数将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数将设置 **`errno`** 为 **`EINVAL`** 并返回 **`NULL`** 。

有关这些错误代码及其他错误代码的详细信息，请参阅 [`_doserrno, errno, _sys_errlist, and _sys_nerr`](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md) 。 

## <a name="remarks"></a>备注

存在这些函数的更多安全版本，请参阅 [`freopen_s, _wfreopen_s`](freopen-s-wfreopen-s.md) 。

**`freopen`** 函数关闭当前与关联的文件，并将其重新分配 *`stream`* *`stream`* 给指定的文件 *`path`* 。 **`_wfreopen`** 是的宽字符版本 **`_freopen`** ; 的 *`path`* 和 *`mode`* 参数 **`_wfreopen`** 是宽字符字符串。 **`_wfreopen`** 和的 **`_freopen`** 行为方式相同。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|`TCHAR.H `例程|_ `UNICODE & _MBCS` 未定义|`_MBCS` 明确|`_UNICODE` 明确|
|---------------------|------------------------------------|--------------------|-----------------------|
|**`_tfreopen`**|**`freopen`**|**`freopen`**|**`_wfreopen`**|

**`freopen`** 通常用于将预先打开的文件 **`stdin`** 、和重定向 **`stdout`** **`stderr`** 到用户指定的文件。 与关联的新文件 *`stream`* 使用打开 *`mode`* ，它是指定文件请求的访问类型的字符串，如下所示：

|*`mode`*|访问|
|-|-|
| **`"r"`** | 打开以便读取。 如果文件不存在或找不到，调用将 **`freopen`** 失败。 |
| **`"w"`** | 打开用于写入的空文件。 如果给定文件存在，则其内容会被销毁。 |
| **`"a"`** | 在文件末尾打开以进行写入（追加），在新数据写入到文件之前不移除文件末尾 (EOF) 标记。 如果文件不存在，则创建文件。 |
| **`"r+"`** | 打开以便读取和写入。 文件必须存在。 |
| **`"w+"`** | 打开用于读取和写入的空文件。 如果文件存在，则其内容会被销毁。 |
| **`"a+"`** | 打开以进行读取和追加。 追加操作包括在新数据写入文件之前移除 EOF 标记。 写入完成后，EOF 标记不会还原。 如果文件不存在，则创建文件。 |

使用 **`"w"`** 和类型时要 **`"w+"`** 小心，因为它们可能会破坏现有文件。 从 C11 开始， **`"x"`** 如果文件存在，你可以将添加到 **`"w"`** 或 **`"w+"`** 以导致函数失败，而不是覆盖它。

当使用 **`"a"`** 或访问类型打开文件时 **`"a+"`** ，所有写入操作都将在文件末尾进行。 尽管可以使用或重新定位文件指针 [`fseek`](fseek-fseeki64.md) [`rewind`](rewind.md) ，但在执行任何写入操作前，文件指针将始终被移回文件末尾。因此，无法覆盖现有数据。

在 **`"a"`** 追加到文件之前，模式不会删除 EOF 标记。 在追加后，MS-DOS TYPE 命令只显示原始 EOF 标记之前的数据，不显示追加到文件的任何数据。 **`"a+"`** 模式会在将 EOF 标记追加到文件之前将其删除。 在追加后，MS-DOS TYPE 命令显示文件中的所有数据。 **`"a+"`** 若要将此模式附加到使用 CTRL + Z EOF 标记终止的流文件，则需要此模式。

**`"r+"`** **`"w+"`** 指定、或 **`"a+"`** 访问类型时，将允许读取和写入 (文件被称为 "更新" ) 。 但是，在读取和写入之间切换时，必须有干预 [`fsetpos`](fsetpos.md) 、 [`fseek`](fseek-fseeki64.md) 或 [`rewind`](rewind.md) 操作。 [`fsetpos`](fsetpos.md)如果需要，可以为或操作指定当前位置 [`fseek`](fseek-fseeki64.md) 。 除了以上值之外，字符串中还可能会包含以下字符之一 *`mode`* 以指定新行的转换模式。

|*`mode`* 组合键|转换模式|
|-|-|
| **`t`** | 在文本（转换）模式下打开。 |
| **`b`** | 在二进制 (未翻译的) 模式下打开;将禁止涉及回车符和换行符的翻译。 |

在文本 (翻译的) 模式下，回车换行符 (CR-LF) 组合转换为单行换行符 (LF) 输入的字符;换行符在输出时转换为 CR-LF 组合。 CTRL+Z 也将在输入时解释为文件尾字符。 在打开以进行读取或使用进行读写的文件中 **`"a+"`** ，运行时库将检查文件末尾的 CTRL + Z 并将其删除（如果可能）。 这是因为使用 [`fseek`](fseek-fseeki64.md) 和 [ftell](ftell-ftelli64.md) 在文件中移动可能导致在 [`fseek`](fseek-fseeki64.md) 文件末尾附近运行不当。 **`t`** 如果需要 ANSI 可移植性，请不要使用此选项，因为它是 Microsoft 扩展。

如果 **`t`** **`b`** 在中未指定或 *`mode`* ，则默认转换模式由全局变量定义 [`_fmode`](../../c-runtime-library/fmode.md) 。 如果 **`t`** 或 **`b`** 是参数的前缀，则函数将失败并返回 **`NULL`** 。

有关文本模式和二进制模式的讨论，请参阅[文本和二进制模式文件 I/O](../../c-runtime-library/text-and-binary-mode-file-i-o.md)。

## <a name="requirements"></a>要求

|函数|必需的标头|
|--------------|---------------------|
|**`freopen`**|`<stdio.h>`|
|**`_wfreopen`**|`<stdio.h>` 或 `<wchar.h>`|

通用 Windows 平台 (UWP) 应用中不支持控制台。 与控制台、和关联的标准流句柄 **`stdin`** **`stdout`** **`stderr`** 必须重定向，然后 C 运行时函数才能在 UWP 应用中使用它们。 有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_freopen.c
// compile with: /W3
// This program reassigns stderr to the file
// named FREOPEN.OUT and writes a line to that file.
#include <stdio.h>
#include <stdlib.h>

FILE *stream;

int main( void )
{
   // Reassign "stderr" to "freopen.out":
   stream = freopen( "freopen.out", "w", stderr ); // C4996
   // Note: freopen is deprecated; consider using freopen_s instead

   if( stream == NULL )
      fprintf( stdout, "error on freopen\n" );
   else
   {
      fprintf( stdout, "successfully reassigned\n" ); fflush( stdout );
      fprintf( stream, "This will go to the file 'freopen.out'\n" );
      fclose( stream );
   }
   system( "type freopen.out" );
}
```

```Output
successfully reassigned
This will go to the file 'freopen.out'
```

## <a name="see-also"></a>请参阅

[流 i/o](../../c-runtime-library/stream-i-o.md)\
[`fclose, _fcloseall`](fclose-fcloseall.md)\
[`_fdopen, _wfdopen`](fdopen-wfdopen.md)\
[`_fileno`](fileno.md)\
[`fopen, _wfopen`](fopen-wfopen.md)\
[`_open, _wopen`](open-wopen.md)\
[`_setmode`](setmode.md)\
