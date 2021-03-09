---
title: AddressSanitizer 阴影字节数
description: 由编译器生成的代码和 AddressSanitizer 运行时写入和读取的影子字节的技术说明。
ms.date: 03/02/2021
helpviewer_keywords:
- Shadow bytes
- AddressSanitizer shadow bytes
- Address Sanitizer shadow bytes
- ASan shadow bytes
ms.openlocfilehash: 89c3051d2e68d579f2f187fcd12b45ff52cd8a58
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470885"
---
# <a name="addresssanitizer-shadow-bytes"></a>AddressSanitizer 阴影字节数

我们简要总结了阴影字节的概念，以及如何使用它们来实现的运行时实现 [`/fsanitize=address`](../build/reference/fsanitize.md) 。 有关更多详细信息，请参阅 [box 白皮书](https://www.usenix.org/system/files/conference/atc12/atc12-final39.pdf) 和 [AddressSanitizer 算法](https://github.com/google/sanitizers/wiki/AddressSanitizerAlgorithm)。

## <a name="core-concept"></a>核心概念

应用程序的虚拟地址空间中的每 **8 个字节** 都可以使用 **一个阴影字节** 进行描述。

一个阴影字节描述当前可访问的字节数，如下所示：

- 0表示全部8个字节
- 1-7 表示1到7个字节
- 负数对运行时用于报告诊断的上下文进行编码。

### <a name="shadow-byte-legend"></a>影子字节图例

请考虑此阴影字节图例，其中定义了所有负数：

:::image type="content" source="./media/asan-shadow-byte-legend.png" alt-text="AddressSanitizer 影子字节图例的屏幕截图。":::

## <a name="mapping---describing-your-address-space"></a>映射-描述地址空间

应用程序的虚拟地址空间中每8个字节的对齐方式都可以映射到在虚拟地址空间中描述该槽的阴影字节。  可以通过 **简单的 shift 和 add** 来完成此映射。

X86：

```cpp
char shadow_byte_value = *((Your_Address >> 3) + 0x30000000)
```

在 x64 上：

```cpp
char shadow_byte_value = *((Your_Address >> 3) + _asan_runtime_assigned_offset)
```

## <a name="code-generation---tests"></a>代码生成-测试

考虑由编译器生成的代码、静态数据或运行时写入特定阴影字节的方式。 此伪代码显示了如何在任何负载或存储之前生成检查：

```cpp
ShadowAddr = (Addr >> 3) + Offset;
if (*ShadowAddr != 0) {
    ReportAndCrash(Addr);
}
```

检测到宽度小于8个字节的内存引用时，检测会稍微复杂一些。 如果阴影值为正 (意味着只有8字节字中的前 k 字节可以访问) ，则需要将地址的最后3位与 k 进行比较。

```cpp
ShadowAddr = (Addr >> 3) + Offset;
k = *ShadowAddr;
if (k != 0 && ((Addr & 7) + AccessSize > k)) {
    ReportAndCrash(Addr);
}
```

运行时和编译器生成的代码都写入阴影字节。 在释放作用域结束或存储时，这些阴影字节要么允许还是撤消访问权限。 上述检查在程序的执行过程中，在应用程序的地址空间中描述8字节的 "槽" 的读取。 除了这些显式生成的检查以外，运行时还会在其截获 (或 "挂钩" ) CRT 中的许多函数后检查阴影字节。

有关详细信息，请参阅截获的 [函数](./asan-runtime.md#default-interceptors)列表。

## <a name="setting-shadow-bytes"></a>设置阴影字节

编译器生成的代码和 AddressSanitizer 运行时都可以写入阴影字节。 例如，编译器可以设置阴影字节，以允许对内部范围中定义的堆栈局部变量进行固定大小的访问。 运行时可以将数据部分中的全局变量括在影子字节内。

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
