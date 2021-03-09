---
title: 'Visual Studio AddressSanitizer 扩展功能库 (VCASan) '
description: Vcasan 的技术说明。
ms.date: 03/02/2021
f1_keywords:
- vcasan
helpviewer_keywords:
- vcasan.lib
- vcasan
- vcasand.lib
- libvcasan.lib
- libvcasand.lib
ms.openlocfilehash: 8728fead94b5d2b4827b34f1a5461c08c821f2e7
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470890"
---
# <a name="visual-studio-addresssanitizer-extended-functionality-library-vcasan"></a>Visual Studio AddressSanitizer 扩展功能库 (VCAsan) 

这些 *`VCAsan*.lib`* 库实现 Visual Studio 中的扩展调试器 IDE 功能。 利用这些功能，IDE 可以在实时调试会话中显示 AddressSanitizer 错误，也可以通过使用元数据保存故障转储文件来脱机显示。 当 MSVC 编译器启用 AddressSanitizer 时，将链接库。

## <a name="vcasan-library-inventory"></a>VCAsan 库清单

| 运行时选项 | VCAsan 链接库  |
|---------------|----------------------|
| **`/MT`**           | *`libvcasan.lib`*        |
| **`/MD`**           | *`vcasan.lib`*           |
| **`/MTd`**          | *`libvcasand.lib`*       |
| **`/MDd`**          | *`vcasand.lib`*          |

## <a name="vcasan-library-features"></a>VCAsan 库功能

### <a name="rich-addresssanitizer-error-report-window-in-visual-studio-ide"></a>Visual Studio IDE 中的 "丰富 AddressSanitizer 错误报告" 窗口

VCAsan 库使用 interface 函数在 AddressSanitizer 运行时中注册回调 [`__asan_set_error_report_callback`](https://github.com/llvm/llvm-project/blob/1ba5ea67a30170053964a28f2f47aea4bb7f5ff1/compiler-rt/include/sanitizer/asan_interface.h#L256) 。 如果生成 AddressSanitizer 报表，此回调将用于引发由 Visual Studio 捕获的异常。 Visual Studio 使用异常数据生成在 IDE 中向用户显示的消息。

> [!NOTE]
> VCAsan 库在 AddressSanitizer 运行时中注册一个回调函数。 如果你的代码第二次调用此注册函数，它将覆盖 VCAsan 库回调注册。 这会导致所有 Visual Studio IDE 集成丢失。 你将还原为默认的 IDE 用户体验。 还可以让注册回叫的用户调用丢失。 如果遇到这两个问题，请提交 [bug](https://aka.ms/feedback/report?space=62)。

### <a name="save-addresssanitizer-errors-in-a-new-type-of-crash-dump-file"></a>在一种新的故障转储文件中保存 AddressSanitizer 错误

将 VCAsan 库链接到可执行文件时，用户可以在运行时使其生成故障转储。 当发生诊断错误时，AddressSanitizer 运行时将生成转储文件。 若要启用此功能，用户可以 `ASAN_SAVE_DUMPS` 通过使用如下所示的命令来设置环境变量：

`set ASAN_SAVE_DUMPS=MyFileName.dmp`

文件必须具有 dmp 后缀才能遵循 Visual Studio IDE 约定。

指定转储文件时，会发生以下情况 `ASAN_SAVE_DUMPS` ：如果 AddressSanitizer 运行时捕获到错误，则会保存包含与错误关联的元数据的故障转储文件。 Visual Studio 版本16.9 及更高版本中的调试器可以分析转储文件中保存的元数据。 您可以 `ASAN_SAVE_DUMPS` 基于每个测试进行设置，存储这些二进制项目，然后在 IDE 中使用正确的源索引来查看这些项目。

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
