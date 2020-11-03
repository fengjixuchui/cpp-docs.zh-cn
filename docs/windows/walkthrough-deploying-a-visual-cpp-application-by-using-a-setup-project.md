---
title: 使用安装项目部署 Visual C++ 应用程序
ms.date: 04/25/2019
helpviewer_keywords:
- deployment for Visual C++
ms.assetid: 66735cda-8fe3-4211-a19a-2cf717a12a3f
ms.openlocfilehash: 4ded30695647b3e9377bc35227283f367816edfa
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92918846"
---
# <a name="walkthrough-deploying-a-visual-c-application-by-using-a-setup-project"></a>演练：使用安装项目部署 Visual C++ 应用程序

介绍如何使用安装项目部署 Visual C++ 应用程序。

## <a name="prerequisites"></a>先决条件

您需要满足以下条件才能完成本演练：

- 已安装 Visual Studio 的计算机。

- 另一台没有 Visual C++ 库的计算机。

## <a name="create-the-setup-project"></a>创建安装项目

根据你安装的 Visual Studio 版本的不同，创建安装项目的说明有所不同。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker range="=msvc-160"

### <a name="to-create-the-project-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建项目

1. 在菜单栏上，选择“文件”>“新建”>“项目”，打开“创建新项目”对话框     。

   ![MFC 应用项目](media/vs2019-mfc-app-new-project.png "新 MFC 应用")

1. 在对话框顶部，在 " `MFC` 搜索" 框中键入，然后从 "结果" 列表中选择 " **MFC 应用** "。 如果看不到它，则需要从 Windows "开始" 菜单启动 Visual Studio 安装程序程序，然后单击 " **c + + 桌面开发工作负荷** " 磁贴。 在 " **各个组件** " 下，确保选中 "MFC 组件"。

1. 在下一页中，输入项目的名称，并指定项目位置（如果需要）。

1. 选择“创建”  按钮创建客户端项目。 **MFC 应用程序向导** 出现时，接受所有默认值。

1. 将活动解决方案配置更改为“发布”  。 从“生成”菜单中，选择“Configuration Manager”  。 从“Configuration Manager”对话框的“活动解决方案配置”下拉框中选择“发布”  。 单击“关闭”。

1. 按 **Ctrl** + **Shift** + **B** 生成应用程序。 或者，在“生成”菜单上，单击“生成解决方案”  。 生成应用程序可使安装项目使用此 MFC 应用程序项目的输出。

1. 如果尚未这样做，请下载 Microsoft Visual Studio 安装程序项目扩展。 此扩展可供 Visual Studio 开发人员免费使用，并向 Visual Studio 添加安装和部署项目模板功能。 连接到 Internet 后，在 Visual Studio 中选择 " **扩展** " "  >  **管理扩展** "。 在“扩展和更新”  对话框下，选择“联机”  选项卡并在搜索框中键入“Microsoft Visual Studio 安装程序项目”  。 按 **enter** ，选择 **Microsoft Visual Studio \<version> 安装程序项目** ，然后单击 " **下载** "。 选择运行并安装扩展，然后重新启动 Visual Studio。

   ![Visual Studio 安装项目](media/vs2019-extension-dialog-installer-project.png "为客户端项目命名")

1. 在 Visual Studio 菜单栏上，选择“文件”“最近使用的项目和解决方案”，然后选择重新打开项目  >  。

1. 在菜单栏上，选择 " **文件** " "  >  **新建**  >  **项目** " 以打开 "新建 **项目** " 对话框。 在搜索框中，键入“设置”，然后从结果列表中选择“设置项目”  。

1. 在“名称”框中输入安装项目的名称  。 在“解决方案”下拉列表中，选择“添加到解决方案”  。 选择“确定”按钮，创建安装项目  。 在编辑器窗口中打开“文件助手(ProjectName)”  选项卡。

::: moniker-end

::: moniker range="=msvc-150"

### <a name="to-create-the-project-in-visual-studio-2017"></a>在 Visual Studio 2017 中创建项目

1. 创建新项目。 在 **“文件”** 菜单上，指向 **“新建”** ，再单击 **“项目”** 。

1. 使用 **MFC 应用程序向导** 创建新的 Visual Studio 解决方案。 要查找该向导，请在“新建项目”对话框中展开“Visual C++”节点，依次选择“MFC”、“MFC 应用程序”，输入项目的名称，然后单击“确定”  。 单击“完成”。

   > [!NOTE]
   > 如果缺少 **MFC 应用程序** 类型，请在 " **新建项目** " 对话框的左窗格中选择 " **打开 Visual Studio 安装程序** "。 安装位于“可选”  组件部分中“使用 C++ 的桌面开发”  下名为“适用于 x86 和 x64 的 Visual C++ MFC”  的选项。

1. 将活动解决方案配置更改为“发布”  。 从“生成”菜单中，选择“Configuration Manager”  。 从“Configuration Manager”对话框的“活动解决方案配置”下拉框中选择“发布”  。 单击“关闭”。

1. 按 **Ctrl** + **Shift** + **B** 生成应用程序。 或者，在“生成”菜单上，单击“生成解决方案”  。 生成应用程序可使安装项目使用此 MFC 应用程序项目的输出。

1. 如果尚未这样做，请下载 Microsoft Visual Studio 安装程序项目扩展。 此扩展可供 Visual Studio 开发人员免费使用，并向 Visual Studio 添加安装和部署项目模板功能。 连接到 Internet 后，在 Visual Studio 中选择 " **工具** " "  >  **扩展和更新** "。 在“扩展和更新”  对话框下，选择“联机”  选项卡并在搜索框中键入“Microsoft Visual Studio 安装程序项目”  。 按 **enter** ，选择 **Microsoft Visual Studio \<version> 安装程序项目** ，然后单击 " **下载** "。 选择运行并安装扩展，然后重新启动 Visual Studio。

1. 在菜单栏上，依次选择“文件”  ，然后选择重新打开项目。

1. 在菜单栏上，选择 **File**  >  **New**  >  "文件" "新建 **项目** " 以打开 " **新建项目** " 对话框。 然后，在对话框的左窗格中，展开 " **已安装** 的  >  **其他项目类型** " 节点，然后选择 " **Visual Studio 安装程序** "。 在中间窗格中，选择“安装项目”  。

1. 在“名称”框中输入安装项目的名称  。 在“解决方案”下拉列表中，选择“添加到解决方案”  。 选择“确定”按钮，创建安装项目  。 在编辑器窗口中打开“文件助手(ProjectName)”  选项卡。

::: moniker-end

::: moniker range="=msvc-140"

### <a name="to-create-the-project-in-visual-studio-2015"></a>在 Visual Studio 2015 中创建项目

1. 创建新项目。 在 **“文件”** 菜单上，指向 **“新建”** ，再单击 **“项目”** 。

1. 使用 **MFC 应用程序向导** 创建新的 Visual Studio 解决方案。 要查找该向导，请在“新建项目”对话框中展开“Visual C++”节点，依次选择“MFC”、“MFC 应用程序”，输入项目的名称，然后单击“确定”  。 单击“完成”。

   > [!NOTE]
   > 如果缺少 **MFC 应用程序** 类型，请单击 Windows 的 "开始" 按钮，然后键入 " **添加删除程序** "。 从结果列表打开程序，然后在已安装程序列表中找到你的 Microsoft Visual Studio 2015 安装程序。 双击它，然后选择“修改”  ，并选择“Visual C++”  下的“Microsoft 基础类”  组件。

1. 将活动解决方案配置更改为“发布”  。 在 " **生成** " 菜单中，选择 **Configuration Manager** 。 从“Configuration Manager”对话框的“活动解决方案配置”下拉框中选择“发布”  。 单击“关闭”。

1. 按 **Ctrl** + **Shift** + **B** 生成应用程序。 或者，在“生成”菜单上，单击“生成解决方案”  。 生成应用程序可使安装项目使用此 MFC 应用程序项目的输出。

1. 如果尚未这样做，请下载 Microsoft Visual Studio 安装程序项目扩展。 此扩展可供 Visual Studio 开发人员免费使用，并向 Visual Studio 添加安装和部署项目模板功能。 连接到 Internet 后，在 Visual Studio 中选择 " **工具** " "  >  **扩展和更新** "。 在“扩展和更新”  对话框下，选择“联机”  选项卡并在搜索框中键入“Microsoft Visual Studio 安装程序项目”  。 按 **enter** ，选择 **Microsoft Visual Studio \<version> 安装程序项目** ，然后单击 " **下载** "。 选择运行并安装扩展，然后重新启动 Visual Studio。

1. 在菜单栏上，依次选择“文件”  ，然后选择重新打开项目。

1. 在菜单栏上，选择 **File**  >  **New**  >  "文件" "新建 **项目** " 以打开 " **新建项目** " 对话框。 然后，在对话框的左窗格中，展开 " **已安装** 的  >  **其他项目类型** " 节点，然后选择 " **Visual Studio 安装程序** "。 在中间窗格中，选择“安装项目”  。

1. 在“名称”框中输入安装项目的名称  。 在“解决方案”下拉列表中，选择“添加到解决方案”  。 选择“确定”按钮，创建安装项目  。 在编辑器窗口中打开“文件助手(ProjectName)”  选项卡。

::: moniker-end

## <a name="add-items-to-the-project"></a>向项目添加项

1. 右键单击“应用程序文件夹”节点，然后选择“添加” > “项目输出”以打开“添加项目输出组”对话框  。 在对话框中，选择“主输出”  ，然后单击“确定”  。 将出现名为“ProjectName (活动)主输出”  的新项。

1. 右键单击“应用程序文件夹”  节点，然后选择“添加”  > “程序集”  以打开“选择组件”  对话框。 选择并添加程序所需的任何 DLL，如文章[确定要重新分发的 DLL](determining-which-dlls-to-redistribute.md) 中所述。

1. 选择“ProjectName (活动)主输出”项  ，右键单击并选择“创建 ProjectName (活动)主输出的快捷方式”  。 将出现名为“ProjectName (活动)主输出的快捷方式”  的新项。 可以重命名快捷方式项，然后将该项拖放到窗口左侧的“用户程序菜单”  节点。

1. 在菜单栏上，依次选择“生成” > “Configuration Manager” 。 在“项目”  表的“生成”  列下，勾选部署项目框。 单击“关闭”。

1. 在菜单栏上，选择 " **生成**  >  **生成解决方案** " 以生成 MFC 项目和部署项目。

1. 在解决方案文件夹中，找到从部署项目中生成的 setup.exe 程序。 可在另一台计算机上复制此文件（和 .msi 文件），来安装应用程序及其所需的库文件。 在没有 Visual C++ 库的第二台计算机上运行安装程序。

## <a name="see-also"></a>请参阅

[部署示例](deployment-examples.md)<br/>
