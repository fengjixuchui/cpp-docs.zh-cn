---
title: 常规属性（Linux C++ 项目）
description: 介绍了可在 Visual Studio 的“常规属性”页面中设置的 Linux 项目属性。
ms.date: 10/14/2020
ms.assetid: 56c800a9-3df9-4196-87b2-81adb00e4767
ms.openlocfilehash: 9d6cb4ca51eff799788776c2641364939d438d63
ms.sourcegitcommit: f19f02f217b80804ab321d463c76ce6f681abcc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92176193"
---
# <a name="general-properties-linux-c"></a>常规属性 (Linux C++)

::: moniker range="vs-2015"

Linux 支持在 Visual Studio 2017 及更高版本中提供。

::: moniker-end

::: moniker range=">=vs-2017"

| Property | 描述 |
|--|--|
| 输出目录 | 指定输出文件目录的相对路径。 它可包括环境变量。 |
| 中间目录 | 指定中间文件目录的相对路径。 它可包括环境变量。 |
| 目标名称 | 指定此项目生成的文件名称。 |
| 目标扩展名 | 指定此项目生成的文件名称（例如 `.a`）。 |
| 清除时要删除的扩展名 | 分号分隔的通配符规范，指定在清除或重新生成时要删除中间目录中的哪些文件。 |
| 生成日志文件 | 指定启用生成日志时要写入的生成日志文件。 |
| 平台工具集 | 指定用于生成当前配置的工具集。 如果未设置，则使用默认工具集。 |
| WSL *.exe 完整路径 | Visual Studio 2019 版本 16.1：用于生成和调试的适用于 Linux 的 Windows 子系统 (WSL) 可执行文件的完整路径。 |
| 远程生成计算机 | 显示用于远程生成、部署和调试的目标计算机或设备。 可通过使用“工具” > “选项” > “跨平台” > “连接管理器”，添加或编辑目标计算机连接   。<br /> Visual Studio 2019 版本 16.1：可在  。 |
| 远程生成根目录 | 指定远程计算机或设备上目录的路径。 |
| 远程生成项目目录 | 指定远程计算机或设备上项目的目录路径。 |
| 远程部署目录 | Visual Studio 2019 版本 16.1：指定远程计算机或设备上的目录路径以部署项目  。 |
| 启用增量生成 | Visual Studio 2019 版本 16.7：指定是否使用 [Ninja](https://ninja-build.org/) 生成系统执行增量生成。 启用此设置后，大多数项目的生成速度通常会更快。 |
| 远程副本包含目录 | Visual Studio 2019 版本 16.5：要以递归方式从 Linux 目标复制的目录列表  。 此属性会影响 IntelliSense 的远程标头副本，但不影响生成。 可在“IntelliSense 使用编译器默认值”设置为 false 时使用它  。 使用“C/C++ 常规”选项卡下的“其他包含目录”来指定要同时对 IntelliSense 和生成使用的其他包含目录  。 |
| 远程副本排除目录 | Visual Studio 2019 版本 16.5：不从 Linux 目标复制的目录列表  。 通常，此属性用于删除包含目录的子目录。 |
| IntelliSense 使用编译器默认值 | Visual Studio 2019 版本 16.5：是否要在此项目引用的编译器中查询其包含目录的默认列表  。 这些位置会自动添加到要复制的远程目录列表中。 仅当编译器不支持类似 gcc 的参数时，才将此属性设置为 false。 gcc 和 clang 编译器都支持查询包含目录（例如 `g++ -x c++ -E -v -std=c++11`）。 |
| 配置类型 | 指定此配置生成的输出类型，如：动态库 (.so)、静态库 (.a)、应用程序 (.out) 和生成文件    |
| STL 的使用 | 指定要用于此配置的 C++ 标准库，如：共享 GNU 标准 C++ 库或静态 GNU 标准 C++ 库 (-static)  |

::: moniker-end
