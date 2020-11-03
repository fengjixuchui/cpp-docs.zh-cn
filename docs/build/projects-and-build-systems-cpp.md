---
title: Visual Studio 中的 C/C++ 项目和生成系统
description: 使用 Visual Studio 基于任何项目系统编译和生成适用于 Windows、ARM 或 Linux 的 C++ 项目。
ms.date: 07/17/2019
helpviewer_keywords:
- builds [C++]
- C++ projects, building
- projects [C++], building
- builds [C++], options
- C++, build options
ms.assetid: fa6ed4ff-334a-4d99-b5e2-a1f83d2b3008
ms.topic: overview
ms.openlocfilehash: 193230ef393aa83d7ce4b9aec11a1fa2cb5052ce
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176068"
---
# <a name="cc-projects-and-build-systems-in-visual-studio"></a>Visual Studio 中的 C/C++ 项目和生成系统

可以使用 Visual Studio 编辑、编译和生成任何具有完整 IntelliSense 支持的 C++ 基本代码，而无需将该代码转换为 Visual Studio 项目或使用 MSVC 工具集进行编译。 例如，可以在 Windows 计算机上的 Visual Studio 中编辑跨平台 CMake 项目，然后在远程 Linux 计算机上使用 g++ 针对 Linux 编译该项目。

## <a name="c-compilation"></a>C++ 编译

生成  C++ 程序意味着要从一个或多个文件编译源代码，然后将这些文件链接到可执行文件 (.exe)、动态加载库 (.dll) 或静态库 (.lib) 中。

基本 C++ 编译涉及三个主要步骤：

- C++ 预处理器会转换每个源文件中的所有 #directives 和宏定义。 这会创建  翻译单元。
- C++ 编译器通过应用已设置的任何编译器选项，将每个翻译单元编译为对象文件 (.obj)。
- 链接器  通过应用已设置的链接器选项，将对象文件合并为单个可执行文件。

## <a name="the-msvc-toolset"></a>MSVC 工具集

Microsoft C++ 编译器、链接器、标准库和相关实用工具组成了 MSVC 编译器工具集（也称为工具链或“生成工具”）。 这些内容包含在 Visual Studio 中。 还可以从[Visual Studio 2019 生成工具下载](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)，以免费独立包的形式下载和使用工具集。

可以通过从命令行直接调用 MSVC 编译器 (cl.exe) 来生成简单程序。 以下命令接受单个源代码文件，并调用 cl.exe 以生成名为 hello.exe  的可执行文件：

```cmd
cl /EHsc hello.cpp
```

编译器 (cl.exe) 在此处会自动调用 C++ 预处理器和链接器以生成最终输出文件。 有关详细信息，请参阅[在命令行上生成](building-on-the-command-line.md)。

## <a name="build-systems-and-projects"></a>生成系统和项目

大多数实际程序使用某种类型的生成系统来管理针对多个配置（调试和发布）、多个平台（x86、x64、ARM 等）、自定义生成步骤甚至是必须按特定顺序编译的多个可执行文件编译多个源文件的复杂性。 可在生成配置文件中进行设置，生成系统会在调用编译器之前接受该文件作为输入。 生成可执行文件所需的一组源代码文件和生成配置文件称为项目  。

以下列表显示 Visual Studio 项目的各种选项 - C++：

- 使用 Visual Studio IDE 创建 Visual Studio 项目，并使用属性页进行配置。 Visual Studio 项目生成在 Windows 上运行的程序。 有关概述，请参阅 Visual Studio 文档中的[编译和生成](/visualstudio/ide/compiling-and-building-in-visual-studio)。

- 打开包含 CMakeLists.txt 文件的文件夹。 CMake 支持已集成到 Visual Studio 中。 可以使用 IDE 进行编辑、测试和调试，而无需以任何方式修改 CMake 文件。 这使你可以与可能使用不同编辑器的其他人一起处理相同的 CMake 项目。 CMake 是用于跨平台开发的建议方法。 有关详细信息，请参阅 [CMake 项目](cmake-projects-in-visual-studio.md)。

- 打开源文件的松散文件夹（不包含任何项目文件）。 Visual Studio 将使用启发式生成文件。 这是一种用于编译和运行小型控制台应用程序的方法。 有关详细信息，请参阅[“打开文件夹”项目](open-folder-projects-cpp.md)。

- 打开包含生成文件的文件夹或任何其他生成系统配置文件。 可以通过将 JSON 文件添加到文件夹中，将 Visual Studio 配置为调用任意生成命令。 有关详细信息，请参阅[“打开文件夹”项目](open-folder-projects-cpp.md)。

- 在 Visual Studio 中，打开 Windows 生成文件。 有关详细信息，请参阅 [NMAKE 参考](reference/nmake-reference.md)。

## <a name="msbuild-from-the-command-line"></a>命令行中的 MSBuild

可以通过向 MSBuild 传递 .vcxproj 文件以及命令行选项，从命令行调用它。 此方法需要充分了解 MSBuild，建议仅在必要时才使用。 有关详细信息，请参阅 [MSBuild](msbuild-visual-cpp.md)。

## <a name="in-this-section"></a>本节内容

[Visual Studio 项目](creating-and-managing-visual-cpp-projects.md)\
如何使用其本机生成系统 (MSBuild) 在 Visual Studio 中创建、配置和生成 C++ 项目。

[CMake 项目](cmake-projects-in-visual-studio.md)\
如何在 Visual Studio 中编码、生成和部署 CMake 项目。

[打开文件夹项目](open-folder-projects-cpp.md)\
如何使用 Visual Studio 基于任意生成系统或完全不基于任何生成系统来编码、生成和部署 C++ 项目。

[发行版本](release-builds.md)\
如何创建优化发行版本并进行故障排除以部署到最终用户。

[通过命令行使用 MSVC 工具集](building-on-the-command-line.md)\
讨论如何直接从命令行使用 C/C++ 编译器和生成工具（而不是使用 Visual Studio IDE）。

[在 Visual Studio 中生成 DLL](dlls-in-visual-cpp.md)\
如何在 Visual Studio 中创建、调试和部署 C/C++ DLL（共享库）。

[演练：创建和使用静态库](walkthrough-creating-and-using-a-static-library-cpp.md)\
如何创建 .lib 二进制文件。

[生成 C/C++ 独立应用程序和并行程序集](building-c-cpp-isolated-applications-and-side-by-side-assemblies.md)\
描述了适用于 Windows 桌面应用程序的部署模型，这一模型的理论基础是独立应用程序和并行程序集。

[针对 64 位 x64 目标配置 C++ 项目](configuring-programs-for-64-bit-visual-cpp.md)\
如何使用 MSVC 生成工具来面向 64 位 x64 硬件。

[将 C++ 项目配置为可用于 ARM 处理器](configuring-programs-for-arm-processors-visual-cpp.md)\
如何使用 MSVC 生成工具来面向 ARM 硬件。

[优化代码](optimizing-your-code.md)\
如何以各种方式优化代码，包括按程序优化。

[配置适用于 Windows XP 的程序](configuring-programs-for-windows-xp.md)\
如何使用 MSVC 生成工具来面向 Windows XP。

[C/C++ 生成参考](reference/c-cpp-building-reference.md)\
提供指向有关使用 C++ 生成程序、编译器和链接器选项以及各种生成工具的参考文章的链接。
