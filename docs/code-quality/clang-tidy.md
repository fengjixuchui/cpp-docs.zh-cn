---
title: 在 Visual Studio 中使用 Clang-Tidy
description: 如何使用 Visual Studio 中的 Clang-Tidy 进行 Microsoft c + + 代码分析。
ms.date: 02/19/2020
ms.topic: conceptual
f1_keywords:
- vs.codeanalysis.clangtidy
ms.openlocfilehash: 5b2da222caea435bdb24d774b5e93c995e734bb7
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92919067"
---
# <a name="using-clang-tidy-in-visual-studio"></a>在 Visual Studio 中使用 Clang-Tidy

::: moniker range="<=msvc-150"

支持 Clang-Tidy 需要 Visual Studio 2019 版本16.4 或更高版本。 若要查看此版本对应的文档，请将本文的 Visual Studio“版本”选择器控件设置为“Visual Studio 2019”。 它位于此页面上目录表的顶部。

::: moniker-end

::: moniker range=">=msvc-160"

无论是使用 Clang 还是 MSVC 工具集，代码分析都以本机 [方式支持 Clang](https://clang.llvm.org/extra/clang-tidy/) 和 CMake 项目。 Clang-Tidy 检查可以作为后台代码分析的一部分运行。 它们显示为编辑器内警告 (波形曲线) ，并显示在错误列表中。

从 Visual Studio 2019 16.4 版开始提供 Clang-Tidy 支持。 在 Visual Studio 安装程序中选择 c + + 工作负荷时，它会自动包含在内。

Clang-Tidy 是使用 LLVM/clang 工具集时的默认分析工具，可在 MSBuild 和 CMake 中使用。 可以在使用 MSVC 工具集运行时对其进行配置，也可以替换标准代码分析体验。 如果使用 clang-cl 工具集，则 Microsoft 代码分析不可用。

Clang-Tidy 在成功编译后运行。 可能需要解决源代码错误才能获得 Clang-Tidy 结果。

## <a name="msbuild"></a>MSBuild

您可以将 Clang-Tidy 配置为作为代码分析的一部分运行，并在项目属性窗口的 " **代码分析** " "  >  **常规** " 页下生成。 用于配置工具的选项可在 "Clang-Tidy" 子菜单中找到。

有关详细信息，请参阅 [如何：设置 C/c + + 项目的代码分析属性](../code-quality/how-to-set-code-analysis-properties-for-c-cpp-projects.md)。

## <a name="cmake"></a>CMake

在 CMake 项目中，您可以在中配置 Clang-Tidy 检查 `CMakeSettings.json` 。 打开后，在 "CMake 项目设置" 编辑器的右上角选择 "编辑 JSON"。 识别以下密钥：

- `enableMicrosoftCodeAnalysis`：启用 Microsoft 代码分析
- `enableClangTidyCodeAnalysis`：启用 Clang-Tidy 分析
- `clangTidyChecks`： Clang-Tidy 配置，指定为以逗号分隔的列表，即要启用或禁用的检查

如果未指定任何 "启用" 选项，则 Visual Studio 将选择与所使用的平台工具集匹配的分析工具。

## <a name="warning-display"></a>警告显示

Clang-Tidy 在错误列表中显示警告，并在代码相关部分下显示为波形曲线编辑器中的警告。 使用错误列表中的 "Category" 列对 Clang-Tidy 警告进行排序和组织。 可以通过在 " **工具** 选项" 下切换 "禁用代码分析波形曲线" 设置来配置编辑器内警告  >  **Options** 。

## <a name="clang-tidy-configuration"></a>Clang-Tidy 配置

您可以通过 " **Clang 检查** " 选项配置检查 clang 是否在 Visual Studio 中运行。 此输入提供给该 **`--checks`** 工具的参数。 可在自定义文件中包含任何进一步的配置 *`.clang-tidy`* 。 有关详细信息，请参阅 [LLVM.org 上的 Clang 文档](https://clang.llvm.org/extra/clang-tidy/)。

## <a name="see-also"></a>请参阅

- [Clang/LLVM 对 MSBuild 项目的支持](https://devblogs.microsoft.com/cppblog/clang-llvm-support-for-msbuild-projects/)
- [CMake 项目的 Clang/LLVM 支持](https://devblogs.microsoft.com/cppblog/visual-studio-cmake-support-clang-llvm-cmake-3-14-vcpkg-and-performance-improvements/)

::: moniker-end
