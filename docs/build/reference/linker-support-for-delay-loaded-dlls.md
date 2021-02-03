---
description: 了解更多：链接器支持延迟加载的 Dll
title: 链接器的延迟加载 DLL 支持
ms.date: 01/19/2021
helpviewer_keywords:
- delayed loading of DLLs, linker support
ms.openlocfilehash: 02991d6ac409ef301e326eea63ece8c5c7775010
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522412"
---
# <a name="linker-support-for-delay-loaded-dlls"></a>链接器的延迟加载 DLL 支持

MSVC 链接器支持 Dll 的延迟加载。 此功能使您不必使用 Windows SDK 函数 [`LoadLibrary`](/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryw) 并 [`GetProcAddress`](/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress) 实现 DLL 延迟加载。

如果没有延迟加载，则在运行时加载 DLL 的唯一方法是使用 `LoadLibrary` 和 `GetProcAddress` ; 当使用和时，操作系统将加载 dll。

使用延迟加载时，隐式链接 DLL 时，链接器会提供一些选项，用于延迟 DLL 加载，直到程序调用该 DLL 中的函数。

应用程序可使用 helper 函数将[ `/DELAYLOAD` (延迟加载导入) ](delayload-delay-load-import.md)链接器选项延迟加载 DLL。  (提供由 Microsoft 提供的默认 helper 函数实现。 ) helper 函数在运行时通过调用 `LoadLibrary` 和来加载 DLL。 `GetProcAddress`

如果出现以下情况，请考虑延迟加载 DLL：

- 程序可能不会调用 DLL 中的函数。

- 在程序执行过程中，可能无法调用 DLL 中的函数。

在 EXE 或 DLL 项目的生成过程中，可以指定 DLL 的延迟加载。 如果 DLL 项目延迟加载一个或多个 Dll，则不应在中调用延迟加载的入口点 `DllMain` 。

## <a name="specify-dlls-to-delay-load"></a><a name="specify-dlls-to-delay-load"></a> 指定要延迟加载的 Dll

您可以使用链接器选项指定要延迟加载的 Dll [`/delayload:`*`dllname`*](delayload-delay-load-import.md) 。 如果不打算使用自己的 helper 函数版本，还必须将程序与 *`delayimp.lib`* 桌面应用程序的 (链接) 或 *`dloadhelper.lib`* (UWP 应用) 。

下面是延迟加载 DLL 的简单示例：

```cpp
// cl t.cpp user32.lib delayimp.lib  /link /DELAYLOAD:user32.dll
#include <windows.h>
// uncomment these lines to remove .libs from command line
// #pragma comment(lib, "delayimp")
// #pragma comment(lib, "user32")

int main() {
   // user32.dll will load at this point
   MessageBox(NULL, "Hello", "Hello", MB_OK);
}
```

生成项目的调试版本。 使用调试器逐句通过代码，你会注意 *`user32.dll`* 到只有在调用时才会加载 `MessageBox` 。

## <a name="explicitly-unload-a-delay-loaded-dll"></a><a name="explicitly-unload-a-delay-loaded-dll"></a> 显式卸载延迟加载的 DLL

使用 [`/delay:unload`](delay-delay-load-import-settings.md) 链接器选项，你的代码可以显式卸载延迟加载的 DLL。 默认情况下，延迟加载的导入将保留在导入地址表中 (IAT) 。 但是，如果在 **`/delay:unload`** 链接器命令行中使用，则 helper 函数支持通过调用显式卸载 DLL `__FUnloadDelayLoadedDLL2` ，并将 IAT 重置为其原始形式。 现无效的指针被覆盖。 IAT 是结构中的一个字段 [`ImgDelayDescr`](understanding-the-helper-function.md#calling-conventions-parameters-and-return-type) ，其中包含原始 IAT 的副本的地址（如果存在）。

### <a name="example-of-unloading-a-delay-loaded-dll"></a>卸载延迟加载的 DLL 的示例

此示例演示如何显式卸载包含函数的 DLL *`MyDll.dll`* `fnMyDll` ：

```C
// link with /link /DELAYLOAD:MyDLL.dll /DELAY:UNLOAD
#include <windows.h>
#include <delayimp.h>
#include "MyDll.h"
#include <stdio.h>

#pragma comment(lib, "delayimp")
#pragma comment(lib, "MyDll")
int main()
{
    BOOL TestReturn;
    // MyDLL.DLL will load at this point
    fnMyDll();

    //MyDLL.dll will unload at this point
    TestReturn = __FUnloadDelayLoadedDLL2("MyDll.dll");

    if (TestReturn)
        printf_s("\nDLL was unloaded");
    else
        printf_s("\nDLL was not unloaded");
}
```

有关卸载延迟加载的 DLL 的重要说明：

- 可以在文件中的 `__FUnloadDelayLoadedDLL2` *`delayhlp.cpp`* MSVC 目录中找到函数的实现 *`include`* 。 有关详细信息，请参阅 [了解延迟加载 helper 函数](understanding-the-helper-function.md)。

- *`name`* 函数的参数 `__FUnloadDelayLoadedDLL2` 必须与 (包括) 导入库包含的大小写完全匹配。  (该字符串也位于图像的导入表中。 ) 可以使用查看导入库的 [`DUMPBIN /DEPENDENTS`](dependents.md) 内容。 如果更愿意使用不区分大小写的字符串匹配，可以更新 `__FUnloadDelayLoadedDLL2` 以使用不区分大小写的 CRT 字符串函数或 WINDOWS API 调用之一。

## <a name="bind-delay-loaded-imports"></a><a name="bind-delay-loaded-imports"></a> 绑定延迟加载的导入

默认链接器行为是为延迟加载的 DLL 创建 (IAT) 的可绑定导入地址表。 如果 DLL 是绑定的，则 helper 函数尝试使用绑定的信息，而不是 `GetProcAddress` 对每个引用的导入调用。 如果时间戳或首选地址与加载的 DLL 中的时间戳或首选地址不匹配，则 helper 函数将假定绑定导入地址表过期。 它将继续执行，就好像 IAT 不存在一样。

如果永远不打算绑定 DLL 的延迟加载导入，请 [`/delay:nobind`](delay-delay-load-import-settings.md) 在链接器命令行上指定。 链接器不会生成绑定的导入地址表，这将节省图像文件中的空间。

## <a name="load-all-imports-for-a-delay-loaded-dll"></a><a name="load-all-imports-for-a-delay-loaded-dll"></a> 加载延迟加载的 DLL 的所有导入

`__HrLoadAllImportsForDll`函数是在中定义的 *`delayhlp.cpp`* ，它通知链接器加载使用 [`/delayload`](delayload-delay-load-import.md) 链接器选项指定的 DLL 中的所有导入。

同时加载所有导入时，可以在一个位置集中处理错误处理。 您可以避免对导入的所有实际调用的结构化异常处理。 它还可以避免应用程序在过程中失败的情况，例如，如果帮助程序代码无法加载导入，则在成功加载其他内容后。

调用 `__HrLoadAllImportsForDll` 不会更改挂钩和错误处理的行为。 有关详细信息，请参阅 [错误处理和通知](error-handling-and-notification.md)。

`__HrLoadAllImportsForDll` 对存储在 DLL 本身中的名称进行区分大小写的比较。

下面的示例 `__HrLoadAllImportsForDll` 在名为的函数中使用 `TryDelayLoadAllImports` 来尝试加载已命名的 DLL。 它使用函数 `CheckDelayException` 来确定异常行为。

```C
int CheckDelayException(int exception_value)
{
    if (exception_value == VcppException(ERROR_SEVERITY_ERROR, ERROR_MOD_NOT_FOUND) ||
        exception_value == VcppException(ERROR_SEVERITY_ERROR, ERROR_PROC_NOT_FOUND))
    {
        // This example just executes the handler.
        return EXCEPTION_EXECUTE_HANDLER;
    }
    // Don't attempt to handle other errors
    return EXCEPTION_CONTINUE_SEARCH;
}

bool TryDelayLoadAllImports(LPCSTR szDll)
{
    __try
    {
        HRESULT hr = __HrLoadAllImportsForDll(szDll);
        if (FAILED(hr))
        {
            // printf_s("Failed to delay load functions from %s\n", szDll);
            return false;
        }
    }
    __except (CheckDelayException(GetExceptionCode()))
    {
        // printf_s("Delay load exception for %s\n", szDll);
        return false;
    }
    // printf_s("Delay load completed for %s\n", szDll);
    return true;
}
```

您可以使用的结果 `TryDelayLoadAllImports` 来控制是否调用导入函数。

## <a name="error-handling-and-notification"></a>错误处理和通知

如果程序使用延迟加载的 Dll，则它必须能够可靠地处理错误。 在程序运行期间发生的故障将导致未经处理的异常。 有关 DLL 延迟加载错误处理和通知的详细信息，请参阅 [错误处理和通知](error-handling-and-notification.md)。

## <a name="dump-delay-loaded-imports"></a><a name="dump-delay-loaded-imports"></a> 转储延迟加载的导入

可以使用转储延迟加载的导入 [`DUMPBIN /IMPORTS`](imports-dumpbin.md) 。 这些导入显示的信息与标准导入略有不同。 它们被隔离到列表中其自己的部分 `/imports` ，并显式标记为延迟加载的导入。 如果映像中存在卸载信息，则说明。 如果存在绑定信息，则与导入的绑定地址一起注明目标 DLL 的时间和日期戳。

## <a name="constraints-on-delay-load-dlls"></a><a name="constraints-on-delay-load-dlls"></a> 延迟加载 Dll 的约束

DLL 导入的延迟加载有几个约束。

- 不支持导入数据。 一种解决方法是使用 `LoadLibrary` (或在 [`GetModuleHandle`](/windows/win32/api/libloaderapi/nf-libloaderapi-getmodulehandlew) 知道延迟加载帮助程序已将 DLL 加载到) 和后使用来显式处理数据导入 `GetProcAddress` 。

- *`Kernel32.dll`* 不支持延迟加载。 必须加载此 DLL，延迟加载帮助程序例程才能工作。

- 不支持[绑定](#bind-delay-loaded-imports)已转发的入口点。

- 如果 DLL 延迟加载，则进程可能具有不同的行为，而不是在启动时加载。 如果在延迟加载的 DLL 的入口点发生了每个进程初始化，则会出现此情况。 其他情况包括静态 TLS (线程本地存储) ，使用声明， [`__declspec(thread)`](../../cpp/thread.md) 这在通过加载 DLL 时不会得到处理 `LoadLibrary` 。 使用 [`TlsAlloc`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-tlsalloc) 、、和的动态 TLS [`TlsFree`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-tlsfree) [`TlsGetValue`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-tlsgetvalue) [`TlsSetValue`](/windows/win32/api/processthreadsapi/nf-processthreadsapi-tlssetvalue) 仍可用于静态或延迟加载的 dll。

- 在第一次调用每个函数后，将静态全局函数指针重新初始化为导入的函数。 这是必需的，因为首次使用函数指针将指向 thunk，而不是加载的函数。

- 当前在使用正常导入机制时，没有办法延迟加载 DLL 中的特定过程。

- 自定义调用约定 (如使用 x86 体系结构上的条件代码) 不受支持。 而且，浮点寄存器不会保存在任何平台上。 如果你的自定义帮助程序例程或挂钩例程使用浮点类型，请注意：例程必须保存并还原在对浮点参数使用 register 调用约定的计算机上完成的浮点状态。 请注意延迟加载 CRT DLL 的情况，尤其是在调用在 NDP 中使用浮点参数的 CRT 函数时，在帮助函数中 () stack。

## <a name="understand-the-delay-load-helper-function"></a>了解针对延迟加载的 helper 函数

链接器支持的延迟加载的 helper 函数是实际在运行时加载 DLL 的内容。 您可以修改 helper 函数以自定义其行为。 *`delayimp.lib`* 编写您自己的函数并将其链接到您的程序，而不是使用中提供的 helper 函数。 一个 helper 函数提供所有延迟加载的 Dll。 有关详细信息，请参阅 [了解延迟加载 helper 函数](understanding-the-helper-function.md) 和 [开发自己的 helper 函数](understanding-the-helper-function.md#develop-your-own-helper-function)。

## <a name="see-also"></a>请参阅

[在 Visual Studio 中创建 C/C++ DLL](../dlls-in-visual-cpp.md)<br/>
[MSVC 链接器参考](linking.md)
