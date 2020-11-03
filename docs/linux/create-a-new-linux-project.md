---
title: 在 Visual Studio 中创建 Linux MSBuild C++ 项目
ms.date: 10/15/2020
description: 在 Visual Studio 中新建一个基于 MSBuild 的 Linux 项目。
ms.assetid: 5d7c1d67-bc31-4f96-8622-2b4cf91372fd
ms.openlocfilehash: dddb9dbdee4cefc58a2bd5ccefa493b25e1c9f24
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176305"
---
# <a name="create-a-linux-msbuild-c-project-in-visual-studio"></a>在 Visual Studio 中创建 Linux MSBuild C++ 项目

::: moniker range="vs-2015"

Linux 项目在 Visual Studio 2017 及更高版本中提供。

::: moniker-end

::: moniker range="vs-2017"

首先，请确保已安装用于 Visual Studio 的 Linux 开发工作负荷  。 有关详细信息，请参阅[下载、安装和设置 Linux 工作负荷](download-install-and-setup-the-linux-development-workload.md)。

对于跨平台编译，我们建议使用 CMake。 在 Visual Studio 2019 中，CMake 的支持更为完整。 如果不选用 CMake，并且你有一个要扩展以针对 Linux 编译的现有 Windows Visual Studio 解决方案，则可以向该 Windows 解决方案中添加一个 Visual Studio Linux 项目和一个“共享项”项目。 将两个平台之间共享的代码置于该“共享项”项目中，并从 Windows 和 Linux 项目添加对该项目的引用。

## <a name="to-create-a-new-linux-project"></a>新建 Linux 项目

如需在 Visual Studio 2017 中创建新的 Linux 项目，请执行以下步骤：

1. 在 Visual Studio 中选择“ **文件 > 新建项目** ”，或按 **Ctrl + Shift + N** 。
1. 依次选择“Visual C++”>“跨平台”>“Linux”节点，然后选择要创建的项目类型  。 输入“名称”和“位置”，然后选择“确定”    。

   ![显示已选中“Visual C++”>“跨平台”>“Linux”，已调用所有项目类型且已调用“名称”和“位置”文本框的“新建项目”对话框的屏幕截图。](media/newproject.png)

   | 项目类型 | 描述 |
   | ------------ | --- |
   | **Blink (Raspberry)** | 面向 Raspberry Pi 设备的项目，附带让 LED 闪烁的示例代码 |
   | **控制台应用程序 (Linux)** | 面向任何 Linux 计算机的项目，附带将文本输出到控制台的示例代码 |
   | **空项目 (Linux)** | 面向任何 Linux 计算机的项目，不带任何示例代码 |
   | **生成文件项目 (Linux)** | 面向任何 Linux 计算机的项目，使用标准生成文件生成系统生成 |

## <a name="next-steps"></a>后续步骤

[配置 MSBuild Linux 项目](configure-a-linux-project.md)

::: moniker-end

::: moniker range="vs-2019"

首先，请确保已安装用于 Visual Studio 的 Linux 开发工作负荷  。 有关详细信息，请参阅[下载、安装和设置 Linux 工作负荷](download-install-and-setup-the-linux-development-workload.md)。

在 Visual Studio 中创建适用于 Linux 的新 C++ 项目时，可以选择创建 Visual Studio 项目或 CMake 项目。 本文介绍如何创建 Visual Studio 项目。 通常情况下，对于可能包括开源代码的新项目或要进行跨平台开发的编译，建议你将 CMake 与 Visual Studio 配合使用。 使用 CMake 项目，你可以在 Windows 和 Linux 上生成和调试同一个项目。 有关详细信息，请参阅[创建和配置 Linux CMake 项目](cmake-linux-project.md)。

如果你有要扩展以针对 Linux 编译的现有 Windows Visual Studio 解决方案，并且不选用 CMake，则可以向 Windows 解决方案添加一个 Visual Studio Linux 项目和一个“共享项”项目。 将两个平台之间共享的代码置于该“共享项”项目中，并从 Windows 和 Linux 项目添加对该项目的引用。

## <a name="create-a-new-linux-project"></a>新建 Linux 项目

如需在 Visual Studio 2019 中创建新的 Linux 项目，请执行以下步骤：

1. 在 Visual Studio 中选择“ **文件 > 新建项目** ”，或按 **Ctrl + Shift + N** 。“创建新项目”对话框随即显示。
1. 在“搜索模板”文本框中输入“Linux”，列出 Linux 项目可用的模板 。
1. 选择要创建的项目类型（如“控制台应用程序”），然后选择“下一步” 。 输入“名称”和“位置”，然后选择“创建”    。

   ![“新建项目”对话框的屏幕截图，其中语言下拉列表设置为 C++，平台下拉列表设置为 Linux。](media/newproject-vs2019.png)

   | 项目类型 | 描述 |
   | ------------ | --- |
   | Raspberry Pi 项目 | 面向 Raspberry Pi 设备的项目，附带让 LED 闪烁的示例代码 |
   | **控制台应用程序** | 面向任何 Linux 计算机的项目，附带将文本输出到控制台的示例代码 |
   | **空项目** | 面向任何 Linux 计算机的项目，不带任何示例代码 |
   | **生成文件项目** | 面向任何 Linux 计算机的项目，使用标准生成文件生成系统生成 |
   | **CMake 项目** | 面向任何 Linux 计算机的项目，使用 CMake 生成系统生成 |

## <a name="next-steps"></a>后续步骤

[配置 Linux MSBuild 项目](configure-a-linux-project.md)

::: moniker-end
