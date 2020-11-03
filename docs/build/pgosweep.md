---
title: pgosweep
description: 使用 pgosweep 命令将配置文件数据写入 PGC 文件，以便在按配置优化中使用。
ms.date: 10/23/2020
helpviewer_keywords:
- pgosweep program
- profile-guided optimizations, pgosweep
ms.openlocfilehash: be96d0f092ff65867c304ddf5eb41c0754f6e4be
ms.sourcegitcommit: bf54b407169900bb668c85a67b31dbc0f069fe65
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92497179"
---
# <a name="pgosweep"></a>pgosweep

在按配置优化中使用，用于将正在运行的程序中的所有配置文件数据写入 PGC 文件。

## <a name="syntax"></a>语法

> **`pgosweep`**  [options] image pgcfile

### <a name="parameters"></a>参数

options\
（可选）“选项”的有效值为  ：

- `/?` 或 `/help` 显示帮助消息。

- `/reset` 会在扫描后将计数重置为零。 此选项为默认行为。

- `/pid:n` 仅扫描指定的 PID，其中 n 是 PID 编号。

- `/wait` 等到指定的 PID 终止后再收集计数。

- `/onlyzero` 不保存 PGC 文件，仅零计数。

- `/pause` 暂停系统上的计数收集。

- `/resume` 暂停系统上的计数收集。

- `/noreset` 保留运行时数据结构中的计数。

*image*\
使用 [`/GENPROFILE`](reference/genprofile-fastgenprofile-generate-profiling-instrumented-build.md)、[`/FASTGENPROFILE`](reference/genprofile-fastgenprofile-generate-profiling-instrumented-build.md) 或 [`/LTCG:PGINSTRUMENT`](reference/ltcg-link-time-code-generation.md) 选项创建的 EXE 或 DLL 文件的完整路径。

pgcfile\
此命令写出数据计数的 PGC 文件。

## <a name="remarks"></a>注解

`pgosweep` 命令适用于使用 [`/GENPROFILE` 或 `/FASTGENPROFILE`](reference/genprofile-fastgenprofile-generate-profiling-instrumented-build.md) 选项或弃用的 [`/LTCG:PGINSTRUMENT`](reference/ltcg-link-time-code-generation.md) 选项生成的程序。 它会中断正在运行的程序并将配置文件数据写入新的 PGC 文件。 默认情况下，该命令在每次写入操作后重置计数。 如果指定 `/noreset` 选项，该命令将记录这些值，但不会在运行的程序中重置它们。 如果稍后检索配置文件数据，此选项将为你提供重复数据。

`pgosweep` 的另一个用途是仅为应用程序的正常操作检索配置文件信息。 例如，可以在启动应用程序并放弃该文件后不久运行 `pgosweep`。 此命令将删除与启动成本相关的配置文件数据。 然后，可以在结束应用程序之前运行 `pgosweep`。 现在，收集的数据只有在用户可以与程序交互时才具有配置文件信息。

使用 pgcfile 参数命名 PGC 文件时，可以使用标准格式，即 `appname!n.pgc` 。 n 表示每个文件增加的数值。 如果使用此格式，编译器将在 `/LTCG /USEPROFILE` 或 `/LTCG:PGO` 阶段自动查找此数据。 如果不使用标准格式，则必须使用 [`pgomgr`](pgomgr.md) 合并 PGC 文件。

> [!NOTE]
> 只能从 Visual Studio 开发人员命令提示启动此工具。 不能从系统命令提示符或从文件资源管理器启动此工具。

有关如何从可执行文件中捕获配置文件数据的信息，请参阅 [`PgoAutoSweep`](pgoautosweep.md)。

## <a name="example"></a>示例

在此示例命令中，`pgosweep` 将 `myapp.exe` 的当前配置文件信息写入 `myapp!1.pgc`。

`pgosweep myapp.exe myapp!1.pgc`

## <a name="see-also"></a>另请参阅

[按配置优化](profile-guided-optimizations.md)\
[PgoAutoSweep](pgoautosweep.md)
