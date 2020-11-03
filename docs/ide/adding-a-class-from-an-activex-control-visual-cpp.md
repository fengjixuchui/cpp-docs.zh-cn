---
title: 从 ActiveX 控件添加类
ms.date: 11/08/2018
f1_keywords:
- vc.codewiz.class.axcontrol
helpviewer_keywords:
- ActiveX controls [C++], adding classes
- classes [C++], creating
- ActiveX Control Wizard
- add class from ActiveX control wizard [C++]
ms.assetid: 729fcb37-54b8-44d5-9b4e-50bb16e0eea4
ms.openlocfilehash: d63f73e17e47f2cabb8f1a55c71325ec7068a2c8
ms.sourcegitcommit: a1676bf6caae05ecd698f26ed80c08828722b237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91500369"
---
# <a name="add-a-class-from-an-activex-control"></a>从 ActiveX 控件添加类

使用此向导从可用的 ActiveX 控件中的接口创建 MFC 类。 可以将 MFC 类添加到 [MFC 应用程序](../mfc/reference/creating-an-mfc-application.md)、[MFC DLL](../mfc/reference/creating-an-mfc-dll-project.md) 或 [MFC ActiveX 控件](../mfc/reference/creating-an-mfc-activex-control.md)。

> [!WARNING]
> 在 Visual Studio 2017 版本 15.9 中，Microsoft 弃用了此代码向导，我们将在 Visual Studio 的未来版本中将其删除。 很少用到此向导。 对 ATL 和 MFC 的常规支持不会受到删除此向导的影响。 如果你希望分享对此弃用的反馈，请完成[此调查](https://www.surveymonkey.com/r/QDWKKCN)。 你的反馈对我们很重要。
<!-- Blank comment here to separate the warning and note. -->
> [!NOTE]
> 无需在启用自动化的情况下创建 MFC 项目，即可从 ActiveX 控件中添加类。

ActiveX 控件是基于组件对象模型 (COM) 的可重用软件组件，支持各种 OLE 功能。 可以对它进行自定义来满足多种软件需求。 普通的 ActiveX 控件容器中或 Internet 上的万维网页面中均可使用 ActiveX 控件。

**从 ActiveX 控件添加 MFC 类：**

1. 在“解决方案资源管理器”或“[类视图](/visualstudio/ide/viewing-the-structure-of-code)”中，右键单击要向其添加 ActiveX 控件类的项目的名称****。

1. 从快捷菜单中，选择“添加”，然后选择“添加类”********。

1. 在[添加类](./adding-a-class-visual-cpp.md#add-class-dialog-box)对话框的“模板”窗格中，从 ActiveX 控件中选择“MFC 类”，然后选择“打开”以显示[从 ActiveX 控件向导添加类](#add-class-from-activex-control-wizard)************。

在向导中，可以在 ActiveX 控件中添加多个接口。 还可以在单个向导会话中从多个 ActiveX 控件创建类。

可以从已在系统中注册的 ActiveX 控件添加类，也可以从位于类型库文件（.tlb、.olb、.dll、.ocx 或 .exe）中的 ActiveX 控件添加类，无需提前在系统中注册。 有关注册 ActiveX 控件的详细信息，请参阅[注册 OLE 控件](../mfc/reference/registering-ole-controls.md)。

该向导为从所选 ActiveX 控件添加的每个接口创建 MFC 类，该类派生自 [CWnd](../mfc/reference/cwnd-class.md) 或 [COleDispatchDriver](../mfc/reference/coledispatchdriver-class.md)。

## <a name="in-this-section"></a>在此部分中

- [从 ActiveX 控件添加类向导](#add-class-from-activex-control-wizard)

## <a name="add-class-from-activex-control-wizard"></a>从 ActiveX 控件添加类向导

使用此向导从可用的 ActiveX 控件添加 MFC 类。 该向导为从所选 ActiveX 控件添加的每个接口创建类。

- **添加类的位置**

  指定从中创建类的类型库的位置。

  |选项|说明|
  |------------|-----------------|
  |**注册表**|在系统中注册类型库。 “可用 ActiveX 控件”中列出了已注册的类型库****。|
  |**File**|类型库不必在系统中注册，但必须存储在文件中。 在“位置”中提供文件位置****。|

- **可用 ActiveX 控件**

  指定当前在系统中注册的 ActiveX 控件。 从此列表中选择一个 ActiveX 控件，以在“接口”列表中显示该控件的接口****。 有关如何注册 ActiveX 控件的详细信息，请参阅 [MFC ActiveX 控件：分发 ActiveX 控件](../mfc/mfc-activex-controls-distributing-activex-controls.md)。

  如果选择“添加类的位置”下的“文件”，则此框无法更改********。

- **位置**

  指定 ActiveX 控件的位置。 如果选择“添加类的位置”下的“文件”，则可提供包含类型库的文件的位置********。 要浏览至该文件的位置，请选择省略号按钮。

  如果选择“添加类的位置”下的“注册表”，则此框无法更改********。

- **接口**

  指定 ActiveX 控件中的接口。 该向导使用“可用 ActiveX 控件”中当前选择的接口，或者使用“位置”中指定的类型库文件中的接口********。

  |传输按钮|说明|
  |---------------------|-----------------|
  |**>**|添加当前在“接口”列表中选择的接口****。 如果没有选择接口，则无法执行此操作。|
  |**>>**|添加 ActiveX 控件中的所有接口。 该向导使用“可用 ActiveX 控件”中当前选择的接口，或者使用“位置”中指定的类型库文件中的接口********。|
  |**\<**|删除当前在“生成的类”列表中选择的类****。 如果当前没有在“生成的类”列表中选择类，则无法执行此操作****。|
  |**\<\<**|删除“生成的类”列表中的所有类****。 如果“生成的类”列表为空，则无法执行此操作****。|

- **生成的类**

  指定要从使用 > 或 >> 按钮添加的接口生成的类名 。 可选择此框以选择一个类，然后使用向上或向下键滚动列表。 当选择“完成”时，可以在“类”框中查看每个生成的类名，并在“.h 文件”框中查看每个生成的文件名************。 在此框中仅能选择一个类。

  可通过在此列表中选择一个类并选择 < 来删除该类。 无需在“生成的类”框中选择一个类来删除所有类****。 选择 <<，即可删除“生成的类”框中的所有类 。

- **类**

   指定在选择“完成”时向导添加的“生成的类”框中选择的类的名称********。 可以在“类”**** 框中编辑该名称。

- **.h 文件**

  为新项目的类的头文件设置名称。 默认情况下，此名称基于你在“生成的类”中提供的名称****。 选择省略号按钮以将该文件名保存至所选位置，或将类声明追加到现有文件中。 如果选择现有文件，则向导在你选择“完成”之前，都不会将其保存至所选位置****。

  向导不会覆盖文件。 如果选择现有文件的名称，然后选择“完成”，向导会询问你是否要将该类声明追加到文件内容****。 选择“是”，则追加至文件；选择“否”，则返回至向导并指定另一个文件名********。

- **.cpp 文件**

  为新项目的类的实现文件设置名称。 默认情况下，此名称基于你在“生成的类”中提供的名称****。 选择省略号按钮以将文件名保存至所选位置。 在选择“完成”之前，向导不会将该文件保存至所选位置****。

  向导不会覆盖文件。 如果选择现有文件的名称，然后选择“完成”，向导会询问你是否要将该类实现追加到文件内容****。 选择“是”，则追加至文件；选择“否”，则返回至向导并指定另一个文件名********。
