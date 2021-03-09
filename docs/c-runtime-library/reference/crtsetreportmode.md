---
title: _CrtSetReportMode
description: 了解详细信息： _CrtSetReportMode
ms.date: 3/8/2021
api_name:
- _CrtSetReportMode
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
- _CrtSetReportMode
- CrtSetReportMode
helpviewer_keywords:
- _CrtSetReportMode function
- CrtSetReportMode function
ms.openlocfilehash: 770ef955894563dc11dee9f13946067d5d024c11
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514377"
---
# `_CrtSetReportMode`

指定由 **_CrtDbgReport** 生成的特定报表类型的目标或目标，以及任何调用的宏， [`_CrtDbgReport, _CrtDbgReportW`](crtdbgreport-crtdbgreportw.md) 如 (仅调试版本) 的 [ `_ASSERT, _ASSERTE, _ASSERT_EXPR` 宏、](assert-asserte-assert-expr-macros.md) [ `_ASSERT, _ASSERTE, _ASSERT_EXPR` 宏](assert-asserte-assert-expr-macros.md)、 [ `_RPT, _RPTF, _RPTW, _RPTFW` 宏](rpt-rptf-rptw-rptfw-macros.md)和 [ `_RPT, _RPTF, _RPTW, _RPTFW` 宏](rpt-rptf-rptw-rptfw-macros.md)。

## <a name="syntax"></a>语法

```C
int _CrtSetReportMode(
   int reportType,
   int reportMode
);
```

### <a name="parameters"></a>参数

*`reportType`*\
报表类型： **`_CRT_WARN`** 、 **`_CRT_ERROR`** 和 **`_CRT_ASSERT`** 。

*`reportMode`*\
的新报表模式或模式 *`reportType`* 。

## <a name="return-value"></a>返回值

成功完成后， **`_CrtSetReportMode`** 将返回中指定的报表类型的前一报表模式或模式 *`reportType`* 。 如果传入的值无效 *`reportType`* 或为指定了无效的模式，则将 *`reportMode`* **`_CrtSetReportMode`** 调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果允许执行继续，则此函数将设置 **`errno`** 为 **`EINVAL`** 并返回-1。 有关详细信息，请参阅 [`errno, _doserrno, _sys_errlist, and _sys_nerr`](../../c-runtime-library/errno-doserrno-sys-errlist-and-sys-nerr.md)。

## <a name="remarks"></a>注解

**`_CrtSetReportMode`** 指定的输出目标 **`_CrtDbgReport`** 。 由于宏 [`_ASSERT`](assert-asserte-assert-expr-macros.md) 、、 [`_ASSERTE`](assert-asserte-assert-expr-macros.md) [`_RPT`](rpt-rptf-rptw-rptfw-macros.md) 和调用，因此指定 [`_RPTF`](rpt-rptf-rptw-rptfw-macros.md) **`_CrtDbgReport`** **`_CrtSetReportMode`** 了与这些宏一起指定的文本的输出目标。

当 [`_DEBUG`](../../c-runtime-library/debug.md) 未定义时，将 **`_CrtSetReportMode`** 在预处理过程中删除对的调用。

如果不调用 **`_CrtSetReportMode`** 来定义消息的输出目标，则下列默认值有效：

- 断言失败和错误会定向到调试消息窗口。

- 警告从 Windows 应用程序发送到调试器的输出窗口。

- 不显示控制台应用程序中的警告。

下表列出了在中定义的报表类型 `Crtdbg.h` 。

|报告类型|说明|
|-----------------|-----------------|
|**`_CRT_WARN`**|不需要立即关注的警告、消息和信息。|
|**`_CRT_ERROR`**|错误、不可恢复的问题和需要立即关注的问题。|
|**`_CRT_ASSERT`**|计算结果为)  (断言表达式 **`FALSE`** 。|

**`_CrtSetReportMode`** 函数将在中指定的新报表模式分配 *`reportMode`* 给在中指定的报表类型 *`reportType`* ，并返回以前为定义的报表模式 *`reportType`* 。 下表列出了的可用选项 *`reportMode`* 和的结果行为 **`_CrtDbgReport`** 。 在 Crtdbg.h 中将这些选项定义为位标志。

|报告模式|_CrtDbgReport 行为|
|-----------------|-----------------------------|
|**`_CRTDBG_MODE_DEBUG`**|将消息写入调试器的输出窗口。|
|**`_CRTDBG_MODE_FILE`**|将消息写入用户提供的文件句柄。 [`_CrtSetReportFile`](crtsetreportfile.md) 应调用以定义要用作目标的特定文件或流。|
|**`_CRTDBG_MODE_WNDW`**|创建一个消息框，用于显示消息以及 "" [`abort`](abort.md) **`Retry`** 和 " **忽略** " 按钮。|
|**`_CRTDBG_REPORT_MODE`**|返回 *`reportMode`* 指定的 *`reportType`* ：<br /><br /> 2   **`_CRTDBG_MODE_FILE`**<br /><br /> pps-2   **`_CRTDBG_MODE_DEBUG`**<br /><br /> 4   **`_CRTDBG_MODE_WNDW`**|

可以使用一种、两种或三种模式，或不使用任何模式报告每种报告类型。 因此，可以为单个报告类型定义多个目标。 例如，以下代码片段将导致断言失败同时发送到调试消息窗口和 **`stderr`** ：

```C
_CrtSetReportMode( _CRT_ASSERT, _CRTDBG_MODE_FILE | _CRTDBG_MODE_WNDW );
_CrtSetReportFile( _CRT_ASSERT, _CRTDBG_FILE_STDERR );
```

此外，可以分别控制报告模式和每个报告类型的模式。 例如，可以指定将 *`reportType`* **`_CRT_WARN`** 发送到输出调试字符串， **`_CRT_ASSERT`** 并使用调试消息窗口显示，并将其发送到 **`stderr`** ，如前文所述。

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|---------------------|
|**`_CrtSetReportMode`**|`<crtdbg.h>`|`<errno.h>`|

有关兼容性的详细信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

**库：** 仅限 [C 运行时库](../../c-runtime-library/crt-library-features.md) 的调试版本。

## <a name="see-also"></a>另请参阅

[调试例程](../../c-runtime-library/debug-routines.md)
