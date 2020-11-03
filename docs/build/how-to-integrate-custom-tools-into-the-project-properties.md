---
title: 如何：将自定义工具集成到项目属性中
description: 如何将自定义工具集成到 Visual Studio C++ 项目中的项目属性。
ms.date: 10/08/2020
helpviewer_keywords:
- 'MSBuild (C++), howto: integrate custom tools'
ms.openlocfilehash: 4b88bf94a92efaf5046fd83e5c6358f3fdf80895
ms.sourcegitcommit: 6e5429e076e552b32e8bdc49480c51498d7924c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2020
ms.locfileid: "92099662"
---
# <a name="how-to-integrate-custom-tools-into-the-project-properties"></a>如何：将自定义工具集成到项目属性中

可通过创建 XML 文件，将自定义工具选项添加到 Visual Studio“属性页”。

“属性页”窗口的“配置属性”部分会显示名为“规则”的设置组 。 每个规则都包含一个工具或一组功能的设置。 例如，“链接器”规则包含链接器工具的设置  。 规则中的设置可以细分为各种类别  。

你可以创建一个包含自定义工具的属性的规则文件，以便在 Visual Studio 启动时加载这些属性。 有关如何修改该文件的信息，请参阅 Visual Studio 项目团队博客上的[平台可扩展性第 2 部分](/archive/blogs/vsproject/platform-extensibility-part-2)。

::: moniker range="vs-2015"

用于放置规则文件的文件夹取决于所使用的 Visual Studio 的区域设置和版本。 在 Visual Studio 2015 或更早版本的开发人员命令提示符中，规则文件夹为 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\<version>\<locale>`。 在 Visual Studio 2015 中，`<version>` 值为 `v140`。 `<locale>` 为 LCID，例如，用于英语的 `1033`。 对于安装的每个 Visual Studio 版本和每种语言，将使用不同的路径。 例如，Visual Studio 2015 Community 英文版的默认规则文件夹路径可能是 `C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\v140\1033\`。

::: moniker-end

::: moniker range="vs-2017"

用于放置规则文件的文件夹取决于所使用的 Visual Studio 的区域设置和版本。 在 Visual Studio 2017 的开发人员命令提示符中，规则文件夹为 `%VSINSTALLDIR%Common7\IDE\VC\VCTargets\<locale>\`。 `<locale>` 为 LCID，例如，用于英语的 `1033`。 在 Visual Studio 2015 或更早版本的开发人员命令提示符中，规则文件夹为 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\<version>\<locale>\`，其中 `<version>` 值是 Visual Studio 2015 中的 `v140` 。 对于安装的每个 Visual Studio 版本和每种语言，将使用不同的路径。 例如，Visual Studio 2017 Community 英文版的默认规则文件夹路径可能是 `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\VC\VCTargets\1033\`。

::: moniker-end

::: moniker range=">=vs-2019"

用于放置规则文件的文件夹取决于所使用的 Visual Studio 的区域设置和版本。 在 Visual Studio 2019 或更高版本的开发人员命令提示符中，规则文件夹为 `%VSINSTALLDIR%MSBuild\Microsoft\VC\<version>\<locale>\`，其中 `<version>` 值是 Visual Studio 2019 中的 `v160` 。 `<locale>` 为 LCID，例如，用于英语的 `1033`。 在 Visual Studio 2017 中，规则文件夹为 `%VSINSTALLDIR%Common7\IDE\VC\VCTargets\<locale>\`。 在 Visual Studio 2015 或更早版本的开发人员命令提示符中，规则文件夹为 `%ProgramFiles(x86)%\MSBuild\Microsoft.Cpp\v4.0\<version>\<locale>\`。 对于安装的每个 Visual Studio 版本和每种语言，将使用不同的路径。 例如，Visual Studio 2019 Community 英文版的默认规则文件夹路径可能是 `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Microsoft\VC\v160\1033\`。

::: moniker-end

### <a name="to-add-or-change-project-properties"></a>添加或更改项目属性

1. 在 XML 编辑器中，创建 XML 文件。

1. 在默认规则文件夹中保存该文件。 调整语言和 Visual Studio 版本的路径。 “属性页”窗口中的每个规则都由此文件夹中的 XML 文件表示  。 请确保该文件在文件夹中具有唯一的名称。

1. 复制现有规则文件夹的内容，例如 `rc.xml`，不保存更改直接关闭，然后将内容粘贴在新的 XML 文件中。 可以复制任何 XML 架构文件以用作模板。 选择与工具类似的一个。

1. 在新的 XML 文件中，根据要求修改内容。 请确保更改文件顶部的“规则名称”和“Rule.DisplayName”   。

1. 保存更改并关闭该文件。

1. 启动 Visual Studio 时，会加载规则文件夹中的 XML 文件。 若要测试新文件，请重启 Visual Studio。

1. 在“解决方案资源管理器”中，右键单击项目，然后选择“属性” 。 在“属性页”窗口中，验证是否存在名称为“规则”的新节点。

## <a name="see-also"></a>请参阅

[命令行上的 MSBuild - C++](msbuild-visual-cpp.md)
