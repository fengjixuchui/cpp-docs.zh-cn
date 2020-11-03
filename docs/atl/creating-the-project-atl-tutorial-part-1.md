---
title: 创建项目（ATL 教程，第 1 部分）
ms.custom: get-started-article
ms.date: 08/19/2019
ms.assetid: f6b727d1-390a-4b27-b82f-daadcd9fc059
ms.openlocfilehash: 8a0374fcaf9a18c82e90bde2abc032158ecc537a
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92923871"
---
# <a name="creating-the-project-atl-tutorial-part-1"></a>创建项目（ATL 教程，第 1 部分）

本教程指导你逐步完成一个非属性化 ATL 项目，该项目创建了一个显示多边形的 ActiveX 对象。 对象包括允许用户更改多边形的边数的选项，以及用于刷新显示的代码。

> [!NOTE]
> 本教程将创建与多边形示例相同的源代码。 如果您想要避免手动输入源代码，可以从 [多边形示例抽象](https://github.com/Microsoft/VCSamples/tree/master/VC2008Samples/ATL/Controls/Polygon)下载。 然后，您可以在学习本教程时参考多边形源代码，或使用它来检查您自己的项目中是否存在错误。
> 若要编译， *请打开 Visual* Studio 2017 和) 更早版本中 ( *stdafx.h* ，并替换：
>
> ```
> #ifndef WINVER
> #define WINVER 0x0400
> #endif
> ```
>
> 替换为
>
> ```
> #ifndef WINVER
> #define WINVER 0x0500
> #define _WIN32_WINNT 0x0500
> #endif
> ```
>
> 编译器仍会抱怨 `regsvr32` 未正确退出，但你仍应为控件的 DLL 生成并可供使用。

### <a name="to-create-the-initial-atl-project-using-the-atl-project-wizard"></a>使用 ATL 项目向导创建初始 ATL 项目

1. 在 Visual Studio 2017 及更早版本中： **文件**  >  **新**  >  **项目** 。 打开 " **Visual C++** " 选项卡，然后选择 " **MFC/ATL** "。 选择 " **ATL 项目** "。

   在 Visual Studio 2019 中：选择 " **文件**  >  " " **新建**  >  **项目** "，在搜索框中键入 "atl"，然后选择 " **atl 项目** "。

1. 键入 *多边形* 作为项目名称。

    源代码的位置通常默认为 \Users \\ \<username> \source\repos，并将自动创建一个新文件夹。

1. 在 Visual Studio 2019 中，接受默认值，然后单击 **"确定"** 。
   在 Visual Studio 2017 中，单击 **"确定"** 以打开 **ATL 项目** 向导。 单击 " **应用程序设置** " 以查看可用选项。 由于此项目创建一个控件，并且控件必须是进程内服务器，因此将 **应用程序类型** 保留为 DLL。 单击 **“确定”** 。

Visual Studio 将通过生成一些文件来创建项目。 您可以通过展开对象在 **解决方案资源管理器** 中查看这些文件 `Polygon` 。 下面列出了这些文件。

::: moniker range="<=msvc-150"

|文件|说明|
|----------|-----------------|
|多边形 .cpp|包含、、、 `DllMain` 和的实现 `DllCanUnloadNow` `DllGetClassObject` `DllRegisterServer` `DllUnregisterServer` 。 还包含对象映射，这是项目中 ATL 对象的列表。 这最初是空白的。|
|多边形 .def|此模块定义文件为链接器提供有关 DLL 所需的导出的信息。|
|多边形 .idl|接口定义语言文件，描述特定于对象的接口。|
|多边|此注册表脚本包含用于注册程序的 DLL 的信息。|
|多边形 .rc|资源文件，最初包含版本信息和包含项目名称的字符串。|
|Resource.h|资源文件的头文件。|
|Polygonps|此模块定义文件为链接器提供有关代理所需的导出的信息以及支持跨单元调用的存根代码。|
|stdafx.cpp|将 stdafx.h 的文件 `#include` *stdafx.h* 。|
|stdafx.h|将 `#include` 和预编译 ATL 标头文件的文件。|

::: moniker-end

::: moniker range=">=msvc-160"

|文件|说明|
|----------|-----------------|
|多边形 .cpp|包含、、、 `DllMain` 和的实现 `DllCanUnloadNow` `DllGetClassObject` `DllRegisterServer` `DllUnregisterServer` 。 还包含对象映射，这是项目中 ATL 对象的列表。 这最初是空白的。|
|多边形 .def|此模块定义文件为链接器提供有关 DLL 所需的导出的信息。|
|多边形 .idl|接口定义语言文件，描述特定于对象的接口。|
|多边|此注册表脚本包含用于注册程序的 DLL 的信息。|
|多边形 .rc|资源文件，最初包含版本信息和包含项目名称的字符串。|
|Resource.h|资源文件的头文件。|
|Polygonps|此模块定义文件为链接器提供有关代理所需的导出的信息以及支持跨单元调用的存根代码。|
|pch|将为 `#include` *pch* 的文件。|
|pch。h|将 `#include` 和预编译 ATL 标头文件的文件。|

::: moniker-end

1. 在 **解决方案资源管理器** 中，右键单击 `Polygon` 项目。

1. 在快捷菜单上，单击 " **属性** "。

1. 单击 " **链接器** "。 将 **UserRedirection** 选项更改为 **Yes** 。

1. 单击 **“确定”** 。

在下一步中，你将向你的项目添加一个控件。

[到步骤2](../atl/adding-a-control-atl-tutorial-part-2.md)

## <a name="see-also"></a>请参阅

[教程](../atl/active-template-library-atl-tutorial.md)
