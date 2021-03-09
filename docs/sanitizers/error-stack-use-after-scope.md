---
title: 错误：堆栈-作用域后
description: 作用域错误之后的堆栈使用的源示例和实时调试屏幕截图。
ms.date: 02/05/2021
f1_keywords:
- stack-use-after-scope
helpviewer_keywords:
- stack-use-after-scope error
- AddressSanitizer error stack-use-after-scope
ms.openlocfilehash: 2b6e3ada1cc5b76b371e8059e045735b16d4b334
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102470851"
---
# <a name="error-stack-use-after-scope"></a>错误：`stack-use-after-scope`

> 地址 Sanitizer 错误：使用超出范围的堆栈内存

在 C 或 c + + 中，使用变量生存期的词法范围外的堆栈地址可能会发生多种情况。

## <a name="example-1---simple-nested-local"></a>示例 1-简单的嵌套本地

```cpp
// example1.cpp
// stack-use-after-scope error
int *gp;

bool b = true;

int main() {
    if (b) {
        int x[5];
        gp = x+1;
    }
    return *gp;  // Boom!
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example1.cpp /fsanitize=address /Zi
devenv /debugexe example1.exe
```

### <a name="resulting-error---simple-nested-local"></a>产生的错误-简单的嵌套本地

:::image type="content" source="media/stack-use-after-scope-example-1.png" alt-text="示例1中显示堆栈-使用后错误的调试器屏幕截图。":::

## <a name="example-2---lambda-capture"></a>示例 2-lambda 捕获

```cpp
// example2.cpp
// stack-use-after-scope error
#include <functional>

int main() {
    std::function<int()> f;
    {
        int x = 0;
        f = [&x]() {
            return x;  // Boom!
        };
    }
    return f();  // Boom!
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example2.cpp /fsanitize=address /Zi
devenv /debugexe example2.exe
```

### <a name="resulting-error---lambda-capture"></a>产生的错误-lambda 捕获

:::image type="content" source="media/stack-use-after-scope-example-2.png" alt-text="示例2中显示堆栈-使用后错误的调试器屏幕截图。":::

## <a name="example-3---destructor-ordering-with-locals"></a>示例 3-用局部变量排序的析构函数

```cpp
// example3.cpp
// stack-use-after-scope error
#include <stdio.h>

struct IntHolder {
    explicit IntHolder(int* val = 0) : val_(val) { }
    ~IntHolder() {
        printf("Value: %d\n", *val_);  // Bom!
    }
    void set(int* val) { val_ = val; }
    int* get() { return val_; }

    int* val_;
};

int main(int argc, char* argv[]) {
    // It's incorrect to use "x" inside the IntHolder destructor,
    // because the lifetime of "x" ends earlier. Per the C++ standard,
    // local lifetimes end in reverse order of declaration.
    IntHolder holder;
    int x = argc;
    holder.set(&x);
    return 0;
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example3.cpp /fsanitize=address /Zi /O1
devenv /debugexe example3.exe
```

### <a name="resulting-error---destructor-ordering"></a>产生的错误-析构函数排序

:::image type="content" source="media/stack-use-after-scope-example-3.png" alt-text="示例3中显示堆栈即用后错误的调试器屏幕截图。":::

## <a name="example-4---temporaries"></a>示例 4-临时内存

```cpp
// example4.cpp
// stack-use-after-scope error
#include <iostream>

struct A {
    A(const int& v) {
        p = &v;
    }
    void print() {
        std::cout << *p;
    }
    const int* p;
};

void explicit_temp() {
    A a(5);     // the temp for 5 is no longer live;
    a.print();
}

void temp_from_conversion() {
    double v = 5;
    A a(v);     // local v is no longer live.
    a.print();
}

void main() {
    explicit_temp();
    temp_from_conversion(); 
}
```

若要生成并测试此示例，请在 Visual Studio 2019 版本16.9 或更高版本的 [开发人员命令提示符下](../build/building-on-the-command-line.md#developer_command_prompt_shortcuts)运行以下命令：

```cmd
cl example4.cpp /EHsc /fsanitize=address /Zi
devenv /debugexe example4.exe
```

### <a name="resulting-error---temporaries"></a>产生的错误-临时内存

:::image type="content" source="media/stack-use-after-scope-example-4.png" alt-text="示例4中显示堆栈-使用后错误的调试器屏幕截图。":::

## <a name="see-also"></a>另请参阅

[AddressSanitizer 概述](./asan.md)\
[AddressSanitizer 的已知问题](./asan-known-issues.md)\
[AddressSanitizer 生成和语言参考](./asan-building.md)\
[AddressSanitizer 运行时引用](./asan-runtime.md)\
[AddressSanitizer 阴影字节数](./asan-shadow-bytes.md)\
[AddressSanitizer 云或分布式测试](./asan-offline-crash-dumps.md)\
[AddressSanitizer 调试器集成](./asan-debugger-integration.md)\
[AddressSanitizer 错误示例](./asan-error-examples.md)
