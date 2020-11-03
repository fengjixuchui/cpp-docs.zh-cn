---
title: 在 Visual Studio 中安装 C++ Linux 工作负载
description: 如何在 Visual Studio 中下载、安装和设置用于 C++ 的 Linux 工作负载。
ms.date: 05/03/2020
ms.assetid: e11b40b2-f3a4-4f06-b788-73334d58dfd9
ms.openlocfilehash: 1a1b42927d440d1cde847fd1c1b6593e87a3824a
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176275"
---
# <a name="download-install-and-set-up-the-linux-workload"></a>下载、安装和设置 Linux 工作负载

::: moniker range="vs-2015"

Visual Studio 2017 及更高版本支持 Linux 项目。 若要查看这些版本的文档，请将本文的 Visual Studio“版本”  选择器控件设置为 Visual Studio 2017 或 Visual Studio 2019。 它位于此页面上目录表的顶部。

::: moniker-end

::: moniker range=">=vs-2017"

可以使用 Windows 中的 Visual Studio IDE 来创建、编辑和调试在远程 Linux 系统、虚拟机或[适用于 Linux 的 Windows 子系统](/windows/wsl/about)上执行的 C++ 项目。

可处理使用 CMake 的现有基本代码，无需将其转换为 Visual Studio 项目。 如果基本代码为跨平台代码，则从 Visual Studio 中可同时面向 Windows 和 Linux。 例如，可以使用 Visual Studio 在 Windows 上编辑、生成和调试代码。 然后快速使项目重新面向 Linux 以在 Linux 环境中生成和调试。 Linux 头文件会自动复制到本地计算机。 Visual Studio 会使用这些文件提供完整 IntelliSense 支持（“语句完成”、“转到定义”等）。

对于任何这些方案，都必须拥有“使用 C++ 的 Linux 开发”工作负载  。

::: moniker-end

::: moniker range=">=vs-2017"

## <a name="visual-studio-setup"></a>Visual Studio 安装程序

1. 在 Windows 搜索框中键入“Visual Studio 安装程序”：

   ![Windows 搜索框](media/visual-studio-installer-search.png)

1. 在“应用”结果下寻找安装程序并双击它  。 打开该安装程序后，选择“修改”  ，然后单击“工作负荷”  选项卡。向下滚动到“其他工具集”  ，然后选择  “使用 C++ 的 Linux 开发”工作负荷。

   ![适用于 Linux 开发的 Visual C++ 工作负荷](media/linuxworkload.png)

1. 如果你面向的是 IoT 或嵌入式平台，请转到右侧的“安装详细信息”  窗格。 在“使用 C++ 的 Linux 开发”  下，展开“可选组件”  ，并选择所需的组件。 默认情况下，选择适用于 Linux 的 CMake 支持。

1. 单击“修改”  以继续进行安装。

## <a name="options-for-creating-a-linux-environment"></a>用于创建 Linux 环境的选项

如果还没有 Linux 计算机，则可以在 Azure 上创建 Linux 虚拟机。 有关详细信息，请参阅[快速入门：在 Azure 门户中创建 Linux 虚拟机](/azure/virtual-machines/linux/quick-create-portal)。

在 Windows 10 上，可以在适用于 Linux 的 Windows 子系统 (WSL) 上安装你最喜欢的 Linux 发行版以及使该发行版以之为目标。 有关详细信息，请参阅 [Windows 10 的适用于 Linux 的 Windows 子系统安装指南](/windows/wsl/install-win10)。 如果无法访问 Windows 应用商店，可以[手动下载 WSL 发行包](/windows/wsl/install-manual)。 WSL 是一种方便的控制台环境，但不建议用于图形应用程序。

::: moniker-end

::: moniker range="vs-2019"

Visual Studio 中的 Linux 项目要求在远程 Linux 系统或 WSL 上安装以下依赖项：

- 编译器  - Visual Studio 2019 为 GCC 和 [Clang](../build/clang-support-cmake.md) 提供完整支持。
- gdb  - Visual Studio 会在 Linux 系统上自动启动 gdb，并使用 Visual Studio 调试器的前端在 Linux 上提供完全保真的调试体验。
- **rsync** 和 zip  - 包含 rsync 和 zip 允许 Visual Studio 将头文件从 Linux 系统提取到 Windows 文件系统以供 IntelliSense 使用。
- **make**
- openssh-server  （仅适用于远程 Linux 系统）- Visual Studio 通过安全 SSH 连接以连接到远程 Linux 系统。
- **CMake** （仅 CMake 项目）- 可以 [为 Linux 安装 Microsoft 的静态链接的 CMake 二进制文件](https://github.com/microsoft/CMake/releases)。
- ninja-build  （仅 CMake 项目）- [Ninja](https://ninja-build.org/) 是 Visual Studio 2019 版本 16.6 或更高版本中适用于 Linux 和 WSL 配置的默认生成器。

以下命令假设你使用的是 g++ 而非 clang。

::: moniker-end

::: moniker range="vs-2017"

Visual Studio 中的 Linux 项目要求在远程 Linux 系统或 WSL 上安装以下依赖项：

- gcc  - Visual Studio 2017 为 GCC 提供完整支持。
- gdb  - Visual Studio 会在 Linux 系统上自动启动 gdb，并使用 Visual Studio 调试器的前端在 Linux 上提供完全保真的调试体验。
- rsync  和 zip  - 包含 rsync 和 zip 允许 Visual Studio 将头文件从 Linux 系统提取到 Windows 文件系统以供 IntelliSense 使用。
- **make**
- **openssh-server** - Visual Studio 通过安全 SSH 连接以连接到远程 Linux 系统。
- **CMake** （仅 CMake 项目）- 可以 [为 Linux 安装 Microsoft 的静态链接的 CMake 二进制文件](https://github.com/microsoft/CMake/releases)。

::: moniker-end

::: moniker range="vs-2019"

## <a name="linux-setup-ubuntu-on-wsl"></a>Linux 安装程序：WSL 上的 Ubuntu

以 WSL 为目标时，无需添加远程连接或配置 SSH 即可进行生成和调试。 使用 Visual Studio for Intellisense 支持自动同步 Linux 标头需要 zip 和 rsync   。 仅 CMake 项目需要 ninja-build  。 如果所需应用程序尚不存在，则可以使用以下命令进行安装：

```bash
sudo apt-get install g++ gdb make ninja-build rsync zip
```

::: moniker-end

::: moniker range=">=vs-2017"

## <a name="ubuntu-on-remote-linux-systems"></a>远程 Linux 系统上的 Ubuntu

目标 Linux 系统必须安装 openssh-server、g++、gdb 和 make     。 仅 CMake 项目需要 ninja-build  。 ssh  守护程序必须正在运行。 需要 zip  和 rsync  才能自动将远程标头与本地计算机同步以获得 Intellisense 支持。 如果这些应用程序尚不存在，则可以按如下所述进行安装：

1. 在 Linux 计算机上的 shell 提示符下，运行：

   ```bash
   sudo apt-get install openssh-server g++ gdb make ninja-build rsync zip
   ```

   可能会提示你输入 root 密码以运行 sudo 命令。 如果是这样，输入密码然后继续。 完成后，可安装所需服务和工具。

1. 通过运行以下命令，确保 ssh 服务在 Linux 计算机上运行：

   ```bash
   sudo service ssh start
   ```

   此命令将启动该服务并在后台运行它，准备接受连接。

::: moniker-end

::: moniker range="vs-2019"

## <a name="fedora-on-wsl"></a>WSL 上的 Fedora

Fedora 使用 dnf 包安装程序  。 要下载 g++、gdb、make、rsync、ninja-build 和 zip，请运行       ：

   ```bash
   sudo dnf install gcc-g++ gdb rsync ninja-build make zip
   ```

使用 Visual Studio for Intellisense 支持自动同步 Linux 标头需要 zip 和 rsync   。 仅 CMake 项目需要 ninja-build  。

::: moniker-end

::: moniker range=">=vs-2017"

## <a name="fedora-on-remote-linux-systems"></a>远程 Linux 系统上的 Fedora

运行 Fedora 的目标计算机使用 dnf 包安装程序  。 要下载 openssh-server、g++、gdb、make、ninja-build、rsync 和 zip 并重启 ssh 守护程序，请遵循以下说明        。 仅 CMake 项目需要 ninja-build  。

1. 在 Linux 计算机上的 shell 提示符下，运行：

   ```bash
   sudo dnf install openssh-server gcc-g++ gdb ninja-build make rsync zip
   ```

   可能会提示你输入 root 密码以运行 sudo 命令。 如果是这样，输入密码然后继续。 完成后，可安装所需服务和工具。

1. 通过运行以下命令，确保 ssh 服务在 Linux 计算机上运行：

   ```bash
   sudo systemctl start sshd
   ```

   此命令将启动该服务并在后台运行它，准备接受连接。

## <a name="next-steps"></a>后续步骤

现在，可以创建或打开 Linux 项目并将其配置为在目标系统上运行。 有关详细信息，请参见:

- [创建新的 Linux MSBuild C++ 项目](create-a-new-linux-project.md)
- [配置 Linux CMake 项目](cmake-linux-project.md)

::: moniker-end
