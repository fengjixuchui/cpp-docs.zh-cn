---
title: 演练：使用新的 MFC Shell 控件
ms.date: 04/25/2019
helpviewer_keywords:
- shell controls (MFC)
ms.assetid: f0015caa-199d-4aaf-9501-5a239fce9095
ms.openlocfilehash: 0d8db9044a64305bd7bb9ef6fe10de9ecef1ce51
ms.sourcegitcommit: 9c2b3df9b837879cd17932ae9f61cdd142078260
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924748"
---
# <a name="walkthrough-using-the-new-mfc-shell-controls"></a>演练：使用新的 MFC Shell 控件

在本演练中，你将创建一个类似于文件资源管理器的应用程序。 您将创建一个具有两个窗格的窗口。 左窗格将包含一个 [CMFCShellTreeCtrl](../mfc/reference/cmfcshelltreectrl-class.md) 对象，该对象在层次结构视图中显示桌面。 右窗格将包含一个 [CMFCShellListCtrl](../mfc/reference/cmfcshelllistctrl-class.md) ，显示在左窗格中选择的文件夹中的文件。

## <a name="prerequisites"></a>先决条件

- 在 Visual Studio 2017 和更高版本中，MFC 支持是一个可选组件。 若要安装它，请从 Windows "开始" 菜单打开 Visual Studio 安装程序。 找到你正在使用的 Visual Studio 版本，然后选择 " **修改** " 按钮。 请确保已选中 " **带 c + + 的桌面开发** " 磁贴。 在 " **可选组件** " 下，查看 **MFC 支持** 按钮。

- 本演练假设已将 Visual Studio 设置为使用 **常规开发设置** 。 如果使用的是其他开发设置，则默认情况下，我们在本演练中使用的某些 Visual Studio 窗口可能不会显示。

## <a name="to-create-a-new-mfc-application-by-using-the-mfc-application-wizard"></a>使用 MFC 应用程序向导创建新的 MFC 应用程序

这些步骤取决于所使用的 Visual Studio 版本。 若要查看 Visual Studio 首选项的文档，请使用“版本”选择器控件。 它位于此页面上目录表的顶部。

::: moniker range="msvc-160"

### <a name="to-create-an-mfc-project-in-visual-studio-2019"></a>在 Visual Studio 2019 中创建 MFC 项目

1. 在主菜单中，依次选择“文件”>“新建”>“项目”，以打开“新建项目”对话框。

1. 在顶部的 "搜索" 框中，键入 " **mfc** "，然后从 "结果" 列表中选择 " **mfc 应用** "。

1. 单击“下一步”。 在下一页中，输入项目的名称，并指定项目位置（如果需要）。

1. 选择“创建”  按钮创建项目。

   显示 **MFC 应用程序向导** 后，使用以下选项：

   1. 选择左侧的 " **应用程序类型** "。 然后选择 " **单个文档** "，并选择 " **文档/视图体系结构支持** "。 在 " **项目样式** " 下，选择 " **visual Studio** "，然后从 " **视觉样式和颜色** " 下拉列表中选择 " **Office 2007 (蓝色主题)** 。

   1. 在 " **复合文档支持** " 窗格上，选择 " **无** "。

   1. 不要对 **文档模板 "属性** " 窗格进行任何更改。

   1. 在 " **用户界面功能** " 窗格上，确保已选中 " **使用菜单栏和工具栏** " 选项。 保留所有其他选项。

   1. 在 " **高级功能** " 窗格中，选择 " **ActiveX 控件** "、" **公共控件清单** " 和 " **导航窗格** 选项"。 将所有其他内容保留原样。 **导航窗格** 选项将使向导在窗口的左侧创建一个 `CMFCShellTreeCtrl` 已嵌入的窗格。

   1. 我们不会对 " **生成的类** " 窗格进行任何更改，因此请单击 " **完成** " 以创建新的 MFC 项目。

::: moniker-end

::: moniker range="<=msvc-150"

### <a name="to-create-an-mfc-project-in-visual-studio-2017-or-earlier"></a>在 Visual Studio 2017 或更早版本中创建 MFC 项目

1. 使用 **Mfc 应用程序向导** 创建新的 MFC 应用程序。 若要运行该向导，请从 " **文件** " 菜单中选择 " **新建** "，然后选择 " **项目** "。 将显示 " **新建项目** " 对话框。

1. 在 " **新建项目** " 对话框中，展开 " **项目类型** " 窗格中的 " **Visual C++** " 节点，然后选择 " **MFC** "。 然后，在 " **模板** " 窗格中选择 " **MFC 应用程序** "。 键入项目的名称，例如， `MFCShellControls` 然后单击 **"确定"** 。

   显示 **MFC 应用程序向导** 后，使用以下选项：

   1. 在 " **应用程序类型** " 窗格中的 " **应用程序类型** " 下，清除 " **选项卡式文档** " 选项。 接下来，选择 " **单个文档** "，并选择 " **文档/视图体系结构支持** "。 在 " **项目样式** " 下，选择 " **visual Studio** "，然后从 " **视觉样式和颜色** " 下拉列表中选择 " **Office 2007 (蓝色主题)** 。

   1. 在 " **复合文档支持** " 窗格上，选择 " **无** "。

   1. 不要对 **文档模板字符串** 窗格进行任何更改。

   1. 在 " **数据库支持** " 窗格 (Visual Studio 2015 及更早版本) 上，选择 " **无** "，因为应用程序不使用数据库。

   1. 在 " **用户界面功能** " 窗格上，确保已选中 " **使用菜单栏和工具栏** " 选项。 保留所有其他选项。

   1. 在 " **高级功能** " 窗格中的 " **高级功能** " 下，仅选择 " **ActiveX 控件** " 和 " **公共控件清单** "。 在 " **高级框架" 窗格** 下，仅选择 " **导航窗格** " 选项。 它将使向导在窗口的左侧创建一个 `CMFCShellTreeCtrl` 已嵌入的窗格。

   1. 我们不会对 " **生成的类** " 窗格进行任何更改，因此请单击 " **完成** " 以创建新的 MFC 项目。

::: moniker-end

通过生成并运行该应用程序，验证该应用程序是否已成功创建。 若要生成应用程序，请从 " **生成** " 菜单中选择 " **生成解决方案** "。 如果应用程序成功生成，则通过从 " **调试** " 菜单中选择 " **启动调试** " 来运行该应用程序。

向导会自动创建一个应用程序，该应用程序具有标准菜单栏、标准工具栏、标准状态栏和窗口左侧的 Outlook 栏以及 " **文件夹** " 视图和 " **日历** " 视图。

### <a name="to-add-the-shell-list-control-to-the-document-view"></a>向文档视图添加 shell 列表控件

1. 在本部分中，您将向向导创建的视图中添加一个实例 `CMFCShellListCtrl` 。 在 **解决方案资源管理器** 中双击 " **MFCShellControlsView** "，打开视图标头文件。

   在 `#pragma once` 标头文件的顶部附近找到指令。 紧靠在它下面添加以下代码，以包含的头文件 `CMFCShellListCtrl` ：

   ```cpp
   #include <afxShellListCtrl.h>
   ```

   现在，添加类型的成员变量 `CMFCShellListCtrl` 。 首先，在标头文件中找到以下注释：

   ```cpp
   // Generated message map functions
   ```

   紧接着该注释，添加以下代码：

   ```cpp
   private:
   CMFCShellListCtrl m_wndList;
   ```

1. **MFC 应用程序向导** 已在类中创建了一个 `CMFCShellTreeCtrl` 对象 `CMainFrame` ，但它是一个受保护的成员。 稍后我们将访问该对象，因此现在为其创建访问器。 在 **解决方案资源管理器** 中双击 mainfrm.cpp 头文件，将其打开。 找到以下注释：

   ```cpp
   // Attributes
   ```

   紧靠在它下面添加以下方法声明：

   ```cpp
   public:
       CMFCShellTreeCtrl& GetShellTreeCtrl();
   ```

   接下来，通过在 **解决方案资源管理器** 中双击来打开 mainfrm.cpp 源文件。 在该文件的底部，添加以下方法定义：

   ```cpp
   CMFCShellTreeCtrl& CMainFrame::GetShellTreeCtrl()
   {
        return m_wndTree;
   }
   ```

1. 现在，我们更新 `CMFCShellControlsView` 类以处理 `WM_CREATE` windows 消息。 打开 " **类视图** " 窗口并选择 " `CMFCShellControlsView` 类"。 右键单击并选择 **“属性”** 。

   接下来，在 [类向导](reference/mfc-class-wizard.md)中，单击 " **消息** " 选项卡。向下滚动，直到找到该 `WM_CREATE` 消息。 从旁的下拉列表中 `WM_CREATE` ，选择 " **\<Add> OnCreate** "。 命令为我们创建消息处理程序并自动更新 MFC 消息映射。

   在 `OnCreate` 方法中，我们将创建 `CMFCShellListCtrl` 对象。 `OnCreate`在 MFCShellControlsView 源文件中查找方法定义，并将其实现替换为以下代码：

    ```cpp
    int CMFCShellControlsView::OnCreate(LPCREATESTRUCT lpCreateStruct)
    {
        if (CView::OnCreate(lpCreateStruct) == -1)
            return -1;

        CRect rectDummy (0, 0, 0, 0);

        m_wndList.Create(WS_CHILD | WS_VISIBLE | LVS_REPORT,
            rectDummy, this, 1);

        return 0;
    }
    ```

1. 重复上述步骤，但对 `WM_SIZE` 消息重复。 当用户更改应用程序窗口的大小时，将导致应用程序视图被重绘。 将方法的定义替换为 `OnSize` 以下代码：

    ```cpp
    void CMFCShellControlsView::OnSize(UINT nType, int cx, int cy)
    {
        CView::OnSize(nType, cx, cy);

        m_wndList.SetWindowPos(NULL, -1, -1, cx, cy,
            SWP_NOMOVE | SWP_NOZORDER | SWP_NOACTIVATE);
    }
    ```

1. 最后一步是 `CMFCShellTreeCtrl` `CMFCShellListCtrl` 使用 [CMFCShellTreeCtrl：： SetRelatedList](../mfc/reference/cmfcshelltreectrl-class.md#setrelatedlist) 方法连接和对象。 调用后 `CMFCShellTreeCtrl::SetRelatedList` ， `CMFCShellListCtrl` 将自动显示中选定项的内容 `CMFCShellTreeCtrl` 。 我们将连接方法中的对象 `OnActivateView` ，该方法从 [CView：： OnActivateView](../mfc/reference/cview-class.md#onactivateview)重写。

   在 MFCShellControlsView 头文件的 `CMFCShellControlsView` 类声明中，添加以下方法声明：

    ```cpp
    protected:
    virtual void OnActivateView(BOOL bActivate,
        CView* pActivateView,
        CView* pDeactiveView);
    ```

   接下来，将方法的定义添加到 MFCShellControlsView 源文件中：

    ```cpp
    void CMFCShellControlsView::OnActivateView(BOOL bActivate,
        CView* pActivateView,
        CView* pDeactiveView)
    {
        if (bActivate&& AfxGetMainWnd() != NULL)
        {
            ((CMainFrame*)AfxGetMainWnd())->GetShellTreeCtrl().SetRelatedList(&m_wndList);
        }

        CView::OnActivateView(bActivate,
            pActivateView,
            pDeactiveView);
    }
    ```

   由于我们要从类调用方法 `CMainFrame` ，因此必须 `#include` 在 MFCShellControlsView 源文件的顶部添加指令：

    ```cpp
    #include "MainFrm.h"
    ```

1. 通过生成并运行该应用程序，验证该应用程序是否已成功创建。 若要生成应用程序，请从 " **生成** " 菜单中选择 " **生成解决方案** "。 如果应用程序成功生成，则通过从 " **调试** " 菜单中选择 " **启动调试** " 来运行该应用程序。

   现在，应会在 "视图" 窗格中的中查看所选项目的详细信息 `CMFCShellTreeCtrl` 。 当单击中的某个节点时 `CMFCShellTreeCtrl` ， `CMFCShellListCtrl` 将自动更新。 同样，如果双击中的文件夹 `CMFCShellListCtrl` ，则 `CMFCShellTreeCtrl` 应自动更新。

   右键单击树控件或列表控件中的任何项。 您将获得与使用实际 **文件资源管理器** 相同的上下文菜单。

## <a name="next-steps"></a>后续步骤

- 向导创建了一个具有 " **文件夹** " 窗格和 " **日历** " 窗格的 Outlook 面板。 在 **资源管理器** 窗口中使用 **日历** 窗格可能并不合理，因此请立即删除该窗格。

- `CMFCShellListCtrl`支持查看不同模式下的文件，例如 **大图标** 、 **小图标** 、 **列表** 和 **详细信息** 。 更新应用程序以实现此功能。 提示：请参阅 [Visual C++ 示例](../overview/visual-cpp-samples.md)。

## <a name="see-also"></a>请参阅

[演练](../mfc/walkthroughs-mfc.md)
