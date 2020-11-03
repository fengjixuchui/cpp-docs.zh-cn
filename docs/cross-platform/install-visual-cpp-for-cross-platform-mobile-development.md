---
title: 使用 C++ 安装跨平台移动开发
ms.date: 10/17/2019
ms.assetid: aaea6b8d-55eb-4427-8185-c050f855c257
ms.openlocfilehash: b9a6b78450969f5228ccf16cc7b0cff02a4d3b1e
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924001"
---
# <a name="install-cross-platform-mobile-development-with-c"></a>使用 C++ 安装跨平台移动开发

你可以在 Visual Studio 中使用 c + + 来生成 Windows 桌面应用程序、通用 Windows 平台 (UWP) 应用程序和 Linux 应用程序。 现在，你可以生成适用于 Android 和 iOS 的 c + + 应用程序。 **使用 c + +** 工作负荷的移动开发是在 Visual Studio 中的一组可安装组件。 它包括跨平台 iOS、Android 和 UWP Visual Studio 模板。 工作负荷会安装快速入门所需的跨平台工具和 Sdk。 您无需自行查找、下载和配置它们。 可以在 Visual Studio 中使用这些工具轻松创建、编辑、调试和测试跨平台项目。

本文介绍了如何使用 Visual Studio 安装在 C++ 内开发跨平台应用所需的工具和第三方软件。 有关概述，请参阅 [Visual C++ 跨平台移动](https://visualstudio.microsoft.com/vs/features/cplusplus-mdd/)

## <a name="requirements"></a>要求

::: moniker range="msvc-150"

- 有关安装要求，请参阅 [Visual Studio 产品系列系统要求](/visualstudio/productinfo/vs2017-system-requirements-vs)。

   > [!IMPORTANT]
   > 如果使用的是 Windows 7 或 Windows Server 2008 R2，则可以针对 Windows 桌面应用程序、Android Native Activity 应用和库以及适用于 iOS 的应用和代码库开发代码，但不能针对 Windows 应用商店或 UWP 应用开发代码。

::: moniker-end
::: moniker range=">=msvc-160"

- 有关安装要求，请参阅 [Visual Studio 产品系列系统要求](/visualstudio/releases/2019/system-requirements)。

   > [!IMPORTANT]
   > 如果使用的是 Windows 7 或 Windows Server 2008 R2，则可以针对 Windows 桌面应用程序、Android Native Activity 应用和库以及适用于 iOS 的应用和代码库开发代码，但不能针对 Windows Phone 或 UWP 应用开发代码。

::: moniker-end

若要为特定的设备平台创建应用，还需要满足一些附加要求：

- Android SDK 附带的 x86 Android 仿真器最适用于可使用硬件加速的计算机，如 Intel 硬件加速执行管理器 (HAXM)。 有关详细信息，请参阅[通过硬件加速提高仿真器性能 (Hyper-V & HAXM)](/xamarin/android/get-started/installation/android-emulator/hardware-acceleration?tabs=vswin&pivots=windows)。

- 生成适用于 iOS 的代码，需要 Apple ID、iOS Developer Program 帐户，以及可在 OS X Mavericks（版本 10.9）或更高版本上运行 [Xcode](https://developer.apple.com/xcode/) 版本 10.2 或更高版本的 Mac 计算机。 有关安装步骤的链接，请参阅[安装适用于 iOS 的工具](#install-tools-for-ios)。

- Windows Phone 仿真器需要一台可以运行 Hyper-V 的计算机。 必须先启用 Windows 中的 Hyper-V 功能，然后才能安装和运行仿真程序。 有关详细信息，请参阅仿真程序的[系统要求](/visualstudio/cross-platform/system-requirements-for-the-visual-studio-emulator-for-android)。

## <a name="get-the-tools"></a>获取工具

在 Visual Studio Community、Visual Studio Professional 和 Visual Studio Enterprise 版本中提供使用 C++ 的移动开发。 要获得 Visual Studio，可转到 [Visual Studio 下载](https://visualstudio.microsoft.com/downloads/)页进行下载。 从 Visual Studio 2015 开始提供跨平台移动开发工具。

## <a name="install-the-tools"></a>安装工具

Visual Studio 安装程序包括使用 C++ 的移动开发工作负载  。 此工作负载安装 Visual Studio 中的 Android 和 iOS 开发所需的 C++ 语言工具、模板和组件。 它包括 Android 生成和调试所需的 GCC 和 Clang 工具集。 工作负载会安装 Android SDK，以及与用于 iOS 开发的 Mac 进行通信的组件。 它还安装支持 iOS 和 Android 应用开发所需的第三方工具和软件开发工具包。 这些大部分第三方工具都是 Android 平台支持所需的开放源代码软件。

- 生成面向 Android 平台的 C++ 代码需要 Android 本机开发工具包 (NDK)、Apache Ant 和 C++ Android 开发工具。

  > [!NOTE]
  > Android NDK 中的某些工具不支持文件路径和文件名中的 Unicode 字符。 如果项目或源文件的路径或文件名中包含 Unicode 字符，则项目将无法生成。

- Google Android 仿真器和 Intel 硬件加速执行管理器 (HAXM) 是可选组件，但推荐使用。  (Intel HAXM 驱动程序仅适用于 Intel 处理器，并与某些 Vm （包括 Hyper-v）不兼容。 ) 可以直接在 Android 设备上进行开发和调试，但使用桌面上的仿真程序进行调试通常更容易。

- 生成面向 iOS 平台的 C++ 代码需要 C++ iOS 开发工具。

> [!NOTE]
> 如果使用的是 Visual Studio 2015，请参阅[安装适用于跨平台移动开发的 Visual C++ (Visual Studio 2015)](install-visual-cpp-for-cross-platform-mobile-development.md?view=msvc-140&preserve-view=true)

### <a name="install-the-mobile-development-with-c-workload"></a>安装使用 C++ 的移动开发工作负载

1. 从“开始”菜单运行“Visual Studio 安装程序”  。

1. 如果已经安装了 Visual Studio，则可以为想要修改的已安装 Visual Studio 版本选择“修改”按钮  。 否则，请选择“安装”  以安装 Visual Studio。

1. 选中“工作负载”  选项卡后，向下滚动并在 Visual Studio 安装程序中选择“使用 C++ 的移动开发”  工作负载。 选中此工作负载后，也会选中 C++ 开发所需的其他组件。 还可以选择其他工作负载和单个组件同时进行安装。 要生成同时面向 UWP 的跨平台代码，请选择“通用 Windows 平台开发”  工作负载。

1. 在“安装详细信息”  窗格中，展开“使用 C++ 的移动开发”  。 在“可选”  部分，你可以选择其他版本的 NDK、Google Android 模拟器、Intel 硬件加速执行管理器以及 IncrediBuild 生成加速工具。

1. 默认情况下，工作负载包含一个或多个 Android SDK 安装程序组件。 提供 Android SDK 的其他版本。 要安装其中一个版本，请选择“单个组件”  选项卡，然后向下滚动到“SDK、库和框架”  部分进行选择。

1. 选择“修改”  或“安装”  按钮以安装“使用 C++ 的移动开发”  工作负载以及选择的其他工作负载和可选组件。

   安装完成后，关闭该安装程序，然后重启计算机。 第三方组件的某些设置操作在计算机重启后才生效。

   > [!IMPORTANT]
   > 你必须重新启动以确保所有软件都得到了正确安装。

1. 打开 Visual Studio。

## <a name="install-tools-for-ios"></a>Install tools for iOS

你可以使用 Visual Studio 来编辑、调试 iOS 代码，并将其部署到 iOS 模拟器。 或者，到 iOS 设备。 由于许可限制，该代码必须在 Mac 上远程生成。 若要使用 Visual Studio 生成和运行 iOS 应用，请先在 Mac 上设置和配置远程代理。 有关安装说明、先决条件和配置选项的详细信息，请参阅[安装并配置使用 iOS 进行构建的工具](../cross-platform/install-and-configure-tools-to-build-using-ios.md)。 如果你不是针对 iOS 构建，则可以跳过此步骤。

## <a name="install-or-update-dependencies-manually"></a>手动安装或更新依赖项

在 **使用 c + +** 工作负荷安装移动开发时，无需安装所有第三方依赖项 (或在 Visual Studio 2015 中，) Visual C++ 移动开发选项。 稍后使用 [安装工具](#install-the-tools)中的步骤安装这些工具。 Visual Studio 安装程序会定期更新以安装最新的第三方组件。 使用它安装更新的 Sdk 和 Ndk。 你还可以独立于 Visual Studio 安装或更新它们。

可以再次在 Android SDK 目录中运行 SDK 管理器应用，以更新 SDK。 和，用于安装可选工具和其他 API 级别。 除非你使用“以管理员身份运行”  运行 SDK Manager 应用，否则安装更新可能会失败。 如果构建 Android 应用存在问题，请检查已安装的 SDK 的 SDK Manager 更新。

若要使用某些 Android SDK 模拟器，你可能需要设置硬件加速。 有关详细信息，请参阅[通过硬件加速提高仿真器性能 (Hyper-V & HAXM)](/xamarin/android/get-started/installation/android-emulator/hardware-acceleration?tabs=vswin)。

在大多数情况下，Visual Studio 可以检测到已安装的第三方软件的配置。 它维护内部环境变量中的安装路径。 可以覆盖 Visual Studio IDE 中的这些跨平台开发工具的默认路径。

### <a name="to-set-the-paths-for-third-party-tools"></a>若要设置第三方工具的路径

1. 在 Visual Studio 菜单栏上，选择 " **工具** " "  >  **选项** "。

1. 在“选项”  对话框中，选择“跨平台”  > “C++”  > “Android”  。

   ![Android 工具路径选项](../cross-platform/media/cppmdd-options-android.png "Android 工具路径选项")

1. 若要更改工具使用的路径，请选中该路径旁的复选框，并在文本框中编辑文件夹路径。 还可以使用浏览按钮 (  对话框以选择文件夹。

1. 选择“确定”  以保存自定义工具文件夹位置。

## <a name="see-also"></a>请参阅

[安装和配置工具以使用 iOS 进行生成](install-and-configure-tools-to-build-using-ios.md)\
[Visual C++ 跨平台移动](https://visualstudio.microsoft.com/vs/features/cplusplus-mdd/)
