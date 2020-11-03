---
title: 参考：vcperf 命令
description: 命令行实用工具 vcperf.exe 参考。
ms.date: 11/03/2019
helpviewer_keywords:
- C++ Build Insights
- throughput analysis
- build time analysis
- vcperf.exe
ms.openlocfilehash: c251d93ce7e9e7325a7146f5697150344cb02d96
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91508814"
---
# <a name="reference-vcperf-commands"></a>参考：vcperf 命令

::: moniker range="<=vs-2017"

Visual Studio 2019 中提供 C++ 生成见解工具。 若要查看此版本对应的文档，请将本文的 Visual Studio“版本”选择器控件设置为“Visual Studio 2019”。 它位于此页面上目录表的顶部。

::: moniker-end
::: moniker range="vs-2019"

本文列出并介绍 vcperf.exe中可用的命令以及如何使用它们。

## <a name="commands-to-start-and-stop-traces"></a>用于开始跟踪和停止跟踪的命令

*重要提示：以下命令都需要管理权限。*

| 选项           | 参数和描述 |
|------------------|---------------------------|
| `/start`         | `[/nocpusampling]` `<sessionName>` |
|                  | 指示 vcperf.exe 在给定的会话名称下启动跟踪。 给定计算机上一次只能有一个活动会话。 <br/><br/> 如果指定了 `/nocpusampling` 选项，vcperf.exe 不会收集 CPU 示例。 它会阻止在 Windows 性能分析器中使用“CPU 使用率”（采样）视图，会使收集的跟踪显得更小。 <br/><br/> 开始跟踪后，vcperf.exe 会立即返回。 对于计算机上运行的所有进程，会收集系统范围内的事件。 这意味着无需从运行 vcperf.exe 时使用的命令提示符处生成项目。 例如，你可以从 Visual Studio 生成项目。 |
| `/stop`          | `<sessionName>` `<outputFile.etl>` |
|                  | 停止由给定会话名称标识的跟踪。 对该跟踪运行一个后处理步骤，以生成在 Windows 性能分析器 (WPA) 中可查看的文件。 为了获得最佳查看体验，请使用包含 C++ Build Insights 加载项的 WPA 版本。 有关详细信息，请参阅 [C++ Build Insights 入门](../get-started-with-cpp-build-insights.md)。 `<outputFile.etl>` 参数指定输出文件的保存位置。 |
| `/stopnoanalyze` | `<sessionName>` `<rawOutputFile.etl>` |
|                  | 停止由给定会话名称标识的跟踪，并将原始的未处理数据写入指定的输出文件中。 生成的文件不适合在 WPA 中查看。 <br/><br/> `/stop` 命令中涉及的后处理步骤有时会很长。 可以使用 `/stopnoanalyze` 命令延迟此后处理步骤。 准备好生成可在 Windows 性能分析器中查看的文件时，请使用 `/analyze` 命令。 |

## <a name="miscellaneous-commands"></a>杂项命令

| 选项     | 参数和描述 |
|------------|---------------------------|
| `/analyze` | `<rawInputFile.etl> <outputFile.etl>` |
|            | 接受 `/stopnoanalyze` 命令生成的原始跟踪文件。 对此跟踪运行一个后处理步骤，以生成在 Windows 性能分析器中可查看的文件。 为了获得最佳查看体验，请使用包含 C++ Build Insights 加载项的 WPA 版本。 有关详细信息，请参阅 [C++ Build Insights 入门](../get-started-with-cpp-build-insights.md)。 |

## <a name="see-also"></a>请参阅

[C++ Build Insights 入门](../get-started-with-cpp-build-insights.md)\
[教程：Windows Performance Analyzer 基础知识](../tutorials/wpa-basics.md)\
[参考：Windows Performance Analyzer 视图](wpa-views.md)\
[Windows Performance Analyzer](/windows-hardware/test/wpt/windows-performance-analyzer)

::: moniker-end
