---
title: 多线程库性能
description: 概述如何从 Microsoft C 运行时多线程库中获取最佳性能。
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- threading [C++], performance
- libraries, multithreaded
- performance, multithreading
- multithreaded libraries
ms.assetid: faa5d808-087c-463d-8f0d-8c478d137296
ms.openlocfilehash: 39d77d52dbc8ac2e725cf46c98c5462df701e3eb
ms.sourcegitcommit: 90c300b74f6556cb5d989802d2e80d79542f55e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514663"
---
# <a name="multithreaded-libraries-performance"></a>多线程库性能

单线程的 CRT 不再可用。 本主题讨论如何从多线程库中获取最佳性能。

## <a name="maximizing-performance"></a>最大程度地提高性能

多线程库的性能已增强，并且接近消除的单线程库的性能。 对于这些需要更高性能的情况，提高了多种新功能。

- 利用独立流锁定，可以锁定流，然后使用直接访问流的 [_nolock 函数](../c-runtime-library/nolock-functions.md)。 这允许在关键循环的外部提升锁定用法。

- 每个线程区域设置可降低多线程方案的区域设置访问成本（请参阅 [_configthreadlocale](../c-runtime-library/reference/configthreadlocale.md)）。

- 区域设置相关的函数（名称以 _l 结尾）将区域设置用作参数，并降低了大量成本（例如，[printf、_printf_l、wprintf、_wprintf_l](../c-runtime-library/reference/printf-printf-l-wprintf-wprintf-l.md)）。

- 公共代码页的优化将降低许多短操作的成本。

- 定义 [_CRT_DISABLE_PERFCRIT_LOCKS](../c-runtime-library/crt-disable-perfcrit-locks.md) 将强制所有 I/O 操作采用单线程的 I/O 模型并使用函数的 _nolock 形式。 这使高度基于 I/O 的单线程应用程序能够获得更好的性能。

- 通过公开 CRT 堆句柄，您可以启用 CRT 堆的 Windows 低分片堆 (LFH)，这将大大改进高度缩放的方案的性能。

## <a name="see-also"></a>另请参阅

[C 运行时 (CRT) 和 c + + 标准库 (STL) `.lib` 文件](../c-runtime-library/crt-library-features.md)
