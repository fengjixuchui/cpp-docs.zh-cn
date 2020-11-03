---
title: 通用 CRT 部署
description: 了解如何、何时以及在何处部署适用于应用的通用 CRT 库。
ms.date: 10/23/2020
helpviewer_keywords:
- deploying the CRT [C++]
- application CRT deployment [C++]
ms.openlocfilehash: 50ab23f3b0276b058685e9c9ef6634caf5a96186
ms.sourcegitcommit: bf54b407169900bb668c85a67b31dbc0f069fe65
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92497201"
---
# <a name="universal-crt-deployment"></a>通用 CRT 部署

从 Visual Studio .NET 到 Visual Studio 2013，C++ 编译器和工具的每个主版本都包含一个新的独立版本的 Microsoft C 运行 (CRT) 库。 CRT 的这些独立版本彼此独立，并在不同程度上彼此不兼容。 例如，Visual Studio 2012 使用的 CRT 库是第 11 版，名为 msvcr110.dll，而 Visual Studio 2013 使用的 CRT 是第 12 版，名为 msvcr120.dll。 从 Visual Studio 2015 开始，不再是这样。 Visual Studio 2015 及更高版本的 Visual Studio 都使用一个通用 CRT。

通用 CRT (UCRT) 是 Microsoft Windows 操作系统组件。 它包含在 Windows 10 和 Windows Server 2016 或更高版本中作为操作系统的一部分。 对于仍处于扩展支持的较早版本的操作系统，可使用 Windows 更新来提供 UCRT。 支持通用 CRT 本地部署，但有一些限制。

## <a name="central-deployment"></a>集中部署

集中安装通用 CRT 的首选方法是使用 Microsoft Windows 更新。 通用 CRT 是所有支持的 Microsoft Windows 操作系统的推荐更新，所以默认情况下，大多数计算机在常规更新过程中都会安装它。 通用 CRT 的初始版本为 [KB2999226](https://support.microsoft.com/kb/2999226)。 在 [KB3118401](https://support.microsoft.com/kb/3118401)中进行了带有各种 bug 修复的更高版本的更新，并且还有其他更新和 bug 修复和新功能。 有关最新更新，请搜索 [support.microsoft.com](https://support.microsoft.com) 以查找通用 C 运行时或通用 CRT。

并非所有 Microsoft Windows 计算机都使用 Windows 更新定期安装更新，有些计算机可能不会安装所有推荐的更新。 若要支持使用通过使用 Visual Studio 2015 和更高版本的 c + + 工具构建的这些计算机上的应用程序，有可供脱机分发的通用 CRT 可再发行文件。 可以从上述某个 KB 链接下载这些可再发行文件。 通用 CRT 可再发行组件要求计算机已更新为当前 Service Pack。 举个例子，Windows 7 的可再发行组件仅安装到 Windows 7 SP1 上，而不会安装到 Windows 7 RTM 上。

由于通用 CRT 是 c + + 库的基本依赖项，因此 Visual C++ 可再发行组件 (VCRedist) 在尚未安装的计算机上安装通用 CRT (版本 10.0.10240) 的初始版本。 此版本足以满足 c + + 库依赖关系。 如果你的应用程序依赖于较新版本的通用 CRT，则必须使用 Windows 更新使你的计算机保持最新状态，或显式安装该版本。 在安装 VCRedist 之前，最好通过 Windows 更新或 MSU 安装通用 C 运行时，以避免潜在的多个需要重新启动。

并非所有操作系统均可通过 Windows 更新满足最新的通用 C 运行时的条件。 在 Windows 10 上，集中部署的版本与操作系统的版本相匹配。 若要进一步更新通用 C 运行时，必须更新操作系统。 对于 Windows Vista 到 Windows 8.1，最新可用的通用 C 运行时当前基于 Windows 10 周年更新中包含的版本，另外还提供了 (版本 10.0.14393) 的其他修补程序。

## <a name="local-deployment"></a>本地部署

支持通用 CRT 的本地部署（但由于性能和安全原因不推荐）。 根据计算机体系结构，本地部署的 DLL 作为 Windows SDK 的一部分包含在 Windows Kits\\10\\Redist\\ucrt\\DLL 子目录中。 所需的 DLL 包括 ucrtbase.dll 和名为 api-ms-win-\*.dll 的一组 APISet forwarder DLL。 每个操作系统上所需的一组 Dll 会有所不同。 强烈建议在本地部署时包括所有 Dll。

本地部署有两个需要注意的限制：

- 在 Windows 10 上，即使应用程序包含通用 CRT 的应用程序本地副本，也始终使用系统目录中的通用 CRT。 即使本地副本是较新的，也是如此，因为通用 CRT 是 Windows 10 上的核心操作系统组件。

- 在 Windows 8 之前的 Windows 版本中，如果不能使用插件在本地打包通用 CRT，则不能将其放在主应用可执行文件目录以外的目录中。 在这种情况下，APISet forwarder DLL 无法成功解析 ucrtbase.dll。 以下为建议的替代解决方案：

  - 静态链接通用 CRT，
  - 集中部署通用 CRT，或
  - 将通用 CRT 文件放在与应用相同的目录中。

## <a name="deployment-on-microsoft-windows-xp"></a>Microsoft Windows XP 上的部署

Visual Studio 2015 和 Visual Studio 2017 继续支持用于 Microsoft Windows XP 的软件开发。 若要支持此开发，通用 CRT 的版本可在 Microsoft Windows XP 上运行。 Microsoft Windows XP 操作系统不再是主流或扩展的支持，因此在 Microsoft Windows XP 上集中部署通用 CRT 不同于其他操作系统。

当在 Windows XP 上安装 Visual C++ 可再发行组件时，它会直接将通用 CRT 及其所有依赖项安装到系统目录中。 它不会安装或依赖于任何 Windows 更新。 可再发行的合并模块（即 Microsoft_VC*version*_CRT_\*.msm 文件）执行相同操作。

Windows XP 上通用 CRT 的本地部署与其他支持的操作系统相同。

## <a name="see-also"></a>另请参阅

- [Visual C++ 中的部署](deployment-in-visual-cpp.md)
