---
title: 创建 MFC 应用程序
ms.date: 08/28/2019
helpviewer_keywords:
- applications [MFC]
- MFC, creating applications
- MFC applications
ms.assetid: b8b8aa08-9c49-404c-8078-b42079ac18f0
ms.openlocfilehash: dd4a0b8a7a06debdc3556d48e000fe09deccf857
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924424"
---
# <a name="creating-an-mfc-application"></a>创建 MFC 应用程序

MFC 应用程序是基于 Microsoft 基础类 (MFC) 库的 Windows 可执行应用程序。 MFC 可执行文件通常分为五类：标准 Windows 应用程序、对话框、基于窗体的应用程序、资源管理器样式的应用程序和 Web 浏览器样式的应用程序。 有关详细信息，请参阅：

- [使用类编写 Windows 应用程序](../../mfc/using-the-classes-to-write-applications-for-windows.md)

- [创建和显示对话框](../../mfc/creating-and-displaying-dialog-boxes.md)

- [创建 Forms-Based MFC 应用程序](../../mfc/reference/creating-a-forms-based-mfc-application.md)

- [Explorer-Style MFC 应用程序创建文件](../../mfc/reference/creating-a-file-explorer-style-mfc-application.md)

- [创建 Web Browser-Style MFC 应用程序](../../mfc/reference/creating-a-web-browser-style-mfc-application.md)

根据在向导中选择的选项，MFC 应用程序向导为上述任何应用程序生成适当的类和文件。

创建 MFC 应用程序的最简单方法是使用 MFC 应用程序向导 (Visual Studio 2019) 中的 **Mfc 应用程序项目** 。 若要创建 MFC 控制台应用程序 (使用 MFC 库但在控制台窗口中运行的命令行程序) ，请使用 Windows 桌面向导，然后选择 " **控制台应用程序** " 和 " **MFC 标头** " 选项。

::: moniker range=">=msvc-160"

## <a name="to-create-an-mfc-forms-or-dialog-based-application"></a>创建 MFC 窗体或基于对话框的应用程序

1. 从主菜单中，选择 " **文件** " " > **新建** > **项目** "。
1. 在搜索框中输入 "MFC"，然后从结果列表中选择 " **Mfc 应用** "。
1. 根据需要修改默认设置，然后按 " **创建** " 以打开 " **MFC 应用程序向导** "。
1. 根据需要修改配置值，然后按 " **完成** "。

有关详细信息，请参阅 [创建 Forms-Based MFC 应用程序](creating-a-forms-based-mfc-application.md)。

![Visual studio 2019 中 MFC 应用程序向导的屏幕截图。](media/mfc-app-wizard.png)

## <a name="to-create-an-mfc-console-application"></a>创建 MFC 控制台应用程序

MFC 控制台应用程序是一个使用 MFC 库但在控制台窗口中运行的命令行程序。

1. 从主菜单中，选择 " **文件** " " > **新建** > **项目** "。
1. 在搜索框中输入 "桌面"，然后从 "结果" 列表中选择 " **Windows 桌面向导** "。
1. 根据需要修改项目名称，然后按 " **下一步** " 以打开 **Windows 桌面向导** 。
1. 选中 " **MFC 标头** " 框并根据需要设置其他值，然后按 " **完成** "。

![Visual studio 2019 中 Windows 桌面向导的屏幕截图。](media/windows-desktop-wizard.png)

::: moniker-end

::: moniker range="=msvc-150"

## <a name="to-create-an-mfc-forms-or-dialog-based-application"></a>创建 MFC 窗体或基于对话框的应用程序

1. 从主菜单中，选择 " **文件** " " > **新建** > **项目** "。
1. 在 " **已安装** 的模板" 下，选择 " **Visual C++**  >  **MFC/ATL** "。 如果看不到这些文件，请使用 Visual Studio 安装程序添加它们。
1. 从中间窗格中选择 " **MFC 应用程序** "。
1. 根据需要修改配置值，然后按 " **完成** "。

有关详细信息，请参阅 [创建 Forms-Based MFC 应用程序](creating-a-forms-based-mfc-application.md)。

![Visual studio 2017 中 MFC 应用程序向导的屏幕截图。](media/mfc-app-wizard.png)

## <a name="to-create-an-mfc-console-application"></a>创建 MFC 控制台应用程序

MFC 控制台应用程序是一个使用 MFC 库但在控制台窗口中运行的命令行程序。

1. 从主菜单中，选择 " **文件** " " > **新建** > **项目** "。
1. 在 " **已安装** 的模板" 下，选择 " **Visual C++** > **Windows 桌面** "。
1. 从中间窗格中选择 " **Windows 桌面向导** "。
1. 根据需要修改项目名称，然后按 **"确定"** 打开 **Windows 桌面向导** 。
1. 选中 " **MFC 标头** " 框并根据需要设置其他值，然后按 " **完成** "。

![Visual studio 2017 中 Windows 桌面向导的屏幕截图。](media/windows-desktop-wizard-2017.png)

::: moniker-end

::: moniker range="=msvc-140"

## <a name="to-create-an-mfc-forms-or-dialog-based-application"></a>创建 MFC 窗体或基于对话框的应用程序

1. 从主菜单中，选择 " **文件** " " > **新建** > **项目** "。
1. 在 " **已安装** 的模板" 下，选择 " **Visual C++** > **MFC** "。
1. 从中间窗格中选择 " **MFC 应用程序** "。
1. 单击 " **下一步** " 以启动 " **MFC 应用程序向导** "。

有关详细信息，请参阅 [创建 Forms-Based MFC 应用程序](creating-a-forms-based-mfc-application.md)。

![Visual studio 2015 中 MFC 应用程序向导的屏幕截图。](media/mfc-app-wizard-2015.png)

## <a name="to-create-an-mfc-console-application"></a>创建 MFC 控制台应用程序

MFC 控制台应用程序是一个使用 MFC 库但在控制台窗口中运行的命令行程序。

1. 从主菜单中，选择 " **文件** " " > **新建** > **项目** "。
1. 在 " **已安装** 的模板" 下，选择 " **Visual C++** > **Win32** "。
1. 从中间窗格中选择 " **Win32 控制台应用程序** "。
1. 根据需要修改项目名称，然后按 **"确定"** 。
1. 在向导的第二页上，选中 "为 **MFC 添加常用标头** " 框，并根据需要设置其他值，然后按 " **完成** "。

::: moniker-end

创建项目后，可以在“解决方案资源管理器”中查看创建的文件  。 有关向导为项目创建的文件的更多信息，请参见项目生成的文件 ReadMe.txt。 有关文件类型的详细信息，请参阅[为 Visual Studio C++ 项目创建的文件类型](../../build/reference/file-types-created-for-visual-cpp-projects.md)。

## <a name="see-also"></a>请参阅

[用代码向导添加功能](../../ide/adding-functionality-with-code-wizards-cpp.md)<br/>
[属性页](../../build/reference/property-pages-visual-cpp.md)
