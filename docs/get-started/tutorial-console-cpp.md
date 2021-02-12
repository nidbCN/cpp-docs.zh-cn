---
title: 用 C++ 创建控制台计算器
description: 在 Visual C++ 中创建 Hello World 控制台应用和计算器应用
ms.custom: mvc
ms.date: 08/19/2019
ms.topic: tutorial
ms.devlang: cpp
ms.assetid: 45138d70-719d-42dc-90d7-1d0ca31a2f54
ms.openlocfilehash: 2a0f605d6b415e1b827bd2d983b087bc110eddd8
ms.sourcegitcommit: 77235bff6a7b2621c501938e30d93cb15f5733cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2021
ms.locfileid: "100254412"
---
# <a name="create-a-console-calculator-in-c"></a>用 C++ 创建控制台计算器

::: moniker range=">=msvc-160"

C++ 程序员通常从在命令行上运行的“Hello, world!” 应用程序开始。 在本文中，你将首先在 Visual Studio 中创建此内容，然后继续创建更具挑战性的对象：计算器应用。

## <a name="prerequisites"></a>先决条件

- 在 Visual Studio 中安装“使用 C++ 的桌面开发”工作负载并在计算机上运行  。 如果尚未安装，请参阅[在 Visual Studio 中安装 C++ 支持](../build/vscpp-step-0-installation.md)。

## <a name="create-your-app-project"></a>创建应用项目

Visual Studio 使用项目来组织应用的代码，使用解决方案来组织项目   。 项目包含用于生成应用的所有选项、配置和规则。 它还负责管理所有项目文件和任何外部文件间的关系。 要创建应用，需首先创建一个新项目和解决方案。

1. 如果刚刚启动 Visual Studio，则可看到“Visual Studio 2019”对话框。 选择“创建新项目”  以开始使用。

   ![Visual Studio 2019 初始对话框](./media/calc-vs2019-initial-dialog.png "Visual Studio 2019 初始对话框")

   否则，在 Visual Studio 中的菜单栏上，选择“文件” > “新建” > “项目”    。 “创建新项目”窗口随即打开  。

1. 在项目模板列表中，选择“控制台应用”，然后选择“下一步”   。

   ![“控制台应用”模板](./media/calc-vs2019-choose-console-app.png "选择“控制台应用”模板")

   > [!Important]
   > 请确保选择 Console App 模板的 C++  版本。 它具有 C++、Windows 和 Console 标记，该图标在角落处有“++”    。

1. 在“配置新项目”对话框中，选择“项目名称”编辑框，将新项目命名为 CalculatorTutorial，然后选择“创建”     。

   ![在“配置新项目”对话框中命名项目](./media/calc-vs2019-name-your-project.png "在“配置新项目”对话框中命名项目")

   将创建一个空的 C++ Windows 控制台应用程序。 控制台应用程序使用 Windows 控制台窗口显示输出并接受用户输入。 在 Visual Studio 中，将打开一个编辑器窗口并显示生成的代码：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include <iostream>

    int main()
    {
        std::cout << "Hello World!\n";
    }

    // Run program: Ctrl + F5 or Debug > Start Without Debugging menu
    // Debug program: F5 or Debug > Start Debugging menu

    // Tips for Getting Started:
    //   1. Use the Solution Explorer window to add/manage files
    //   2. Use the Team Explorer window to connect to source control
    //   3. Use the Output window to see build output and other messages
    //   4. Use the Error List window to view errors
    //   5. Go to Project > Add New Item to create new code files, or Project > Add Existing Item to add existing code files to the project
    //   6. In the future, to open this project again, go to File > Open > Project and select the .sln file
    ```

## <a name="verify-that-your-new-app-builds-and-runs"></a>验证新应用是否生成并运行

新的 Windows 控制台应用程序模板创建了一个简单的 C++“Hello World”应用。 此时，可以看到 Visual Studio 如何生成并运行直接从 IDE 创建的应用。

1. 若要生成项目，请从“生成”菜单选择“生成解决方案”   。 “输出”窗口将显示生成过程的结果  。

   ![“输出”窗口显示生成过程结果的 Visual Studio 2019 的屏幕截图。](./media/calc-vs2019-build-your-project.png "生成项目")

1. 若要运行代码，请在菜单栏上选择“调试”、“开始执行(不调试)”   。

   ![显示代码成功运行的 Visual Studio 2019 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calc-vs2019-hello-world-console.png "启动项目")

   随即将打开控制台窗口，然后运行你的应用。 在 Visual Studio 中启动控制台应用时，它会运行代码，然后输出“按任意键关闭此窗口。 。 。” 让你有机会看到输出。 祝贺你！ 你在 Visual Studio 中已创建首个“Hello, world!” 控制台应用！

1. 按任意键关闭该控制台窗口并返回到 Visual Studio。

现在即可使用你的工具在每次更改后生成并运行应用，以验证代码是否仍按预期运行。 如果未按预期运行，稍后，我们将向你演示调试方法。

## <a name="edit-the-code"></a>编辑代码

现在，将此模板中的代码转换为计算器应用。

1. 在“CalculatorTutorial.cpp”文件中，编辑代码以匹配此示例  ：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include <iostream>

    using namespace std;

    int main()
    {
        cout << "Calculator Console Application" << endl << endl;
        cout << "Please enter the operation to perform. Format: a+b | a-b | a*b | a/b"
            << endl;
        return 0;
    }

    // Run program: Ctrl + F5 or Debug > Start Without Debugging menu
    // Debug program: F5 or Debug > Start Debugging menu
    // Tips for Getting Started:
    //   1. Use the Solution Explorer window to add/manage files
    //   2. Use the Team Explorer window to connect to source control
    //   3. Use the Output window to see build output and other messages
    //   4. Use the Error List window to view errors
    //   5. Go to Project > Add New Item to create new code files, or Project > Add Existing Item to add existing code files to the project
    //   6. In the future, to open this project again, go to File > Open > Project and select the .sln file
    ```

   > 了解代码：
   >
   > - `#include` 语句允许引用位于其他文件中的代码。 有时，文件名使用尖括号 (\<\>) 包围；其他情况下，使用引号 (" ") 包围 。 通常，引用 C++ 标准库时使用尖括号，引用其他文件时使用引号。
   > - `using namespace std;` 行提示编译器期望在此文件中使用 C++ 标准库中的内容。 如果没有此行，库中的每个关键字都必须以 `std::` 开头，以表示其范围。 例如，如果没有该行，则对 `cout` 的每个引用都必须写为 `std::cout`。 添加 `using` 语句是为了让代码看起来更清晰。
   > - `cout` 关键字用于在 C++ 中打印到标准输出。 “\<\<”运算符提示编译器将其右侧的任何内容发送到标准输出  。
   > - “endl”关键字与 Enter 键类似；用于结束该行并将光标移动到下一行  。 如果要执行相同的操作，最好在字符串中使用 `\n`用 "" 包含），因为使用 `endl` 会始终刷新缓冲，进而可能影响程序的性能，但由于这是一个非常小的应用，所以改为使用 `endl` 以提高可读性。
   > - 所有 C++ 语句都必须以分号结尾，所有 C++ 应用程序都必须包含 `main()` 函数。 该函数是程序开始运行时运行的函数。 若要使用所有代码，必须可从 `main()` 访问所有代码。

1. 要保存文件，请输入“Ctrl+S”，或者选择 IDE 顶部附近的“保存”图标，即菜单栏下工具栏中的软盘图标   。

1. 要运行该应用程序，请按“Ctrl+F5”或转到“调试”菜单，然后选择“启动但不调试”    。 应会显示一个控制台窗口，其中显示代码中指定的文本。

1. 完成后，请关闭控制台窗口。

## <a name="add-code-to-do-some-math"></a>添加代码来执行一些数学运算

现在可添加一些数学逻辑。

### <a name="to-add-a-calculator-class"></a>添加 Calculator 类

1. 转到“项目”菜单，并选择“添加类”   。 在“类名”编辑框中，输入“Calculator”   。 选择 **“确定”** 。 这会向项目中添加两个新文件。 若要同时保存所有已更改的文件，请按“Ctrl+Shift+S”  。 这是“文件” > “全部保存”的键盘快捷方式   。 在“保存”按钮旁边还有一个用于“全部保存”的工具栏按钮，这是两个软盘的图标   。 一般来说，最好经常使用“全部保存”，这样你保存时就不会遗漏任何文件  。

   ![在“类名称”文本框中键入 Calculator 的“添加类”对话框的屏幕截图。](./media/calc-vs2019-create-calculator-class.png "创建 Calculator 类")

   类就像执行某事的对象的蓝图。 在本示例中，我们定义了 Calculator 以及它的工作原理。 上文使用的“添加类”向导创建了与该类同名的 .h 和 .cpp 文件  。 可以在 IDE 一侧的“解决方案资源管理器”窗口中看到项目文件的完整列表  。 如果该窗口不可见，则可从菜单栏中打开它：选择“查看” > “解决方案资源管理器”   。

   ![显示 Calculator Tutorial 项目的 Visual Studio 2019“解决方案资源管理器”窗口的屏幕截图。](./media/calc-vs2019-solution-explorer.png "“解决方案资源管理器”")

   现在编辑器中应打开了三个选项卡：CalculatorTutorial.cpp、Calculator.h 和 Calculator.cpp    。 如果你无意关闭了其中一个，可通过在“解决方案资源管理器”窗口中双击来重新打开它  。

1. 在“Calculator.h”中，删除生成的 `Calculator();` 和 `~Calculator();` 行，因为在此处不需要它们  。 接下来，添加以下代码行，以便文件现在如下所示：

    ```cpp
    #pragma once
    class Calculator
    {
    public:
        double Calculate(double x, char oper, double y);
    };
    ```

   > 了解代码
   >
   > - 所添加的行声明了一个名为 `Calculate` 的新函数，我们将使用它来运行加法、减法、乘法和除法的数学运算。
   > - C ++ 代码被组织成标头 (.h) 文件和源 (.cpp) 文件   。 所有类型的编译器都支持其他几个文件扩展名，但这些是要了解的主要文件扩展名。 函数和变量通常在头文件中进行声明（即在头文件中指定名称和类型）和实现（或在源文件中指定定义）   。 若要访问在另一个文件中定义的代码，可以使用 `#include "filename.h"`，其中“filename.h”是声明要使用的变量或函数的文件的名称。
   > - 已删除的两行为该类声明了“构造函数”和“析构函数”   。 对于像这样的简单类，编译器会为你创建它们，但本教程不涉及其用法。
   > - 最好根据代码的功能将代码组织到不同的文件中，方便稍后需要这些代码时能够轻易查找到。 在本例中，我们分别定义了 `Calculator` 类和包含 `main()` 函数的文件，但我们计划在 `main()` 中引用 `Calculator` 类。

1. 你会看到 `Calculate` 下显示绿色波浪线。 因为我们还没有在 .cpp 文件中定义 `Calculate` 函数。 将鼠标悬停在单词上，单击弹出的灯泡（在此示例中为螺丝刀），然后选择“在 Calculator.cpp 中创建 Calculate 定义”  。

   ![显示在 Calculator C P P 选项中突出显示“创建 Calculate 定义”的 Visual Studio 2019 的屏幕截图。](./media/calc-vs2019-create-definition.png "创建 Calculate 定义")

   随即将出现一个弹出窗口，可在其中查看在另一个文件中进行的代码更改。 该代码已添加到“Calculator.cpp”  。

   ![包含 Calculate 定义的弹出项](./media/calc-vs2019-pop-up-definition.png "包含 Calculate 定义的弹出项")

   目前，它只返回 0.0。 让我们来更改它。 按 Esc 关闭弹出窗口  。

1. 切换到编辑器窗口中的“Calculator.cpp”文件  。 删除 `Calculator()` 和 `~Calculator()` 部分（就像在 .h 文件中一样）并将以下代码添加到 `Calculate()`：

    ```cpp
    #include "Calculator.h"

    double Calculator::Calculate(double x, char oper, double y)
    {
        switch(oper)
        {
            case '+':
                return x + y;
            case '-':
                return x - y;
            case '*':
                return x * y;
            case '/':
                return x / y;
            default:
                return 0.0;
        }
    }
    ```

   > 了解代码
   >
   > - 函数 `Calculate` 使用数字、运算符和第二个数字，然后对数字执行请求的操作。
   > - Switch 语句检查提供了哪个运算符，并仅执行与该操作对应的情况。 “default: case”是一个回滚，以防用户键入一个不被接受的运算符，因此程序不会中断。 通常，最好以更简洁的方式处理无效的用户输入，但这超出了本教程的范围。
   > - `double` 关键字表示支持十进制的数字类型。 因此，Calculator 可以处理十进制数学和整数数学。 要始终返回这样的数字，必须有 `Calculate` 函数，因为代码的最开始处出现了 `double`（这表示函数的返回类型），正因为此我们即使在默认情况下也返回 0.0。
   > - .h 文件声明函数“原型”，它预先告诉编译器它需要什么参数，以及期望它返回什么样的返回类型  。 .cpp 文件包含该函数的所有实现细节。

如果此时再次生成并运行代码，则在询问要执行的操作后，它仍然会退出。 接下来，将修改 `main` 函数以进行一些计算。

### <a name="to-call-the-calculator-class-member-functions"></a>调用 Calculator 类的成员函数

1. 现在让我们更新“CalculatorTutorial.cpp”中的 `main` 函数  ：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include <iostream>
    #include "Calculator.h"

    using namespace std;

    int main()
    {
        double x = 0.0;
        double y = 0.0;
        double result = 0.0;
        char oper = '+';

        cout << "Calculator Console Application" << endl << endl;
        cout << "Please enter the operation to perform. Format: a+b | a-b | a*b | a/b"
             << endl;

        Calculator c;
        while (true)
        {
            cin >> x >> oper >> y;
            result = c.Calculate(x, oper, y);
            cout << "Result is: " << result << endl;
        }

        return 0;
    }
    ```

   > 了解代码
   >
   > - 由于 C++ 程序总是从 `main()` 函数开始，我们需要从这里调用其他代码，因此需要 `#include` 语句。
   > - 声明了一些初始变量 `x`、`y`、`oper` 和 `result`，分别用于存储第一个数字、第二个数字、运算符和最终结果。 提供一些初始变量始终是最佳做法，这样可避免未定义的行为，此示例即是如此。
   > - `Calculator c;` 行声明一个名为“c”的对象作为 `Calculator` 类的实例。 类本身只是计算器工作方式的蓝图；对象是进行数学运算的特定计算器。
   > - `while (true)` 语句是一个循环。 只要 `()` 内的条件成立，循环内的代码就会一遍又一遍地执行。 由于条件被直接列出为 `true`，因此它始终为 true，所以循环永远运行。 若要关闭程序，用户必须手动关闭控制台窗口。 否则，程序始终等待新输入。
   > - `cin` 关键字用于接受来自用户的输入。 假设用户输入符合所需规范，此输入流足够智能，可以处理在控制台窗口中输入的一行文本，并按顺序将其放入列出的每个变量中。 可以修改此行以接受不同类型的输入，例如，两个以上的数字，但还需要更新 `Calculate()` 函数来处理此问题。
   > - `c.Calculate(x, oper, y);` 表达式调用前面定义的 `Calculate` 函数，并提供输入的输入值。 然后该函数返回一个存储在 `result` 中的数字。
   > - 最后，将 `result` 输出到控制台，以便用户查看计算结果。

### <a name="build-and-test-the-code-again"></a>再次生成和测试代码

现在是时候再次测试程序以确保一切正常。

1. 按“Ctrl+F5”重建并启动应用  。

1. 输入 `5 + 5`，然后按 Enter  。 验证结果是否为 10。

   ![显示 5 + 5 的正确结果的 Visual Studio 2019 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calc-vs2019-five-plus-five.png "5 + 5 的结果")

## <a name="debug-the-app"></a>调试应用

由于用户可以自由地在控制台窗口中输入任何内容，因此请确保计算器按预期处理某些输入。 我们不是运行程序，而是调试程序，因此可以逐步检查程序所执行的每一项操作。

### <a name="to-run-the-app-in-the-debugger"></a>在调试器中运行应用

1. 在用户被要求输入之后，在 `result = c.Calculate(x, oper, y);` 行上设置断点。 若要设置断点，请在该行旁边编辑器窗口左边缘的灰色竖线上单击。 将显示一个红点。

   ![显示表示断点的红点的 Visual Studio 2019 的屏幕截图。](./media/calc-vs2019-set-breakpoint.png "设置断点")

   现在，当我们调试程序时，它总是暂停该行的执行。 我们已大概了解了该程序可用于简单案例。 由于我们不想每次暂停执行，因此可以设置断点条件。

1. 右键单击表示断点的红点，并选择“条件”  。 在条件的编辑框中，输入 `(y == 0) && (oper == '/')`。 完成后，选择“关闭”按钮  。 条件将自动保存。

   ![显示“断点设置”部分以及添加到“为 true”值的条件的 Visual Studio 2019 的屏幕截图。](./media/calc-vs2019-conditional-breakpoint.png "设置条件断点")

   现在，如果尝试被 0 除，我们将在断点处暂停执行。

1. 若要调试程序，请按 F5 或选择带绿色箭头图标的“本地 Windows 调试程序”工具栏按钮   。 在控制台应用中，如果输入类似“5 - 0”的内容，程序将正常运行并继续运行。 但是，如果键入“10/0”，它会在断点处暂停。 你甚至可以在运算符和数字之间放置任意数量的空格：`cin` 足够智能，可以适当地解析输入。

   ![显示程序在条件断点处暂停的 Visual studio 2019 的屏幕截图。](./media/calc-vs2019-debug-breakpoint.png "在条件断点暂停")

### <a name="useful-windows-in-the-debugger"></a>调试器中有用的窗口

每当调试代码时，你可能会注意到会出现一些新窗口。 你可借助这些窗口提高调试体验。 了解一下“自动”窗口  。 显示的“自动”窗口指示，在当前行之前，变量的当前值至少使用了三行  。 若要查看该函数的所有变量，请切换到“局部变量”窗口  。 实际上，可以在调试时修改这些变量的值，以查看它们对程序的影响。 在这种情况下，将不必理会。

   ![Visual Studio 2019 中“局部变量”窗口的屏幕截图。](./media/calc-vs2019-debug-locals.png "“局部变量”窗口")

也可将鼠标悬停在代码本身中的变量上，以查看当前暂停执行的当前值。 请先单击编辑器窗口，确保其处于焦点位置。

   ![显示出现变量值的工具提示的 Visual Studio 2019 的屏幕截图。](./media/calc-vs2019-hover-tooltip.png "悬停以查看当前变量值")

### <a name="to-continue-debugging"></a>继续调试

1. 左侧的黄线表示当前的执行点。 当前行调用 `Calculate`，因此按 F11 以“单步执行”函数   。 你会发现自己处于 `Calculate` 函数的主体中。 使用单步执行  时请小心；如果使用次数过多，可能会浪费大量时间。 它将进入你所在行中使用的任意代码，包括标准库函数。

1. 现在执行点位于 `Calculate` 函数的开头，按 F10 移动到程序执行的下一行  。 F10 也称为“单步跳过”   。 可以使用“单步跳过”在行与行之间移动，而无需深入研究行的每个部分的详细信息  。 一般情况下，应使用“单步跳过”而不是“单步执行”，除非希望深入研究从其他地方调用的代码（就像你访问 `Calculate` 的主体一样）   。

1. 继续使用 F10 “单步跳过”每一行，直到返回到另一个文件中的 `main()` 函数，然后停在 `cout` 行   。

   看起来程序是在按预期执行操作：取第一个数字，然后除以第二个数字。 在 `cout` 行，将鼠标悬停在 `result` 变量上，或在“自动”窗口中查看 `result` 。 你将看到其值以“inf”列出，这看起来似乎不正确，因此我们来修复此错误。 `cout` 行只输出存储在 `result` 中的任何值，因此当使用 F10 向前再执行一行时，控制台窗口将显示以下内容  ：

   ![显示除以零的结果的 Visual Studio 2019 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calc-vs2019-divide-by-zero-fail.png "除以零的结果")

   发生这种情况是因为除以零未定义，因此程序无法给请求的操作提供数值解。

### <a name="to-fix-the-divide-by-zero-error"></a>修复“除数为零”错误

让我们更简洁地处理除数为零的情况，以便用户可以了解问题。

1. 在 CalculatorTutorial.cpp 中进行以下更改  。 （借助“编辑并继续”调试器功能，你可以在编辑时让程序继续运行）  ：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include <iostream>
    #include "Calculator.h"

    using namespace std;

    int main()
    {
        double x = 0.0;
        double y = 0.0;
        double result = 0.0;
        char oper = '+';

        cout << "Calculator Console Application" << endl << endl;
        cout << "Please enter the operation to perform. Format: a+b | a-b | a*b | a/b" << endl;

        Calculator c;
        while (true)
        {
            cin  >> x  >> oper  >> y;
            if (oper == '/' && y == 0)
            {
                cout << "Division by 0 exception" << endl;
                continue;
            }
            else
            {
                result = c.Calculate(x, oper, y);
            }
            cout << "Result is: " << result << endl;
        }

        return 0;
    }
    ```

1. 现在，按 F5 一次  。 程序将继续执行，直到它必须暂停以请求用户输入。 再次输入 `10 / 0`。 现在，将输出更有用的信息。 用户被要求输入更多内容，程序继续正常执行。

   ![显示更改后的最终结果的 Visual Studio 2019 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calc-vs2019-final-verification.png "更改后的最终结果")

   > [!Note]
   > 在调试模式下编辑代码时，有可能会遇到旧代码。 当调试器仍在运行旧代码并且尚未使用更改进行更新时，将发生这种情况。 调试器会弹出一个对话框，通知你何时发生这种情况。 有时，你可能需要按 F5 来刷新正在执行的代码  。 特别是，如果在函数内部进行更改而执行点位于该函数内部，则需要退出该函数，然后再次返回该函数以获取更新的代码。 如果由于某种原因该操作不起作用，且你看到错误消息，则可以通过单击 IDE 顶部菜单下工具栏中的红色方块来停止调试，然后通过输入 F5 或通过选择工具栏上“停止”按钮旁的绿色“播放”箭头重新开始调试。

   > 了解“运行”和“调试”快捷方式
   >
   > - 按 F5（或“调试” > “启动调试”）启动调试会话（如果尚未激活），并运行程序，直到到达断点或程序需要用户输入    。 如果不需要用户输入，也没有可用的断点，程序将终止，当程序运行结束时，控制台窗口将自动关闭。 如果要运行类似“Hello World”程序，请使用 Ctrl+F5 或在输入 F5 之前设置断点以保持窗口打开   。
   > - Ctrl+F5（或“调试” > “开始执行(不调试)”在不进入调试模式的情况下运行应用程序    。 此快捷方式比调试要略微快一些，并且在程序执行完成后控制台窗口仍保持打开状态。
   > - F10（称为“单步跳过”步骤）可逐行迭代代码，并可视化代码的运行方式，以及执行每个步骤的变量值   。
   > - F11（称为“单步执行”）的工作方式类似于“单步跳过”，只是它将单步执行在执行行上调用的任何函数    。 例如，如果正在执行的行调用了一个函数，按下 F11 可将指针移动到函数体中，这样就可以在返回开始的行之前遵循正在运行的函数代码  。 按 F10 可执行函数调用并移动到下一行；函数调用仍然会发生，但是程序不会暂停来显示它在做什么  。

### <a name="close-the-app"></a>关闭应用程序

- 如果它仍在运行，请关闭计算器应用的控制台窗口。

## <a name="the-finished-app"></a>完成的应用

祝贺你！ 你已经完成计算器应用的代码，并在 Visual Studio 中生成和调试了该代码。

## <a name="next-steps"></a>后续步骤

[了解有关 Visual Studio for C++ 的更多信息](https://devblogs.microsoft.com/cppblog/getting-started-with-visual-studio-for-c-and-cpp-development/)

::: moniker-end

::: moniker range="<msvc-160"

C++ 程序员通常从在命令行上运行的“Hello, world!” 应用程序开始。 这就是本文中你将在 Visual Studio 中创建的内容，然后我们将继续介绍更具挑战性的内容：计算器应用。

## <a name="prerequisites"></a>先决条件

- 在 Visual Studio 中安装“使用 C++ 的桌面开发”工作负载并在计算机上运行  。 如果尚未安装，请参阅[在 Visual Studio 中安装 C++ 支持](../build/vscpp-step-0-installation.md)。

## <a name="create-your-app-project"></a>创建应用项目

Visual Studio 使用项目来组织应用的代码，使用解决方案来组织项目   。 项目包含用于生成应用的所有选项、配置和规则。 它还负责管理所有项目文件和任何外部文件间的关系。 要创建应用，需首先创建一个新项目和解决方案。

1. 在 Visual Studio 中的菜单栏上，选择“文件” > “新建” > “项目”    。 随即将打开“新建项目”窗口  。

2. 在左侧边栏上，确保选中“Visual C++”  。 在中心，选择“Windows 控制台应用程序”  。

3. 在底部的“名称”编辑框中，将新项目命名为“CalculatorTutorial”，然后选择“确定”    。

   ![“新建项目”对话框](./media/calculator-new-project-dialog.png "“新建项目”对话框")

   将创建一个空的 C++ Windows 控制台应用程序。 控制台应用程序使用 Windows 控制台窗口显示输出并接受用户输入。 在 Visual Studio 中，将打开一个编辑器窗口并显示生成的代码：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include "pch.h"
    #include <iostream>

    int main()
    {
        std::cout << "Hello World!\n";
    }

    // Run program: Ctrl + F5 or Debug > Start Without Debugging menu
    // Debug program: F5 or Debug > Start Debugging menu

    // Tips for Getting Started:
    //   1. Use the Solution Explorer window to add/manage files
    //   2. Use the Team Explorer window to connect to source control
    //   3. Use the Output window to see build output and other messages
    //   4. Use the Error List window to view errors
    //   5. Go to Project > Add New Item to create new code files, or Project > Add Existing Item to add existing code files to the project
    //   6. In the future, to open this project again, go to File > Open > Project and select the .sln file
    ```

## <a name="verify-that-your-new-app-builds-and-runs"></a>验证新应用是否生成并运行

新的 Windows 控制台应用程序模板创建了一个简单的 C++“Hello World”应用。 此时，可以看到 Visual Studio 如何生成并运行直接从 IDE 创建的应用。

1. 若要生成项目，请从“生成”菜单选择“生成解决方案”   。 “输出”窗口将显示生成过程的结果  。

   ![“输出”窗口显示生成过程结果的 Visual Studio 的屏幕截图。](./media/calculator-initial-build-output.png "生成项目")

1. 若要运行代码，请在菜单栏上选择“调试”、“开始执行(不调试)”   。

   ![显示代码成功运行的 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calculator-hello-world-console.png "启动项目")

   随即将打开控制台窗口，然后运行你的应用。 在 Visual Studio 中启动控制台应用时，它会运行代码，然后输出“按任意键继续。 。 。” 让你有机会看到输出。 祝贺你！ 你在 Visual Studio 中已创建首个“Hello, world!” 控制台应用！

1. 按任意键关闭该控制台窗口并返回到 Visual Studio。

现在即可使用你的工具在每次更改后生成并运行应用，以验证代码是否仍按预期运行。 如果未按预期运行，稍后，我们将向你演示调试方法。

## <a name="edit-the-code"></a>编辑代码

现在，将此模板中的代码转换为计算器应用。

1. 在“CalculatorTutorial.cpp”文件中，编辑代码以匹配此示例  ：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include "pch.h"
    #include <iostream>

    using namespace std;

    int main()
    {
        cout << "Calculator Console Application" << endl << endl;
        cout << "Please enter the operation to perform. Format: a+b | a-b | a*b | a/b"
            << endl;
        return 0;
    }

    // Run program: Ctrl + F5 or Debug > Start Without Debugging menu
    // Debug program: F5 or Debug > Start Debugging menu
    // Tips for Getting Started:
    //   1. Use the Solution Explorer window to add/manage files
    //   2. Use the Team Explorer window to connect to source control
    //   3. Use the Output window to see build output and other messages
    //   4. Use the Error List window to view errors
    //   5. Go to Project > Add New Item to create new code files, or Project > Add Existing Item to add existing code files to the project
    //   6. In the future, to open this project again, go to File > Open > Project and select the .sln file
    ```

   > 了解代码：
   >
   > - `#include` 语句允许引用位于其他文件中的代码。 有时，文件名使用尖括号 (\<\>) 包围；其他情况下，使用引号 (" ") 包围 。 通常，引用 C++ 标准库时使用尖括号，引用其他文件时使用引号。
   > - `#include "pch.h"`（或 Visual Studio 2017 及更低版本中的 `#include "stdafx.h"`）行引用预编译标头。 专业程序员经常使用它们来缩短编译时间，但它们超出了本教程的范围。
   > - `using namespace std;` 行提示编译器期望在此文件中使用 C++ 标准库中的内容。 如果没有此行，库中的每个关键字都必须以 `std::` 开头，以表示其范围。 例如，如果没有该行，则对 `cout` 的每个引用都必须写为 `std::cout`。 添加 `using` 语句是为了让代码看起来更清晰。
   > - `cout` 关键字用于在 C++ 中打印到标准输出。 “\<\<”运算符提示编译器将其右侧的任何内容发送到标准输出  。
   > - “endl”关键字与 Enter 键类似；用于结束该行并将光标移动到下一行  。 如果要执行相同的操作，最好在字符串中使用 `\n`用 "" 包含），因为使用 `endl` 会始终刷新缓冲，进而可能影响程序的性能，但由于这是一个非常小的应用，所以改为使用 `endl` 以提高可读性。
   > - 所有 C++ 语句都必须以分号结尾，所有 C++ 应用程序都必须包含 `main()` 函数。 该函数是程序开始运行时运行的函数。 若要使用所有代码，必须可从 `main()` 访问所有代码。

1. 要保存文件，请输入“Ctrl+S”，或者选择 IDE 顶部附近的“保存”图标，即菜单栏下工具栏中的软盘图标   。

1. 要运行该应用程序，请按“Ctrl+F5”或转到“调试”菜单，然后选择“启动但不调试”    。 如果看到“此项目已过期”弹出窗口，可选择“不再显示此对话框”，然后选择“是”生成应用程序    。 应会显示一个控制台窗口，其中显示代码中指定的文本。

   ![生成和启动应用程序](./media/calculator-first-launch.gif "生成和启动应用程序")

1. 完成后，请关闭控制台窗口。

## <a name="add-code-to-do-some-math"></a>添加代码来执行一些数学运算

现在可添加一些数学逻辑。

### <a name="to-add-a-calculator-class"></a>添加 Calculator 类

1. 转到“项目”菜单，并选择“添加类”   。 在“类名”编辑框中，输入“Calculator”   。 选择 **“确定”** 。 这会向项目中添加两个新文件。 若要同时保存所有已更改的文件，请按“Ctrl+Shift+S”  。 这是“文件” > “全部保存”的键盘快捷方式   。 在“保存”按钮旁边还有一个用于“全部保存”的工具栏按钮，这是两个软盘的图标   。 一般来说，最好经常使用“全部保存”，这样你保存时就不会遗漏任何文件  。

   ![显示用户打开“添加类”对话框，在“类名称”字段中键入 Calculator，然后选择“确定”的简短视频。](./media/calculator-create-class.gif "创建 Calculator 类")

   类就像执行某事的对象的蓝图。 在本示例中，我们定义了 Calculator 以及它的工作原理。 上文使用的“添加类”向导创建了与该类同名的 .h 和 .cpp 文件  。 可以在 IDE 一侧的“解决方案资源管理器”窗口中看到项目文件的完整列表  。 如果该窗口不可见，则可从菜单栏中打开它：选择“查看” > “解决方案资源管理器”   。

   ![显示 Calculator Tutorial 项目的“解决方案资源管理器”窗口的屏幕截图。](./media/calculator-solution-explorer.png "“解决方案资源管理器”")

   现在编辑器中应打开了三个选项卡：CalculatorTutorial.cpp、Calculator.h 和 Calculator.cpp    。 如果你无意关闭了其中一个，可通过在“解决方案资源管理器”窗口中双击来重新打开它  。

1. 在“Calculator.h”中，删除生成的 `Calculator();` 和 `~Calculator();` 行，因为在此处不需要它们  。 接下来，添加以下代码行，以便文件现在如下所示：

    ```cpp
    #pragma once
    class Calculator
    {
    public:
        double Calculate(double x, char oper, double y);
    };
    ```

   > 了解代码
   >
   > - 所添加的行声明了一个名为 `Calculate` 的新函数，我们将使用它来运行加法、减法、乘法和除法的数学运算。
   > - C ++ 代码被组织成标头 (.h) 文件和源 (.cpp) 文件   。 所有类型的编译器都支持其他几个文件扩展名，但这些是要了解的主要文件扩展名。 函数和变量通常在头文件中进行声明（即在头文件中指定名称和类型）和实现（或在源文件中指定定义）   。 若要访问在另一个文件中定义的代码，可以使用 `#include "filename.h"`，其中“filename.h”是声明要使用的变量或函数的文件的名称。
   > - 已删除的两行为该类声明了“构造函数”和“析构函数”   。 对于像这样的简单类，编译器会为你创建它们，但本教程不涉及其用法。
   > - 最好根据代码的功能将代码组织到不同的文件中，方便稍后需要这些代码时能够轻易查找到。 在本例中，我们分别定义了 `Calculator` 类和包含 `main()` 函数的文件，但我们计划在 `main()` 中引用 `Calculator` 类。

1. 你会看到 `Calculate` 下显示绿色波浪线。 因为我们还没有在 .cpp 文件中定义 `Calculate` 函数。 将鼠标悬停在单词上，单击弹出的灯泡，然后选择“在 Calculator.cpp 中创建 Calculate 定义”  。 随即将出现一个弹出窗口，可在其中查看在另一个文件中进行的代码更改。 该代码已添加到“Calculator.cpp”  。

   ![显示用户在 Calculator C P P 选项中选择“创建 Calculate 定义”的简短视频。](./media/calculator-create-definition.gif "创建 Calculate 定义")

   目前，它只返回 0.0。 让我们来更改它。 按 Esc 关闭弹出窗口  。

1. 切换到编辑器窗口中的“Calculator.cpp”文件  。 删除 `Calculator()` 和 `~Calculator()` 部分（就像在 .h 文件中一样）并将以下代码添加到 `Calculate()`：

    ```cpp
    #include "pch.h"
    #include "Calculator.h"

    double Calculator::Calculate(double x, char oper, double y)
    {
        switch(oper)
        {
            case '+':
                return x + y;
            case '-':
                return x - y;
            case '*':
                return x * y;
            case '/':
                return x / y;
            default:
                return 0.0;
        }
    }
    ```

   > 了解代码
   >
   > - 函数 `Calculate` 使用数字、运算符和第二个数字，然后对数字执行请求的操作。
   > - Switch 语句检查提供了哪个运算符，并仅执行与该操作对应的情况。 “default: case”是一个回滚，以防用户键入一个不被接受的运算符，因此程序不会中断。 通常，最好以更简洁的方式处理无效的用户输入，但这超出了本教程的范围。
   > - `double` 关键字表示支持十进制的数字类型。 因此，Calculator 可以处理十进制数学和整数数学。 要始终返回这样的数字，必须有 `Calculate` 函数，因为代码的最开始处出现了 `double`（这表示函数的返回类型），正因为此我们即使在默认情况下也返回 0.0。
   > - .h 文件声明函数“原型”，它预先告诉编译器它需要什么参数，以及期望它返回什么样的返回类型  。 .cpp 文件包含该函数的所有实现细节。

如果此时再次生成并运行代码，则在询问要执行的操作后，它仍然会退出。 接下来，将修改 `main` 函数以进行一些计算。

### <a name="to-call-the-calculator-class-member-functions"></a>调用 Calculator 类的成员函数

1. 现在让我们更新“CalculatorTutorial.cpp”中的 `main` 函数  ：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include "pch.h"
    #include <iostream>
    #include "Calculator.h"

    using namespace std;

    int main()
    {
        double x = 0.0;
        double y = 0.0;
        double result = 0.0;
        char oper = '+';

        cout << "Calculator Console Application" << endl << endl;
        cout << "Please enter the operation to perform. Format: a+b | a-b | a*b | a/b"
             << endl;

        Calculator c;
        while (true)
        {
            cin >> x >> oper >> y;
            result = c.Calculate(x, oper, y);
            cout << "Result is: " << result << endl;
        }

        return 0;
    }
    ```

   > 了解代码
   >
   > - 由于 C++ 程序总是从 `main()` 函数开始，我们需要从这里调用其他代码，因此需要 `#include` 语句。
   > - 声明了一些初始变量 `x`、`y`、`oper` 和 `result`，分别用于存储第一个数字、第二个数字、运算符和最终结果。 提供一些初始变量始终是最佳做法，这样可避免未定义的行为，此示例即是如此。
   > - `Calculator c;` 行声明一个名为“c”的对象作为 `Calculator` 类的实例。 类本身只是计算器工作方式的蓝图；对象是进行数学运算的特定计算器。
   > - `while (true)` 语句是一个循环。 只要 `()` 内的条件成立，循环内的代码就会一遍又一遍地执行。 由于条件被直接列出为 `true`，因此它始终为 true，所以循环永远运行。 若要关闭程序，用户必须手动关闭控制台窗口。 否则，程序始终等待新输入。
   > - `cin` 关键字用于接受来自用户的输入。 假设用户输入符合所需规范，此输入流足够智能，可以处理在控制台窗口中输入的一行文本，并按顺序将其放入列出的每个变量中。 可以修改此行以接受不同类型的输入，例如，两个以上的数字，但还需要更新 `Calculate()` 函数来处理此问题。
   > - `c.Calculate(x, oper, y);` 表达式调用前面定义的 `Calculate` 函数，并提供输入的输入值。 然后该函数返回一个存储在 `result` 中的数字。
   > - 最后，将 `result` 输出到控制台，以便用户查看计算结果。

### <a name="build-and-test-the-code-again"></a>再次生成和测试代码

现在是时候再次测试程序以确保一切正常。

1. 按“Ctrl+F5”重建并启动应用  。

1. 输入 `5 + 5`，然后按 Enter  。 验证结果是否为 10。

   ![显示 5 + 5 的正确结果的 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calculator-five-plus-five.png "5 + 5 的结果")

## <a name="debug-the-app"></a>调试应用

由于用户可以自由地在控制台窗口中输入任何内容，因此请确保计算器按预期处理某些输入。 我们不是运行程序，而是调试程序，因此可以逐步检查程序所执行的每一项操作。

### <a name="to-run-the-app-in-the-debugger"></a>在调试器中运行应用

1. 在用户被要求输入之后，在 `result = c.Calculate(x, oper, y);` 行上设置断点。 若要设置断点，请在该行旁边编辑器窗口左边缘的灰色竖线上单击。 将显示一个红点。

   ![显示用户创建表示断点的红点的 Visual Studio 的简短视频。](./media/calculator-set-breakpoint.gif "设置断点")

   现在，当我们调试程序时，它总是暂停该行的执行。 我们已大概了解了该程序可用于简单案例。 由于我们不想每次暂停执行，因此可以设置断点条件。

1. 右键单击表示断点的红点，并选择“条件”  。 在条件的编辑框中，输入 `(y == 0) && (oper == '/')`。 完成后，选择“关闭”按钮  。 条件将自动保存。

   ![显示用户打开“断点设置”部分并设置条件断点的 Visual Studio 2019 的简短视频。](./media/calculator-conditional-breakpoint.gif "设置条件断点")

   现在，如果尝试被 0 除，我们将在断点处暂停执行。

1. 若要调试程序，请按 F5 或选择带绿色箭头图标的“本地 Windows 调试程序”工具栏按钮   。 在控制台应用中，如果输入类似“5 - 0”的内容，程序将正常运行并继续运行。 但是，如果键入“10/0”，它会在断点处暂停。 你甚至可以在运算符和数字之间放置任意数量的空格；`cin` 足够智能，可以适当地解析输入。

   ![显示程序在条件断点处暂停的 Visual studio 的简短视频。](./media/calculator-debug-conditional.gif "在条件断点暂停")

### <a name="useful-windows-in-the-debugger"></a>调试器中有用的窗口

每当调试代码时，你可能会注意到会出现一些新窗口。 你可借助这些窗口提高调试体验。 了解一下“自动”窗口  。 显示的“自动”窗口指示，在当前行之前，变量的当前值至少使用了三行  。

   ![“自动”窗口](./media/calculator-autos.png "“自动”窗口")

若要查看该函数的所有变量，请切换到“局部变量”窗口  。 实际上，可以在调试时修改这些变量的值，以查看它们对程序的影响。 在这种情况下，将不必理会。

   ![“局部变量”窗口的屏幕截图。](./media/calculator-locals.png "“局部变量”窗口")

也可将鼠标悬停在代码本身中的变量上，以查看当前暂停执行的当前值。 请先单击编辑器窗口，确保其处于焦点位置。

   ![显示出现变量值的工具提示的简短视频。](./media/calculator-hover-tooltip.gif "悬停以查看当前变量值")

### <a name="to-continue-debugging"></a>继续调试

1. 左侧的黄线表示当前的执行点。 当前行调用 `Calculate`，因此按 F11 以“单步执行”函数   。 你会发现自己处于 `Calculate` 函数的主体中。 使用单步执行  时请小心；如果使用次数过多，可能会浪费大量时间。 它将进入你所在行中使用的任意代码，包括标准库函数。

1. 现在执行点位于 `Calculate` 函数的开头，按 F10 移动到程序执行的下一行  。 F10 也称为“单步跳过”   。 可以使用“单步跳过”在行与行之间移动，而无需深入研究行的每个部分的详细信息  。 一般情况下，应使用“单步跳过”而不是“单步执行”，除非希望深入研究从其他地方调用的代码（就像你访问 `Calculate` 的主体一样）   。

1. 继续使用 F10 “单步跳过”每一行，直到返回到另一个文件中的 `main()` 函数，然后停在 `cout` 行   。

   ![退出 Calculate 并检查结果](./media/calculator-undefined-zero.gif "退出 Calculate 并检查结果")

   看起来程序是在按预期执行操作：取第一个数字，然后除以第二个数字。 在 `cout` 行，将鼠标悬停在 `result` 变量上，或在“自动”窗口中查看 `result` 。 你将看到其值以“inf”列出，这看起来似乎不正确，因此我们来修复此错误。 `cout` 行只输出存储在 `result` 中的任何值，因此当使用 F10 向前再执行一行时，控制台窗口将显示以下内容  ：

   ![显示除以零的结果的 Microsoft Visual Studio 调试控制台的屏幕截图。](./media/calculator-divide-by-zero-fail.png "除以零的结果")

   发生这种情况是因为除以零未定义，因此程序无法给请求的操作提供数值解。

### <a name="to-fix-the-divide-by-zero-error"></a>修复“除数为零”错误

让我们更简洁地处理除数为零的情况，以便用户可以了解问题。

1. 在 CalculatorTutorial.cpp 中进行以下更改  。 （借助“编辑并继续”调试器功能，你可以在编辑时让程序继续运行）  ：

    ```cpp
    // CalculatorTutorial.cpp : This file contains the 'main' function. Program execution begins and ends there.
    //

    #include "pch.h"
    #include <iostream>
    #include "Calculator.h"

    using namespace std;

    int main()
    {
        double x = 0.0;
        double y = 0.0;
        double result = 0.0;
        char oper = '+';

        cout << "Calculator Console Application" << endl << endl;
        cout << "Please enter the operation to perform. Format: a+b | a-b | a*b | a/b" << endl;

        Calculator c;
        while (true)
        {
            cin  >> x  >> oper  >> y;
            if (oper == '/' && y == 0)
            {
                cout << "Division by 0 exception" << endl;
                continue;
            }
            else
            {
                result = c.Calculate(x, oper, y);
            }
            cout << "Result is: " << result << endl;
        }

        return 0;
    }
    ```

1. 现在，按 F5 一次  。 程序将继续执行，直到它必须暂停以请求用户输入。 再次输入 `10 / 0`。 现在，将输出更有用的信息。 用户被要求输入更多内容，程序继续正常执行。

   ![显示更改后的最终结果的 Microsoft Visual Studio 调试控制台的简短视频。](./media/calculator-final-verification.gif "更改后的最终结果")

   > [!Note]
   > 在调试模式下编辑代码时，有可能会遇到旧代码。 当调试器仍在运行旧代码并且尚未使用更改进行更新时，将发生这种情况。 调试器会弹出一个对话框，通知你何时发生这种情况。 有时，你可能需要按 F5 来刷新正在执行的代码  。 特别是，如果在函数内部进行更改而执行点位于该函数内部，则需要退出该函数，然后再次返回该函数以获取更新的代码。 如果由于某种原因该操作不起作用，且你看到错误消息，则可以通过单击 IDE 顶部菜单下工具栏中的红色方块来停止调试，然后通过输入 F5 或通过选择工具栏上“停止”按钮旁的绿色“播放”箭头重新开始调试。

   > 了解“运行”和“调试”快捷方式
   >
   > - 按 F5（或“调试” > “启动调试”）启动调试会话（如果尚未激活），并运行程序，直到到达断点或程序需要用户输入    。 如果不需要用户输入，也没有可用的断点，程序将终止，当程序运行结束时，控制台窗口将自动关闭。 如果要运行类似“Hello World”程序，请使用 Ctrl+F5 或在输入 F5 之前设置断点以保持窗口打开   。
   > - Ctrl+F5（或“调试” > “开始执行(不调试)”在不进入调试模式的情况下运行应用程序    。 此快捷方式比调试要略微快一些，并且在程序执行完成后控制台窗口仍保持打开状态。
   > - F10（称为“单步跳过”步骤）可逐行迭代代码，并可视化代码的运行方式，以及执行每个步骤的变量值   。
   > - F11（称为“单步执行”）的工作方式类似于“单步跳过”，只是它将单步执行在执行行上调用的任何函数    。 例如，如果正在执行的行调用了一个函数，按下 F11 可将指针移动到函数体中，这样就可以在返回开始的行之前遵循正在运行的函数代码  。 按 F10 可执行函数调用并移动到下一行；函数调用仍然会发生，但是程序不会暂停来显示它在做什么  。

### <a name="close-the-app"></a>关闭应用程序

- 如果它仍在运行，请关闭计算器应用的控制台窗口。

## <a name="the-finished-app"></a>完成的应用

祝贺你！ 你已经完成计算器应用的代码，并在 Visual Studio 中生成和调试了该代码。

## <a name="next-steps"></a>后续步骤

[了解有关 Visual Studio for C++ 的更多信息](https://devblogs.microsoft.com/cppblog/getting-started-with-visual-studio-for-c-and-cpp-development/)

::: moniker-end
