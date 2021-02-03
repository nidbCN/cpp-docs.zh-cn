---
description: '了解详细信息：/DELAY (延迟加载导入设置) '
title: /DELAY（延迟加载导入设置）
ms.date: 01/28/2021
f1_keywords:
- /delay
- VC.Project.VCLinkerTool.DelayNoBind
- VC.Project.VCLinkerTool.SupportUnloadOfDelayLoadedDLL
- VC.Project.VCLinkerTool.DelayUnload
helpviewer_keywords:
- delayed loading of DLLs, /DELAY option
- DELAY linker option
- /DELAY linker option
- -DELAY linker option
ms.openlocfilehash: 0dd6aaaffd378afe4ca7d75180da869b2748d639
ms.sourcegitcommit: c20734f18d3d49bb38b1628c68b53b54b3eeeb03
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99522191"
---
# <a name="delay-delay-load-import-settings"></a>`/DELAY` (延迟加载导入设置) 

用于控制在运行时延迟加载 Dll 的链接器选项。

## <a name="syntax"></a>语法

> **`/DELAY:UNLOAD`**\
> **`/DELAY:NOBIND`**

## <a name="remarks"></a>备注

**`/DELAY`** 选项控制 dll 的 [延迟加载](linker-support-for-delay-loaded-dlls.md)：

- **`/DELAY:UNLOAD`** 限定符告知延迟加载 helper 函数支持 DLL 的显式卸载。 导入地址表 (IAT) 被重置为其原始形式，从而使 IAT 指针无效并导致它们被覆盖。

   如果不选择 **`/DELAY:UNLOAD`** ，对的任何调用 [`__FUnloadDelayLoadedDLL`](linker-support-for-delay-loaded-dlls.md#explicitly-unload-a-delay-loaded-dll) 都将失败。

- **`/DELAY:NOBIND`** 限定符通知链接器不要在最终映像中包含可绑定的 IAT。 默认值是为延迟加载的 DLL 创建可绑定的 IAT。 生成的图像不能静态绑定。 可在执行前静态绑定具有可绑定 Iat 的 (映像。 ) 有关详细信息，请参阅 [`/BIND`](bind.md) 。

   如果 DLL 是绑定的，则 helper 函数尝试使用绑定的信息，而不是 [`GetProcAddress`](/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress) 对每个引用的导入调用。 如果时间戳或首选地址与加载的 DLL 中的时间戳或首选地址不匹配，则 helper 函数假定绑定的 IAT 已过时。 它将继续运行，就像绑定的 IAT 不存在一样。

   **`/DELAY:NOBIND`** 使程序图像更大，但可以加快 DLL 的加载时间。 如果永远不打算绑定 DLL，则 **`/DELAY:NOBIND`** 会阻止生成绑定的 IAT。

若要指定要延迟加载的 Dll，请使用 [`/DELAYLOAD`](delayload-delay-load-import.md) 选项。

### <a name="to-set-this-linker-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此链接器选项

1. 打开项目的“属性页”  对话框。 有关信息，请参阅 [在 Visual Studio 中设置 c + + 编译器和生成属性](../working-with-project-properties.md)。

1. 选择 "**配置属性**  >  **链接器**  >  **高级** 属性" 页。

1. 修改 " **延迟加载的 DLL** " 属性。 选择“确定”以保存更改  。

### <a name="to-set-this-linker-option-programmatically"></a>以编程方式设置此链接器选项

- 请参阅 <xref:Microsoft.VisualStudio.VCProjectEngine.VCLinkerTool.DelayLoadDLLs%2A>。

## <a name="see-also"></a>另请参阅

[MSVC 链接器参考](linking.md)\
[MSVC 链接器选项](linker-options.md)
