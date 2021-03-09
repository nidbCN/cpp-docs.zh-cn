---
description: 了解详细信息： _CrtMemDumpStatistics
title: _CrtMemDumpStatistics
ms.date: 11/04/2016
api_name:
- _CrtMemDumpStatistics
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
- CrtMemDumpStatistics
- _CrtMemDumpStatistics
helpviewer_keywords:
- _CrtMemDumpStatistics function
- CrtMemDumpStatistics function
ms.assetid: 27b9d731-3184-4a2d-b9a7-6566ab28a9fe
ms.openlocfilehash: 684b5ac50af453dbe2800ded595c5747370717a6
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514585"
---
# <a name="_crtmemdumpstatistics"></a>_CrtMemDumpStatistics

以用户可读的形式转储指定堆状态的调试标头信息（仅限调试版本）。

## <a name="syntax"></a>语法

```C
void _CrtMemDumpStatistics(
   const _CrtMemState *state
);
```

### <a name="parameters"></a>参数

State <br/>
指向要转储的堆状态的指针。

## <a name="remarks"></a>注解

**_CrtMemDumpStatistics** 函数以用户可读的形式转储指定堆状态的调试标头信息。 应用程序可以使用转储统计信息来跟踪分配并检测内存问题。 内存状态可以包含特定的堆状态或两个状态之间的差异。 未定义 [_DEBUG](../../c-runtime-library/debug.md) 时，将在预处理过程中删除对 **_CrtMemDumpStatistics** 的调用。

*State* 参数必须是指向 **_CrtMemState** 结构的指针，该结构已通过 [_CrtMemCheckpoint](crtmemcheckpoint.md)或在调用 **_CrtMemDumpStatistics** 之前由 [_CrtMemDifference](crtmemdifference.md)返回。 如果 *state* 为 **NULL**，则调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则将 **errno** 设置为 **EINVAL** ，并且不执行任何操作。 有关详细信息，请参阅 [errno、_doserrno、_sys_errlist 和 _sys_nerr](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

有关堆状态函数和 **_CrtMemState** 结构的详细信息，请参阅 [堆状态报告函数](/visualstudio/debugger/crt-debug-heap-details)。 有关如何在基堆的调试版本中分配、初始化和管理内存块的详细信息，请参阅 [CRT Debug Heap Details](/visualstudio/debugger/crt-debug-heap-details)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|----------------------|
|**_CrtMemDumpStatistics**|\<crtdbg.h>|\<errno.h>|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

**库：** 仅限 [C 运行时库](../../c-runtime-library/crt-library-features.md) 的调试版本。

## <a name="see-also"></a>另请参阅

[调试例程](../../c-runtime-library/debug-routines.md)<br/>
