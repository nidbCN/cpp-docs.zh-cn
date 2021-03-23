---
title: Visual Studio 2019 中的 C++ 一致性改进
description: Visual Studio 中的 Microsoft C++ 正朝着完全符合 C++20 语言标准的方向发展。
ms.date: 03/10/2021
ms.technology: cpp-language
ms.openlocfilehash: 3b78551ee4d8590403cdfaf77c267eef0ab6d811
ms.sourcegitcommit: f8ba5db09d05683b24c58505f0e57c21f85545dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103087254"
---
# <a name="c-conformance-improvements-behavior-changes-and-bug-fixes-in-visual-studio-2019"></a>Visual Studio 2019 中的 C++ 符合性改进、行为更改和 bug 修复

Visual Studio 中的 Microsoft C++ (MSVC) 在每个版本中进行了符合性改进和 bug 修复。 本文按主要版本然后次要版本列出了改进之处。 若要直接跳转到特定版本的更改，请使用本文下面的列表。

本文档列出了 Visual Studio 2019 中的更改。 有关 Visual Studio 2017 中的更改的指南，请参阅 [Visual Studio 2017 中的 C++ 符合性改进](cpp-conformance-improvements-2017.md)。 有关之前符合性改进的完整列表，请参阅 [Visual C++ 2003 - 2015 中的新增功能](../porting/visual-cpp-what-s-new-2003-through-2015.md)。

## <a name="conformance-improvements-in-visual-studio-2019-rtw-version-160"></a><a name="improvements_160"></a>Visual Studio 2019 RTW（版本 16.0）中的符合性改进

Visual Studio 2019 RTW 包含 Microsoft C++ 编译器的以下符合性改进、bug 修复和行为更改。

> [!NOTE]
> C++20 功能将在 `/std:c++latest` 模式下提供，直到编译器和 IntelliSense 的 C++20 实现完成。 届时，我们将引入 `/std:c++20` 编译器模式。

### <a name="improved-modules-support-for-templates-and-error-detection"></a>针对模板和错误检测的改进模块支持

模块现在正式采用 C++20 标准。 Visual Studio 2017 版本 15.9 中增加了改进的支持。 有关详细信息，请参阅 [MSVC 2017 版本 15.9 中针对 C++ 模块的更好模板支持和错误检测](https://devblogs.microsoft.com/cppblog/better-template-support-and-error-detection-in-c-modules-with-msvc-2017-version-15-9/)。

### <a name="modified-specification-of-aggregate-type"></a>修改了聚合类型的规范

C++20 中的聚合类型规范已更改（请参阅[禁止使用用户声明的构造函数聚合](https://wg21.link/p1008r1)）。 在 `/std:c++latest` 模式下的 Visual Studio 2019 中，具有任何用户声明的构造函数（例如，包括声明为 `= default` 或 `= delete` 的构造函数）的类都不是聚合。 之前，仅用户提供的构造函数才会使类失去成为聚合的资格。 此更改对如何初始化此类类型进行了更多限制。

以下代码在 Visual Studio 2017 中进行编译且没有错误，但在 `/std:c++latest` 模式下的 Visual Studio 2019 中则会抛出错误 C2280 和 C2440：

```cpp
struct A
{
    A() = delete; // user-declared ctor
};

struct B
{
    B() = default; // user-declared ctor
    int i = 0;
};

A a{}; // ill-formed in C++20, previously well-formed
B b = { 1 }; // ill-formed in C++20, previously well-formed
```

### <a name="partial-support-for-operator-"></a>对 `operator <=>` 的部分支持

[P0515R3](https://wg21.link/p0515r3) C++20 引入 `<=>` 三向比较运算符，也称为“太空船运算符”。 `/std:c++latest` 模式下的 Visual Studio 2019 通过针对现在被禁用的语法抛出错误，引入对运算符的部分支持。 例如，以下代码在 Visual Studio 2017 中进行编译且没有错误，但在 `/std:c++latest` 模式下的 Visual Studio 2019 中则会抛出多个错误：

```cpp
struct S
{
    bool operator<=(const S&) const { return true; }
};

template <bool (S::*)(const S&) const>
struct U { };

int main(int argc, char** argv)
{
    U<&S::operator<=> u; // In Visual Studio 2019 raises C2039, 2065, 2146.
}
```

若要避免这些错误，请在最后一个尖括号之前有问题的行中插入一个空格：`U<&S::operator<= > u;`。

### <a name="references-to-types-with-mismatched-cv-qualifiers"></a>对具有不匹配的 cv 限定符的类型的引用

MSVC 之前允许直接绑定来自具有顶级以下不匹配 cv 限定符的类型的引用。 此绑定可允许修改引用可能引用的 const 数据。 编译器现在将根据标准要求创建一个临时文件。 在 Visual Studio 2017 中，编译以下代码时不会发出警告。 在 Visual Studio 2019 中，编译器会引发警告 C4172：

```cpp
struct X
{
    const void* const& PData() const
    {
        return _pv;
    }

    void* _pv;
};

int main()
{
    X x;
    auto p = x.PData(); // C4172 <func:#1 "?PData@X@@QBEABQBXXZ"> returning address of local variable or temporary
}
```

### <a name="reinterpret_cast-from-an-overloaded-function"></a>来自重载函数的 `reinterpret_cast`

`reinterpret_cast` 的参数不是允许使用已重载函数的地址的上下文之一。 以下代码在 Visual Studio 2017 中进行编译时没有错误，但在 Visual Studio 2019 中则会引发错误 C2440：

```cpp
int f(int) { return 1; }
int f(float) { return .1f; }
using fp = int(*)(int);

int main()
{
    fp r = reinterpret_cast<fp>(&f); // C2440: cannot convert from 'overloaded-function' to 'fp'
}
```

若要避免该错误，请为此方案使用允许的强制转换：

```cpp
int f(int);
int f(float);
using fp = int(*)(int);

int main()
{
    fp r = static_cast<fp>(&f); // or just &f;
}
```

### <a name="lambda-closures"></a>Lambda 闭包

在 C++14 中，lambda 闭包类型不是文本。 此规则的主要后果是，Lambda 可能不会赋给 `constexpr` 变量。 以下代码在 Visual Studio 2017 中进行编译时没有错误，但在 Visual Studio 2019 中则会引发错误 C2127：

```cpp
int main()
{
    constexpr auto l = [] {}; // C2127 'l': illegal initialization of 'constexpr' entity with a non-constant expression
}
```

为了避免此错误，请删除 `constexpr` 限定符，或将符合性模式更改为 `/std:c++17`。

### <a name="stdcreate_directory-failure-codes"></a>`std::create_directory` 失败代码

根据 C++20 无条件地实现了 [P1164](https://wg21.link/p1164r1)。 这会更改 `std::create_directory` 来检查目标是否已经为故障目录。 之前，所有 ERROR_ALREADY_EXISTS 类型错误都会转换为成功但未创建目录的代码。

### `operator<<(std::ostream, nullptr_t)`

根据 [LWG 2221](https://cplusplus.github.io/LWG/issue2221)，增加了 `operator<<(std::ostream, nullptr_t)` 用于将 `nullptr` 写入数据流。

### <a name="more-parallel-algorithms"></a>更多并行算法

`is_sorted`、`is_sorted_until`、`is_partitioned`、`set_difference`、`set_intersection`、`is_heap` 和 `is_heap_until` 的新并行版本。

### <a name="atomic-initialization"></a>原子初始化

[P0883“修复原子初始化”](https://wg21.link/p0883r1)将 `std::atomic` 更改为对包含的 `T` 进行值初始化，而不是默认初始化。 将 Clang/LLVM 与 Microsoft 标准库配合使用时，将启用此修复。 作为 `constexpr` 处理中的 bug 的解决方法，它目前在 Microsoft C++ 编译器中被禁用。

### <a name="remove_cvref-and-remove_cvref_t"></a>`remove_cvref` 和 `remove_cvref_t`

根据 [P0550](https://wg21.link/p0550r2) 实现了 `remove_cvref` 和 `remove_cvref_t` 类型特征。 这些类型特征可从类型移除引用性和 cv 限定，而无需将函数和数组衰减为指针（与 `std::decay` 和 `std::decay_t` 不同）。

### <a name="feature-test-macros"></a>功能测试宏

[P0941R2 - 功能测试宏](https://wg21.link/p0941r2)已完成，支持 `__has_cpp_attribute`。 所有标准模式都支持功能测试宏。

### <a name="prohibit-aggregates-with-user-declared-constructors"></a>禁止使用用户声明的构造函数进行聚合

C++ 20 [P1008R1 - 禁止使用用户声明的构造函数进行聚合](https://wg21.link/p1008r1)已完成。

### <a name="reinterpret_cast-in-a-constexpr-function"></a>`constexpr` 函数中的 `reinterpret_cast`

`reinterpret_cast` 在 `constexpr` 函数中是非法的。 Microsoft C++ 编译器以前只会拒绝在 `constexpr` 上下文中使用的 `reinterpret_cast`。 在 Visual Studio 2019 中，在所有语言标准模式下，编译器都能在 `constexpr` 函数的定义中正确诊断 `reinterpret_cast`。 以下代码现在生成错误 C3615：

```cpp
long long i = 0;
constexpr void f() {
    int* a = reinterpret_cast<int*>(i); // C3615: constexpr function 'f' cannot result in a constant expression
}
```

为了避免此错误，请从函数声明中删除 `constexpr` 修饰符。

### <a name="correct-diagnostics-for-basic_string-range-constructor"></a>basic_string 范围构造函数的正确诊断

在 Visual Studio 2019 中，`basic_string` 范围构造函数不再禁止显示包含 `static_cast` 的编译器诊断。 尽管在初始化 `out` 时可能会丢失从 `wchar_t` 到 `char` 的数据，但在 Visual Studio 2017 中编译以下代码时不会出现警告：

```cpp
std::wstring ws = /* … */;
std::string out(ws.begin(), ws.end()); // VS2019 C4244: 'argument': conversion from 'wchar_t' to 'const _Elem', possible loss of data.
```

Visual Studio 2019 正确地引发警告 C4244。 若要避免此警告，可初始化 `std::string`，如以下示例所示：

```cpp
std::wstring ws = L"Hello world";
std::string out;
for (wchar_t ch : ws)
{
    out.push_back(static_cast<char>(ch));
}
```

### <a name="incorrect-calls-to--and---under-clr-or-zw-are-now-correctly-detected"></a>现在可正确检测到在 `/clr` 或 `/ZW` 下对 `+=` 和 `-=` 的不正确调用

Visual Studio 2017 中引入了一个 bug，会导致编译器以静默方式忽略错误，并且不会在 `/clr` 或 `/ZW` 下对 `+=` 和 `-=` 的无效调用生成代码。 在 Visual Studio 2017 中编译以下代码时不会引发错误，但在 Visual Studio 2019 中，它会正确引发错误 C2845:

```cpp
public enum class E { e };

void f(System::String ^s)
{
    s += E::e; // in VS2019 C2845: 'System::String ^': pointer arithmetic not allowed on this type.
}
```

若要避免此示例中的错误，请将 `+=` 运算符与 `ToString()` 方法配合使用：`s += E::e.ToString();`。

### <a name="initializers-for-inline-static-data-members"></a>内联静态数据成员的初始化表达式

现在可正确检测到 `inline` 和 `static constexpr` 初始化表达式中的无效成员访问。 以下示例在 Visual Studio 2017 中进行编译时没有错误，但在 Visual Studio 2019 的 `/std:c++17` 模式下则会引发错误 C2248：

```cpp
struct X
{
    private:
        static inline const int c = 1000;
};

struct Y : X
{
    static inline int d = c; // VS2019 C2248: cannot access private member declared in class 'X'.
};
```

若要避免该错误，请将 `X::c` 成员声明为受保护：

```cpp
struct X
{
    protected:
        static inline const int c = 1000;
};
```

### <a name="c4800-reinstated"></a>C4800 已恢复

MSVC 曾有关于隐式转换为 `bool` 的性能警告 C4800。 此警告的干扰性太高，且无法取消，导致我们在 Visual Studio 2017 中删除了它。 但是，在 Visual Studio 2017 的生命周期中，我们收到了大量关于解决该问题的有用案例的反馈。 我们在 Visual Studio 2019 中重新推出了精心定制的 C4800 及说明性 C4165。 这两个警告都可以轻松取消：要么通过使用显式强制转换，要么通过与适当类型的 0 进行比较。 C4800 是默认关闭的 4 级警告，C4165 是默认关闭的 3 级警告。 使用 `/Wall` 编译器选项可以发现这两个警告。

以下示例在 `/Wall` 下引发 C4800 和 C4165：

```cpp
bool test(IUnknown* p)
{
    bool valid = p; // warning C4800: Implicit conversion from 'IUnknown*' to bool. Possible information loss
    IDispatch* d = nullptr;
    HRESULT hr = p->QueryInterface(__uuidof(IDispatch), reinterpret_cast<void**>(&d));
    return hr; // warning C4165: 'HRESULT' is being converted to 'bool'; are you sure this is what you want?
}
```

若要避免上一示例中的警告，可编写如下代码：

```cpp
bool test(IUnknown* p)
{
    bool valid = p != nullptr; // OK
    IDispatch* d = nullptr;
    HRESULT hr = p->QueryInterface(__uuidof(IDispatch), reinterpret_cast<void**>(&d));
    return SUCCEEDED(hr);  // OK
}
```

### <a name="local-class-member-function-doesnt-have-a-body"></a>局部类成员函数没有函数体

在 Visual Studio 2017 中，只有在编译器选项 `/w14822` 被显式设置时，才会引发警告 C4822。 它不会使用 `/Wall` 显示。 在 Visual Studio 2019 中，C4822 是默认关闭的警告，这使其在 `/Wall` 下无需显式设置 `/w14822` 即可被发现。

```cpp
void example()
{
    struct A
        {
            int boo(); // warning C4822: Local class member function doesn't have a body
        };
}
```

### <a name="function-template-bodies-containing-if-constexpr-statements"></a>包含 `if constexpr` 语句的函数模板主体

包含 `if constexpr` 语句的模板函数体启用了一些与 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 分析相关的检查。 例如，在 Visual Studio 2017 中，只有在 `/permissive-` 选项未设置时，以下代码才会生成 C7510。 在 Visual Studio 2019 中，即使 `/permissive-` 选项已设置，相同的代码也会抛出错误：

```cpp
template <typename T>

int f()
{
    T::Type a; // error C7510: 'Type': use of dependent type name must be prefixed with 'typename'

    if constexpr (T::val)
    {
        return 1;
    }
    else
    {
        return 2;
    }
}

struct X
{
    using Type = X;
    constexpr static int val = 1;
};

int main()
{
    return f<X>();
}
```

为了避免此错误，请将 `typename` 关键字添加到 `a` 的声明中：`typename T::Type a;`。

### <a name="inline-assembly-code-isnt-supported-in-a-lambda-expression"></a>lambda 表达式不支持内联程序集代码

Microsoft C++ 团队最近获悉一个安全问题，即在 lambda 中使用内联汇编程序可能会导致 `ebp`（返回地址寄存器）在运行时损坏。 恶意攻击者可能能够利用此方案。 内联汇编程序只在 x86 上受支持，并且内联汇编程序与编译器的其余部分之间的交互很差。 鉴于这些事实和此问题的本质，此问题的最安全解决方案是，不允许在 Lambda 表达式中使用内联汇编程序。

我们发现“自然情况下”，在 lambda 表达式内使用内联汇编程序的唯一一种情况是用于捕获返回地址。 在此方案中，只需使用编译器内部函数 `_ReturnAddress()` 即可在所有平台上捕获返回地址。

以下代码在 Visual Studio 2017 15.9 和 Visual Studio 2019 中都生成 C7552：

```cpp
#include <cstdio>

int f()
{
    int y = 1724;
    int x = 0xdeadbeef;

    auto lambda = [&]
    {
        __asm {  // C7552: inline assembler is not supported in a lambda

            mov eax, x
            mov y, eax
        }
    };

    lambda();
    return y;
}
```

若要避免该错误，请将程序集代码移动到命名函数中，如以下示例所示：

```cpp
#include <cstdio>

void g(int& x, int& y)
{
    __asm {
        mov eax, x
        mov y, eax
    }
}

int f()
{
    int y = 1724;
    int x = 0xdeadbeef;
    auto lambda = [&]
    {
        g(x, y);
    };
    lambda();
    return y;
}

int main()
{
    std::printf("%d\n", f());
}
```

### <a name="iterator-debugging-and-stdmove_iterator"></a>迭代器调试和 `std::move_iterator`

已讲解如何使用迭代器调试功能来正确解包 `std::move_iterator`。 例如，`std::copy(std::move_iterator<std::vector<int>::iterator>, std::move_iterator<std::vector<int>::iterator>, int*)` 现在可占用 `memcpy` 快速路径。

### <a name="fixes-for-xkeycheckh-keyword-enforcement"></a>对 \<xkeycheck.h> 关键字强制执行问题的修复

标准库在 \<xkeycheck.h> 中强制宏替换关键字得到了修复。 该库现在会发出实际检测到的问题关键字，而不是一般消息。 它还支持 C++20 关键字，并可避免以欺骗手段使 IntelliSense 将随机关键字视为宏。

### <a name="allocator-types-no-longer-deprecated"></a>不再弃用的分配器类型

`std::allocator<void>`、`std::allocator::size_type` 和 `std::allocator::difference_type` 不再弃用。

### <a name="correct-warning-for-narrowing-string-conversions"></a>缩短字符串转换的正确警告

从 `std::string` 中删除了不符合标准要求且意外禁止显示 C4244 窄化警告的虚假 `static_cast`。 如果现在尝试调用 `std::string::string(const wchar_t*, const wchar_t*)`，将正确地发出 C4244“正在将 `wchar_t` 缩短为 `char`。”

### <a name="various-fixes-for-filesystem-correctness"></a>针对 \<filesystem> 正确性的各种修复

- 修复了 `std::filesystem::last_write_time` 在尝试更改目录的上次写入时间时失败的问题。
- 如果提供不存在的目标路径，`std::filesystem::directory_entry` 构造函数现在会存储失败的结果，而不引发异常。
- `std::filesystem::create_directory` 的 2 个参数的版本已更改为调用 1 个参数的版本，因为当 `existing_p` 为符号链接时，基础 `CreateDirectoryExW` 函数会使用 `copy_symlink`。
- 发现损坏的符号链接时，`std::filesystem::directory_iterator` 不再失败。
- `std::filesystem::space` 现在接受相对路径。
- `std::filesystem::path::lexically_relative` 不再因尾部反斜杠而产生混淆（报告为 [LWG 3096](https://cplusplus.github.io/LWG/issue3096)）。
- 解决了 `CreateSymbolicLinkW` 拒绝 `std::filesystem::create_symlink` 中包含正斜杠的路径的问题。
- 解决了 Windows 10 LTSB 1609 中有 POSIX 删除模式 `delete` 函数，但实际上无法删除文件的问题。
- `std::boyer_moore_searcher` 和 `std::boyer_moore_horspool_searcher` 复制构造函数和复制赋值运算符现在实际上会复制内容。

### <a name="parallel-algorithms-on-windows-8-and-later"></a>Windows 8 及更高版本上的并行算法

并行算法库现在可以在 Windows 8 及更高版本上正确使用真实的 `WaitOnAddress` 系列，而不是始终使用 Windows 7 及早期虚假版本。

### <a name="stdsystem_categorymessage-whitespace"></a>`std::system_category::message()` whitespace

`std::system_category::message()` 现在会截断返回消息的尾随空格。

### <a name="stdlinear_congruential_engine-divide-by-zero"></a>`std::linear_congruential_engine` 被零除

导致 `std::linear_congruential_engine` 触发被 0 除的一些条件已得到修复。

### <a name="fixes-for-iterator-unwrapping"></a>修复迭代器解包问题

在 Visual Studio 2017 15.8 中，首次公开了一些迭代器-解包机制，用于程序员-用户集成。 在 C++ 团队博客文章 [VS 2017 15.8 中的 STL 功能和修补程序](https://devblogs.microsoft.com/cppblog/stl-features-and-fixes-in-vs-2017-15-8/)中进行了介绍。 此机制不再解包从标准库迭代器派生的迭代器。 例如，从 `std::vector<int>::iterator` 派生并尝试自定义行为的用户现在在调用标准库算法时会获得其自定义行为，而不是指针的行为。

无序容器 `reserve` 函数现在实际上为 N 个元素保留，如 [LWG 2156](https://cplusplus.github.io/LWG/issue2156) 中所述。

### <a name="time-handling"></a>时间处理

- 之前，传递给并发库的某些时间值会溢出，例如，`condition_variable::wait_for(seconds::max())`。 这些溢出问题现已修复，其在一个看似随机的 29 天周期（当基础 Win32 API 接受的 uint32_t 毫秒溢出时）中改变了行为。

- \<ctime> 标头现可在命名空间 `std` 中正确声明 `timespec` 和 `timespec_get`，还可在全局命名空间中声明它们。

### <a name="various-fixes-for-containers"></a>针对容器的各种修复

- 许多标准库内部容器函数都已变为 `private`，以改善 IntelliSense 体验。 在后续版本的 MSVC 中，预期会提供将成员标记为 `private` 的更多修复。

- 修复了导致基于节点的容器（如 `list`、`map` 和 `unordered_map`）将损坏的异常安全正确性问题。 在 `propagate_on_container_copy_assignment` 或 `propagate_on_container_move_assignment` 重新分配操作期间，我们将使用旧分配器释放容器的 sentinel 节点、通过旧分配器执行 POCCA/POCMA 分配，然后尝试从新分配器获取 sentinel 节点。 如果此分配失败，则容器会损坏。 甚至可能无法销毁，因为拥有 sentinel 节点是硬数据结构所固有的特点。 修复此代码是为了在销毁现有 sentinel 节点之前通过使用源容器的分配器创建新的 sentinel 节点。

- 根据 `propagate_on_container_copy_assignment`、`propagate_on_container_move_assignment` 和 `propagate_on_container_swap`，容器固定为始终复制/移动/交换分配器，即使对于声明为 `is_always_equal` 的分配器也是如此。

- 为接受 rvalue 容器的容器合并和提取成员函数添加了重载。 有关详细信息，请参阅 [P0083“拼接映射和集”](https://wg21.link/p0083r3)

### <a name="stdbasic_istreamread-processing-of-rn-n"></a>`std::basic_istream::read` 处理 `\r\n`` =>`\n`

`std::basic_istream::read` 已修复为作为 `\r\n` 到 `\n` 处理的一部分暂时不写入所提供缓冲区的部分。 此更改导致在读取大小超过 4K 的文件时，放弃了一些在 Visual Studio 2017 15.8 中获得的性能优势。 但是，仍存在从避免每个字符三个虚拟调用的效率改进。

### <a name="stdbitset-constructor"></a>`std::bitset` 构造函数

对于大型 bitset，`std::bitset` 构造函数不再以相反的顺序读取 1 和 0。

### <a name="stdpairoperator-regression"></a>`std::pair::operator=` 回归

修复了在实现 [LWG 2729 “`std::pair::operator=` 上缺少 SFINAE”时引入的 `std::pair` 赋值运算符回归问题；](https://cplusplus.github.io/LWG/issue2729)。 它现在能够再次正确接受可转换为 `std::pair` 的类型。

### <a name="non-deduced-contexts-for-add_const_t"></a>`add_const_t` 的非导出上下文

修复了一个有关类型特征的小 bug，其中 `add_const_t` 和相关函数应该是非导出上下文。 换而言之，`typename add_const<T>::type` 的别名应该是 `add_const_t`，而不是 `const T`。

## <a name="conformance-improvements-in-161"></a><a name="improvements_161"></a> 16.1 中的符合性改进

### <a name="char8_t"></a>char8_t

[P0482r6](https://wg21.link/p0482r6)。 C++20 增加了新的字符类型，可用于表示 UTF-8 代码单元。 C++20 中的 `u8` 字符串文本具有 `const char8_t[N]` 类型而不是 `const char[N]` 类型，这是之前的情况。 在 [N2231](https://wg14.link/n2231) 中已针对 C 标准提出了类似的更改。 [P1423r3](https://wg21.link/p1423r3) 中给出了有关 `char8_t` 后向兼容性修正的建议。 Microsoft C++ 编译器现已开始在 Visual Studio 2019 版本 16.1 中支持 `char8_t`，前提是你指定 `/Zc:char8_t` 编译器选项。 将来，提供此支持的前提是指定 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md)（可通过 `/Zc:char8_t-` 还原为 C++17 行为）。 支持 IntelliSense 的 EDG 编译器尚不支持。 你可能会看到不影响实际编译的虚假仅 IntelliSense 错误。

#### <a name="example"></a>示例

```cpp
const char* s = u8"Hello"; // C++17
const char8_t* s = u8"Hello"; // C++20
```

### <a name="stdtype_identity-metafunction-and-stdidentity-function-object"></a>`std::type_identity` 元函数和 `std::identity` 函数对象

[P0887R1 type_identity](https://wg21.link/p0887r1)。 弃用的 `std::identity` 类模板扩展已删除，并替换为 C++20 `std::type_identity` 元函数和 `std::identity` 函数对象。 两者仅在 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 下可用。

以下示例在 Visual Studio 2017 中针对 `std::identity`（在 \<type_traits> 中定义）生成弃用警告 C4996：

```cpp
#include <type_traits>

using T = std::identity<int>::type;
T x, y = std::identity<T>{}(x);
int i = 42;
long j = std::identity<long>{}(i);
```

以下示例显示如何将新的 `std::identity`（在 \<functional> 中定义）与新的 `std::type_identity` 配合使用：

```cpp
#include <type_traits>
#include <functional>

using T = std::type_identity<int>::type;
T x, y = std::identity{}(x);
int i = 42;
long j = static_cast<long>(i);
```

### <a name="syntax-checks-for-generic-lambdas"></a>针对泛型 lambda 的语法检查

在 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 下或在具有 `/experimental:newLambdaProcessor` 的其他任何语言模式下，新的 Lambda 处理器在泛型 Lambda 中启用部分符合性模式语法检查。

在 Visual Studio 2017 中，此代码可进行编译且没有警告，但在 Visual Studio 2019 中会生成错误 C2760：

```cpp
void f() {
    auto a = [](auto arg) {
        decltype(arg)::Type t; // C2760 syntax error: unexpected token 'identifier', expected ';'
    };
}
```

以下示例显示正确的语法，现在由编译器强制执行：

```cpp
void f() {
    auto a = [](auto arg) {
        typename decltype(arg)::Type t;
    };
}
```

### <a name="argument-dependent-lookup-for-function-calls"></a>函数调用的依赖于参数的查找

[P0846R0](https://wg21.link/p0846r0) (C++20) 增强了以下功能：通过对包含显式模板参数的函数调用表达式进行参数相关查找来查找函数模板。 需要 `/std:c++latest`。

### <a name="designated-initialization"></a>指定的初始化

[P0329R4](https://wg21.link/p0329r4) (C++20) 指定的初始化允许在聚合初始化中通过使用 `Type t { .member = expr }` 语法选择特定成员。 需要 `/std:c++latest`。

### <a name="new-and-updated-standard-library-functions-c20"></a>新的和更新的标准库函数 (C++20)

- 用于 `basic_string` 和 `basic_string_view` 的 `starts_with()` 和 `ends_with()`。
- 关联容器的 `contains()`。
- `list` 和 `forward_list` 的 `remove()`、`remove_if()` 和 `unique()` 现在返回 `size_type`。
- 向 \<algorithm> 添加了 `shift_left()` 和 `shift_right()`。

## <a name="conformance-improvements-in-162"></a><a name="improvements_162"></a> 16.2 中的符合性改进

### <a name="noexcept-constexpr-functions"></a>`noexcept` `constexpr` 函数

当在常数表达式中使用时，`constexpr` 函数默认不再被视为 `noexcept`。 此行为更改来自解决方法核心工作组 (CWG) [1351](https://wg21.link/cwg1351)，并已在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 中启用。 以下示例在 Visual Studio 2019 版本 16.1 和更早版本中编译，但在 Visual Studio 2019 版本 16.2 中生成 C2338：

```cpp
constexpr int f() { return 0; }

int main() {
    static_assert(noexcept(f()), "f should be noexcept"); // C2338 in 16.2
}
```

若要修复此错误，请将 `noexcept` 表达式添加到函数声明中：

```cpp
constexpr int f() noexcept { return 0; }

int main() {
    static_assert(noexcept(f()), "f should be noexcept");
}
```

### <a name="binary-expressions-with-different-enum-types"></a>具有不同枚举类型的二进制表达式

C++20 弃用了对操作数的常用算术转换，其中：

- 一个操作数为枚举类型，

- 另一个是不同的枚举类型或浮点类型。

有关详细信息，请参阅 [P1120R0](https://wg21.link/p1120r0)。

在 Visual Studio 2019 版本 16.2 和更高版本中，以下代码在启用 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 编译器选项的情况下，生成级别 4 警告：

```cpp
enum E1 { a };
enum E2 { b };
int main() {
    int i = a | b; // warning C5054: operator '|': deprecated between enumerations of different types
}
```

要避免出现此警告，请使用 [`static_cast`](../cpp/static-cast-operator.md) 转换第二个操作数：

```cpp
enum E1 { a };
enum E2 { b };
int main() {
  int i = a | static_cast<int>(b);
}
```

如果 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 编译器选项已启用，在枚举类型和浮点类型之间进行二进制运算现在会导致警告抛出：

```cpp
enum E1 { a };
int main() {
  double i = a * 1.1;
}
```

要避免出现此警告，请使用 [`static_cast`](../cpp/static-cast-operator.md) 转换第二个操作数：

```cpp
enum E1 { a };
int main() {
   double i = static_cast<int>(a) * 1.1;
}
```

### <a name="equality-and-relational-comparisons-of-arrays"></a>数组的相等性和关系比较

C++20 中已弃用数组类型的两个操作数之间的相等性和关系比较 ([P1120R0](https://wg21.link/p1120r0))。 换句话说，两个数组之间的比较运算（尽管级别和范围相似）现在会导致警告抛出。 自 Visual Studio 2019 版本 16.2 起，如果 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 编译器选项已启用，以下代码会生成 C5056：

```cpp
int main() {
    int a[] = { 1, 2, 3 };
    int b[] = { 1, 2, 3 };
    if (a == b) { return 1; } // warning C5056: operator '==': deprecated for array types
}
```

要避免出现此警告，可以比较第一个元素的地址：

```cpp
int main() {
    int a[] = { 1, 2, 3 };
    int b[] = { 1, 2, 3 };
    if (&a[0] == &b[0]) { return 1; }
}
```

要确定两个数组内容是否相等，可以使用 [`std::equal`](../standard-library/algorithm-functions.md#equal) 函数：

```cpp
std::equal(std::begin(a), std::end(a), std::begin(b), std::end(b));
```

### <a name="effect-of-defining-spaceship-operator-on--and-"></a>对 `==` 和 `!=` 定义宇宙飞船运算符的效果

仅定义宇宙飞船运算符 (`<=>`) 将不再重写涉及 `==` 或 `!=` 的表达式，除非宇宙飞船运算符标记为 `= default` ([P1185R2](https://wg21.link/p1185r2))。 以下示例在 Visual Studio 2019 RTW 和版本 16.1 中编译，但在 Visual Studio 2019 版本 16.2 中生成 C2678：

```cpp
#include <compare>

struct S {
  int a;
  auto operator<=>(const S& rhs) const {
    return a <=> rhs.a;
  }
};
bool eq(const S& lhs, const S& rhs) {
  return lhs == rhs; // error C2676
}
bool neq(const S& lhs, const S& rhs) {
    return lhs != rhs; // error C2676
}
```

要避免此错误，请定义`operator==` 或将其声明为默认值：

```cpp
#include <compare>

struct S {
  int a;
  auto operator<=>(const S& rhs) const {
    return a <=> rhs.a;
  }
  bool operator==(const S&) const = default;
};
bool eq(const S& lhs, const S& rhs) {
  return lhs == rhs;
}
bool neq(const S& lhs, const S& rhs) {
    return lhs != rhs;
}
```

### <a name="standard-library-improvements"></a>标准库改进

- 具有固定/科学精度的 \<charconv> `to_chars()`。 （一般精度目前为 16.4 计划。）
- [P0020R6](https://wg21.link/p0020r6)：`atomic<float>`、`atomic<double>`、`atomic<long double>`
- [P0463R1](https://wg21.link/p0463r1)：endian
- [P0482R6](https://wg21.link/p0482r6)：`char8_t` 的库支持
- [P0600R1](https://wg21.link/p0600r1)：`[[nodiscard]]` 针对 STL，第 1 部分
- [P0653R2](https://wg21.link/p0653r2)：`to_address()`
- [P0754R2](https://wg21.link/p0754r2)：\<version>
- [P0771R1](https://wg21.link/p0771r1)：`noexcept` 用于 `std::function` 的移动构造函数

### <a name="const-comparators-for-associative-containers"></a>相关容器的 Const 比较运算符

[`set`](../standard-library/set-class.md)、[`map`](../standard-library/map-class.md)、[`multiset`](../standard-library/multiset-class.md) 和 [`multimap`](../standard-library/multimap-class.md) 中用于搜索和插入的代码已合并，以减少代码量。 插入操作现在对 `const` 比较函子调用小于比较，与之前搜索操作采用的方式相同。 以下代码在 Visual Studio 2019 版本 16.1 和更早版本中编译，但在 Visual Studio 2019 版本 16.2 中引发 C3848：

```cpp
#include <iostream>
#include <map>

using namespace std;

struct K
{
   int a;
   string b = "label";
};

struct Comparer  {
   bool operator() (K a, K b) {
      return a.a < b.a;
   }
};

map<K, double, Comparer> m;

K const s1{1};
K const s2{2};
K const s3{3};

int main() {

   m.emplace(s1, 1.08);
   m.emplace(s2, 3.14);
   m.emplace(s3, 5.21);

}
```

为了避免此错误，请设置比较运算符 `const`：

```cpp
struct Comparer  {
   bool operator() (K a, K b) const {
      return a.a < b.a;
   }
};

```

## <a name="conformance-improvements-in-visual-studio-2019-version-163"></a><a name="improvements_163"></a>Visual Studio 2019 版本 16.3 的符合性改进

### <a name="stream-extraction-operators-for-char-removed"></a>删除了 `char*` 的流提取运算符

指针到字符的流提取运算符已删除，并替换为数组到字符的提取运算符（按照 [P0487R1](https://wg21.link/p0487r1)）。 WG21 认为删除的重载不安全。 在 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 模式下，以下示例现在生成 C2679：

```cpp
// stream_extraction.cpp
// compile by using: cl /std:c++latest stream_extraction.cpp

#include <iostream>
#include <iomanip>

int main() {
    char x[42];
    char* p = x;
    std::cin >> std::setw(42);
    std::cin >> p;  // C2679: binary '>>': no operator found which takes a right-hand operand of type 'char *' (or there is no acceptable conversion)
}
```

要避免此错误，请将提取运算符与 `char[]` 变量一起使用：

```cpp
#include <iostream>
#include <iomanip>

int main() {
    char x[42];
    std::cin >> std::setw(42);
    std::cin >> x;  // OK
}
```

### <a name="new-keywords-requires-and-concept"></a>新关键字 `requires` 和 `concept`

已向 Microsoft C++ 编译器添加新关键字 `requires` 和 `concept`。 如果尝试在 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 模式下将这两个关键字中的任何一个用作标识符，编译器都会引发 C2059：“语法错误”。

### <a name="constructors-as-type-names-disallowed"></a>不允许将构造函数作为类型名称

在以下情况下，编译器不再将构造函数名视为注入类名：当构造函数名出现在类模板特殊化的别名之后的限定名中。 以前，可以使用构造函数作为类型名称来声明其他实体。 下面的示例现在生成 C3646:

```cpp
#include <chrono>

class Foo {
   std::chrono::milliseconds::duration TotalDuration{}; // C3646: 'TotalDuration': unknown override specifier
};
```

要避免该错误，请将 `TotalDuration` 声明为以下形式：

```cpp
#include <chrono>

class Foo {
  std::chrono::milliseconds TotalDuration {};
};
```

### <a name="stricter-checking-of-extern-c-functions"></a>严格检查 `extern "C"` 函数

以前，如果在不同的命名空间中声明了 `extern "C"` 函数，旧版 Microsoft C++ 编译器不会检查这些声明是否兼容。 从 Visual Studio 2019 版本 16.3 开始，编译器会检查兼容性。 在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，以下代码生成错误 C2371 和 C2733：

```cpp
using BOOL = int;

namespace N
{
   extern "C" void f(int, int, int, bool);
}

void g()
{
   N::f(0, 1, 2, false);
}

extern "C" void f(int, int, int, BOOL){}
    // C2116: 'N::f': function parameter lists do not match between declarations
    // C2733: 'f': you cannot overload a function with 'extern "C"' linkage
```

若要避免上一示例中的错误，请在 `f` 的两个声明中一致地使用 `bool`（而不是 `BOOL`）。

### <a name="standard-library-improvements"></a>标准库改进

已删除非标准标头 \<stdexcpt.h> 和 \<typeinfo.h>。 包含它们的代码应改为分别包括标准标头 \<exception> 和 \<typeinfo>。

## <a name="conformance-improvements-in-visual-studio-2019-version-164"></a><a name="improvements_164"></a> Visual Studio 2019 版本 16.4 的符合性改进

### <a name="better-enforcement-of-two-phase-name-lookup-for-qualified-ids-in-permissive-"></a>改进了强制执行在 `/permissive-` 中对限定 ID 进行两阶段名称查找

两阶段名称查找要求模板正文中使用的非相关名称必须在定义时对模板可见。 以前，当模板实例化时，可找到此类名称。 在此更改发布后，可以轻松地在 MSVC 中的 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 标志下编写可移植的符合性代码。

在包含 `/permissive-` 标志集的 Visual Studio 2019 版本 16.4 中，以下示例生成错误，因为在定义 `f<T>` 模板时 `N::f` 不可见：

```cpp
template <class T>
int f() {
    return N::f() + T{}; // error C2039: 'f': is not a member of 'N'
}

namespace N {
    int f() { return 42; }
}
```

通常，通过添加缺少的头或前向声明函数/变量，可以修复此错误，如以下示例所示：

```cpp
namespace N {
    int f();
}

template <class T>
int f() {
    return N::f() + T{};
}

namespace N {
    int f() { return 42; }
}
```

### <a name="implicit-conversion-of-integral-constant-expressions-to-null-pointer"></a>将整数常量表达式隐式转换为 null 指针

现在，MSVC 编译器在符合性模式 (`/permissive-`) 下实现 [CWG 问题 903](https://wg21.link/cwg903)。 此规则不允许将整数常量表达式（整数文本“0”除外）隐式转换为 null 指针常量。 以下示例在符合模式下生成 C2440：

```cpp
int* f(bool* p) {
    p = false; // error C2440: '=': cannot convert from 'bool' to 'bool *'
    p = 0; // OK
    return false; // error C2440: 'return': cannot convert from 'bool' to 'int *'
}
```

若要修复此错误，请使用 `nullptr`，而不是 `false`。 文本 0 仍是允许的：

```cpp
int* f(bool* p) {
    p = nullptr; // OK
    p = 0; // OK
    return nullptr; // OK
}
```

### <a name="standard-rules-for-types-of-integer-literals"></a>用于整数文本类型的标准准则

在符合性模式（由 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 启用）下，MSVC 将标准规则用于整数文本类型。 十进制文本过大而无法适应 `signed int`，以前为其提供类型 `unsigned int`。 现在为此类文本提供第二大 `signed` 整型类型 `long long`。 此外，如果带“ll”后缀的文本过大而无法适应 `signed` 类型，则会为其提供类型 `unsigned long long`。

此更改会导致生成不同的警告诊断，以及对文本执行的算术运算出现行为差异。

以下示例展示了 Visual Studio 2019 版本 16.4 中的新行为。 现在，`i` 变量的类型为 `unsigned int`。 这就是抛出警告的原因所在。 变量 `j` 的高阶位设置为 0。

```cpp
void f(int r) {
    int i = 2964557531; // warning C4309: truncation of constant value
    long long j = 0x8000000000000000ll >> r; // literal is now unsigned, shift will fill high-order bits with 0
}
```

以下示例展示了如何保留旧行为，并避免警告抛出和运行时行为更改：

```cpp
void f(int r) {
int i = 2964557531u; // OK
long long j = (long long)0x8000000000000000ll >> r; // shift will keep high-order bits
}
```

### <a name="function-parameters-that-shadow-template-parameters"></a>隐藏模板参数的函数参数

现在，当函数参数隐藏模板参数时，MSVC 编译器会引发错误：

```cpp
template<typename T>
void f(T* buffer, int size, int& size_read);

template<typename T, int Size>
void f(T(&buffer)[Size], int& Size) // error C7576: declaration of 'Size' shadows a template parameter
{
    return f(buffer, Size, Size);
}
```

若要修复此错误，请更改以下参数之一的名称：

```cpp
template<typename T>
void f(T* buffer, int size, int& size_read);

template<typename T, int Size>
void f(T (&buffer)[Size], int& size_read)
{
    return f(buffer, Size, size_read);
}
```

### <a name="user-provided-specializations-of-type-traits"></a>用户提供的类型特征专用化

按照标准的 meta.rqmts 子句，现在当 MSVC 编译器在 `std` 命名空间中发现指定 `type_traits` 模板之一的一个用户定义专用化时，会引发错误。 若未另行指定，此类专用化会生成未定义的行为。 以下示例包含未定义的行为，因为它违反了规则，且 `static_assert` 失败，错误为 C2338。

```cpp
#include <type_traits>
struct S;

template<>
struct std::is_fundamental<S> : std::true_type {};

static_assert(std::is_fundamental<S>::value, "fail");
```

若要避免此错误，请定义继承自首选 `type_trait` 的结构，并将其专用化：

```cpp
#include <type_traits>

struct S;

template<typename T>
struct my_is_fundamental : std::is_fundamental<T> {};

template<>
struct my_is_fundamental<S> : std::true_type { };

static_assert(my_is_fundamental<S>::value, "fail");
```

### <a name="changes-to-compiler-provided-comparison-operators"></a>对编译器提供的比较运算符的更改

现在，如果 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 选项已启用，MSVC 编译器按照 [P1630R1](https://wg21.link/p1630r1) 对比较运算符实现以下更改：

如果表达式的返回类型不是 `bool`，编译器将不再使用 `operator==` 重写它们。 以下代码现在生成错误 C2088：

```cpp
struct U {
    operator bool() const;
};

struct S {
    U operator==(const S&) const;
};

bool neq(const S& lhs, const S& rhs) {
    return lhs != rhs;  // C2088: '!=': illegal for struct
}
```

若要避免此错误，必须显式定义所需的运算符：

```cpp
struct U {
    operator bool() const;
};

struct S {
    U operator==(const S&) const;
    U operator!=(const S&) const;
};

bool neq(const S& lhs, const S& rhs) {
    return lhs != rhs;
}
```

若默认的比较运算符是类似联合的类的成员，则编译器不再定义它。 以下示例现在生成错误 C2120：

```cpp
#include <compare>

union S {
    int a;
    char b;
    auto operator<=>(const S&) const = default;
};

bool lt(const S& lhs, const S& rhs) {
    return lhs < rhs;
}
```

若要避免此错误，请为此运算符定义主体：

```cpp
#include <compare>

union S {
    int a;
    char b;
    auto operator<=>(const S&) const { ... }
};

bool lt(const S& lhs, const S& rhs) {
    return lhs < rhs;
}
```

若默认的比较运算符的类包含引用成员，编译器将不再定义它。 以下代码现在生成错误 C2120：

```cpp
#include <compare>

struct U {
    int& a;
    auto operator<=>(const U&) const = default;
};

bool lt(const U& lhs, const U& rhs) {
    return lhs < rhs;
}
```

若要避免此错误，请为此运算符定义主体：

```cpp
#include <compare>

struct U {
    int& a;
    auto operator<=>(const U&) const { ... };
};

bool lt(const U& lhs, const U& rhs) {
    return lhs < rhs;
}
```

## <a name="conformance-improvements-in-visual-studio-2019-version-165"></a><a name="improvements_165"></a> Visual Studio 2019 版本 16.5 的符合性改进

### <a name="explicit-specialization-declaration-without-an-initializer-isnt-a-definition"></a>没有初始值设定项的显式专用化声明不是定义

在 `/permissive-` 下，MSVC 现强制实施一个标准规则，规定没有初始化表达式的显式专用化声明不是定义。 之前，该声明被视为具有默认初始化表达式的定义。 由于据此行为，程序现在可能具有无法解析的符号，因此可在链接时间观察效果。 此示例现在会导致一个错误：

```cpp
template <typename> struct S {
    static int a;
};

// In permissive-, this declaration isn't a definition, and the program won't link.
template <> int S<char>::a;

int main() {
    return S<char>::a;
}
```

```Output
error LNK2019: unresolved external symbol "public: static int S<char>::a" (?a@?$S@D@@2HA) referenced in function _main at link time.
```

若要解决此问题，请添加初始化表达式：

```cpp
template <typename> struct S {
    static int a;
};

// Add an initializer for the declaration to be a definition.
template <> int S<char>::a{};

int main() {
    return S<char>::a;
}
```

### <a name="preprocessor-output-preserves-newlines"></a>预处理器输出保留换行符

在将 `/P` 或 `/E` 与 `/experimental:preprocessor` 结合使用时，试验预处理器现在会保留换行符和空格。

给定以下示例源：

```cpp
#define m()
line m(
) line
```

以前的 `/E` 输出为：

```Output
line line
#line 2
```

现在 `/E` 的新输出为：

```Output
line
 line
```

### <a name="import-and-module-keywords-are-context-dependent"></a>`import` 和 `module` 关键字是上下文相关的

根据 [P1857R1](https://wg21.link/P1857R1)，`import` 和 `module` 预处理器指令对其语法具有新的限制。 此示例不再编译：

```cpp
import // Invalid
m;     // error C2146: syntax error: missing ';' before identifier 'm'
```

若要解决此问题，请将 import 保留在同一行上：

```cpp
import m; // OK
```

### <a name="removal-of-stdweak_equality-and-stdstrong_equality"></a>删除 `std::weak_equality` 和 `std::strong_equality`

[P1959R0](https://wg21.link/P1959R0) 中的合并要求编译器删除行为和对 `std::weak_equality` 和 `std::strong_equality` 类型的引用。

此示例中的代码不再编译：

```cpp
#include <compare>

struct S {
    std::strong_equality operator<=>(const S&) const = default;
};

void f() {
    nullptr<=>nullptr;
    &f <=> &f;
    &S::operator<=> <=> &S::operator<=>;
}
```

该示例现在导致以下错误：

```Output
error C2039: 'strong_equality': is not a member of 'std'
error C2143: syntax error: missing ';' before '<=>'
error C4430: missing type specifier - int assumed. Note: C++ does not support default-int
error C4430: missing type specifier - int assumed. Note: C++ does not support default-int
error C7546: binary operator '<=>': unsupported operand types 'nullptr' and 'nullptr'
error C7546: binary operator '<=>': unsupported operand types 'void (__cdecl *)(void)' and 'void (__cdecl *)(void)'
error C7546: binary operator '<=>': unsupported operand types 'int (__thiscall S::* )(const S &) const' and 'int (__thiscall S::* )(const S &) const'
```

若要解决此问题，请更新以 prefer 内置关系运算符并替换删除的类型：

```cpp
#include <compare>

struct S {
    std::strong_ordering operator<=>(const S&) const = default; // prefer 'std::strong_ordering'
};

void f() {
    nullptr != nullptr; // use pre-existing builtin operator != or ==.
    &f != &f;
    &S::operator<=> != &S::operator<=>;
}
```

### <a name="tls-guard-changes"></a>TLS 防护更改

以前，DLL 中的线程局部变量未正确初始化。 它们在第一次用于加载 DLL 之前存在的线程（而非加载 DLL 的线程）上未初始化。 此缺陷现已更正。 此类 DLL 中的线程局部变量在第一次用于此类线程之前即会初始化。

通过使用 `/Zc:tlsGuards-` 编译器开关，可以禁用“针对使用线程局部变量的初始化进行测试”这一全新行为。 或者，还可以通过将 `[[msvc:no_tls_guard]]` 特性添加到特定的线程局部变量中。

### <a name="better-diagnosis-of-call-to-deleted-functions"></a>更好地诊断对删除函数的调用

以前，我们的编译器对删除函数的调用更宽松。 例如，如果调用发生在模板主体的上下文中，则我们不会诊断该调用。 此外，如果有多个对删除函数调用的实例，只需发出一条诊断。 现在，我们为每个用户发出一条诊断。

此新行为的一个结果可能会产生较小的中断性变更：如果代码生成从不需要对删除函数调用的代码，则不会对其进行诊断。 现在，我们预先对其进行诊断。

此示例显示现在会生成错误的代码：

```cpp
struct S {
  S() = delete;
  S(int) { }
};

struct U {
  U() = delete;
  U(int i): s{ i } { }

  S s{};
};

U u{ 0 };
```

```Output
error C2280: 'S::S(void)': attempting to reference a deleted function
note: see declaration of 'S::S'
note: 'S::S(void)': function was explicitly deleted
```

若要解决此问题，请删除对删除函数的调用：

```cpp
struct S {
  S() = delete;
  S(int) { }
};

struct U {
  U() = delete;
  U(int i): s{ i } { }

  S s;  // Do not call the deleted ctor of 'S'.
};

U u{ 0 };
```

## <a name="conformance-improvements-in-visual-studio-2019-version-166"></a><a name="improvements_166"></a> Visual Studio 2019 版本 16.6 中的符合性改进

### <a name="standard-library-streams-reject-insertions-of-mis-encoded-character-types"></a>标准库流拒绝插入错误编码的字符类型

传统上，将 `wchar_t` 插入到 `std::ostream` 中以及将 `char16_t` 或 `char32_t` 插入到 `std::ostream` 或 `std::wostream` 中会输出其整型值。 将指针插入到这些字符类型会输出指针值。 程序员认为这两种情况都不直观。 他们通常希望标准库改为转码字符或生成以 null 结尾的字符串，并输出结果。

C++20 提议 [P1423R3](https://wg21.link/p1423r3) 为流和字符或字符指针类型的这些组合添加已删除的流插入运算符重载。 在 `/std:c++latest` 下，重载导致这些插入不规范，而不是采用一种可能是非预期的行为方式。 当找到一个时，编译器就会抛出错误 C2280。 可以将“安全门”宏 `_HAS_STREAM_INSERTION_OPERATORS_DELETED_IN_CXX20` 定义为 `1`，以还原旧行为。 （此建议还删除 `char8_t` 的流插入运算符。 标准库在我们添加 `char8_t` 支持时实现了类似的重载，因此 `char8_t` 永远不会出现“错误”行为。）

以下示例展示了此更改的行为：

```cpp
#include <iostream>
int main() {
    const wchar_t cw = L'x', *pw = L"meow";
    const char16_t c16 = u'x', *p16 = u"meow";
    const char32_t c32 = U'x', *p32 = U"meow";
    std::cout << cw << ' ' << pw << '\n';
    std::cout << c16 << ' ' << p16 << '\n';
    std::cout << c32 << ' ' << p32 << '\n';
    std::wcout << c16 << ' ' << p16 << '\n';
    std::wcout << c32 << ' ' << p32 << '\n';
}
```

此代码现在生成以下诊断消息：

```Output
error C2280: 'std::basic_ostream<char,std::char_traits<char>> &std::<<<std::char_traits<char>>(std::basic_ostream<char,std::char_traits<char>> &,wchar_t)': attempting to reference a deleted function
error C2280: 'std::basic_ostream<char,std::char_traits<char>> &std::<<<std::char_traits<char>>(std::basic_ostream<char,std::char_traits<char>> &,char16_t)': attempting to reference a deleted function
error C2280: 'std::basic_ostream<char,std::char_traits<char>> &std::<<<std::char_traits<char>>(std::basic_ostream<char,std::char_traits<char>> &,char32_t)': attempting to reference a deleted function
error C2280: 'std::basic_ostream<wchar_t,std::char_traits<wchar_t>> &std::<<<std::char_traits<wchar_t>>(std::basic_ostream<wchar_t,std::char_traits<wchar_t>> &,char16_t)': attempting to reference a deleted function
error C2280: 'std::basic_ostream<wchar_t,std::char_traits<wchar_t>> &std::<<<std::char_traits<wchar_t>>(std::basic_ostream<wchar_t,std::char_traits<wchar_t>> &,char32_t)': attempting to reference a deleted function
```

通过将字符类型转换为 `unsigned int`，或将指针到字符类型转换为 `const void*`，可以在所有语言模式下实现旧行为的效果：

```c++
#include <iostream>
int main() {
    const wchar_t cw = L'x', *pw = L"meow";
    const char16_t c16 = u'x', *p16 = u"meow";
    const char32_t c32 = U'x', *p32 = U"meow";
    std::cout << (unsigned)cw << ' ' << (const void*)pw << '\n'; // Outputs "120 0052B1C0"
    std::cout << (unsigned)c16 << ' ' << (const void*)p16 << '\n'; // Outputs "120 0052B1CC"
    std::cout << (unsigned)c32 << ' ' << (const void*)p32 << '\n'; // Outputs "120 0052B1D8"
    std::wcout << (unsigned)c16 << ' ' << (const void*)p16 << '\n'; // Outputs "120 0052B1CC"
    std::wcout << (unsigned)c32 << ' ' << (const void*)p32 << '\n'; // Outputs "120 0052B1D8"
}
```

### <a name="changed-return-type-of-stdpow-for-stdcomplex"></a>为 `std::complex` 更改了 `std::pow()` 的返回类型

以前，函数模板 `std::pow()` 的返回类型适用的升级规则的 MSVC 实现是不正确的。 例如，以前 `pow(complex<float>, int)` 返回 `complex<float>`。 现在，它正确返回 `complex<double>`。 在 Visual Studio 2019 版本 16.6 中，已经无条件地为所有标准模式实现了此修补程序。

此更改可能会导致编译器错误抛出。 例如，以前可以用 `pow(complex<float>, int)` 乘以 `float`。 由于 `complex<T> operator*` 需要相同类型的参数，因此以下示例现在发出编译器错误 C2676：

```cpp
// pow_error.cpp
// compile by using: cl /EHsc /nologo /W4 pow_error.cpp
#include <complex>

int main() {
    std::complex<float> cf(2.0f, 0.0f);
    (void) (std::pow(cf, -1) * 3.0f);
}
```

```Output
pow_error.cpp(7): error C2676: binary '*': 'std::complex<double>' does not define this operator or a conversion to a type acceptable to the predefined operator
```

有许多可能的修补程序：

- 将 `float` 被乘数的类型更改为 `double`。 此参数可以直接转换为 `complex<double>`，以匹配 `pow` 返回的类型。

- 通过指出 `complex<float>{pow(ARG, ARG)}`，将 `pow` 的结果窄化为 `complex<float>`。 然后，可以继续乘以 `float` 值。

- 将 `float`（而不是 `int`）传递给 `pow`。 此运算可能会慢一些。

- 在某些情况下，可以完全避免 `pow`。 例如，可以用除法替换 `pow(cf, -1)`。

### <a name="switch-related-warnings-for-c"></a>C 的开关相关警告

自 Visual Studio 2019 版本 16.6 起，编译器为编译为 C 的代码实现了一些预先存在的 C++ 警告。现已在不同级别启用了以下警告：C4060、C4061、C4062、C4063、C4064、C4065、C4808 和 C4809。 警告 C4065 和 C4060 在 C 中默认是禁用的。

这些警告在缺少 `case` 语句、`enum` 未定义以及 `bool` 开关错误（即包含过多案例）时触发。 例如：

```c
#include <stdbool.h>

int main() {
    bool b = true;
    switch (b) {
        case true: break;
        case false: break;
        default: break; // C4809: switch statement has redundant 'default' label;
                        // all possible 'case' labels are given
    }
}
```

若要修复此代码，请删除冗余 `default` 案例：

```c
#include <stdbool.h>

int main() {
    bool b = true;
    switch (b) {
        case true: break;
        case false: break;
    }
}
```

### <a name="unnamed-classes-in-typedef-declarations"></a>`typedef` 声明中未命名的类

自 Visual Studio 2019 版本 16.6 起，`typedef` 声明的行为被限制为符合 [P1766R1](https://wg21.link/P1766R1)。 在此更新中，`typedef` 声明中未命名的类除了以下成员之外不能有其他任何成员：

- 非静态数据成员；
- 成员类；
- 成员枚举；以及
- 默认成员初始值设定项。

相同的限制以递归方式应用于每个嵌套类。 此限制旨在确保具有用于链接目的的 `typedef` 名称的结构的简单性。 它们必须足够简单，在编译器获取用于链接目的的 `typedef` 名称前，不需要进行任何链接计算。

此更改会影响编译器的所有标准模式。 在默认 (`/std:c++14`) 和 `/std:c++17` 模式下，编译器针对非符合性代码抛出警告 C5208。 如果指定的是 `/permissive-`，编译器在 `/std:c++14` 下抛出警告 C5208 作为错误，并在 `/std:c++17` 下抛出错误 C7626。 如果指定的是 `/std:c++latest`，编译器针对非符合性代码抛出错误 C7626。

以下示例展示了未命名的结构中不再允许使用的构造。 将抛出 C5208 或 C7626 错误或警告，具体视指定的标准模式而定：

```cpp
struct B { };
typedef struct : B { // inheriting from 'B'; ill-formed
    void f(); // ill-formed
    static int i; // ill-formed
    struct U {
        void f(); // nested class has non-data member; ill-formed
    };
    int j = 10; // default member initializer; ill-formed
} S;
```

若要修复上面的代码，可以为未命名的类命名：

```cpp
struct B { };
typedef struct S_ : B {
    void f();
    static int i;
    struct U {
        void f();
    };
    int j = 10;
} S;
```

### <a name="default-argument-import-in-ccli"></a>C++/CLI 中的默认参数导入

越来越多的 API 在 .NET Core 中具有默认参数。 这就是为什么我们现在支持 C++/CLI 中的默认参数导入。 此更改可能会破坏声明多个重载的现有代码，如以下示例所示：

```cpp
public class R {
    public void Func(string s) {}   // overload 1
    public void Func(string s, string s2 = "") {} // overload 2;
}
```

当此类导入 C++/CLI 时，对其中一个重载的调用会导致错误抛出：

```cpp
    (gcnew R)->Func("abc"); // error C2668: 'R::Func' ambiguous call to overloaded function
```

编译器抛出错误 C2668，因为两个重载都与此参数列表匹配。 在第二个重载中，第二个参数由默认参数填充。 若要解决此问题，可以删除冗余重载 (1)。 或者，使用完整参数列表，并显式提供默认参数。

## <a name="conformance-improvements-in-visual-studio-2019-version-167"></a><a name="improvements_167"></a> Visual Studio 2019 版本 16.7 中的符合性改进

### <a name="is-trivially-copyable-definition"></a>可复制定义

C++ 20 更改了可复制定义。 如果类具有 **`volatile`** 限定类型的非静态数据成员，则它不再意味着任何编译器生成的复制或移动构造函数、或者复制或移动赋值运算符都是重要的。 C++ 标准委员会将此更改以追溯方式应用为缺陷报告。 在 MSVC 中，编译器行为不会在不同的语言模式下发生更改，如 `/std:c++14` 或 `/std:c++latest`。

下面是该新行为的示例：

```cpp
#include <type_traits>

struct S
{
    volatile int m;
};

static_assert(std::is_trivially_copyable_v<S>, "Meow!");
```

此代码不会在 Visual Studio 2019 版本 16.7 之前的 MSVC 版本中进行编译。 可以使用默认关闭的编译器警告来检测此更改。 如果使用 `cl /W4 /w45220` 编译以上代码，则会看到以下警告：

```Output
warning C5220: `'S::m': a non-static data member with a volatile qualified type no longer implies that compiler generated copy/move constructors and copy/move assignment operators are non trivial`
```

### <a name="pointer-to-member-and-string-literal-conversions-to-bool-are-narrowing"></a>指向成员的指针和字符串文本到 `bool` 的转换正在收缩

C++ 标准委员会最近采用了缺陷报告 [P1957R2](https://wg21.link/p1957r2)，它将 `T*` 到 `bool` 视为收缩转换。 MSVC 修复了其实现中的一个 bug，该 bug 之前将 `T*` 到 `bool` 诊断为收缩，但未诊断字符串文本到 `bool` 的转换或指向成员的指针到 `bool` 的转换。

以下程序在 Visual Studio 2019 版本 16.7 中格式不正确：

```cpp
struct X { bool b; };
void f(X);

int main() {
    f(X { "whoops?" }); // error: conversion from 'const char [8]' to 'bool' requires a narrowing conversion

    int (X::* p) = nullptr;
    f(X { p }); // error: conversion from 'int X::*' to 'bool' requires a narrowing conversion
}
```

若要更正此代码，请将显式比较添加到 `nullptr`，或者避免使用收缩转换的格式不正确的上下文：

```cpp
struct X { bool b; };
void f(X);

int main() {
    f(X { "whoops?" != nullptr }); // Absurd, but OK

    int (X::* p) = nullptr;
    f(X { p != nullptr }); // OK
}
```

### <a name="nullptr_t-is-only-convertible-to-bool-as-a-direct-initialization"></a>`nullptr_t` 只能以直接初始化形式转换为 `bool`

在 C++ 11 中，`nullptr` 只能以直接转换的形式转换为 `bool`；例如，使用大括号内的初始化表达式列表初始化 `bool` 时。 MSVC 不会强制实施此限制。 MSVC 现在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 下实现规则。 隐式转换现被诊断为格式错误。 仍允许对 `bool` 进行上下文转换，因为直接初始化 `bool b(nullptr)` 是有效的。

在大多数情况下，可以通过将 `nullptr` 替换为 `false` 来修复该错误，如以下示例所示：

```cpp
struct S { bool b; };
void g(bool);
bool h() { return nullptr; } // error, should be 'return false;'

int main() {
    bool b1 = nullptr; // error: cannot convert from 'nullptr' to 'bool'
    S s { nullptr }; // error: cannot convert from 'nullptr' to 'bool'
    g(nullptr); // error: cannot convert argument 1 from 'nullptr' to 'bool'

    bool b2 { nullptr }; // OK: Direct-initialization
    if (!nullptr) {} // OK: Contextual conversion to bool
}
```

### <a name="conforming-initialization-behavior-for-array-initializations-with-missing-initializers"></a>在缺少初始化表达式时符合数组初始化的初始化行为

以前，MSVC 为缺少初始化表达式的数组初始化提供了不一致行为。 对于没有初始化表达式的每个数组元素，MSVC 始终调用默认构造函数。 标准行为是使用空的大括号内初始化表达式列表 (`{}`) 来初始化每个元素。 空大括号内初始化表达式列表的初始化上下文为复制初始化，这不允许调用显式构造函数。 也可能存在运行时差异，因为使用 `{}` 进行初始化可能会调用采用 `std::initializer_list` 的构造函数（而不是默认构造函数）。 已在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 下启用一致行为。

下面是更改后的行为示例：

```cpp
struct B {
    explicit B() {}
};

void f() {
    B b1[1]{}; // Error in /permissive-, because aggregate init calls explicit ctor
    B b2[1]; // OK: calls default ctor for each array element
}
```

### <a name="initialization-of-class-members-with-overloaded-names-is-correctly-sequenced"></a>具有重载名称的类成员的初始化进行了正确排序

我们发现，当类型名称也作为数据成员的名称重载时，类数据成员的内部表示形式中存在一个 bug。 此 bug 导致聚合初始化和成员初始化顺序不一致。 生成的初始化代码现在是正确的。 但是，此更改可能会导致无意中依赖于错误排序成员的源出现错误或警告，如以下示例所示：

```cpp
// Compiling with /w15038 now gives:
// warning C5038: data member 'Outer::Inner' will be initialized after data member 'Outer::v'
struct Outer {
    Outer(int i, int j) : Inner{ i }, v{ j } {}

    struct Inner { int x; };
    int v;
    Inner Inner; // 'Inner' is both a type name and data member name in the same scope
};
```

在以前的版本中，构造函数会在数据成员 `v` 之前错误初始化数据成员 `Inner`。 （C++ 标准要求的初始化顺序与成员的声明顺序相同）。 生成的代码遵循标准后，成员初始化列表顺序将出错。 编译器将为此示例生成一个警告。 若要修复此问题，请重新排列成员初始化表达式列表以反映声明顺序。

### <a name="overload-resolution-involving-integral-overloads-and-long-arguments"></a>涉及整数重载和 `long` 参数的重载决策

C++ 标准要求对以标准转换的形式从 `long` 到 `int` 的转换设置优先级。 以前的 MSVC 编译器将其错误地分级为整型提升，这会使重载决策的排名更高。 如果认为此排名不明确，则可能导致重载决策成功解析。

现在，编译器会在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下正确考虑排名。 正确诊断无效代码，如以下示例所示：

```cpp
void f(long long);
void f(int);

int main() {
    long x {};
    f(x); // error: 'f': ambiguous call to overloaded function
    f(static_cast<int>(x)); // OK
}
```

可以通过以下几种方式解决此问题：

- 在调用站点，将传递的参数类型更改为 `int`。 可以更改变量类型，也可以进行强制转换。

- 如果有多个调用站点，则可以添加另一个重载，该重载采用 `long` 参数。 在此函数中，强制转换参数并将其转发给 `int` 重载。

### <a name="use-of-undefined-variable-with-internal-linkage"></a>使用带有内部链接的未定义变量

在 Visual Studio 2019 16.7 版本之前，MSVC 接受了使用一个声明为 `extern` 的变量，该变量具有内部链接且未定义。 此类变量不能在任何其他翻译单元中定义，也不能构成有效的程序。 编译器现在会在编译时诊断此事例。 此错误类似于未定义的静态函数的错误。

```cpp
namespace {
    extern int x; // Not a definition, but has internal linkage because of the anonymous namespace
}

int main()
{
    return x; // Use of 'x' that no other translation unit can possibly define.
}
```

此程序以前未正确编译和链接，但现在将发出错误 C7631。

```Output
error C7631: '`anonymous-namespace'::x': variable with internal linkage declared but not defined
```

此类变量必须在使用它们的同一翻译单元中定义。 例如，可以提供显式初始化表达式或单独的定义。

### <a name="type-completeness-and-derived-to-base-pointer-conversions"></a>类型完整性和派生到基的指针转换

在 C++ 20 之前的 C++ 标准中，从派生类到基类的转换不要求派生类是完整的类类型。 C++ 标准委员会批准了适用于所有版本 C++ 语言的追溯缺陷报告更改。 此更改将转换过程与类型特征（如 `std::is_base_of`）对齐，这要求派生类是完整的类类型。

下面是一个示例：

```cpp
template<typename A, typename B>
struct check_derived_from
{
    static A a;
    static constexpr B* p = &a;
};

struct W { };
struct X { };
struct Y { };

// With this change this code will fail as Z1 is not a complete class type
struct Z1 : X, check_derived_from<Z1, X>
{
};

// This code failed before and it will still fail after this change
struct Z2 : check_derived_from<Z2, Y>, Y
{
};

// With this change this code will fail as Z3 is not a complete class type
struct Z3 : W
{
    check_derived_from<Z3, W> cdf;
};
```

此行为更改适用于 MSVC 的所有 C++ 语言模式，而不只是 `/std:c++latest`。

### <a name="narrowing-conversions-are-more-consistently-diagnosed"></a>收缩转换诊断更为一致

MSVC 对大括号内初始化表达式中的收缩转换发出警告。 以前，编译器不会诊断从较大的 `enum` 基础类型到缩小的整型类型的收缩转换。 （编译器错误地将其视为整型提升，而不是转换）。 如果是有意的收缩转换，则可以通过对初始化表达式参数使用 `static_cast` 来避免此警告。 或者，选择更大的目标整型类型。

下面是使用显式 `static_cast` 来解决此警告的示例：

```cpp
enum E : long long { e1 };
struct S { int i; };

void f(E e) {
    S s = { e }; // warning: conversion from 'E' to 'int' requires a narrowing conversion
    S s1 = { static_cast<int>(e) }; // Suppress warning with explicit conversion
}
```

## <a name="conformance-improvements-in-visual-studio-2019-version-168"></a><a name="improvements_168"></a> Visual Studio 2019 版本 16.8 中的符合性改进

### <a name="class-rvalue-used-as-lvalue-extension"></a>“类 rvalue 用作 lvalue”扩展

MSVC 具有允许将类 rvalue 用作 lvalue 的扩展。 此扩展未延长类 rvalue 的生存期，可能会导致未定义的运行时行为。 现在我们强制执行标准规则，并在 `/permissive-` 下禁止使用此扩展。
如果尚无法使用 `/permissive-`，可以使用 `/we4238` 显式禁止使用此扩展。 下面是一个示例：

```cpp
// Compiling with /permissive- now gives:
// error C2102: '&' requires l-value
struct S {};

S f();

void g()
{
    auto p1 = &(f()); // The temporary returned by 'f' is destructed after this statement. So 'p1' points to an invalid object.

    const auto &r = f(); // This extends the lifetime of the temporary returned by 'f'
    auto p2 = &r; // 'p2' points to a valid object
}
```

### <a name="explicit-specialization-in-non-namespace-scope-extension"></a>“非命名空间范围中的显式专用化”扩展

MSVC 曾具有允许非命名空间范围包含显式专用化的扩展。 推出 CWG 727 解决办法后，它现在成为了一个标准部分。 但是有一些行为差异。 我们已调整了编译器的行为，以便与标准一致。

```cpp
// Compiling with 'cl a.cpp b.cpp /permissive-' now gives:
//   error LNK2005: "public: void __thiscall S::f<int>(int)" (??$f@H@S@@QAEXH@Z) already defined in a.obj
// To fix the linker error,
// 1. Mark the explicit specialization with 'inline' explicitly. Or,
// 2. Move its definition to a source file.

// common.h
struct S {
    template<typename T> void f(T);
    template<> void f(int);
};

// This explicit specialization is implicitly inline in the default mode.
template<> void S::f(int) {}

// a.cpp
#include "common.h"

int main() {}

// b.cpp
#include "common.h"
```

### <a name="checking-for-abstract-class-types"></a>检查是否有抽象类类型

C++20 标准更改了编译器用于检测将抽象类类型用作函数参数的过程。 具体而言，这不再是 SFINAE 错误。 以前，如果编译器检测到某个函数模板的专用化将抽象类类型实例作为函数参数，则将该专用化视为格式不正确。 它无法被添加到可行的候选函数集中。 在 C++20 中，对抽象类类型参数的检查在调用函数之前不会发生。 结果是，用于进行编译的代码不会引发错误。 下面是一个示例：

```cpp
class Node {
public:
    int index() const;
};

class String : public Node {
public:
    virtual int size() const = 0;
};

class Identifier : public Node {
public:
    const String& string() const;
};

template<typename T>
int compare(T x, T y)
{
    return x < y ? -1 : (x > y ? 1 : 0);
}

int compare(const Node& x, const Node& y)
{
    return compare(x.index(), y.index());
}

int f(const Identifier& x, const String& y)
{
    return compare(x.string(), y);
}
```

以前，对 `compare` 的调用会尝试通过将 `String` 模板参数用于 `T` 来将函数模板 `compare` 专用化。 这样无法生成有效的专用化，因为 `String` 是抽象类。 唯一可行的候选项就是 `compare(const Node&, const Node&)`。 但是，在 C++20 中，对抽象类类型的检查在调用函数之前不会发生。 因此，专用化 `compare(String, String)` 会添加到一组可行候选项中，并被选为最佳候选项，因为从 `const String&` 转换到 `String` 的转换顺序比从 `const String&` 转换到 `const Node&` 更好。

在 C++20 中，修复此示例的一种可行方法是，使用概念；也就是说，将 `compare` 的定义更改为：

```cpp
template<typename T>
int compare(T x, T y) requires !std::is_abstract_v<T>
{
    return x < y ? -1 : (x > y ? 1 : 0);
}
```

如果无法使用 C++ 概念，也可以回退到 SFINAE：

```cpp
template<typename T, std::enable_if_t<!std::is_abstract_v<T>, int> = 0>
int compare(T x, T y)
{
    return x < y ? -1 : (x > y ? 1 : 0);
}
```

### <a name="support-for-p0960r3---allow-initializing-aggregates-from-a-parenthesized-list-of-values"></a>支持 P0960R3 - 允许从带圆括号的值列表初始化聚合

C++20 [P0960R3](https://wg21.link/P0960R3) 添加了对使用带圆括号的初始值设定项列表的初始化聚合的支持。 例如，在 C++20 中以下代码是有效的：

```cpp
struct S {
    int i;
    int j;
};

S s(1, 2);
```

此功能的大部分都是累加的，也就是说，代码现在编译之前未编译的内容。 不过，它确实更改了 `std::is_constructible` 的行为。 在 C++17 模式下，此 `static_assert` 会失败，但在 C++20 模式下它将成功：

```cpp
static_assert(std::is_constructible_v<S, int, int>, "Assertion failed!");
```

如果使用此类型特征来控制重载解析，它可能会导致 C++17 和 C++20 之间的行为发生变化。

### <a name="overload-resolution-involving-function-templates"></a>重载解析涉及函数模板

以前，编译器允许在 `/permissive-` 下编译一些不应编译的代码。 结果就是，编译器调用了错误的函数，导致运行时行为更改：

```cpp
int f(int);

namespace N
{
    using ::f;
    template<typename T>
    T f(T);
}

template<typename T>
void g(T&& t)
{
}

void h()
{
    using namespace N;
    g(f);
}
```

对 `g` 的调用使用包含 `::f` 和 `N::f` 两个函数的重载集。 由于 `N::f` 是函数模板，因此编译器应将函数参数视为未推导的上下文。 在这种情况下，这意味着对 `g` 的调用应失败，因为编译器无法推导模板参数 `T` 的类型。 遗憾的是，编译器已判定 `::f` 是执行函数调用的适当匹配项，它无法弃用这个事实。 编译器会生成代码，将 `::f` 用作参数来调用 `g`，而不会发出错误。

在许多情况下将 `::f` 用作函数参数就是用户所需的做法，鉴于此，只有使用 `/permissive-` 编译代码时我们才会发出错误。

### <a name="migrating-from-await-to-c20-coroutines"></a>从 `/await` 迁移到 C++20 协同例程

现在 `/std:c++latest` 下默认启用标准 C++20 协同例程。 它们与 `/await` 开关下的协同例程 TS 及支持不同。 从 `/await` 迁移到标准协同例程可能需要执行一些源更改。

#### <a name="non-standard-keywords"></a>非标准关键字

C++20 模式不支持旧的 `await` 和 `yield` 关键字。 代码必须改用 `co_await` 和 `co_yield`。 标准模式也不允许协同例程中使用 `return`。 协同例程中的每个 `return` 都必须使用 `co_return`。

```cpp
// /await
task f_legacy() {
    ...
    await g();
    return n;
}
// /std:c++latest
task f() {
    ...
    co_await g();
    co_return n;
}
```

#### <a name="types-of-initial_suspendfinal_suspend"></a>initial_suspend/final_suspend 的类型

在 `/await` 下，可将承诺初始函数和挂起函数声明为返回 `bool`。 此行为不是标准行为。 在 C++20 中，这些函数必须返回可等待类类型，通常情况下，如果函数之前返回了 `true`，则为一个普通的可等待类型 `std::suspend_always`，如果它返回了 `false`，则为 `std::suspend_never`。

```cpp
// /await
struct promise_type_legacy {
    bool initial_suspend() noexcept { return false; }
    bool final_suspend() noexcept { return true; }
    ...
};

// /std:c++latest
struct promise_type {
    auto initial_susepend() noexcept { return std::suspend_never{}; }
    auto final_suspend() noexcept { return std::suspend_always{}; }
    ...
};
```

#### <a name="type-of-yield_value"></a>`yield_value` 的类型

在 C++20 中，承诺 `yield_value` 函数必须返回一个可等待类型。 在 `/await` 模式中，允许 `yield_value` 函数返回 `void`，并且该函数始终会挂起。 可将此类函数替换为返回 `std::suspend_always` 的函数。

```cpp
// /await
struct promise_type_legacy {
    ...
    void yield_value(int x) { next = x; };
};

// /std:c++latest
struct promise_type {
    ...
    auto yield_value(int x) { next = x; return std::suspend_always{}; }
};
```

#### <a name="exception-handling-function"></a>异常处理函数

`/await` 支持不具有任何异常处理函数的承诺类型或具有采用 `std::exception_ptr` 的名为 `set_exception` 的异常处理函数的承诺类型。 在 C++20 中，承诺类型必须具有一个不带任何参数的名为 `unhandled_exception` 的函数。 如果需要，可以从 `std::current_exception` 中获取异常对象。

```cpp
// /await
struct promise_type_legacy {
    void set_exception(std::exception_ptr e) { saved_exception = e; }
    ...
};
// /std:c++latest
struct promise_type {
    void unhandled_exception() { saved_exception = std::current_exception(); }
    ...
};
```

#### <a name="deduced-return-types-of-coroutines-not-supported"></a>不支持协同例程的推导返回类型

C++20 不支持返回类型包含占位符类型（如 `auto`）的协同例程。 必须显式声明协同例程的返回类型。 在 `/await` 下，这些推导出的类型总是涉及实验类型并需要包含定义所需类型的标头：`std::experimental::task<T>`、`std::experimental::generator<T>` 或 `std::experimental::async_stream<T>` 之一。

```cpp
// /await
auto my_generator() {
    ...
    co_yield next;
};

// /std:c++latest
#include <experimental/generator>
std::experimental::generator<int> my_generator() {
    ...
    co_yield next;
};
```

#### <a name="return-type-of-return_value"></a>返回 `return_value` 的类型

承诺 `return_value` 函数的返回类型必须为 `void`。 在 `/await` 模式中，返回类型将被忽略，可为任意类型。 此诊断可帮助检测作者错误地假设 `return_value` 的返回值返回给调用方这样的细微错误。

```cpp
// /await
struct promise_type_legacy {
    ...
    int return_value(int x) { return x; } // incorrect, the return value of this function is unused and the value is lost.
};

// /std:c++latest
struct promise_type {
    ...
    void return_value(int x) { value = x; }; // save return value
};
```

#### <a name="return-object-conversion-behavior"></a>返回对象转换行为

如果协同例程声明的返回类型与承诺 `get_return_object` 函数的返回类型不匹配，则从 `get_return_object` 返回的对象将转换为协同例程的返回类型。 在 `/await` 下，将在协同例程主体执行前提前完成此转换。 在 `/std:c++latest` 中，向调用方返回值时会执行此转换。 它允许不在初始挂起点挂起的协同例程利用该协同例程主体中的 `get_return_object` 返回的对象。

#### <a name="coroutine-promise-parameters"></a>协同例程承诺参数

在 C++20 中，编译器尝试将协同例程参数（如果有）传递给承诺类型的构造函数。 如果此操作失败，它将使用默认构造函数重试。 在 `/await` 模式中，仅使用默认构造函数。 如果承诺包含多个构造函数，则此更改可能会导致行为差异。 或者，如果存在从协同例程参数到承诺类型的转换，也会导致行为差异。

```cpp
struct coro {
    struct promise_type {
        promise_type() { ... }
        promise_type(int x) { ... }
        ...
    };
};

coro f1(int x);

// Under /await the promise gets constructed using the default constructor.
// Under /std:c++latest the promise gets constructed using the 1-argument constructor.
f1(0);

struct Object {
template <typename T> operator T() { ... } // Converts to anything!
};

coro f2(Object o);

// Under /await the promise gets constructed using the default constructor
// Under /std:c++latest the promise gets copy- or move-constructed from the result of
// Object::operator coro::promise_type().
f2(Object{});
```

### <a name="permissive--and-c20-modules-are-on-by-default-under-stdclatest"></a>在 `/std:c++latest` 下，默认为启用 `/permissive-` 和 C++20 模块

在 `/std:c++latest` 下，默认为启用 C++20 模块支持。 有关此更改的详细信息以及根据条件将 `module` 和 `import` 视为关键字的场景，请参阅 [Visual Studio 2019 版本 16.8 中 MSVC 的标准 C++20 模块支持](https://devblogs.microsoft.com/cppblog/standard-c20-modules-support-with-msvc-in-visual-studio-2019-version-16-8/)。

`permissive-`  是模块支持的必备组件，现在指定 `/std:c++latest` 后即可启用它。 有关详细信息，请参阅 [`/permissive-`](../build/reference/permissive-standards-conformance.md)。

如果代码是之前在 `/std:c++latest` 下编译的并且需要不一致的编译器行为，则可指定 `permissive`  来禁用编译器的严格一致模式。 编译器选项必须出现在命令行参数列表中的 `/std:c++latest` 之后。 但是，如果检测到模块的使用，`permissive` 会引发错误：

> 错误 C1214：模块与通过“option”请求的非标准行为发生冲突

option 最常见的值为：

| 选项 | 说明 |
|--|--|
| **`/Zc:twoPhase-`** | 对于 C++20 模块，两阶段名称查找是必需的，并由 `permissive-` 包含。 |
| **`/Zc:hiddenFriend-`** | 对于 C++20 模块，标准隐藏朋友名称查找规则是必需的，并且由 `permissive-` 所包含。 |
| **`/Zc:preprocessor-`** | 仅 C++20 标头单元使用和创建需要符合性预处理器。 命名模块无需此选项。 |

若要使用 Visual Studio 随附的 `std.*` 模块，仍需使用 [`/experimental:module`](../build/reference/experimental-module.md) 选项，因为这些模块尚未标准化。

`/experimental:module` 选项也包含 `/Zc:twoPhase` 和 `/Zc:hiddenFriend`。 以前，如果只是占用了模块，使用模块编译的代码有时可以使用 `/Zc:twoPhase-` 编译。 现在不再支持此行为。

## <a name="conformance-improvements-in-visual-studio-2019-version-169"></a><a name="improvements_169"></a> Visual Studio 2019 版本 16.9 中的符合性改进

### <a name="copy-initialization-of-temporary-in-reference-direct-initialization"></a>引用直接初始化中的临时文件复制初始化

核心工作组问题 [CWG 2267](https://wg21.link/cwg2267) 处理了带圆括号的初始化表达式列表和大括号内初始化表达式列表之间的不一致问题。 解决方法是使这两种形式协调一致。

Visual Studio 2019 版本 16.9 实现了所有 `/std` 编译器模式下的已更改行为。 但是，因为它可能属于源中断性变更，所以仅在使用 `/permissive-` 编译代码时才受支持。

以下示例展示了行为更改：

```cpp
struct A { };

struct B {
    explicit B(const A&);
};

void f()
{
    A a;
    const B& b1(a);     // Always an error
    const B& b2{ a };   // Allowed before resolution to CWG 2267 was adopted: now an error
}
```

### <a name="destructor-characteristics-and-potentially-constructed-subobjects"></a>析构函数特性和潜在构造子对象

核心工作组问题 [CWG 2336](https://wg21.link/cwg2336) 涵盖了类中具有虚拟基类的析构函数的隐式异常规范的省略。 这种省略意味着派生类中的析构函数的异常规范可能比基类更弱，前提是该基类是抽象的并且具有 `virtual` 基类。

Visual Studio 2019 版本 16.9 实现了所有 `/std` 编译器模式下的已更改行为。

下面的示例展示了解释的更改：

```cpp
class V {
public:
    virtual ~V() noexcept(false);
};

class B : virtual V {
    virtual void foo () = 0;
    // BEFORE: implicitly defined virtual ~B() noexcept(true);
    // AFTER: implicitly defined virtual ~B() noexcept(false);
};

class D : B {
    virtual void foo ();
    // implicitly defined virtual ~D () noexcept(false);
};
```

在进行此更改之前，`B` 的隐式定义的析构函数是 `noexcept`，因为只考虑潜在构造子对象。 而且，基类 `V` 不是潜在构造子对象，因为它是 `virtual` 基类，并且 `B` 是抽象的。 但是，基类 `V` 是类 `D` 的一个潜在构造子对象，因此 `D::~D` 被确定为 `noexcept(false)`，导致派生类的异常规范比基类更弱。 这种解释是不安全的。 如果 B 派生的类的析构函数引发异常，则可能导致不正确的运行时行为。

进行此更改后，如果析构函数存在虚拟析构函数，且任何虚拟基类存在潜在引发的析构函数，则该析构函数也可能引发。

### <a name="similar-types-and-reference-binding"></a>类似类型和引用绑定

核心工作组问题 [CWG 2352](https://wg21.link/cwg2352) 处理引用绑定规则与类型相似性更改之间的不一致性。 早期的缺陷报告（例如 [CWG 330](https://wg21.link/cwg330)）中引入了不一致性问题。 进行此更改后，以前将引用绑定到临时文件的代码现在可以在所涉及的类型仅存在不同的 cv 限定符时直接绑定。

Visual Studio 2019 版本 16.9 实现了所有 `/std` 编译器模式下的已更改行为。 这可能属于源中断性变更。

以下示例展示了更改的行为：

```cpp
int *ptr;
const int *const &f() {
    return ptr; // Now returns a reference to 'ptr' directly.
    // Previously returned a reference to a temporary and emitted C4172
}
```

此更新可能会更改依赖于引入的临时文件的程序行为：

```cpp
int func() {
    int i1 = 13;
    int i2 = 23;
    
    int* iptr = &i1;
    int const * const&  iptrcref = iptr;

    // iptrcref is a reference to a pointer to i1 with value 13.
    if (*iptrcref != 13)
    {
        return 1;
    }
    
    // Now change what iptr points to.

    // Prior to CWG 2352 iptrcref should be bound to a temporary and still points to the value 13.
    // After CWG 2352 it is bound directly to iptr and now points to the value 23.
    iptr = &i2;
    if (*iptrcref != 23)
    {
        return 1;
    }

    return 0;
}
```

### <a name="zctwophase-and-zctwophase--switch-behavior-change"></a>`/Zc:twoPhase` 和 `/Zc:twoPhase-` 开关行为更改

通常情况下，MSVC 编译器开关的工作原理是最新的一个开关胜出。 遗憾的是，`/Zc:twoPhase` 和 `/Zc:twoPhase-` 开关不属于这种情况。 这些开关是“关联”的，所以较高版本的开关无法重写较低版本的开关。 例如：

`cl /Zc:twoPhase /permissive a.cpp`

在这种情况下，第一个 `/Zc:twoPhase` 开关启用严格的两阶段名称查找。 第二个开关是为了禁用严格的符合性模式（它与 `/permissive-` 相反），但它未禁用 `/Zc:twoPhase`。

Visual Studio 2019 版本 16.9 更改了所有 `/std` 编译器模式下的此行为。 `/Zc:twoPhase` 和 `/Zc:twoPhase-` 不再“关联”，更高版本的开关可以重写它们。

### <a name="explicit-noexcept-specifiers-on-destructor-templates"></a>析构函数模板上的显式 noexcept 说明符

编译器以前接受了一个用非引发异常规范声明的析构函数模板，但它是在没有使用显式 noexcept 说明符的情况下定义的。 析构函数的隐式异常规范取决于类的属性，在定义模板时可能不知道这些属性。 C++ 标准同样需要此行为：如果在未使用 noexcept 说明符的情况下声明了某个析构函数，则此析构函数具有隐式异常规范，并且此函数的其他任何声明都不具有 noexcept 说明符。

Visual Studio 2019 版本 16.9 更改了所有 `/std` 编译器模式下的符合性行为。

下面的示例展示了编译器行为的更改：

```cpp
template <typename T>
class B {
    virtual ~B() noexcept; // or throw()
};

template <typename T>
B<T>::~B() { /* ... */ } // Before: no diagnostic.
// Now diagnoses a definition mismatch. To fix, define the implementation by 
// using the same noexcept-specifier. For example,
// B<T>::~B() noexcept { /* ... */ }
```

### <a name="rewritten-expressions-in-c20"></a>C++20 中重写的表达式

自 Visual Studio 2019 版本 16.2 起，在 `/std:c++latest` 下，编译器已接受如下所示的代码：

```cpp
#include <compare>

struct S {
    auto operator<=>(const S&) const = default;
    operator bool() const;
};

bool f(S a, S b) {
    return a < b;
}
```

但是，编译器不会调用作者可能期望的比较函数。 上述代码应将 `a < b` 重写为 `(a <=> b) < 0`。 相反，编译器使用了 `operator bool()` 用户定义的转换函数并比较了 `bool(a) < bool(b)`。 从 Visual Studio 2019 版本16.9 开始，编译器将使用所需的宇宙飞船运算符表达式来重写表达式。

#### <a name="source-breaking-change"></a>源代码重大更改

对重写的表达式正确应用转换还有另一个作用：编译器还可以正确地诊断在尝试重写表达式时出现的歧义。 请看以下示例：

```cpp
struct Base {
    bool operator==(const Base&) const;
};

struct Derived : Base {
    Derived();
    Derived(const Base&);
    bool operator==(const Derived& rhs) const;
};

bool b = Base{} == Derived{};
```

在 C++17 中，将接受此代码，因为在表达式右侧有一个派生到基的转换 `Derived`。 在 C++20 中，还添加了综合表达式候选项：`Derived{} == Base{}`。 由于标准中基于转换的函数入选规则，因此无法决定在 `Base::operator==` 和 `Derived::operator==` 之间如何选择。 这是因为两个表达式中的转换序列难分好坏，因此示例代码会导致歧义。

要消除歧义，请添加一个不受这两个转换序列约束的新候选项：

```cpp
bool operator==(const Derived&, const Base&);
```

#### <a name="runtime-breaking-change"></a>运行时中断性变更

由于 C++20 中的运算符重写规则，重载解析可能会发现新的候选项，而在较低的语言模式下却找不到它。 而且，新的候选项可能比旧的候选项更适合。 请看以下示例：

```cpp
struct iterator;
struct const_iterator {
  const_iterator(const iterator&);
  bool operator==(const const_iterator &ci) const;
};

struct iterator {
  bool operator==(const const_iterator &ci) const { return ci == *this; }
};
```

在 C++17 中，`ci == *this` 的唯一候选项是 `const_iterator::operator==`。 这是一个匹配项，因为 `*this` 经历了到 `const_iterator` 的派生到基转换。 在 C++20 中，添加了另一个重写的候选项 `*this == ci`，它将调用 `iterator::operator==`。 此候选项不需要转换，因此它比 `const_iterator::operator==` 更适合。 新候选项的问题是，它是当前正在定义的函数，因此函数的新语义导致了 `iterator::operator==` 的无限递归定义。

为了帮助处理示例类似的代码，编译器实现了一个新的警告：

```cmd
$ cl /std:c++latest /c t.cpp
t.cpp
t.cpp(8): warning C5232: in C++20 this comparison calls 'bool iterator::operator ==(const const_iterator &) const' recursively
```

要修复代码，请明确要使用哪种转换：

```cpp
struct iterator {
  bool operator==(const const_iterator &ci) const { return ci == static_cast<const const_iterator&>(*this); }
};
```

## <a name="see-also"></a>请参阅

[Microsoft C++ 语言一致性表](visual-cpp-language-conformance.md)
