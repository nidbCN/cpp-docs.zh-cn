---
description: 了解详细信息：链接选项
title: 链接选项
ms.date: 11/04/2016
helpviewer_keywords:
- nothrownew.obj
- newmode.obj
- noenv.obj
- psetargv.obj
- legacy_stdio_float_rounding.obj
- loosefpmath.obj
- smallheap.obj
- fp10.obj
- nochkclr.obj
- chkstk.obj
- pcommode.obj
- pnoenv.obj
- link options [C++]
- invalidcontinue.obj
- pnothrownew.obj
- pwsetargv.obj
- pinvalidcontinue.obj
- wsetargv.obj
- binmode.obj
- setargv.obj
- noarg.obj
- pnewmode.obj
- commode.obj
- pthreadlocale.obj
- pbinmode.obj
- threadlocale.obj
- pnoarg.obj
ms.assetid: 05b5a77b-9dd1-494b-ae46-314598c770bb
ms.openlocfilehash: 3fce62718518138303900be379458dce950acfbe
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514390"
---
# <a name="link-options"></a>链接选项

CRT lib 目录中包括大量无需更改任何代码就能启用特定的 CRT 功能的小型对象文件。 这些文件被称为“链接选项”，因为只需将它们添加到链接器命令行就能使用。

这些对象的 CLR 纯模式版本在 Visual Studio 2015 中已弃用并在 Visual Studio 2017 中不受支持。 将常规版本用于本机和 /clr 代码。

|本机和 /clr|纯模式|说明|
|----------------------|---------------|-----------------|
|binmode.obj|pbinmode.obj|为二进制设置默认文件转换模式。 请参阅 [_fmode](../c-runtime-library/fmode.md)。|
|chkstk.obj|不适用|在未使用 CRT 时提供堆栈检查和分配支持。|
|commode.obj|pcommode.obj|设置用于“提交”的全局提交标志。 请参阅 [fopen、_wfopen](../c-runtime-library/reference/fopen-wfopen.md) 和 [fopen_s、_wfopen_s](../c-runtime-library/reference/fopen-s-wfopen-s.md)。|
|exe_initialize_mta.lib|不适用|在 EXE 启动期间初始化 MTA 单元，以便在全局智能指针中使用 COM 对象。 因为此选项在关闭期间会泄漏 MTA 单元引用，所以请不要将其用于 DLL。 链接到此选项等效于包括 combase.h 和定义 _EXE_INITIALIZE_MTA。 |
|fp10.obj|不适用|将默认精度控制更改为 64 位。 请参阅[浮点支持](../c-runtime-library/floating-point-support.md)。|
|invalidcontinue.obj|pinvalidcontinue.obj|设置不执行任何操作的默认无效参数处理程序，这意味着传递到 CRT 函数的无效参数将只会设置 errno 并返回一个错误结果。|
|legacy_stdio_float_rounding .obj|不适用|打印浮点值 (例如，使用带有 Windows 10 19041 通用 C 运行时的 [printf](../c-runtime-library/reference/printf-printf-l-wprintf-wprintf-l.md)) 时已修复。 它现在正确地舍入精确表示的浮点数，并遵循 [fesetround](../c-runtime-library/reference/fegetround-fesetround2.md)请求的浮点舍入。 Visual Studio 2019 版本16.2 及更高版本中提供了此行为更新。 早期版本的 Visual Studio 或提供此链接选项将使用旧行为。|
|loosefpmath.obj|不适用|确保浮点代码容忍不正常的值。|
|newmode.obj|pnewmode.obj|导致 [malloc](../c-runtime-library/reference/malloc.md) 调用新处理程序失败。 请参阅 [_set_new_mode](../c-runtime-library/reference/set-new-mode.md)、[_set_new_handler](../c-runtime-library/reference/set-new-handler.md)、[calloc](../c-runtime-library/reference/calloc.md)，和 [realloc](../c-runtime-library/reference/realloc.md)。|
|noarg.obj|pnoarg.obj|禁用所有 argc 和 argv 进程。|
|nochkclr.obj|不适用|不执行任何操作。 从项目中删除。|
|noenv.obj|pnoenv.obj|禁止为 CRT 创建缓存环境。|
|nothrownew.obj|pnothrownew.obj|启用 CRT 中新增功能的非引发版本。 请参阅 [new 和 delete 运算符](../cpp/new-and-delete-operators.md)。|
|setargv.obj|psetargv.obj|启用命令行参数通配符扩展。 请参阅[扩展通配符参数](../c-language/expanding-wildcard-arguments.md)。|
|threadlocale.obj|pthreadlocale.obj|默认情况下，启用所有新线程的每线程区域设置。|
|wsetargv.obj|pwsetargv.obj|启用命令行参数通配符扩展。 请参阅[扩展通配符参数](../c-language/expanding-wildcard-arguments.md)。|

## <a name="see-also"></a>另请参阅

- [C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
