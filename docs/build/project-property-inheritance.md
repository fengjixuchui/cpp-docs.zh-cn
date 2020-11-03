---
title: Visual Studio 项目中的属性继承 - C++
description: 属性在本机 (MSBuild) Visual Studio C++ 项目中的继承方式。
ms.date: 02/21/2020
helpviewer_keywords:
- C++ projects, property inheritance
ms.openlocfilehash: 00afe982156597aa166c2c5de98f3027e3f84bdb
ms.sourcegitcommit: 6e5429e076e552b32e8bdc49480c51498d7924c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2020
ms.locfileid: "92099701"
---
# <a name="property-inheritance-in-visual-studio-projects"></a>Visual Studio 项目中的属性继承

Visual Studio 本机项目系统基于 MSBuild。 MSBuild 定义用于生成任何类型项目的文件格式和规则。 它还管理生成多个配置和平台过程中的大部分复杂性。 你会发现，了解它的工作原理很有用。 如果要定义自定义配置，这一点尤其重要。 或者，创建可重用的属性集，以便共享和导入到多个项目中。

## <a name="the-vcxproj-file-props-files-and-targets-files"></a>.vcxproj 文件、.props 文件和 .targets 文件

::: moniker range="vs-2015"

项目属性存储在多个文件中。 某些属性直接存储在 `.vcxproj` 项目文件中。 其他属性来自项目文件导入的其他 `.targets` 或 `.props` 文件（提供默认值） 。 可在基本目录 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\v140` 下特定于区域设置的文件夹中找到 Visual Studio 2015 项目文件。

::: moniker-end

::: moniker range="vs-2017"

项目属性存储在多个文件中。 某些属性直接存储在 `.vcxproj` 项目文件中。 其他属性来自项目文件导入的其他 `.targets` 或 `.props` 文件（提供默认值） 。 可在基本目录 `%VSINSTALLDIR%Common7\IDE\VC\VCTargets\` 下特定于区域设置的文件夹中找到 Visual Studio 2017 项目文件。

::: moniker-end

::: moniker range=">=vs-2019"

项目属性存储在多个文件中。 某些属性直接存储在 `.vcxproj` 项目文件中。 其他属性来自项目文件导入的其他 `.targets` 或 `.props` 文件（提供默认值） 。 可在基本目录 `%VSINSTALLDIR%MSBuild\Microsoft\VC\<version>` 下特定于区域设置的文件夹中找到 Visual Studio 项目文件。 `<version>` 特定于 Visual Studio 的版本。 对于 Visual Studio 2019，它是 `v160`。

::: moniker-end

属性也存储在你可能想要添加到自己项目的任何自定义 `.props` 文件中  。 强烈建议不要手动编辑这些文件。 而是使用 IDE 中的属性页来更改所有属性，特别是参与继承的属性，除非你深入了解 MSBuild 和 `.vcxproj` 文件。

如前面所示，相同配置的同一属性在不同文件中可能分配有不同值。 生成项目时，MSBuild 引擎以明确定义的顺序评估项目文件和所有导入的文件（稍后将介绍）。 在评估每个文件时，该文件中定义的任何属性值都将覆盖现有值。 未指定的任何值都从之前已评估的文件继承。 使用属性页设置属性时，也请务必注意设置它的位置。 如果在 `.props` 文件中将属性设置为“X”，但该属性在项目文件中已设为“Y”，然后将在属性设为“Y”的情况下生成项目  。 如果在项目项（例如 `.cpp` 文件）上，相同的属性已设为“Z”，然后 MSBuild 引擎将使用“Z”值  。

这是基本的继承树：

1. MSBuild CPP 工具集中的默认设置（基本目录中的 `Microsoft.Cpp.Default.props` 文件，由 `.vcxproj` 文件导入。 ）

1. 属性表

1. `.vcxproj` 文件  。 （此文件可能重写默认设置和属性页设置。）

1. 项元数据

> [!TIP]
> 在属性页中，bold 的属性在当前上下文中定义  。 普通字体的属性将被继承。

## <a name="view-an-expanded-project-file-with-all-imported-values"></a>查看包含所有导入值的已扩展项目文件

有时，通过查看展开的文件来确定给定的属性值如何继承非常有用。 若要查看扩展版本，请在 Visual Studio 命令提示中输入以下命令。 （将占位符文件名称更改为要使用的名称。）

> msbuild /pp: _temp_ .txt _myapp_ .vcxproj   

除非你十分熟悉 MSBuild，否则展开的项目文件可能会很大并且难以理解。 这是项目文件的基本结构：

1. 基本项目属性，不在 IDE 中显示。

1. 导入 `Microsoft.cpp.default.props`，该文件定义了一些基本的、不依赖于工具集的属性  。

1. 全局配置属性在“常规配置”页面上显示为“PlatformToolset”和“项目”默认属性    。 这些属性决定下一步将哪个工具集和内部属性表导入 `Microsoft.cpp.props` 中  。

1. 导入 `Microsoft.cpp.props`，该文件设置大多数项目默认值  。

1. 导入所有属性表，包括 `.user` 文件  。 这些属性表可以重写内容，但“PlatformToolset”和“项目”默认属性除外   。

1. 项目配置属性的其余部分。 这些值可以重写属性表中设置的内容。

1. 项（文件）及其元数据。 这些项始终是 MSBuild 评估规则的最后一项，即使它们出现在其他属性和导入之前仍是如此。

有关详细信息，请参阅 [MSBuild 属性](/visualstudio/msbuild/msbuild-properties)。

## <a name="build-configurations"></a>生成配置

配置只是给定名称的任意一组属性。 Visual Studio 提供“调试”和“发布”配置。 每个配置都会为调试版本或发布版本适当地设置各种属性。 可以使用“配置管理器”来定义自定义配置  。 它们是一种方便的方法，可以为特定风格的版本对属性进行分组。

要更好地了解生成配置，请打开“属性管理器”  。 你可以通过选择“视图”>“属性管理器”或“视图”>“其他窗口”>“属性管理器”来打开它，具体取决于你的设置   。 “属性管理器”具有项目中每个配置和平台对的节点  。 这些节点的每个节点下都是为该配置设置某些特定属性的属性表（`.props` 文件）的节点  。

![属性管理器](media/property-manager.png "“属性管理器”")

例如，可以转到属性页中的“常规”窗格。 将字符集属性更改为“未设置”而不是“使用 Unicode”，然后单击“确定”  。 属性管理器现在不显示“Unicode 支持”属性表  。 对于当前配置，它已被删除，但对于其他配置，它仍然存在。

有关属性管理器和属性表的更多信息，请参阅[共享或重用 Visual Studio C++ 项目设置](create-reusable-property-configurations.md)。

> [!TIP]
> `.user` 文件是一项旧功能  。 建议将其删除，以便根据配置和平台正确地对属性分组。
