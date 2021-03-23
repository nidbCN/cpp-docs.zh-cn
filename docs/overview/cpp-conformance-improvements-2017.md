---
title: Visual Studio 2017 中 C++ 的符合性改进
description: Visual Studio 2017 中的 Microsoft C/C++ 正朝着完全符合 C++20 语言标准的方向发展。
ms.date: 03/10/2021
ms.technology: cpp-language
ms.openlocfilehash: b2f697148c7671dcc56a6fd27a53131d01e3b88f
ms.sourcegitcommit: f8ba5db09d05683b24c58505f0e57c21f85545dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103147268"
---
# <a name="c-conformance-improvements-behavior-changes-and-bug-fixes-in-visual-studio-2017"></a>Visual Studio 2017 中的 C++ 符合性改进、行为更改和 bug 修复

Visual Studio 中的 Microsoft C++ (MSVC) 在每个版本中进行了符合性改进和 bug 修复。 本文按主要版本然后次要版本列出了改进之处。 若要直接跳转到特定版本的更改，请使用本文下面的列表。

本文档列出了 Visual Studio 2017 中的更改。 有关 Visual Studio 2019 中的更改的指南，请参阅 [Visual Studio 2019 中的 C++ 符合性改进](cpp-conformance-improvements.md)。 有关之前符合性改进的完整列表，请参阅 [Visual C++ 2003 - 2015 中的新增功能](../porting/visual-cpp-what-s-new-2003-through-2015.md)。

## <a name="conformance-improvements-in-visual-studio-2017-rtw-version-150"></a><a name="improvements_150"></a>Visual Studio 2017 RTW（版本 15.0）中的符合性改进

有了对通用 `constexpr` 和用于聚合的非静态数据成员初始化 (NSDMI) 的支持，Visual Studio 2017 中的 MSVC 编译器现在已经完成了 C++14 标准中增加的功能要求。 但编译器仍缺少 C++11 和 C++98 标准版中的一些功能。 请参阅 [Microsoft C++ 语言一致性表](./visual-cpp-language-conformance.md)，获取显示编译器当前状态的表。

### <a name="c11-expression-sfinae-support-in-more-libraries"></a>C++11：在更多库中支持表达式 SFINAE

编译器持续改进对表达式 SFINAE 的支持。 当 `decltype` 和 `constexpr` 表达式可能作为模板参数出现时，需要在进行模板参数推导和替换时使用它。 有关详细信息，请参阅 [Visual Studio 2017 RC 中的表达式 SFINAE 改进之处](https://devblogs.microsoft.com/cppblog/expression-sfinae-improvements-in-vs-2015-update-3/)。

### <a name="c14-nsdmi-for-aggregates"></a>C++14：用于聚合的 NSDMI

聚合是一个数组或类，其中不包含用户提供的构造函数、专用或受保护的非静态数据成员、基类和虚拟函数。 自 C++14 起，聚合可能包含成员初始值设定项。 有关详细信息，请参阅 [Member initializers and aggregates](https://wg21.link/n3605)（成员初始值设定项和聚合）。

### <a name="c14-extended-constexpr"></a>C++14：扩展 `constexpr`

现在允许声明为 `constexpr` 的表达式包含某些种类的声明、if 和 switch 语句、loop 语句，以及在 `constexpr` 表达式求值期间开始生存期的对象的变异。 不再要求 `constexpr` 非静态成员函数必须是隐式的 `const`。 有关详细信息，请参阅[放松对 `constexpr` 函数的约束](https://wg21.link/n3652)。

### <a name="c17-terse-static_assert"></a>C++17：简要 `static_assert`

`static_assert` 的消息参数是可选的。 有关详细信息，请参阅 [N3928：扩展 static_assert, v2](https://wg21.link/n3928)。

### <a name="c17-fallthrough-attribute"></a>C++17：`[[fallthrough]]` 属性

在 `/std:c++17` 模式下，`[[fallthrough]]` 属性可以在 switch 语句的上下文中用作对预期发生贯穿行为的编译器的提示。 此属性可防止编译器在这类情况下发出警告。 有关详细信息，请参阅 [P0188R0 - `[[fallthrough]]` 属性的用词](https://wg21.link/p0188r0)。

### <a name="generalized-range-based-for-loops"></a>一般化的基于范围的 `for` 循环

基于范围的 `for` 循环不再需要 `begin()` 和 `end()` 返回相同类型的对象。 此更改使 `end()` 能够返回 [`range-v3`](https://github.com/ericniebler/range-v3) 中的范围和完成但尚未发布的范围技术规范使用的 sentinel。 有关详细信息，请参阅 [P0184R0 - 通用化基于范围的 `for` 循环](https://wg21.link/p0184r0)。

### <a name="copy-list-initialization"></a>复制列表初始化

Visual Studio 2017 正确地抛出与使用初始值设定项列表创建对象相关的编译器错误。 这些错误在 Visual Studio 2015 中没有被发现，它们可能会导致故障或未定义的运行时行为。 根据 [N4594 13.3.1.7p1](https://wg21.link/n4594)，在 `copy-list-initialization` 中，编译器需要考虑用于重载决策的显式构造函数。 但是，如果实际选择了该重载，则必须引发错误。

以下两个示例在 Visual Studio 2015 中编译，但在 Visual Studio 2017 中不编译。

```cpp
struct A
{
    explicit A(int) {}
    A(double) {}
};

int main()
{
    A a1 = { 1 }; // error C3445: copy-list-initialization of 'A' cannot use an explicit constructor
    const A& a2 = { 1 }; // error C2440: 'initializing': cannot convert from 'int' to 'const A &'

}
```

为更正此错误，应使用直接初始化：

```cpp
A a1{ 1 };
const A& a2{ 1 };
```

在 Visual Studio 2015 中，编译器以与常规复制初始化相同的方式错误地处理复制列表初始化：它只考虑将转换构造函数用于重载决策。 在以下示例中，Visual Studio 2015 选择 `MyInt(23)`。 Visual Studio 2017 正确引发错误。

```cpp
// From http://www.open-std.org/jtc1/sc22/wg21/docs/cwg_closed.html#1228
struct MyStore {
    explicit MyStore(int initialCapacity);
};

struct MyInt {
    MyInt(int i);
};

struct Printer {
    void operator()(MyStore const& s);
    void operator()(MyInt const& i);
};

void f() {
    Printer p;
    p({ 23 }); // C3066: there are multiple ways that an object
        // of this type can be called with these arguments
}
```

此示例与上一个示例类似，但引发了不同的错误。 它在 Visual Studio 2015 中成功，在 Visual Studio 2017 中失败 (C2668)。

```cpp
struct A {
    explicit A(int) {}
};

struct B {
    B(int) {}
};

void f(const A&) {}
void f(const B&) {}

int main()
{
    f({ 1 }); // error C2668: 'f': ambiguous call to overloaded function
}
```

### <a name="deprecated-typedefs"></a>弃用的的 Typedef

Visual Studio 2017 现在针对在类或结构中声明的已弃用 Typedef 发出正确警告。 在 Visual Studio 2015 中，编译以下示例时不会发出警告。 它在 Visual Studio 2017 中生成 C4996。

```cpp
struct A
{
    // also for __declspec(deprecated)
    [[deprecated]] typedef int inttype;
};

int main()
{
    A::inttype a = 0; // C4996 'A::inttype': was declared deprecated
}
```

### `constexpr`

当条件性运算操作的左侧操作数在 `constexpr` 上下文中无效时，Visual Studio 2017 会正确引发错误。 以下代码在 Visual Studio 2015 中进行编译，但在 Visual Studio 2017 中不进行编译，而是引发 C3615：

```cpp
template<int N>
struct array
{
    int size() const { return N; }
};

constexpr bool f(const array<1> &arr)
{
    return arr.size() == 10 || arr.size() == 11; // C3615 constexpr function 'f' cannot result in a constant expression
}
```

若要更正此错误，请将 `array::size()` 函数声明为 `constexpr`，或从 `f` 中删除 `constexpr` 限定符。

### <a name="class-types-passed-to-variadic-functions"></a>传递给 variadic 函数的类类型

在 Visual Studio 2017 中，传递给可变参数函数（如 `printf`）的类或结构必须是可复制的。 传递此类对象时，编译器只是执行按位复制，不会调用构造函数或析构函数。

```cpp
#include <atomic>
#include <memory>
#include <stdio.h>

int main()
{
    std::atomic<int> i(0);
    printf("%i\n", i); // error C4839: non-standard use of class 'std::atomic<int>'
                        // as an argument to a variadic function.
                        // note: the constructor and destructor will not be called;
                        // a bitwise copy of the class will be passed as the argument
                        // error C2280: 'std::atomic<int>::atomic(const std::atomic<int> &)':
                        // attempting to reference a deleted function

    struct S {
        S(int i) : i(i) {}
        S(const S& other) : i(other.i) {}
        operator int() { return i; }
    private:
        int i;
    } s(0);
    printf("%i\n", s); // warning C4840 : non-portable use of class 'main::S'
                      // as an argument to a variadic function
}
```

若要更正错误，可调用一种成员函数，该函数返回可完全复制的类型，

```cpp
    std::atomic<int> i(0);
    printf("%i\n", i.load());
```

或者使用静态强制转换，以在传递对象之前将其进行转换：

```cpp
    struct S {/* as before */} s(0);
    printf("%i\n", static_cast<int>(s))
```

对于使用 `CString` 生成和管理的字符串，提供的 `operator LPCTSTR()` 应用于将 `CString` 对象强制转换为格式字符串所需的 C 指针。

```cpp
CString str1;
CString str2 = _T("hello!");
str1.Format(_T("%s"), static_cast<LPCTSTR>(str2));
```

### <a name="cv-qualifiers-in-class-construction"></a>类构造中的 cv 限定符

在 Visual Studio 2015 中，编译器有时会在通过构造函数调用生成类对象时错误地忽略 cv 限定符。 此问题可能会导致故障或意外的运行时行为。 以下示例在 Visual Studio 2015 中编译，但在 Visual Studio 2017 中引发了编译器错误：

```cpp
struct S
{
    S(int);
    operator int();
};

int i = (const S)0; // error C2440
```

若要更正此错误，请将 `operator int()` 声明为 `const`。

### <a name="access-checking-on-qualified-names-in-templates"></a>对模板中限定名称的访问检查

在某些模板上下文中，旧版编译器不检查对限定名称的访问。 此问题可能会干扰预期的 SFINAE 行为，在这一行为中，由于名称的不可访问性，替换预期会失败。 这可能会导致在运行时发生故障或意外行为，因为编译器错误地调用了运算符的错误重载。 在 Visual Studio 2017 中，引发了编译器错误。 具体错误可能会有所不同，但通常引发的错误是 C2672“找不到匹配的重载函数”。 下列代码在 Visual Studio 2015 中进行编译，但在 Visual Studio 2017 中引发错误：

```cpp
#include <type_traits>

template <class T> class S {
    typedef typename T type;
};

template <class T, std::enable_if<std::is_integral<typename S<T>::type>::value, T> * = 0>
bool f(T x);

int main()
{
    f(10); // C2672: No matching overloaded function found.
}
```

### <a name="missing-template-argument-lists"></a>缺少的模板参数列表

在 Visual Studio 2015 和更早版本中，编译器不会诊断出所有缺失的模板参数列表。 它不会注意到模板参数列表中出现了缺失模板的情况：例如，当部分默认模板参数或非类型模板参数缺失时。 此问题可能导致不可预知的行为，包括编译器故障或意外的运行时行为。 下列代码在 Visual Studio 2015 中进行编译，但在 Visual Studio 2017 中引发错误。

```cpp
template <class T> class ListNode;
template <class T> using ListNodeMember = ListNode<T> T::*;
template <class T, ListNodeMember M> class ListHead; // C2955: 'ListNodeMember': use of alias
                                                     // template requires template argument list

// correct:  template <class T, ListNodeMember<T> M> class ListHead;
```

### <a name="expression-sfinae"></a>表达式 SFINAE

为了支持表达式 SFINAE，编译器现在声明模板（而不是实例化模板）时分析 `decltype` 参数。 因此，如果在 decltype 参数中找到非依赖专用化，则它不会被推迟到实例化时间。 而是被立即处理，并且在当时诊断产生的所有错误。

以下示例显示了在声明时引发的这类编译器错误：

```cpp
#include <utility>
template <class T, class ReturnT, class... ArgsT>
class IsCallable
{
public:
    struct BadType {};

    template <class U>
    static decltype(std::declval<T>()(std::declval<ArgsT>()...)) Test(int); //C2064. Should be declval<U>

    template <class U>
    static BadType Test(...);

    static constexpr bool value = std::is_convertible<decltype(Test<T>(0)), ReturnT>::value;
};

constexpr bool test1 = IsCallable<int(), int>::value;
static_assert(test1, "PASS1");
constexpr bool test2 = !IsCallable<int*, int>::value;
static_assert(test2, "PASS2");
```

### <a name="classes-declared-in-anonymous-namespaces"></a>在匿名命名空间内声明的类

根据 C++ 标准，在匿名命名空间内部声明的类具有内部链接，这意味着它不能导出。 在 Visual Studio 2015 及更早版本中，此规则不是强制执行的。 在 Visual Studio 2017 中，部分强制执行此规则。 在 Visual Studio 2017 中，以下示例抛出错误 C2201 

```cpp
struct __declspec(dllexport) S1 { virtual void f() {} };
  // C2201 const anonymous namespace::S1::vftable: must have external linkage
  // in order to be exported/imported.
```

### <a name="default-initializers-for-value-class-members-ccli"></a>值类成员的默认初始值设定项 (C++/CLI)

在 Visual Studio 2015 及更早版本中，编译器允许（但会忽略）值类成员的默认成员初始值设定项。 默认初始化值类始终零初始化成员。 不允许使用默认构造函数。 在 Visual Studio 2017 中，默认成员初始值设定项引发编译器错误，如下例所示：

```cpp
value struct V
{
    int i = 0; // error C3446: 'V::i': a default member initializer
               // isn't allowed for a member of a value class
};
```

### <a name="default-indexers-ccli"></a>默认索引器 (C++/CLI)

在 Visual Studio 2015 及更早版本中，编译器在某些情况下将默认属性误识别为默认索引器。 可以通过使用标识符 `default` 访问此属性来解决这个问题。 在将 `default` 作为关键字引入 C++11 后，解决方法本身就出现了问题。 在 Visual Studio 2017 中，修复了需要解决方法的缺陷。 现在，当 `default` 用于访问类的默认属性时，编译器会抛出错误。

```cpp
//class1.cs

using System.Reflection;
using System.Runtime.InteropServices;

namespace ClassLibrary1
{
    [DefaultMember("Value")]
    public class Class1
    {
        public int Value
        {
            // using attribute on the return type triggers the compiler bug
            [return: MarshalAs(UnmanagedType.I4)]
            get;
        }
    }
    [DefaultMember("Value")]
    public class Class2
    {
        public int Value
        {
            get;
        }
    }
}

// code.cpp
#using "class1.dll"

void f(ClassLibrary1::Class1 ^r1, ClassLibrary1::Class2 ^r2)
{
       r1->Value; // error
       r1->default;
       r2->Value;
       r2->default; // error
}
```

在 Visual Studio 2017 中，可以通过属性名称同时访问两个值属性：

```cpp
#using "class1.dll"

void f(ClassLibrary1::Class1 ^r1, ClassLibrary1::Class2 ^r2)
{
       r1->Value;
       r2->Value;
}
```

## <a name="conformance-improvements-in-153"></a><a name="improvements_153"></a> 15.3 中的符合性改进

### <a name="constexpr-lambdas"></a>`constexpr` Lambda

现在可以在常数表达式中使用 Lambda 表达式。 有关详细信息，请参阅 [C++ 中的 `constexpr` Lambda 表达式](../cpp/lambda-expressions-constexpr.md)。

### <a name="if-constexpr-in-function-templates"></a>函数模板中的 `if constexpr`

函数模板可能包含 `if constexpr` 语句，用于启用编译时分支。 有关详细信息，请参阅 [`if constexpr` 语句](../cpp/if-else-statement-cpp.md#if_constexpr)。

### <a name="selection-statements-with-initializers"></a>具有初始化表达式的选择语句

`if` 语句可以包含在语句本身内的块范围中引入变量的初始值设定项。 有关详细信息，请参阅[包含初始值设定项的 `if` 语句](../cpp/if-else-statement-cpp.md#if_with_init)。

### <a name="maybe_unused-and-nodiscard-attributes"></a>`[[maybe_unused]]` 和 `[[nodiscard]]` 属性

实体未使用时，将出现新属性 `[[maybe_unused]]` 静默处理警告。 如果放弃函数调用的返回值，`[[nodiscard]]` 属性将创建一条警告。 有关详细信息，请参阅 [C++ 中的属性](../cpp/attributes.md)。

### <a name="using-attribute-namespaces-without-repetition"></a>不重复使用属性命名空间

仅在属性列表中启用单个命名空间标识符的新语法。 有关详细信息，请参阅 [C++ 中的属性](../cpp/attributes.md)。

### <a name="structured-bindings"></a>结构化绑定

现在可以在一个声明中存储包含组件的各个名称的值，前提是值为数组、`std::tuple` 或 `std::pair` 或者包含所有公共非静态数据成员。 有关详细信息，请参阅 [P0144R0 - 结构化绑定](https://wg21.link/p0144r0)和[从一个函数返回多个值](../cpp/functions-cpp.md#multi_val)。

### <a name="construction-rules-for-enum-class-values"></a>`enum class` 值的构造规则

现在为非收缩的区分范围的枚举提供了隐式转换。 它将从区分范围的枚举的基础类型转换为枚举本身。 如果此转换的定义未引入枚举器，并且源使用列表初始化语法，则可以使用此转换。 有关详细信息，请参阅 [P0138R2 - `enum class` 值的构造规则](https://wg21.link/p0138r2)和[枚举](../cpp/enumerations-cpp.md#no_enumerators)。

### <a name="capturing-this-by-value"></a>按值捕获 `*this`

Lambda 表达式中的 `*this` 对象现在可以通过值捕获。 此更改可以在并行和异步操作中实现调用 lambda 的情况，特别是在较新的计算机体系结构中。 有关详细信息，请参阅 [P0018R3 - 通过值执行的 \*this 的 Lambda 捕获为 \[=,\*this\]](https://wg21.link/p0018r3)。

### <a name="removing-operator-for-bool"></a>为 `bool` 删除 `operator++`

`operator++` 在 `bool` 类型上不再受支持。 有关详细信息，请参阅 [P0002R1 - 删除已弃用的 operator++(bool)](https://wg21.link/p0002r1)。

### <a name="removing-deprecated-register-keyword"></a>删除弃用的 `register` 关键字

之前弃用（且被编译器忽略的）`register` 关键字现在已从语言中删除。 有关详细信息，请参阅 [P0001R1 - 删除已弃用的 `register` 关键字](https://wg21.link/p0001r1)。

### <a name="calls-to-deleted-member-templates"></a>调用的成员模板已遭删除

在旧版 Visual Studio 中，在某些情况下，编译器可能无法在对已删除的成员模板执行不规范的调用时抛出错误。 这些调用可能会在运行时导致故障发生。 以下代码现在生成 C2280：

```cpp
template<typename T>
struct S {
   template<typename U> static int f() = delete;
};

void g()
{
   decltype(S<int>::f<int>()) i; // this should fail with
// C2280: 'int S<int>::f<int>(void)': attempting to reference a deleted function
}
```

若要修复此错误，请将 `i` 声明为 `int`。

### <a name="pre-condition-checks-for-type-traits"></a>类型特征的前提条件检查

为了更严格地遵循标准，Visual Studio 2017 版本 15.3 改进了类型特征的前提条件检查。 此类检查验证的是类型是否可赋值。 以下代码在 Visual Studio 2017 版本 15.3 中生成错误 C2139：

```cpp
struct S;
enum E;

static_assert(!__is_assignable(S, S), "fail"); // C2139 in 15.3
static_assert(__is_convertible_to(E, E), "fail"); // C2139 in 15.3
```

### <a name="new-compiler-warning-and-runtime-checks-on-native-to-managed-marshaling"></a>有关从本机到托管的封送的新编译器警告和运行时检查

从托管函数到本机函数的调用需要执行封送。 虽然 CLR 会执行封送，但并不理解 C++ 语义。 如果通过值传递本机对象，CLR 要么调用对象的复制构造函数，要么使用 `BitBlt`，而这可能会导致未定义的运行时行为发生。

现在，如果编译器在编译时发现以下错误，将发出警告：含有已删除的复制构造函数的本机对象按值在本机和托管边界之间传递。 如果编译器在编译时不知晓，则插入运行时检查，以便在出现格式错误的封送时，程序能够立即调用 `std::terminate`。 在 Visual Studio 2017 版本 15.3 中，以下代码生成警告 C4606：

```cpp
class A
{
public:
   A() : p_(new int) {}
   ~A() { delete p_; }

   A(A const &) = delete;
   A(A &&rhs) {
   p_ = rhs.p_;
}

private:
   int *p_;
};

#pragma unmanaged

void f(A a)
{
}

#pragma managed

int main()
{
    // This call from managed to native requires marshaling. The CLR doesn't
    // understand C++ and uses BitBlt, which results in a double-free later.
    f(A()); // C4606 'A': passing argument by value across native and managed
    // boundary requires valid copy constructor. Otherwise, the runtime
    // behavior is undefined.`
}
```

若要修复此错误，请删除 `#pragma managed` 指令以将调用方标记为本机，并避免执行封送。

### <a name="experimental-api-warning-for-winrt"></a>WinRT 的实验性 API 警告

为了获取反馈而发布的实验性 WinRT API 使用 `Windows.Foundation.Metadata.ExperimentalAttribute` 进行修饰。 在 Visual Studio 2017 版本 15.3 中，编译器会针对此属性生成警告 C4698。 旧版 Windows SDK 中的一些 API 已使用此特性进行修饰，调用这些 API 现在会触发这一编译器警告。 更高版本的 Windows SDK 从所有已发布的类型中删除了此属性。 如果使用的是更低版本的 SDK，则需要取消针对已发布类型的所有调用抛出的这些警告。

以下代码生成警告 C4698：

```cpp
Windows::Storage::IApplicationDataStatics2::GetForUserAsync(); // C4698
// 'Windows::Storage::IApplicationDataStatics2::GetForUserAsync' is for
// evaluation purposes only and is subject to change or removal in future updates
```

若要禁用此警告，请添加 #pragma：

```cpp
#pragma warning(push)
#pragma warning(disable:4698)

Windows::Storage::IApplicationDataStatics2::GetForUserAsync();

#pragma warning(pop)
```

### <a name="out-of-line-definition-of-a-template-member-function"></a>模板成员函数的外部定义

Visual Studio 2017 版本 15.3 会对未在类中声明的模板成员函数的外部定义生成错误。 以下代码现在生成错误 C2039：

```cpp
struct S {};

template <typename T>
void S::f(T t) {} // C2039: 'f': is not a member of 'S'
```

若要修复此错误，请在类中添加声明：

```cpp
struct S {
    template <typename T>
    void f(T t);
};
template <typename T>
void S::f(T t) {}
```

### <a name="attempting-to-take-the-address-of-this-pointer"></a>尝试使用 `this` 指针的地址

在 C++ 中，`this` 是指向 X 的类型指针的 prvalue。不能使用 `this` 的地址，也不能将其绑定到 lvalue 引用。 在旧版 Visual Studio 中，编译器允许通过使用强制转换来规避此限制。 在 Visual Studio 2017 版本 15.3 中，编译器会生成错误 C2664。

### <a name="conversion-to-an-inaccessible-base-class"></a>转换成不可访问的基类

如果尝试将类型转换成不可访问的基类，Visual Studio 2017 版本 15.3 会生成错误。 下面的代码格式错误，可能会导致运行时故障发生。 现在，编译器在看到如下代码时将生成 C2243：

```cpp
#include <memory>

class B { };
class D : B { }; // C2243: 'type cast': conversion from 'D *' to 'B *' exists, but is inaccessible

void f()
{
   std::unique_ptr<B>(new D());
}
```

### <a name="default-arguments-arent-allowed-on-out-of-line-definitions-of-member-functions"></a>不允许对成员函数的外部定义使用默认自变量

不允许对模板类中成员函数的外部定义使用默认自变量。 编译器在 `/permissive` 模式下发出警告，并在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下发出硬错误。

在以前版本的 Visual Studio 中，以下格式错误的代码可能会导致发生运行时故障。 Visual Studio 2017 版本 15.3 发出警告 C5034：

```cpp
template <typename T>
struct A {
    T f(T t, bool b = false);
};

template <typename T>
T A<T>::f(T t, bool b = false) // C5034: 'A<T>::f': an out-of-line definition of a member of a class template cannot have default arguments
{
    // ...
}
```

若要修复此错误，请删除 `= false` 默认自变量。

### <a name="use-of-offsetof-with-compound-member-designator"></a>将 `offsetof` 与复合成员指示符结合使用

在 Visual Studio 2017 版本 15.3 中，使用 `/Wall` 选项进行编译时，如果使用 `offsetof(T, m)`（其中 m 是“复合成员指示符”），则会导致警告生成。 下面的代码格式错误，可能会导致运行时发生故障。 Visual Studio 2017 版本 15.3 生成警告 C4841：

```cpp
struct A {
   int arr[10];
};

// warning C4841: non-standard extension used: compound member designator in offsetof
constexpr auto off = offsetof(A, arr[2]);
```

若要修复此代码，请使用 pragma 禁用此警告，或将此代码更改为不使用 `offsetof`：

```cpp
#pragma warning(push)
#pragma warning(disable: 4841)
constexpr auto off = offsetof(A, arr[2]);
#pragma warning(pop)
```

### <a name="using-offsetof-with-static-data-member-or-member-function"></a>将 `offsetof` 与静态数据成员或成员函数结合使用

在 Visual Studio 2017 版本 15.3 中，使用 `offsetof(T, m)`（其中 m 表示静态数据成员或成员函数）会导致出现错误。 以下代码生成错误 C4597：

```cpp
#include <cstddef>

struct A {
   int ten() { return 10; }
   static constexpr int two = 2;
};

constexpr auto off = offsetof(A, ten);  // C4597: undefined behavior: offsetof applied to member function 'A::ten'
constexpr auto off2 = offsetof(A, two); // C4597: undefined behavior: offsetof applied to static data member 'A::two'
```

此代码格式错误，可能会导致运行时发生故障。 若要修复此错误，请将此代码更改为不再调用未定义的行为。 这是 C++ 标准不允许使用的不可移植代码。

### <a name="new-warning-on-__declspec-attributes"></a><a name="declspec"></a>有关 `__declspec` 属性的新警告

在 Visual Studio 2017 版本 15.3 中，如果在 `extern "C"` 链接规范前应用了 `__declspec(...)`，则编译器不再忽略此特性。 以前，编译器会忽略此特性，进而可能会产生运行时影响。 如果 `/Wall` 和 `/WX` 选项已设置，以下代码将生成警告 C4768：

```cpp
__declspec(noinline) extern "C" HRESULT __stdcall // C4768: __declspec attributes before linkage specification are ignored
```

若要修复此警告，请将 `extern "C"` 前置：

```cpp
extern "C" __declspec(noinline) HRESULT __stdcall
```

此警告在 15.3 中默认禁用，但在 15.5 中默认启用，并且只影响使用 `/Wall` `/WX` 编译的代码。

### <a name="decltype-and-calls-to-deleted-destructors"></a>`decltype` 和调用的析构函数已删除

在旧版 Visual Studio 中，编译器没有检测到在与 `decltype` 关联的表达式上下文中何时对已删除的析构函数进行了调用。 在 Visual Studio 2017 版本 15.3 中，以下代码生成错误 C2280：

```cpp
template<typename T>
struct A
{
   ~A() = delete;
};

template<typename T>
auto f() -> A<T>;

template<typename T>
auto g(T) -> decltype((f<T>()));

void h()
{
   g(42); // C2280: 'A<T>::~A(void)': attempting to reference a deleted function
}
```

### <a name="uninitialized-const-variables"></a>未初始化的 const 变量

Visual Studio 2017 RTW 版本包含一个回归：C++ 编译器不会对未初始化的 `const` 变量发出诊断。 Visual Studio 2017 版本 15.3 修复了此回归。 以下代码现在生成警告 C4132：

```cpp
const int Value; // C4132: 'Value': const object should be initialized
```

若要修复此错误，请向 `Value` 赋值。

### <a name="empty-declarations"></a>空声明

现在，Visual Studio 2017 版本 15.3 不仅会对内置类型发出警告，还会对所有类型的空声明发出警告。 下面的代码现在对所有四种声明生成第 2 级 C4091 警告：

```cpp
struct A {};
template <typename> struct B {};
enum C { c1, c2, c3 };

int;    // warning C4091 : '' : ignored on left of 'int' when no variable is declared
A;      // warning C4091 : '' : ignored on left of 'main::A' when no variable is declared
B<int>; // warning C4091 : '' : ignored on left of 'B<int>' when no variable is declared
C;      // warning C4091 : '' : ignored on left of 'C' when no variable is declared
```

若要移除这些警告，请注释掉或删除空声明。 如果未命名的对象会造成副作用（如 RAII），应命名对象。

此警告在 `/Wv:18` 模式下被排除在外，而在低于警告等级 W2 时默认是启用的。

### <a name="stdis_convertible-for-array-types"></a>`std::is_convertible` 为数组类型

早期版本的编译器为数组类型提供了不正确的 [`std::is_convertible`](../standard-library/is-convertible-class.md) 结果。 这要求库编写者在使用 `std::is_convertible<...>` 类型特征时，将 Microsoft C++ 编译器作为特例处理。 在以下示例中，静态断言在早期版本的 Visual Studio 中是通过的，但在 Visual Studio 2017 版本 15.3 中不通过：

```cpp
#include <type_traits>

using Array = char[1];

static_assert(std::is_convertible<Array, Array>::value);
static_assert(std::is_convertible<const Array, const Array>::value, "");
static_assert(std::is_convertible<Array&, Array>::value, "");
static_assert(std::is_convertible<Array, Array&>::value, "");
```

`std::is_convertible<From, To>` 是通过检查虚函数定义是否完整计算而得：

```cpp
   To test() { return std::declval<From>(); }
```

### <a name="private-destructors-and-stdis_constructible"></a>私有析构函数和 `std::is_constructible`

在决定 [`std::is_constructible`](../standard-library/is-constructible-class.md) 的结果时，早期版本的编译器忽略了析构函数是否是私有的。 现在会考虑这一点。 在以下示例中，静态断言在早期版本的 Visual Studio 中是通过的，但在 Visual Studio 2017 版本 15.3 中不通过：

```cpp
#include <type_traits>

class PrivateDtor {
   PrivateDtor(int) { }
private:
   ~PrivateDtor() { }
};

// This assertion used to succeed. It now correctly fails.
static_assert(std::is_constructible<PrivateDtor, int>::value);
```

私有析构函数导致类型不可构造。 `std::is_constructible<T, Args...>` 的计算方式类似编写以下声明：

```cpp
   T obj(std::declval<Args>()...)
```

此调用表示一个析构函数调用。

### <a name="c2668-ambiguous-overload-resolution"></a>C2668:重载决策不明确

当发现多个候选项（都使用声明和参数依赖查找）时，以前版本的编译器有时无法检测到多义性。 此故障可能导致选择错误的重载并出现异常的运行时行为。 在以下示例中，Visual Studio 2017 版本 15.3 正确地引发 C2668：

```cpp
namespace N {
   template<class T>
   void f(T&, T&);

   template<class T>
   void f();
}

template<class T>
void f(T&, T&);

struct S {};
void f()
{
   using N::f;

   S s1, s2;
   f(s1, s2); // C2668: 'f': ambiguous call to overloaded function
}
```

要修复代码，请在打算调用 `::f()` 时删除正在使用的 `N::f` 语句。

### <a name="c2660-local-function-declarations-and-argument-dependent-lookup"></a>C2660：局部函数声明与参数依赖查找

局部函数声明将函数声明隐藏在封闭作用域中，并禁用参数依赖查找。 在这种情况下，旧版编译器始终执行的是参数相关查找。 如果编译器选择了错误的重载，则可能会导致意外的运行时行为。 出现此错误，通常是因为局部函数声明的签名不正确。 在以下示例中，Visual Studio 2017 版本 15.3 正确地引发 C2660：

```cpp
struct S {};
void f(S, int);

void g()
{
   void f(S); // C2660 'f': function does not take 2 arguments:
   // or void f(S, int);
   S s;
   f(s, 0);
}
```

要修改此问题，请更改或删除 `f(S)` 签名。

### <a name="c5038-order-of-initialization-in-initializer-lists"></a>C5038：初始值设定项列表中的初始化顺序

类成员按它们声明的顺序（而不是按它们在初始值设定项列表中出现的顺序）进行初始化。 如果初始值设定项列表的顺序不同于声明顺序，早期版本的编译器不会发出警告。 如果一个成员的初始化依赖于列表中已经初始化的另一个成员，那么这个问题可能会导致未定义的运行时行为。 在以下示例中，Visual Studio 2017 版本 15.3（带有 `/Wall`）引发警告 C5038：

```cpp
struct A
{    // Initialized in reverse, y reused
    A(int a) : y(a), x(y) {} // C5038: data member 'A::y' will be initialized after data member 'A::x'
    int x;
    int y;
};
```

要修复此问题，请将初始值设定项列表的顺序设置为与声明顺序相同。 如果一个或两个初始化表达式同时引用基类成员，则会引发类似警告。

此警告默认禁用，并且只影响通过 `/Wall` 编译的代码。

## <a name="conformance-improvements-in-155"></a><a name="improvements_155"></a> 15.5 中的符合性改进

标有 \[14] 的功能是无条件使用的，即使在 `/std:c++14` 模式下，也不例外。

### <a name="new-compiler-switch-for-extern-constexpr"></a>`extern constexpr` 的新编译器开关

在旧版 Visual Studio 中，编译器总是提供 `constexpr` 变量内部链接，即使在变量标记为 `extern` 时，也不例外。 在 Visual Studio 2017 版本 15.5 中，新编译器开关 ([`/Zc:externConstexpr`](../build/reference/zc-externconstexpr.md)) 启用正确且符合标准的行为。 有关详细信息，请参阅 [`extern constexpr` 链接](#extern_linkage)。

### <a name="removing-dynamic-exception-specifications"></a>删除动态异常规范

[P0003R5](https://wg21.link/p0003r5) C++11 已弃用动态异常规范。 C++17 中已删除此功能，但是弃用的 `throw()` 规范（仍然）作为 `noexcept(true)` 的别名严格保留。 有关详细信息，请参阅[动态异常规范的删除和 `noexcept`](#noexcept_removal)。

### `not_fn()`

[P0005R4](https://wg21.link/p0005r4)`not_fn` 替换了 `not1` 和 `not2`。

### <a name="rewording-enable_shared_from_this"></a>改写 `enable_shared_from_this`

[P0033R1](https://wg21.link/p0033r1) C++11 中添加了 `enable_shared_from_this`。 C++17 标准更新规范以更好地处理某些特殊情况。 \[14]

### <a name="splicing-maps-and-sets"></a>拼接映射和集

[P0083R3](https://wg21.link/p0083r3) 此功能从关联容器（即 `map`、`set`、`unordered_map`、`unordered_set`）中提取节点，然后可修改这些节点，并插入回同一容器或使用相同节点类型的不同容器中。 （常见用例是从 `std::map` 提取节点、更改密钥并重新插入。）

### <a name="deprecating-vestigial-library-parts"></a>弃用残留库部分

[P0174R2](https://wg21.link/p0174r2) 这些年来，C++ 标准库的多个功能已被更新的功能取代，或者已发现不可用或存在问题。 C++17 中正式弃用了这些功能。

### <a name="removing-allocator-support-in-stdfunction"></a>删除 `std::function` 中的分配器支持

[P0302R1](https://wg21.link/p0302r1) 在 C++17 之前，经典模板 `std::function` 有多个采用分配器参数的构造函数。 但是，在此上下文中，分配器的使用出现了问题并且语义不明。 已删除问题构造函数。

### <a name="fixes-for-not_fn"></a>`not_fn()` 的修复

[P0358R1](https://wg21.link/p0358r1)`std::not_fn` 的新措词提供对在调用包装器时传播值类别的支持。

### <a name="shared_ptrt-shared_ptrtn"></a>`shared_ptr<T[]>`, `shared_ptr<T[N]>`

[P0414R2](https://wg21.link/p0414r2) 将库基础知识中的 `shared_ptr` 更改合并到 C++17。 \[14]

### <a name="fixing-shared_ptr-for-arrays"></a>修复数组的 `shared_ptr`

[P0497R0](https://wg21.link/p0497r0) 修复数组的 shared_ptr 支持。 \[14]

### <a name="clarifying-insert_return_type"></a>阐明 `insert_return_type`

[P0508R0](https://wg21.link/p0508r0) 有唯一键的关联容器和有唯一键的无序容器均有一个返回嵌套类型 `insert_return_type` 的成员函数 `insert`。 该返回类型现在定义为在容器 Iterator 和 NodeType 上参数化的类型的专用化。

### <a name="inline-variables-for-the-standard-library"></a>标准库的内联变量

对于 [P0607R0](https://wg21.link/p0607r0)，标准库中声明的几个常见变量现在是内联声明的。

### <a name="annex-d-features-deprecated"></a>弃用的 Annex D 功能

C++ 标准的 Annex D 包含所有已弃用的功能，包括 `shared_ptr::unique()`、`<codecvt>` 和 `namespace std::tr1`。 如果 `/std:c++17` 编译器开关已设置，Annex D 中的几乎所有标准库功能都被标记为“弃用”。 有关详细信息，请参阅 [Annex D 中的标准库功能被标记为已弃用](#annex_d)。

现在，`<experimental/filesystem>` 中的 `std::tr2::sys` 命名空间在 `/std:c++14` 下默认抛出弃用警告，在 `/std:c++17` 下默认被删除。

通过避免非标准扩展（类内显式专用化）改进了 `<iostream>` 中的一致性。

标准库现在在内部使用变量模板。

标准库根据 C++17 编译器的更改进行了更新。 更新包括在类型系统中添加了 `noexcept`，以及删除了动态异常规范。

### <a name="partial-ordering-change"></a>部分排序更改

现在，编译器正确拒绝以下代码，并提供正确的错误消息：

```cpp
template<typename... T>
int f(T* ...)
{
    return 1;
}

template<typename T>
int f(const T&)
{
    return 2;
}

int main()
{
    int i = 0;
    f(&i);    // C2668
}
```

```Output
t161.cpp
t161.cpp(16): error C2668: 'f': ambiguous call to overloaded function
t161.cpp(8): note: could be 'int f<int*>(const T &)'
        with
        [
            T=int*
        ]
t161.cpp(2): note: or       'int f<int>(int*)'
t161.cpp(16): note: while trying to match the argument list '(int*)'
```

上面的示例中的问题是类型中有两种差异（const 和 non-const 以及 pack 和 non-pack）。 要消除编译器错误，请删除其中一种差异。 然后，编译器可以明确地对函数排序。

```cpp
template<typename... T>
int f(T* ...)
{
    return 1;
}

template<typename T>
int f(T&)
{
    return 2;
}

int main()
{
    int i = 0;
    f(&i);
}
```

### <a name="exception-handlers"></a>异常处理程序

对数组或函数类型的引用的处理程序从不匹配任意异常对象。 现在，编译器正确遵循此规则并引发第 4 级警告。 使用 `/Zc:strictStrings` 时，它也不再将 `char*` 或 `wchar_t*` 的处理程序与字符串文本相匹配。

```cpp
int main()
{
    try {
        throw "";
    }
    catch (int (&)[1]) {} // C4843 (This should always be dead code.)
    catch (void (&)()) {} // C4843 (This should always be dead code.)
    catch (char*) {} // This should not be a match under /Zc:strictStrings
}
```

```Output
warning C4843: 'int (&)[1]': An exception handler of reference to array or function type is unreachable, use 'int*' instead
warning C4843: 'void (__cdecl &)(void)': An exception handler of reference to array or function type is unreachable, use 'void (__cdecl*)(void)' instead
```

以下代码可避免此错误：

```cpp
catch (int (*)[1]) {}
```

### <a name="stdtr1-namespace-is-deprecated"></a><a name="tr1"></a> `std::tr1` 命名空间已弃用

在 C++14 和 C++17 模式下，非标准 `std::tr1` 命名空间现在标记为已弃用。 在 Visual Studio 2017 版本 15.5 中，以下代码引发错误 C4996：

```cpp
#include <functional>
#include <iostream>
using namespace std;

int main() {
    std::tr1::function<int (int, int)> f = std::plus<int>(); //C4996
    cout << f(3, 5) << std::endl;
    f = std::multiplies<int>();
    cout << f(3, 5) << std::endl;
}
```

```Output
warning C4996: 'std::tr1': warning STL4002: The non-standard std::tr1 namespace and TR1-only machinery are deprecated and will be REMOVED. You can define _SILENCE_TR1_NAMESPACE_DEPRECATION_WARNING to acknowledge that you have received this warning.
```

若要修复此错误，请删除对 `tr1` 命名空间的引用：

```cpp
#include <functional>
#include <iostream>
using namespace std;

int main() {
    std::function<int (int, int)> f = std::plus<int>();
    cout << f(3, 5) << std::endl;
    f = std::multiplies<int>();
    cout << f(3, 5) << std::endl;
}
```

### <a name="standard-library-features-in-annex-d-are-marked-as-deprecated"></a><a name="annex_d"></a> Annex D 中的标准库功能标记为已弃用

如果 `/std:c++17` 模式编译器开关已设置，Annex D 中的几乎所有标准库功能都被标记为“弃用”。

在 Visual Studio 2017 版本 15.5 中，以下代码引发错误 C4996：

```cpp
#include <iterator>

class MyIter : public std::iterator<std::random_access_iterator_tag, int> {
public:
    // ... other members ...
};

#include <type_traits>

static_assert(std::is_same<MyIter::pointer, int*>::value, "BOOM");
```

```Output
warning C4996: 'std::iterator<std::random_access_iterator_tag,int,ptrdiff_t,_Ty*,_Ty &>::pointer': warning STL4015: The std::iterator class template (used as a base class to provide typedefs) is deprecated in C++17. (The <iterator> header is NOT deprecated.) The C++ standard has never required user-defined iterators to derive from std::iterator. To fix this warning, stop deriving from std::iterator and start providing publicly accessible typedefs named iterator_category, value_type, difference_type, pointer, and reference. Note that value_type is required to be non-const, even for constant iterators. You can define _SILENCE_CXX17_ITERATOR_BASE_CLASS_DEPRECATION_WARNING or _SILENCE_ALL_CXX17_DEPRECATION_WARNINGS to acknowledge that you have received this warning.
```

若要修复此错误，请按警告文本中的说明操作，如以下代码所示：

```cpp
#include <iterator>

class MyIter {
public:
    typedef std::random_access_iterator_tag iterator_category;
    typedef int value_type;
    typedef ptrdiff_t difference_type;
    typedef int* pointer;
    typedef int& reference;

    // ... other members ...
};

#include <type_traits>

static_assert(std::is_same<MyIter::pointer, int*>::value, "BOOM");
```

### <a name="unreferenced-local-variables"></a>未引用的本地变量

在 Visual Studio 15.5 中，会在较多的情况下发出警告 C4189，如以下代码所示：

```cpp
void f() {
    char s[2] = {0}; // C4189. Either use the variable or remove it.
}
```

```Output
warning C4189: 's': local variable is initialized but not referenced
```

若要修复此错误，请删除未使用的变量。

### <a name="single-line-comments"></a>单行注释

在 Visual Studio 2017 版本 15.5 中，警告 C4001 和 C4179 不再由 C 编译器发出。 以前，它们只在 `/Za` 编译器开关下抛出。  不再需要这些警告，因为单行注释自 C99 以来已成为 C 标准的一部分。

```cpp
/* C only */
#pragma warning(disable:4001) // C4619
#pragma warning(disable:4179)
// single line comment
//* single line comment */
```

```Output
warning C4619: #pragma warning: there is no warning number '4001'
```

如果代码不需要向后兼容，可以通过删除 C4001 和 C4179 禁止来避免此警告。 如果代码不需要向后兼容，则仅禁止显示 C4619。

```C
/* C only */

#pragma warning(disable:4619)
#pragma warning(disable:4001)
#pragma warning(disable:4179)

// single line comment
/* single line comment */
```

### <a name="__declspec-attributes-with-extern-c-linkage"></a>带 `extern "C"` 链接的 `__declspec` 属性

在 Visual Studio 的早期版本中，在 `extern "C"` 链接规范之前应用 `__declspec(...)` 时，编译器会忽略 `__declspec(...)` 属性。 此行为导致生成用户意料之外的代码并且可能产生运行时影响。 Visual Studio 版本 15.3 中添加了此警告，但是默认关闭。 Visual Studio 2017 版本 15.5 默认启用此警告。

```cpp
__declspec(noinline) extern "C" HRESULT __stdcall // C4768
```

```Output
warning C4768: __declspec attributes before linkage specification are ignored
```

要修复此错误，请将链接规范放在 __declspec 属性之前：

```cpp
extern "C" __declspec(noinline) HRESULT __stdcall
```

在 Visual Studio 2017 15.3 或更低版本（例如版本 10.0.15063.0，也称为 RS2 SDK）附带的某些 Windows SDK 标头上会有 C4768 这一个新警告。 但是，较高版本的 Windows SDK 标头（具体指 ShlObj.h 和 ShlObj_core.h）已修复，因此它们不会生成警告。 看见来自 Windows SDK 标头的这一警告时，可以采取以下措施：

1. 切换到 Visual Studio 2017 版本 15.5 附带的最新 Windows SDK。

1. 关闭 Windows SDK 标头声明 #include 附近的警告：

```cpp
   #pragma warning (push)
   #pragma warning(disable:4768)
   #include <shlobj.h>
   #pragma warning (pop)
   ```

### <a name="extern-constexpr-linkage"></a><a name="extern_linkage"></a> `extern constexpr` 链接

在旧版 Visual Studio 中，编译器总是提供 `constexpr` 变量内部链接，即使在变量标记为 `extern` 时，也不例外。 在 Visual Studio 2017 版本 15.5 中，新编译器开关 (`/Zc:externConstexpr`) 启用正确且符合标准的行为。 此行为最终将成为默认设置。

```cpp
extern constexpr int x = 10;
```

```Output
error LNK2005: "int const x" already defined
```

如果头文件包含声明 `extern constexpr` 的变量，需将它标记为 `__declspec(selectany)`，以便正确组合其重复声明：

```cpp
extern constexpr __declspec(selectany) int x = 10;
```

### <a name="typeid-cant-be-used-on-incomplete-class-type"></a>不能在不完整的类类型上使用 `typeid`

在早期版本的 Visual Studio 中，编译器不正确地允许以下代码，导致可能不正确的类型信息。 在 Visual Studio 2017 版本 15.5 中，编译器正确引发错误：

```cpp
#include <typeinfo>

struct S;

void f() { typeid(S); } //C2027 in 15.5
```

```Output
error C2027: use of undefined type 'S'
```

### <a name="stdis_convertible-target-type"></a>`std::is_convertible` 目标类型

`std::is_convertible` 要求目标类型为有效返回类型。 在早期版本的 Visual Studio 中，编译器不正确地允许抽象类型，这可能导致不正确的重载决策和意外的运行时行为。  现在，以下代码正确引发错误 C2338：

```cpp
#include <type_traits>

struct B { virtual ~B() = 0; };
struct D : public B { virtual ~D(); };

static_assert(std::is_convertible<D, B>::value, "fail"); // C2338 in 15.5
```

若要避免此错误，使用 `is_convertible` 时应该比较指针类型，因为如果有一个类型是抽象的，那么非指针类型的比较可能会失败：

```cpp
#include <type_traits>

struct B { virtual ~B() = 0; };
struct D : public B { virtual ~D(); };

static_assert(std::is_convertible<D *, B *>::value, "fail");
```

### <a name="dynamic-exception-specification-removal-and-noexcept"></a><a name="noexcept_removal"></a> 动态异常规范的删除和 `noexcept`

在 C++17 中，`throw()` 是 `noexcept` 的别名，删除了 `throw(<type list>)` 和 `throw(...)`，并且某些类型可能包含 `noexcept`。 此更改可能导致符合 C++14 或更低版本的代码的源兼容性问题。 通常，在使用 C++17 模式时，可使用 `/Zc:noexceptTypes-` 开关恢复到 `noexcept` 的 C++14 版本。 这样可以将源代码更新为符合 C++17，而无需在同时重写所有 `throw()` 代码。

现在，编译器还诊断 C++17 模式声明中或者带有新警告 C5043 的 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 所具有的更多不匹配的异常规范。

当 `/std:c++17` 开关应用时，以下代码在 Visual Studio 2017 版本 15.5 中生成 C5043 和 C5040：

```cpp
void f() throw(); // equivalent to void f() noexcept;
void f() {} // warning C5043
void g() throw(); // warning C5040

struct A {
    virtual void f() throw();
};

struct B : A {
    virtual void f() { } // error C2694
};
```

若要在仍使用 `/std:c++17` 的情况下删除错误，请将 `/Zc:noexceptTypes-` 开关添加到命令行，或将代码更新为使用 `noexcept`，如下面的示例所示：

```cpp
void f() noexcept;
void f() noexcept { }
void g() noexcept(false);

struct A {
    virtual void f() noexcept;
};

struct B : A {
    virtual void f() noexcept { }
};
```

### <a name="inline-variables"></a>内联变量

现在，静态 `constexpr` 数据成员是隐式 `inline`，这意味着它们在类中的声明现在是它们的定义。 使用 `static constexpr` 数据成员的外部定义是冗余的，现已弃用。 在 Visual Studio 2017 版本 15.5 中，当应用 `/std:c++17` 开关时，以下代码现在生成警告 C5041：

```cpp
struct X {
    static constexpr int size = 3;
};
const int X::size; // C5041: 'size': out-of-line definition for constexpr static data member is not needed and is deprecated in C++17
```

### <a name="extern-c-__declspec-warning-c4768-now-on-by-default"></a>现在默认打开 `extern "C" __declspec(...)` 警告 C4768

Visual Studio 2017 版本 15.3 中添加了此警告，但是默认关闭。 Visual Studio 2017 版本 15.5 默认启用此警告。 有关详细信息，请参阅[有关 `__declspec` 属性的新警告](#declspec)。

### <a name="defaulted-functions-and-__declspecnothrow"></a>默认函数和 `__declspec(nothrow)`

以前，当相应基/成员函数允许异常时，编译器允许使用 `__declspec(nothrow)` 声明默认函数。 此行为与 C++ 标准冲突，可能导致在运行时发生未定义的行为。 如果有异常规范不匹配，标准要求此类函数定义为已删除。  在 `/std:c++17` 下，以下代码引发 C2280：

```cpp
struct A {
    A& operator=(const A& other) { // No exception specification; this function may throw.
        ...
    }
};

struct B : public A {
    __declspec(nothrow) B& operator=(const B& other) = default;
};

int main()
{
    B b1, b2;
    b2 = b1; // error C2280: attempting to reference a deleted function.
             // Function was implicitly deleted because the explicit exception
             // specification is incompatible with that of the implicit declaration.
}
```

要更正此代码，请从默认函数删除 __declspec(nothrow)，或者删除 `= default` 并提供函数定义以及任何所需异常处理：

```cpp
struct A {
    A& operator=(const A& other) {
        // ...
    }
};

struct B : public A {
    B& operator=(const B& other) = default;
};

int main()
{
    B b1, b2;
    b2 = b1;
}
```

### <a name="noexcept-and-partial-specializations"></a>`noexcept` 和部分专用化

有了类型系统中的 `noexcept`，用于匹配特定“可调用”类型的部分专门化可能无法编译，或无法选择主模板，因为缺少指针到 noexcept 函数的部分专门化。

在这种情况下，可能需要添加更多部分专门化，以处理 `noexcept` 函数指针和指向成员函数的 `noexcept` 指针。 这些重载只在 `/std:c++17` 模式下是合法的。 如果必须维持与 C++14 的向后兼容性，并且正在编写他人将使用的代码，那么应该保证这些新重载位于 `#ifdef` 指令内。 如果使用的是独立式模块，那么可以使用 `/Zc:noexceptTypes-` 开关进行编译，而不是使用 `#ifdef` 临界子句。

以下代码在 `/std:c++14` 下进行编译，但在 `/std:c++17` 下则会失败并引发错误 C2027：

```cpp
template <typename T> struct A;

template <>
struct A<void(*)()>
{
    static const bool value = true;
};

template <typename T>
bool g(T t)
{
    return A<T>::value;
}

void f() noexcept {}

int main()
{
    return g(&f) ? 0 : 1; // C2027: use of undefined type 'A<T>'
}
```

以下代码在 `/std:c++17` 下成功，因为编译器选择新的部分专用化 `A<void (*)() noexcept>`：

```cpp
template <typename T> struct A;

template <>
struct A<void(*)()>
{
    static const bool value = true;
};

template <>
struct A<void(*)() noexcept>
{
    static const bool value = true;
};

template <typename T>
bool g(T t)
{
    return A<T>::value;
}

void f() noexcept {}

int main()
{
    return g(&f) ? 0 : 1; // OK
}
```

## <a name="conformance-improvements-in-156"></a><a name="improvements_156"></a> 15.6 中的符合性改进

### <a name="c17-library-fundamentals-v1"></a>C++17 Library Fundamentals V1

[P0220R1](https://wg21.link/p0220r1) 将用于 C++17 的 Library Fundamentals 技术规范纳入了标准。 涵盖 \<experimental/tuple>、\<experimental/optional>、\<experimental/functional>、\<experimental/any>、\<experimental/string_view>、\<experimental/memory>、\<experimental/memory_resource> 和 \<experimental/algorithm> 的更新。

### <a name="c17-improving-class-template-argument-deduction-for-the-standard-library"></a>C++17：改进针对标准库的类模板参数推导

[P0739R0](https://wg21.link/p0739r0) 将 `adopt_lock_t` 移动到 `scoped_lock` 参数列表的前面，以启用对 `scoped_lock` 的一致使用。 允许 `std::variant` 构造函数在更多事例中参与重载解析，以便启用副本分配。

## <a name="conformance-improvements-in-157"></a><a name="improvements_157"></a> 15.7 中的符合性改进

### <a name="c17-rewording-inheriting-constructors"></a>C++17：重新组织继承构造函数

[P0136R1](https://wg21.link/p0136r1) 规定，命名构造函数的 `using` 声明现在使相应的基类构造函数对派生类的初始化可见，而不用声明更多派生类构造函数。 此重新组织是从 C++14 更改的。 在 `/std:c++17` 模式下的 Visual Studio 2017 版本 15.7 及更高版本中，C++14 中有效且使用继承构造函数的代码可能无效或具有不同的语义。

以下示例演示 C++14 行为：

```cpp
struct A {
    template<typename T>
    A(T, typename T::type = 0);
    A(int);
};

struct B : A {
    using A::A;
    B(int n) = delete; // Error C2280
};

B b(42L); // Calls B<long>(long), which calls A(int)
          //  due to substitution failure in A<long>(long).
```

以下示例展示了在 Visual Studio 15.7 中的 `/std:c++17` 行为：

```cpp
struct A {
    template<typename T>
    A(T, typename T::type = 0);
    A(int);
};

struct B : A {
    using A::A;
    B(int n)
    {
        //do something
    }
};

B b(42L); // now calls B(int)
```

有关详细信息，请参阅[构造函数](../cpp/constructors-cpp.md#inheriting_constructors)。

### <a name="c17-extended-aggregate-initialization"></a>C++17：扩展的聚合初始化

[P0017R1](https://wg21.link/p0017r1)

如果基类的构造函数是非公共的，但可由派生类进行访问，那么在 `/std:c++17` 模式下的 Visual Studio 2017 版本 15.7 中，就不能再使用空括号来初始化派生类型的对象。
以下示例演示 C++14 一致行为：

```cpp
struct Derived;
struct Base {
    friend struct Derived;
private:
    Base() {}
};

struct Derived : Base {};
Derived d1; // OK. No aggregate init involved.
Derived d2 {}; // OK in C++14: Calls Derived::Derived()
               // which can call Base ctor.
```

在 C++17，`Derived` 现被视作聚合类型。 这意味着 `Base` 通过私有默认构造函数进行的初始化将作为扩展的聚合初始化规则的一部分而直接发生。 以前，`Base` 私有构造函数通过 `Derived` 构造函数调用，它之所以能够成功是因为友元声明。
以下示例展示了在 `/std:c++17` 模式下的 Visual Studio 版本 15.7 中的 C++17 行为：

```cpp
struct Derived;
struct Base {
    friend struct Derived;
private:
    Base() {}
};
struct Derived : Base {
    Derived() {} // add user-defined constructor
                 // to call with {} initialization
};
Derived d1; // OK. No aggregate init involved.
Derived d2 {}; // error C2248: 'Base::Base': cannot access
               // private member declared in class 'Base'
```

### <a name="c17-declaring-non-type-template-parameters-with-auto"></a>C++17：使用 auto 声明非类型模板参数

[P0127R2](https://wg21.link/p0127r2)

在 `/std:c++17` 模式下，编译器现在可以推导使用 `auto` 声明的非类型模板参数的类型：

```cpp
template <auto x> constexpr auto constant = x;

auto v1 = constant<5>;      // v1 == 5, decltype(v1) is int
auto v2 = constant<true>;   // v2 == true, decltype(v2) is bool
auto v3 = constant<'a'>;    // v3 == 'a', decltype(v3) is char
```

此新功能的一个影响是有效的 C++14 代码可能无效或具有不同的语义。 例如，某些以前无效的重载现在变得有效。 以下示例演示由于对 `example(p)` 的调用绑定到 `example(void*);` 而编译的 C++14 代码。 在 `/std:c++17` 模式下的 Visual Studio 2017 版本 15.7 中，`example` 函数模板是最佳匹配。

```cpp
template <int N> struct A;
template <typename T, T N> int example(A<N>*) = delete;

void example(void *);

void sample(A<0> *p)
{
    example(p); // OK in C++14
}
```

以下示例展示了在 `/std:c++17` 模式下的 Visual Studio 15.7 中的 C++17 代码：

```cpp
template <int N> struct A;
template <typename T, T N> int example(A<N>*);

void example(void *);

void sample(A<0> *p)
{
    example(p); // C2280: 'int example<int,0>(A<0>*)': attempting to reference a deleted function
}
```

### <a name="c17-elementary-string-conversions-partial"></a>C++17：基本字符串转换（部分）

[P0067R5](https://wg21.link/p0067r5) 用于在整数和字符串之间、浮点数和字符串之间进行转换的低级别、独立于区域设置的函数。

### <a name="c20-avoiding-unnecessary-decay-partial"></a>C++20：避免不必要的 decay（部分）

[P0777R1](https://wg21.link/p0777r1) 添加“decay”概念与“简单删除常量或引用限定符”概念之间的区别。  在某些上下文中，新的类型特征 `remove_reference_t` 将替换 `decay_t`。 对 `remove_cvref_t` 的支持在 Visual Studio 2019 中实现。

### <a name="c17-parallel-algorithms"></a>C++17：并行算法

[P0024R2](https://wg21.link/p0024r2) 并行 TS 纳入到标准中，以及少许修改。

### <a name="c17-hypotx-y-z"></a>C++17：`hypot(x, y, z)`

[P0030R1](https://wg21.link/p0030r1) 向 `std::hypot` 添加三个新的重载，对于类型 `float`、`double` 和 `long double`，每个都有三个输入参数。

### <a name="c17-filesystem"></a>C++17：\<filesystem>

[P0218R1](https://wg21.link/p0218r1) 将文件系统 TS 纳入到标准，外加一些字词修改。

### <a name="c17-mathematical-special-functions"></a>C++17：数学特殊函数

[P0226R1](https://wg21.link/p0220r1) 将以前的用于特殊数学函数的技术规范纳入到标准 \<cmath> 标头中。

### <a name="c17-deduction-guides-for-the-standard-library"></a>C++17：标准库的推导指南

[P0433R2](https://wg21.link/p0433r2) 对 STL 进行更新以利用 [P0091R3](https://wg21.link/p0091r3) 的 C++17 采纳，其添加了对类模板参数推导的支持。

### <a name="c17-repairing-elementary-string-conversions"></a>C++17：修复基本字符串转换

[P0682R1](https://wg21.link/p0682r1) 将新的基本字符串转换函数从 P0067R5 移动到新的 \<charconv> 标头，并进行了其他改进，包括更改错误处理以使用 `std::errc` 而非 `std::error_code`。

### <a name="c17-constexpr-for-char_traits-partial"></a>C++17：`char_traits` 为 `constexpr`（部分）

[P0426R1](https://wg21.link/p0426r1) 对 `std::traits_type` 成员函数 `length`、`compare` 和 `find` 进行了更改，以便使 `std::string_view` 在常数表达式中可用。 （在 Visual Studio 2017 版本 15.6 中，仅支持 Clang/LLVM。 在版本 15.7 预览版 2 中，也几乎完全支持 ClXX。）

### <a name="c17-default-argument-in-the-primary-class-template"></a>C++17：主类模板中的默认参数

此行为变更是 [P0091R3 - 类模板的模板参数推导](https://wg21.link/p0091r3)的前提条件。

此前，编译器会忽略主类模板中的默认参数：

```cpp
template<typename T>
struct S {
    void f(int = 0);
};

template<typename T>
void S<T>::f(int = 0) {} // Re-definition necessary
```

在 `/std:c++17` 模式下的 Visual Studio 2017 版本 15.7 中，默认参数不会被忽略：

```cpp
template<typename T>
struct S {
    void f(int = 0);
};

template<typename T>
void S<T>::f(int) {} // Default argument is used
```

### <a name="dependent-name-resolution"></a>依赖名称解析

此行为变更是 [P0091R3 - 类模板的模板参数推导](https://wg21.link/p0091r3)的前提条件。

在下面的示例中，Visual Studio 15.6 及更低版本中的编译器在主类模板中将 `D::type` 解析为 `B<T>::type`。

```cpp
template<typename T>
struct B {
    using type = T;
};

template<typename T>
struct D : B<T*> {
    using type = B<T*>::type;
};
```

Visual Studio 2017 版本 15.7 在 `/std:c++17` 模式下要求在 D 内的 `using` 语句中使用 `typename` 关键字。如果没有 `typename`，编译器会抛出警告 C4346 `'B<T*>::type': dependent name is not a type` 和错误 C2061 `syntax error: identifier 'type'`：

```cpp
template<typename T>
struct B {
    using type = T;
};

template<typename T>
struct D : B<T*> {
    using type = typename B<T*>::type;
};
```

### <a name="c17-nodiscard-attribute---warning-level-increase"></a>C++17：`[[nodiscard]]` 属性 - 警告级别增加

在 `/std:c++17` 模式下的 Visual Studio 2017 版本 15.7 中，C4834 的警告等级从 W3 提高到了 W1。 可通过强制转换为 `void` 或将 `/wd:4834` 传递给编译器来禁用警告

```cpp
[[nodiscard]] int f() { return 0; }

int main() {
    f(); // warning C4834: discarding return value
         // of function with 'nodiscard'
}
```

### <a name="variadic-template-constructor-base-class-initialization-list"></a>变参模板构造函数基类初始化列表

旧版 Visual Studio 错误地允许使用缺少模板参数的变参模板构造函数基类初始化列表，而未生成任何错误消息。 Visual Studio 2017 版本 15.7 抛出了编译器错误。

下面的代码示例在 Visual Studio 2017 版本 15.7 中引发错误 C2614：

```cpp
template<typename T>
struct B {};

template<typename T>
struct D : B<T>
{

    template<typename ...C>
    D() : B() {} // C2614: D<int>: illegal member initialization: 'B' is not a base or member
};

D<int> d;
```

若要修复此错误，请将 B() 表达式更改为 B\<T>()。

### <a name="constexpr-aggregate-initialization"></a>`constexpr` 聚合初始化

旧版 C++ 编译器错误地处理了 `constexpr` 聚合初始化。 编译器接受了无效的代码（其中 aggregate-init-list 有太多元素），并为其生成了错误的对象代码。 以下代码是一个此类代码的示例：

```cpp
#include <array>
struct X {
    unsigned short a;
    unsigned char b;
};

int main() {
    constexpr std::array<X, 2> xs = { // C2078: too many initializers
        { 1, 2 },
        { 3, 4 }
    };
    return 0;
}
```

在 Visual Studio 2017 版本 15.7 Update 3 及更高版本中，上面的示例现在引发 C2078。 以下示例演示了如何修复此代码。 在使用嵌套 brace-init-lists 初始化 `std::array` 时，给予内部数组一个自己的 braced-list：

```cpp
#include <array>
struct X {
    unsigned short a;
    unsigned char b;
};

int main() {
    constexpr std::array<X, 2> xs = {{ // note double braces
        { 1, 2 },
        { 3, 4 }
    }}; // note double braces
    return 0;
}
```

## <a name="conformance-improvements-in-158"></a><a name="update_158"></a> 15.8 中的符合性改进

### <a name="typename-on-unqualified-identifiers"></a>非限定标识符的 `typename`

在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，编译器不再接受别名模板定义中非限定标识符上的虚假 `typename` 关键字。 以下代码现在生成 C7511：

```cpp
template <typename T>
using  X = typename T; // C7511: 'T': 'typename' keyword must be 
                       // followed by a qualified name
```

要修复此错误，将第二行更改为 `using  X = T;`。

### <a name="__declspec-on-right-side-of-alias-template-definitions"></a>别名模板定义右侧的 `__declspec()`

别名模板定义的右侧不再允许 [`__declspec`](../cpp/declspec.md)。 以前，编译器接受但忽略了此代码。 当使用别名时，它永远不会导致弃用警告生成。

可改用标准 C++ 属性 [`[[deprecated]]`](../cpp/attributes.md)，并从 Visual Studio 2017 版本 15.6 开始遵循。 以下代码现在生成 C2760：

```cpp
template <typename T>
using X = __declspec(deprecated("msg")) T; // C2760: syntax error:
                                           // unexpected token '__declspec',
                                           // expected 'type specifier'`
```

要修复此错误，将代码改为以下代码（将属性放置在别名定义的“=“前）：

```cpp
template <typename T>
using  X [[deprecated("msg")]] = T;
```

### <a name="two-phase-name-lookup-diagnostics"></a>两阶段名称查找诊断

两阶段名称查找要求模板正文中使用的非相关名称必须在定义时对模板可见。 过去，Microsoft C++ 编译器会使未找到的名称处于非查找状态，直到实例化时。 现在，它要求非相关名称绑定在模板正文中。

这其中一个表现是查找相关基类。 以前，编译器允许使用在依赖基类中定义的名称。 这是因为，当所有类型都得到解析时，会在实例化期间查找它们。 现在将该代码视为错误。 在这些情况下，可在实例化时强制查找变量，方法是使用基类类型对其进行限定或将其设置为相关，例如添加 `this->` 指针。

在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，以下代码现在会引发 C3861：

```cpp
template <class T>
struct Base {
    int base_value = 42;
};

template <class T>
struct S : Base<T> {
    int f() {
        return base_value; // C3861: 'base_value': identifier not found
    }
};
```

若要修复此错误，请将 `return` 语句更改为 `return this->base_value;`。

> [!NOTE]
> 在 1.70 版本之前的 Boost.Python 库中，对于 [`unwind_type.hpp`](https://github.com/boostorg/python/blame/develop/include/boost/python/detail/unwind_type.hpp) 中的模板前向声明，有一个特定于 MSVC 的解决方法。 从 Visual Studio 2017 版本 15.8 (`_MSC_VER==1915`) 起，在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，MSVC 编译器正确地执行参数相关名称查找 (ADL)。 现在，它与其他编译器一致，因此不需要此变通临界子句。 为避免错误 C3861：`'unwind_type': identifier not found`，请更新 Boost.Python 库。

### <a name="forward-declarations-and-definitions-in-namespace-std"></a>命名空间 `std` 中的转发声明和定义

C++ 标准不允许用户将转发声明或定义添加到命名空间 `std`。 如果将声明或定义添加到命名空间 `std` 或命名空间 `std` 中的命名空间，会导致未定义的行为。

未来的某个时间，Microsoft 将移动定义一些标准库类型的位置。 此更改会破坏将转发声明添加到命名空间 `std` 的现有代码。 新警告 C4643 有助于识别此类源问题。 此警告在 `/default` 模式下启用，默认是禁用的。 它会影响使用 `/Wall` 或 `/WX` 编译的程序。

现在，以下代码引发 C4643：

```cpp
namespace std {
    template<typename T> class vector;  // C4643: Forward declaring 'vector'
                                        // in namespace std is not permitted
                                        // by the C++ Standard`
}
```

要修复此错误，请使用 `#include` 指令，而不是转发声明：

```cpp
#include <vector>
```

### <a name="constructors-that-delegate-to-themselves"></a>对自身进行委托的构造函数

C++ Standard 建议当委托构造函数对自身进行委托时，编译器应发出诊断。 在 [`/std:c++17`](../build/reference/std-specify-language-standard-version.md) 和 [`/std:c++latest`](../build/reference/std-specify-language-standard-version.md) 模式下的 Microsoft C++ 编译器现在引发 C7535。

如果不出现此错误，以下程序将编译，但会生成无限循环：

```cpp
class X {
public:
    X(int, int);
    X(int v) : X(v){} // C7535: 'X::X': delegating constructor calls itself
};
```

若要避免无限循环，可委托给其他构造函数：

```cpp
class X {
public:

    X(int, int);
    X(int v) : X(v, 0) {}
};
```

### <a name="offsetof-with-constant-expressions"></a>带常量表达式的 `offsetof`

传统上，使用需要 [`reinterpret_cast`](../cpp/reinterpret-cast-operator.md) 的宏实现 [`offsetof`](../c-runtime-library/reference/offsetof-macro.md)。 在需要常量表达式的上下文中，此用法是非法的，但从传统上，Microsoft C++ 编译器允许此操作。 标准库中附带的 `offsetof` 宏正确使用内部编译器 ( **`__builtin_offsetof`** )，但许多人使用此宏来定义自己的 `offsetof`。

在 Visual Studio 2017 版本 15.8 中，编译器限制这些 `reinterpret_cast` 运算符可以在默认模式下出现的位置，以帮助代码符合标准 C++ 行为。 在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 下，此类限制更严格。 在需要常数表达式的位置使用 `offsetof` 的结果可能会导致代码发出警告 C4644 或 C2975。

以下代码在 `/default` 和 `/std:c++17` 模式下发出 C4644，并在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下发出 C2975：

```cpp
struct Data {
    int x;
};

// Common pattern of user-defined offsetof
#define MY_OFFSET(T, m) (unsigned long long)(&(((T*)nullptr)->m))

int main()

{
    switch (0) {
    case MY_OFFSET(Data, x): return 0; // C4644: usage of the
        // macro-based offsetof pattern in constant expressions
        // is non-standard; use offsetof defined in the C++
        // standard library instead
        // OR
        // C2975: invalid template argument, expected
        // compile-time constant expression

    default: return 1;
    }
}
```

要修复此错误，使用通过 \<cstddef> 定义的 `offsetof`：

```cpp
#include <cstddef>

struct Data {
    int x;
};

int main()
{
    switch (0) {
    case offsetof(Data, x): return 0;
    default: return 1;
    }
}
```

### <a name="cv-qualifiers-on-base-classes-subject-to-pack-expansion"></a>基类上的 cv-qualifiers 受包扩展约束

Microsoft C++ 编译器的以前版本不会检测基类是否有 cv 限定符，如果它也受包扩展约束。

在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下的 Visual Studio 2017 版本 15.8 中，以下代码引发 C3770：

```cpp
template<typename... T>
class X : public T... { };

class S { };

int main()
{
    X<const S> x; // C3770: 'const S': is not a valid base class
}
```

### <a name="template-keyword-and-nested-name-specifiers"></a>`template` 关键字和嵌套名称说明符

在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，编译器现在要求在依赖的嵌套名称说明符之后的模板名称前添加 `template` 关键字。

在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，以下代码现在会引发 C7510：

```cpp
template<typename T> struct Base
{
    template<class U> void example() {}
};

template<typename T>
struct X : Base<T>
{
    void example()
    {
        Base<T>::example<int>(); // C7510: 'example': use of dependent
            // template name must be prefixed with 'template'
            // note: see reference to class template instantiation
            // 'X<T>' being compiled
    }
};
```

若要修复此错误，请将 `template` 关键字添加到 `Base<T>::example<int>();` 语句中，如下面的示例所示：

```cpp
template<typename T> struct Base
{
    template<class U> void example() {}
};

template<typename T>
struct X : Base<T>
{
    void example()
    {
        // Add template keyword here:
        Base<T>::template example<int>();
    }
};
```

## <a name="conformance-improvements-in-159"></a><a name="improvements_159"></a> 15.9 中的符合性改进

### <a name="left-to-right-evaluation-order-for-operators-----and-"></a>运算符 `->*`、`[]`、`>>` 和 `<<` 从左到右的计算顺序

从 C++17 开始，运算符 `->*`、`[]`、`>>` 和 `<<` 的操作数必须按从左到右的顺序计算。 在以下两种情况下，编译器无法保证此顺序：

- 其中一个操作数表达式是由值传递的对象或包含由值传递的对象时，或

- 使用 `/clr` 进行编译，且其中一个操作数是对象字段或数组元素时。

当编译器无法保证从左到右计算时，它将发出警告 [C4866](../error-messages/compiler-warnings/c4866.md)。 只有在 `/std:c++17` 或更高版本已指定时，编译器才会生成此警告，因为这些运算符的从左到右顺序要求是在 C++17 中引入的。

若要解决此警告，请先考虑是否需要从左到右计算操作数。 例如，当操作数的计算可能产生与顺序相关的副作用时，可能就需要从左到右计算。 在许多情况下，操作数的计算顺序没有明显的影响。 如果计算顺序必须是从左到右，则考虑是否可以改为通过常量引用来传递操作数。 此更改可消除以下代码示例中的警告：

```cpp
// C4866.cpp
// compile with: /w14866 /std:c++17

class HasCopyConstructor
{
public:
    int x;

    HasCopyConstructor(int x) : x(x) {}
    HasCopyConstructor(const HasCopyConstructor& h) : x(h.x) { }
};

int operator>>(HasCopyConstructor a, HasCopyConstructor b) { return a.x >> b.x; }

// This version of operator>> does not trigger the warning:
// int operator>>(const HasCopyConstructor& a, const HasCopyConstructor& b) { return a.x >> b.x; }

int main()
{
    HasCopyConstructor a{ 1 };
    HasCopyConstructor b{ 2 };

    a>>b;        // C4866 for call to operator>>
};
```

### <a name="identifiers-in-member-alias-templates"></a>成员别名模板中的标识符

在成员别名模板定义中使用的标识符必须在使用前进行声明。

在以前版本的编译器中，允许以下代码。 在 Visual Studio 2017 版本 15.9 中，在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下，编译器引发 C3861：

```cpp
template <typename... Ts>
struct A
{
  public:
    template <typename U>
    using from_template_t = decltype(from_template(A<U>{})); // C3861:
        // 'from_template': identifier not found

  private:
    template <template <typename...> typename Type, typename... Args>
    static constexpr A<Args...> from_template(A<Type<Args...>>);
};

A<>::from_template_t<A<int>> a;
```

若要修复此错误，请在 `from_template_t` 之前声明 `from_template`。

### <a name="modules-changes"></a>模块更改

在 Visual Studio 2017 版本 15.9 中，每当模块的命令行选项在模块创建端和模块消耗端之间不一致时，编译器会引发 C5050。 以下示例中存在两个问题：

- 在消耗端 (main.cpp) 上，没有指定 `/EHsc` 选项。

- 创建端和消耗端上的 C++ 版本分别为 `/std:c++17` 和 `/std:c++14`。

```cmd
cl /EHsc /std:c++17 m.ixx /experimental:module
cl /experimental:module /module:reference m.ifc main.cpp /std:c++14
```

编译器对这两种情况都引发 C5050：

```Output
warning C5050: Possible incompatible environment while
importing module 'm': mismatched C++ versions.
Current "201402" module version "201703"`.
```

每当篡改 `.ifc` 文件时，编译器还会引发 C7536。 模块接口的标头包含它下面内容的 SHA2 哈希。 在导入时，先对 `.ifc` 文件进行哈希处理，然后根据头中提供的哈希对它进行检查。 如果这些内容不匹配，则会引发错误 C7536：

```Output
error C7536: ifc failed integrity checks.
Expected SHA2: '66d5c8154df0c71d4cab7665bab4a125c7ce5cb9a401a4d8b461b706ddd771c6'
```

### <a name="partial-ordering-involving-aliases-and-non-deduced-contexts"></a>部分排序涉及别名和非推导上下文

涉及非推导上下文中别名的部分排序规则出现实现分歧。 在以下示例中，在 Clang 接受代码时，GCC 和 Microsoft C++ 编译器（在 [`/permissive-`](../build/reference/permissive-standards-conformance.md) 模式下）抛出错误。

```cpp
#include <utility>
using size_t = std::size_t;

template <typename T>
struct A {};
template <size_t, size_t>
struct AlignedBuffer {};
template <size_t len>
using AlignedStorage = AlignedBuffer<len, 4>;

template <class T, class Alloc>
int f(Alloc &alloc, const AlignedStorage<T::size> &buffer)
{
    return 1;
}

template <class T, class Alloc>
int f(A<Alloc> &alloc, const AlignedStorage<T::size> &buffer)
{
    return 2;
}

struct Alloc
{
    static constexpr size_t size = 10;
};

int main()
{
    A<void> a;
    AlignedStorage<Alloc::size> buf;
    if (f<Alloc>(a, buf) != 2)
    {
        return 1;
    }

    return 0;
}
```

上例中引发了 C2668：

```Output
partial_alias.cpp(32): error C2668: 'f': ambiguous call to overloaded function
partial_alias.cpp(18): note: could be 'int f<Alloc,void>(A<void> &,const AlignedBuffer<10,4> &)'
partial_alias.cpp(12): note: or       'int f<Alloc,A<void>>(Alloc &,const AlignedBuffer<10,4> &)'
        with
        [
            Alloc=A<void>
        ]
partial_alias.cpp(32): note: while trying to match the argument list '(A<void>, AlignedBuffer<10,4>)'
```

发生实现分歧是由于 C++ 标准措词中出现回归。 对核心问题 2235 的解决方法中删除了一些文本，并允许对这些重载进行排序。 当前的 C++ 标准未提供机制来对这些函数进行部分排序，因此它们被视为不明确。

作为变通方法，建议不要依赖部分排序来解决此问题。 而是改用 SFINAE 来删除特定重载。 在下面的示例中，当 `Alloc` 是 `A` 的专用形式时，我们使用帮助程序类 `IsA` 删除第一个重载：

```cpp
#include <utility>
using size_t = std::size_t;

template <typename T>
struct A {};
template <size_t, size_t>
struct AlignedBuffer {};
template <size_t len>
using AlignedStorage = AlignedBuffer<len, 4>;

template <typename T> struct IsA : std::false_type {};
template <typename T> struct IsA<A<T>> : std::true_type {};

template <class T, class Alloc, typename = std::enable_if_t<!IsA<Alloc>::value>>
int f(Alloc &alloc, const AlignedStorage<T::size> &buffer)
{
    return 1;
}

template <class T, class Alloc>
int f(A<Alloc> &alloc, const AlignedStorage<T::size> &buffer)
{
    return 2;
}

struct Alloc
{
    static constexpr size_t size = 10;
};

int main()
{
    A<void> a;
    AlignedStorage<Alloc::size> buf;
    if (f<Alloc>(a, buf) != 2)
    {
        return 1;
    }

    return 0;
}
```

### <a name="illegal-expressions-and-non-literal-types-in-templated-function-definitions"></a>模板化函数定义中的非法表达式和非文本类型

现在，在显式专用化的模板化函数定义中，可以正确诊断非法表达式和非文本类型。 以前，函数定义不会发出此类错误。 但是，如果作为常量表达式的一部分进行评估，则仍然可诊断出非法表达式或非文本类型。

在早期版本的 Visual Studio 中，编译以下代码不会发出警告：

```cpp
void g();

template<typename T>
struct S
{
    constexpr void f();
};

template<>
constexpr void S<int>::f()
{
    g(); // C3615 in 15.9
}
```

在 Visual Studio 2017 版本 15.9 中，以下代码引发此错误：

```Output
error C3615: constexpr function 'S<int>::f' cannot result in a constant expression.
note: failure was caused by call of undefined function or one not declared 'constexpr'
note: see usage of 'g'.
```

为了避免此错误，请从函数 `f()` 的显式实例化中删除 `constexpr` 限定符。

## <a name="see-also"></a>请参阅

[Microsoft C++ 语言一致性表](visual-cpp-language-conformance.md)
