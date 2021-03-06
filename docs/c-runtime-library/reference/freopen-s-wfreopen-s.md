---
description: 了解详细信息： freopen_s、_wfreopen_s
title: freopen_s、_wfreopen_s
ms.date: 2/23/2021
api_name:
- _wfreopen_s
- freopen_s
- _o__wfreopen_s
- _o_freopen_s
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
- freopen_s
- _tfreopen_s
- _wfreopen_s
helpviewer_keywords:
- _tfreopen_s function
- _wfreopen_s function
- file pointers [C++], reassigning
- tfreopen_s function
- wfreopen_s function
- freopen_s function
ms.openlocfilehash: a2b9bf86d50972fcc820da3e53fd066717879ff3
ms.sourcegitcommit: c0c9cdae79f19655e809a4979227c51bb19cff63
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102236733"
---
# <a name="freopen_s-_wfreopen_s"></a>`freopen_s`, `_wfreopen_s`

关闭当前与关联的文件，并将其重新分配 `oldStream` `stream` 给指定的文件 `fileName` 。

这些版本的 [`freopen, _wfreopen`](freopen-wfreopen.md) 具有安全增强功能，如 [CRT 中的安全功能](../../c-runtime-library/security-features-in-the-crt.md)中所述。

## <a name="syntax"></a>语法

```C
errno_t freopen_s(
   FILE ** stream,
   const char * fileName,
   const char * mode,
   FILE* oldStream
);

errno_t _wfreopen_s(
   FILE ** stream,
   const wchar_t * fileName,
   const wchar_t * mode,
   FILE * oldStream
);
```

### <a name="parameters"></a>参数

*`stream`*\
当函数返回时，将指向重新打开的流的 out 参数。

*`fileName`*\
要重新打开的文件的路径。

*` mode`*\
重新打开的流的模式。

*`oldStream`*\
要重新打开的流。 它会刷新，并且任何与之关联的文件都将关闭。

## <a name="return-value"></a>返回值

如果成功，则为零;否则为错误代码。 如果发生错误，则将关闭原始文件并 **`NULL`** 将其写入， *`stream`* 除非 *`stream`* 也 **`NULL`**

有关错误代码的详细信息，请参阅 [`errno, _doserrno, _sys_errlist, and _sys_nerr`](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md) 。

## <a name="remarks"></a>备注

**`freopen_s`** 函数通常用于将与关联的预先打开的流附加 `stdin` `stdout` `stderr` 到另一个文件。

**`freopen_s`** 函数关闭当前与关联的文件 *`stream`* ，并将其重新分配 *`stream`* 给 *path* 指定的文件。 **`_wfreopen_s`** 是的宽字符版本; 的 **`freopen_s`** *路径* 和 *` mode`* 参数 **`_wfreopen_s`** 是宽字符字符串。 **`_wfreopen_s`** 和的 **`freopen_s`** 行为方式相同。

如果 *.pfile*、 *path*、 *` mode`* 或 *`stream`* 为 **`NULL`** ，或者如果 *path* 为空字符串，则这些函数将调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则这些函数将设置 **`errno`** 为 **`EINVAL`** 并返回 **`EINVAL`** 。

默认情况下，此函数的全局状态的作用域限定为应用程序。 若要更改此项，请参阅 [CRT 中的全局状态](../global-state.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|`TCHAR.H` 例程|_ `UNICODE & _MBCS` 未定义|`_MBCS` 明确|`_UNICODE` 明确|
|---------------------|------------------------------------|--------------------|-----------------------|
|**`_tfreopen_s`**|**`freopen_s`**|**`freopen_s`**|**`_wfreopen_s`**|

**`freopen_s`** 通常用于将预先打开的文件 **`stdin`** 、和重定向 **`stdout`** **`stderr`** 到用户指定的文件。 与关联的新文件 *`stream`* 使用打开 *` mode`* ，它是指定文件请求的访问类型的字符串，如下所示：

|*` mode`*|访问|
|-|-|
| **`"r"`** | 打开以便读取。 如果文件不存在或找不到，调用将 **`freopen_s`** 失败。 |
| **`"w"`** | 打开用于写入的空文件。 如果给定文件存在，则其内容会被销毁。 |
| **`"a"`** | 在文件末尾打开以进行写入（追加），在新数据写入到文件之前不移除文件末尾 (EOF) 标记。 如果文件不存在，则创建文件。 |
| **`"r+"`** | 打开以便读取和写入。 文件必须存在。 |
| **`"w+"`** | 打开用于读取和写入的空文件。 如果文件存在，则其内容会被销毁。 |
| **`"a+"`** | 打开以进行读取和追加。 追加操作包括在新数据写入文件之前移除 EOF 标记。 写入完成后，EOF 标记不会还原。 如果文件不存在，则创建文件。 |

使用 **`"w"`** 和类型时要 **`"w+"`** 小心，因为它们可能会破坏现有文件。 从 C11 开始， **`"x"`** 如果文件存在，你可以将追加到 **`"w"`** 或 **`"w+"`** 以使函数失败，而不是覆盖它。

当使用 **`"a"`** 或访问类型打开文件时 **`"a+"`** ，所有写入操作都将在文件末尾进行。 尽管可以使用或重新定位文件指针 [`fseek`](fseek-fseeki64.md) [`rewind`](rewind.md) ，但在执行任何写入操作前，文件指针将始终被移回文件末尾。因此，无法覆盖现有数据。

在 **`"a"`** 追加到文件之前，模式不会删除 EOF 标记。 在追加后，MS-DOS TYPE 命令只显示原始 EOF 标记之前的数据，不显示追加到文件的任何数据。 **`"a+"`** 模式会在将 EOF 标记追加到文件之前将其删除。 在追加后，MS-DOS TYPE 命令显示文件中的所有数据。 **`"a+"`** 若要将此模式附加到使用 CTRL + Z EOF 标记终止的流文件，则需要此模式。

**`"r+"`** **`"w+"`** 指定、或 **`"a+"`** 访问类型时，将允许读取和写入 (文件被称为 "更新" ) 。 但是，在读取和写入之间切换时，必须有干预 [`fsetpos`](fsetpos.md) 、 [`fseek`](fseek-fseeki64.md) 或 [`rewind`](rewind.md) 操作。 [`fsetpos`](fsetpos.md)如果需要，可以为或操作指定当前位置 [`fseek`](fseek-fseeki64.md) 。 除了以上值之外，字符串中还可能会包含以下字符之一 *` mode`* 以指定新行的转换模式。

|*` mode`* 组合键|转换模式|
|-|-|
| **`t`** | 在文本（转换）模式下打开。 |
| **`b`** | 在二进制 (未翻译的) 模式下打开;将禁止涉及回车符和换行符的翻译。 |

在文本 (翻译的) 模式下，回车换行符 (CR-LF) 组合转换为单行换行符 (LF) 输入的字符;换行符在输出时转换为 CR-LF 组合。 CTRL+Z 也将在输入时解释为文件尾字符。 在打开以进行读取或使用进行读写的文件中 **`"a+"`** ，运行时库将检查文件末尾的 CTRL + Z 并将其删除（如果可能）。 这是因为使用 [`fseek`](fseek-fseeki64.md) 和 [`ftell`](ftell-ftelli64.md) 在文件中移动时，可能会导致 [`fseek`](fseek-fseeki64.md) 在文件末尾附近运行不当。 **`t`** 如果需要 ANSI 可移植性，请不要使用此选项，因为它是 Microsoft 扩展。

如果 **`t`** **`b`** 在中未指定或 *` mode`* ，则默认转换模式由全局变量定义 [`_fmode`](../../c-runtime-library/fmode.md) 。 如果 **`t`** 或 **`b`** 是参数的前缀，则函数将失败并返回 **`NULL`** 。

有关文本模式和二进制模式的讨论，请参阅[文本和二进制模式文件 I/O](../../c-runtime-library/text-and-binary-mode-file-i-o.md)。

## <a name="requirements"></a>要求

|函数|必需的标头|
|--------------|---------------------|
|**`freopen_s`**|`<stdio.h>`|
|**`_wfreopen_s`**|`<stdio.h>` 或 `<wchar.h>`|

通用 Windows 平台 (UWP) 应用中不支持控制台。 与控制台、和关联的标准流句柄 **`stdin`** **`stdout`** **`stderr`** 必须重定向，然后 C 运行时函数才能在 UWP 应用中使用它们。 

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```C
// crt_freopen_s.c
// This program reassigns stderr to the file
// named FREOPEN.OUT and writes a line to that file.

#include <stdio.h>
#include <stdlib.h>

FILE *stream;

int main( void )
{
   errno_t err;
   // Reassign "stderr" to "freopen.out":
   err = freopen_s( &stream, "freopen.out", "w", stderr );

   if( err != 0 )
      fprintf( stdout, "error on freopen\n" );
   else
   {
      fprintf( stdout, "successfully reassigned\n" ); 
      fflush( stdout );
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

[`Stream I/O`](../../c-runtime-library/stream-i-o.md)\
[`freopen, _wfreopen`](freopen-wfreopen.md)\
[`fclose, _fcloseall`](fclose-fcloseall.md)\
[`_fdopen, _wfdopen`](fdopen-wfdopen.md)\
[`_fileno`](fileno.md)\
[`fopen, _wfope`北](fopen-wfopen.md)\
[`_open, _wopen`](open-wopen.md)\
[`_setmode`](setmode.md)
