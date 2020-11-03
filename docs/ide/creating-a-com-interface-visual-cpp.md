---
title: 创建 COM 接口
ms.date: 05/14/2019
helpviewer_keywords:
- COM interfaces, creating
- methods [C++], adding to COM interfaces
- COM interfaces, editing
- properties [C++], adding to COM interfaces
ms.assetid: 1be84d3c-6886-4d1e-8493-56c4d38a96d4
ms.openlocfilehash: 6ad8d50049d34a711937f3d1f73157ce26f69808
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91509688"
---
# <a name="create-a-com-interface"></a>创建 COM 接口

Visual Studio 提供了向导和模板，用于创建使用 COM 为 COM 对象和自动化类定义接口及调度接口的项目。

这些向导可用于执行以下三个常见任务：

- [向 MFC 项目添加 ATL 支持](../mfc/reference/adding-atl-support-to-your-mfc-project.md)。

  在使用 [MFC 应用程序向导](../mfc/reference/mfc-application-wizard.md)并运行“向 MFC 添加 ATL 支持”代码向导创建 MFC 项目之后，将 ATL 支持添加到 MFC 应用程序****。 此支持仅适用于添加到 MFC 可执行文件或 DLL 项目的简单 COM 对象。 这些 ATL 对象可能有多个接口。

- [创建 MFC ActiveX 控件](../mfc/reference/creating-an-mfc-activex-control.md)。

  打开 [MFC ActiveX 控件向导](../mfc/reference/mfc-activex-control-wizard.md)创建一个 ActiveX 控件，该控件在 .idl 文件中定义了一个调度接口，在控件类中定义了一个事件映射。

- [添加 ATL control](../atl/reference/adding-an-atl-control.md).

  结合使用 [ATL 项目向导](../atl/reference/atl-project-wizard.md)和 [ATL 控件向导](../atl/reference/atl-control-wizard.md)创建 ATL ActiveX 控件。

  还可将 ATL 控件添加到已具有 ATL 支持的 MFC 项目中，如上所述。 此外，如果在“添加类”对话框中选择“ATL 控件”，但尚未将 ATL 支持添加到 MFC 项目，Visual Studio 将显示一个对话框，要求确认是否将 ATL 支持添加到 MFC 项目********。

  此向导在项目类中生成 IDL 源和 COM 映射。

打开 ATL 项目后，可在[添加类](./adding-a-class-visual-cpp.md#add-class-dialog-box)对话框选择其他向导和模板，以将 COM 接口添加到项目中。 以下向导允许为对象建立一个或多个接口：

- [ATL COM+ 1.0 组件向导](../atl/reference/atl-com-plus-1-0-component-wizard.md)
- [ATL 简单对象向导](../atl/reference/atl-simple-object-wizard.md)
- [ATL Active Server Page 组件向导](../atl/reference/atl-active-server-page-component-wizard.md)
- [ATL 控件向导](../atl/reference/atl-control-wizard.md)

此外，还可以在 COM 控件上实现新接口。 只需在“类视图”中右键单击对象的控件类，然后选择[实现接口](./implementing-an-interface-visual-cpp.md#implement-interface-wizard)。

> [!NOTE]
> Visual Studio 不提供将接口添加到项目的向导。 可将接口添加到 ATL 项目或通过使用 [ATL 简单对象向导](../atl/reference/atl-simple-object-wizard.md)添加一个简单对象，从而[向 MFC 项目添加 ATL 支持](../mfc/reference/adding-atl-support-to-your-mfc-project.md)。 或者打开项目的 .idl 文件并通过键入以下内容创建接口：

```
interface IMyInterface {
};
```

有关详细信息，请参阅[实现接口](../ide/implementing-an-interface-visual-cpp.md)和[向 ATL 项目添加对象和控件](../atl/reference/adding-objects-and-controls-to-an-atl-project.md)。

Visual C++ 提供了多种方式来查看和[编辑为项目定义的 COM 接口](#edit-a-com-interface)。 [类视图](/visualstudio/ide/viewing-the-structure-of-code)显示 C++ 项目中 .idl 文件内定义的任何接口或调度接口的图标。

对于基于 ATL 的 COM 对象类，“类视图”读取 ATL 类中的 COM 映射，以显示 ATL 类与其实现的任何接口之间的关系。

在“类视图”及其快捷菜单中，可使用以下接口：

- 将 ATL 对象添加到基于 MFC 的应用程序。
- 添加方法、属性和事件。
- 双击项目直接跳转到该项的接口代码。

## <a name="in-this-section"></a>在此部分中

- [编辑 COM 接口](#edit-a-com-interface)

## <a name="edit-a-com-interface"></a>编辑 COM 接口

使用“类视图”上下文菜单的命令，可以在 Visual Studio C++ 项目中为 COM 接口定义新方法和属性。 此外，还可以从工具箱定义 ActiveX 控件的事件。

对于 ATL 和基于 MFC 的 COM 对象类，可以在编辑接口的同时编辑类实现。

> [!NOTE]
> 对于从“添加类”对话框以外定义的接口，Visual C++ 会将方法或属性添加到 .idl 文件，并将存根添加到实现方法的类，即使已手动添加接口也是如此****。

以下三个向导有助于自定义现有接口。 可以从类视图中获取：

|向导|项目类型|
|------------|------------------|
|[添加属性向导](./adding-a-property-visual-cpp.md#names-add-property-wizard)|支持 ATL 的 ATL 或 MFC 项目。 右键单击要添加属性的接口。<br /><br />Visual C++ 会检测项目类型并根据需要修改“添加属性向导”中的选项：<br /><br />- 对于使用 [MFC 应用程序向导](../mfc/reference/mfc-application-wizard.md)创建的项目中的调度接口，调用“添加属性向导”将提供特定于 MFC 的选项。<br />- 对于 MFC ActiveX 控件接口，“添加属性向导”将提供一系列常用方法和属性，你可为你的控件使用所述的这些方法和属性，也可进行自定义。<br />- 对于所有其他接口，“添加属性向导”将提供可用于大多数情况的选项。|
|[添加方法向导](./adding-a-method-visual-cpp.md#add-method-wizard)|支持 ATL 的 ATL 或 MFC 项目。 右键单击要添加方法的接口。<br /><br />Visual C++ 会检测项目类型并根据需要修改“添加方法向导”中的选项：<br /><br />- 对于使用 [MFC 应用程序向导](../mfc/reference/mfc-application-wizard.md)创建的项目中的调度接口，使用“添加方法向导”将提供特定于 MFC 的选项。<br />- 对于 MFC ActiveX 控件接口，“添加方法向导”将提供一系列常用方法和属性，你可为你的控件使用所述的这些方法和属性，也可进行自定义。<br />- 对于所有其他接口，“添加方法向导”将提供可用于大多数情况的选项。|

此外，还可以在 COM 控件上实现新接口。 只需在“类视图”中右键单击对象的控件类，然后选择[实现接口](./implementing-an-interface-visual-cpp.md#implement-interface-wizard)。
