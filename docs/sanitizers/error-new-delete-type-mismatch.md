---
title: 错误：新的-删除-类型不匹配
description: 新删除类型不匹配错误的源示例和实时调试屏幕截图。
ms.date: 03/02/2021
f1_keywords:
- new-delete-type-mismatch
helpviewer_keywords:
- new-delete-type-mismatch error
- AddressSanitizer error new-delete-type-mismatch
ms.openlocfilehash: 02ea69b16fbb18878fd46544488ac8f3e0d4e3b5
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470861"
---
# <a name="error-new-delete-type-mismatch"></a>错误：`new-delete-type-mismatch`

> 地址 Sanitizer 错误：释放大小与分配大小不同

在此示例中，仅 `~Base` 调用，而不 `~Derived` 调用。 编译器将生成对的调用， `~Base()` 因为 `Base` 析构函数不是 **`virtual`** 。 当我们调用时 `delete b` ，对象的析构函数将绑定到默认定义。 此代码会在 Windows) 上删除一个空基类 (或1个字节。 **`virtual`** 析构函数声明中缺少关键字是使用继承时的常见 c + + 错误。

## <a name="example---virtual-destructor"></a>示例-虚拟析构函数

```cpp
// example1.cpp
// new-delete-type-mismatch error
#include <memory>
#include <vector>

struct T {
    T() : v(100) {}
    std::vector<int> v;
};

struct Base {};

struct Derived : public Base {
    T t;
};

int main() {
    Base *b = new Derived;

    delete b;  // Boom! 

    std::unique_ptr<Base> b1 = std::make_unique<Derived>();

    return 0;
}
```

多态基类应声明 **`virtual`** 析构函数。 如果某个类有任何虚函数，则它应具有一个虚拟析构函数。

若要修复此示例，请添加：

```cpp
struct Base {
  virtual ~Base() = default;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error"></a>产生的错误

:::image type="content" source="media/new-delete-type-mismatch-example-1.png" alt-text="示例1中显示新的-删除类型不匹配错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
