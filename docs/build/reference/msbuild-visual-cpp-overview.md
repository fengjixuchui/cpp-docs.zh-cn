---
title: Visual Studio 中的 C++ 项目的 MSBuild 内部项
description: MSBuild 用于 Visual Studio c + + 项目的支持文件、属性和目标。
ms.date: 10/14/2020
helpviewer_keywords:
- MSBuild overview
ms.openlocfilehash: e99b9a428d9c6149debc06e1dfab7a69c3590196
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924388"
---
# <a name="msbuild-internals-for-c-projects"></a>C++ 项目的 MSBuild 内部项

如果在 IDE 中设置项目属性并保存项目，则 Visual Studio 会将项目设置写入项目文件。 项目文件包含项目特有的设置。 但是，它并不包含生成项目所需的所有设置。 项目文件包含 `Import` 元素，该元素包括由其他支持文件形成的网络  。 支持文件包含生成项目所需的剩余属性、目标和设置。

支持文件中的大部分目标和属性只用于实现生成系统。 本文介绍可在 MSBuild 命令行中指定的有用目标和属性。 若要了解更多目标和属性，请浏览支持文件目录中的文件。

## <a name="support-file-directories"></a>支持文件目录

默认情况下，主要的 Visual Studio 支持文件位于以下目录中。 此信息特定于版本。

::: moniker range=">=msvc-160"

### <a name="visual-studio-2019"></a>Visual Studio 2019

- *`%VSINSTALLDIR%MSBuild\Microsoft\VC\<version>\`*

  包含目标使用的主要目标文件 (.targets) 和属性文件 (.props)。 默认情况下，$(VCTargetsPath) 宏引用此目录。 *`<version>`* 占位符是指 visual studio 版本： v160 For visual studio 2019、v150 For Visual studio 2017。

- *`%VSINSTALLDIR%MSBuild\Microsoft\VC\<version>\Platforms\<platform>\`*

  包含特定于平台的目标文件和属性文件，这些文件会覆盖其父目录中的目标和属性。 此目录还包含定义此目录中的目标所使用的任务的 DLL。 *`<platform>`* 占位符表示 ARM、ARM64、Win32 或 x64 子目录。

- *`%VSINSTALLDIR%MSBuild\Microsoft\VC\<version>\Platforms\<platform>\PlatformToolsets\<toolset>\`*

  包含使生成能够使用指定的生成 c + + 应用程序的目录 *`<toolset>`* 。 *`<platform>`* 占位符表示 ARM、ARM64、Win32 或 x64 子目录。 *`<toolset>`* 占位符表示工具集子目录。

::: moniker-end

::: moniker range=">=msvc-150"

### <a name="visual-studio-2017"></a>Visual Studio 2017

- *`%VSINSTALLDIR%Common7\IDE\VC\VCTargets\`*

  包含 *`.targets`* 目标 () 和属性文件)  (目标使用的主目标文件 *`.props`* 。 默认情况下，该 `$(VCTargetsPath)` 宏引用此目录。

- *`%VSINSTALLDIR%Common7\IDE\VC\VCTargets\Platforms\<platform>\`*

  包含特定于平台的目标文件和属性文件，这些文件会覆盖其父目录中的目标和属性。 此目录还包含定义此目录中的目标所使用的任务的 DLL。 *`<platform>`* 占位符表示 ARM、ARM64、Win32 或 x64 子目录。

- *`%VSINSTALLDIR%Common7\IDE\VC\VCTargets\Platforms\<platform>\PlatformToolsets\<toolset>\`*

  包含使生成能够使用指定的生成 c + + 应用程序的目录 *`<toolset>`* 。 *`<platform>`* 占位符表示 ARM、Win32 或 x64 子目录。 *`<toolset>`* 占位符表示工具集子目录。

::: moniker-end

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 及更早版本

- *`<drive>:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\<version>\`*

  包含目标使用的主要目标文件 (.targets) 和属性文件 (.props)。 默认情况下，$(VCTargetsPath) 宏引用此目录。

- *`<drive>:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\<version>\Platforms\<platform>\`*

  包含特定于平台的目标文件和属性文件，这些文件会覆盖其父目录中的目标和属性。 此目录还包含定义此目录中的目标所使用的任务的 DLL。 *`<platform>`* 占位符表示 ARM、Win32 或 x64 子目录。

- *`<drive>:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\<version>\Platforms\<platform>\PlatformToolsets\<toolset>\`*

  包含使生成能够使用指定的生成 c + + 应用程序的目录 *`<toolset>`* 。 *`<version>`* 占位符是 V110 For Visual studio 2012、V120 for Visual Studio 2013 和 Visual studio 2015 的 V140。 *`<platform>`* 占位符表示 ARM、Win32 或 x64 子目录。 *`<toolset>`* 占位符表示工具集子目录。 例如，使用 Visual Studio 2015 工具集生成 Windows 应用程序是 v140 的。 或者，使用 Visual Studio 2013 工具集为 Windows XP 构建 v120_xp。

- *`<drive>:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\Platforms\<platform>\PlatformToolsets\<toolset>\`*

  允许生成生成 Visual Studio 2008 或 Visual Studio 2010 应用程序的路径不包括 *`<version>`* 。 在这些版本中， *`<platform>`* 占位符表示 Itanium、Win32 或 x64 子目录。 *`<toolset>`* 占位符表示 v90 或 v100 工具集子目录。

## <a name="support-files"></a>支持文件

支持文件目录包含带有以下扩展名的文件：

| 分机 | 说明 |
| --------- | ----------- |
| *`.targets`* | 包含指定由目标执行的任务的 `Target` XML 元素。 可能还包含 `PropertyGroup`、`ItemGroup`、`ItemDefinitionGroup` 和用户定义的 `Item` 元素，这些元素用于将文件和命令行选项分配给任务参数。<br /><br /> 有关详细信息，请参阅 [Target 元素 (MSBuild)](/visualstudio/msbuild/target-element-msbuild)。 |
| *`.props`* | 包含 `Property Group` 和用户定义的 `Property` XML 元素，这些元素指定在生成过程中使用的文件和参数设置。<br /><br /> 还可能包含 `ItemDefinitionGroup` 和用户定义的 `Item` XML 元素，这些元素指定其他设置。 项定义组中定义的项与属性类似，但无法从命令行访问。 Visual Studio 项目文件经常使用项（而不是属性）来表示设置。<br /><br /> 有关详细信息，请参阅 [ItemGroup 元素 (msbuild) ](/visualstudio/msbuild/itemgroup-element-msbuild)、 [ItemDefinitionGroup 元素 (Msbuild) ](/visualstudio/msbuild/itemdefinitiongroup-element-msbuild)和 [Item 元素 (msbuild) ](/visualstudio/msbuild/item-element-msbuild)。 |
| *`.xml`* | 包含声明和初始化 IDE 用户界面元素的 XML 元素。 例如，属性表、属性页、textbox 控件和 listbox 控件。<br /><br /> *`.xml`* 文件直接支持 IDE，而不是 MSBuild。 不过 IDE 属性的值分配给生成属性和项。<br /><br /> 大多数 *`.xml`* 文件位于特定于区域设置的子目录中。 例如，美国英语区域的文件位于中 `$(VCTargetsPath)\1033\` 。 |

## <a name="user-targets-and-properties"></a>用户目标和属性

若要有效地使用 MSBuild，这有助于了解哪些属性和目标有用且相关。 大多数属性和目标都有助于实现 Visual Studio 生成系统，因此与用户无关。 本部分介绍了需要了解的面向用户的属性和目标。

### <a name="platformtoolset-property"></a>`PlatformToolset` 属性

`PlatformToolset` 属性确定生成中使用的 MSVC 工具集。 默认使用当前工具集。 设置此属性时，其值将与文本字符串连接，以形成路径。 它是包含为特定平台生成项目所需的属性和目标文件的目录。 若要使用某个平台工具集版本进行生成，则必须安装该平台工具集。

例如，将 `PlatformToolset` 属性设为 `v140`，以使用 Visual Studio 2015 工具和库来生成应用程序：

`msbuild myProject.vcxproj /p:PlatformToolset=v140`

### <a name="preferredtoolarchitecture-property"></a>`PreferredToolArchitecture` 属性

`PreferredToolArchitecture` 属性确定在生成中是使用 32 位还是 64 位编译器和工具。 此属性不会影响输出平台体系结构或配置。 默认情况下，如果未设置此属性，MSBuild 将使用 x86 版本的编译器和工具。

例如，将 `PreferredToolArchitecture` 属性设为 `x64`，以使用 64 位编译器和工具来生成应用程序：

`msbuild myProject.vcxproj /p:PreferredToolArchitecture=x64`

### <a name="useenv-property"></a>`UseEnv` 属性

默认情况下，当前项目的特定于平台的设置会覆盖 PATH、INCLUDE、LIB、LIBPATH、CONFIGURATION 和 PLATFORM 环境变量。 将 `UseEnv` 属性设置为， **`true`** 以确保不会重写环境变量。

> `msbuild myProject.vcxproj /p:UseEnv=true`

### <a name="targets"></a>目标

Visual Studio 支持文件中有数百个目标。 但是大多数目标都是面向系统的，用户可以忽略。 大多数系统目标以下划线 (`_`) ，或者名称以 `PrepareFor` 、、、、 `Compute` 或开头 `Before` `After` `Pre` `Post` 。

下表列出了一些有用的面向用户的目标。

| 目标 | 描述 |
| ------ | ----------- |
| BscMake | 执行 Microsoft 浏览信息维护实用工具 (bscmake.exe)。 |
| 构建 | 生成项目。<br /><br /> 此目标是项目的默认值。 |
| ClCompile | 执行 MSVC 编译器工具 (cl.exe)。 |
| clean | 删除临时生成文件和中间生成文件。 |
| Lib | 执行 Microsoft 32 位库管理器工具 (lib.exe)。 |
| 链接 | 执行 MSVC 链接器工具 (link.exe)。 |
| ManifestResourceCompile | 从清单中提取资源列表，然后执行 Microsoft Windows 资源编译器工具 (rc.exe)。 |
| Midl | 执行 Microsoft 接口定义语言 (MIDL) 编译器工具 (midl.exe)。 |
| 重新生成 | 清理然后生成项目。 |
| ResourceCompile | 执行 Microsoft Windows 资源编译器工具 (rc.exe)。 |
| XdcMake | 执行 XML 文档工具 (xdcmake.exe)。 |
| Xsd | 执行 XML 架构定义工具 (Xsd.exe)。 *请参阅注释。* |

> [!NOTE]
> 在 Visual Studio 2017 和更高版本中，已弃用对 xsd 文件的 C++ 项目支持  。 仍可通过向 GAC 手动添加 CppCodeProvider.dll 来使用 Microsoft.VisualC.CppCodeProvider  。

## <a name="see-also"></a>请参阅

[MSBuild 任务参考](/visualstudio/msbuild/msbuild-task-reference)\
[BscMake 任务](/visualstudio/msbuild/bscmake-task)\
[CL 任务](/visualstudio/msbuild/cl-task)\
[CPPClean 任务](/visualstudio/msbuild/cppclean-task)\
[LIB 任务](/visualstudio/msbuild/lib-task)\
[链接任务](/visualstudio/msbuild/link-task)\
[MIDL 任务](/visualstudio/msbuild/midl-task)\
[MT 任务](/visualstudio/msbuild/mt-task)\
[RC 任务](/visualstudio/msbuild/rc-task)\
[SetEnv 任务](/visualstudio/msbuild/setenv-task)\
[VCMessage 任务](/visualstudio/msbuild/vcmessage-task)\
[XDCMake 任务](/visualstudio/msbuild/xdcmake-task)
