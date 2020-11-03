---
title: 在 Android 和 iOS 上生成 OpenGL ES 应用程序
ms.date: 10/09/2019
ms.assetid: 76a67886-df57-4a81-accb-2e3c2eaf607b
ms.openlocfilehash: 278fd66202332417f7663542f0d66a3ec545b715
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924304"
---
# <a name="build-an-opengl-es-application-on-android-and-ios"></a>在 Android 和 iOS 上生成 OpenGL ES 应用程序

可以为共享通用代码的 iOS 应用和 Android 应用创建 Visual Studio 解决方案和项目。 本文将指导你完成组合的解决方案模板。 它创建 iOS 应用和 Android 本机活动应用。 应用具有共同的 C++ 代码，该代码使用 OpenGL ES 在每个平台上显示相同的动画旋转多维数据集。 OpenGL ES (OpenGL for Embedded Systems 或 GLES) 为2D 和3D 图形 API。 它在许多移动设备上都受支持。

## <a name="requirements"></a>要求

满足所有系统要求，为 iOS 和 Android 创建 OpenGL ES 应用。 如果尚未满足要求，请在 Visual Studio 安装程序中安装使用 C++ 的移动开发工作负载。 若要获取 OpenGL ES 模板并针对 iOS 进行生成，请包括可选的 C++ iOS 开发工具。 若要针对 Android 进行生成，请安装 c + + Android 开发工具和所需的第三方工具： Android NDK、Apache Ant 和 Google Android Emulator。

为了获得更好的 Intel 平台模拟器性能，一种选择是安装 Intel 硬件加速执行管理器 (HAXM) 。 有关详细说明，请参阅 [安装带有 c + + 的跨平台移动开发](../cross-platform/install-visual-cpp-for-cross-platform-mobile-development.md)。

若要生成和测试 iOS 应用，需要 Mac 计算机。 根据安装说明进行设置。 有关如何为 iOS 开发进行设置的详细信息，请参阅[安装并配置使用 iOS 进行生成的工具](../cross-platform/install-and-configure-tools-to-build-using-ios.md)。

## <a name="create-a-new-opengles-application-project"></a>创建新的 OpenGLES 应用程序项目

本教程中首先会创建一个新的 OpenGL ES 应用程序项目， 然后在 Android 仿真器中生成并运行默认应用。 接下来生成适用于 iOS 的应用并在 iOS 设备上运行该应用。

::: moniker range="msvc-150"

1. 在 Visual Studio 中，选择 " **文件** " " > **新建** > **项目** "。

1. 在  “新建项目”对话框中，在  “模板”下，选择“Visual C++”  >  “跨平台”，然后选择“OpenGLES 应用程序(Android、iOS)”  模板。

1. 为应用命名（例如 MyOpenGLESApp），然后选择“确定”  。

   ![新的 OpenGLES 应用程序项目](../cross-platform/media/cppmdd-opengles-newproj.png "新的 OpenGLES 应用程序项目")

   Visual Studio 创建新的解决方案并打开解决方案资源管理器。

   ![解决方案资源管理器中的 MyOpenGLESApp](../cross-platform/media/cppmdd-opengles-solexpl.png "解决方案资源管理器中的 MyOpenGLESApp")

::: moniker-end

::: moniker range=">=msvc-160"

1. 在 Visual Studio 中，选择 " **文件** " " > **新建** > **项目** "。

1. 在“创建新项目”对话框中，选择“OpenGLES 应用程序(Android、iOS)”模板，然后选择“下一步”  。

1. 在“配置新项目”对话框的“项目名称”中，输入项目名称（例如 MyOpenGLESApp），然后选择“创建”  。

   Visual Studio 创建新的解决方案并打开解决方案资源管理器。

   ![解决方案资源管理器中的 MyOpenGLESApp](../cross-platform/media/cppmdd-opengles-solexpl.png "解决方案资源管理器中的 MyOpenGLESApp")

::: moniker-end

新的 OpenGL ES 应用程序解决方案包括三个库项目和两个应用程序项目。 库文件夹包含一个共享代码项目。 和，两个引用共享代码的特定于平台的项目：

- `MyOpenGLESApp.Android.NativeActivity` 包含引用和粘附代码，可将应用作为本机活动在 Android 上实现。 粘附代码入口点在 main.cpp 中实现，它包括 `MyOpenGLESApp.Shared` 中的公共共享代码。  预编译头位于 pch.h。  此本机活动应用项目编译为一个由 `MyOpenGLESApp.Android.Packaging` 项目选取的共享库 (.so) 文件。 

- `MyOpenGLESApp.iOS.StaticLibrary` 创建一个 iOS 静态库 (.a) 文件，其中包含 `MyOpenGLESApp.Shared` 中的共享代码。  它链接到由 `MyOpenGLESApp.iOS.Application` 项目创建的应用。

- `MyOpenGLESApp.Shared` 包含跨平台运行的共享代码。 它使用预处理器宏进行特定于平台代码的条件编译。 共享代码由 `MyOpenGLESApp.Android.NativeActivity` 和 `MyOpenGLESApp.iOS.StaticLibrary` 的项目引用拾取。

该解决方案包含两个项目来生成适用于 Android 和 iOS 平台的应用：

- `MyOpenGLESApp.Android.Packaging` 创建 .apk 文件，用于 Android 设备或仿真程序上的开发。  此文件包括在此设置清单属性的资源和 AndroidManifest.xml 文件。 它还包括控制 Ant 生成过程的 build.xml 文件。  默认情况下，它被设置为启动项目，因此可从 Visual Studio 直接部署和运行它。

- `MyOpenGLESApp.iOS.Application` 包含资源和 Objective-C 粘附代码，以用于创建链接到 `MyOpenGLESApp.iOS.StaticLibrary` 中 C++ 静态库代码的 iOS 应用。 此项目创建一个生成包，该包通过 Visual Studio 和远程代理传输到你的 Mac。 当创建此项目时，Visual Studio 将发送文件和命令以在 Mac 上生成并部署应用。

## <a name="build-and-run-the-android-app"></a>生成并运行 Android 应用

由模板创建的解决方案将 Android 应用设置为默认项目。  你可以生成并运行此应用以验证安装和设置。 对于初始测试，在由适用于 Android 的仿真器安装的其中一个设备配置文件上运行该应用。 如果希望在其他目标上测试应用，可以加载目标模拟器。 或者，将设备连接到计算机。

### <a name="to-build-and-run-the-android-native-activity-app"></a>若要生成并运行 Android 本机活动应用

1. 如果尚未选中，则从“解决方案平台”  下拉列表中选择“x86”  。

   ![将解决方案平台设置为 x86](../cross-platform/media/cppmdd-opengles-solutionplat.png "将解决方案平台设置为 x86")

   使用 x86 指定仿真器目标。 若要针对一种设备，则基于设备处理器选择解决方案平台。 如果未显示“解决方案平台”  列表，则从“添加/删除按钮”  列表中选择“解决方案平台”  ，然后选择你的平台。

1. 在“解决方案资源管理器”中，打开 `MyOpenGLESApp.Android.Packaging` 项目的快捷菜单，然后选择“生成”。 

   ![构建 Android 打包项目](../cross-platform/media/cppmdd-opengles-andbuild.png "构建 Android 打包项目")

   “输出”窗口显示 Android 共享库和 Android 应用的生成过程的输出。

   ![为 Android 项目生成输出](../cross-platform/media/cppmdd-opengles-andoutput.png "为 Android 项目生成输出")

1. 选择其中一个仿真 Android 设备配置文件作为部署目标。

   ![选择部署目标](../cross-platform/media/cppmdd-opengles-pickemulator.png "选择部署目标")

   可能已安装了其他仿真程序或连接了 Android 设备。 您可以在 "部署目标" 下拉列表中选择它们。 若要运行该应用，生成的解决方案平台必须与目标设备的平台匹配。

1. 按 **f5** 开始调试 **，或按 f5** + **F5** 以启动（不调试）。

   Visual Studio 启动仿真程序，需要几秒钟时间来加载和部署代码。 下面介绍了此应用如何显示在仿真器中：

   ![运行在 Android 仿真器中的应用](../cross-platform/media/cppmdd-opengles-andemulator.png "在 Android 仿真器中运行的应用")

   一旦你的应用启动，你可以设置断点，并使用调试器逐步调试代码，检查局部变量并监视值。

1. 按 **Shift** + **F5** 停止调试。

   仿真程序是一个继续运行的单独进程。 你可多次编辑、编译和部署代码到同一仿真程序。 应用将显示在仿真程序中的应用集合上，它可以从那里直接启动。

   生成的 Android 本机活动应用和库项目将 c + + 共享代码放入动态库。 它包含与 Android 平台交互的 "胶水" 代码。 大多数应用代码位于库中。 清单、资源和生成说明位于打包项目中。 共享代码从 NativeActivity 项目中的 main.cpp 调用。 有关如何对 Android 本机活动进行编程的详细信息，请参阅 Android 开发人员 NDK [概念](https://developer.android.com/ndk/guides/concepts.html) 页。

   Visual Studio 使用 Android NDK 生成 Android 本机活动项目。 它使用 Clang 作为平台工具集。 Visual Studio 会将该项目的属性映射到目标平台上的编译、链接和调试命令。 有关详细信息，打开 MyOpenGLESApp.Android.NativeActivity 项目的“属性页”  对话框。 有关命令行开关的详细信息，请参阅 [Clang 编译器用户手册](https://clang.llvm.org/docs/UsersManual.html)。

## <a name="build-and-run-the-ios-app-on-an-ios-device"></a>在 iOS 设备上生成并运行 iOS 应用

在 Visual Studio 中创建和编辑 iOS 应用项目。 由于许可限制，它必须从 Mac 生成和部署。 Visual Studio 与在 Mac 上运行的远程代理进行通信，以传输项目文件并执行生成、部署和调试命令。 在生成 iOS 应用前设置和配置 Mac 和 Visual Studio 以进行通信。 有关详细说明，请参阅[安装并配置使用 iOS 进行生成的工具](../cross-platform/install-and-configure-tools-to-build-using-ios.md)。 在 Mac 上运行远程代理，并将其与 Visual Studio 配对。 然后，可以生成并运行 iOS 应用以验证安装和设置。

若要将应用部署到 iOS 设备，请先在 Xcode 中设置自动登录。 自动签名会创建一个配置文件，用于对应用程序的生成进行签名。

### <a name="to-set-up-automatic-signing-on-xcode"></a>在 Xcode 上设置自动签名

1. 如果尚未设置，请在 Mac 上安装 [Xcode](https://developer.apple.com/xcode/)。

1. 在 Mac 上打开 Xcode 应用。

1. 创建新的“单视图应用程序”  Xcode 项目。 在项目创建期间填写必填字段。 值可以是任意的，因为该项目仅用于创建预配配置文件，该配置文件稍后用于对应用生成进行签名。

1. 将在 [Apple 开发人员计划](https://developer.apple.com/programs/)帐户中注册的 Apple ID 添加到 Xcode。 你的 Apple ID 用作签名标识以对应用进行签名。 若要添加 Xcode 中的签名标识，请打开 Xcode  菜单并选择  “首选项”。 选择“帐户”  并单击“添加按钮(+)”以添加你的 Apple ID。 有关详细说明，请参阅[添加 Apple ID 帐户](https://help.apple.com/xcode/mac/current/#/devaf282080a)。

1. 从 Xcode 项目的“常规”设置中，将“捆绑标识符”  的值更改为 `com.<NameOfVSProject>`，其中 `<NameOfVSProject>` 与你创建的 Visual Studio 解决方案项目同名。 例如，如果已在 Visual Studio 上创建名为 `MyOpenGLESApp` 的项目，则将“捆绑标识符”  设置为 `com.MyOpenGLESApp`。

   ![Xcode 捆绑标识符](../cross-platform/media/cppmdd-opengles-iosxcodeid.png "Xcode 捆绑标识符")

1. 若要启用自动签名，请选中 “自动管理签名**”。

   ![Xcode 自动签名](../cross-platform/media/cppmdd-opengles-iosxcodesign.png "Xcode 自动签名")

1. 选择添加为开发团队  的 Apple ID 的团队名称。

   ![Xcode 团队](../cross-platform/media/cppmdd-opengles-iosxcodeteam.png "Xcode 团队")

### <a name="to-build-and-run-the-ios-app-on-an-ios-device"></a>在 iOS 设备上生成并运行 iOS 应用

1. 在 Mac 上运行远程代理，并验证 Visual Studio 是否与远程代理配对。 若要启动远程代理，请打开终端应用窗口并输入 `vcremote`。 有关详细信息，请参阅[在 Visual Studio 中配置远程代理](../cross-platform/install-and-configure-tools-to-build-using-ios.md#ConfigureVS)。

   ![运行 vcremote 的 Mac 终端窗口](../cross-platform/media/cppmdd-common-vcremote.png "运行 vcremote 的 Mac 终端窗口")

1. 将 iOS 设备连接到 Mac。 首次将设备连接到计算机后，会出现询问你是否信任计算机访问设备的警报。 使设备能够信任 Mac 计算机。

1. 在 Visual Studio 上，如果尚未选中，请基于你的设备处理器从“解决方案平台”  下拉列表中选择解决方案平台。 在此示例中为 ARM64  处理器。

   ![将解决方案平台设置为 ARM64](../cross-platform/media/cppmdd-opengles-pickplatformarm64.png "将解决方案平台设置为 ARM64")

1. 在“解决方案资源管理器”中，打开 MyOpenGLESApp.iOS.Application 项目的快捷菜单，然后选择“卸载项目”以卸载项目  。

1. 再次，打开已卸载的 MyOpenGLESApp.iOS.Application 项目的快捷菜单，然后选择“编辑 project.pbxproj”  以编辑项目文件。 在 `project.pbxproj` 文件中，查找 `buildSettings` 属性并使用 Apple 团队 ID 添加 `DEVELOPMENT_TEAM`。 以下屏幕截图显示 Apple 团队 ID 的 `123456ABC` 的示例值。 可以从 Xcode 中找到 Apple 团队 ID 的值。 转到“生成设置”  并将鼠标悬停在开发团队名称上以显示工具提示。 工具提示显示团队 ID。

   ![设置开发团队](../cross-platform/media/cppmdd-opengles-iosdevelopmentteam.png "设置开发团队")

1. 关闭 `project.pbxproj` 文件，然后打开已卸载的 MyOpenGLESApp.iOS.Application 项目的快捷菜单，然后选择“重新加载项目”  以重新加载项目。

1. 现在，通过打开 MyOpenGLESApp.iOS.Application 项目的快捷菜单并选择“生成”来生成该项目  。

   ![生成 iOS 应用程序项目](../cross-platform/media/cppmdd-opengles-iosbuild.png "构建 iOS 应用程序项目")

   "输出" 窗口将显示生成过程的输出。 它显示 iOS 静态库和 iOS 应用的结果。 在 Mac 上，运行远程代理的终端窗口显示命令和文件传输活动。

   在 Mac 计算机上，系统可能会提示你允许 codesign 访问密钥链。 选择 " **允许** " 以继续。

1. 选择工具栏上的 iOS 设备以在连接到 Mac 的设备上运行该应用。 如果应用不启动，请验证设备是否为已部署的应用程序提供在设备上执行的权限。 可以通过转到 **Settings**  >  **General**  >  设备上的 "设置" "常规" " **设备管理** " 来设置此权限。 选择你的开发人员应用帐户，信任你的帐户并验证该应用。 尝试从 Visual Studio 再次运行该应用。

   ![iOS 设备上的 iOS 应用](../cross-platform/media/cppmdd-opengles-iosdevice.png "iOS 设备上的 iOS 应用")

   启动应用后，你可以设置断点，并使用 Visual Studio 调试器检查局部变量、查看调用堆栈并监视值。

   ![iOS 应用中断点处的调试器](../cross-platform/media/cppmdd-opengles-iosdebug.png "iOS 应用中断点处的调试器")

1. 按 **Shift** + **F5** 停止调试。

   生成的 iOS 应用和库项目将 C++ 代码放入仅实现共享代码的静态库。 大多数应用程序代码位于 `Application` 项目中。 对此模板项目中共享库代码的调用是在 *gameviewcontroller.m* 文件中进行的。 若要生成 iOS 应用，Visual Studio 将使用 Xcode 平台工具集，这要求与在 Mac 上运行的远程客户端进行通信。

   Visual Studio 将项目文件传输到远程客户端。 然后，它使用 Xcode 发送命令来生成应用。 远程客户端将生成状态信息返回到 Visual Studio。 成功生成应用后，Visual Studio 可以发送命令来运行和调试应用。 Visual Studio 中的调试器控制在连接到 Mac 的 iOS 设备上运行的应用。 Visual Studio 将项目属性映射到用于在目标 iOS 平台上进行编译、链接和调试的选项。 若要了解编译器命令行选项的详细信息，请打开 MyOpenGLESApp.iOS.StaticLibrary 项目的  “属性页”对话框。

## <a name="customize-your-apps"></a>自定义应用

可修改共享的 C++ 代码以添加或更改常用功能。 更改和项目中共享代码的调用 `MyOpenGLESApp.Android.NativeActivity` `MyOpenGLESApp.iOS.Application` 以进行匹配。 可使用预处理器宏来指定公共代码中特定于平台的部分。 预处理器宏 `__ANDROID__` 在你针对 Android 进行生成时进行了预定义。 预处理器宏 `__APPLE__` 在你针对 iOS 进行生成时进行了预定义。

若要查看特定项目平台的 IntelliSense，请在上下文切换器下拉列表中选择该项目。 它在编辑器窗口顶部的导航栏中。

![在编辑器中投射上下文切换器下拉列表](../cross-platform/media/cppmdd-opengles-contextswitcher.png)

当前项目所使用的代码中的 IntelliSense 问题都标有红色的波浪线。 紫色波浪线标记其他项目中的问题。 Visual Studio 不支持 Java 或 Objective-C 文件的代码着色或 IntelliSense。 但是，您仍可以修改源文件和资源。 使用它们来设置应用程序名称、图标和其他实现详细信息。
