---
title: 错误处理和通知
description: 了解更多： DLL 延迟加载错误处理和通知
ms.date: 01/19/2021
helpviewer_keywords:
- error handling, and notification
ms.openlocfilehash: efff7ba9956bee8fe6ccf1df96a3f4b49852ce43
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522555"
---
# <a name="error-handling-and-notification"></a>错误处理和通知

如果程序使用延迟加载的 Dll，它必须能够可靠地处理错误，因为在程序运行期间发生的失败将导致未经处理的异常。 故障处理由两部分组成：通过挂钩恢复，并通过异常进行报告。

有关 DLL 延迟加载错误处理和通知的详细信息，请参阅 [了解 helper 函数](understanding-the-helper-function.md)。

有关挂钩函数的详细信息，请参阅 [结构和常量定义](understanding-the-helper-function.md#structure-and-constant-definitions)。

## <a name="recovery-through-a-hook"></a>通过挂钩恢复

你的代码可能需要在出现故障时恢复，或者需要提供备用库或例程。 可以向 helper 函数提供挂钩，该函数可提供备用代码或解决该问题。 挂钩例程需要返回合适的值，以便处理可以继续 (`HINSTANCE` 或 `FARPROC`) 。 或者，它可以返回0以指示应引发异常。 它还可能会引发自己的异常或 `longjmp` 超出挂钩。 存在通知挂钩和失败挂钩。 可以同时使用同一例程。

## <a name="notification-hooks"></a><a name="notification-hooks"></a> 通知挂钩

在帮助程序例程中执行以下操作之前，将调用延迟加载通知挂钩：

- 系统将检查库的存储句柄，以查看其是否已加载。

- `LoadLibrary` 调用以尝试加载 DLL。

- `GetProcAddress` 调用以尝试获取过程的地址。

- 返回延迟导入加载 thunk。

通知挂钩已启用：

- 通过提供已初始化的指针的新定义， `__pfnDliNotifyHook2` 使其指向接收通知的自己的函数。

   \- 或 -

- 在对 `__pfnDliNotifyHook2` 程序进行延迟加载的任何调用之前，将指针设置为挂钩函数。

如果通知为 `dliStartProcessing` ，则挂钩函数可以返回：

- `NULL`

   默认 helper 处理 DLL 的加载。 仅出于信息目的而调用是非常有用的。

- 函数指针

   绕过默认的延迟负载处理。 它允许你提供自己的负载处理程序。

如果通知为 `dliNotePreLoadLibrary` ，则挂钩函数可以返回：

- 如果只需要信息性通知，则为0。

- `HMODULE`加载的 dll 的（如果它加载了 dll 本身）。

如果通知为 `dliNotePreGetProcAddress` ，则挂钩函数可以返回：

- 如果只需要信息性通知，则为0。

- 导入函数的地址（如果挂钩函数获取地址本身）。

如果通知为 `dliNoteEndProcessing` ，则忽略挂钩函数的返回值。

如果此指针初始化 (非零) ，延迟加载帮助器将在执行过程中的某些通知点调用函数。 函数指针具有以下定义：

```C
// The "notify hook" gets called for every call to the
// delay load helper.  This allows a user to hook every call and
// skip the delay load helper entirely.
//
// dliNotify == {
//  dliStartProcessing |
//  dliNotePreLoadLibrary  |
//  dliNotePreGetProc |
//  dliNoteEndProcessing}
//  on this call.
//
ExternC
PfnDliHook   __pfnDliNotifyHook2;

// This is the failure hook, dliNotify = {dliFailLoadLib|dliFailGetProc}
ExternC
PfnDliHook   __pfnDliFailureHook2;
```

通知会将结构中的传递 `DelayLoadInfo` 给挂钩函数以及通知值。 此数据与 "延迟加载帮助程序" 例程使用的数据相同。 通知值将是在 [结构和常量定义](understanding-the-helper-function.md#structure-and-constant-definitions)中定义的值之一。

## <a name="failure-hooks"></a><a name="failure-hooks"></a> 失败挂钩

启用失败挂钩的方式与 [通知挂钩](#notification-hooks)相同。 挂钩例程需要返回合适的值，以便处理可以继续 (`HINSTANCE` 或 `FARPROC`) ，或为0以指示应引发异常。

引用用户定义函数的指针变量是：

```C
// This is the failure hook, dliNotify = {dliFailLoadLib|dliFailGetProc}
ExternC
PfnDliHook   __pfnDliFailureHook2;
```

**`DelayLoadInfo`** 结构包含错误详细报告所需的所有相关数据，其中包括中的值 `GetLastError` 。

如果通知为 **`dliFailLoadLib`** ，则挂钩函数可以返回：

- 如果无法处理失败，则为0。

- `HMODULE`如果故障挂接修复了问题并加载了库本身，则为。

如果通知为 **`dliFailGetProc`** ，则挂钩函数可以返回：

- 如果无法处理失败，则为0。

- 有效的过程地址 (导入函数地址) （如果失败挂钩已成功获取地址本身）。

## <a name="report-by-using-an-exception"></a>使用异常报告

如果处理错误所需的全部操作都是中止过程，则只要用户代码可以处理异常，就不需要挂钩。

## <a name="delay-load-exception-codes"></a><a name="delay-load-exception-codes"></a> 延迟加载异常代码

在延迟加载过程中发生失败时，可能会引发结构化异常代码。 使用宏指定异常值 `VcppException` ：

```C
//
// Exception information
//
#define FACILITY_VISUALCPP  ((LONG)0x6d)
#define VcppException(sev,err)  ((sev) | (FACILITY_VISUALCPP<<16) | err)
```

`LoadLibrary`如果失败，将引发标准 `VcppException(ERROR_SEVERITY_ERROR, ERROR_MOD_NOT_FOUND)` 。 对于 `GetProcAddress` 失败，引发的错误为 `VcppException(ERROR_SEVERITY_ERROR, ERROR_PROC_NOT_FOUND)` 。 异常传递指向结构的指针 `DelayLoadInfo` 。 它位于 `LPDWORD` 从结构中的检索到的值 `GetExceptionInformation` [`EXCEPTION_RECORD`](/windows/win32/api/winnt/ns-winnt-exception_record) （在字段中） `ExceptionInformation[0]` 。

如果在字段中设置了不正确的位 `grAttrs` ，则 `ERROR_INVALID_PARAMETER` 会引发异常。 此例外是出于所有意图和目的，都是致命的。

有关详细信息，请参阅 [结构和常量定义](understanding-the-helper-function.md#structure-and-constant-definitions)。

## <a name="see-also"></a>另请参阅

[链接器的延迟加载 DLL 支持](linker-support-for-delay-loaded-dlls.md)
