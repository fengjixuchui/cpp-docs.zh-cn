---
title: 重新分发 Visual C++ 文件
description: Visual Studio 包含可以在应用中部署的可再发行库和组件。
ms.date: 07/16/2020
helpviewer_keywords:
- application deployment [C++], file redistributing
- redistributing applications [C++]
- deploying applications [C++], file redistributing
- file redistribution [C++]
- redistributing applications [C++], about redistributing applications
ms.assetid: d201b2ce-36f1-44e5-a96c-0db81a1ba652
ms.openlocfilehash: a660e67b2664417438ea9fa7acddbde4c20c307a
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924137"
---
# <a name="redistributing-visual-c-files"></a>重新分发 Visual C++ 文件

> [!NOTE]
> 你是否正在寻找一个 Visual C++ 运行时文件的下载？ 请在 [Microsoft 网站](https://www.microsoft.com/) 上，在 "搜索" 框中输入 **Visual C++ 可再发行组件** 。 下载并安装适用于你的计算机体系结构的可再发行组件包（例如，如果运行 64 位 Windows，请使用 x64）以及所需的 Visual C++ 版本（例如 2015 版）。

## <a name="redistributable-files-and-licensing"></a>可再发行文件和许可

部署应用程序时，还必须部署支持该应用程序所需的文件。 如果这些文件中有任何文件由 Microsoft 提供，请检查是否允许重新发布这些文件。 可以在 IDE 中找到指向 Visual Studio 许可条款的链接。 使用 "关于 Microsoft Visual Studio" 对话框中的 "许可条款" 链接。 或者，从 Visual Studio [许可证目录](https://visualstudio.microsoft.com/license-terms/)中下载相关的 eula 和许可证。

::: moniker range="msvc-160"

若要查看 Visual Studio 2019 Microsoft 软件许可条款中 "可分发代码" 部分引用的 "再发行列表"，请参阅 [Microsoft Visual Studio 2019 的可分发代码文件](/visualstudio/releases/2019/redistribution#-distributable-code-files-for-visual-studio-2019)

::: moniker-end

::: moniker range="msvc-150"

若要查看 Visual Studio 2017 Microsoft 软件许可条款中 "可分发代码" 部分引用的 "再发行列表"，请参阅 [Microsoft Visual Studio 2017 的可分发代码文件](/visualstudio/productinfo/2017-redistribution-vs#-distributable-code-files-for-visual-studio-2017)。

::: moniker-end

::: moniker range="msvc-140"

若要查看 Visual Studio 2015 Microsoft 软件许可条款中 "可分发代码" 部分引用的 "再发行列表"，请参阅 [Microsoft Visual Studio 2015 的可分发代码文件](/visualstudio/productinfo/2015-redistribution-vs#-distributable-code-files-for-visual-studio-2015)。

::: moniker-end

有关可再发行文件的详细信息，请参阅 [确定要重新分发的 dll](determining-which-dlls-to-redistribute.md) 和 [部署示例](deployment-examples.md)。

## <a name="locate-the-redistributable-files"></a>找到可再发行文件

若要部署可再发行的文件，可以使用 Visual Studio 安装的可再发行组件包。 自2017以来，在 Visual Studio 版本中，这些文件被命名为 *`vc_redist.arm64.exe`* 、 *`vc_redist.x64.exe`* 和 *`vc_redist.x86.exe`* 。 在 Visual Studio 2015、Visual Studio 2017 和 Visual Studio 2019 中，它们在名称 *`vcredist_x86.exe`* 、 *`vcredist_x64.exe`* 和 *`vcredist_arm.exe`* 2015 (仅) 下可用。

查找可再发行文件的最简单方法是使用开发人员命令提示中设置的环境变量。 在最新版本的 Visual Studio 2019 中，可以找到文件夹中的可再发行文件 *`%VCINSTALLDIR%Redist\MSVC\v142`* 。 在 Visual Studio 2017 和 Visual Studio 2019 中，它们也位于中 *`%VCToolsRedistDir%`* 。 在 Visual Studio 2015 中，可以在中找到这些文件 *`%VCINSTALLDIR%redist\<locale>`* ，其中 *`<locale>`* 是可再发行组件包的区域设置。

另一种部署选项是使用可再发行的合并模块 ( *`.msm`* 文件) 。 在 Visual Studio 2019 中，这些文件属于 Visual Studio 安装程序中名为 **c + + 2019 可再发行** 组件的可选可安装组件 MSMs。 默认情况下，在 Visual Studio 2017 和 Visual Studio 2015 中将合并模块作为 c + + 安装的一部分进行安装。 在最新版本的 Visual Studio 2019 中安装时，你将在中找到可再发行的合并模块 *`%VCINSTALLDIR%Redist\MSVC\v142\MergeModules`* 。 在 Visual Studio 2019 和 Visual Studio 2017 中，它们也位于中 *`%VCToolsRedistDir%MergeModules`* 。 在 Visual Studio 2015 中，可以找到它们 *`Program Files [(x86)]\Common Files\Merge Modules`* 。

## <a name="install-the-redistributable-packages"></a>安装可再发行组件包

Visual C++ Redistributable Package 将安装并注册所有 Visual C++ 库。 如果使用一个，请在安装应用程序之前，在目标系统上运行它作为必备组件。 我们建议你在部署中使用这些包，因为它们能够启用 Visual C++ 库的自动更新。 有关如何使用这些包的示例，请参阅[演练：使用 Visual C++ 可再发行组件包部署 Visual C++ 应用程序](deploying-visual-cpp-application-by-using-the-vcpp-redistributable-package.md)。

每个 Visual C++ 可再发行包都会检查计算机上是否存在较新版本。 如果找到了最新版本，则不会安装包。 从 Visual Studio 2015 开始，可再发行包会显示一个表明安装失败的错误消息。 如果使用标志来运行包，则 **`/quiet`** 不会显示错误消息。 在任一情况下，Microsoft 安装程序都会记录错误，并且会将错误结果返回给调用方。 从 Visual Studio 2015 包开始，可以检查注册表是否安装了更新的版本，从而避免出现此错误。 当前安装的版本号存储在 `HKEY_LOCAL_MACHINE\SOFTWARE[\Wow6432Node]\Microsoft\VisualStudio\14.0\VC\Runtimes\{x86|x64|ARM}` 密钥中。 对于 Visual Studio 2015、Visual Studio 2017 和 Visual Studio 2019，版本号为14.0，因为最新的可再发行组件与2015版本二进制兼容。 键为 `ARM` 、 `x86` 或， `x64` 具体取决于平台的已安装 vcredist 版本。  (`Wow6432Node` 仅当使用 Regedit 查看 x64 平台上已安装的 x86 包的版本时，才需要在该子项下检查。 ) 版本号存储在 REG_SZ 字符串值中，同时还存储在 **`Version`** **`Major`** 、 **`Minor`** 、 **`Bld`** 和 **`Rbld`** `REG_DWORD` 值的集合中。 为了避免在安装时出错，如果当前安装的版本较新，必须跳过可再发行组件包的安装。

## <a name="install-the-redistributable-merge-modules"></a>安装可再发行合并模块

可再发行的合并模块必须包含在用于部署应用程序的 Windows Installer 包 (或类似安装包) 中。 有关详细信息，请参阅[使用合并模块重新分发](redistributing-components-by-using-merge-modules.md)。 有关示例，请参阅 [演练：使用安装项目部署 Visual C++ 应用程序](walkthrough-deploying-a-visual-cpp-application-by-using-a-setup-project.md)。

## <a name="install-individual-redistributable-files"></a>安装单独的可再发行文件

还可以直接将可再发行的 Dll 安装到 *应用程序本地文件夹* 中。 这就是包含可执行应用程序文件的文件夹。 出于处理原因，我们不建议使用此安装位置。

## <a name="potential-run-time-errors"></a>可能的运行时错误

如果 Windows 找不到你的应用程序所需的一个可再发行库 Dll，它可能会显示如下消息： "此应用程序无法启动，因为找不到 *库* 。 重新安装应用程序可能会解决此问题。 "

若要解决此类错误，请确保应用程序安装程序已正确生成。 验证可再发行库是否已正确部署到目标系统上。 有关详细信息，请参阅[了解 Visual C++ 应用程序的依赖项](understanding-the-dependencies-of-a-visual-cpp-application.md)。

## <a name="related-articles"></a>相关文章

[使用合并模块重新发布](redistributing-components-by-using-merge-modules.md)\
介绍如何使用 Visual C++ 可再发行合并模块将 Visual C++ 运行时库作为共享 Dll 安装到 *`%windir%\system32\`* 文件夹中。

[重新分发 Visual C++ ActiveX 控件](redistributing-visual-cpp-activex-controls.md)\
描述如何重新发布使用 ActiveX 控件的应用程序。

[重新分发 MFC 库](redistributing-the-mfc-library.md)\
描述如何重新发布使用 MFC 的应用程序。

[重新分发 ATL 应用程序](redistributing-an-atl-application.md)\
描述如何重新发布使用 ATL 的应用程序。 从 Visual Studio 2012 开始，无需 ATL 的可再发行库。

[部署示例](deployment-examples.md)\
指向演示如何部署 Visual C++ 应用程序的示例的链接。

[部署桌面应用程序](deploying-native-desktop-applications-visual-cpp.md)\
介绍 Visual C++ 部署概念和技术。
