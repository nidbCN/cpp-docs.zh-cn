---
title: _CrtMemDifference
description: 了解详细信息： _CrtMemDifference
ms.date: 3/8/2021
api_name:
- _CrtMemDifference
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
- _CrtMemDifference
- CrtMemDifference
helpviewer_keywords:
- CrtMemDifference function
- _CrtMemDifference function
ms.openlocfilehash: 9a6a213df867f98bfb921abd940ba23297c5ffef
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514598"
---
# `_CrtMemDifference`

比较两个内存状态，并返回它们的差异（仅限调试版本）。

## <a name="syntax"></a>语法

```C
int _CrtMemDifference(
   _CrtMemState *stateDiff,
   const _CrtMemState *oldState,
   const _CrtMemState *newState
);
```

### <a name="parameters"></a>参数

*`stateDiff`*\
指向 **`_CrtMemState`** 结构的指针，该结构用于存储两个内存状态之间的差异 (返回) 。

*`oldState`*\
指向较早内存状态的指针 (**`_CrtMemState`** 结构) 。

*`newState`*\
指向更高内存状态的指针 (**`_CrtMemState`** 结构) 。

## <a name="return-value"></a>返回值

如果内存状态明显不同，则 **`_CrtMemDifference`** 返回 `TRUE` 。 否则，此函数返回 FALSE。

## <a name="remarks"></a>注解

**`_CrtMemDifference`** 函数比较 *`oldState`* 并 *`newState`* 存储它们在中的差异 *`stateDiff`* ，应用程序随后可以使用这些差异来检测内存泄漏和其他内存问题。 未定义 [_DEBUG](../../c-runtime-library/debug.md) 时，将 **`_CrtMemDifference`** 在预处理过程中删除对的调用。

*`newState`* 和都 *`oldState`* 必须是指向 **`_CrtMemState`** 在 crtdbg.h 中定义的结构的有效指针，该结构在调用之前已由填充 [`_CrtMemCheckpoint`](crtmemcheckpoint.md) **`_CrtMemDifference`** 。 *`stateDiff`* 必须是指向结构的以前分配的实例的指针 **`_CrtMemState`** 。 如果 *`stateDiff`* 、 *`newState`* 或 *`oldState`* 为 **`NULL`** ，则调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续， [`errno, _doserrno, _sys_errlist, and _sys_nerr`](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md) 则将设置为 **`EINVAL`** ，并且该函数将返回 FALSE。

**`_CrtMemDifference`** 将 **`_CrtMemState`** 中块的字段值与中的值进行比较 *`oldState`* *`newState`* ，并将结果存储在中 *`stateDiff`* 。 当这两个内存状态的已分配块类型数目或各类型的已分配块数不同时，将这两个状态称为大不相同。 同时为两个状态分配的最大数量与这两个状态的总分配数之间的差异也存储在中 *`stateDiff`* 。

默认情况下， **`_CRT_BLOCK`** 内存状态操作中不包含内部 C 运行时块 () 。 [_CrtSetDbgFlag](crtsetdbgflag.md)函数可用来打开的 **`_CRTDBG_CHECK_CRT_DF`** 位 **`_crtDbgFlag`** ，以将这些块包含在泄漏检测和其他内存状态操作中。 释放内存块 (**`_FREE_BLOCK`**) 不会导致 **`_CrtMemDifference`** 返回 `TRUE` 。

有关堆状态函数和结构的详细信息 **`_CrtMemState`** ，请参阅 [堆状态报告函数](/visualstudio/debugger/crt-debug-heap-details)。 若要了解如何在基堆的调试版本中分配、初始化和管理内存块，请参阅 [CRT Debug Heap Details](/visualstudio/debugger/crt-debug-heap-details)。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|---------------------|
|**`_CrtMemDifference`**|`<crtdbg.h>`|`<errno.h>`|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

**库：** 仅限 [C 运行时库](../../c-runtime-library/crt-library-features.md) 的调试版本。

## <a name="see-also"></a>另请参阅

[调试例程](../../c-runtime-library/debug-routines.md)\
[`_crtDbgFlag`](../../c-runtime-library/crtdbgflag.md)\
