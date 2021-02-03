---
description: 了解有关延迟加载 helper 函数的详细信息
title: 了解针对延迟加载的 helper 函数
ms.date: 01/19/2021
helpviewer_keywords:
- delayed loading of DLLs, helper function
- __delayLoadHelper2 function
- delayimp.lib
- __delayLoadHelper function
- delayhlp.cpp
- delayimp.h
- helper functions
ms.openlocfilehash: 6108e896b6d7a370f2b4d6ab8f5baa880112a21e
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522737"
---
# <a name="understand-the-delay-load-helper-function"></a>了解针对延迟加载的 helper 函数

链接器支持的延迟加载的 helper 函数是实际在运行时加载 DLL 的内容。 您可以修改 helper 函数以自定义其行为。 *`delayimp.lib`* 编写您自己的函数并将其链接到您的程序，而不是使用中提供的 helper 函数。 一个 helper 函数提供所有延迟加载的 Dll。

如果要基于 DLL 或导入的名称进行特定处理，则可以提供自己的 helper 函数版本。

Helper 函数会执行以下操作：

- 检查库的存储句柄，查看其是否已加载

- 调用 `LoadLibrary` 以尝试加载 DLL

- 调用 `GetProcAddress` 以尝试获取过程的地址

- 返回到延迟导入加载 thunk 以调用现在加载的入口点

在以下每个操作后，helper 函数可以回调到程序中的通知挂钩：

- Helper 函数启动时

- 紧靠 `LoadLibrary` 在 helper 函数中调用之前

- 紧靠 `GetProcAddress` 在 helper 函数中调用之前

- 如果 `LoadLibrary` helper 函数中的调用失败

- 如果 `GetProcAddress` helper 函数中的调用失败

- 完成处理 helper 函数后

其中每个挂钩点都可以返回一个值，该值将以某种方式更改 helper 例程的正常处理，但返回延迟导入负载 thunk 除外。

可以在 *`delayhlp.cpp`* 和 *`delayimp.h`* 的 MSVC 目录中找到默认帮助程序代码 *`include`* 。 它 *`delayimp.lib`* 在 *`lib`* 目标体系结构的 MSVC 目录中进行编译。 你需要在编译中包含此库，除非你编写自己的 helper 函数。

## <a name="delay-load-helper-calling-conventions-parameters-and-return-type"></a><a name="calling-conventions-parameters-and-return-type"></a> 延迟加载助手调用约定、参数和返回类型

延迟加载 helper 例程的原型是：

```C
FARPROC WINAPI __delayLoadHelper2(
    PCImgDelayDescr pidd,
    FARPROC * ppfnIATEntry
);
```

### <a name="parameters"></a>参数

*`pidd`*\
**`const`** 指向的指针， `ImgDelayDescr` 该指针包含各种导入相关数据的偏移量、绑定信息的时间戳，以及提供有关描述符内容的进一步信息的一组属性。 目前只有一个属性，该属性 `dlattrRva` 指示描述符中的地址是相对虚拟地址。 有关详细信息，请参阅中的声明 *`delayimp.h`* 。

延迟描述符中 (的指针 `ImgDelayDescr` *`delayimp.h`*) 使用相对虚拟地址 (rva) 在32位和64位程序中按预期方式工作。 若要使用它们，请使用中的函数将这些 Rva 转换回指针 `PFromRva` *`delayhlp.cpp`* 。 可以对描述符中的每个字段使用此函数，以将其转换回32位或64位指针。 默认延迟加载 helper 函数是一个用作示例的好模板。

有关结构的定义 `PCImgDelayDescr` ，请参阅 [结构和常量定义](#structure-and-constant-definitions)。

*`ppfnIATEntry`*\
指向延迟加载导入地址表中的槽的指针 (IAT) 。 这是用导入函数的地址更新的槽。 Helper 例程需要存储返回到此位置的相同值。

### <a name="expected-return-values"></a>预期的返回值

如果 helper 函数成功，则它将返回导入函数的地址。

如果函数失败，则会引发结构化异常并返回0。 引发的异常有三种类型：

- 参数无效，如果未正确指定中的属性，则会发生这种情况 *`pidd`* 。 将其视为不可恢复的错误。

- `LoadLibrary` 在指定的 DLL 上失败。

- `GetProcAddress` 失败。

您负责处理这些异常。 有关详细信息，请参阅 [错误处理和通知](error-handling-and-notification.md)。

### <a name="remarks"></a>备注

Helper 函数的调用约定是 **`__stdcall`** 。 返回值的类型不相关，因此 `FARPROC` 使用。 此函数具有 C 链接，这意味着它需要 `extern "C"` 在 c + + 代码中声明时进行包装。 `ExternC`此宏会为你处理此包装器。

若要使用 helper 例程作为通知挂钩，你的代码必须指定要返回的相应函数指针。 然后，链接器生成的 thunk 代码可将该返回值用作导入的实际目标并直接跳转到该目标。 如果不想使用 helper 例程作为通知挂钩，请将 helper 函数的返回值存储在中 `ppfnIATEntry` ，传入的函数指针位置。

## <a name="sample-hook-function"></a>示例挂钩函数

下面的代码演示如何实现基本挂钩函数。

```C
FARPROC WINAPI delayHook(unsigned dliNotify, PDelayLoadInfo pdli)
{
    switch (dliNotify) {
        case dliStartProcessing :

            // If you want to return control to the helper, return 0.
            // Otherwise, return a pointer to a FARPROC helper function
            // that will be used instead, thereby bypassing the rest
            // of the helper.

            break;

        case dliNotePreLoadLibrary :

            // If you want to return control to the helper, return 0.
            // Otherwise, return your own HMODULE to be used by the
            // helper instead of having it call LoadLibrary itself.

            break;

        case dliNotePreGetProcAddress :

            // If you want to return control to the helper, return 0.
            // If you choose you may supply your own FARPROC function
            // address and bypass the helper's call to GetProcAddress.

            break;

        case dliFailLoadLib :

            // LoadLibrary failed.
            // If you don't want to handle this failure yourself, return 0.
            // In this case the helper will raise an exception
            // (ERROR_MOD_NOT_FOUND) and exit.
            // If you want to handle the failure by loading an alternate
            // DLL (for example), then return the HMODULE for
            // the alternate DLL. The helper will continue execution with
            // this alternate DLL and attempt to find the
            // requested entrypoint via GetProcAddress.

            break;

        case dliFailGetProc :

            // GetProcAddress failed.
            // If you don't want to handle this failure yourself, return 0.
            // In this case the helper will raise an exception
            // (ERROR_PROC_NOT_FOUND) and exit.
            // If you choose, you may handle the failure by returning
            // an alternate FARPROC function address.

            break;

        case dliNoteEndProcessing :

            // This notification is called after all processing is done.
            // There is no opportunity for modifying the helper's behavior
            // at this point except by longjmp()/throw()/RaiseException.
            // No return value is processed.

            break;

        default :

            return NULL;
    }

    return NULL;
}

/*
and then at global scope somewhere:

ExternC const PfnDliHook __pfnDliNotifyHook2 = delayHook;
ExternC const PfnDliHook __pfnDliFailureHook2 = delayHook;
*/
```

## <a name="delay-load-structure-and-constant-definitions"></a><a name="structure-and-constant-definitions"></a> 延迟加载结构和常量定义

默认的延迟加载 helper 例程使用几个结构来与挂钩函数和任何异常进行通信。 这些结构是在中定义的 *`delayimp.h`* 。 下面是传递到挂钩的宏、typedef、通知和失败值、信息结构和指向挂钩的指针函数类型：

```C
#define _DELAY_IMP_VER  2

#if defined(__cplusplus)
#define ExternC extern "C"
#else
#define ExternC extern
#endif

typedef IMAGE_THUNK_DATA *          PImgThunkData;
typedef const IMAGE_THUNK_DATA *    PCImgThunkData;
typedef DWORD                       RVA;

typedef struct ImgDelayDescr {
    DWORD           grAttrs;        // attributes
    RVA             rvaDLLName;     // RVA to dll name
    RVA             rvaHmod;        // RVA of module handle
    RVA             rvaIAT;         // RVA of the IAT
    RVA             rvaINT;         // RVA of the INT
    RVA             rvaBoundIAT;    // RVA of the optional bound IAT
    RVA             rvaUnloadIAT;   // RVA of optional copy of original IAT
    DWORD           dwTimeStamp;    // 0 if not bound,
                                    // O.W. date/time stamp of DLL bound to (Old BIND)
    } ImgDelayDescr, * PImgDelayDescr;

typedef const ImgDelayDescr *   PCImgDelayDescr;

enum DLAttr {                   // Delay Load Attributes
    dlattrRva = 0x1,                // RVAs are used instead of pointers
                                    // Having this set indicates a VC7.0
                                    // and above delay load descriptor.
    };

//
// Delay load import hook notifications
//
enum {
    dliStartProcessing,             // used to bypass or note helper only
    dliNoteStartProcessing = dliStartProcessing,

    dliNotePreLoadLibrary,          // called just before LoadLibrary, can
                                    //  override w/ new HMODULE return val
    dliNotePreGetProcAddress,       // called just before GetProcAddress, can
                                    //  override w/ new FARPROC return value
    dliFailLoadLib,                 // failed to load library, fix it by
                                    //  returning a valid HMODULE
    dliFailGetProc,                 // failed to get proc address, fix it by
                                    //  returning a valid FARPROC
    dliNoteEndProcessing,           // called after all processing is done, no
                                    //  bypass possible at this point except
                                    //  by longjmp()/throw()/RaiseException.
    };

typedef struct DelayLoadProc {
    BOOL                fImportByName;
    union {
        LPCSTR          szProcName;
        DWORD           dwOrdinal;
        };
    } DelayLoadProc;

typedef struct DelayLoadInfo {
    DWORD               cb;         // size of structure
    PCImgDelayDescr     pidd;       // raw form of data (everything is there)
    FARPROC *           ppfn;       // points to address of function to load
    LPCSTR              szDll;      // name of dll
    DelayLoadProc       dlp;        // name or ordinal of procedure
    HMODULE             hmodCur;    // the hInstance of the library we have loaded
    FARPROC             pfnCur;     // the actual function that will be called
    DWORD               dwLastError;// error received (if an error notification)
    } DelayLoadInfo, * PDelayLoadInfo;

typedef FARPROC (WINAPI *PfnDliHook)(
    unsigned        dliNotify,
    PDelayLoadInfo  pdli
    );
```

## <a name="calculate-necessary-values-for-delay-loading"></a><a name="calculate-necessary-values"></a> 计算延迟加载所需的值

延迟加载 helper 例程需要计算两项关键信息。 为了帮助，中有两个 *`delayhlp.cpp`* 用于计算此信息的内联函数。

- 第一种是， `IndexFromPImgThunkData` 计算当前导入到三个不同表中的索引 (导入地址表 (IAT) 、绑定导入地址表 (BIAT) 和未绑定导入地址表 (UIAT) # A7。

- 第二个对 `CountOfImports` 有效 IAT 中的导入次数进行计数。

```C
// utility function for calculating the index of the current import
// for all the tables (INT, BIAT, UIAT, and IAT).
__inline unsigned
IndexFromPImgThunkData(PCImgThunkData pitdCur, PCImgThunkData pitdBase) {
    return pitdCur - pitdBase;
    }

// utility function for calculating the count of imports given the base
// of the IAT. NB: this only works on a valid IAT!
__inline unsigned
CountOfImports(PCImgThunkData pitdBase) {
    unsigned        cRet = 0;
    PCImgThunkData  pitd = pitdBase;
    while (pitd->u1.Function) {
        pitd++;
        cRet++;
        }
    return cRet;
    }
```

## <a name="support-unload-of-a-delay-loaded-dll"></a><a name="unload-a-delay-loaded-dll"></a> 支持卸载延迟加载的 DLL

加载延迟加载的 DLL 时，默认的延迟加载帮助程序会检查延迟加载描述符是否有一个指针，以及该字段中 (IAT) 的原始导入地址表的副本 `pUnloadIAT` 。 如果是这样，则帮助器将列表中的指针保存到导入延迟描述符。 此项允许 helper 函数按名称查找 DLL，以支持显式卸载该 DLL。

下面是用于显式卸载延迟加载的 DLL 的关联结构和函数：

```cpp
//
// Unload support from delayimp.h
//

// routine definition; takes a pointer to a name to unload

ExternC
BOOL WINAPI
__FUnloadDelayLoadedDLL2(LPCSTR szDll);

// structure definitions for the list of unload records
typedef struct UnloadInfo * PUnloadInfo;
typedef struct UnloadInfo {
    PUnloadInfo     puiNext;
    PCImgDelayDescr pidd;
    } UnloadInfo;

// from delayhlp.cpp
// the default delay load helper places the unloadinfo records in the
// list headed by the following pointer.
ExternC
PUnloadInfo __puiHead;
```

`UnloadInfo`结构是使用 `LocalAlloc` 和实现分别使用和实现的 c + + 类实现的 `LocalFree` `operator new` `operator delete` 。 这些选项保留在一个标准链接列表中，该列表使用 `__puiHead` 作为列表的头。

当你调用时 `__FUnloadDelayLoadedDLL` ，它会尝试查找你在加载的 dll 列表中提供的名称。  (需要完全匹配。 ) 如果找到，则中 IAT 的副本将 `pUnloadIAT` 复制到正在运行的 iat 顶部，以还原 thunk 指针。 然后，将使用释放库 `FreeLibrary` ，匹配的记录与 `UnloadInfo` 列表取消链接并删除，并 `TRUE` 返回。

函数的参数 `__FUnloadDelayLoadedDLL2` 区分大小写。 例如，你可以指定：

```cpp
__FUnloadDelayLoadedDLL2("user32.dll");
```

而不是：

```cpp
__FUnloadDelayLoadedDLL2("User32.DLL");
```

有关卸载延迟加载的 DLL 的示例，请参阅 [显式卸载延迟加载的 dll](linker-support-for-delay-loaded-dlls.md)。

## <a name="develop-your-own-delay-load-helper-function"></a><a name="develop-your-own-helper-function"></a> 开发您自己的延迟加载 helper 函数

你可能需要提供自己的延迟加载帮助程序例程的版本。 在您自己的例程中，您可以根据 DLL 的名称或导入执行特定的处理。 可以通过两种方式插入自己的代码：编写自己的 helper 函数代码，可能基于提供的代码。 或者，使用 [通知挂钩](error-handling-and-notification.md#notification-hooks)挂钩提供的帮助程序调用自己的函数。

### <a name="code-your-own-helper"></a>编写自己的帮助器

创建自己的帮助程序例程非常简单。 您可以使用现有代码作为新函数的指南。 函数必须使用与现有 helper 相同的调用约定。 而且，如果它返回到链接器生成的 thunk，则它必须返回适当的函数指针。 创建代码后，你可以根据需要满足调用或从调用中取出。

### <a name="use-the-start-processing-notification-hook"></a>使用启动处理通知挂钩

最简单的方法是提供一个新的指针，指向用户提供的通知挂钩函数，该函数采用与通知的默认 helper 相同的值 `dliStartProcessing` 。 此时，挂钩函数可能会成为新的 helper 函数，因为成功返回到默认帮助器会绕过默认帮助器中的所有进一步处理。

## <a name="see-also"></a>另请参阅

[链接器的延迟加载 DLL 支持](linker-support-for-delay-loaded-dlls.md#explicitly-unload-a-delay-loaded-dll)
