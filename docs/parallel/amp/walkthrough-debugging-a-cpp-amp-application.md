---
description: 了解详细信息：演练：调试 C++ AMP 应用程序
title: 演练：调试 C++ AMP 应用程序
ms.date: 03/07/2021
helpviewer_keywords:
- debugging, C++ Accelerated Massive Parallelism
- C++ AMP, debugging
- C++ Accelerated Massive Parallelism, debugging
- debugging, C++ AMP
ms.openlocfilehash: 7073de40a3b985da89d4f0bb03046696f35c8f02
ms.sourcegitcommit: 6ed44d9c3fb32e965e363b9c69686739a90a2117
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465581"
---
# <a name="walkthrough-debugging-a-c-amp-application"></a>演练：调试 C++ AMP 应用程序

本文演示了如何调试使用 C++ Accelerated Massive Parallelism (C++ AMP) 的应用程序，以利用图形处理单元 (GPU) 。 它使用总结大整数数组的并行缩减程序。 本演练演示以下任务：

- 启动 GPU 调试器。

- 在“GPU 线程”窗口中检查 GPU 线程。

- 使用 " **并行堆栈** " 窗口同时观察多个 GPU 线程的调用堆栈。

- 使用 " **并行监视** " 窗口可以同时检查多个线程的单个表达式的值。

- 标记、冻结、解冻和组合 GPU 线程。

- 将 Tile 的所有线程执行到代码中的特定位置。

## <a name="prerequisites"></a>先决条件

在开始本演练之前：

- 阅读 [C++ AMP 概述](../../parallel/amp/cpp-amp-overview.md)。

- 确保文本编辑器中显示行号。 有关详细信息，请参阅 [如何：在编辑器中显示行号](/visualstudio/ide/reference/how-to-display-line-numbers-in-the-editor)。

- 请确保至少运行 Windows 8 或 Windows Server 2012，以支持在软件模拟器上进行调试。

[!INCLUDE[note_settings_general](../../mfc/includes/note_settings_general_md.md)]

### <a name="to-create-the-sample-project"></a>创建示例项目

根据所使用的 Visual Studio 版本，创建项目的说明有所不同。 请确保在此页上的目录上选择了正确的文档版本。

::: moniker range="msvc-160"

### <a name="to-create-the-sample-project-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建示例项目

1. 在菜单栏上，选择“文件”>“新建”>“项目”，打开“创建新项目”对话框     。

1. 在对话框顶部，将“语言”  设置为“C++”  ，将“平台”  设置为“Windows”  ，并将“项目类型”  设置为“控制台”  。

1. 从筛选的项目类型列表中，选择“控制台应用”，然后选择“下一步”   。 在下一页中，在 `AMPMapReduce` " **名称** " 框中输入以指定项目的名称，并指定项目位置（如果需要不同的位置）。

   ![为项目命名](../../build/media/mathclient-project-name-2019.png "将该项目命名为")

1. 选择“创建”  按钮创建客户端项目。

::: moniker-end

::: moniker range="<=msvc-150"

### <a name="to-create-the-sample-project-in-visual-studio-2017-or-visual-studio-2015"></a>在 Visual Studio 2017 或 Visual Studio 2015 中创建示例项目

1. 启动 Visual Studio。

1. 在菜单栏上，依次选择“文件”  >“新建”  >“项目”  。

1. 在 "模板" 窗格中的 " **已安装** " 下，选择 **Visual C++**。

1. 选择 " **Win32 控制台应用程序**"， `AMPMapReduce` 在 " **名称** " 框中键入，然后选择 " **确定"** 按钮。

1. 选择“下一步”按钮  。

1. 清除 " **预编译标头** " 复选框，然后选择 " **完成** " 按钮。

1. 在 **解决方案资源管理器** 中，从项目中删除 *stdafx.h*、 *targetver.h* 和 *stdafx.h。*

::: moniker-end

接下来：

1. 打开 AMPMapReduce.cpp 并用下面的代码替换其中的内容。

```cpp
    // AMPMapReduce.cpp defines the entry point for the program.
    // The program performs a parallel-sum reduction that computes the sum of an array of integers.

    #include <stdio.h>
    #include <tchar.h>
    #include <amp.h>

    const int BLOCK_DIM = 32;

    using namespace concurrency;

    void sum_kernel_tiled(tiled_index<BLOCK_DIM> t_idx, array<int, 1> &A, int stride_size) restrict(amp)
    {
        tile_static int localA[BLOCK_DIM];

        index<1> globalIdx = t_idx.global * stride_size;
        index<1> localIdx = t_idx.local;

        localA[localIdx[0]] =  A[globalIdx];

        t_idx.barrier.wait();

        // Aggregate all elements in one tile into the first element.
        for (int i = BLOCK_DIM / 2; i > 0; i /= 2)
        {
            if (localIdx[0] < i)
            {

                localA[localIdx[0]] += localA[localIdx[0] + i];
            }

            t_idx.barrier.wait();
        }

        if (localIdx[0] == 0)
        {
            A[globalIdx] = localA[0];
        }
    }

    int size_after_padding(int n)
    {
        // The extent might have to be slightly bigger than num_stride to
        // be evenly divisible by BLOCK_DIM. You can do this by padding with zeros.
        // The calculation to do this is BLOCK_DIM * ceil(n / BLOCK_DIM)
        return ((n - 1) / BLOCK_DIM + 1) * BLOCK_DIM;
    }

    int reduction_sum_gpu_kernel(array<int, 1> input)
    {
        int len = input.extent[0];

        //Tree-based reduction control that uses the CPU.
        for (int stride_size = 1; stride_size < len; stride_size *= BLOCK_DIM)
        {
            // Number of useful values in the array, given the current
            // stride size.
            int num_strides = len / stride_size;

            extent<1> e(size_after_padding(num_strides));

            // The sum kernel that uses the GPU.
            parallel_for_each(extent<1>(e).tile<BLOCK_DIM>(), [&input, stride_size] (tiled_index<BLOCK_DIM> idx) restrict(amp)
            {
                sum_kernel_tiled(idx, input, stride_size);
            });
        }

        array_view<int, 1> output = input.section(extent<1>(1));
        return output[0];
    }

    int cpu_sum(const std::vector<int> &arr) {
        int sum = 0;
        for (size_t i = 0; i < arr.size(); i++) {
            sum += arr[i];
        }
        return sum;
    }

    std::vector<int> rand_vector(unsigned int size) {
        srand(2011);

        std::vector<int> vec(size);
        for (size_t i = 0; i < size; i++) {
            vec[i] = rand();
        }
        return vec;
    }

    array<int, 1> vector_to_array(const std::vector<int> &vec) {
        array<int, 1> arr(vec.size());
        copy(vec.begin(), vec.end(), arr);
        return arr;
    }

    int _tmain(int argc, _TCHAR* argv[])
    {
        std::vector<int> vec = rand_vector(10000);
        array<int, 1> arr = vector_to_array(vec);

        int expected = cpu_sum(vec);
        int actual = reduction_sum_gpu_kernel(arr);

        bool passed = (expected == actual);
        if (!passed) {
            printf("Actual (GPU): %d, Expected (CPU): %d", actual, expected);
        }
        printf("sum: %s\n", passed ? "Passed!" : "Failed!");

        getchar();

        return 0;
    }
```

1. 在菜单栏上，依次选择“文件” > “全部保存”。

1. 在 **解决方案资源管理器** 中，打开 **AMPMapReduce** 的快捷菜单，然后选择 " **属性**"。

1. 在 "**属性页**" 对话框中的 "**配置属性**" 下，选择 " **c/c + +**  >  **预编译标头**"。

1. 对于 " **预编译标头** " 属性，选择 " **不使用预编译头**"，然后选择 **"确定"** 按钮。

1. 在菜单栏上，依次选择“生成” > “生成解决方案” 。

## <a name="debugging-the-cpu-code"></a>调试 CPU 代码

在此过程中，您将使用本地 Windows 调试器，以确保此应用程序中的 CPU 代码是正确的。 此应用程序中的 CPU 代码段特别值得注意的是 **`for`** 函数中的循环 `reduction_sum_gpu_kernel` 。 它控制运行在 GPU 上的基于树的并行缩减。

### <a name="to-debug-the-cpu-code"></a>调试 CPU 代码

1. 在 **解决方案资源管理器** 中，打开 **AMPMapReduce** 的快捷菜单，然后选择 " **属性**"。

2. 在 " **属性页** " 对话框中的 " **配置属性**" 下，选择 " **调试**"。 验证是否在调试器中选择了 " **本地 Windows 调试器** " **以启动** 列表。

3. 返回到 **代码编辑器**。

4. 在下图所示的代码行上设置断点（大约第 67 和 70 行）。

   ![在编辑器中的代码行旁边标记的 CPU 断点。](../../parallel/amp/media/campcpubreakpoints.png "CPU 断点") <br/>
   CPU 断点

5. 在菜单栏上，依次选择“调试” > “开始调试”。

6. 在 " **局部变量** " 窗口中，观察的值， `stride_size` 直到达到第70行的断点。

7. 在菜单栏上，依次选择“调试” > “停止调试”。

## <a name="debugging-the-gpu-code"></a>调试 GPU 代码

本部分说明如何调试 GPU 代码，即 `sum_kernel_tiled` 函数包含的代码。 GPU 代码为每个“块”并行计算整数和。

### <a name="to-debug-the-gpu-code"></a>调试 GPU 代码

1. 在 **解决方案资源管理器** 中，打开 **AMPMapReduce** 的快捷菜单，然后选择 " **属性**"。

2. 在 " **属性页** " 对话框中的 " **配置属性**" 下，选择 " **调试**"。

3. 在“要启动的调试器”列表中，选择“本地 Windows 调试器” 。

4. 在 " **调试器类型** " 列表中，验证是否选择了 " **自动** "。

    **自动** 为默认值。 在 Windows 10 之前的版本中， **仅 GPU** 是所需的值而不是 " **自动**"。

5. 选择 **“确定”** 按钮。

6. 如下图所示，在第 30 行处设置一个断点。

   ![在编辑器中的代码行旁边标记的 GPU 断点。](../../parallel/amp/media/campgpubreakpoints.png "GPU 断点") <br/>
   GPU 断点

7. 在菜单栏上，依次选择“调试” > “开始调试”。 在 GPU 调试期间，CPU 67 代码中的断点不会执行70，因为这些代码行在 CPU 上运行。

### <a name="to-use-the-gpu-threads-window"></a>使用“GPU 线程”窗口

1. 若要打开 " **GPU 线程**" 窗口，请在菜单栏上选择 "**调试**  >  **Windows**  >  **GPU 线程**"。

   可以在显示的 " **Gpu 线程** " 窗口中检查 gpu 线程的状态。

2. 将 " **GPU 线程** " 窗口停靠在 Visual Studio 的底部。 选择 " **展开线程切换** " 按钮以显示 "磁贴" 和 "线程" 文本框。 " **GPU 线程** " 窗口显示活动和阻塞的 GPU 线程的总数，如下图所示。

   ![具有4个活动线程的 GPU 线程窗口。](../../parallel/amp/media/campc.png "包含 4 个活动线程的 GPU 线程窗口") <br/>
   “GPU 线程”窗口

   为此计算分配了313磁贴。 每个 Tile 包含 32 个线程。 由于本地 GPU 调试在软件模拟器中进行，因此有四个活动的 GPU 线程。 四个线程同时执行指令，然后一起移动到下一条指令。

   在 " **GPU 线程** " 窗口中，有四个可用的 gpu 线程，并在大约第21行 () 上定义的 [tile_barrier：： wait](reference/tile-barrier-class.md#wait) 语句中阻止了28个 gpu 线程 `t_idx.barrier.wait();` 。 所有这 32 个GPU 线程都属于第一个 Tile `tile[0]`。 当前线程所在的行由箭头指示。 若要切换到其他线程，请使用下列方法之一：

    - 在 " **GPU 线程** " 窗口中要切换到的线程所在的行中，打开快捷菜单，然后选择 " **切换到线程**"。 如果行表示多个线程，将根据线程坐标切换到第一个线程。

    - 在对应的文本框中输入线程的磁贴和线程值，然后选择 " **切换线程** " 按钮。

   " **调用堆栈** " 窗口显示当前 GPU 线程的调用堆栈。

### <a name="to-use-the-parallel-stacks-window"></a>使用“并行堆栈”窗口

1. 若要打开 "**并行堆栈**" 窗口，请在菜单栏上选择 "**调试**  >  **Windows**  >  **并行堆栈**"。

   您可以使用 " **并行堆栈** " 窗口同时检查多个 GPU 线程的堆栈帧。

2. 将 " **并行堆栈** " 窗口停靠在 Visual Studio 的底部。

3. 请确保在左上角的列表中选择 " **线程** "。 在下图中，" **并行堆栈** " 窗口显示在 " **gpu 线程** " 窗口中看到的 gpu 线程的调用堆栈焦点视图。

   ![具有4个活动线程的并行堆栈窗口。](../../parallel/amp/media/campd.png "包含 4 个活动线程的并行堆栈窗口") <br/>
   “并行堆栈”窗口

   32 个线程从 `_kernel_stub` 执行到 `parallel_for_each` 函数调用中的 lambda 语句，随后执行到 `sum_kernel_tiled` 函数，再从这里进行并行缩减。 32线程中的28已执行到语句， [`tile_barrier::wait`](reference/tile-barrier-class.md#wait) 并在第22行保持阻塞，而其他四个线程在函数中的 `sum_kernel_tiled` 第30行保持活动状态。

   可以检查 GPU 线程的属性。 它们在 "**并行堆栈**" 窗口的丰富数据提示的 " **GPU 线程**" 窗口中提供。 若要查看它们，请将指针悬停在的堆栈帧上 `sum_kernel_tiled` 。 下图显示了数据提示。

   !["并行堆栈" 窗口的数据提示。](../../parallel/amp/media/campe.png "并行堆栈窗口的数据提示") <br/>
   GPU 线程数据提示

   有关 " **并行堆栈** " 窗口的详细信息，请参阅 [使用 "并行堆栈" 窗口](/visualstudio/debugger/using-the-parallel-stacks-window)。

### <a name="to-use-the-parallel-watch-window"></a>使用“并行监视”窗口

1. 若要打开 "**并行监视**" 窗口，请在菜单栏上选择 "**调试**  >  **Windows**  >  **并行监视**  >  **并行监视 1**"。

   您可以使用 " **并行监视** " 窗口跨多个线程检查表达式的值。

2. 将 " **并行监视 1** " 窗口停靠在 Visual Studio 的底部。 " **并行监视** " 窗口的表中有32行。 每个对应于出现在 "GPU 线程" 窗口和 " **并行堆栈** " 窗口中的 gpu 线程。 现在，可以输入所需的表达式，以检查其在所有这 32 个 GPU 线程中的值。

3. 选择 " **添加监视** " 列标题，输入 `localIdx` ，然后选择 **enter** 键。

4. 再次选择 " **添加监视** " 列标题，键入 `globalIdx` ，然后选择 **Enter** 键。

5. 再次选择 " **添加监视** " 列标题，键入 `localA[localIdx[0]]` ，然后选择 **Enter** 键。

   您可以通过选择相应的列标题来按指定表达式排序。

   选择 " **localA [localIdx [0]]** " 列标题对列进行排序。 下图显示了按 **localA [localIdx [0]]** 排序的结果。

   ![带有排序结果的并行监视窗口。](../../parallel/amp/media/campf.png "包含分类结果的并行监视窗口") <br/>
   排序结果

   可以通过选择 " **excel** " 按钮，然后选择 "**在 excel 中打开**"，将 "**并行监视**" 窗口中的内容导出到 excel。 如果在开发计算机上安装了 Excel，则该按钮将打开包含内容的 Excel 工作表。

6. 在 " **并行监视** " 窗口的右上角有一个 "筛选器" 控件，可用于通过使用布尔表达式来筛选内容。 `localA[localIdx[0]] > 20000`在 "筛选器控件" 文本框中输入，然后选择 **enter** 键。

   该窗口现在只包含 `localA[localIdx[0]]` 值大于 20000 的线程。 内容仍按 `localA[localIdx[0]]` 列排序，这是你之前选择的排序操作。

## <a name="flagging-gpu-threads"></a>标记 GPU 线程

可以通过在 " **Gpu 线程** " 窗口、" **并行监视** " 窗口或 " **并行堆栈** " 窗口中的数据提示中标记特定的 gpu 线程来标记这些线程。 如果 "GPU 线程" 窗口中的某行包含多个线程，则标记该行会标记该行中包含的所有线程。

### <a name="to-flag-gpu-threads"></a>标记 GPU 线程

1. 选择 "**并行监视 1** " 窗口中的 " **[Thread]** " 列标题，按图块索引和线程索引进行排序。

2. 在菜单栏上，选择 "**调试**  >  **继续**"，这会导致四个处于活动状态的线程 (在 AMPMapReduce) 的第32行定义。

3. 在当前处于活动状态的四个线程所在行的左侧，选择标志符号。

   下图显示了 " **GPU 线程** " 窗口中的四个活动标记的线程。

   ![带有标记线程的 GPU 线程窗口。](../../parallel/amp/media/campg.png "包含已标记线程的 GPU 线程窗口") <br/>
   GPU 线程窗口中的活动线程

   "并行 **监视** " 窗口和 " **并行堆栈** " 窗口的数据提示都指示已标记的线程。

4. 如果要将重点放在标记的四个线程上，可以选择仅显示标记的线程。 它限制显示在 " **GPU 线程**"、" **并行监视**" 和 " **并行堆栈** " 窗口中的内容。

   选择任何窗口或 "**调试位置**" 工具栏上的 "**只显示标记** 为" 按钮。 下图显示了 "**调试位置**" 工具栏上的 "**仅显示标记** 的" 按钮。

   ![带有仅显示标记图标的 "调试位置" 工具栏。](../../parallel/amp/media/camph.png "具有“仅显示已标记项”图标的“调试位置”工具栏") <br/>
   **显示仅标记** 按钮

   现在，" **GPU 线程**"、" **并行监视**" 和 " **并行堆栈** " 窗口仅显示标记的线程。

## <a name="freezing-and-thawing-gpu-threads"></a>冻结和解冻 GPU 线程

可以在 " **Gpu 线程** " 窗口或 " **并行监视** " 窗口中冻结 (挂起) 和解冻 (恢复) gpu 线程。 可以采用相同的方式冻结和解冻 CPU 线程;有关信息，请参阅 [如何：使用 "线程" 窗口](/visualstudio/debugger/how-to-use-the-threads-window)。

### <a name="to-freeze-and-thaw-gpu-threads"></a>冻结和解冻 GPU 线程

1. 选择 " **仅显示标记** 的按钮" 以显示所有线程。

2. 在菜单栏上，选择 "**调试**  >  **继续**"。

3. 打开活动行的快捷菜单，然后选择 " **冻结**"。

   下图 " **GPU 线程** " 窗口显示了所有四个线程均已冻结。

   ![显示冻结线程的 GPU 线程窗口。](../../parallel/amp/media/campk.png "显示已冻结线程的 GPU 线程窗口") <br/>
   " **GPU 线程** " 窗口中的已冻结线程

   同样，" **并行监视** " 窗口会显示所有四个线程均已冻结。

4. 在菜单栏上，选择 "**调试**  >  " "**继续**"，以允许接下来的四个 GPU 线程越过第22行的关卡并到达第30行的断点处。 " **GPU 线程** " 窗口显示四个以前冻结的线程保持冻结状态和处于活动状态。

5. 在菜单栏上，依次选择 " **调试**" 和 " **继续**"。

6. 在 " **并行监视** " 窗口中，还可以解冻单个或多个 GPU 线程。

### <a name="to-group-gpu-threads"></a>对 GPU 线程分组

1. 在 " **GPU 线程** " 窗口中某个线程的快捷菜单上，选择 " **分组依据**"、" **地址**"。

   " **GPU 线程** " 窗口中的线程按地址分组。 该地址对应于每组线程所在的反汇编指令。 24个线程位于第22行，其中 [tile_barrier：： Wait 方法](reference/tile-barrier-class.md#wait) 执行。 12 个线程位于第 32 行屏障的指令处。 其中 4 个线程经过标记。 8 个线程位于第 30 行的断点处。 其中 4 个线程已冻结。 下图显示了 " **GPU 线程** " 窗口中的分组线程。

   ![包含按地址分组的线程的 GPU 线程窗口。](../../parallel/amp/media/campl.png "其中的线程按地址进行分组的 GPU 线程窗口") <br/>
   " **GPU 线程** " 窗口中的分组线程

2. 还可以通过打开 "**并行监视**" 窗口的数据网格的快捷菜单来执行 "**分组依据**" 操作。 选择 " **分组依据**"，然后选择与要将线程分组的方式相对应的菜单项。

## <a name="running-all-threads-to-a-specific-location-in-code"></a>将所有线程运行到代码中的特定位置

使用 " **运行当前磁贴到光标处**" 将给定磁贴中的所有线程运行到包含光标的行。

### <a name="to-run-all-threads-to-the-location-marked-by-the-cursor"></a>将所有线程运行到光标指示的位置

1. 在冻结的线程的快捷菜单上，选择 " **解冻**"。

2. 在 **代码编辑器** 中，将光标置于第30行。

3. 在 **代码编辑器** 的快捷菜单中，选择 " **运行当前磁贴到光标处**"。

   之前在第 21 行处受阻的 24 个线程将继续运行到第 32 行。 它显示在 " **GPU 线程** " 窗口中。

## <a name="see-also"></a>另请参阅

[C++ AMP 概述](../../parallel/amp/cpp-amp-overview.md)\
[调试 GPU 代码](/visualstudio/debugger/debugging-gpu-code)\
[如何：使用 "GPU 线程" 窗口](/visualstudio/debugger/how-to-use-the-gpu-threads-window)\
[如何：使用并行监视窗口](/visualstudio/debugger/how-to-use-the-parallel-watch-window)\
[利用并发可视化工具分析 C++ AMP 代码](/archive/blogs/nativeconcurrency/analyzing-c-amp-code-with-the-concurrency-visualizer)
