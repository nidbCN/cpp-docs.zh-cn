---
title: 跨 DLL 边界传递 CRT 对象时可能的错误
description: 概述通过动态链接库传递 Microsoft C 运行时对象时可能遇到的潜在问题 (DLL) 边界。
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- DLL conflicts [C++]
ms.assetid: c217ffd2-5d9a-4678-a1df-62a637a96460
ms.openlocfilehash: 7af0fe8b5819bf428753c9ec71099113df0fa79e
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514546"
---
# <a name="potential-errors-passing-crt-objects-across-dll-boundaries"></a>跨 DLL 边界传递 CRT 对象时可能的错误

通过跨 DLL 边界的函数调用将 C 运行时 (CRT) 对象（如文件句柄、区域设置和环境变量）传入或传出 DLL 时，如果 DLL 或调用到 DLL 中的任何文件使用不同的 CRT 库副本，则可能会出现意外的行为。

如果 () 显式分配内存 `new` `malloc` ，或使用、等隐式分配内存， `strdup` `strstreambuf::str` 然后跨 DLL 边界传递指针，则会出现相关问题。 如果 DLL 及其使用者使用的是不同的 CRT 库副本，则这可能会导致内存访问冲突或堆损坏。

此问题的另一个症状是调试期间 "输出" 窗口中的错误，例如 `HEAP[]: Invalid Address specified to RtlValidateHeap(#,#)`

## <a name="causes"></a>原因

CRT 库的每个副本都具有单独且完全不同的状态，且按应用或 DLL 保存于线程本地存储中。

CRT 对象（如文件句柄、环境变量和区域设置）仅对在其中分配或设置这些对象的应用程序或 DLL 中的 CRT 副本有效。 当 DLL 及其客户端使用不同的 CRT 库副本时，您不能跨 DLL 边界传递这些 CRT 对象，并希望它们在另一侧正确使用。 这适用于 Visual Studio 2015 和更高版本中的通用 CRT 之前的 CRT 版本。

对于使用 Visual Studio 2013 或更低版本生成的 Visual Studio 的每个版本，均有特定于版本的 CRT 库。 每个版本中的 CRT 的内部实现详细信息（如数据结构和命名约定）都是不同的。 从未支持将为 CRT 的某个版本编译的代码动态链接到不同版本的 CRT DLL。 这种情况有时是可行的，但由于好运而不是设计。

由于 CRT 库的每个副本都具有自己的堆管理器，因此分配一个 CRT 库中的内存并跨 DLL 边界传递要由 CRT 库的不同副本释放的指针可能导致堆损坏。 如果您在设计 DLL 时，使其跨 DLL 边界传递 CRT 对象，或分配内存并希望它在 DLL 之外被释放，则 DLL 的客户端必须使用与 DLL 相同的 CRT 库副本。

仅在 DLL 及其客户端都在加载时链接到相同版本的 CRT DLL 时，二者才使用相同的 CRT 库副本。 由于 Visual Studio 2015 所用的通用 CRT 库的 DLL 版本和更高版本的 Windows 10 现在是 (ucrtbase.dll) 集中部署的 Windows 组件，因此它对于使用 Visual Studio 2015 和更高版本生成的应用程序是相同的。 但是，即使 CRT 代码是相同的，也不能向使用不同堆的组件分配一个堆中分配的内存。

## <a name="example-pass-file-handle-across-dll-boundary"></a>示例：跨 DLL 边界传递文件句柄

### <a name="description"></a>说明

此示例跨 DLL 边界传递文件句柄。

DLL 和 .exe 文件是使用生成的 `/MD` ，因此它们共享 CRT 的单个副本。

如果重新生成以 `/MT` 使其使用单独的 CRT 副本，则运行生成的 **test1Main.exe** 会导致访问冲突。

```cpp
// test1Dll.cpp
// compile with: cl /EHsc /W4 /MD /LD test1Dll.cpp
#include <stdio.h>
__declspec(dllexport) void writeFile(FILE *stream)
{
   char   s[] = "this is a string\n";
   fprintf( stream, "%s", s );
   fclose( stream );
}
```

```cpp
// test1Main.cpp
// compile with: cl /EHsc /W4 /MD test1Main.cpp test1Dll.lib
#include <stdio.h>
#include <process.h>
void writeFile(FILE *stream);

int main(void)
{
   FILE  * stream;
   errno_t err = fopen_s( &stream, "fprintf.out", "w" );
   writeFile(stream);
   system( "type fprintf.out" );
}
```

```Output
this is a string
```

## <a name="example-pass-environment-variables-across-dll-boundary"></a>示例：跨 DLL 边界传递环境变量

### <a name="description"></a>说明

此示例跨 DLL 边界传递环境变量。

```cpp
// test2Dll.cpp
// compile with: cl /EHsc /W4 /MT /LD test2Dll.cpp
#include <stdio.h>
#include <stdlib.h>

__declspec(dllexport) void readEnv()
{
   char *libvar;
   size_t libvarsize;

   /* Get the value of the MYLIB environment variable. */
   _dupenv_s( &libvar, &libvarsize, "MYLIB" );

   if( libvar != NULL )
      printf( "New MYLIB variable is: %s\n", libvar);
   else
      printf( "MYLIB has not been set.\n");
   free( libvar );
}
```

```cpp
// test2Main.cpp
// compile with: cl /EHsc /W4 /MT test2Main.cpp test2dll.lib
#include <stdlib.h>
#include <stdio.h>

void readEnv();

int main( void )
{
   _putenv( "MYLIB=c:\\mylib;c:\\yourlib" );
   readEnv();
}
```

```Output
MYLIB has not been set.
```

如果 DLL 和 .exe 文件都是使用生成的 `/MD` ，只使用一个 CRT 副本，则程序将成功运行并产生以下输出：

```
New MYLIB variable is: c:\mylib;c:\yourlib
```

## <a name="see-also"></a>另请参阅

[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
