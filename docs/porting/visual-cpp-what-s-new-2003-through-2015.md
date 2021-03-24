---
description: 了解详细信息： Visual C++&#39;的新2003至2015
title: Visual C++ 新增功能（2003 - 2015）
ms.date: 07/02/2019
ms.assetid: c4afde6f-3d75-40bf-986f-be57e3818e26
ms.openlocfilehash: e9a036e79ba90e289e40144c4f5291c43358b850
ms.sourcegitcommit: 977b5151e7dae7584112328bab515fb15622a6cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104883904"
---
# <a name="visual-c-what39s-new-2003-through-2015"></a>Visual C++ 新增功能（2003 - 2015）

本页面包括从 Visual Studio 2003 到 Visual Studio 2015 的所有 Visual C++ 版本的“新增功能”页。 提供这些信息的目的是方便用户从早期版本的 Visual Studio 进行升级。

> [!NOTE]
> 有关当前版本的 Visual Studio 的信息，请参阅 [Visual Studio 中 Visual C++ 的新增功能](../overview/what-s-new-for-visual-cpp-in-visual-studio.md)和 [Visual Studio 中 Visual C++ 的符合性改进](../overview/cpp-conformance-improvements.md)。

## <a name="whats-new-for-c-in-visual-studio-2015"></a>Visual Studio 2015 中 C++ 的新增功能

在 Visual Studio 2015 及更高版本中，对编译器符合性的持续改进有时会改变编译器理解现有源代码的方式。 发生这种情况时，可能会在生成过程中遇到新的或不同的错误，甚至以前生成且似乎运行正常的代码也可能出现行为差异。

幸运的是，这些差异对大部分源代码没有影响或影响极小，而且需要更改源代码或进行其他更改以解决这些差异时，修补程序通常小型且简单。 我们列出了以前可接受、现在可能需要更改的许多源代码示例（之前）及其修补程序（之后）。

虽然这些差异可能会影响源代码或其他生成项目，但其不会影响 Visual C++ 版本更新之间的二进制文件兼容性。 重大更改是严重性较高的更改，可能会影响二进制文件兼容性，但此类二进制文件兼容性中断问题仅发生在 Visual C++ 的主版本之间。 例如，在 Visual C ++ 2013 和 Visual C ++ 2015 之间。 有关 Visual C++ 2013 和 Visual C++ 2015 之间的重大更改的详细信息，请参阅 [Visual C++ 更改历史记录（2003 - 2015）](../porting/visual-cpp-change-history-2003-2015.md)。

- [Visual Studio 2015 中的一致性改进](#VS_RTM)

- [Visual Studio 2015 Update 1 的符合性改进](#VS_Update1)

- [Visual Studio 2015 Update 2 中的一致性改进](#VS_Update2)

- [Visual Studio 2015 Update 3 的符合性改进](#VS_Update3)

### <a name="conformance-improvements-in-visual-studio-2015"></a><a name="VS_RTM"></a>Visual Studio 2015 的符合性改进

- **/Zc:forScope- 选项**

   `/Zc:forScope-` 编译器选项已弃用，并将从未来版本中删除。

   ```output
    Command line warning  D9035: option 'Zc:forScope-' has been deprecated and will be removed in a future release
   ```

   以前会经常用到此选项，以便允许非标准代码在点的位置之后使用循环变量，根据标准规范，这些变量本应该在范围之外。 仅在使用 `/Za` 选项进行编译时才需要，因为如果没有 `/Za`，将始终允许在循环结束后使用 for 循环变量。 如果你不在乎标准符合性（例如，如果你的代码并不要移植到其他编译器），则可关闭 `/Za` 选项（或将“禁用语言扩展”属性设置为“否”）。 如果你确实关心编写可移植且符合标准的代码，则应重写代码，以便通过将此类变量的声明移到循环以外的点使其符合标准。

   ```cpp
    // zc_forScope.cpp
    // compile with: /Zc:forScope- /Za
    // C2065 expected
    int main() {
       // Uncomment the following line to resolve.
       // int i;
       for (int i =0; i < 1; i++)
          ;
       i = 20;   // i has already gone out of scope under /Za
    }
   ```

- **Zg 编译器选项。**

   `/Zg` 编译器选项（生成函数原型）不再可用。 此此编译器选项已被弃用。

- 你无法再使用 mstest.exe 从命令行运行 C++/CLI 单元测试。 请改用 vstest.console.exe

- **可变关键字。**

   **`mutable`** 存储类说明符在以前未发生错误的情况下不再允许存在。 现在，编译器报告错误 C2071（非法存储类）。 根据标准，可变说明符仅可应用于类数据成员的名称，不能应用于声明为 const 或 static 的名称，也不能应用于引用成员。

   例如，考虑以下代码：

   ```cpp
    struct S {
        mutable int &r;
    };
   ```

   早期版本的 Microsoft C++ 编译器接受此代码，但编译器现在会报告以下错误：

   ```Output
    error C2071: 'S::r': illegal storage class
   ```

   若要修复此错误，只需删除多余的 **`mutable`** 关键字。

- **char_16_t 和 char32_t**

   你不能再使用 **`char16_t`** 或 **`char32_t`** 作为 typedef 中的别名，因为这些类型现在被视为内置。 用户和库作者经常将 **`char16_t`** 和 **`char32_t`** 分别定义为 `uint16_t` 和别名 `uint32_t` 。

   ```cpp
    #include <cstdint>

    typedef uint16_t char16_t; //C2628
    typedef uint32_t char32_t; //C2628

    int main(int argc, char* argv[])
    {
    uint16_t x = 1; uint32_t y = 2;
    char16_t a = x;
    char32_t b = y;
    return 0;
    }
   ```

   若要更新你的代码，请删除 **`typedef`** 声明并重命名与这些名称冲突的任何其他标识符。

- **非类型模板参数**

   现在会在提供显式模板参数时准确检查包含非类型模板参数的某些代码的类型符合性。 例如，在早期版本的 Visual C++ 中正确编译的以下代码。

   ```cpp
    struct S1
    {
        void f(int);
        void f(int, int);
    };

    struct S2
    {
        template <class C, void (C::*Function)(int) const> void f() {}
    };

    void f()
    {
        S2 s2;
        s2.f<S1, &S1::f>();
    }
   ```

   当前编译器正确地提供错误，因为模板参数类型不匹配模板参数 (参数是指向 const 成员的指针，但函数 f 是非常量) ：

   ```Output
    error C2893: Failed to specialize function template 'void S2::f(void)'note: With the following template arguments:note: 'C=S1'note: 'Function=S1::f'
   ```

   若要在代码中修复此错误，请确保你使用的模板自变量类型匹配模板参数声明的类型。

- **`__declspec(align)`**

   编译器不再接受函数上的 `__declspec(align)` 。 以前会始终忽略此项，但现在会产生编译器错误。

   ```cpp
    error C3323: 'alignas' and '__declspec(align)' are not allowed on function declarations
   ```

   若要解决此问题，请从函数声明中删除 `__declspec(align)` 。 因为它不起作用，将其删除不会更改任何内容。

- **异常处理**

   有几个对异常处理的更改。 首先，异常对象必须可复制或可移动。 下列代码在 Visual Studio 2013 中进行编译，但不在 Visual Studio 2015 中进行编译：

   ```cpp
    struct S {
    public:
        S();
    private:
        S(const S &);
    };

    int main()
    {
        throw S(); // error
    }
   ```

   问题在于，复制构造函数是私有的，因此对象无法像处理异常的标准过程那样进行复制。 声明复制构造函数时，也是如此 **`explicit`** 。

   ```cpp
    struct S {
        S();
        explicit S(const S &);
    };

    int main()
    {
        throw S(); // error
    }
   ```

   若要更新你的代码，请确保异常对象的复制构造函数是公用的且未标记为 **`explicit`** 。

   通过值捕获异常还要求异常对象可复制。 下列代码在 Visual Studio 2013 中进行编译，但不在 Visual Studio 2015 中进行编译：

   ```cpp
    struct B {
    public:
        B();
    private:
        B(const B &);
    };

    struct D : public B {
    };

    int main()
    {
        try
        {
        }
        catch (D d) // error
        {
        }
    }
   ```

   可以通过将的参数类型更改为引用来解决此问题 **`catch`** 。

   ```cpp
    catch(D& d)
    {
    }
   ```

- **后跟宏的字符串文本**

   编译器现在支持用户定义的文本。 因此，宏之前没有任何干预空格的字符串文本被视为用户定义的文本，这可能会产生错误或意外结果。 例如，在早期的编译器中，成功编译了以下代码：

   ```cpp
    #define _x "there"
    char* func() {
        return "hello"_x;
    }
    int main()
    {
        char * p = func();
        return 0;
    }
   ```

   编译器将此视为后跟宏的字符串文本“hello”，该宏是展开的“there”，然后两个字符串串联成一个。 在 Visual Studio 2015 中，编译器将其解释为用户定义的文本，但由于未定义所匹配的用户定义文本 _x，它将报告错误。

   ```cpp
    error C3688: invalid literal suffix '_x'; literal operator or literal operator template 'operator ""_x' not found
    note: Did you forget a space between the string literal and the prefix of the following string literal?

   ```

   若要解决此问题，请在字符串文本和宏之间添加一个空格。

- **相邻字符串文本**

   与上文类似，由于字符串分析中的相关变化，没有任何空格的相邻字符串文本（或宽或窄的字符字符串文本）被视为 Visaul C++ 早期版本中的单个串联字符串。 在 Visual Studio 2015 中，现必须在两个字符串之间添加空格。 例如，必须更改以下代码：

   ```cpp
    char * str = "abc""def";
   ```

   只需在两个字符串之间添加空间。

   ```cpp
    char * str = "abc" "def";
   ```

- **placement new 和 placement delete**

   已对操作员做出更改 **`delete`** ，使其符合 c + + 14 标准。 标准更改的详细信息位于 [C++ 调整了大小的释放](https://isocpp.org/files/papers/n3778.html)。 更改将添加 **`delete`** 采用大小参数的全局运算符的形式。 重大更改是，如果你以前使用 **`delete`** 同一签名 (的运算符与 **位置 new** 运算符) 相对应，你将收到 (C2956 的编译器错误，该错误发生在使用 **new** 运算符的位置，因为这是代码中编译器尝试标识适当匹配 **`delete`** 运算符) 的位置。

   函数 `void operator delete(void *, size_t)` 是与 C++11 中的 placement new 函数 `void * operator new(size_t, size_t)` 对应的 placement delete 运算符。 使用 c + + 14 大小的释放，此 **`delete`** 函数现在是 (全局运算符) 的 *常用释放函数* **`delete`** 。 标准要求，如果使用 "放置" 功能，则 " **新建** " 查找相应的 **`delete`** 函数并找到常用的释放函数，则程序格式不正确。

   例如，假设你的代码同时定义了一个 **新** 的和一个 **位置删除**：

   ```cpp
    void * operator new(std::size_t, std::size_t);
    void operator delete(void*, std::size_t) noexcept;
   ```

   出现此问题的原因是你定义的 **放置删除** 运算符与新的全局大小运算符之间的函数签名匹配 **`delete`** 。 考虑是否可以使用不同于 `size_t` 任何 **位置 new** 和运算符的其他类型 **`delete`** 。  请注意，的类型与 `size_t` **`typedef`** 编译器相关; 它是 **`typedef`** **`unsigned int`** Visual C++ 中的。 较好的解决办法就是使用如下的枚举类型：

   ```cpp
    enum class my_type : size_t {};
   ```

   然后，更改放置的定义 **`new`** ，并 **`delete`** 使用此类型作为第二个参数，而不是 `size_t` 。 还需要更新对 **位置 new** 的调用以传递新类型 (例如，通过使用 `static_cast<my_type>` 从整数值转换) 并更新和的定义 **`new`** **`delete`** 以强制转换回整数类型。 不需要使用 **`enum`** 来实现此操作; 包含成员的类类型 `size_t` 也将起作用。

   另一种解决方法是，可以完全消除新的 **位置** 。 如果你的代码使用 **放置 new** 实现内存池，其中位置参数是要分配或删除的对象的大小，则调整了大小的释放功能可能适合替换你自己的自定义内存池代码，并且你可以删除位置函数，仅使用自己的双参数 **`delete`** 运算符而不是放置函数。

   如果你不想立即更新代码，可通过编译器选项 `/Zc:sizedDealloc-` 还原到之前的行为。 如果使用此选项，则两参数 **`delete`** 函数不存在，因此不会导致与您的 **放置 delete** 运算符发生冲突。

- **联合数据成员**

   联合数据成员不再具有引用类型。 下列代码在 Visual Studio 2013 中成功编译，但在 Visual Studio 2015 中引发错误。

   ```cpp
    union U1 {
        const int i;
    };
    union U2 {
       int &i;
    };
    union U3 {
        struct {int &i;};
    };
   ```

   前面的代码产生以下错误：

   ```Output
    test.cpp(67): error C2625: 'U2::i': illegal union member; type 'int &' is reference type
    test.cpp(70): error C2625: 'U3::i': illegal union member; type 'int &' is reference type
   ```

   若要解决此问题，请将引用类型更改为指针或值。 更改指针类型需要对使用联合字段的代码进行更改。 将代码更改为值将更改存储在联合中的数据，这会影响其他字段，因为联合类型中的字段共享相同的内存。 根据值的大小，它还可能更改联合的大小。

- **匿名联合**

   现在更符合标准。 早期版本的编译器生成了匿名联合的显式构造函数和析构函数。 Visual Studio 2015 中已将其删除。

   ```cpp
   struct S {
      S();
   };

   union {
      struct {
         S s;
      };
   } u; // C2280
   ```

   前述代码在 Visual Studio 2015 中生成以下错误：

   ```cpp
    error C2280: '<unnamed-type-u>::<unnamed-type-u>(void)': attempting to reference a deleted function
    note: compiler has generated '<unnamed-type-u>::<unnamed-type-u>' here
   ```

   若要解决此问题，请提供你对构造函数和/或析构函数的定义。

   ```cpp
    struct S {
       // Provide a default constructor by adding an empty function body.
       S() {}
    };

    union {
       struct {
          S s;
       };
    } u;
   ```

- **具有匿名结构的联合**

   为了符合标准，已对联合中的匿名结构的成员更改了运行时行为。 创建此类联合时，将不再隐式调用联合中的匿名结构成员的构造函数。 此外，联合超出范围时，不再隐式调用联合中的匿名结构成员的析构函数。 请考虑以下代码，其中联合 U 包含一个匿名结构，此匿名结构包含的成员是一个具有析构函数的命名结构 S。

   ```cpp
    #include <stdio.h>
    struct S {
        S() { printf("Creating S\n"); }
        ~S(){ printf("Destroying S\n"); }
    };
    union U {
        struct {
        S s;
    };
        U() {}
        ~U(){}
    };

    void f()
    {
        U u;
        // Destructor implicitly called here.
    }

    int main()
    {
        f();

        char s[1024];
        printf("Press any key.\n");
        gets_s(s);
        return 0;
    }
   ```

   在 Visual Studio 2013 中，创建联合时会调用 S 的构造函数，清理函数 f 的堆栈时会调用 S 的析构函数。 但在 Visual Studio 2015 中，不调用构造函数和析构函数。 编译器会对关于此行为的更改发出警告。

   ```Output
    warning C4587: 'U::s': behavior change: constructor is no longer implicitly calledwarning C4588: 'U::s': behavior change: destructor is no longer implicitly called
   ```

   若要还原原始行为，请赋予匿名结构一个名称。 无论编译器版本为何，非匿名结构的运行时行为都是相同的。

   ```cpp
    #include <stdio.h>

    struct S {
        S() { printf("Creating S.\n"); }
        ~S() { printf("Destroying S\n"); }
    };
    union U {
        struct {
            S s;
        } namedStruct;
        U() {}
        ~U() {}
    };

    void f()
    {
        U u;
    }

    int main()
    {
        f();

        char s[1024];
        printf("Press any key.\n");
        gets_s(s);
        return 0;
    }
   ```

   或者，尝试将构造函数和析构函数代码移到新的函数中，并从联合的构造函数和析构函数添加对这些函数的调用。

   ```cpp
    #include <stdio.h>

    struct S {
        void Create() { printf("Creating S.\n"); }
        void Destroy() { printf("Destroying S\n"); }
    };
    union U {
        struct {
            S s;
        };
        U() { s.Create();  }
        ~U() { s.Destroy(); }
    };

    void f()
    {
        U u;
    }

    int main()
    {
        f();

    char s[1024];
    printf("Press any key.\n");
    gets_s(s);
    return 0;
    }
   ```

- **模板解析**

   对模板的名称解析进行了更改。 在 C++ 中，考虑名称解析的候选对象时，可能会出现作为潜在匹配项考虑的一个或多个名称生成无效的模板实例化的情况。 这些无效的实例化通常不会导致编译器错误，这被称为 SFINAE（替换失败不是错误）原则。

   现在，如果 SFINAE 要求编译器将类模板专用化进行实例化，则在此过程中发生的任何错误都是编译器错误。 在早期版本中，编译器会忽略此类错误。 例如，考虑以下代码：

   ```cpp
    #include <type_traits>

    template<typename T>
    struct S
    {
    S() = default;
    S(const S&);
    S(S&&);

    template<typename U, typename = typename std::enable_if<std::is_base_of<T, U>::value>::type>
    S(S<U>&&);
    };

    struct D;

    void f1()
    {
    S<D> s1;
        S<D> s2(s1);
    }

    struct B
    {
    };

    struct D : public B
    {
    };

    void f2()
    {
    S<D> s1;
        S<D> s2(s1);
    }
   ```

   如果使用当前编译器进行编译，将得到以下错误：

   ```Output
    type_traits(1110): error C2139: 'D': an undefined class is not allowed as an argument to compiler intrinsic type trait '__is_base_of'
    ..\t331.cpp(14): note: see declaration of 'D'
    ..\t331.cpp(10): note: see reference to class template instantiation 'std::is_base_of<T,U>' being compiled
            with
            [
                T=D,
                U=D
            ]
   ```

   这是因为在第一次调用 is_base_of 时，尚未定义类“D”。

   在这种情况下，解决方法是在定义类之前，不使用此类类型特征。 如果将 D 和 B 的定义移到代码文件的开头，错误将得到解决。 如果定义位于标头文件中，请检查标头文件的 include 语句的顺序，以确保在使用有问题的模板之前，对任何类定义进行了编译。

- **复制构造函数**

   在 Visual Studio 2013 和 Visual Studio 2015 中，如果某个类具有用户定义的移动构造函数，但没有用户定义的复制构造函数，则编译器生成该类的复制构造函数。 在 Dev14 中，此隐式生成的复制构造函数也标记为“= delete”。

### <a name="conformance-improvements-in-visual-studio-2015-update-1"></a><a name="VS_Update1"></a> Visual Studio 2015 Update 1 中的一致性改进

- **私有虚拟基类和间接继承**

   早期版本的编译器允许派生类调用 *间接派生* `private virtual` 基类的成员函数。 这种旧行为不正确，也不符合 C++ 标准。 编译器不再接受这种方式编写的代码，因此会发出编译器错误 C2280。

   ```Output
    error C2280: 'void *S3::__delDtor(unsigned int)': attempting to reference a deleted function
   ```

   示例（之前）

   ```cpp
    class base
    {
    protected:
        base();
        ~base();
    };

    class middle: private virtual base {};class top: public virtual middle {};

    void destroy(top *p)
    {
        delete p;  //
    }
   ```

   示例（之后）

   ```cpp
    class base;  // as above

    class middle: protected virtual base {};
    class top: public virtual middle {};

    void destroy(top *p)
    {
        delete p;
    }
   ```

  \- 或 -

   ```cpp
    class base;  // as above

    class middle: private virtual base {};
    class top: public virtual middle, private virtual bottom {};

    void destroy(top *p)
    {
        delete p;
    }
   ```

- **重载的 new 运算符和 delete 运算符**

   早期版本的编译器允许非成员 new 运算符和非成员 delete 运算符声明为静态，并在全局命名空间之外的命名空间中声明。  这种旧行为会导致程序无法调用程序员所需的 **`new`** 或 **`delete`** 运算符实现，从而导致无提示的运行时行为。 编译器不再接受这种方式编写的代码，因此会发出编译器错误 C2323。

   ```Output
    error C2323: 'operator new': non-member operator new or delete functions may not be declared static or in a namespace other than the global namespace.
   ```

   示例（之前）

   ```cpp
    static inline void * __cdecl operator new(size_t cb, const std::nothrow_t&)  // error C2323
   ```

   示例（之后）

   ```cpp
    void * __cdecl operator new(size_t cb, const std::nothrow_t&)  // removed 'static inline'
   ```

   此外，尽管编译器没有给予特定诊断，但内联运算符 new 被视为格式不正确。

- **对非类类型调用“operator type()”（用户定义的转换）** 早期版本的编译器允许以无提示忽略的方式对非类类型调用“operator type()”。 这种旧行为会导致无提示代码生成错误风险，从而导致不可预知的运行时行为。 编译器不再接受这种方式编写的代码，因此会发出编译器错误 C2228。

   ```Output
    error C2228: left of '.operator type' must have class/struct/union
   ```

   示例（之前）

   ```cpp
    typedef int index_t;

    void bounds_check(index_t index);

    void login(int column)
    {
        bounds_check(column.operator index_t());  // error C2228
    }
   ```

   示例（之后）

   ```cpp
    typedef int index_t;

    void bounds_check(index_t index);

    void login(int column)
    {
         bounds_check(column);  // removed cast to 'index_t', 'index_t' is an alias of 'int'
    }
   ```

- **详细的类型说明符中的冗余类型名称**  早期版本的编译器允许 **`typename`** 在详细的类型说明符中使用; 以这种方式编写的代码在语义上不正确。 编译器不再接受这种方式编写的代码，因此会发出编译器错误 C3406。

   ```Output
    error C3406: 'typename' cannot be used in an elaborated type specifier
   ```

   示例（之前）

   ```cpp
    template <typename class T>
    class container;
   ```

   示例（之后）

   ```cpp
    template <class T>  // alternatively, could be 'template <typename T>'; 'typename' is not elaborating a type specifier in this case
    class container;
   ```

- **初始值设定项列表中数组的类型推断** 早期版本的编译器不支持对初始值设定项列表中的数组进行类型推断。 编译器现在支持这种形式的类型推断，因此调用使用初始值设定项列表的函数模板现在可能会不明确，或者选择一个与以前版本的编译器不同的重载。 要解决这些问题，程序现在必须显式指定程序员所需的重载。

   当这一新行为导致重载解决方法要考虑与以往候选一样好的其他候选时，调用变得不明确，编译器会发出编译器错误 C2668。

   ```Output
    error C2668: 'function' : ambiguous call to overloaded function.
   ```

   示例 1： 对重载函数的调用不明确（之前）

   ```cpp
    // In previous versions of the compiler, code written in this way would unambiguously call f(int, Args...)
    template <typename... Args>
    void f(int, Args...);  //

    template <int N, typename... Args>
    void f(const int (&)[N], Args...);

    int main()
    {
        // The compiler now considers this call ambiguous, and issues a compiler error
        f({3});  error C2668: 'f' ambiguous call to overloaded function
    }
   ```

   示例 1： 对重载函数的调用不明确（之后）

   ```cpp
    template <typename... Args>
    void f(int, Args...);  //

    template <int N, typename... Args>
    void f(const int (&)[N], Args...);

    int main()
    {
        // To call f(int, Args...) when there is just one expression in the initializer list, remove the braces from it.
        f(3);
    }
   ```

   这一新行为会导致重载解决方法要考虑比以往候选更适合的其他候选时，调用将明确地解析为新候选，导致程序行为的更改可能与程序员的需要有所不同。

   示例 2：重载解决方法的更改（之前）

   ```cpp
    // In previous versions of the compiler, code written in this way would unambiguously call f(S, Args...)
    struct S
    {
        int i;
        int j;
    };

    template <typename... Args>
    void f(S, Args...);

    template <int N, typename... Args>
    void f(const int *&)[N], Args...);

    int main()
    {
        // The compiler now resolves this call to f(const int (&)[N], Args...) instead
        f({1, 2});
    }
   ```

   示例 2：重载解决方法的更改（之后）

   ```cpp
    struct S;  // as before

    template <typename... Args>
    void f(S, Args...);

    template <int N, typename... Args>
    void f(const int *&)[N], Args...);

    int main()
    {
        // To call f(S, Args...), perform an explicit cast to S on the initializer list.
        f(S{1, 2});
    }
   ```

- **switch 语句警告的还原**

   以前版本的编译器删除了以前存在的与语句相关的警告 **`switch`** ; 这些警告现在已还原。 编译器现在将发出还原的警告，并且现在会在包含有问题用例的行中发出与特定用例（包括默认情况下）相关的警告，而不是在 switch 语句的最后一行发出。 因此，现在发出这些警告的行与过去不同，按照需要使用 `#pragma warning(disable:####)` 可不再禁止显示以前禁止显示的警告。 要按照需要禁止显示这些警告，可能需要将 `#pragma warning(disable:####)` 指令移到第一个可能有问题的用例上面的行。 以下是还原的警告。

   ```Output
    warning C4060: switch statement contains no 'case' or 'default' labels
   ```

   ```Output
    warning C4061: enumerator 'bit1' in switch of enum 'flags' is not explicitly handled by a case label
   ```

   ```Output
    warning C4062: enumerator 'bit1' in switch of enum 'flags' is not handled
   ```

   ```Output
    warning C4063: case 'bit32' is not a valid value for switch of enum 'flags'
   ```

   ```Output
    warning C4064: switch of incomplete enum 'flags'
   ```

   ```Output
    warning C4065: switch statement contains 'default' but no 'case' labels
   ```

   ```Output
    warning C4808: case 'value' is not a valid value for switch condition of type 'bool'
   ```

   ```Output
    Warning C4809: switch statement has redundant 'default' label; all possible 'case' labels are given
   ```

   C4063 示例（之前）

   ```cpp
    class settings
    {
    public:
        enum flags
        {
            bit0 = 0x1,
            bit1 = 0x2,
            ...
        };
        ...
    };

    int main()
    {
        auto val = settings::bit1;

        switch (val)
        {
        case settings::bit0:
            break;

        case settings::bit1:
            break;

        case settings::bit0 | settings::bit1:  // warning C4063
            break;
        }
    };
   ```

   C4063 示例（之后）

   ```cpp
    class settings {...};  // as above

    int main()
    {
        // since C++11, use std::underlying_type to determine the underlying type of an enum
        typedef std::underlying_type<settings::flags>::type flags_t;

        auto val = settings::bit1;

        switch (static_cast<flags_t>(val))
        {
        case settings::bit0:
            break;

        case settings::bit1:
            break;

        case settings::bit0 | settings::bit1:  // ok
            break;
        }
    };
   ```

   在其文档中提供了其他还原警告的示例。

- #include：在路径名中使用父目录说明符“..”（只影响 `/Wall` `/WX`）

   早期版本的编译器没有检测到使用父目录说明符“..” （在  `#include` 指令的路径名中）。 以这种方式编写的代码通常用于包含因不正确使用项目相对路径而留在项目外的标头。 这一旧行为会引发风险，导致编译程序时包含了程序员不需要的源文件来，或这些相对路径不能移植到其他生成环境中。 编译器现在会检测以这种方式编写的代码并通知程序员，并发出可选编译器警告 C4464（如果已启用）。

   ```Output
    warning C4464: relative include path contains '..'
   ```

   示例（之前）

   ```cpp
    #include "..\headers\C4426.h"  // emits warning C4464
   ```

   示例（之后）

   ```cpp
    #include "C4426.h"  // add absolute path to 'headers\' to your project's include directories
   ```

   此外，虽然编译器并不会进行具体诊断，但建议不应将父目录说明符“..”用于指定项目的包含目录。

- #pragma optimize() 超出标头文件的末尾（只影响 `/Wall` `/WX`）

   早期版本的编译器无法检测到对转义翻译单元中包含的标头文件的优化标志设置的更改。 编译器现在会检测以这种方式编写的代码并通知程序员，并在有问题的 `#include`的位置发出可选编译器警告 C4426（如果已启用）。 只有更改与编译器命令行参数设置的优化标志发生冲突时，才发出此警告。

   ```Output
    warning C4426: optimization flags changed after including header, may be due to #pragma optimize()
   ```

   示例（之前）

   ```cpp
    // C4426.h
    #pragma optimize("g", off)
    ...
    // C4426.h ends

    // C4426.cpp
    #include "C4426.h"  // warning C4426
   ```

   示例（之后）

   ```cpp
    // C4426.h
    #pragma optimize("g", off)
    ...
    #pragma optimize("", on)  // restores optimization flags set via command-line arguments
    // C4426.h ends

    // C4426.cpp
    #include "C4426.h"
   ```

- #pragma warning(push) 和 #pragma warning(pop)（只影响 `/Wall` `/WX`）

   早期版本的编译器无法检测到不同源文件中与 `#pragma warning(pop)` 状态更改配对的 `#pragma warning(push)` 状态更改，这并不是我们所预期的。 这种旧行为会引发风险，导致程序编译时会启用一组程序员不希望出现的警告，可能会导致无提示的运行时行为错误。 编译器现在能够检测以这种方式编写的代码并通知程序员，并在匹配 `#pragma warning(pop)` 位置发出可选编译器警告 C5031（如果已启用）。 此警告中有一个注释，其中引用了相应 `#pragma warning(push)` 的位置。

   ```Output
    warning C5031: #pragma warning(pop): likely mismatch, popping warning state pushed in different file
   ```

   示例（之前）

   ```cpp
    // C5031_part1.h
    #pragma warning(push)
    #pragma warning(disable:####)
    ...
    // C5031_part1.h ends without #pragma warning(pop)

    // C5031_part2.h
    ...
    #pragma warning(pop)  // pops a warning state not pushed in this source file
    ...
    // C5031_part1.h ends

    // C5031.cpp
    #include "C5031_part1.h" // leaves #pragma warning(push) 'dangling'
    ...
    #include "C5031_part2.h" // matches 'dangling' #pragma warning(push), resulting in warning C5031
    ...
   ```

   示例（之后）

   ```cpp
    // C5031_part1.h
    #pragma warning(push)
    #pragma warning(disable:####)
    ...
    #pragma warning(pop)  // pops the warning state pushed in this source file
    // C5031_part1.h ends without #pragma warning(pop)

    // C5031_part2.h
    #pragma warning(push)  // pushes the warning state pushed in this source file
    #pragma warning(disable:####)
    ...
    #pragma warning(pop)
    // C5031_part1.h ends

    // C5031.cpp
    #include "C5031_part1.h" // #pragma warning state changes are self-contained and independent of other source files or their #include order.
    ...
    #include "C5031_part2.h"
    ...
   ```

   虽然不常见，但是有时会故意以这种方式编写代码。 以这种方式编写的代码对于 `#include` 顺序的更改比较敏感；如果可能，我们建议源代码文件以自包含的方式管理警告状态。

- #pragma warning(push) 不匹配（只影响 `/Wall` `/WX`）

   早期版本的编译器无法检测到翻译单元末尾出现的不匹配 `#pragma warning(push)` 状态更改。 而现在，编译器可检测按此方式编写的代码并通知程序员，还可在 `#pragma warning(push)` 不匹配的位置发出编译器警告 C5032（如已启用）。 只有翻译单元中没有任何编译错误时，才会发出此警告。

   ```Output
    warning C5032: detected #pragma warning(push) with no corresponding #pragma warning(pop)
   ```

   示例（之前）

   ```cpp
    // C5032.h
    #pragma warning(push)
    #pragma warning(disable:####)
    ...
    // C5032.h ends without #pragma warning(pop)

    // C5032.cpp
    #include "C5032.h"
    ...
    // C5032.cpp ends -- the translation unit is completed without #pragma warning(pop), resulting in warning C5032 on line 1 of C5032.h
   ```

   示例（之后）

   ```cpp
    // C5032.h
    #pragma warning(push)
    #pragma warning(disable:####)
    ...
    #pragma warning(pop) // matches #pragma warning (push) on line 1
    // C5032.h ends

    // C5032.cpp
    #include "C5032.h"
    ...
    // C5032.cpp ends -- the translation unit is completed without unmatched #pragma warning(push)
   ```

- **#pragma 警告状态跟踪改进后可能会发出更多警告**

   早期版本的编译器无法有效跟踪 `#pragma warning` 状态更改，因而无法发出所有所需的警告。 这种行为会引发风险，导致在程序不希望的情况下有效禁止显示某些警告。 而现在，编译器可更加可靠地跟踪 `#pragma warning` 状态（尤其与模板内部的 `#pragma warning` 状态更改相关），并选择性发出新警告 C5031 和 C5032，目的是帮助程序员了解意外使用 `#pragma warning(push)` 和 `#pragma warning(pop)` 的情况。

   由于 `#pragma warning` 状态更改跟踪得到了改进，现在可能会发出之前误禁的警告或与之前误诊问题相关的警告。

- **对无法访问代码标识的改进**

   针对早期版本的编译器进行的 C++ 标准库的更改和内联函数调用能力改进可能会使编译器能够证明某些代码现在无法访问。 这一新行为可能导致新警告并更频繁地发出警告 C4720 实例。

   ```Output
    warning C4720: unreachable code
   ```

   在许多情况下，只有启用优化进行编译时，才会发出此警告，因为优化可能嵌入更多函数调用，消除冗余代码或者能够确定某些代码是否无法访问。 我们发现，C4720 的新实例在 **try/catch** 块中经常发生，尤其是在使用 [std：： find](../standard-library/algorithm-functions.md#find)的情况下。

   示例（之前）

   ```cpp
    try
    {
        auto iter = std::find(v.begin(), v.end(), 5);
    }
    catch(...)
    {
        do_something();  // ok
    }
   ```

   示例（之后）

   ```cpp
    try
    {
        auto iter = std::find(v.begin(), v.end(), 5);
    }
    catch(...)
    {
        do_something();  // warning C4702: unreachable code
    }
   ```

- * * 删除 `pow(T, int)` 展开优化 * *

   以前版本的 c + + 标准库定义了一个 `pow(T, int)` 函数模板，该模板会将 `pow` 函数调用展开为一系列乘法运算。 由于浮点运算的性质，这种方法会导致大量的导致，从而导致最终结果明显不准确。 在 Visual Studio 2015 Update 1 中，此行为已被删除，以避免在使用函数时意外丢失准确性 `pow` 。 但是，此版本的 `pow` 比正确的计算快得多。 如果此更改导致显著的性能回归，并且你的项目不需要精确的浮点结果 (例如，你的项目已经用/fp： fast) 进行了编译，则请考虑将对的调用替换为以下 `pow` 解决函数：
   
   ```cpp
   template <class T> 
   inline T pow_int(T x, int y) throw() {
       unsigned int n;
       if (y >= 0) {
           n = (unsigned int)(y);
       } else {
           n = (unsigned int)(-y);
       }
       for (T z = T(1); ; x *= x) {
           if ((n & 1) != 0) {
               z *= x;
           }
           if ((n >>= 1) == 0) {
               return (y < 0 ? T(1) / z : z);
           }
       }
   }
   ```

   此实现与 Visual Studio 的早期版本中所包含的实现完全相同。

### <a name="conformance-improvements-in-visual-studio-2015-update-2"></a><a name="VS_Update2"></a>Visual Studio 2015 Update 2 的符合性改进

- **可能会因对表达式 SFINAE 的部分支持而发出其他警告和错误**

   **`decltype`** 由于缺少对表达式 SFINAE 的支持，早期版本的编译器不会分析说明符内的某些类型的表达式。 这种旧行为不正确，也不符合 C++ 标准。 由于持续的符合性改进，此编译器现已可分析这些表达式，并能为表达式 SFINAE 提供部分支持。 因此，编译器现在会发出警告和在以前版本的编译器未分析的表达式中发现的错误。

   此新行为分析包含尚未 **`decltype`** 声明的类型的表达式时，编译器会发出编译器错误 C2039。

   ```Output
    error C2039: 'type': is not a member of '`global namespace''
   ```

   示例 1：使用未声明的类型（之前）

   ```cpp
    struct s1
    {
      template <typename T>
      auto f() -> decltype(s2<T>::type::f());  // error C2039

      template<typename>
      struct s2 {};
    }
   ```

   示例 1（之后）

   ```cpp
    struct s1
    {
      template <typename>  // forward declare s2struct s2;

      template <typename T>
      auto f() -> decltype(s2<T>::type::f());

      template<typename>
      struct s2 {};
    }
   ```

   当这一新行为分析的 **`decltype`** 表达式缺少必要使用 **`typename`** 关键字来指定依赖名称是类型时，编译器会发出编译器警告 C4346 以及编译器错误 C2923。

   ```Output
    warning C4346: 'S2<T>::Type': dependent name is not a type
   ```

   ```Output
    error C2923: 's1': 'S2<T>::Type' is not a valid template type argument for parameter 'T'
   ```

   示例 2：依赖名称不是类型（之前）

   ```cpp
    template <typename T>
    struct s1
    {
      typedef T type;
    };

    template <typename T>
    struct s2
    {
      typedef T type;
    };

    template <typename T>
    T declval();

    struct s
    {
      template <typename T>
      auto f(T t) -> decltype(t(declval<S1<S2<T>::type>::type>()));  // warning C4346, error C2923
    };
   ```

   示例 2（之后）

   ```cpp
    template <typename T> struct s1 {...};  // as above
    template <typename T> struct s2 {...};  // as above

    template <typename T>
    T declval();

    struct s
    {
      template <typename T>
      auto f(T t) -> decltype(t(declval<S1<typename S2<T>::type>::type>()));
    };
   ```

- **`volatile`****成员变量会阻止隐式定义的构造函数和赋值运算符** 编译器的早期版本允许具有成员变量的类 **`volatile`** 自动生成默认复制/移动构造函数和默认复制/移动赋值运算符。 这种旧行为不正确，也不符合 C++ 标准。 编译器现在认为拥有可变成员变量的类具有非常用构造函数和赋值运算符，这将防止自动生成这些运算符的默认实现。 当此类为某一联合（或类中的匿名联合）的成员时，会将联合（或包含匿名联合的类）的复制/移动构造函数和复制/移动赋值运算符的隐式定义为已删除。 如果尝试构造或复制联合（或包含匿名联合的类）而不显示定义它们是错误的，将导致编译器发出编译器错误 C2280。

   ```Output
    error C2280: 'B::B(const B &)': attempting to reference a deleted function
   ```

   示例（之前）

   ```cpp
    struct A
    {
      volatile int i;
      volatile int j;
    };

    extern A* pa;

    struct B
    {
      union
      {
        A a;
        int i;
      };
    };

    B b1 {*pa};
    B b2 (b1);  // error C2280
   ```

   示例（之后）

   ```cpp
    struct A
    {
      int i;int j;
    };

    extern volatile A* pa;

    A getA()  // returns an A instance copied from contents of pa
    {
      A a;
      a.i = pa->i;
      a.j = pa->j;
      return a;
    }

    struct B;  // as above

    B b1 {GetA()};
    B b2 (b1);  // error C2280
   ```

- **静态成员函数不支持 cv 限定符。**

   Visual C++ 2015 的早期版本允许静态成员函数具有 cv 限定符。 此行为是由于 Visual C++ 2015 和 Visual C++ 2015 Update 1 中的回归而导致的；Visual C++ 2013 和 Visual C++ 的早期版本拒绝接受以这种方式编写的代码。 Visual C++ 2015 和 Visual C++ 2015 Update 1 的行为不正确且不符合 C++ 标准。  Visual Studio 2015 Update 2 拒绝接受以这种方式编写的代码，并改为发出编译器错误 C2511。

   ```Output
    error C2511: 'void A::func(void) const': overloaded member function not found in 'A'
   ```

   示例（之前）

   ```cpp
    struct A
    {
      static void func();
    };

    void A::func() const {}  // C2511
   ```

   示例（之后）

   ```cpp
    struct A
    {
      static void func();
    };

    void A::func() {}  // removed const
   ```

- **WinRT 代码中不允许枚举的前向声明**（仅影响 `/ZW`）

   为 Windows 运行时 (WinRT) 编译的代码不允许 **`enum`** 前向声明类型，这与使用编译器开关为 .Net Framework 编译托管 c + + 代码时相似 `/clr` 。 此行为可确保枚举大小始终为已知，并可将其正确映射到 WinRT 类型系统。 编译器将拒绝接受以这种方式编写的代码，并发出编译器错误 C2599 和编译器错误 C3197。

   ```Output
    error C2599: 'CustomEnum': the forward declaration of a WinRT enum is not allowed
   ```

   ```Output
    error C3197: 'public': can only be used in definitions
   ```

   示例（之前）

   ```cpp
    namespace A {
      public enum class CustomEnum: int32;  // forward declaration; error C2599, error C3197
    }

    namespace A {
      public enum class CustomEnum: int32
      {
        Value1
      };
    }

    public ref class Component sealed
    {
    public:
      CustomEnum f()
      {
        return CustomEnum::Value1;
      }
    };
   ```

   示例（之后）

   ```cpp
              // forward declaration of CustomEnum removed
    namespace A {
      public enum class CustomEnum: int32
      {
        Value1
      };
    }

    public ref class Component sealed
    {
    public:
      CustomEnum f()
      {
        return CustomEnum::Value1;
      }
    };
   ```

- **重载的非成员运算符 new 和运算符 delete 可能不是以内联方式声明的**（默认开启等级 1 (`/W1`)）

   当以内联方式声明非成员运算符 new 和运算符 delete 函数时，编译器的早期版本不会发出警告。 按方式编写的代码格式有误（无需诊断），并且可能由于不匹配的 new 和 delete 运算符（尤其是与调整了大小的释放共同使用时）而导致难以诊断的内存问题。 编译器现将发出编译器警告 C4595 以帮助识别以这种方式编写的代码。

   ```Output
    warning C4595: 'operator new': non-member operator new or delete functions may not be declared inline
   ```

   示例（之前）

   ```cpp
    inline void* operator new(size_t sz)  // warning C4595
    {
      ...
    }
   ```

   示例（之后）

   ```cpp
    void* operator new(size_t sz)  // removed inline
    {
      ...
    }
   ```

   修复以这种方式编写的代码可能需要将运算符定义从头文件移动到相应的源文件中。

### <a name="conformance-improvements-in-visual-studio-2015-update-3"></a><a name="VS_Update3"></a> Visual Studio 2015 Update 3 中的一致性改进

- **现在，std::is_convertable 可以检测自我赋值**（标准库） 以前版本的 `std::is_convertable` type-trait 在其复制构造函数被删除或私有时，无法正确检测类类型的自我赋值。 现在， `std::is_convertable<>::value` **`false`** 在应用于具有已删除或私有复制构造函数的类类型时，正确设置为。

   没有与此更改相关联的编译器诊断。

   示例

   ```cpp
    #include <type_traits>

    class X1
    {
    public:
        X1(const X1&) = delete;
    };

    class X2
    {
    private:
        X2(const X2&);
    };

    static_assert(std::is_convertible<X1&, X1>::value, "BOOM");static_assert(std::is_convertible<X2&, X2>::value, "BOOM");
   ```

   在 Visual C++ 的以前版本中，此示例底部的静态断言通过，因为 `std::is_convertable<>::value` 错误地设置为 **`true`** 。 现在， `std::is_convertable<>::value` 正确设置为 **`false`** ，导致静态断言失败。

- **默认设置或已删除的日常复制和移动构造函数遵从访问说明符**

   对于默认设置或已删除的日常复制和移动构造函数的访问说明符，早期版本的编译器在允许调用之前不进行检查。 这种旧行为不正确，也不符合 C++ 标准。 在某些情况下，这种旧行为会导致无提示代码生成错误风险，从而导致不可预知的运行时行为。 现在，编译器检查默认设置或已删除的日常复制和移动构造函数的访问说明符，以确定是否能调用它，如果不能，则发出编译器警告 C2248。

   ```Output
    error C2248: 'S::S' cannot access private member declared in class 'S'
   ```

   示例（之前）

   ```cpp
    class S {
    public:
       S() = default;
    private:
        S(const S&) = default;
    };

    void f(S);  // pass S by value

    int main()
    {
        S s;
        f(s);  // error C2248, can't invoke private copy constructor
    }
   ```

   示例（之后）

   ```cpp
    class S {
    public:
       S() = default;
    private:
        S(const S&) = default;
    };

    void f(const S&);  // pass S by reference

    int main()
    {
        S s;
        f(s);
    }
   ```

- **弃用属性化 ATL 代码支持**（默认开启等级 1 (`/W1`)）

   以前版本的编译器支持属性化 ATL 代码。 由于下一阶段将删除[从 Visual C++ 2008](#whats-new-for-c-in-visual-studio-2008) 开始的属性化 ATL 代码支持，所以已弃用属性化 ATL 代码。 编译器现将发出编译器警告 C4467 以帮助识别这类已弃用的代码。

   ```Output
    warning C4467: Usage of ATL attributes is deprecated
   ```

   若要在编译器删除支持之前继续使用属性化 ATL 代码，可以通过将 `/Wv:18` 或 `/wd4467` 命令行参数传递给编译器或在源代码中添加 `#pragma warning(disable:4467)` 来禁用此警告。

   示例 1（之前）

   ```cpp
              [uuid("594382D9-44B0-461A-8DE3-E06A3E73C5EB")]
    class A {};
   ```

   示例 1（之后）

   ```cpp
    __declspec(uuid("594382D9-44B0-461A-8DE3-E06A3E73C5EB")) A {};
   ```

   有时需要创建 IDL 文件以避免使用已弃用的 ATL 属性，如以下示例代码所示

   示例 2（之前）

   ```cpp
    [emitidl];
    [module(name="Foo")];

    [object, local, uuid("9e66a290-4365-11d2-a997-00c04fa37ddb")]
    __interface ICustom {
        HRESULT Custom([in] long l, [out, retval] long *pLong);
        [local] HRESULT CustomLocal([in] long l, [out, retval] long *pLong);
    };

    [coclass, appobject, uuid("9e66a294-4365-11d2-a997-00c04fa37ddb")]
    class CFoo : public ICustom
    {
        // ...
    };
   ```

   首先，创建 *.idl 文件；vc140.idl 生成的文件可用于获取包含接口和注释的 \*.idl文件。

   然后，将 MIDL 步骤添加到生成中以确保生成 C++ 接口定义。

   示例 2 IDL（之后）

   ```cpp
    import "docobj.idl";

    [
        object,local,uuid(9e66a290-4365-11d2-a997-00c04fa37ddb)
    ]

    interface ICustom : IUnknown {
        HRESULT  Custom([in] long l, [out,retval] long *pLong);
        [local] HRESULT  CustomLocal([in] long l, [out,retval] long *pLong);
    };

    [ version(1.0), uuid(29079a2c-5f3f-3325-99a1-3ec9c40988bb) ]
    library Foo
    {
        importlib("stdole2.tlb");
        importlib("olepro32.dll");
            [
                version(1.0),
                appobject,uuid(9e66a294-4365-11d2-a997-00c04fa37ddb)
            ]

        coclass CFoo {
            interface ICustom;
        };
    }
   ```

   然后，在实现文件中直接使用 ATL，如以下示例代码所示。

   示例 2 实现（之后）

   ```cpp
    #include <idl.header.h>
    #include <atlbase.h>

    class ATL_NO_VTABLE CFooImpl :
        public ICustom,
        public ATL::CComObjectRootEx<CComMultiThreadModel>
    {
    public:
        BEGIN_COM_MAP(CFooImpl)
        COM_INTERFACE_ENTRY(ICustom)
        END_COM_MAP()
    };
   ```

- **预编译标头 (PCH) 文件和不匹配的 #include 指令**（仅影响 `/Wall` `/WX`）

   使用预编译标头 (PCH) 文件时，以前版本的编译器接受 `-Yc` 和 `-Yu` 编译之间的源文件中不匹配的 `#include` 指令。 编译器不再接受以这种方式编写的代码。 使用 PCH 文件时，编译器现将发出编译器警告 CC4598 以帮助识别不匹配的 `#include` 指令。

   ```Output
    warning C4598: 'b.h': included header file specified for Ycc.h at position 2 does not match Yuc.h at that position
   ```

   示例（之前）：

   X.cpp (-Ycc.h)

   ```cpp
    #include "a.h"
    #include "b.h"
    #include "c.h"
   ```

   Z.cpp (-Yuc.h)

   ```cpp
    #include "b.h"
    #include "a.h"  // mismatched order relative to X.cpp
    #include "c.h"
   ```

   示例（之后）

   X.cpp (-Ycc.h)

   ```cpp
    #include "a.h"
    #include "b.h"
    #include "c.h"
   ```

   Z.cpp (-Yuc.h)

   ```cpp
    #include "a.h"
    #include "b.h" // matched order relative to X.cpp
    #include "c.h"
   ```

- **预编译标头 (PCH) 文件和不匹配的包含目录**（仅影响 `/Wall` `/WX`）

   使用预编译标头 (PCH) 文件时，对于 `-Yc` 和 `-Yu` 编译之间的编译器，以前版本的编译器接受不匹配的包含目录 (`-I`) 命令行参数。 编译器不再接受以这种方式编写的代码。   使用 PCH 文件时，编译器现将发出编译器警告 CC4599 以帮助识别不匹配的包含目录 (`-I`) 命令行参数。

   ```Output
    warning C4599: '-I..' : specified for Ycc.h at position 1 does not match Yuc.h at that position
   ```

   示例（之前）

   ```ms-dos
    cl /c /Wall /Ycc.h -I.. X.cpp
    cl /c /Wall /Yuc.h Z.cpp
   ```

   示例（之后）

   ```ms-dos
    cl /c /Wall /Ycc.h -I.. X.cpp
    cl /c /Wall /Yuc.h -I.. Z.cpp
   ```

## <a name="whats-new-for-c-in-visual-studio-2013"></a>Visual Studio 2013 中 C++ 的新增功能

### <a name="improved-iso-cc-standards-support"></a>改进的 ISO C/C++ 标准支持

#### <a name="compiler"></a>编译器

MSVC 支持以下 ISO C++11 语言功能：

- 函数模板的默认模板参数。
- 委托构造函数
- 显式转换运算符。
- 初始值设定项列表和统一初始化。
- 原始字符串文本。
- 可变参数模板。
- 别名模板。
- 已删除的函数。
- 非静态数据成员初始值设定项 (NSDMI)。
- 默认的函数。 \*
- 支持以下 ISO C99 语言功能：
- _Bool
- 复合文本。
- 指定的初始值设定项。
- 组合带有代码的声明。
- 字符串文本转换为可修改的值可通过使用新编译器选项 `/Zc:strictStrings` 禁用。 在 c + + 98 中，已弃用从字符串文本到 **`char*`** (和宽字符串) 文本的转换 `wchar_t*` 。 在 C++11 中，已将转换完全移除。 虽然编译器可以严格遵循该标准，但提供了 `/Zc:strictStrings` 选项，以便您控制转换。 默认情况下，该选项是关闭的。 注意，当您在调试模式下使用此选项，STL 将无法编译。
- rvalue/lvalue 引用转换。 通过 rvalue 引用，C++11 可清晰地区分 lvalue 和 rvalue。 过去，在特定强制转换方案中，编译器不提供此功能。 已添加新编译器选项（`/Zc:rvalueCast`），以使编译器与 C++ 语言的工作文件相符（请参阅第 5.4 节 [expr.cast]/1）。 未指定选项时，该默认行为与 Visual Studio 2012 中的相同。

> [!NOTE]
> 对于默认函数，不支持使用 =default 请求逐一移动成员的构造函数和移动赋值运算符。

### <a name="c99-libraries"></a>C99 库

为下列标头中缺少的函数添加了声明和实现：math.h、ctype.h、wctype.h、stdio.h、stdlib.h 和 wchar.h。 同样添加的还有新标头 complex.h、stdbool.h、fenv.h 和 inttypes.h，以及这些新标头中声明的所有函数的实现。 新增了一些 C++ 包装器标头（ccomplex、cfenv、cinttypes 和 ctgmath）并更新了许多其他标头（ccomplex、cctype、clocale、cmath、cstdint、cstdio、cstring、cwchar 和 cwctype）。

### <a name="standard-template-library"></a>标准模板库

支持 C++11 显式转换运算符、初始值设定项列表、范围枚举和 variadic 模板。
现在所有容器都支持 C++11 细化的元素要求。
支持这些 C++14 功能：

- “透明运算符函子”less<>、greater<>、plus<>、multiplies<> 等等。
- make_unique\<T>(args...) 和 make_unique <T[]>(n)
- cbegin()/cend()、rbegin()/rend() 和 crbegin()/crend() 非成员函数。
- \<atomic> 接收了许多性能增强功能。
- \<type_traits> 收到重大的稳定和代码修补程序。

### <a name="breaking-changes"></a>重大更改

对 ISO C/C++ 标准的改进支持可能需要对现有代码进行更改，从而符合 C++11 并在 Visual Studio 2013 的 Visual C++ 中正确编译。

### <a name="visual-c-library-enhancements"></a>Visual C++ 库增强功能

- 添加了 C++ REST SDK。 它具有 REST 服务的现代 C++ 实现。
- C++ AMP 纹理支持已改进。 现在包括对 mipmap 和新采样模式的支持。
- PPL 任务支持多个计划技术和异步调试。 采用新 API，可为常规结果和异常条件创建 PPL 任务。

### <a name="c-application-performance"></a>C++ 应用程序性能

- 自动向量化现在可以识别和优化更多 C++ 模式，加快代码运行速度。
- ARM 平台和 Atom 微型体系结构代码质量增强功能。
- 添加了 __vectorcall 调用约定。 使用 __vectorcall 调用约定来传递向量类型参数，从而使用向量寄存器。
- 新链接器选项。 使用 `/Gw`（编译器）和 `/Gy`（汇编）开关，使链接器优化生成精简二进制文件。
- C++ AMP 共享内存支持，可减少或消除 CPU 和 GPU 间的数据复制。

### <a name="profile-guided-optimization-pgo-enhancements"></a>按配置优化选项 (PGO) 增强

- 通过使用 PGO 实现已优化的应用程序工作集的缩减，从而提高性能。
- 用于 Windows 运行时应用开发的新 PGO。

### <a name="windows-runtime-app-development-support"></a>Windows 运行时应用开发支持

- **支持值结构中的装箱类型。**

   现在可以使用可以为空的字段（例如，而不是）来定义值类型 `IBox<int>^` **`int`** 。 这意味着字段可以具有值，也可以等于 **`nullptr`** 。

- **更丰富的异常信息。**

   C++/CX 支持能够在整个应用程序二进制接口 (ABI) 中获取和传播各种异常信息的新 Windows 错误模型；这包括调用堆栈和自定义消息字符串。

- **Object::ToString() 现在为虚拟。**

   现在可以重写用户定义的 Windows 运行时引用类型中的 ToString。

- **支持已弃用的 API。**

   公共 Windows 运行时 API 现在可标记为已弃用并可收到一条自定义消息，此消息显示为生成警告并可提供迁移指南。

- **调试器改进。**

   支持本机/JavaScript 互操作调试、Windows 运行时异常诊断和异步代码调试（windows 运行时和 PPL）。

> [!NOTE]
> 除本节中介绍的 C++ 特定功能和增强功能外，Visual Studio 中的其他增强功能还可帮助你编写更好的 Windows 运行时应用。

### <a name="diagnostics-enhancements"></a>诊断增强功能

- 调试器改进。 支持异步调试和“仅我的代码”调试。
- 代码分析类别。 现在可以查看代码分析器的分类输出，帮助您找到并修复代码缺陷。
- XAML 诊断。 现在可以诊断 XAML 中的 UI 响应和电池使用情况问题。
- 图像和 GPU 调试改进。
- 在实际设备上远程捕获和重放。
- 同步 C++ AMP 和 CPU 调试。
- 改进的 C++ AMP 运行时诊断。
- HLSL 计算着色器跟踪调试。

### <a name="3-d-graphics-enhancements"></a>三维图形增强功能

- 图像内容管线支持预乘 alpha DDS 格式。
- 图像编辑器使用内部预乘 alpha 进行呈现，从而避免呈现暗的光晕等项目。
- 图像和模型编辑器。 图像编辑器和模型编辑器的“着色器设计器”现在支持用户定义的筛选器创建。

### <a name="ide-and-productivity"></a>IDE 与工作效率

**已改进的代码格式设置**。 您可以将多个格式设置应用于 C++ 代码。 使用这些设置，您可以控制大括号和关键字、缩进、间距和自动换行的新行位置。 当完成语句和块并且将代码粘贴到文件中时，代码将自动进行格式化。

**大括号完成。** 现在，C++ 代码会自动完成对应于这些开始字符的结束字符：

- {（大括号）
- [（方括号）
- (（括号）
- '（单引号）
- "（双引号）

**附加 C++ 自动完成功能。**

- 添加用于类类型的分号。
- 完成对原始字符串文本使用括号。
-  (/ \* /) 完成多行注释 \*

**查找所有引用** 在后台引用显示出文本匹配列表后自动对其进行解析和筛选。

**基于上下文的成员列表筛选。** 无法访问的成员已从 IntelliSense 成员列表中筛选出来。 例如，私有成员不会在成员列表中显示，除非您修改了实现此类型的代码。 打开成员列表时，可以按 **Ctrl** + **J** 删除筛选 (仅适用于当前成员列表窗口) 。 您可以再次按 **Ctrl** + **J** 删除文本筛选并显示每个成员。

**参数帮助滚动。** 参数帮助工具提示中显示的函数签名现在将根据实际输入参数的数量而改变，而不是只显示一个随机的签名且不根据当前上下文更新。 函数显示在嵌套函数上时，参数也会适当地帮助函数。

**切换标头/代码文件。** 现在，通过使用快捷菜单或键盘快捷方式上的命令，可以在标题及其相应代码文件之间切换。

**可调整大小的 C++ 项目属性窗口。**

**在 C++/CX 和 C++/CLI 中自动生成事件处理程序代码。**  在键入代码向 C++/CX 或 C++/CLI 代码文件中添加事件处理程序时，编辑器可以自动生成委托实例和事件处理程序定义。 可以自动生成事件处理程序代码时，会显示工具提示窗口。

**DPI 识别增强功能。** 现在，针对应用程序清单文件的 DPI 识别设置支持“每个高 DPI 识别监视器”的设置。

**更快的配置切换。** 对于大型应用程序，切换配置（尤其是后续切换操作）将更快速地执行。

**生成时效。** 许多优化和多核使用率使生成更加快速，对于大型项目来说尤为如此。 引用了 C++ WinMD 的 C++ 应用程序的增量生成也更加快速。

## <a name="whats-new-for-c-in-visual-studio-2012"></a>Visual Studio 2012 中 C++ 的新增功能

### <a name="improved-c11-standards-support"></a>改进的 C++11 标准支持

#### <a name="standard-template-library"></a>标准模板库

- 支持新的 STL 标头： \<atomic> 、 \<chrono> 、 \<condition_variable> 、 \<filesystem> 、、、 \<future> \<mutex> \<ratio> 和 \<thread> 。
- 为了优化内存资源的使用，现在减小了容器。 例如，在具有默认设置的 x86 发布模式下，`std::vector` 从 Visual Studio 2010 中的 16 字节缩小为 Visual Studio 2012 中的 12 字节，而 `std::map` 也从 Visual Studio 2010 中的 16 字节缩小为 Visual Studio 2012 中的 8 字节。
- 作为 C++ 11 标准的一个可选项，SCARY 迭代器也得到了实现。

#### <a name="other-c11-enhancements"></a>其他 C + + 11 增强功能

- 基于范围的 for 循环。 可编写以 for ( for-range-declaration : expression ) 形式使用数组、STL 容器和 Windows 运行时集合的更可靠循环。 这是核心语言支持的一部分。
- 无状态 lambda（即以空的 lambda 引导 [] 为开头并且不捕获本地变量的代码块）现在可以根据需要以 C++11 标准隐式转换为函数指针。
- 区分范围的枚举支持。 现在支持 C++ 枚举类 enum-key。 下面的代码示范了现在的 enum-key 与以前的枚举行为的不同之处。

   ```cpp
  enum class Element { Hydrogen, Helium, Lithium, Beryllium };
  void func1(Element e);
  func1(Hydrogen); // error C2065: 'Hydrogen' : undeclared identifier
  func1(Element::Helium); // OK
   ```

### <a name="windows-runtime-app-development-support"></a>Windows 运行时应用开发支持

- **基于本机 XAML 的 UI 模型**。 可将新的基于本机 XAML 的 UI 模型用于 Windows 运行时应用。
- **Visual C++ 组件扩展**。 这些扩展减少了 Windows 运行时对象的使用，是 Windows 运行时应用不可缺少的部分。 有关详细信息，请参阅[使用 C++ 的 Windows 运行时应用产品指南](../cppcx/universal-windows-apps-cpp.md)和 [Visual C++ 语言参考 (C++/CX)](../cppcx/visual-c-language-reference-c-cx.md)
- **DirectX 游戏**。 可以利用 Windows 运行时应用的新的 DirectX 支持来开发有趣的游戏。
- **XAML/DirectX 交互**。 使用 XAML 和 DirectX 的 Windows 运行时应用现可有效进行交互。
- **Windows 运行时组件 DLL 的开发**。 组件 DLL 的开发使 Windows 运行时环境能够扩展。

### <a name="compiler-and-linker"></a>编译器和链接器

- **自动向量化**。 编译器会分析代码中的循环，如果可能，还会发出使用向量寄存器的指令和显示在所有新式处理器上的指令。 这使循环运行得更快。 （处理器指令被称为 SSE，即流式处理 SIMD 扩展）。 此项优化自动应用，无需启用或请求。
- **自动并行**。 编译器可以分析编码中的循环并发出指令，向多个核心或处理器分散计算。 这能让循环运行得更快。 此项优化不会自动启用，必须进行请求。 在许多情况下，这都有助于在想并行化的循环之前将 `#pragma loop(hint_parallel(N))` 循环 (hint_parallel(N)) 包含在你的代码中。
- 自动向量化和自动并行化可以共同协作以便向多个核心分散计算，并且每个核心上的代码都使用该核心的向量寄存器。

### <a name="new-in-visual-studio-2012-update-1"></a>Visual Studio 2012 Update 1 中的新功能

在生成 C++ 代码时锁定 Windows XP。
可以使用 Microsoft C++ 编译器和库来锁定 Windows XP 和 Windows Server 2003。

#### <a name="parallel-programming-support"></a>并行编程的支持

##### <a name="c-accelerated-massive-parallelism-amp"></a>C++ Accelerated Massive Parallelism (AMP)

C++ AMP 利用数据并行硬件（通常是离散图形卡上的 GPU）加快 C++ 代码的执行。 C++ AMP 编程模型包括多维数组、索引、内存传输、平铺和数学函数库。 通过使用 C++ AMP 语言扩展和编译器限制，可以控制数据在 CPU 和 GPU 之间来回移动的方式。

**调试.** 使用 C++ AMP 锁定 GPU 的应用的调试体验就像其他 C++ 应用的调试一样。 这包含之前提到的新的并行调试补充内容。

**分析.** 现在对基于 C++ AMP 的 GPU 活动和其他基于 Direct3D 的编程模型提供分析支持。

#### <a name="general-parallel-programming-enhancements"></a>并行编程的常规增强功能

随着硬件移至多内核和很多内核的结构体系，开发人员可以不再依赖于单内核日益增长的时钟速度。 并发运行时中的并行编程支持使开发人员可以利用这些新的体系结构。
Visual Studio 2010 中引入了像并行模式库这样功能强大的 C++ 并行化库，还有其他功能可以用于利用通过准确精细的数据流管道实现的并发。 在 Visual Studio 2012 中将这些库进行了扩展以在并行模式上提供更好的性能、更强的控制力以及更丰富的支持，这也是开发人员最需要的。 现在产品涵盖以下内容：

- 丰富的基于任务的编程模型，支持异步和延续。
- 支持分叉-联结并行性（parallel_for、具有相关性的 parallel_for、parallel_for_each、parallel_sort、parallel_reduce 以及 parallel_transform）的并行算法。
- 并发安全容器，提供线程安全版本的 std 数据结构（例如 priority_queue、queue、vector 和 map）。
- 异步代理库，开发人员可以将其用于表达自然分散至并发单元的数据流管道。
- 可自定义的计划程序和资源管理器，便于将此列表中的模式平滑组合。

##### <a name="general-parallel-debugging-enhancements"></a>并行调试的常规增强功能

除了 " **并行任务** " 窗口和 " **并行堆栈** " 窗口，Visual Studio 2012 还提供了一个新的 " **并行监视** " 窗口，以便你可以跨所有线程和进程检查表达式的值，并对结果执行排序和筛选。 也可使用可视化工具来扩展窗口，并可利用跨所有工具窗口的全新多进程支持。

### <a name="ide"></a>IDE

**Visual Studio 模板支持。** 现在可以使用 Visual Studio 模板技术创建 C++ 项目和项模板。

**异步解决方案加载。** 项目现在异步加载（解决方案的关键部分优先加载），从而提供工作效率。

**用于远程调试的自动部署。** Visual C++ 中简化了用于远程调试的文件部署。 项目上下文菜单上的 " **部署** " 选项会自动将调试配置属性中指定的文件复制到远程计算机。 不再需要手动将文件复制到远程计算机了。

**C++/CLI IntelliSense。** C++/CLI 现在完全支持 IntelliSense。 像快速信息、参数帮助、列表成员和自动完成这样的 IntelliSense 功能现在适用于 C++/CLI。 此外，本文档中列出的其他 IntelliSense 和 IDE 增强也适用于 C++/CLI。

**更丰富的 IntelliSense 工具提示。** C++ IntelliSense 快速信息工具提示现在显示更丰富的 XML 文档注释样式信息。 如果使用库中具有 XML 文档注释的 API（例如 C++ AMP），除声明以外，IntelliSense 工具提示还会显示更多信息。 此外，如果你的代码具有 XML 文档注释，IntelliSense 工具提示将显示更丰富的信息。

**C++ 代码构造。** 主干代码可用于 switch、if-else、for 循环以及列表成员下拉列表中的其他基本代码构造。 从列表中选择一部分代码插入到你的代码中，然后填写需要的逻辑。 还可以创建自己的自定义代码片段，以便在编辑器中使用。

**列表成员的增强功能。** 在代码编辑器中键入代码时，会自动显示 " **列出成员** " 下拉列表。 结果是筛选过的，所以只会显示与你的类型相关的成员。 可控制成员列表采用的筛选逻辑。为此，请在“文本编辑器” > “C/C++” > “高级”下的“选项”对话框中操作。

**语义着色。** 现在默认对类型、枚举、宏和其他 C++ 标记进行着色。

**引用突出显示。** 选择一个符号现在将在当前文件中突出显示该符号的所有实例。 按 **ctrl** + **shift** + **向上键** 或按 **ctrl** + **shift** + **向下** 键在突出显示的引用之间移动。 可在“文本编辑器” > “C/C++” > “高级”下的“选项”对话框中关闭此功能。

### <a name="application-lifecycle-management-tools"></a>Application Lifecycle Management 工具

#### <a name="static-code-analysis"></a>静态代码分析

已更新 C++ 的静态代码分析，以提供更丰富的错误上下文信息、更多分析规则以及更好的分析结果。 在新的代码分析窗口，可按关键字、项目以及严重性来筛选消息。 当在窗口中选择一条消息时，代码编辑器会突出显示触发该消息的代码行。 对于某些 C++ 警告，消息会列出源行，显示导致该警告的执行路径；并突出显示采用该特定路径决策点和理由。
Visual Studio 2012 的大部分版本都包含代码分析。 Professional 版、Premium 版和 Ultimate 版中包含了所有规则。 Windows 8 和 Windows Phone 的 Express 版中只包括最关键的警告。 Express 网页版中不包括代码分析。
下面是代码分析的一些其他增强：

- 新的并发警告可确保在多线程 C/C++ 程序中使用正确的锁定方式，帮助规避并发 Bug。 分析器检测潜在的争用条件、锁定顺序反转、调用方/被调用方锁定协定冲突、不匹配的同步操作和其他并发 Bug。
- 你可以通过使用规则集指定想应用于代码分析的 C++ 规则。
- 在 " **代码分析** " 窗口中，您可以将禁止显示选定警告的杂注插入源代码中。
- 通过使用新版的 Microsoft 源代码注释语言 (SAL) 描述函数使用参数的方式、关于参数做出的假设以及对完成结果的保证，可以增强静态代码分析的准确性和完整性。
- 支持 64 位 C++ 项目。

#### <a name="updated-unit-test-framework"></a>已更新的单元测试框架

可使用 Visual Studio 中新的 C++ 单元测试框架编写 C++ 单元测试。 可通过在“新建项目”对话框的“Visual C++ 类别”下找到 C++ 单元测试项目模板，将新的单元测试项目添加到现有 C++ 解决方案。 开始在生成的 TEST_METHOD 代码（在 Unittest1.cpp 文件中存根）中编写你的单元测试。 编写测试代码时，生成解决方案。 要运行测试，请通过“视图” > “其他窗口” > “单元测试管理器”打开“单元测试资源管理器”窗口，然后在所需测试用例的快捷菜单上选择“运行所选测试”。 测试运行完成以后，可以在同一窗口查看结果和其他堆栈跟踪信息。

#### <a name="architecture-dependency-graphs"></a>体系结构依赖项关系图

为了更好地理解代码，可以对二进制、类、命名空间和解决方案中的包含文件生成依赖项关系图。 在菜单栏上，依次选择 "**体系结构**" "  >  **生成依赖关系图**" 和 "**解决方案**"，或选择 "**包含文件**" 以生成依赖项关系图。 Graph 生成完成后，可以通过展开每个节点来浏览，通过在节点间移动来了解依赖关系，并通过在节点的快捷菜单上选择 " **查看内容** " 来浏览源代码。 若要为包含文件生成依赖项关系图，请在 \* .cpp 源代码文件或 .h 头文件的快捷菜单上 \* ，选择 " **生成包含文件的关系图**"。

#### <a name="architecture-explorer"></a>体系结构资源管理器

使用 " **体系结构资源管理器**" 可以浏览 c + + 解决方案、项目或文件中的资产。 在菜单栏上，选择 **体系结构**  >  **Windows**  >  **体系结构资源管理器**。 您可以选择您感兴趣的节点，例如 **类视图**。 这时工具窗口的右侧会展开一个命名空间列表。 如果选择一个命名空间，会出现一个新列，显示此命名空间中的类、结构和枚举列表。 你可以继续浏览这些资产，也可以返回最左侧的那一列开始另一项查询。 请参阅 **通过体系结构资源管理器查找代码**。

#### <a name="code-coverage"></a>代码覆盖率

已更新代码覆盖率，在运行时动态检测二进制。 这降低了配置开销，提升了性能。 你还可以从 C++ 应用的单元测试收集代码覆盖率数据。 创建 c + + 单元测试后，可以使用 " **单元测试资源管理器** " 发现解决方案中的测试。 若要运行单元测试并为其收集代码覆盖率数据，请在 **单元测试资源管理器** 中选择 " **分析代码覆盖率**"。 可在“代码覆盖率结果”窗口（在菜单栏上选择“测试” > “窗口” > “代码覆盖率结果”）中查看代码覆盖率结果。

## <a name="whats-new-for-c-in-visual-studio-2010"></a>Visual Studio 2010 中 C++ 的新增功能

### <a name="c-compiler-and-linker"></a>C++ 编译器和链接器

**auto 关键字。** **`auto`** 关键字具有新用途。 使用关键字的默认含义 **`auto`** 声明一个变量，该变量的类型是从变量声明中的初始化表达式推导出来的。 `/Zc:auto`编译器选项调用关键字的新含义或上一个含义 **`auto`** 。

**decltype 类型说明符。** **`decltype`** 类型说明符返回指定表达式的类型。 将 **`decltype`** 类型说明符与关键字结合使用 **`auto`** ，以声明只对编译器而言复杂或已知的类型。 例如，将这个组合用于声明返回类型取决于其模板自变量类型的模板函数。 或者声明调用其他函数且返回被调用函数类型的模板函数。

**Lambda 表达式。** Lambda 函数有函数体但没有名称。 lambda 函数兼具函数指针和函数对象的最佳特性。 使用 lambda 函数本身作为模板函数参数而不是函数对象，或将其与关键字一起使用 **`auto`** 来声明类型为 lambda 的变量。

**Rvalue 引用。** Rvalue 引用声明符 (&&) 声明对 rvalue 的引用。 通过 rvalue 引用，可以使用更多语义和完美转移编写出更有效的构造函数、函数和模板。

**static_assert 声明。** **`static_assert`** 声明在编译时测试软件断言，这不同于在运行时测试的其他断言机制。 如果断言失败，则编译将失败，并且会发出特定的错误消息。

**nullptr 和 __nullptr 关键字。** MSVC 使你可以将 **`nullptr`** 关键字与本机代码或托管代码一起使用。 **`nullptr`** 关键字指示对象句柄、内部指针或本机指针类型不指向对象。 当 **`nullptr`** 你使用编译器选项时，编译器将解释为托管代码 `/clr` ，当你不使用选项时，它将解释为本机代码 `/clr` 。
特定于 Microsoft 的 **__nullptr** 关键字与相同 **`nullptr`** ，但它仅适用于本机代码。 如果使用编译器选项编译本机 C/c + + 代码 `/clr` ，则编译器无法确定 **`nullptr`** 关键字是本机术语还是托管术语。 若要使编译器清晰明了，请使用 nullptr 关键字指定托管术语，并 **__nullptr** 指定本机术语。

**`/Zc:trigraphs` 编译器选项。** 默认情况下，禁用对三元组的支持。 使用 **`/Zc:trigraphs`** 编译器选项启用 trigraphs 支持。
三元祖由两个连续的问号 (??) 及后跟的唯一的第三个字符组成。 编译器将三元组替换为相应的标点字符。 例如，编译器会将三元祖 ??= 替换为字符 #（数字字符）。 对于使用的字符集未包含某些标点字符的 C 源文件，可在其中使用三元组。

**新的按配置优化选项。** PogoSafeMode 是一个新的按配置优化选项，使用此选项可以指定在优化应用程序时使用安全模式还是快速模式。 安全模式是线程安全的，但比快速模式的运行速度慢。 快速模式是默认行为。

**新的公共语言运行时 (CLR) 选项/clr:nostdlib。** 为 `/clr`（公共语言运行时编译）新增了一个选项。 如果包含同一库的不同版本，则会发出编译器错误。 使用该新选项可以排除默认的 CLR 库，以便程序可以使用指定版本。

**新的杂注指令 detect_mismatch。** 杂注指令 detect_mismatch 能够在文件中放置一个要与其他具有相同名称的标记进行比较的标记。 如果同一个名称存在多个值，则链接器将发出错误。

**XOP 内部函数、FMA4 内部函数和 LWP 内部函数。** 添加了新的内部函数来支持针对 Visual Studio 2010 SP1 添加的 XOP 内部函数、针对 Visual Studio 2010 SP1 添加的 FMA4 内部函数和针对 Visual Studio 2010 SP1 处理器技术添加的 LWP 内部函数。 使用 __cpuid、__cpuidex 可确定特定计算机上支持的处理器技术。

### <a name="visual-studio-c-projects-and-the-build-system"></a>Visual Studio C++ 项目和生成系统

**MSBuild.** Visual C++ 解决方案和项目现在使用 MSBuild.exe（取代了 VCBuild.exe）生成。 MSBuild 同样是基于 XML 的、灵活的、可扩展的生成工具，可由其他 Visual Studio 语言和项目类型使用。 由于此更改，Visual Studio C++ 项目文件现在使用 XML 文件格式并具有文件扩展名 .vcxproj。 Visual Studio 早期版本的 Visual Studio C++ 项目文件会自动转换为新文件格式。

**VC + + 目录。** VC++ 目录设置现在位于两个位置。 使用项目属性页可为 VC++ 目录设置每个项目的值。 使用 **属性管理器** 和属性表为 VC + + 目录设置每个配置的全局值。

**项目到项目依赖项。** 在早期版本中，项目之间的定义依赖项存储在解决方案文件中。 在这些解决方案转换为新项目文件格式时，依赖项会转换为项目到项目引用。 因为解决方案依赖项的概念与项目到项目引用的概念不同，所以此更改会影响应用程序。

**宏和环境变量。** 新的 _ITERATOR_DEBUG_LEVEL 宏将调用对迭代器的调试支持。 使用此新宏取代旧的 _SECURE_SCL 和 _HAS_ITERATOR_DEBUGGING 宏。

### <a name="visual-c-libraries"></a>Visual C++ 库

**并发运行库。** 并发运行时框架支持同时运行的应用程序和组件，并且它是在 Visual C++ 中进行并发应用程序编程的框架。 为了支持并发应用程序编程，并行模式库 (PPL) 提供了用于执行细粒度并行操作的通用容器和算法。 异步代理库提供了用于粗粒度数据流和管道任务的基于角色的编程模型和消息传递接口。

**标准 C++ 库。** 下表描述了对标准 C++ 库进行的很多更改。

- 使用了新的 rvalue 引用 C++ 语言功能来为标准模板库中的很多函数实现移动语义和完美转移。 移动语义和完美转移极大地提高了分配或指派变量或参数操作的性能。
- 还使用了 rvalue 引用来实现新的 `unique_ptr` 类，此类是一个比 `auto_ptr` 类更安全的智能指针类型。 `unique_ptr` 类可移动但不可复制，在不影响安全的情况下实现严格的所有权语义，并且可以很好地与可识别 rvalue 引用的容器结合使用。 `auto_ptr` 类已弃用。
- 十五个新函数，例如、 `find_if_not` `copy_if` 和，已 `is_sorted` 添加到 \<algorithm> 标头中。
- 在 \<memory> 标头中，新的 make_shared 函数是一种方便、可靠且高效的方法，可在构造对象的同时，为对象创建共享指针。
- 标头支持单向链接列表 \<forward_list> 。
- 新的 `cbegin`、`cend`、`crbegin` 和 `crend` 成员函数提供可在整个容器中前后移动的 `const_iterator`。
- \<system_error>标头和相关模板支持处理低级别系统错误。 `exception_ptr` 类的成员可用于在线程之间传输异常。
- \<codecvt>标头支持将 Unicode 字符的各种编码转换为其他编码。
- \<allocators>标头定义了多个模板，可帮助为基于节点的容器分配和释放内存块。
- 标头有很多更新 \<random> 。

### <a name="microsoft-foundation-class-mfc-library"></a>Microsoft 基础类 (MFC) 库

**Windows 7 功能。** MFC 支持 Windows 7 的很多功能，例如，功能区用户界面 (UI)、任务栏、跳转列表、标签式缩略图、缩略图预览、进度栏、图标覆盖和搜索索引。 由于 MFC 自动支持 Windows 7 的很多功能，因此可能不必修改现有应用程序。 若要支持新应用程序中的其他功能，可使用“MFC 应用程序向导”指定要使用的功能。

**多点触控感知。** MFC 支持具有多点触控用户界面的应用程序，例如，针对 Microsoft Surface 操作系统编写的应用程序。 多点触控应用程序可处理 Windows 触控消息和手势消息，也就是触控消息的组合。 为应用程序注册触控事件和手势事件之后，操作系统就会将多点触控事件路由至事件处理程序。

**高 DPI 识别。** 默认情况下，MFC 应用程序现在可以识别高 DPI。 如果应用程序可以识别高 DPI（每英寸像素数），则操作系统可将窗口、文本和其他 UI 元素缩放至当前屏幕分辨率。 这意味着，缩放的图像更可能是经过正确布局的，而不是经过剪切或像素化的。

**重启管理器。** 重新启动管理器会在应用程序意外关闭或重新启动时，自动保存文档并重新启动该应用程序。 例如，在自动更新关闭了某个应用程序后，可以使用重新启动管理器来启动该应用程序。 有关如何配置应用程序以使用重新启动管理器的详细信息，请参阅 **如何：添加重新启动管理器支持**。

**CTaskDialog.** `CTaskDialog` 类可用来替代标准的 `AfxMessageBox` 消息框。 `CTaskDialog` 类显示和收集的信息与标准消息框的多。

#### <a name="safeint-library"></a>SafeInt 库

新的 SafeInt 库执行安全的算数运算，可防止整数溢出。 该库还会比较不同类型的整数。

#### <a name="new-active-template-library-atl-macros"></a>新的活动模板库 (ATL) 宏

向 ATL 中添加了新的宏来扩展 PROP_ENTRY_TYPE 和 PROP_ENTRY_TYPE_EX 的功能。 PROP_ENTRY_INTERFACE 和 PROP_ENTRY_INTERFACE_EX 可用于添加有效的 CLSID 列表。 PROP_ENTRY_INTERFACE_CALLBACK 和 PROP_ENTRY_INTERFACE_CALLBACK_EX 可用于指定一个回调函数来确定 CLSID 是否有效。

#### <a name="analyze-warnings"></a>/analyze 警告

大多数 **`/analyze`** (企业代码分析) 警告已从 C Run-Time (CRT) 、MFC 和 ATL 库中删除。

#### <a name="animation-and-d2d-support"></a>动画和 D2D 支持

MFC 现在支持动画和 Direct2D 图形。 MFC 库中新增了一些 MFC 类和函数来支持此功能。 此外还新增了两个演练来分别演示如何将 D2D 对象和动画对象添加到项目中。 这些演练是 **演练：向 Mfc 项目添加 D2D 对象** 和 **演练：向 Mfc 项目添加动画**。

### <a name="ide"></a>IDE

**改进的 IntelliSense。** 对用于 Visual C++ 的 IntelliSense 进行了完全重新设计，以便能够更快、更准确并且有能力处理更大的项目。 为了完成此次改进，IDE 对开发人员查看和修改源代码的方式与 IDE 使用源代码和项目设置生成解决方案的方式进行了区分。
由于这种责任分离，因此，浏览功能（如 **类视图** 和新的 " **导航到** " 对话框）由基于新的 SQL Server 桌面数据库的系统处理 ( .sdf) 文件，该文件替换旧的 "不编译" 浏览 ( 文件。 IntelliSense 功能（例如，快速信息、自动完成和参数帮助）仅在需要时才会分析转换单元。 混合功能（如新的 " **调用层次结构** " 窗口）结合使用浏览功能和 IntelliSense 功能。
由于 IntelliSense 仅处理你目前需要的信息，因此 IDE 的响应速度更快。 另外，由于信息更新，IDE 视图和窗口更准确了。 最后，由于 IDE 基础结构的组织更合理、处理能力更强并且可伸缩性更大，因此可以处理更大的项目。

**改进的 IntelliSense 错误。** IDE 可更好地检测可能会损失 IntelliSense 的错误，并在错误下显示红色波浪下划线。 此外，IDE 还向 " **错误列表" 窗口** 报告 IntelliSense 错误。 若要显示导致问题的代码，请双击 " **错误列表" 窗口** 中的错误。

**#include 自动完成功能。** IDE 支持 `#include` 关键字自动完成。 在键入 `#include` 时，IDE 会创建一个包含有效头文件的下拉列表框。 如果继续键入文件名，则 IDE 会基于输入内容筛选该列表。 可以随时从该列表中选择要包含的文件。 这样可快速地包含文件，而无需知道确切的文件名。

**导航到。** " **导航到** " 对话框允许您在项目中搜索与指定字符串匹配的所有符号和文件。 随着你在搜索字符串中键入其他字符，搜索结果会即时进行修改。 " **结果** 反馈" 字段告诉您找到的项数，并帮助您决定是否限制搜索。 " **类型/范围**"、" **位置**" 和 " **预览** " 反馈字段可帮助您消除具有相似名称的项的歧义。 此外，你可以扩展此功能以支持其他编程语言。

**并行调试和分析。** Visual Studio 调试器可识别并发运行时，帮助排除并行处理应用程序故障。 可使用新的并发探查器工具直观地显示应用程序的总体行为。 此外，可使用新的工具窗口直观地显示任务及其调用堆栈的状态。

**功能区设计器。** **功能区设计器** 是一个图形编辑器，可用于创建和修改 MFC 功能区 UI。 最终的功能区 UI 由基于 XML 的资源文件 (.mfcribbon-ms) 表示。 对于现有应用程序，你可以通过暂时添加几行代码，然后调用 **功能区设计器** 来捕获当前功能区 UI。 创建功能区资源文件之后，可以使用一些用于加载功能区资源的语句来替换手写的功能区 UI 代码。

**调用层次结构。** 通过 " **调用层次结构** " 窗口，您可以导航到特定函数调用的所有函数，或导航到调用特定函数的所有函数。

### <a name="tools"></a>工具

**MFC 类向导。** Visual C++ 2010 恢复了备受好评的 MFC 类向导工具。 通过使用 MFC 类向导，可以很方便地向项目中添加类、消息和变量，而不必手动修改源文件集。

**ATL 控件向导。** ATL 控件向导不再自动填充 `ProgID` 字段。 如果某个 ATL 控件没有 `ProgID`，则其他工具可能无法使用该控件。 例如，“插入活动控件”对话框需要控件具有 `ProgID`。 有关该对话框的详细信息，请参阅 [插入 ActiveX 控件](../windows/adding-editing-or-deleting-controls.md#insert-activex-controls)。

### <a name="microsoft-macro-assembler-reference"></a>Microsoft 宏汇编程序参考

新增的 YMMWORD 数据类型支持 Intel 高级矢量扩展 (AVX) 指令中包含的 256 位多媒体操作数。

## <a name="whats-new-for-c-in-visual-studio-2008"></a>Visual Studio 2008 中 C++ 的新增功能

### <a name="visual-c-integrated-development-environment-ide"></a>Visual C++ 集成开发环境 (IDE)

- 在 ATL、MFC 和 Win32 应用程序中创建的对话框现在符合 Windows Vista 样式指南。 使用 Visual Studio 2008 创建新项目时，插入应用程序中的所有对话框将符合 Windows Vista 样式指南。 如果重新编译使用早期版本的 Visual Studio 创建的项目，所有现有对话框将保持以前的外观。 有关如何向应用程序插入对话框的详细信息，请参阅 **对话框编辑器**。

- **ATL 项目** 向导现在提供了一个选项，用于为所有用户注册组件。 从 Visual Studio 2008 开始， **ATL 项目** 向导创建的 COM 组件和类型库在注册表的 HKEY_CURRENT_USER 节点中注册，除非你选择 **了 "为所有用户注册组件**"。
- **ATL 项目** 向导不再提供用于创建特性化 ATL 项目的选项。 从 Visual Studio 2008 开始， **ATL 项目** 向导没有用于更改新项目的属性化状态的选项。 该向导创建的所有新 ATL 项目现在都未经属性化。
- 写入注册表时可以进行重定向。 随着 Windows Vista 的引入，写入注册表的特定区域需要在提升模式下运行程序。 但不需要始终在提升模式下运行 Visual Studio。 无需任何编程更改，按用户重定向可以自动将注册表写入操作从 HKEY_CLASSES_ROOT 重定向到 HKEY_CURRENT_USER。
- **类设计器** 现在为本机 c + + 代码提供有限的支持。 在 Visual Studio 的早期版本中， **类设计器** 仅适用于 Visual c # 和 Visual Basic。 现在，c + + 用户可以使用 **类设计器**，但只能在只读模式下使用。 有关如何在 c + + 中使用 **类设计器** 的详细信息，请参阅 **类设计器中的使用 Visual C++ 代码**。
- 项目向导不再提供用于创建 C++ SQL Server 项目的选项。 从 Visual Studio 2008 开始，新建项目向导没有创建 C++ SQL Server 项目的选项。 使用 Visual Studio 早期版本创建的 SQL Server 项目仍将编译并正常运行。

### <a name="visual-c-libraries"></a>Visual C++ 库

#### <a name="general"></a>常规

- 可将应用程序绑定到特定版本的 Visual C++ 库。 有时应用程序依赖于版本发布后对 Visual C++ 库所做的更新。 在这种情况下，在具有早期版本库的计算机上运行应用程序可能会导致意外行为。 现在可以将应用程序绑定到特定版本的库，使应用程序不会在具有早期版本库的计算机上运行。

#### <a name="stlclr-library"></a>STL/CLR 库

- Visual C++ 现在包括了 STL/CLR 库。 STL/CLR 库由标准模板库 (STL)（标准 C++ 库的一个子集）打包而成，与 C++ 和 .NET Framework 公共语言运行时 (CLR) 一起使用。 通过 STL/CLR，现在可以在托管环境中使用 STL 的所有容器、迭代器和算法。

#### <a name="mfc-library"></a>MFC 库

- Windows Vista 支持公共控件。 已经添加了 18 个新类或现有类中的 150 多个方法，用于支持 Windows Vista 中的功能，或改进当前 MFC 类中的功能。
- 新的 `CNetAddressCtrl` 类让你能够输入和验证 IPv4 和 IPv6 地址或 DNS 名称。
- 新的 `CPagerCtrl` 类简化了 Windows 页导航控件的使用。
- 新的 `CSplitButton` 类让你能更轻松地使用 Windows 拆分按钮控件来选择默认或可选操作。

#### <a name="c-support-library"></a>C++ 支持库

- C++ 引入了封送处理库。 封送处理库为在本机和托管环境之间封送数据提供了一个易于使用的优化方法。 封送处理库可以替代其他更复杂、效率更低的方法（例如使用 PInvoke）。 有关详细信息，请参阅 **c + + 中的封送处理概述** 。

#### <a name="atl-server"></a>ATL 服务器

- ATL 服务器作为共享源项目发布。
- 大多数 ATL 服务器基本代码已作为共享源项目发布在 CodePlex 上，而不是作为 Visual Studio 2008 的一部分进行安装。 与 ATL 服务器相关联的多个文件不再属于 Visual Studio。 有关删除的文件的列表，请参阅 **删除的 ATL Server 文件**。
- 来自 atlenc.h 的数据编码和解码类以及来自 atlutil.h 和 atlpath.h 的实用工具函数和类现在都属于 ATL 库。
- Microsoft 将继续支持早期版本的 Visual Studio 中包含的 ATL 服务器版本，只要这些版本的 Visual Studio 受到支持。 CodePlex 将继续以社区项目的形式开发 ATL 服务器代码。 Microsoft 不支持 CodePlex 版本的 ATL 服务器。

### <a name="visual-c-compiler-and-linker"></a>Visual C++ 编译器和链接器

#### <a name="compiler-changes"></a>编译器的更改

- 编译器支持托管增量生成。 如果指定此选项，则编译器在引用程序集更改时不会重新编译代码。 它将执行增量生成。 仅当更改影响依赖代码时才会重新编译文件。
- 不再支持与 ATL 服务器相关的属性。 编译器不再支持曾与 ATL 服务器直接相关的一些属性。 有关已删除属性的完整列表，请参阅“重大更改”。
- 编译器支持 Intel 内核微架构。 编译器包含在代码生成过程中对 Intel 内核微架构进行的优化。 该优化默认启用，并且不能禁用，因为它也用于 Pentium 4 和其他处理器。
- 内部函数支持更新的 AMD 和 Intel 处理器。 多条新的内部函数指令在新型 AMD 和 Intel 处理器中提供更强大的功能。 有关新的内部函数的详细信息，请参阅 **补充流式处理 Simd 扩展3说明**、 **流式处理 Simd 扩展4说明**、 **SSE4A 和高级位操作内部函数**、 **AES 内部函数**、 **_mm_clmulepi64_si128** 和 **__rdtscp**。
- `__cpuid` 函数已更新。 `__cpuid` 和 `__cpuidex` 函数现支持最新修订版的 AMD 和 Intel 处理器的多项新功能。 `__cpuidex` 内部函数是新增的，它能从最新处理器收集更多信息。
- `/MP` 编译器选项缩短了总生成时间。 `/MP` 选项通过创建多个同时编译文件的进程，显著减少了编译多个源文件需要的总时间。 此选项在支持超线程、多处理器或多内核的计算机上尤其有用。
- `/Wp64` **`__w64`** 不推荐使用编译器选项和关键字。 `/Wp64`编译器选项和 **`__w64`** 关键字（检测64位可移植性问题）已弃用，并将在编译器的未来版本中删除。 请改用面向 64 位平台的 MSVC 来代替此编译器选项和关键字。
- `/Qfast_transcendentals` 生成先验函数的内联代码。
- `/Qimprecise_fwaits` 在使用 `/fp:except` 编译器选项时删除 try 块内部的 fwait 命令。

### <a name="linker-changes"></a>链接器的更改

- 用户帐户控制信息现在由 Visual C++ 链接器 (link.exe) 嵌入到可执行文件的清单文件中。 此功能默认启用。 要详细了解如何禁用此功能或如何修改默认行为，请参阅 `/MANIFESTUAC`（将 UAC 信息嵌入到清单中）。
- 链接器现具有 `/DYNAMICBASE` 选项，它用于实现 Windows Vista 的地址空间布局随机化功能。 此选项修改可执行文件标头，以指示是否应在加载时对应用程序进行随机变基。

## <a name="whats-new-for-c-in-visual-studio-2005"></a>Visual Studio 2005 中 C++ 的新增功能

Visual C++ 2005 Service Pack 1 中新增了以下功能：

### <a name="intrinsics-for-x86-and-x64"></a>面向 x86 和 x64 的内部函数

- __halt
- __lidt
- __nop
- __readcr8
- __sidt
- __svm_clgi
- __svm_invlpga
- __svm_skinit
- __svm_stgi
- __svm_vmload
- __svm_vmrun
- __svm_vmsave
- __ud2
- __vmx_off
- __vmx_vmptrst
- __writecr8

### <a name="intrinsics-for-x64-only"></a>仅限 x64 的内部函数

- __vmx_on
- __vmx_vmclear
- __vmx_vmlaunch
- __vmx_vmptrld
- __vmx_vmread
- __vmx_vmresume
- __vmx_vmwrite

### <a name="new-language-keywords"></a>新语言关键字

__sptr、__uptr

### <a name="new-compiler-features"></a>编译器的新增功能

此版本对编译器进行了重大更改。

- 64 位本机和跨平台编译器。
- 添加了 `/analyze`（企业代码分析）编译器选项。
- 添加了 `/bigobj` 编译器选项。
- 添加了 `/clr:pure`、`/clr:safe` 和 `/clr:oldSyntax`。 （后来已在 Visual Studio 2015 中弃用，并已从 Visual Studio 2017 中删除。）
- 弃用的编译器选项：此版本中已弃用许多编译器选项;有关详细信息，请参阅已 **弃用的编译器选项** 。
- 减少了 `/clr` 代码中的双重形式转换；详情请参阅“双重形式转换 (C++)”。
- `/EH`（异常处理模型）或 `/EHs` 无法再用于捕捉非引发性异常；请使用 `/EHa`。
- 添加了 `/errorReport`（报告内部编译器错误）编译器选项。
- 添加了 `/favor`（64 位优化）编译器选项。
- 添加了 `/FA`、`/Fa`（列表文件）编译器选项。
- 添加了 `/FC`（所诊断源代码文件的完整路径）编译器选项。
- 添加了 `/fp`（指定浮点行为）编译器选项。
- 添加了 `/G`（处理器优化）选项编译器选项。
- 添加了 `/G`（处理器优化）选项编译器选项。
- 删除了 `/G3`、`/G4`、`/G5`、`/G6`、`/G7` 和 `/GB` 编译器选项。 该编译器现在使用“混合模式”，会尝试为所有体系结构创建最佳输出文件。
- `/Gf` 已删除。 请改用 `/GF`（消除重复的字符串）。
- `/GL`（全程序优化）现与 `/CLRHEADER` 兼容。
- `/GR` 现默认为打开状态。
- `/GS`（缓冲区安全检查）现可对容易受到攻击的指针参数提供安全保护。 `/GS` 现默认为打开状态。 `/GS` 现在也可用于通过 `/clr`（公共语言运行时编译）编译为 MSIL 的函数。
- 添加了 `/homeparams`（将寄存器参数复制到堆栈）编译器选项。
- 添加了 `/hotpatch`（创建可热修补的映像）编译器选项。
- 已更新内联函数试探法;**`inline`** 有关详细信息，请参阅、 **`__inline`** **`__forceinline`** 和 **inline_depth**
- 添加了很多内部函数，而且很多以前未记录的内部函数现在都已记录。
- 默认情况下，任何对 new 的调用失败都将引发异常。
- 删除了 `/ML` 和 `/MLd` 编译器选项。 Visual C++ 不再支持单线程静态链接的 CRT 库支持。
- 编译器实现了命名返回值优化。当你使用 `/O1`、`/O2`（最小化大小、最大化速度）、`/Og`（全局优化）和 `/Ox`（完全优化）进行编译时，会启用此优化。
- 删除了 `/Oa` 编译器选项，它将被忽略且没有提示；请使用 `noalias` 或 `restrict__declspec` 修饰符来指定编译器命名别名的方式。
- 删除了 `/Op` 编译器选项。 请改用 `/fp`（指定浮点行为）。
- Visual C++ 现在支持 OpenMP。
- 添加了 `/openmp`（启用 OpenMP 2.0 支持）编译器选项。
- 删除了 `/Ow` 编译器选项，它将被忽略且没有提示。 请使用 `noalias` 或 `restrict__declspec` 修饰符来指定编译器命名别名的方式。

### <a name="profile-guided-optimizations"></a>按配置文件优化

- `/QI0f` 已删除。
- `/QIfdiv` 已删除。
- 添加了 `/QIPF_B`（B CPU 单步执行的勘误表）编译器选项。
- 添加了 `/QIPF_C`（C CPU 单步执行的勘误表）编译器选项。
- 添加了 `/QIPF_fr32`（不使用高 96 浮点寄存器）编译器选项。
- 添加了 `/QIPF_noPIC`（生成依赖于位置的代码）编译器选项。
- 添加了 `/QIPF_restrict_plabels`（假定运行时不创建任何函数）编译器选项。

### <a name="unicode-support-in-the-compiler-and-linker"></a>编译器和链接器中的 Unicode 支持

- `/vd`（禁用构造置换）现在允许你对正在构造（使用 /vd2）的对象使用 dynamic_cast 运算符
- 删除了 `/YX` 编译器选项。 请改用 `/Yc`（创建预编译头文件） 或 `/Yu`（使用预编译头文件）。 如果从生成配置中删除 `/YX`，且不提供替换项，则可能会加快生成速度。
- `/Zc:forScope` 现默认为打开状态。
- `/Zc:wchar_t` 现默认为打开状态。
- 删除了 `/Zd` 编译器选项。 不再支持仅限行号的调试信息。 请改用 `/Zi`（详情请参见 /Z7、/Zi、/ZI（调试信息格式））。
- `/Zg` 现仅对 C 源代码文件有效，对 C++ 源代码文件无效。
- 添加了 `/Zx`（调试经过优化的 Itanium 代码）编译器选项。

### <a name="new-language-features"></a>新语言功能

- Attributeattribute 现已弃用。
- 添加了 `appdomain__declspec` 修饰符。
- 添加了 `__clrcall` 调用约定。
- 借助于已弃用的 (C++) declspec 修饰符，用户可以在试图访问已弃用的类或函数时，指定将在编译时显示的字符串。
- **`dynamic_cast`** 运算符有重大更改。
- 本机枚举现在允许指定基础类型。
- 添加了 `jitintrinsicdeclspec` 修饰符。
- 添加了 `noaliasdeclspec` 修饰符。
- 添加了 `process__declspec` 修饰符。
- **abstract**、 **override** 和 **sealed** 对于本机编译有效。
- **`__restrict`** 已添加关键字。
- 添加了 `restrictdeclspec` 修饰符。
- **`__thiscall`** 现在为关键字。
- **`__unaligned`** 关键字现在已记录。
- **`volatile`** (c + +) 具有对优化的更新行为。

### <a name="new-preprocessor-features"></a>预处理器的新增功能

- 添加了预定义宏 __CLR_VER。
- 注释 (C/C++) 杂注现在接受 `/MANIFESTDEPENDENCY` 作为链接器注释。 注释的 exestr 选项现已弃用。
- `embedded_idl` 属性（`#import` 指令）现具有可选参数。
- `fenv_access` 杂注
- `float_control` 杂注
- `fp_contract` 杂注
- 如果杂注的托管和非托管部分中存在全局变量，则不会按照声明全局变量的顺序初始化全局变量。 这是一项潜在的重大更改，例如，如果使用托管的全局变量来初始化非托管全局变量，则需要一个构造完整的托管对象。
- 用 init_seg 指定的部分现为只读状态，而不是早期版本中的读/写。
- inline_depth 默认值现为 16。 在 Visual C++ .NET 2003 中，默认值 16 也有效。
- 添加了预定义宏 _INTEGRAL_MAX_BITS，请参阅“预定义宏”。
- 添加了预定义宏 _M_CEE、_M_CEE_PURE 和 _M_CEE_SAFE，请参阅“预定义宏”。
- 添加了预定义宏 _M_IX86_FP。
- 添加了预定义宏 _M_X64。
- `make_public` 杂注
- 更新了 `managed`、`unmanaged` 杂注语法（现具有 `push` 和 `pop`）
- `#using` 指令现在所有 `/clr` 编译中隐式引用 mscorlib.dll。
- 添加了预定义宏 _OPENMP。
- 更新了优化杂注，a 和 w 不再是有效参数。
- 添加了 no_registry#import 属性。
- 添加了 `region`、`endregion` 杂注
- 添加了预定义宏 _VC_NODEFAULTLIB。
- 现在实现了 Variadic 宏。
- `vtordisp` 已弃用，并将从 Visual C++ 的未来版本中移除。
- `warning` 杂注现在具有 suppress 说明符。

### <a name="new-linker-features"></a>链接器的新增功能

- 现在允许将模块（非程序集 MSIL 输出文件）作为链接器的输入内容。
- 添加了 `/ALLOWISOLATION`（清单查找）链接器选项。
- 更新了 `/ASSEMBLYRESOURCE`（嵌入托管资源），现可指定程序集中的资源名称，还可在程序集中将此资源指定为“私有”。
- 添加了 `/CLRIMAGETYPE`（指定 CLR 映像的类型）链接器选项。
- 添加了 `/CLRSUPPORTLASTERROR`（为 PInvoke 调用保留上次的错误代码）链接器选项。
- 添加了 `/CLRTHREADATTRIBUTE`（设置 CLR 线程属性）链接器选项。
- 添加了 `/CLRUNMANAGEDCODECHECK`（添加 SuppressUnmanagedCodeSecurityAttribute）链接器选项。
- 添加了 `/ERRORREPORT`（报告内部链接器错误）链接器选项。
- 删除了 `/EXETYPE` 链接器选项。 链接器不再支持创建 Windows 95 和 Windows 98 设备驱动程序。 使用适当的 DDK 来创建这些设备驱动程序。 EXETYPE 关键字不再对模块定义文件有效。
- 添加了 `/FUNCTIONPADMIN`（创建可热修补的映像）链接器选项。
- 使用 `/clr` 编译的模块现支持 `/LTCG` 链接器选项。 还更新了 `/LTCG`，使其支持按配置优化。
- 添加了 `/MANIFEST`（创建并行程序集清单）链接器选项。
- 添加了 `/MANIFESTDEPENDENCY`（指定清单依赖项）连接器选项。
- 添加了 `/MANIFESTFILE`（命名清单文件）链接器选项。
- 删除了 `/MAPINFO:LINES` 链接器选项。
- 添加了 `/NXCOMPAT`（与数据执行保护兼容）链接器选项。
- 添加了 `/PGD`（为按配置文件优化指定数据库）链接器选项。
- 添加了 `/PROFILE`（性能工具分析器）链接器选项。
- `/SECTION`（指定节属性）链接器选项现支持属性求反，而不再支持 L 或 D（与 VxD 相关）属性。
- 编译器和链接器中的 Unicode 支持
- `/VERBOSE`（打印进度消息）链接器选项现还接受 ICF 和 REF。
- 删除了 `/VXD` 链接器选项。 链接器不再支持创建 Windows 95 和 Windows 98 设备驱动程序。 使用适当的 DDK 来创建这些设备驱动程序。 VXD 关键字不再对模块定义文件有效。
- 删除了 `/WS` 链接器选项。 `/WS` 用于修改针对 Windows NT 4.0 的映像。 可使用 IMAGECFG.exe -R 文件名代替 `/WS`。 可在 Windows NT 4.0 CD-ROM 上找到 IMAGECFG.exe，路径为 SUPPORT\DEBUG\I386\IMAGECFG.EXE。
- `/WX`（将链接器警告视为错误）链接器选项现已记录。

### <a name="new-linker-utility-features"></a>链接器实用工具的新增功能

- 添加了 `/ALLOWISOLATION` editbin 选项
- 删除了 DESCRIPTION 模块定义文件语句。 链接器不再支持生成虚拟设备驱动程序。
- 在 bscmake.exe、dumpbin.exe、editbin.exe 和 lib.exe 中添加了 `/ERRORREPORT` 选项。
- 添加了 `/LTCG` lib 选项。
- 添加了 `/NXCOMPAT` editbin 选项。
- 添加了 `/RANGE` dumpbin 选项。
- 添加了 `/TLS` dumpbin 选项。
- 删除了 `/WS` editbin 选项。 `/WS` 用于修改针对 Windows NT 4.0 的映像。 可使用 IMAGECFG.exe -R 文件名代替 `/WS`。 可在 Windows NT 4.0 CD-ROM 上找到 IMAGECFG.exe，路径为 SUPPORT\DEBUG\I386\IMAGECFG.EXE。
- 添加了 /WX[:NO] lib 选项。

### <a name="new-nmake-features"></a>NMAKE 的新增功能

- 添加了 `/ERRORREPORT`。
- 添加了 `/G`。
- 更新了预定义规则。
- 递归宏中记录的 $(MAKE) 宏现在提供 nmake.exe 的完整路径。

### <a name="new-masm-features"></a>MASM 的新增功能

- MASM 表达式现在为 64 位值。 在早期版本中，MASM 表达式为 32 位值。
- 现在指令 __asm int 3 可使函数被编译到本机中。
- ALIAS (MASM) 现已记录。
- 添加了 `/ERRORREPORT` ml.exe 和 ml64.exe 选项。
- .FPO 现已记录。
- Visual C++ 2005 中将不提供 H2INC.exe。 如果需要继续使用 H2INC，请使用 Visual C++ 早期版本中的 H2INC.exe。
- 添加了运算符 IMAGEREL。
- 添加了运算符 HIGH32。
- 添加了运算符 LOW32。
- ml64.exe 是 x64 体系结构的 MASM 的一个版本。 它将 x64 .asm 文件组装到 x64 对象文件中。 x64 编译器中不支持内联程序集语言。 为 ml64.exe (x64) 添加了以下 MASM 指令：
- .ALLOCSTACK
- .ENDPROLOG
- .PUSHFRAME
- .PUSHREG
- .SAVEREG
- .SAVEXMM128
- .SETFRAME。此外，已用仅适用于 x64 的语法更新了 PROC 指令。
- 添加了 MMWORD 指令
- `/omf`（ML.exe 命令行选项）现表示 `/c`。 ML.exe 不支持链接 OMF 格式对象。
- SEGMENT 指令现支持附加属性。
- 添加了运算符 SECTIONREL。
- 添加了 XMMWORD 指令

### <a name="new-crt-features"></a>CRT 的新增功能

- 添加了一些函数的安全版本。 这些函数以更好的方式处理错误，并强制对缓冲区实施更严格的控制以避免常见的安全漏洞。 新的安全版本由 **_s** 后缀标识。
- 已弃用许多函数现有的安全性较低的版本。 若要禁用弃用警告，请定义 _CRT_SECURE_NO_WARNINGS。
- 许多现有函数现在会验证其参数，并在传递了无效参数时调用无效参数处理程序。
- 许多现有函数现在之前未设置 `errno` 的位置对其进行了设置。
- 添加了具有整数类型的 typedef `errno_t`。 每当函数返回类型或参数处理来自 `errno` 的错误代码时，就会使用 `errno_t`。 `errno_t` 替换了 `errcode`。
- 依赖于区域设置的函数现在有了新的版本，这些版本将区域设置作为参数，而不是使用当前的区域设置。 这些新函数具有 **_l** 后缀。 添加了一些新函数，用于区域设置对象。 新函数包括 `_get_current_locale`、`_create_locale` 和 `_free_locale`。
- 添加了新函数，以支持锁定和解锁文件句柄。
- `_spawn` 系列函数不会像在早期版本中那样，在成功时将 errno 重置为零。
- 提供了可用于指定参数使用顺序的 `printf` 系列函数版本。
- 现已支持 Unicode 文本格式。 函数 `_open` 支持 _O_TEXTW、_O_UTF8 和 _O_UTF16 属性。 `fopen` 函数支持指定 Unicode 格式的“ccs=ENCODING”方法。
- 现提供以托管代码 (MSIL) 生成的 CRT 库的新版本，可在用 `/clr`（公共语言运行时编译）选项进行编译时使用。
- _fileinfo 已被移除。
- `time_t` 的默认大小现为 64 位，可将 `time_t` 和一些时间函数的范围扩展到 3000 年。
- CRT 现在支持按每个线程进行区域设置。 添加了 `_configthreadlocale` 函数，使其支持此功能。
- 添加了 `_statusfp2` 和 `__control87_2` 函数，让你能够对 x87 和 SSE2 浮点处理器上的浮点控制字进行访问和控制。
- 添加了 `_mkgmtime` 和 `_mkgmtime64` 函数，以支持将时间 (struct tm) 转换为格林威治标准时间 (GMT)。
- 更改了 `swprintf` 和 `vswprintf`，使其更好地符合标准。
- 新的头文件 INTRIN.H 提供了某些内部函数的原型。
- `fopen` 函数现在具有一个 N 属性。
- `_open` 函数现在具有一个 _O_NOINHERIT 属性。
- `atoi` 函数现会在溢出时返回 INT_MAX 并将 `errno` 设置为 ERANGE。 早期版本中未定义溢出行为。
- `printf` 系列函数支持根据 ANSI C99 标准使用格式类型说明符 %a 和 %A 实现的十六进制浮点输出。
- `printf` 系列现在支持“ll”(long long) 大小前缀。
- 优化了 `_controlfp` 函数，以提供更好的性能。
- 添加了某些函数的调试版本。
- 添加了 `_chgsignl` 和 `_cpysignl`（长双精度版本）。
- 将 `_locale_t` 类型添加到了类型表。
- 添加了新的 `_countof` 宏，用于计算数组中的元素数量。
- 在每个函数主题中，添加了一节有关 .NET Framework 等效项的内容。
- 现在，有几个字符串函数可以选择在输出缓冲区过小时截断字符串，而不会失败;请参阅 **_TRUNCATE**。
- `_set_se_translator` 现需要使用`/EHa` 编译器选项。
- `fpos_t` 现在为 **`__int64`** `/Za` 适用于 c 代码)  (，并且在为 c + + 代码)  (手动设置了 __STDC__ 时。 它用于 **`struct`** 。
- _CRT_DISABLE_PERFCRIT_LOCKS 可以提高单线程程序的 I/O 性能。
- 在对符合 ISO C++ 名称的优选中，弃用了 POSIX 名称（例如使用 `_getch` 而不是 `getch`）。
- 新的链接选项 .obj 文件可用于纯模式
- `_recalloc` 将 `realloc` 和 `calloc` 功能组合在一起。

## <a name="whats-new-for-c-in-visual-studio-2003"></a>Visual Studio 2003 中 C++ 的新增功能

### <a name="compiler"></a>编译器

- 介绍如何在早期版本的运行时上运行用当前版本的编译器生成的 Managed Extensions for C++ 应用程序。
- Managed Extensions for C++ 的常见问题解答。
- 添加了一个演练，演示如何移植现有本机应用程序以使用 Managed Extensions for C++：“演练：移植现有本机 C++ 应用程序，使其能与 .NET Framework 组件进行交互”。
- 现在可以在值类型的方法上创建委托。
- Visual C++ .NET 2003 已极大地增强了编译器与 C++ 标准的符合性。
- 添加了 `/arch` 编译器选项。
- 已弃用 `/Gf`，并将从 Visual C++ 的下一版本中删除。
- 添加了 `/G7` 编译器选项。
- 改进了 `/GS` 编译器选项，使其帮助保护本地变量，防止直接缓冲区溢出。
- 删除了 `/noBool` 编译器选项。 编译器现在允许 **`bool`** 在 c + + 源代码文件中作为关键字 (而不是标识符) 出现。
- 现在，该 **`long long`** 类型可用作 **`typedef`** **`__int64`** CRT 中尚不支持的 **`long long`** 。
- `/Zm` 编译器选项现在指定预编译标头内存分配限制。
- _InterlockedCompareExchange 内部函数现已记录。
- _InterlockedDecrement 内部函数现已记录。
- _InterlockedExchange 内部函数现已记录。
- _InterlockedExchangeAdd 内部函数现已记录。
- _InterlockedIncrement 内部函数现已记录。
- 添加了 _ReadWriteBarrier 内部函数。

### <a name="attributes"></a>属性

- `implements` 属性现已记录。

### <a name="linker-features"></a>链接器的功能

添加了以下链接器开关：

- /ASSEMBLYDEBUG
- /ASSEMBLYLINKRESOURCE
- DELAYSIGN
- /KEYFILE
- /KEYCONTAINER
- /SAFESEH

### <a name="masm"></a>MASM

添加了 .SAFESEH 指令和 `/safeseh` ml.exe 选项。

## <a name="see-also"></a>请参阅

[Visual C++ 移植和升级指南](visual-cpp-porting-and-upgrading-guide.md)
