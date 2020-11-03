---
title: 在 Visual Studio 中配置 Linux CMake 项目
description: 如何在 Visual Studio 中配置 Linux CMake 设置
ms.date: 08/08/2020
ms.openlocfilehash: 32c851791402b59c941ae088fa637d3d9953dd1b
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91504729"
---
# <a name="configure-a-linux-cmake-project-in-visual-studio"></a>在 Visual Studio 中配置 Linux CMake 项目

::: moniker range="vs-2015"
Linux 支持在 Visual Studio 2017 及更高版本中提供。 若要查看这些版本的文档，请将目录上方的“版本”下拉列表设置为“Visual Studio 2017”或“Visual Studio 2019”。
::: moniker-end

::: moniker range=">=vs-2017"
本主题介绍如何将 Linux 配置添加到面向远程 Linux 系统或适用于 Linux 的 Windows 子系统 (WSL) 的 CMake 项目。 它将延续从[在 Visual Studio 中创建 Linux CMake 项目](cmake-linux-project.md)开始的系列文章。 如果使用的是 MSBuild，请参阅[在 Visual Studio 中配置 Linux MSBuild 项目](configure-a-linux-project.md)

## <a name="add-a-linux-configuration"></a>添加 Linux 配置

配置可用于针对具有相同源代码的不同平台（Windows、WSL、远程系统）。 配置还可用于设置编译器、传递环境变量以及自定义 CMake 的调用方式。 CMakeSettings.json 文件指定在[自定义 CMake 设置](../build/customize-cmake-settings.md)中列出的部分或所有属性，以及控制远程 Linux 计算机上的生成设置的其他属性。
::: moniker-end

::: moniker range="vs-2017"
若要在 Visual Studio 2017 中更改默认 CMake 设置，请从主菜单依次选择“CMake” > “更改 CMake 设置” > “CMakeLists.txt”。 或右键单击“解决方案资源管理器”中的“CMakeLists.txt”，然后选择“更改 CMake 设置”。 然后，Visual Studio 会在根项目文件夹中创建一个新的 CMakeSettings.json 文件。 若要进行更改，请打开文件并直接修改。 有关详细信息，请参阅[自定义 CMake 设置](../build/customize-cmake-settings.md)。

Visual Studio 2017（以及 Visual Studio 2019 版本 16.0）中 Linux-Debug 的默认配置如下所示：

```json
{
      "name": "Linux-Debug",
      "generator": "Unix Makefiles",
      "remoteMachineName": "${defaultRemoteMachineName}",
      "configurationType": "Debug",
      "remoteCMakeListsRoot": "/var/tmp/src/${workspaceHash}/${name}",
      "cmakeExecutable": "/usr/local/bin/cmake",
      "buildRoot": "${env.LOCALAPPDATA}\\CMakeBuilds\\${workspaceHash}\\build\\${name}",
      "installRoot": "${env.LOCALAPPDATA}\\CMakeBuilds\\${workspaceHash}\\install\\${name}",
      "remoteBuildRoot": "/var/tmp/build/${workspaceHash}/build/${name}",
      "remoteInstallRoot": "/var/tmp/build/${workspaceHash}/install/${name}",
      "remoteCopySources": true,
      "remoteCopySourcesOutputVerbosity": "Normal",
      "remoteCopySourcesConcurrentCopies": "10",
      "remoteCopySourcesMethod": "rsync",
      "remoteCopySourcesExclusionList": [".vs", ".git"],
      "rsyncCommandArgs" : "-t --delete --delete-excluded",
      "remoteCopyBuildOutput" : "false",
      "cmakeCommandArgs": "",
      "buildCommandArgs": "",
      "ctestCommandArgs": "",
      "inheritEnvironments": [ "linux-x64" ]
}
```

::: moniker-end

::: moniker range="vs-2019"
若要在 Visual Studio 2019 中更改默认 CMake 设置，请在主工具栏中打开“配置”下拉列表，然后选择“管理配置” 。

![CMake 管理配置](../build/media/vs2019-cmake-manage-configurations.png "CMake 配置下拉列表")

此命令将打开“CMake 设置编辑器”，可使用它来编辑根项目文件夹中的 CMakeSettings.json 文件。 也可以通过单击编辑器中的“编辑 JSON”按钮，使用 JSON 编辑器打开文件。 有关详细信息，请参阅[自定义 CMake 设置](../build/customize-cmake-settings.md)。

Visual Studio 2019 版本 16.1 中的默认 Linux 调试配置如下所示：

```json
{
      "name": "Linux-Debug",
      "generator": "Unix Makefiles",
      "configurationType": "Debug",
      "cmakeExecutable": "/usr/bin/cmake",
      "remoteCopySourcesExclusionList": [ ".vs", ".git", "out" ],
      "cmakeCommandArgs": "",
      "buildCommandArgs": "",
      "ctestCommandArgs": "",
      "inheritEnvironments": [ "linux_x64" ],
      "remoteMachineName": "${defaultRemoteMachineName}",
      "remoteCMakeListsRoot": "$HOME/.vs/${projectDirName}/${workspaceHash}/src",
      "remoteBuildRoot": "$HOME/.vs/${projectDirName}/${workspaceHash}/out/build/${name}",
      "remoteInstallRoot": "$HOME/.vs/${projectDirName}/${workspaceHash}/out/install/${name}",
      "remoteCopySources": true,
      "rsyncCommandArgs": "-t --delete --delete-excluded",
      "remoteCopyBuildOutput": false,
      "remoteCopySourcesMethod": "rsync",
      "addressSanitizerRuntimeFlags": "detect_leaks=0",
      "variables": []
    }
  ]
}
```

在 Visual Studio 2019 版本 16.6 或更高版本中，Ninja 是针对远程系统或 WSL 的配置的默认生成器。 有关详细信息，请在 [C++ 团队博客](https://devblogs.microsoft.com/cppblog/linux-development-with-visual-studio-first-class-support-for-gdbserver-improved-build-times-with-ninja-and-updates-to-the-connection-manager/)中阅读本文。

::: moniker-end
::: moniker range=">=vs-2017"
有关这些设置的详细信息，请参阅 [CMakeSettings.json 引用](../build/cmakesettings-reference.md)。

执行生成时：

- 如果以远程系统为目标，Visual Studio 将为远程目标默认选择“工具”>“选项”>“跨平台”>“连接管理器”下列表中的第一个远程系统。
- 如果未找到任何远程连接，那么系统会提示你进行创建。 有关详细信息，请参阅[连接到远程 Linux 计算机](connect-to-your-remote-linux-computer.md)。

## <a name="choose-a-linux-target"></a>选择 Linux 目标

打开 CMake 项目文件夹时，Visual Studio 会分析 CMakeLists.txt 文件，并指定 Windows 目标“x86-Debug”。 要以远程 Linux 系统为目标，请根据 Linux 编译器更改项目设置。 例如，如果在 Linux 上使用 GCC 并使用调试信息进行编译，则选择：Linux-GCC-Debug 或 Linux-GCC-Release。

如果指定远程 Linux 目标，则会将源复制到远程系统。

选择目标后，CMake 会在 Linux 系统上自动运行，以便为项目生成 CMake 缓存：

![在 Linux 上生成 CMake 缓存](media/cmake-linux-1.png "在 Linux 上生成 CMake 缓存")

::: moniker-end
::: moniker range="vs-2019"

### <a name="target-windows-subsystem-for-linux"></a>面向适用于 Linux 的 Windows 子系统

如果以适用于 Linux 的 Windows 子系统 (WSL) 为目标，则无需添加远程连接。

若要以 WSL 为目标，请选择主工具栏配置下拉列表中的“管理配置”：

![CMake 管理配置](../build/media/vs2019-cmake-manage-configurations.png "CMake 配置下拉列表")

此时将显示“CMakeSettings.json”窗口。

![添加配置](media/cmake-linux-configurations.png "向 CMake 设置中添加配置")

按“添加配置”（绿色的 "+" 按钮），然后选择“Linux-GCC-Debug”或（如果使用 GCC）“Linux-GCC-Release”。 如果使用 Clang/LLVM 工具集，则选择 Clang 变体。  按“选择”，然后按 Ctrl+S 以保存配置。

Visual Studio 2019 版本 16.1 以 WSL 为目标时，Visual Studio 不需要复制源文件和维护生成树的两个同步副本，因为 Linux 上的编译器可以直接访问已装入的 Windows 文件系统中的源文件。
::: moniker-end
::: moniker range=">=vs-2017"

### <a name="intellisense"></a>IntelliSense

准确的 C++ IntelliSense 需要访问 C++ 源文件所引用的 C++ 标头。 从 Linux 到 Windows，Visual Studio 会自动使用 CMake 项目引用的标头信息来提供完全保真的 IntelliSense 体验。 有关详细信息，请参阅[远程标头的 IntelliSense](configure-a-linux-project.md#remote_intellisense)。

### <a name="locale-setting"></a>区域设置

Visual Studio 语言设置不会传播到 Linux 目标，因为 Visual Studio 不会管理和配置已安装的包。 “输出”窗口中显示是消息（例如生成错误）是采用 Linux 目标的语言的区域设置显示的。 你需要为所需区域设置配置 Linux 目标。

## <a name="additional-settings"></a>其他设置

使用以下设置，在生成前后以及 CMake 生成前在 Linux 系统中运行命令。 值可以是远程系统上的任何有效命令。 输出通过管道传递回 Visual Studio。

```json
{
      "remotePrebuildCommand": "",
      "remotePreGenerateCommand": "",
      "remotePostbuildCommand": "",
}
```

## <a name="next-steps"></a>后续步骤

[配置 CMake 调试会话](../build/configure-cmake-debugging-sessions.md?toc=/cpp/linux/toc.json&bc=/cpp/_breadcrumb/toc.json)

## <a name="see-also"></a>另请参阅

[使用项目属性](../build/working-with-project-properties.md)<br/>
[自定义 CMake 设置](../build/customize-cmake-settings.md)<br/>
[CMake 预定义配置引用](../build/cmake-predefined-configuration-reference.md)

::: moniker-end
