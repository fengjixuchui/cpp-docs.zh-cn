---
title: “自定义生成步骤”属性页：常规
description: 本文介绍在 "属性页" 对话框的 "自定义生成步骤" 页上可用的属性。
ms.date: 10/27/2020
f1_keywords:
- VC.Project.VCCustomBuildStep.AdditionalInputs
- VC.Project.VCCustomBuildStep.CustomBuildAfterTargets
- VC.Project.VCCustomBuildStep.CustomBuildBeforeTargets
- VC.Project.VCCustomBuildStep.Outputs
- VC.Project.VCCustomBuildStep.Message
- VC.Project.VCCustomBuildStep.Command
helpviewer_keywords:
- project properties, custom build step
- custom build step (general)
ms.assetid: bd319741-0491-46c4-a428-7c61b4b46a02
ms.openlocfilehash: 53f2deef931821981b3301f44ba37660975fb811
ms.sourcegitcommit: 9c801a43ee0d4d84956b03fd387716c818705e0d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907579"
---
# <a name="custom-build-step-property-page-general"></a>“自定义生成步骤”属性页：常规

对于项目中的每个项目配置和目标平台组合，你可以指定在生成项目时执行的自定义步骤。

有关此页的 Linux 版本，请参阅[自定义生成步骤属性 (Linux C++)](../../linux/prop-pages/custom-build-step-linux.md)。

## <a name="general-page"></a>“常规”页

- **命令行**

   将由自定义生成步骤执行的命令。

- 说明

   自定义生成步骤运行时显示的消息。

- **输出**

   自定义生成步骤生成的输出文件。 需要该设置，以使增量生成正确执行。

- **附加依赖项**

   用于自定义生成步骤的任何其他输入文件的以分号分隔的列表。

- **在以下操作之后执行和在以下操作之前执行**

   这些选项定义相对于列出的目标，何时在生成过程中运行自定义生成步骤。 最常列出的目标是 `BuildGenerateSources` 、 `BuildCompile` 和 `BuildLink` ，因为它们表示生成过程中的主要步骤。 其他经常列出的目标为 `Midl` 、 `CLCompile` 和 `Link` 。

- **将输出视为内容**

   此选项仅对通用 Windows 平台或 Windows Phone 应用有意义，其中包含包中的所有内容文件 *`.appx`* 。

### <a name="to-specify-a-custom-build-step"></a>指定自定义生成步骤

1. 在菜单栏上，选择“项目” > “属性” 。 有关详细信息，请参阅[在 Visual Studio 中设置 C++ 编译器和生成属性](../working-with-project-properties.md)。

1. 在 " **属性页** " 对话框中，导航到 " **配置属性** " "  >  **自定义生成步骤** " "  >  **常规** " 页。

1. 修改设置。

## <a name="see-also"></a>请参阅

[C++ 项目属性页参考](property-pages-visual-cpp.md)
