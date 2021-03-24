---
title: 在 Windows、Linux 和 macOS 上安装 vcpkg
description: 了解如何在 Windows、macOS 和 Linux 上安装和更新 vcpkg。
ms.date: 12/17/2020
ms.topic: reference
ms.technology: cpp-ide
ms.openlocfilehash: b508134693d5687898f0e5202f54ee35b91680bb
ms.sourcegitcommit: 977b5151e7dae7584112328bab515fb15622a6cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104883852"
---
# <a name="install-vcpkg-on-windows-linux-and-macos"></a>在 Windows、Linux 和 macOS 上安装 vcpkg

可通过从 vcpkg GitHub 存储库创建本地克隆（副本）来安装 vcpkg。

## <a name="install-vcpkg"></a>安装 vcpkg

选择特定说明的平台：

### <a name="linux"></a>[Linux](#tab/linux)

Linux 的先决条件：

- [Git](https://git-scm.com/downloads)
- g++ 版本 6 或更高版本

#### <a name="to-install-linux-development-tools"></a>安装 Linux 开发工具

不同的 Linux 发行版可能需要安装不同的包。 按照 Debian、Ubuntu、popOS 和其他基于 Debian 的发行版上的说明进行操作：

1. 在 shell 窗口中，运行以下命令：

   **`sudo apt-get update`**

1. 更新完成后，运行以下命令：

   **`sudo apt-get install build-essential tar curl zip unzip`**

按照 CentOS 上的以下说明执行操作：

1. 在 shell 窗口中，运行以下命令：

   **`sudo yum install centos-release-scl`**

1. 接下来，运行以下命令，安装并启用开发工具：

   **`sudo yum install devtoolset-7`**

   **`scl enable devtoolset-7 bash`**

对于任何其他发行版，请确保安装 g++ 6 或更高版本。

#### <a name="to-copy-and-set-up-vcpkg-on-linux"></a>在 Linux 上复制和设置 vcpkg

1. 在 shell 窗口中，为 vcpkg 的克隆实例创建目录。 如果打算为不同的生成目标安装库，最好在目录名称中包含目标。 建议使用短路径名称（不含空格）（如 `./x64` 或 `./iot`），否则，某些端口生成系统可能会出现路径问题 。 在 shell 窗口中，切换到刚刚创建的目录。

1. 从 GitHub 克隆 vcpkg 存储库：[https://github.com/Microsoft/vcpkg](https://github.com/Microsoft/vcpkg)。

   > **`git clone https://github.com/microsoft/vcpkg`**

   此命令在 `vcpkg` 子目录中创建存储库的本地副本。 此位置是此 vcpkg 克隆的 vcpkg 根目录。

1. 接下来，切换到 vcpkg 根目录，并运行 vcpkg 引导程序命令：

   > **`./bootstrap-vcpkg.sh`**

   引导程序将使用编译器、工具和标准库的位置配置 vcpkg。

### <a name="macos"></a>[macOS](#tab/macos)

macOS 的先决条件：

- macOS 开发人员工具
- 在 macOS 10.14 或更低版本上，你还需要：
  - Homebrew
  - g++ 版本 6 或更高版本

#### <a name="to-install-macos-developer-tools"></a>安装 macOS 开发人员工具

1. 在 macOS 10.15 上，在“终端”中运行以下命令：

   **`xcode-select --install`**

   接下来，按照窗口中显示的提示进行操作。

在 macOS 10.14 及更早版本中，还需要从 homebrew 安装 g++。 仅当使用早于 10.15 的 macOS 版本时，才需要执行此过程。

#### <a name="to-install-gcc-for-macos-before-1015"></a>安装早于 10.15 的用于 macOS 的 GCC

1. 若要安装 Homebrew，请打开“终端”，然后运行以下命令：

   **`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`**

1. 若要安装最新版本的 GCC，请在“终端”中运行以下命令：

   **`brew install gcc`**

#### <a name="to-copy-and-set-up-vcpkg-on-macos"></a>在 macOS 上复制和设置 vcpkg

1. 在“终端”窗口中，为 vcpkg 的克隆实例创建目录。 如果打算为不同的生成目标安装库，最好在目录名称中包含目标。 建议使用短路径名称（不含空格）（如 `./macos` 或 `./iot`），否则，某些端口生成系统可能会出现路径问题 。 在“终端”窗口中，切换到刚刚创建的目录。

1. 从 GitHub 克隆 vcpkg 存储库：[https://github.com/Microsoft/vcpkg](https://github.com/Microsoft/vcpkg)。

   > **`git clone https://github.com/microsoft/vcpkg`**

   此命令在 `vcpkg` 子目录中创建存储库的本地副本。 此位置是此 vcpkg 克隆的 vcpkg 根目录。

1. 接下来，切换到 vcpkg 根目录，并运行 vcpkg 引导程序命令：

   > **`./bootstrap-vcpkg.sh`**

   引导程序将使用编译器、工具和标准库的位置配置 vcpkg。

### <a name="windows"></a>Windows

先决条件：

- Windows 7 或更高版本
- [用于 Windows 的 Git](https://git-scm.com/downloads)
- [Visual Studio](https://visualstudio.microsoft.com/) 2015 更新 3 或更高版本（带英文语言包）

#### <a name="to-copy-and-set-up-vcpkg-on-windows"></a>在 Windows 上复制和设置 vcpkg

1. 在“命令提示符”窗口中，为 vcpkg 的克隆实例创建目录。 如果打算为不同的生成目标安装库，最好在目录名称中包含目标。 建议使用短路径名称（不含空格）（如 `C:\src\win32\` 或 `C:\dev\iot\`），否则，某些端口生成系统可能会出现路径问题 。 在命令窗口中，切换到刚刚创建的目录。

1. 从 GitHub 克隆 vcpkg 存储库：[https://github.com/Microsoft/vcpkg](https://github.com/Microsoft/vcpkg)。

   > **`git clone https://github.com/microsoft/vcpkg`**

   此命令在 `vcpkg` 子目录中创建存储库的本地副本。 此位置是此 vcpkg 克隆的 vcpkg 根目录。

1. 下载完成后，在“命令提示符”窗口切换到 `vcpkg` 目录。

1. 在 vcpkg 根目录下，运行 vcpkg 引导程序命令：

   > **`bootstrap-vcpkg.bat`**

   引导程序将使用 Microsoft C/C++ 工具、库和 Windows SDK 的位置配置 vcpkg。

---

设置 vcpkg 后，即可安装库。 有关详细信息，请参阅[使用 vcpkg 管理库](manage-libraries-with-vcpkg.md)。 Vcpkg 也可以与 Windows 上的 Visual Studio 集成，或与任何平台的 Visual Studio Code 集成。 有关详细信息，请参阅[集成 vcpkg](integrate-vcpkg.md)。

## <a name="update-vcpkg"></a>更新 vcpkg

vcpkg 包管理器在 GitHub 上定期更新。 若要将 vcpkg 的克隆更新到最新版本，请从 vcpkg 根目录运行 `git pull`。 此命令会将 vcpkg 的副本与 GitHub 上的版本同步。 下载完成后，再次运行引导程序。 引导程序会重新生成 vcpkg 程序，但保留已安装的库。

## <a name="uninstall-vcpkg"></a>卸载 vcpkg

若要卸载 vcpkg，只需删除 `vcpkg` 目录。 删除此目录会卸载 vcpkg 分发以及 vcpkg 已安装的所有库。

但是，如果已执行 `vcpkg integrate install`，则应执行 `vcpkg integrate remove` 来确保在删除文件夹之前已清理集成 。 有关详细信息，请参阅[集成 vcpkg](integrate-vcpkg.md)。

## <a name="see-also"></a>另请参阅

[vcpkg：用于 Windows、Linux 和 MacOS 的 C++ 包管理器](./vcpkg.md)\
[GitHub 上的 vcpkg](https://github.com/Microsoft/vcpkg)\
[集成 vcpkg](integrate-vcpkg.md)\
[使用 vcpkg 管理库](manage-libraries-with-vcpkg.md)\
[vcpkg 命令行参考](vcpkg-command-line-reference.md)\
[快速入门](https://github.com/microsoft/vcpkg/blob/master/docs/README.md)\
[常见问题解答](https://github.com/microsoft/vcpkg/blob/master/docs/about/faq.md)
